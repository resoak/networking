# Port number 與常見服務

## IP 只能找到主機

`IP address` 可以幫資料找到哪一台電腦，但一台電腦上可能同時有很多程式在通訊。

例如同一台電腦可能同時有：

- 瀏覽器
- 通訊軟體
- 遊戲
- Email 用戶端
- 背景更新程式

只靠 IP 不知道資料要交給哪一個程式，所以傳輸層需要 `Port number`。

## Port 是什麼

- Port 是**邏輯編號**，不是實體插孔。
- 作業系統會依照 port，把收到的資料交給對應的應用程式或服務。
- 可以把 port 想成同一棟大樓裡的房間號碼。

白話記法：

- `IP address`：找到哪一台主機。
- `Port number`：找到那台主機上的哪一支程式。

## Port number 範圍

Port number 是 **16 bits**。

所以範圍是：

- `0 ~ 65535`

## Port number 分類

| 類型 | 範圍 | 說明 |
|---|---:|---|
| Well-known ports | `0 ~ 1023` | 常見標準服務使用 |
| Registered ports | `1024 ~ 49151` | 特定應用或廠商註冊使用 |
| Dynamic / Private ports | `49152 ~ 65535` | 用戶端臨時連線常用 |

## 常見 Port

| Port | 常見用途 |
|---:|---|
| `20 / 21` | FTP |
| `22` | SSH |
| `23` | Telnet |
| `25` | SMTP / Email 傳送 |
| `53` | DNS |
| `80` | HTTP |
| `110` | POP3 |
| `143` | IMAP |
| `443` | HTTPS |

## 伺服器端與用戶端 Port

伺服器端通常使用固定 port。

例如：

- 網站 HTTP：`80`
- 網站 HTTPS：`443`
- DNS：`53`

用戶端通常由作業系統臨時分配一個 port。

一條連線常用這四個資訊區分：

- 來源 IP
- 來源 Port
- 目的 IP
- 目的 Port

這組資訊常被稱為連線的識別組合。

## 用 netstat 觀察

課堂中也示範可以用 `netstat` 看自己電腦目前有哪些連線。

觀察重點：

- 本機 IP 與本機 port。
- 遠端 IP 或網域與遠端 port。
- 連線狀態。
- 遠端 `:443` 通常代表 HTTPS。
- 遠端 `:80` 通常代表 HTTP。
- `127.0.0.1` 是 loopback，代表本機自己跟自己通訊。

## 怎麼記

- IP 是地址。
- Port 是房間號。
- TCP / UDP header 都會放 source port 與 destination port。

## 關聯筆記

- [[TCP 與 UDP]]
- [[UDP Header]]
- [[TCP Header 與序號確認]]
