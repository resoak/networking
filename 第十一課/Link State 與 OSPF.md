# Link State 與 OSPF

## Link State 是什麼

`Link State` 是另一種動態路由的做法。

它的重點不是只問鄰居「到某地多遠」，而是讓 Router 分享自己的連線狀態，讓網路中的 Router 能掌握較完整的拓撲資訊。

## 一句話記法

> Link State：大家分享自己連到誰、連線成本如何，再自己算最短路徑。

## Link State 會分享什麼

課堂中提到的連線狀態可以包含：

- 這台 Router 連到哪些鄰居。
- 鄰居的 `IP address` 或識別資訊。
- 連線成本。
- 連線是否健康。
- 連線狀態是否有變動。

這些資訊會被包成類似 `Link State Packet` 的資訊，再往外傳播。

## Flooding

`Flooding` 是把連線狀態資訊傳給其他 Router 的方式。

白話來說：

1. Router 產生自己的連線狀態資訊。
2. 送給相鄰 Router。
3. 相鄰 Router 再轉送給其他 Router。
4. 最後同一區域內的 Router 都能掌握這些資訊。

為了避免同一筆資訊一直被重複轉送，封包通常會有識別資訊，Router 收過同一筆後就不再重複送。

## Dijkstra 最短路徑

當 Router 掌握拓撲後，就可以用 Dijkstra 類型的最短路徑演算法，計算自己到各目的網路的較佳路徑。

錄音 20 補充了 Dijkstra 的直覺流程。可以想成：

1. 從自己這個起點開始，距離設為 `0`。
2. 在還沒確認的節點中，挑目前距離最小的節點。
3. 把它加入已確認集合。
4. 用這個節點去更新它鄰居的距離。
5. 重複直到目的地或所有節點都被確認。

課堂提醒這部分屬於演算法與圖論概念，不需要在這一課死背細節。現在的重點是知道：

- Link State 會建立較完整的網路圖。
- Router 會根據這張圖算出到各目的地的路徑。
- 算完後結果會更新到 routing table。

Dijkstra 適合邊的權重為非負數的情境。如果圖中有負權重，應改用 Bellman-Ford 類型演算法；但一般網路路由的 cost 通常不會設成負數。

## OSPF

`OSPF` 全名是 `Open Shortest Path First`，是常見的 Link State routing protocol。

課堂中提到，因為大型網路如果所有 Router 都在同一個範圍內交換完整資訊，複雜度會很高，所以 `OSPF` 會把網路切成不同 area。

錄音 21 補充，`OSPF` 是常見的 `IGP`，也就是同一個 AS 內部使用的路由協定。它利用 Link State 的觀念掌握區域內拓撲，並用 cost 選出較佳路徑。

### Area 與 Backbone

`OSPF` 的區域化概念可以這樣記：

- 區域內的 Router 主要處理自己區域的拓撲資訊。
- 不同區域之間透過 backbone area 連接。
- 這樣可以降低 flooding 與計算的範圍。
- 網路規模變大時比較容易管理。

## 優點

- 掌握的拓撲資訊較完整。
- 路徑計算較精準。
- 網路狀態變動時，可以比較有系統地更新。
- 適合較大型或較複雜的網路。

## 缺點

- 協定與設定比 Distance Vector 複雜。
- 一開始建立拓撲與計算路徑需要較多資源。
- 大型網路需要靠 area 設計降低複雜度。

## 怎麼記

- Link State：先知道網路圖，再算路。
- Flooding：把連線狀態傳出去。
- Dijkstra：用來算較短或較佳路徑。
- OSPF：常見 Link State 協定，會用 area 管理大型網路。
- OSPF 是 IGP，主要用在同一個 AS 內部。

## 關聯筆記

- [[Router 與 Routing]]
- [[Routing Table 路由表]]
- [[Distance Vector 路由]]
- [[Hybrid Routing 與 EIGRP]]
- [[Autonomous System 與 BGP]]
- [[路由選擇 AD 與 Cost]]
