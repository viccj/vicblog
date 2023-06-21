---
title: "Install SSL"
date: 2023-06-18T22:00:41+08:00
tags:
- tags
- Infra
---

Domain 是在 [namecheap](https://www.namecheap.com/) 買的，設定好A紀錄跟CNAME之後就可以用了，再來就是申請SSL憑證
糊裡糊塗就把SSL弄好了，隨手記錄一下印象比較深刻的步驟跟坑。

{{< toc >}}

## 領取SSL憑證


1. 生成私鑰：

```
openssl genrsa -out private.key 2048
```

生成一個2048位的RSA私鑰，並將其保存在名為 `private.key` 的文件中。

接著用剛剛的私鑰生成CSR（證書簽署請求）

```
openssl req -new -key private.key -out csr.csr
```

會要求提供與SSL證書相關的信息，例如域名、組織名稱、所在地等。請根據提示輸入正確的信息。

Common Name (CN)：這邊要填你的域名 domain.com，它後面本來寫說填個名字就好我就傻傻的填我的英文名字，結果一直失敗..

2. 生成的CSR文件 `csr.csr` 就可以整個複製上去。





## 收到憑證後

通常在取得 SSL 憑證時，您會收到兩個檔案：`.crt` 憑證檔案和 `.ca-bundle`（或 `.bundle`）憑證束檔案。

`.crt` 憑證檔案是您的 SSL 證書，它包含您的網站的公開金鑰以及其他相關的證書資訊。

`.ca-bundle`（或 `.bundle`）憑證束檔案是根憑證和中繼憑證的集合。它包含了憑證鏈中的所有憑證，用於驗證您的 SSL 憑證的合法性。憑證鏈包括根憑證（Root Certificate）和中繼憑證（Intermediate Certificate）。根憑證是由受信任的憑證授權中心（Certificate Authority）簽署的，而中繼憑證是由根憑證簽署的其他憑證。

在設定 Nginx 的 SSL 時，**需要將 `.crt` 憑證檔案和 `.ca-bundle`（或 `.bundle`）憑證束檔案合併為一個檔案**，形成完整的憑證鏈。



>順序不能錯，要先crt再ca-bundle 

```bash
 cat domain.crt domain.ca-bundle > server.crt
```



## nginx設定 



```bash
# HTTP 重定向到 HTTPS
server {
    listen       80;
    server_name  lazyvic.com;
    return 301 https://lazyvic.com$request_uri;  # 返回 301 重定向到相應的 HTTPS URL
}

# HTTPS 配置
server {
    listen 443 ssl;  # 監聽 443 端口，啟用 SSL
    server_name lazyvic.com;
    ssl_certificate /etc/nginx/ssl/server.crt;  # SSL 憑證路徑
    ssl_certificate_key /etc/nginx/ssl/private.key;  # SSL 憑證私鑰路徑

    location / {
        root   /var/www/html/public;  # 根目錄，用於存放網站文件
        index  index.html index.htm;  # 默認索引文件
    }

    # 自定義 404 頁面
    error_page 404 /404.html;
    location = /404.html {
        # 這裡可以指定自定義的 404 頁面的路徑和檔名
    }

    # 自定義 50x 錯誤頁面
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;  # 50x 錯誤頁面的路徑
    }
}

```





## nginx -t 通過但是一直重啟不了nginx



在重啟前有先用 `nginx -t` 確認無誤，但是重啟nginx卻一直失敗，看了log說是沒有權限取得檔案，查了一下解法如下：

```bash
restorecon -v -R /etc/nginx
```

查了一下這個restorecon到底是啥東西

>`restorecon` 指令用於恢復檔案和目錄的 SELinux 安全上下文。在某些 Linux 發行版中，如 CentOS 和 RHEL，安全上下文是用於控制檔案和目錄的存取權限和 SELinux 政策的一部分。

> 當您遭遇 Nginx SSL 憑證無法載入的問題時，有時是由於 SELinux 安全上下文不正確導致的。透過執行 `restorecon` 指令，您可以將 `/etc/nginx` 目錄及其內部的檔案和目錄恢復為預設的 SELinux 安全上下文。

> 這樣做可以確保 Nginx 服務以適當的安全上下文執行，並且具有正確的存取權限，從而能夠正確載入並使用 SSL 憑證。
