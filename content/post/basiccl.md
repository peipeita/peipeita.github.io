---
title: "Basics of Shell like sth"
date: 2021-05-18
tags: ["hello", "shell"]
---

## Purpose  
UNIX-like な操作に慣れる。つまりディレクトリ、ファイルなどの操作が CUI 上でできるようにする。  
bash script を書いて実行できるようになる。  

## Hello world

```{shell}
echo "Hello world!"
echo Hello world!
```

## Directory  

```{shell}
pwd # Present Working Dir.
cd ~ # Change Dir. to ~
cd ~/  # Go to home directory
```

## Variable  

```{shell}
greet="Hello world!"
# greet=Hello world!

echo $greet
# echo greet
echo ${greet}
```

## Make dir.

```{shell}
mkdir planets
mkdir --help
# mkdir ~/planets
# mkdir ./planets
cd planets
ls
ls -a #show all
# What is ., .. ?
# Try this
cd .
cd ..
pwd

cd # Hit TAB key hereby
```
## Input/Output

```{shell}
echo "Venus is 金星."
echo "Venus is 金星." >venus.txt
cat venus.txt
# Add more info.

echo "Venus is next to our planet." >>venus.txt
cat venus.txt
```
## Removing obj.

```{shell}
echo "Sun is hot!" >sun.txt
rm sun.txt

mkdir Sun
rm --help
# pay attention for rm command
# I rather prefer -i option
rm -i Sun
rm -ri Sun

echo "Sun is not a planet" >sun2.txt
cat sun2.txt
mkdir ../star

cp sun2.txt ../star/sun2.txt
mv sun2.txt ../star/sun2_1.txt

cd ../star
ls
cd ~
```

## Txt Editing

### `Religios` battle
`Vim`, `Emacs`, `NeoVim` ?  
Noop!!, just `nano`, the simplest.

```{shell}
nano jupiter.txt
```
Write down as follows:  

Jupiter is the largest planet in this system.
Surrouneded by many satellites. 

Please exit.  

```{shell}
cat jupiter.txt
grep Jupiter jupiter.txt
# cat jupiter.txt | grep Jupiter
```

## Practical bio-informatic-ians  

Blastp(R) MpGID2.  
Fetch fasta file as I show u here. 

```{fasta}
>XP_003550317.1 F-box protein GID2 [Glycine max]
MKRAFSVDTAEDNKNNRNTKKTKLDSEEEEEHGTRGGSVYLDDNVLFEVLKHVDARSLAMAGCVSKQWQKAARDERLWELICTKQWANTGCGEQQLRSVVLALGGFRRLHALYLWPLSKPHAPSSSSSSSSWPAIPHVLRSKPRWGKDEVHLSLSLLSIRYYEKMNFLNTKKNT
>PTQ39804.1 hypothetical protein MARPO_0043s0052 [Marchantia polymorpha]
MVSIFECQLVDGHHHQETRWIAREGAKRQKVVTTLQMDSNDDIIYEVMKHLDAKSLATATCVSKQWRKVAEDESLWENVCIQHWPSPVARQKQLRSVVLALGGFRRLYVLCLRPLLARGRPQPQALPSSLKGAEESGEREWSKDEVHLSLSLFSIDCYERLGRRHMTPSSMKFLCKPSANLSIGAHRMLGLGQQQQSASNVR
>XP_002984876.2 F-box protein GID2 [Selaginella moellendorffii]
MLANPGKKPRICEISGELEWRIAMSQPPTVISQVERDDDMFYEILKHLDAKALAMAACVNRRWRRAAEDETLWENVCTINWSSASGASSQASQLRSVVLALGGFRRLYVLCLRPLLSRSSSVPAKASVQQQEKWSKDEVHLSLSLFSIDCYERLGRRYSNCHGSPLKLLCKSSRKADQRYLVPNAAAALELSGVN
```

```{shell}
nano MpGID2.fa
```

```{shell}
head MpGID2.fa
#cat MpGID2.fa | head
tail MpGID2.fa
less MpGID2.fa
```

```{shell}
grep QQQQ MpGID2.fa
grep WWWW MpGID2.fa
grep Q??Q MpGID2.fa
grep W*W MpGID2.fa
grep \> -c MpGID2.fa # How many entries are there? 
grep \> MpGID2.fa | wc -l
```

For more info, 正規表現 is marvelous wit `grep`.

## Kil runnig process

`repeat 10000 echo "Hey"`  
ctrl + C  
^C  

## Bash script

`Bash` is a family member of `shell`.  
私自身その違いなどを未だ感じたことがない。  

```{shell}
nano test.sh
```

```{shell}
#! /bin/bash

pwd 
echo "I am showing GID2 blast result"
cat ./MpGID2.fa
echo "Fin."
```

```{shell}
bash test.sh
```
