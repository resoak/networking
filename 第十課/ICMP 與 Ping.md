# ICMP 與 Ping

## ICMP 在做什麼

`ICMP` 全名是 `Internet Control Message Protocol`。

它不是拿來搬運一般應用程式資料，而是拿來做：

- 錯誤回報
- 狀態通知
- 連線偵測
- 基本網路診斷

白話說，`ICMP` 比較像網路在互相說明狀況的語言。

## 為什麼需要 ICMP

`IP` 本身偏向盡力傳送。資料送出去之後，中間如果出現：

- 主機沒開機
- 路由找不到
- TTL 用完
- 封包格式有問題

網路仍然需要一個機制把這些情況回報回來，這就是 `ICMP` 的角色。

## 從分層角度看 ping

在封裝時可以這樣看：

- 第二層 frame 會放來源 MAC、目的 MAC。
- 第三層 IP header 會放來源 IP、目的 IP。
- `ICMP` 則是被包在 IP 封包裡面。

這也是為什麼 `ping` 很適合做第一步診斷：

- 它主要檢查第三層可達性。
- 能先避開很多第 4 到第 7 層的應用程式干擾。
- 所以如果 `ping` 都不通，就先不要急著懷疑瀏覽器、網站服務或特定 port。

## ping 為什麼能測網路

我們平常用的 `ping`，本質上就是在送 `ICMP Echo Request`，等對方回 `ICMP Echo Reply`。

如果對方有收到，也願意回應，就能知道：

- 對方主機至少還在線上。
- 從本機到對方的路上，基本路由可能是通的。
- 往返時間可以拿來做初步判斷。

## Echo Request / Echo Reply

這兩個是 `ping` 最核心的訊息。

| 訊息 | Type | 用途 |
|---|---:|---|
| Echo Request | `8` | 主動詢問對方是否可達 |
| Echo Reply | `0` | 對方收到後回應 |

### 白話流程

1. 我送一個 `Echo Request` 給對方。
2. 對方若正常收到，回一個 `Echo Reply`。
3. 我收到 reply，就知道這次 `ping` 有通。

## ICMP Header 重要欄位

課堂中老師特別強調 `ping` 相關封包要看這幾個欄位：

| 欄位 | 用途 |
|---|---|
| Type | 代表是哪一種 ICMP 訊息 |
| Code | 在同一個 Type 下再細分原因 |
| Checksum | 檢查 ICMP 封包是否可能出錯 |
| Identifier | 同一次 `ping` 常共用同一組識別值 |
| Sequence Number | 每一對 request / reply 會對應同一個序號，下一對再加一 |

## Identifier 與 Sequence Number

### Identifier

- 同一次 `ping` 的多個封包，常會共用同一組 `identifier`。
- 如果重新啟動一輪 `ping`，可能會換成新的 identifier。

### Sequence Number

- 第一對 request / reply 可能是序號 `1`。
- 第二對就變 `2`。
- 第三對再變 `3`。

這能幫助我們把成對的封包對起來看。

## ping 看得到什麼，看不到什麼

### 看得到的

- 對方是否有回應。
- 大致的往返時間。
- 是否有明顯封包遺失。

### 看不到的

- 不代表應用程式一定正常。
- 不代表網站服務一定正常。
- 不代表 TCP 或 UDP 某個特定 port 一定有開。

所以 `ping` 是很好用的第一步，但不是全部。

## 為什麼有時候 ping 不通

`ping` 不通不一定等於主機真的壞掉，還可能是：

- 對方防火牆擋掉 `ICMP Echo Request`。
- 中間設備禁止回應 ICMP。
- 路由異常。
- DNS 解析有問題。

課堂也提到，如果主機不想讓別人 `ping`，可以在防火牆上只擋掉相關的 `Type 8` 請求。

反過來說，如果真的需要讓別人 `ping`，比較安全的做法通常是：

- 只開放需要的 `ICMP Echo Request`。
- 不要為了方便就把整個 ICMP 全部放行。

## Wireshark 觀察 ping

抓 `ping` 封包時，可以從三層一起看：

### 第二層

- 來源 MAC 是本機網卡。
- 目的 MAC 可能是同網段主機，或是 `default gateway`。

### 第三層

- 來源 IP 是本機 IP。
- 目的 IP 是你 `ping` 的對象。

### ICMP

- `Type 8` 是 request。
- `Type 0` 是 reply。
- 同一輪 `ping` 的 `identifier` 常相同。
- 一對 request / reply 的 `sequence number` 相同。

### 補充：traceroute 不一定用同一種探測封包

- Windows 常見的 `tracert` 多半和 ICMP Echo 類探測有關。
- 很多 Unix / Linux 的 `traceroute` 預設可能用 UDP 探測。
- 但不管探測封包本身是什麼，核心觀念都一樣：依靠 `TTL` 遞增，並觀察中間設備回來的 `ICMP Time Exceeded`。

## ping 和 ARP 的關係

如果你在區域網路裡第一次 `ping` 某個目標，真正順序通常是：

1. 系統先查 `ARP cache`。
2. 查不到就先送 `ARP request`。
3. 拿到 MAC 後，才開始送 `ICMP Echo Request`。

所以看到 `ping` 前面先出現 ARP，很正常。

## 怎麼記

- `ICMP` 是網路診斷與錯誤回報的重要協定。
- `ping` 用的是 `ICMP Echo Request / Reply`。
- `Type 8` 是 request，`Type 0` 是 reply。
- 看 `ping` 封包時，重點欄位是 `Type`、`Code`、`Checksum`、`Identifier`、`Sequence Number`。

## 關聯筆記

- [[ICMP 訊息類型與 Traceroute]]
- [[ARP Cache 與指令]]
- [[網路連線診斷流程]]
