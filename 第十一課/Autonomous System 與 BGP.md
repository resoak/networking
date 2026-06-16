# Autonomous System 與 BGP

## 為什麼需要 AS

網際網路上的網路太多，如果每台 Router 都要保存全世界所有路由細節，routing table 會非常大，搜尋、更新與管理成本也會變得很高。

所以大型網路會被切成一個一個可管理的區塊，稱為 `Autonomous System`，常簡寫為 `AS`。

## AS 是什麼

`Autonomous System` 可以想成由同一個組織或管理政策控制的一群網路與 Router。

白話來說：

- 自己 AS 裡面的路由，自己管理。
- 別人的 AS 裡面的細節，不需要每台 Router 都完全知道。
- AS 與 AS 之間只交換必要的可達資訊與路由政策。

## IGP 與 EGP

路由協定可以依使用範圍分成兩層：

| 類型 | 使用範圍 | 代表協定 |
|---|---|---|
| `IGP` | 同一個 AS 內部 | `RIP`、`OSPF`、`EIGRP`、`IS-IS` |
| `EGP` | 不同 AS 之間 | `BGP` |

`IGP` 偏向處理同一組織內部的路由效率，例如哪條線成本比較低、哪條線斷了要改走哪裡。

`EGP` 偏向處理不同組織之間的路由交換，除了可達性，也常牽涉政策、商業關係與路徑控制。

## BGP

`BGP` 全名是 `Border Gateway Protocol`，是現代 Internet 上不同 AS 之間最重要的路由協定。

課堂補充的重點：

- `BGP` 用在 AS 與 AS 之間。
- 它會記錄路徑經過哪些 AS，形成 `AS path`。
- 如果同一個 AS 在路徑中重複出現，就可能代表路由迴圈，因此可以被避免。
- `BGP` 不只看距離，也能依照政策過濾或選擇路由。

## AS Path 與路徑選擇

錄音 21 強調了 AS path 的概念：

> `BGP` 會記錄路徑經過哪些 AS，形成 `AS path`。如果同一個 AS 在路徑中重複出現，就可能代表路由迴圈，因此可以被避免。

**為什麼 AS path 重要：**

1. **防止迴圈**：如果路徑中已經出現自己的 AS，代表這條路可能繞回來，應避免採用。
2. **策略選路**：不同組織之間不一定只比距離，也可能依政策、商業關係或安全需求選路。
3. **降低細節負擔**：AS 之間交換的是可達資訊與路由政策，不需要把每個內部網段細節都暴露給所有外部 Router。

## IGP 與 EGP 的差別

錄音 21 把 IGP 與 EGP 的差別整理成「不同層級的 routing」：

- `IGP` 用在同一個 AS 內部，通常假設這些 Router 由同一組織管理、彼此信任，路由資訊可以比較自由地交換。
- `EGP` 用在不同 AS 之間，重點不只是能不能到達，還包含政策、路徑控制與跨組織協調。
- `RIP`、`OSPF`、`EIGRP` 屬於 IGP；現代 Internet 跨 AS 的代表協定是 `BGP`。

## BGP 與早期 EGP

錄音中也提到，早期 `EGP` 協定已經不太適合現在龐大又分散的 Internet。現代主要使用 `BGP` 作為 AS 之間的 routing protocol。

BGP 的特點可以這樣記：

- 它用在 AS 與 AS 之間。
- 它會記錄 `AS path`，協助避免路由迴圈。
- 它可以依政策選路，不只是單純比 hop count 或 cost。
- 它適合處理大型 Internet 中不同組織之間的路由交換。

## 怎麼記

- `AS`：一個可獨立管理的大型網路區塊。
- `IGP`：AS 裡面用的協定。
- `EGP`：AS 之間用的協定。
- `BGP`：Internet 上 AS 之間的主要路由協定。
- `AS path`：記錄路徑經過的所有 AS，用來防止迴圈和選路。
- `IGP` 偏好效率，`EGP` 偏好策略。

## 關聯筆記

- [[Static 與 Dynamic Routing]]
- [[Distance Vector 路由]]
- [[Link State 與 OSPF]]
- [[Hybrid Routing 與 EIGRP]]
