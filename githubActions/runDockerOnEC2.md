# EC2 + github actions + go + docker container

1. 建立專案 + github repo

```
$ mkdir <project_name>
$ cd <project_name>
$ go mod init <project_name>
$ touch main.go
$ git init
$ git remote add origin
$ git add .
$ git commit -m "init"
$ git push -u origin main
```

2. 建立 EC2

- 使用 AWS EC2 建立一個新的實例，選擇適合的 AMI（例如 Amazon Linux 2 /2023 , Ubuntu）。
  Amazon Linux 2 版本較舊 X64
  Amazon Linux 2023 版本較新 ARM64, 實作我選擇 2023, ARM64 版本

3. EC2 install docker  
   refer: https://docs.aws.amazon.com/zh_cn/serverless-application-model/latest/developerguide/install-docker.html

```
$ sudo yum update -y

# Amazon Linux 2
$ sudo amazon-linux-extras install docker
# Amazon Linux 2023
$ sudo yum install -y docker

$ sudo service docker start
# ec2-user添加到 docker group, docker 相關操作可以減少 sudo 命令
$ sudo usermod -a -G docker ec2-user

# 確認 docker 安裝成功
$ docker ps
```

4. EC2 install github actions self-hosted runner, 讓 github repo pull request 可以觸發 docker build & push  
   refer: https://docs.github.com/zh/actions/hosting-your-own-runners/managing-self-hosted-runners/adding-self-hosted-runners  
   AWS Linux 2023 會有一些問題，參考這篇文章解決  
   refer: https://github.com/actions/runner/issues/2511
5. EC2 configuring the self hosted runner application as a service  
   讓 self-hosted runner 在 EC2 啟動時自動背景執行，這樣就不需要每次都手動啟動 runner。  
   refer: https://docs.github.com/zh/actions/hosting-your-own-runners/managing-self-hosted-runners/configuring-the-self-hosted-runner-application-as-a-service

6. 完成 actions yaml  
   refer: https://github.com/zen006598/accounting/pull/3/files
