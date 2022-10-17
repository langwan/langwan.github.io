---
title: "MP3解码器"
bookHidden: true
---

## MP3文件结构

MP3是一种音频压缩技术，其全称是动态影像专家压缩标准音频层面3（Moving Picture Experts Group Audio Layer III），简称为MP3。它被设计用来大幅度地降低音频数据量。利用 MPEG Audio Layer 3 的技术，将音乐以1:10 甚至 1:12 的压缩率，压缩成容量较小的文件，而对于大多数用户来说重放的音质与最初的不压缩音频相比没有明显的下降。它是在1991年由位于德国埃尔朗根的研究组织Fraunhofer-Gesellschaft的一组工程师发明和标准化的。用MP3形式存储的音乐就叫作MP3音乐，能播放MP3音乐的机器就叫作MP3播放器。

MP3文件分为TAG_V2(ID3V2)，Frame, TAG_V1(ID3V1)共3部分，简单的可以理解成从每一个Frame当中把音频信号解码出来，编码的方法就是huffman（哈夫曼编码），这种编码在winzip等各种压缩领域都广泛应用，是现在软件设计中最基础的媒体压缩编码。

### frame

一个frame有1152个samples构成

## huffman（哈夫曼编码）

哈夫曼编码(Huffman Coding)，又称霍夫曼编码，是一种编码方式，哈夫曼编码是可变字长编码(VLC)的一种。Huffman于1952年提出一种编码方法，该方法完全依据字符出现概率来构造异字头的平均长度最短的码字，有时称之为最佳编码，一般就叫做Huffman编码（有时也称为霍夫曼编码）。

## MP3解码器

* [PDMP3](https://github.com/technosaurus/PDMP3) 纯c语言编写的mp3解码器。
* [go-mp3](https://github.com/hajimehoshi/go-mp3) 纯go语言编写的mp3解码器。

