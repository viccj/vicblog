---
title: "Typora + Upic"
date: 2023-06-22T17:00:41+08:00
type: posts
weight : 4
tags:
- hugo
---
Hugo是用MarkDown來編輯文件的，這時候有個好用的MarkDown 編輯器就很好用，最後我是選擇用Typroa來編輯文件，再貼到Hugo發布，會選擇Typora的原因是因為Typora支援自動上傳圖片並產生連結功能，非常適合Lazy的我。
簡單記錄一下如何用uPic自動上傳圖片到我的GitHub

{{< toc >}}

## GitHub開一個新的Repo並產生token

在GitHub首頁->setting->Developer Setting-> Personal Ascess Token裡面產生一組新的Token。

我是選classic的token，因為看到另一個還在beta。



![Screen Shot 2023-06-22 at 17.19.50](https://raw.githubusercontent.com/viccj/upic/master/uPic/Screen%20Shot%202023-06-22%20at%2017.19.50.png)



可以只勾public_repo，剩下我都沒勾，Note隨便打就好。



## uPic

###  安裝uPic

```
 brew install bigwig-club/brew/upic --cask
```

### 配置

安裝好之後打開uPic，是Mac的話會在最上面那一條出現，一開始沒注意到還一直想說怎麼打不開...



![Screen Shot 2023-06-22 at 17.24.36](https://raw.githubusercontent.com/viccj/upic/master/uPic/Screen%20Shot%202023-06-22%20at%2017.24.36.png)



進入設定頁面

- Owner -> 你的github帳號
- Repo -> 你的repo
- Token-> GitHub token

然後可以按下驗證聽說會有成功畫面出現，但我按了都沒反應就是了。最後是去Typora那邊測試可不可以成功



![Screen Shot 2023-06-22 at 17.25.25](https://raw.githubusercontent.com/viccj/upic/master/uPic/Screen%20Shot%202023-06-22%20at%2017.25.25.png)

設定好後再把uPic預設上傳方式改成GitHub即可


![Screen Shot 2023-06-22 at 17.55.09](https://raw.githubusercontent.com/viccj/upic/master/uPic/Screen%20Shot%202023-06-22%20at%2017.55.09.png)


## Typora設定

![Screen Shot 2023-06-22 at 17.28.15](https://raw.githubusercontent.com/viccj/upic/master/uPic/Screen%20Shot%202023-06-22%20at%2017.28.15.png)

照著設定按一下test Uploader顯示成功，之後圖片拖進來就會自動寫好Markdown嚕。