---
title: "Pandoc tutorial"
date: "2021/09/01"
tags: ["pandoc", "docker", "beamer"]
---

## Purpose 

`Docker` を使って `.md` のドキュメントから `beamer` 形式のスライドを作成してみたいと思ってやってみました。  
パワポを使ってスライドを作ると、勝手に頭文字を英語にされたり、非常に頭にくる為。  
意外なことに、筆者の WSL の環境ではできなかった。後半部分では linux server で `singularity` を使って目的を達成した方法を記載しています。

## Pull image

```bash
docker pull pandoc/latex
```
## First try on WSL2 environmemt

### set alias
毎回 `docker run -it --rm` なんとか、と打ち込むのは面倒なのでエイリアスを設定しました。  

```bash
alias pandocker='docker run -it --rm -v `pwd`:/mnt -w /mnt pandoc/latex pandoc'
alias #確認
pandocker --help #これが動けば OK!!
```
このままでは永続的に使えないので `vim` とかで `~/.bashrc` に書き込む必要がある。書き込んだら保存してターミナルを再起動するか、`source ~/.bashrc` を実行。

### Convert .md file to beamer slides.

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

`pwd` で指定するのではなく絶対パスで指定しないといけないらしい。 

```bash
alias pandocker='docker run -it --rm -v "c:/home/user/test:/mnt" -w /mnt pandoc/latex pandoc'

pandocker --help
```

今度はまた違うエラーをはいた。  

`docker: Error response from daemon: invalid mode: /mnt.
See 'docker run --help'.`

原因はマウントの指定に `:` が二つ以上入ることらしい。[refer here](https://qiita.com/masayuki14/items/633c7ab3e0f89cc5a30e)  

相対パスも絶対パスも使えない？

最後の悪あがきとして  
`-v "/mnt/c/Users/User/Documents/test:/mnt"` とか `-v $(pwd):/mnt` を試してみた。 
しかしうまくいかなかった。  
万事休す。

## Second try on LINUX server
おそらく、Windows と Docker の相性が悪いのだと想像する。筆者は mac を持っていないので確認できないが、mac ならできるのだろうか。  
Linux のサーバーを使わせてもらっているので、そちらに移動して上の操作をやってみることにした。結果としてはうまくいった。  
ただ、root 権限が必要な `Docker` は使えないので、`singularity` で代用した。  
`singularity` の詳しい説明は他所に譲るが、公式のイメージなら Docker Hub から pull できるし、自分で Dockerfile を local でビルドしたものを Docker Hub に登録してそれをサーバー上から pull して singularity image を作成できる。 

```bash
mkdir -p ~/singularity_img

singularity pull ~/singularity_img/pandoc.sif docker://pandoc/latex:latest
```
`singularity image` の拡張子は `.sif` とする慣習らしい。  

## Render beamer slides

```bash
cd ~
mkdir test
cd test
vim sample.md # 上のをコピペ
```

```bash
singularity exec -B `pwd`:/mnt ~/singularity_img/pandoc.sif pandoc /mnt/sample.md -t beamer -o /mnt/output.pdf
```
- `-B` はマウント用のオプション
- input と output のファイルは `/mnt` でパスを指定。
:w

[Slides](images/output.pdf)

もう少し凝ったスライドは