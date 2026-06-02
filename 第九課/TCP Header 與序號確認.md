# TCP Header 與序號確認

## TCP Header 為什麼比較大

TCP 要提供可靠傳輸，所以 header 比 UDP 複雜。

- UDP header 固定 `8 bytes`。
- TCP header 最少 `20 bytes`。
- 如果有 options，TCP header 會更長。

TCP header 裡的欄位不是裝飾，而是用來支援：

- 連線建立。
- 可靠傳輸。
- 資料排序。
- ACK 確認。
- 重傳。
- 流量控制。
- 連線結束。

## TCP Header 常見欄位

| 欄位 | 用途 |
|---|---|
| Source Port | 來源應用程式 port |
| Destination Port | 目的應用程式 port |
| Sequence Number | 這段資料從哪個 byte 位置開始 |
| Acknowledgement Number | 下一個期望收到的 byte 位置 |
| Header Length / Data Offset | TCP header 長度，單位是 4 bytes |
| Flags | 控制連線狀態，例如 SYN、ACK、FIN |
| Window Size | 接收端目前還能收多少資料 |
| Checksum | 檢查 TCP segment 是否可能出錯 |
| Urgent Pointer | 搭配 URG 使用，指出緊急資料位置 |
| Options | 額外控制資訊，例如 MSS、Window Scale |

## Header Length / Data Offset

TCP header 長度欄位常以 **4 bytes** 為單位。

例如：

- 欄位值是 `5`。
- 代表 `5 × 4 bytes = 20 bytes`。
- 這也是 TCP header 沒有 options 時的最小長度。

## Sequence Number

`Sequence Number` 用來標記資料在整個 TCP byte stream 中的位置。

重點：

- TCP 不是只看「第幾個封包」。
- TCP 是以 **byte** 為單位編號。
- 接收端可用 sequence number 判斷資料順序。
- 如果資料重複或亂序，也能靠 sequence number 處理。

### 例子

如果 A 傳資料給 B：

- Sequence Number = `60000`
- Data Length = `1000 bytes`

代表這段資料包含：

- `60000 ~ 60999`

## Acknowledgement Number

`Acknowledgement Number` 可以記成：

> 下一個期望收到的 byte 編號。

沿用剛才例子：

- B 收到 `60000 ~ 60999`
- B 回 ACK = `61000`

意思是：

- 我已經收到 `60000 ~ 60999`。
- 下一段請從 `61000` 開始送。

## 雙向傳輸時要分開看

TCP 是 full-duplex，雙方都能同時傳資料。

所以：

- A 傳給 B 的資料有 A 自己的 sequence number。
- B 傳給 A 的資料有 B 自己的 sequence number。
- ACK number 是針對「對方傳來的資料」做確認。
- Sequence number 與 ACK number 會互相搭配，但不是同一個數字。

## Flags：控制訊號

課堂中最常用到的是：

| Flag | 用途 |
|---|---|
| SYN | 建立連線時同步初始序號 |
| ACK | 確認收到資料或控制訊號 |
| FIN | 表示資料送完，想結束連線 |
| RST | 重設連線 |
| PSH | 盡快把資料推給應用程式 |
| URG | 搭配 Urgent Pointer，表示有緊急資料 |

三向交握主要看 `SYN` 與 `ACK`。

四次揮手主要看 `FIN` 與 `ACK`。

## Window Size

`Window Size` 用來告訴對方：

> 我目前還有多少緩衝空間可以收資料。

這是 TCP 做 flow control 的核心欄位。

如果接收端處理不過來，可以把 window 變小，甚至變成 `0`，讓傳送端先不要再塞資料。

錄音 13 補充：實際傳送時，傳送端還會考慮 [[TCP 壅塞控制]] 裡的 congestion window；所以可送量不是只看接收端 window，而是還要看中間網路是否可能壅塞。

## Options

TCP options 可以放額外控制資訊。

課堂錄音 12 特別補到：

- `MSS`：單一 TCP segment 最多承載多少資料。
- `Window Scale`：讓 window size 可以超過原本 16-bit 欄位的限制。

## Wireshark 觀察方式

看 TCP 封包時，可以特別觀察：

- Source Port / Destination Port：資料是哪個程式送到哪個程式。
- Sequence Number：這段資料從哪裡開始。
- Acknowledgement Number：對方下一個期待收到哪裡。
- Data Length：這段資料有多長。
- Window Size：接收端還能收多少。
- Flags：這個封包是在建立、確認、傳資料，還是結束連線。

## 關聯筆記

- [[TCP 連線建立與結束]]
- [[Sliding Window 與流量控制]]
- [[TCP 壅塞控制]]
