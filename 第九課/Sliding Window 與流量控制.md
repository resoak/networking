# Sliding Window 與流量控制

## 為什麼需要 Sliding Window

最直覺的可靠傳輸方式是：

1. 傳一段資料。
2. 等對方 ACK。
3. 收到 ACK 後再傳下一段。

這種方式簡單，但效率很差。

因為每次都要等一來一回，中間很多時間都浪費在等待。

## Sliding Window 的想法

Sliding window 允許傳送端在還沒收到每一段 ACK 前，先連續送出多段資料。

可以把 window 想成：

> 目前允許連續送出的資料範圍。

收到 ACK 後，window 往前滑動，傳送端就可以繼續送新的資料。

## 緩衝區與接收能力

接收端通常有 buffer，可以暫時存放收到但還沒處理完的資料。

但 buffer 不是無限大。

所以接收端需要告訴傳送端：

- 我現在還能收多少？
- 你可以繼續送多少？
- 我快滿了，能不能先慢一點？

這就是 `Window Size` 欄位的用途。

## Flow control

`Flow control` 是針對「傳送端與接收端」之間的控制。

核心問題是：

> 不要讓傳送端送得比接收端能處理的速度還快。

如果接收端來不及處理，它可以：

- 把 window size 變小。
- 甚至宣告 window size = `0`。
- 等 buffer 空出來後，再告訴對方可以繼續送。

## 與 congestion control 的差別

錄音 13 進一步補充 TCP 還有 congestion control。

可以先這樣分：

| 機制 | 主要看誰 | 問題 |
|---|---|---|
| Flow control | 接收端 | 對方收不收得下？ |
| Congestion control | 中間網路 | 網路會不會太塞？ |

這份筆記先重點放在 flow control 與 sliding window。

更完整的壅塞控制整理請看 [[TCP 壅塞控制]]。

## Cumulative ACK

TCP 的 ACK 常是累積式確認。

例如：

- 傳送端送出 `60000 ~ 60999`。
- 接收端回 `ACK = 61000`。

代表：

- `61000` 之前的資料都已經連續收到。
- 下一段希望從 `61000` 開始。

如果中間某段沒收到，ACK 可能會停在缺口的位置，讓傳送端知道要從缺口補起。

## 遺失與重傳

課堂用例子說明：如果第 5 段資料遺失，即使後面的第 6、7、8 段可能已經到達，接收端仍可能因為中間缺口而一直要求從第 5 段開始補。

可以先這樣記：

- ACK 不往前，代表前面還有資料缺口。
- 傳送端等太久沒收到預期 ACK，就會重傳。
- 重傳時仍靠 sequence number 判斷是同一段資料。

## MSS

`MSS` = Maximum Segment Size。

它表示單一 TCP segment 最多可以承載多少資料。

例如課堂影片用簡化例子：

- 總資料量：`6000 bytes`
- MSS：`1000 bytes`
- 接收端 window：`3000 bytes`

那傳送端可以先切成 6 段，每段最多 1000 bytes，並且先連續送出 3000 bytes 以內的資料。

## Window Scale

TCP header 的 window size 欄位本身是 16 bits。

原始最大值約是：

- `65535 bytes`

現代網路速度很快，這可能不夠用，所以 TCP options 裡可以使用 `Window Scale`，把可表示的 window 放大。

## 怎麼記

- Stop-and-wait：傳一段，等一次，效率低。
- Sliding window：可以連續傳多段，ACK 回來後 window 往前滑。
- Window size：接收端告訴傳送端自己還能收多少。
- Flow control：不要把接收端塞爆。
- Congestion control：不要把中間網路塞爆。

## 關聯筆記

- [[TCP Header 與序號確認]]
- [[TCP 連線建立與結束]]
- [[TCP 壅塞控制]]
