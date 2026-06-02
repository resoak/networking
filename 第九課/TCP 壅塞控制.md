# TCP 壅塞控制

## 壅塞控制在解決什麼

前面的 [[Sliding Window 與流量控制]] 主要解決的是：

> 接收端收不收得下？

但錄音 13 補充的 `congestion control` 更在意的是：

> 中間網路塞不塞？

兩台主機中間會經過許多 router、switch、ISP 與共享網路資源。即使接收端電腦很快，如果中間網路或 router buffer 已經塞住，傳送端還一直送，就會造成排隊、延遲與丟包。

## Flow control vs Congestion control

| 機制 | 主要觀察對象 | 解決問題 |
|---|---|---|
| Flow control | 接收端 | 接收端 buffer 會不會被塞爆 |
| Congestion control | 中間網路 | 共享網路與 router buffer 會不會太塞 |

簡單說：

- `Flow control`：不要撐爆對方的胃。
- `Congestion control`：不要把路上的車道塞爆。

## 兩個重要視窗

TCP 傳送端不只看接收端提供的 window。

它還要同時考慮：

| 名稱 | 意義 |
|---|---|
| `rwnd` / receive window | 接收端宣告自己還能收多少 |
| `cwnd` / congestion window | 傳送端依網路壅塞狀態估計目前可送多少 |

真正能送出去、但還沒收到 ACK 的資料量，可以先記成：

> 可傳送量 ≈ min(rwnd, cwnd)

也就是兩者取比較小的那個。

原因：

- 如果 `rwnd` 小，代表接收端收不下。
- 如果 `cwnd` 小，代表中間網路可能太塞。
- 任一邊不允許，都不能硬送太多。

## 怎麼判斷網路塞了

傳送端通常無法直接看到中間每台 router 的狀態。

它只能從現象推測，例如：

- 指定時間內沒收到 ACK。
- 收到重複 ACK。
- 封包需要重傳。

這些都可能代表網路某處發生壅塞或封包遺失。

## Slow Start：慢開始

TCP 一開始不知道網路能承受多少資料，所以不會一開始就把大量資料全部丟出去。

`Slow Start` 的想法是：

1. 一開始先用較小的 `cwnd`。
2. 如果 ACK 正常回來，表示目前網路還撐得住。
3. `cwnd` 就逐步增加。
4. 直到碰到門檻或發生丟包。

雖然名字叫「慢開始」，但它常是**指數成長**。

簡化例子：

- 第一次：可送 1 個 MSS。
- ACK 正常後：可送 2 個 MSS。
- 再正常：可送 4 個 MSS。
- 再正常：可送 8 個 MSS。

## MSS

`MSS` = Maximum Segment Size。

它代表單一 TCP segment 最多承載多少資料。

壅塞控制常用「幾個 MSS」來描述目前 congestion window 的大小。

## ssthresh：慢開始門檻

`ssthresh` = slow start threshold。

它是 Slow Start 和 Congestion Avoidance 的分界。

可以先這樣記：

- `cwnd < ssthresh`：用 Slow Start，成長比較快。
- `cwnd > ssthresh`：進入 Congestion Avoidance，成長比較保守。
- `cwnd = ssthresh`：實作上可選擇進入其中一種狀態。

## Congestion Avoidance：壅塞避免

當 `cwnd` 長到接近門檻後，TCP 不會再一直指數增加。

它會改成比較保守的方式，例如每個 RTT 大約增加 1 個 MSS。

可以想成：

- 前期先快速試探網路能力。
- 到某個程度後，改成慢慢加。
- 一邊加，一邊觀察有沒有丟包。

這樣比較不容易突然把共享網路塞爆。

## Duplicate ACK 與 Fast Retransmit

錄音 13 補充了一個重要判斷：重複 ACK。

假設傳送端送出多個 TCP segment：

1. 接收端收到第 1 段，回 `ACK 2`，表示下一段想收第 2 段。
2. 第 2 段遺失。
3. 接收端收到第 3 段，但因為缺第 2 段，仍回 `ACK 2`。
4. 接收端又收到第 4 段，還是回 `ACK 2`。
5. 接收端又收到第 5 段，還是回 `ACK 2`。

傳送端如果看到多個重複 ACK，就能推測：

> 某一段很可能掉了，不必等 timeout 才重傳。

這就是 `Fast Retransmit`（快速重傳）的直覺。

## Fast Recovery：快速恢復

如果每次丟包都把速度打回最一開始，效率會很差。

所以 TCP 會嘗試「退一步，但不要完全重來」。

錄音 13 用例子說明：

- 如果丟包時 `cwnd = 24`。
- 可以把門檻更新成大約一半，例如 `ssthresh = 12`。
- 接著從較低但不是零的速度繼續。

這就是快速恢復的直覺：

- 知道網路出問題了，所以要降速。
- 但網路不一定完全壞掉，所以不用全部從頭開始。

## 怎麼記

- `rwnd`：接收端說「我還能收多少」。
- `cwnd`：傳送端估「網路目前能承受多少」。
- 真正可送量大致取 `min(rwnd, cwnd)`。
- `Slow Start`：一開始小心試，ACK 正常就快速增加。
- `ssthresh`：慢開始與壅塞避免的分界。
- `Congestion Avoidance`：靠近上限後慢慢加。
- `Duplicate ACK`：一直要求同一段，通常代表中間缺了一段。
- `Fast Retransmit`：收到足夠重複 ACK 後，不等 timeout 就重傳。
- `Fast Recovery`：丟包後降速，但不完全歸零。

## 關聯筆記

- [[Sliding Window 與流量控制]]
- [[TCP Header 與序號確認]]
