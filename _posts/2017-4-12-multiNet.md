---
layout: post
title: "論筆記－多媒體網路：影音串流"
categories: [多媒體網路]
tags: [multinetwork, 論筆記]
image_description: false
description: 何為影音串流。
comments: true
published: true
---
# 課本將多媒體的應用分成三類：
1. 串流存取聲音/影像 streaming stored audio/video
2. 透過IP交流的聲音/影像 conversational voice/video-over-IP
3. 串流即時聲音/影像(即時串流聲音/影像)(同步串流聲音/影像) streaming live audio/video
<br><br><br>
Q:List 3 application types for multimedia networking.

## 接著討論1.這一類的應用是怎麼實現的；有三種方式實現：
<pre>
1. UDP串流 UDP streaming
2. HTTP串流 HTTP streaming
3. 適應性(自適應)HTTP串流 adaptive HTTP streaming

跟著研究了CDNs，這個如何能讓大量資料分送給世界各地的使用者;

還調查了三大家影像串流公司用的技術是什麼。
</pre>
<br>
Q:List 3 key distinguishing features of streaming stored video.

## 再接著討論2.這一類的應用(簡單來講就是VoIP)，測試它們如何能夠在最有效率的情況下交流聲音/影像
<pre>
這個部分
1. 時間考量 timing considerations <b>很重要</b> 因為對延遲很敏感 delay-sensitive
2. 已知對時間敏感，另外說明他對訊息沒傳到是相反的容忍性較高 loss-tolerant，而且這些
訊息的遺失能夠透過<b>客戶端的緩衝</b> client buffers，<b>封包順序號碼</b> packet sequence
   numbers，<b>時間戳記</b> timestamps 來使網路上的塞車大大地減少降低

還調查了Skype這家公司用的技術是什麼。
</pre>

### 已知VoIP的技術，接著就解釋兩個，對VoIP最重要的協議 protocol
>
    > 
    > <pre>
    > 1. RTP
    > 2. SIP
    > </pre>
<br>

## 最後最後，介紹了被用來提升不同分類交通的有區別的服務
<pre>
1. link-level scheduling disciplines
2. 交通警察(交通管制)traffic policing
</pre>
