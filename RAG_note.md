# RAG

## 用徒

- 加強知識
- 減少幻覺
- 同步 real-time data
- 數據隱私性（rag 可以自託管）

## 建立 RAG

![](https://developer-blogs.nvidia.com/wp-content/uploads/2023/12/rag-pipeline-ingest-query-flow-b.png)

1. 文件攝取 Document ingestion

- 收集各種文件數據
- Document loader 可將各式文件轉為可處理的文本

2. 文件預處理 Document pre-processing

- 清理文件
- Normalization
- chunking

3. 生成嵌入 Generating embeddings

- 把文字轉為向量

4. 在向量資料庫中存儲嵌入 Storing embeddings in vector databases
