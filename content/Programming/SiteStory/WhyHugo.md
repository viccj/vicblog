---
title: "Why Hugo"
date: 2023-06-21T22:00:41+08:00
type: posts
weight : 1
tags:
- hugo
- trash talk
---

為啥我搞了半天最後決定用 [Hugo](https://gohugo.io/)弄個網站？

{{< toc >}}

## 錯綜複雜的原因

會用Hugo架網站完全就是個意外，或許可以說是不同時間點覺得有趣的東西結合起來的結果。

### 斷斷續續的筆記習慣

在轉職成為軟體工程師前，就有在記錄我最近學了什麼東西的習慣，原本是記在[HackMD](https://hackmd.io/)，但上班後就棄用了，
上班後用的東西就記在Apple Notes上，比較偏各專案得紀錄跟常用的指令。前陣子也有用過一陣子medium寫過幾篇刷題紀錄，
真的是幾篇就四篇吧 (而且都是複製別人的解法用自己的話重講一次，還都是easy)。

### VPS

跟Billy大神閒聊的時候知道他最近考過了AWS證照，夢想成為離前端越遠越好的軟體工程師的我很羨慕又欽佩，便在1 on 1的時候與主管聊起這話題，在主管的推薦下建議我可以先玩玩VPS，比起AWS單純也便宜，原理也都差不多，熟悉之後去用AWS會比較快上手因為AWS實在太雜了。

然後我就馬上開了一開VPS起來然後發現我不知道要幹嘛... 就又關掉了。

### Domain

做某專案的時候要請客戶提供Domain讓我們設定，或是我們幫他們弄一組，但我一開始完全不知道這些東西如何去用專業術語溝通，就問了旁邊的同事，發現他熟的跟啥一樣，我跟他比起來大概是全熟的牛排VS旁邊的生菜。

後來知道他有在架NAS，也有買自己的Domain，還親手登入後台設定一遍給我看。聽到他有自己的domain我眼睛都亮起來了，感覺超屌，心想我一定也要一個自己的Domain。





## 為了讓Domain的錢花得心安

接著好一陣子我每天朝思暮想要買一個domain，也去Godaddy上面瞎晃，不過畢竟 我也不是啥有錢人，買domain一定要物盡其用，只能為了說服我自己買domain而努力找出一個用途來，最好還要對我的職業生涯很有幫助。



最後想著想著突然想到我可以用WordPress弄個blog來寫學習筆記，綁上我的domain！咦～聽起來不錯，便開始利用通勤時研究WordPress怎麼弄。



## Hugo

WordPress研究了半天，也不時看到其他架靜態blog的工具，發現這東西真是五花八門，馬上就又稍微研究了一下 (真的只有稍微)。



- Jekyll：Ruby寫的，沒什麼感覺
- Hugo：Go寫的，好像很潮
- Hexo：Node.js寫的，直接刪掉



刪掉了Jekyll跟Hexo最後在wordpress跟Hugo選一個，一開始還是有點猶豫不決，最後壓垮我的倒數第二根稻草是看到一篇 {{<ref-out href="https://blog.lancitou.net/using-github-actions-to-deploy-hugo-blog-to-self-hosted-vps/">}} 
Hugo+Github Action 自動部署到 VPS{{</ref-out>}} 
的文章，
心裡想哇靠這不是把我的三個願望一次完成了嗎？還多點了一滴滴CI/CD。



最後一根稻草是也去看了一下技術名人的網站，看到 {{<ref-out href="https://openhome.cc/zh-tw/">}}良葛格的新網站{{</ref-out>}} 也是用Hugo架的，整個簡潔又專業。



因此最後，就決定是Hugo了



