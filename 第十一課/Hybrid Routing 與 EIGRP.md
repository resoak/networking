# Hybrid Routing 與 EIGRP

## Hybrid Routing 是什麼

`Hybrid routing` 是把不同路由方法的優點結合起來的做法。

課堂中老師把它放在 `Distance Vector` 與 `Link State` 之後講，因為它可以理解成：

> 不完全只靠距離向量，也不完全照連線狀態，而是混合兩邊的優點。

## 為什麼需要 Hybrid

`Distance Vector` 的優點是簡單、資源需求低，但資訊較不完整，網路變大時更新與收斂可能比較慢。

`Link State` 的優點是拓撲資訊較完整、路徑計算較精準，但初期交換資訊與計算成本比較高。

所以後來就有人思考：

- 能不能保留 Distance Vector 的簡單？
- 又能不能吸收 Link State 對狀態更新與路徑品質的優點？

這就是 Hybrid 的核心想法。

錄音 20 補充：Hybrid routing 會吸收 Distance Vector「建立到目的地最佳路徑的路由表」這個方向，也吸收 Link State「連線狀態改變時才更新」的優點，目標是減少不必要的更新，同時保持較好的路徑選擇能力。

## EIGRP

課堂中提到 `EIGRP` 可以作為 Hybrid routing protocol 的代表。

`EIGRP` 會結合多種概念來做路由選擇與更新，不是單純只用 hop count，也不是完全只靠 Link State flooding 的方式。

這一課先知道它的定位即可：

- 它是動態路由協定。
- 它常被歸在混合式路由協定。
- 它試著兼顧更新效率與路徑選擇品質。

課堂也提到 `IS-IS` 常被放在進階路由協定脈絡中一起比較；這一課重點仍以 `EIGRP` 作為 Hybrid 的主要記憶例子。

## 和前兩者比較

| 類型 | 白話理解 | 代表協定 |
|---|---|---|
| Distance Vector | 跟鄰居交換「到哪裡多遠」 | `RIP` |
| Link State | 分享連線狀態，建立拓撲後算路 | `OSPF` |
| Hybrid | 結合不同方法的優點 | `EIGRP` |

## 課堂定位

老師這裡沒有要求深入推導 `EIGRP` 的所有細節，而是希望先把名詞和分類建立起來。

也就是看到這些協定時，要能大致判斷：

- `RIP`：Distance Vector。
- `OSPF`：Link State。
- `EIGRP`：Hybrid。

## 怎麼記

- Hybrid：混合式。
- EIGRP：把距離向量與連線狀態的部分優點結合。
- 先背分類，不急著背完整演算法細節。

## 關聯筆記

- [[Distance Vector 路由]]
- [[Link State 與 OSPF]]
- [[Static 與 Dynamic Routing]]
- [[Autonomous System 與 BGP]]
