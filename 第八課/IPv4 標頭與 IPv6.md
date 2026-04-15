# IPv4 標頭與 IPv6

## IPv4 標頭重點

- `Version`
- `IHL`
- `Total Length`
- `Identification`
- `Flags`
- `Fragment Offset`
- `TTL`
- `Protocol`
- `Header Checksum`
- `Source IP`
- `Destination IP`

## 各欄位在做什麼

- `Version`：告訴你這是 IPv4 還是 IPv6。
- `IHL`：標頭有多長。
- `Total Length`：整個封包總長度。
- `Identification`：分片重組時拿來辨認同一批封包。
- `Flags`：控制分片行為，例如能不能分片。
- `Fragment Offset`：這塊分片在原本資料中的位置。
- `TTL`：每經過一台 router 減 1，避免封包永遠繞圈。
- `Protocol`：指出上層協定，例如 `TCP`、`UDP`、`ICMP`。
- `Header Checksum`：檢查 IPv4 標頭是否有錯。
- `Source / Destination IP`：來源與目的位址。

## 幾個一定要懂的欄位

- `TTL`：封包每經過一台路由器就會減少，避免無限循環。
- `Protocol`：指出上層協定，例如 `TCP`、`UDP`、`ICMP`。
- `Flags / Fragment Offset`：和分片、重組有關。

## 分片觀念

- 如果資料太大，可能需要切成好幾片再送。
- 這時候就會用到：
  - `Identification`
  - `Flags`
  - `Fragment Offset`
- 接收端再依這些資訊把資料拼回去。

## IPv6 為什麼出現

- 因為 `IPv4` 位址不夠用了。
- `IPv6` 提供 **128 bits** 位址空間。

## IPv6 不只是「位址比較長」

- `IPv6` 的確最直觀的差別是位址空間大很多。
- 但它同時也代表：
  - 更適合大量裝置連網
  - 結構更整齊
  - 對未來網路、IoT、雲端環境更有彈性

## IPv6 重點

- 可省略前導 0。
- 連續的 0 可用 `::` 表示，但一個位址中只能出現一次。
- 常見特殊位址：
  - `::`：未指定
  - `::1`：Loopback

## IPv6 寫法怎麼看

- `IPv6` 是 **128 bits**。
- 常用 16 進位分成 8 組表示。
- 一開始看到很長很正常，先抓兩件事就好：
  - 前導 0 可以省略
  - 連續的 0 可以用 `::` 壓縮一次

## IPv6 傳播方式

- `Unicast`
- `Multicast`
- `Anycast`
- 不再使用 Broadcast

## IPv4 和 IPv6 轉換期

- 現實世界不會一夜之間全部改成 IPv6。
- 所以常會出現：
  - 漸進遷移
  - 雙棧並行
  - tunneling / 過渡機制
- 這部分先知道大方向就好：**IPv4 和 IPv6 在一段時間內會共存**。

## 怎麼記

- `IPv4` 重點在欄位與分片。
- `IPv6` 重點在位址空間大、寫法不同、架構更適合未來大量裝置。

## 再補一句

- `TTL`：防止無限繞路
- `Fragmentation`：大資料切小塊
- `IPv6`：不只是變長，而是整體設計更面向未來

## 連到其他主題

- [[IP 位址與子網路]]
- [[DHCP 與 DNS]]
