# DHCP 與 DNS

## DHCP

- `DHCP` = Dynamic Host Configuration Protocol
- 用途是**自動分配 IP 位址**。
- 網路中通常要有 `DHCP Server`。
- 設備上線後會用廣播方式要求設定。
- 有 `lease` 概念，避免位址長期被占住。

## DHCP 為什麼重要

- 如果每台電腦都手動設定 IP：
  - 容易重複
  - 容易設錯
  - 管理量很大
- `DHCP` 的價值就是把這件事集中處理。
- 對教室、公司、宿舍、家用分享器都很實用。

## DHCP 大概怎麼運作

- 先把概念記成：
  1. 主機剛上線，先問「有沒有人能給我 IP？」
  2. DHCP Server 回覆可用設定
  3. 主機選擇並要求使用
  4. Server 確認租給它
- 你不用一開始硬背封包流程，但要知道它是**自動協調、動態租用**。

## DNS

- `DNS` = Domain Name System
- 功能是把人比較好記的網域名稱，轉成電腦實際使用的 IP 位址。

## DNS 為什麼重要

- 人比較擅長記名字，例如：`www.flag.com.tw`
- 電腦真正拿來找主機的是 IP 位址。
- `DNS` 就是把「人類看得懂的名字」翻成「機器真正拿來連線的位址」。

## FQDN

- `FQDN` = Fully Qualified Domain Name
- 例如：`www.flag.com.tw.`

## DNS 的階層觀念

- `Root`
- `TLD`
- `Domain`
- `Host`

### 先這樣理解

- `www.flag.com.tw.` 中：
  - `www`：主機名稱
  - `flag`：網域名稱
  - `com` / `tw`：較高層的網域結構
- 所以 DNS 不是亂對照，而是**有階層、有管理單位**的命名系統。

## DNS 查詢

- 正向查詢：名稱 → IP
- 反向查詢：IP → 名稱

## 遞迴與反覆查詢

- `Recursive Query`：你把問題丟給 DNS Server，它幫你一路問到答案。
- `Iterative Query`：DNS Server 不一定替你問到底，但會告訴你下一步應該去問誰。

## 管理單位

- `ICANN`：全球網域與位址管理的重要單位。
- `TWNIC`：台灣和 `.tw` 相關管理常會提到的單位。

## 常見概念

- `Root`
- `TLD`
- `gTLD`
- `ccTLD`
- `Resolver`
- `Recursive Query`
- `Iterative Query`

## 補充例子

- 正向查詢：輸入 `www.flag.com.tw`，DNS 幫你找到對應 IP。
- 反向查詢：已知某個 IP，反查它對應的名稱。
- `DHCP` 與 `DNS` 常一起出現：
  - `DHCP` 先幫你拿到 IP
  - `DNS` 再幫你把網址翻成 IP

## 怎麼記

- `DHCP` 幫你**拿到 IP**。
- `DNS` 幫你**把名字翻成 IP**。

## 一句話版本

- `DHCP`：自動發號碼
- `DNS`：把名字翻成號碼

## 連到其他主題

- [[IP 位址與子網路]]
- [[IPv4 標頭與 IPv6]]
