# ICMP 訊息類型與 Traceroute

## 先記最重要的概念

`ICMP` 不是只有 `ping`。

不同的 `Type` 代表不同用途，有些拿來做查詢，有些拿來回報錯誤。看見 `ICMP` 訊息時，先看 `Type`，再看需不需要進一步看 `Code`。

## Type 與 Code

- `Type`：先分大類。
- `Code`：在同一個 Type 裡再細分原因。

例如 `Type 3` 都叫 `Destination Unreachable`，但還要靠 `Code` 才知道到底是網路不可達、主機不可達，還是協定不可達。

## 課堂常見 ICMP 類型

| Type | 名稱 | 重點 |
|---:|---|---|
| `0` | Echo Reply | 回應 `ping` |
| `3` | Destination Unreachable | 目標不可達，常要再看 Code |
| `4` | Source Quench | 早期用來要求對方慢一點，現在多半視為歷史性概念 |
| `5` | Redirect | 告訴主機有更好的下一跳 |
| `8` | Echo Request | `ping` 發出去的請求 |
| `11` | Time Exceeded | TTL 用完，常見於 `traceroute` |
| `12` | Parameter Problem | 封包欄位或格式可能有問題 |
| `13` | Timestamp Request | 請求時間戳記 |
| `14` | Timestamp Reply | 回應時間戳記 |
| `17` | Address Mask Request | 早期查詢子網路遮罩 |
| `18` | Address Mask Reply | 回應子網路遮罩 |

## Destination Unreachable，Type 3

這是課堂中特別強調的類型，因為平常排錯時很常遇到。

### 常見 Code

| Type | Code | 意義 |
|---:|---:|---|
| `3` | `0` | Network Unreachable，網路不可達 |
| `3` | `1` | Host Unreachable，主機不可達 |
| `3` | `2` | Protocol Unreachable，協定不可達 |

### 白話理解

- `Code 0`：連那個網段都找不到。
- `Code 1`：網段找到了，但那台主機找不到或沒回應。
- `Code 2`：主機到了，但該協定不接受或不支援。

### 為什麼課堂一直強調要看 Code

因為螢幕上都只顯示 `Destination Unreachable` 還不夠精確。真正排錯時，要抓封包看 `Code`，才能更接近根因。

## Source Quench，Type 4

這是比較早期的概念，表示：

> 網路或接收端在說，請你傳慢一點。

現在實務上較少當成主要控制方式，但課堂有提到它的歷史角色，所以知道大意即可。

## Redirect，Type 5

`Redirect` 的意思是：

> 你這樣送也不是不行，但有更好的路由可以走。

Router 可能回給主機這種訊息，提示之後可以改走更合適的下一跳。

## Time Exceeded，Type 11

這是 `traceroute` 很重要的基礎。

當封包在路上每經過一台 Router，`TTL` 都會減少。如果減到 `0`，Router 就把封包丟掉，並回一個 `ICMP Time Exceeded`。

所以我們能利用這個特性，一跳一跳把路徑找出來。

## Parameter Problem，Type 12

這類訊息表示封包某些欄位可能有問題，例如格式異常或參數不合理。它不像 `Echo Request` 那麼常見，但在理解 ICMP 的錯誤回報角色時很重要。

## Timestamp 與 Address Mask

### Timestamp，Type 13 / 14

- `Timestamp Request` 用來詢問對方時間資訊。
- `Timestamp Reply` 用來回應。

課堂主要把它當成概念性介紹，知道它和時間測量、同步概念有關就夠了。

### Address Mask，Type 17 / 18

- `Address Mask Request` 用來詢問子網路遮罩。
- `Address Mask Reply` 用來回覆子網路遮罩。

這也比較偏早期或概念性內容，複習時知道名稱與用途即可。

## traceroute 在做什麼

`traceroute` 或 Windows 常見的 `tracert`，用來回答這個問題：

> 從我這台主機到目的地主機，中間經過哪些路由器？

## traceroute 的核心想法

它不是直接問「路徑在哪裡」，而是利用 `TTL` 逐步增加：

1. 先送 `TTL = 1` 的封包。
2. 第一台 Router 收到後，TTL 歸零，回 `Time Exceeded`。
3. 再送 `TTL = 2` 的封包。
4. 第二台 Router 回 `Time Exceeded`。
5. 這樣一路往下，就能把整條路徑慢慢列出來。

> 補充：不同系統用的探測封包可能不同。Windows 的 `tracert` 常和 ICMP Echo 類探測有關，許多 Unix / Linux 的 `traceroute` 則常見 UDP 探測；但它們都同樣利用 `TTL` 遞增與 `ICMP Time Exceeded` 回應來推路徑。

## traceroute 能看到什麼

- 每一跳的 Router 或中間設備。
- 每一跳的大致回應時間。
- 哪一段可能延遲高。
- 哪一跳開始沒有回應。

## traceroute 的限制

- 有些 Router 會限制或忽略 ICMP，所以不一定每一跳都完整。
- 某些路徑不回應，不代表後面一定完全斷線。
- 它適合看路徑，但不保證能精準還原所有中間設備狀態。

## Wireshark 觀察重點

如果抓 `traceroute` 或相關 ICMP 封包，可以注意：

- `Time Exceeded` 封包是否從中間 Router 回來。
- `Type 11` 是否隨著不同 hop 出現。
- 封包中是否帶有原始封包的一部分，方便主機辨認是哪一次探測。

## 怎麼記

- `Type 8` 發問，`Type 0` 回答。
- `Type 3` 代表到不了，還要看 `Code`。
- `Type 11` 常和 `traceroute` 有關。
- `Redirect` 是告訴你有更好的路。
- `Source Quench`、`Timestamp`、`Address Mask` 先知道大意即可。

## 關聯筆記

- [[ICMP 與 Ping]]
- [[網路連線診斷流程]]
- [[第十課-課堂錄音重點]]
