---
title: "Use pandoc"
date: 2021/09/01
tags: ["pandoc", "docker", "beamer"]
---

## Purpose 

`Docker` を使って `.md` のドキュメントから `beamer` 形式のスライドを作成してみたいと思ってやってみました。  
パワポを使ってスライドを作ると、勝手に頭文字を英語にされたり、非常に頭にくる為。  

## Pull image

```bash
docker pull pandoc/latex
```
## Set alias

毎回 `docker run -it --rm` なんとか、と打ち込むのは面倒なのでエイリアスを設定しました。  

```bash
alias pandocker='docker run -it --rm -v `pwd`:/workdir -w /workdir pandoc/latex pandoc'
alias #確認
pandocker --help #これが動けば OK!!
```
このままでは永続的に使えないので `vim` とかで `~/.bashrc` に書き込む必要がある。書き込んだら保存してターミナルを再起動するか、`source ~/.bashrc` を実行。

## Convert .md file to beamer slides.

とりあえずサンプルとして以下のような `sample.md` を作成。

```md
---
title: "Habits"
author: "peipeita"
date: "2021/09/01"
output: beamer_presentation
---

## Getting up

- Brush my teeth
- Drink hot water

## Go to bed

- Brush my teeth carefully
- Wash my fase carefully
```

```bash
pandocker sample.md -t beamer -o output.pdf
```

`pandoc: pandoc: openBinaryFile: does not exist (No such file or directory)`

この様なエラーがはかれた。いろいろ調べると WSL では Docker のマウントに変なクセがあるらしい。[see here](https://tamosblog.wordpress.com/2019/03/11/volume_mount_by_docker_on_wsl/)  

`pwd` で指定するのではなく絶対パスで指定しないといけないらしい。 `-v c:/ `

```bash
alias pandocker='docker run -it --rm -v "c:/home/user/test:/workdir" -w /workdir pandoc/latex pandoc'

pandocker --help
```

今度はまた違うエラーをはいた。  

`docker: Error response from daemon: invalid mode: /workdir.
See 'docker run --help'.`

原因はマウントの指定に `:` が二つ以上入ることらしい。[refer here](https://qiita.com/masayuki14/items/633c7ab3e0f89cc5a30e)  

相対パスも絶対パスも使えないので万事休す？

最後の悪あがきとして  
`-v "/mnt/c/Users/User/Documents/test:/workdir"`  

`-v $(pwd):/data` を試してみた。 
しかしうまくいかなかった。 

```bash
docker run -it --rm -v $(pwd):/data pandoc/latex pandoc sample.md -t beamer -o output.pdf
```