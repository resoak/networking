# Distance Vector 路由

## Distance Vector 是什麼

`Distance Vector` 是一種動態路由的做法。

白話來說，每台 Router 會和相鄰的 Router 交換自己知道的路由資訊，讓大家慢慢知道到各個目的網路要走多遠、往哪個方向走。

## 一句話記法

> Distance Vector：跟鄰居交換「我知道到哪裡要多遠」。

## 基本流程

可以想成下面幾步：

1. Router 一開始只知道自己直接連到的網路。
2. 它把自己的路由資訊告訴相鄰 Router。
3. 相鄰 Router 收到後，把距離加上自己到鄰居的成本。
4. 如果這條路比較好，就更新自己的 routing table。
5. 之後再把更新後的資訊告訴其他鄰居。
6. 網路中的 Router 逐漸學到更多路徑。

## 課堂例子

錄音 18 用 A、B、C、D 節點來說明：

- 一開始，Router 只知道直接相鄰的節點。
- 經過一次交換後，能知道隔壁節點知道的路。
- 再經過幾次交換，距離較遠的路徑也會被學到。
- 最後 routing table 會記錄到各目的地大概要經過幾個 hop。

這種做法很直覺，但需要時間讓資訊一層一層傳開。

## 優點

- 概念簡單。
- Router 負擔相對低。
- 小型網路容易使用。
- 不需要每台 Router 一開始就知道完整拓撲。

## 缺點

- 大型網路中，交換與更新的資訊量會變大。
- Router 掌握的是從鄰居聽來的資訊，不一定知道整個網路拓撲。
- 網路狀態改變時，更新需要時間傳開。
- 收斂速度可能比 Link State 慢。

## RIP

課堂中提到 `RIP` 可作為 Distance Vector 類型的代表。

`RIP` 常用 `hop count` 作為 metric，也就是看封包到目的地要經過幾個 Router。這種 metric 很容易懂，但也比較粗略，因為它不一定反映速度、穩定度或成本。

課堂補充，`RIP` 常見最大 hop count 是 `15`，`16` 可以視為不可達。這個限制可以避免封包或路由資訊在網路裡無止境繞圈。

## Bellman-Ford 直覺

錄音 20 補充了 Distance Vector 背後的 Bellman-Ford 類型想法。

白話公式可以這樣想：

> 我到目的地的成本 = 我到某個鄰居的成本 + 那個鄰居到目的地的成本。

Router 會向鄰居詢問「你到目的地要花多少成本」，再加上自己到該鄰居的成本，從所有鄰居裡選出最小的那一條。

所以 Distance Vector 很依賴鄰居提供的資訊。如果鄰居的資訊還沒更新或判斷錯誤，自己也可能跟著學到錯誤路由。

## Routing Loop 與 Count to Infinity

Distance Vector 的常見問題是 `routing loop`。

假設某個網路已經斷線，但相鄰 Router 還沒同步知道，Router 之間可能互相相信對方「還有路」，結果封包或路由資訊在幾台 Router 之間繞圈。

`count to infinity` 則是距離數字一直增加，直到達到協定定義的最大值才停止。這代表網路要花一段時間才會收斂到「這條路不可達」的狀態。

## 避免迴圈的常見做法

課堂提到幾個處理方式：

- 設定最大 hop count，超過就視為不可達。
- `split horizon`：從某個方向學到的路由，不再往同一方向公告回去。
- `poison reverse`：明確告訴對方，這條從你那邊學來的路對我來說不可用。
- `hold-down timer`：路由變動後先等待一段時間，避免立刻相信不穩定資訊。
- `triggered update`：狀態一變就立刻通知，不只等週期性更新。

## 和 Link State 的差別

| 項目 | Distance Vector | Link State |
|---|---|---|
| 資訊來源 | 主要跟鄰居交換距離或路由表 | 分享連線狀態，建立較完整拓撲 |
| 複雜度 | 較低 | 較高 |
| 適合 | 小型或較單純網路 | 較大型、需要較完整拓撲資訊的網路 |
| 代表 | `RIP` | `OSPF` |

## 怎麼記

- Distance：到目的地多遠。
- Vector：往哪個方向或鄰居走。
- 它像是問隔壁鄰居：「你知道到那裡要幾步嗎？」
- 它簡單，但要小心迴圈與收斂慢。

## 關聯筆記

- [[Static 與 Dynamic Routing]]
- [[Routing Table 路由表]]
- [[Link State 與 OSPF]]
- [[Hybrid Routing 與 EIGRP]]
- [[Autonomous System 與 BGP]]
