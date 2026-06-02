# ARP 與 RARP

## 為什麼需要 ARP

第八課學的 `IP address` 是**邏輯位址**，用來告訴網路「資料要送到哪一台主機」。

但在區域網路中，真正送出乙太網路 frame 時，第二層還需要填入**來源 MAC** 和 **目的 MAC**。所以主機只知道 `IP address` 還不夠，還必須先知道對方的 `MAC address`。

`ARP` 的工作就是：

> 把 `IP address` 解析成 `MAC address`。

## 一句話記法

- `ARP`：`IP → MAC`
- `RARP`：`MAC → IP`

## ARP 的白話理解

可以把 `IP address` 想成住址，`MAC address` 想成那張網卡的身分證號。

主機在區域網路裡送資料時，如果只知道對方住哪裡，還是不夠。它還要知道收件設備的第二層位址，這樣交換器與網卡才知道 frame 要交給誰。

所以 ARP 的核心問題是：

- 我知道對方的 `IP address`。
- 但我還不知道對方的 `MAC address`。
- 這時候就要用 `ARP` 去問。

## ARP Request / Reply 流程

### 基本流程

1. 傳送端先檢查自己的 `ARP cache`。
2. 如果沒有目標 IP 的對照，就送出 `ARP request`。
3. `ARP request` 會用**廣播**方式送到同一個區域網路。
4. 同網段所有主機都會收到這個 request。
5. 只有目標 IP 那台主機會回 `ARP reply`。
6. 傳送端收到 reply 後，把 `IP ↔ MAC` 寫進 `ARP cache`。
7. 之後才用這組 MAC 封裝真正要送的資料。

### 白話例子

主機 A 想找 `192.168.1.10` 的 MAC，流程像這樣：

- A 對大家喊：**誰是 `192.168.1.10`？請告訴我你的 MAC。**
- 同網段所有設備都會聽到。
- 只有 `192.168.1.10` 會回：**我是 `192.168.1.10`，我的 MAC 是 `xx-xx-xx-xx-xx-xx`。**
- A 把這筆資料記起來，之後就能直接送資料。

### 補一句更精確的說法

雖然我們常口語說「去問目標主機的 MAC」，但真正送出去的第一個 `ARP request` 並不是直接單播給目標主機，因為此時還不知道它的 MAC。

更精確的說法是：

- 針對**目標 IP** 發出一個**廣播 ARP request**。
- 交換器會把這個廣播 frame 往其他連接埠轉送。
- 不符合的主機會丟棄。
- 只有擁有該 IP 的主機回覆 `ARP reply`。

## 同網段與不同網段

這是 ARP 最常考也最容易混的地方。

### 同網段

如果目標 IP 和自己在**同一個子網路**，主機會：

- 直接查目標主機的 MAC。
- 針對目標 IP 發出廣播 ARP request。
- 拿到目標主機的 MAC 後，直接傳送 frame。

### 不同網段

如果目標 IP 和自己**不在同一個子網路**，主機不會直接問遠端主機的 MAC，因為 ARP 廣播只在本地區網有效。

它會改成：

- 先找 `default gateway` 的 MAC。
- 第二層的目的 MAC 填 Router 的 MAC。
- 第三層的目的 IP 仍然是遠端主機的 IP。
- 之後由 Router 幫忙往外轉送。

### 重點整理

| 情況 | ARP 查誰的 MAC | 第二層目的 MAC | 第三層目的 IP |
|---|---|---|---|
| 同網段 | 目標主機 | 目標主機的 MAC | 目標主機 IP |
| 不同網段 | Default Gateway | Router 的 MAC | 遠端主機 IP |

## RARP 是什麼

`RARP` 全名是 `Reverse Address Resolution Protocol`。

它的方向和 ARP 相反：

> 已知 `MAC address`，反查 `IP address`。

### 為什麼以前會需要 RARP

較早期有些設備開機時，可能只知道自己的網卡位址，但不知道該用哪個 IP。這時候會透過 `RARP` 向伺服器詢問。

### 現在怎麼看待 RARP

- 它是**歷史上曾重要**的機制。
- 現在實務上比 ARP 少見很多。
- 現代網路通常改用 `BOOTP`、`DHCP` 這類機制取得 IP。
- 考試或上課時，重點通常只是知道它和 ARP 的方向相反。

## ARP / RARP 封包觀念

課堂中也提到，ARP 與 RARP 的封包格式很像，差別主要在操作類型。

可以先掌握概念，不用死背每個 bit：

- 硬體類型，例如 Ethernet。
- 協定類型，例如 IPv4。
- 硬體位址長度，例如 MAC 長度。
- 協定位址長度，例如 IP 長度。
- 操作碼，代表 request 或 reply。
- 來源硬體位址、來源協定位址。
- 目的硬體位址、目的協定位址。

### 課堂中提到的辨識值

- `ARP` 在乙太網路中常見 EtherType 是 `0x0806`。
- `RARP` 歷史上常見 EtherType 是 `0x8035`。

這不是每次都要死背，但在 Wireshark 或封包欄位中看到時，要能知道它在分辨哪一種協定。

## 分層角度怎麼看 ARP

ARP 很容易讓人混亂，因為它同時碰到兩層資訊：

- 第三層知道的是 `IP address`。
- 第二層真正送 frame 時需要的是 `MAC address`。

所以可以把 ARP 記成：

> 它是在幫第三層的 IP 資訊，找到第二層真正要封裝的 MAC 位址。

## Wireshark 觀察重點

看 ARP 封包時，可以注意：

- `Who has x.x.x.x? Tell y.y.y.y`，這通常是 request。
- `x.x.x.x is at aa:bb:cc:dd:ee:ff`，這通常是 reply。
- request 常是廣播送出。
- reply 通常回給原本發問的主機。

## 怎麼記

- `ARP` 解決的是第二層封裝前的位址問題。
- 同網段找目標主機 MAC。
- 不同網段找 `default gateway` MAC。
- `RARP` 是反方向，偏歷史性補充。

## 關聯筆記

- [[ARP Cache 與指令]]
- [[ICMP 與 Ping]]
- [[網路連線診斷流程]]
