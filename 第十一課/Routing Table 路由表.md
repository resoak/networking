# Routing Table 路由表

## Routing Table 是什麼

`Routing table` 是 Router 用來決定封包轉送方向的表。

可以把它想成 Router 的地圖或導航：

> 封包要去某個目的網路時，Router 查 routing table，決定要從哪個 interface 或下一跳送出去。

## Router 查表時在看什麼

當封包進入 Router，Router 會看封包的目的地 `IP address`，再查表判斷：

- 目的網路是否已知。
- 封包應該從哪個 interface 離開。
- 下一跳 Router 的位址是什麼。
- 這筆路由是直接連接、靜態設定，還是動態學到。

如果路由表裡找不到可用路徑，Router 可能就會丟棄封包。

## 直接連接路由

`Directly connected route` 是 Router interface 直接連到的網路。

例如一台 Router 有兩個 interface：

| Interface | 連接網路 |
|---|---|
| `Ethernet 0` | `192.168.0.0` |
| `Ethernet 1` | `10.0.0.0` |

當 interface 設定好 `IP address` 與 `subnet mask` 後，Router 就能知道這些網路是直接接在自己身上的，因此會自動放進 routing table。

## 只知道直接連接還不夠

課堂影片用一個例子說明：

- Router A 直接連到 `192.168.0.0`、`10.0.0.0`、`125.0.0.0`。
- 但目的地是 `174.16.0.0`。
- 如果 Router A 的表裡沒有 `174.16.0.0` 的路由，它就不知道封包該往哪裡送。

這時候就需要：

- 手動加入 [[Static 與 Dynamic Routing|static route]]。
- 或讓 Router 透過 dynamic routing protocol 自動學到。

## Routing Table 的來源

路由表的資料主要有三種來源：

| 來源 | 說明 |
|---|---|
| Directly connected | Router 自己的 interface 直接連到的網路 |
| Static | 網管人員手動設定的路徑 |
| Dynamic | 其他 Router 透過路由協定交換後學到的路徑 |

## 白話理解

可以把 Router 想成站在十字路口的交通指揮。

- 直接連接路由：它眼前看得到的路。
- 靜態路由：有人事先告訴它「要去某地就往這邊走」。
- 動態路由：其他路口的指揮會回報路況，它再更新自己的導航。

## 怎麼記

- `Routing table` 是 Router 的導航表。
- 封包進來後，Router 查目的地 IP，再決定出口或下一跳。
- 直接連接只知道自己接到的網路，不一定知道更遠的網路。

## 關聯筆記

- [[Router 與 Routing]]
- [[Static 與 Dynamic Routing]]
- [[Distance Vector 路由]]
