---
layout: post
title:  "Kubectl 使用環境設置"
date:   2021-09-17
tags: [kubernetes,wsl]
categories: docker
---

- Auto Complete  
附加至 .bashrc
```bash
echo "" >> ~/.bashrc
echo "# add autocomplete permanently to your bash shell." >> ~/.bashrc
echo "source <(kubectl completion bash)" >> ~/.bashrc
```

- 別名
```bash
echo "" >> ~/.bashrc
echo "alias k=kubectl" >> ~/.bashrc
echo "complete -F __start_kubectl k" >> ~/.bashrc
```


> [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)