---
title: "Setting Up this blog"
date: 2021/04/04
tags: ["git page", "hugo"]
---

## Prerequisities  
まず`Golang`と`Hugo`をインストール  
静的ウェブサイトの構築にはHugoがいいとのことなのでこれを使うことにした。  

## Locally build-up
ほとんど自動でやってくれる  
```shell
$ hugo new site blog; cd blog
$ git init

```

好みのテーマを選んでくる。  
今回は[Minimal](https://themes.gohugo.io/minimal/)を使用した。

```shell
$ git submodule add https://github.com/calintat/minimal.git themes/minimal
$ git submodule init
$ git submodule update
$ git submodule update --remote themes/minimal # Update
$ cp themes/minimal/exampleSite/config.toml . #Configuration  
```
この`config.toml`を書き換えていく。

```toml
# baseurl= "http://example.com"
baseurl= "YOUR Github page URL"

# accent= "red"
accent= "green"
```
よくわからないが、ここで `baseurl` を github page の URL に変更すると、ローカルでサイトの構築を確認する `hugo server` がちゃんとできなくなる。  
さらに、github page の要求で、render するパスを変更しなければいけない。ブラウザから github pages の setting から Source を `/` (root) から `/docs` に変更する必要がありそう。ほかにもレンダリングの方法はあるらしいが、これが最も単純。  
