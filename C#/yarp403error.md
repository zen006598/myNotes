# YARP Gateway 403 除錯筆記

## 問題

**情境：**  
使用 `Docker Compose` 打包 `YARP` Gateway 與後端 API（order-api），在瀏覽器中存取 `http://localhost:5000/order-api/order/1` 時出現 `HTTP 403 Forbidden` 錯誤。

**問題：**

- 在 gateway container 內使用 `curl http://localhost:5000/order-api/order/1` 能正確 proxy 到 order-api 服務。
- 從主機瀏覽器存取時卻回傳 403。

---

## 原因

1. **確認容器與網路：**

   - Docker Compose 中 gateway 與 order-api 均在同一個 network（`tongdemicro-network`），服務名稱與埠口對應正常（gateway: 5000，order-api: 5002）。
   - 容器內部透過 DNS 能正確解析對方，因此容器間通訊無誤。

2. **YARP 路由匹配與 Transform**

   - YARP 的 Route 設定：
     ```json
     "Match": {
       "Path": "/order-api/{**catch-all}"
     },
     "Transforms": [
       {
         "PathPattern": "api/{**catch-all}"
       }
     ]
     ```
   - 在容器內測試時可能因為環境直接發起請求，而瀏覽器發出的請求包含其他 header 或實際請求路徑略有不同，需要留意路由匹配是否完全符合。

3. **主機環境端口佔用問題**
   - 使用 `lsof -i :5000` 查詢後發現本機端口 5000 被系統服務（`ControlCe`，對應 `/etc/services` 中的 `commplex-main`）占用。
   - 這表示當你用瀏覽器發送請求到 `http://localhost:5000/...` 時，實際上連上的是本機其他服務，而不是 Docker container 裡的 Gateway。
   - 而這個服務返回了 403 Forbidden。

---

## 解決方式

1. **變更 Gateway 對外映射的端口**  
   修改 `docker-compose.yml` 中 gateway 服務的配置，將本機端口從 5000 改為其他未被占用的端口（例如 8888）：

   ```yaml
   services:
     gateway:
       ports:
         - "8888:5000" # 外部 8888 對應 container 內部的 5000
       environment:
         - ASPNETCORE_HTTP_PORTS=5000
       networks:
         - tongdemicro-network
   ```

2. **驗證**
   - 重新啟動 docker-compose
   - 再用瀏覽器與 curl 測試確認請求能夠正確進入 container 並由 YARP 正常 proxy 至後端 API。

---

## 除錯重點總結

- **確認端口映射：**  
  確保主機端口未被其他系統服務占用，導致連線錯誤。  
  使用 `lsof -i :5000` 檢查端口使用情況。

- **核對 YARP 的 Route 與 Transform：**  
  確認請求路徑能夠正確匹配到設定的 route，避免因路由不符合而直接返回 403 Forbidden。

- **測試環境分離：**  
  容器內測試與主機測試可能因環境不同而結果不一，需確保從主機發起請求真正連上 Docker container 中運行的服務。
