# TCP 連線建立與結束

## 為什麼 TCP 要先建立連線

TCP 是 connection-oriented。

正式傳資料前，雙方要先確認：

- 對方存在。
- 對方願意建立連線。
- 雙方收送方向都能正常運作。
- 雙方的初始 sequence number 可以同步。

這個過程就是三向交握。

## 三向交握

三向交握可以簡化成：

1. Client → Server：`SYN`
   - 我想建立連線。
   - 我先給你我的初始 sequence number。
2. Server → Client：`SYN + ACK`
   - 我收到你的 SYN。
   - 我也同意建立連線。
   - 我也給你我的初始 sequence number。
3. Client → Server：`ACK`
   - 我收到你的 SYN + ACK。
   - 連線正式建立。

完成後，雙方才開始傳送資料。

## 為什麼不是兩次交握

如果只做兩次交握，可能發生：

- 舊的連線請求在網路中延遲很久後才到。
- Server 誤以為 Client 又要建立新連線。
- Server 分配資源等待，但 Client 其實沒有要連。

三向交握能讓雙方都確認：「這次連線真的有效，而且雙方都知道」。

## 初始序號

三向交握不只是打招呼，也會同步雙方的初始 sequence number。

這很重要，因為：

- TCP 要靠 sequence number 判斷資料順序。
- 也要靠它辨識重複或過期資料。
- 初始序號通常不會固定從 0 開始，而是由系統產生。

### 錄音 13 封包觀察補充

用 Wireshark 觀察三向交握時，可以看到一個重要細節：

- `SYN` 本身會消耗 1 個 sequence number。
- 所以接收端回 `SYN + ACK` 時，ACK number 會是對方初始 sequence number `+ 1`。
- 第三步 client 回 `ACK` 時，也會確認 server 的初始 sequence number `+ 1`。

也就是說，三向交握不是只看旗標，也會同步雙方後續資料流要從哪個序號開始。

## SYN flood 概念

錄音 12 也提到：如果有人大量送出 `SYN`，但不完成後續 ACK，Server 可能會一直保留半開連線資源。

這類攻擊常稱為 `SYN flood`。

它的核心問題是：

- Server 以為很多連線正在建立。
- 但對方不完成第三步。
- Server 資源可能被耗盡。

## 資料傳送期間

連線建立後，雙方可以同時傳資料。

TCP 是 full-duplex：

- A 可以傳給 B。
- B 也可以傳給 A。
- 雙方各自維護自己的 sequence number。
- 雙方也各自用 ACK 確認對方傳來的資料。

## 四次揮手

資料傳完後，TCP 需要結束連線。

常見流程是四次揮手：

1. A → B：`FIN`
   - A 說：我這邊資料送完了，想結束。
2. B → A：`ACK`
   - B 說：我知道你想結束。
3. B → A：`FIN`
   - B 說：我這邊資料也送完了，也要結束。
4. A → B：`ACK`
   - A 說：我收到你的結束通知。

### FIN 也會消耗序號

錄音 13 補充：`FIN` 和 `SYN` 類似，也會消耗 1 個 sequence number。

所以對方收到 `FIN` 後，回 ACK 時會把 FIN 算進去：

- 如果 FIN 的 sequence number 是 `x`。
- 對方確認時通常會回 `ACK = x + 1`。

這代表：「我知道你這個方向要結束了」。

## 為什麼通常是四次

因為 TCP 是雙向資料流。

一方說「我不送了」，不代表另一方也馬上沒有資料要送。

所以：

- `FIN` 表示某一方向的資料送完。
- `ACK` 表示收到對方的 FIN。
- 另一方如果還有資料，就可以先送完。
- 另一方送完後，再送自己的 FIN。

## TIME_WAIT 的直覺

主動結束的一方送出最後 ACK 後，通常不會立刻完全消失，而會等待一段時間。

可以先這樣理解：

- 確認最後的 ACK 有機會送達。
- 如果對方沒收到 ACK 又重送 FIN，自己還能再回 ACK。
- 避免舊連線的殘留封包干擾下一次新連線。

## 怎麼記

- 建立連線：`SYN → SYN + ACK → ACK`
- 結束連線：`FIN → ACK → FIN → ACK`
- 建立是雙方確認彼此存在與初始序號。
- 結束是雙方各自把自己的傳送方向關掉。

## 關聯筆記

- [[TCP Header 與序號確認]]
- [[Sliding Window 與流量控制]]
