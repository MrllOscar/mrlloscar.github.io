---
title: 基礎hexo使用，架設
date: 2021-10-03 22:51:59
excerpt: 快速開始，_config.yml內的設定
categories: 
- [hexo]
tags: [ Hexo使用]
---
## [Quick Start](https://hexo.io/zh-tw/docs/)

```cmd=
$ npm install -g hexo-cli
```

### Setting 設定，設定，設定。

最外層```_config.yml```會是你需要修改有關主題、git、語言、網站標題內容等等的地方，他會是一個類似全域的東西去修改很整個網站的東西，所以不要亂加一些有得沒得不然可能會跟我一樣進得去出不來，發不了大財。

### Create a new post 創造一個全新的！！！！！　　　　文章？

```cmd=
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### ready to run server 產生server前該做的事情

```cmd=
$ hexo g -debug
```

###### Generate static files 跨博，產生靜態檔案的意思

```cmd=
$ hexo generate
```

* 一段hexo都可以互相組合，底下是常用組合內容

|選項|描述|
|---|---|
|`-d`,`--deploy`|產生完成即部屬網站|
|`-w`,`--watch`|監看檔案變更|
|`--debug`|除錯模式|

More info: [Generating](https://hexo.io/docs/generating.html)

###### Deploy to remote sites 部屬網站...dase?其實跟上面可以一起組合用

```cmd=
$ hexo deploy
```

|選項|描述|
|---|---|
|`-g`,`--generate`|部屬網站前先產生靜態檔案|

### Run server 模擬server

```cmd=
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)

摸應佛:[中文指令](https://hexo.io/zh-tw/docs/commands)