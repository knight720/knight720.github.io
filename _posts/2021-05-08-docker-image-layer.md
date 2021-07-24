---
layout: post
title:  "我不知道的 docker image layer"
date:   2021-05-08 12:00:00 +0800
tags: [docker]
categories: docker
---

有寫過幾次 Dockerfile 的經驗，知道 Docker 在 build image 時有 layer 的概念，Dockerfile 中的每一個指令都會建立 layer，因此會看到有些 Dockerfile 在一個指令內去完成想要做的事，如下。

```dockerfile
RUN apt-get update && apt-get install -y \
    package-bar \
    package-baz \
    package-foo  \
    && rm -rf /var/lib/apt/lists/*
```

> [Dockerfile best practices, RUN](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#run)

直到有一天我在書中看到一句話**"後續資料層所刪除的系統檔案，依然存在於映像檔中，只是無法被存取。"**才驚覺原來 layer 有些特性我其實不知道，因此我進行了以下的情境來驗證。 

> Kubernetes 建置與執行 : 邁向基礎設施的未來, 2/e

Alpine_01_Base
```dockerfile
FROM alpine
WORKDIR /app
```
 
Alpine_02_NoPipe
```dockerfile
FROM alpine
WORKDIR /app
RUN fallocate -l 100M big_file.dat
RUN rm ./big_file.dat
```

Alpine_03_Pipe
```dockerfile
FROM alpine
WORKDIR /app
RUN fallocate -l 100M big_file.dat | rm ./big_file.dat
```

可以明顯看出 Alpine_01_Base 的 image 大小為 000MB，而 Alpine_02_NoPipe 即使把 100MB 的刪除掉之後 image 仍然還有 000MB，Alpine_03_Pipe 則是使用 pipe 的方式刪除檔案 image 大小只有 000MB。由此可知下層 layer 的內容並不會因為被上層 layer 刪除而真的是放原本占用的空間。  
[圖]

書中另外還有提到關於安全性的部分**"不能將密碼放進容器裡，不只是最後一層，在印象檔的任一層都不行。任何擁有工具的人都可以存取，攻擊者可以直接建立一個包含密碼資料層的映像檔。"**‧以 .Net 的開發習慣來說設定檔都是會跟著一起佈署的，所以打包 docker image 時也不例外，不過這麼做其實是有安全性的疑慮的，docker image 的原則應該是不包含設定值的，而是執行的當下才將參數注入到 container 當中運行。

> Kubernetes 建置與執行 : 邁向基礎設施的未來, 2/e

所以在撰寫 Dockerfile 實應該注意的兩個議題，image 的大小及安全性。參考 MSDN 上的資訊 .Net 提供的 Dockerfile 範例檔會使用 multi-stage 來建立 docker image ，如下。

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /source

COPY *.sln .
COPY aspnetapp/*.csproj ./aspnetapp/
RUN dotnet restore

COPY aspnetapp/. ./aspnetapp/
WORKDIR /source/aspnetapp
RUN dotnet publish -c release -o /app --no-restore

FROM mcr.microsoft.com/dotnet/aspnet:5.0
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "aspnetapp.dll"]
```

> [Docker images for ASP.NET Core, The Dockerfile](https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/building-net-docker-images#the-dockerfile)

接著使用 multi-stage 來驗證一下，是不是能有效避免 image 遇上大小及安全性的問提，內容如下。

Alpine_04_MultiStage
```dockerfile
FROM alpine AS build
WORKDIR /code
RUN fallocate -l 100M big_file.dat
RUN fallocate -l 10M small_file.dat
RUN rm ./big_file.dat

FROM alpine
WORKDIR /app
COPY --from=build /code ./
```

[圖]

由此可知我們在 build 的階段刪除不必要的資料或設定檔，可以減少 image 的大小及解決安全性的問題，透過這一連串的試驗，讓我對 Dockerfile 的撰寫有了更深一層的認識。