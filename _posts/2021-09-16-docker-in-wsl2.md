---
layout: post
title:  "在 WSL 開機啟動 docker service"
date:   2021-09-16
tags: [docker,wsl]
categories: docker
---

緣起於 Docker Desktop 要開始收費，順便降低 Windows 系統資源的使用量，所以有了把 docker 放到 WSL 內執行的念頭，不過每次開機都還要另外啟動 docker 服務有點麻煩，那就讓他預設啟動吧...

- 編輯 /etc/sudoers  
```bash
sudo visudo
```

在 /etc/sudoers 新增內容([USER_NAME] 須置換)
```bash
[USER_NAME] ALL=(ALL) NOPASSWD: /usr/bin/dockerd
```
> [Linux中的sudoers檔案設定簡介](https://ithelp.ithome.com.tw/articles/10053821)

- 於 ~/.bashrc 新增內容
```bash
echo '' >> ~/.bashrc
echo '# Start Docker daemon automatically when logging in if not running.' >> ~/.bashrc
echo 'RUNNING=`ps aux | grep dockerd | grep -v grep`' >> ~/.bashrc
echo 'if [ -z "$RUNNING" ]; then' >> ~/.bashrc
echo '    sudo dockerd > /dev/null 2>&1 &' >> ~/.bashrc
echo '    disown' >> ~/.bashrc
echo 'fi' >> ~/.bashrc
```

- 新增權限
```bash
sudo usermod -a -G docker $USER
```

- 驗證
```bash
docker run hello-world
```

> [How to automatically start the Docker daemon on WSL2](https://blog.nillsf.com/index.php/2020/06/29/how-to-automatically-start-the-docker-daemon-on-wsl2/)
