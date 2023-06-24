---
title: "Hugo GitHub Page"
date: 2023-06-22T14:00:41+08:00
weight : 2
tags:
- hugo
---

離安裝好Hugo並胡搞瞎搞也過了大概一週，用我僅存的記憶記錄一下，前面都是湊字數的，重點是最後一段哈哈


{{< toc >}}

## 安裝Hugo

因為我是Mac，所以直接用 `brew intstall hugo`安裝。

安裝好之後就可以用 `hugo new site blog-test`來建立新的網站。


## 主題安裝

安裝好Hugo之後，下一步就是挑選主題，我是使用 [geekdoc](https://geekdocs.de/) 這個主題，主要是看到他的example site覺得很簡單，剛好我也不想花太多時間在把我的頁面弄的很fancy

安裝的方法是直接下載他們的release bundle

```bash
// 在themes的folder下面建立hugo-geekdoc的folder
mkdir -p themes/hugo-geekdoc/

// 下載
curl -L https://github.com/thegeeklab/hugo-geekdoc/releases/latest/download/hugo-geekdoc.tar.gz | tar -xz -C themes/hugo-geekdoc/ --strip-components=1
```



安裝好之後再到Hugo的設定檔中把主題設定上去

```
theme = "hugo-geekdoc"
```

然後啟動Hugo就可以看到預設的頁面了

```
hugo server -D
```


{{< hint type=tip >}}
雖然hugo server會自動重build頁面，但是我發現有時候更改檔案內容或是資料夾結構，會無法正確顯示，這時候還是手動重啟hugo server比較穩。

{{< /hint >}}

編輯一下就可以用

```
hugo
```

生成靜態內容，會建立一個`public`的資料夾將你剛剛的文件都放進去。

然後就可以把他推到github的repository拉

## Github Page 抓不到css

其實前面的步驟網路上都一堆教學，唯一我遇到的問題就是部署到github page後，發現

我的css一直跑掉，打開開發工具看一下發現抓不到我的css跟js等檔案。

在stackoverflow上找了半天，發現是相對路徑的問題，最後在config.toml檔案上加上

```
relativeURLs= true
```

就解決了。


