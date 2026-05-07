# IPv4 標頭與 IPv6

## IPv4 標頭重點

- `Version`
- `IHL`
- `Type of Service / DSCP`
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
- `IHL`：標頭有多長，單位是 32-bit word。
- `Type of Service / DSCP`：讓封包可以標示優先權、延遲、吞吐量、可靠度等需求。
- `Total Length`：整個封包總長度，包含 IPv4 標頭與資料。
- `Identification`：分片重組時拿來辨認同一批封包。
- `Flags`：控制分片行為，例如能不能分片。
- `Fragment Offset`：這塊分片在原本資料中的位置。
- `TTL`：每經過一台 router 減 1，避免封包永遠繞圈。
- `Protocol`：指出上層協定，例如 `TCP`、`UDP`、`ICMP`。
- `Header Checksum`：檢查 IPv4 標頭是否有錯。
- `Source / Destination IP`：來源與目的位址。
- `Options / Padding`：選用欄位；如果標頭長度沒有對齊 32 bits，會用 padding 補齊。

## 標頭像信封

- 新錄音裡老師用「信封」來講 IPv4 標頭。
- 資料本身像信件內容，標頭像信封上面的資訊：
  - 從哪裡來
  - 要送到哪裡
  - 內容多長
  - 要不要分片
  - 這個封包還能活多久
  - 上層是 TCP、UDP 還是 ICMP
- 接收端先看標頭，才知道後面的資料要怎麼處理。
- 因為大家都照固定欄位擺放，路由器和主機才能快速解析。

## 幾個一定要懂的欄位

- `TTL`：封包每經過一台路由器就會減少，避免無限循環。
- `Protocol`：指出上層協定，例如 `TCP`、`UDP`、`ICMP`。
- `Flags / Fragment Offset`：和分片、重組有關。
- `Header Checksum`：用來確認標頭在傳送過程中有沒有錯。
- `Source / Destination IP`：就像信封上的寄件人與收件人。

## Type of Service / DSCP

- 這個欄位和封包服務品質有關。
- 可以先理解成：封包可以告訴路由器「我比較急」或「我需要比較穩」。
- 常見概念包括：
  - 優先權
  - 延遲需求
  - 傳輸量需求
  - 可靠度需求
  - 成本考量
- 但這不是說每個封包都可以亂設最高優先權。
- 如果大家都說自己最重要，網路就分不出真正需要優先處理的資料。

## 分片觀念

- 如果資料太大，可能需要切成好幾片再送。
- 錄音裡老師提到，常見最大傳輸單位可以先記成約 `1500 bytes`。
- IPv4 標頭本身也要占空間，所以真正可放的資料不一定剛好是 1500 bytes。
- 這時候就會用到：
  - `Identification`
  - `Flags`
  - `Fragment Offset`
- 接收端再依這些資訊把資料拼回去。

### Flags 怎麼記

- `DF` = Don't Fragment
  - 表示不要切片
  - 如果封包太大又不能切，就可能送不出去
- `MF` = More Fragments
  - 表示後面還有其他分片
  - 最後一片通常就不會再標示還有下一片

### Fragment Offset 怎麼記

- `Fragment Offset` 用來標示這一片在原始資料中的位置。
- 接收端靠它把碎片照順序拼回原本資料。
- 可以先記：**分片不是只切開就好，還要能重新排回去**。

## TTL

- `TTL` = Time To Live
- 它不是讓封包真的「活到幾歲」，而是限制封包在網路上可以存在多久。
- 每經過一台 router，數值就會減少。
- 如果減到 `0` 還沒送到，就丟掉。
- 目的：避免封包在錯誤路徑或路由迴圈中永遠繞來繞去。
- 常見最大值可以先記成 `255`。

## Protocol

- `Protocol` 欄位告訴接收端：IPv4 後面包的是哪一種上層協定。
- 常見值：
  - `ICMP = 1`
  - `IGMP = 2`
  - `TCP = 6`
  - `UDP = 17`
  - `OSPF = 89`
- 所以 `Protocol` 不是 port number，而是告訴你下一層協定類型。

## Header Checksum

- `Header Checksum` 用來檢查 IPv4 標頭有沒有傳錯。
- 傳送端先根據標頭算出一個檢查值。
- 接收端收到後再算一次。
- 如果算出來不一致，就代表標頭可能有錯，這個封包就不應該繼續被正常處理。
- 先記重點：**它檢查的是 IPv4 header，不是整個資料內容**。

## Options / Padding

- `Options` 是選用欄位，不是每個 IPv4 封包都一定有。
- 可能用在測試、路徑記錄、時間戳記、安全等特殊用途。
- 因為 Options 長度不固定，所以才需要 `IHL` 告訴別人標頭到底有多長。
- `Padding` 用來把標頭補到 32 bits 的整數倍，讓後面的資料位置對齊。

## Wireshark 觀察

- 新錄音裡老師有提到可以用 `Wireshark` 把封包抓出來看。
- 用 Wireshark 可以看到：
  - `Version`
  - `Header Length / IHL`
  - `Total Length`
  - `Protocol`
  - `Checksum`
  - `Source IP`
  - `Destination IP`
- 這能幫你把課本上的欄位，對到真實封包中的位置。

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

- `IPv4` 重點在欄位、分片、TTL、Protocol、Checksum。
- `IPv6` 重點在位址空間大、寫法不同、架構更適合未來大量裝置。

## 再補一句

- `TTL`：防止無限繞路
- `Fragmentation`：大資料切小塊
- `Protocol`：告訴你後面是哪種上層協定
- `Checksum`：檢查 IPv4 標頭有沒有錯
- `IHL`：標頭長度，因為 Options 可能讓標頭變長
- `IPv6`：不只是變長，而是整體設計更面向未來

## 連到其他主題

- [[IP 位址與子網路]]
- [[DHCP 與 DNS]]
