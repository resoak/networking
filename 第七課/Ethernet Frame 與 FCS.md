# Ethernet Frame 與 FCS

## 重點

- Ethernet 傳資料時不是一個位元一個位元亂送，而是用 `Frame` 的格式來包。
- 錄音中老師提到 `802.2`、欄位格式、以及放在最後面的檢查資訊。
- 可以把 Ethernet Frame 先記成幾個核心區塊：
  - 目的 MAC 位址
  - 來源 MAC 位址
  - Type / Length
  - Payload
  - FCS

## 802.2 的位置

- 老師在這段提到 `802.2`，重點是在提醒大家：802 家族不只管媒體存取，還有更上層的格式定義。
- 這一段不用先背得很細，但要知道 Ethernet 的欄位設計不是隨便排的，而是有標準格式。

## FCS / CRC 在做什麼

- 錄音裡反覆提到「檢查」與「資料最後面放一段資訊」。
- 這個概念就是 `FCS`，常搭配 `CRC` 來做錯誤檢查。
- 作用是讓接收端知道：
  - 資料在傳送過程中有沒有壞掉
  - 這筆資料還能不能相信

## 這段要抓的核心

- Frame 幫你把資料包成可辨識的格式。
- MAC 位址欄位幫你標出誰傳、要傳給誰。
- 最後面的 FCS 幫你做基本錯誤檢查。

## 怎麼記

- `前面看誰收、誰送`
- `中間放資料`
- `最後做檢查`

## 連到其他主題

- [[IEEE 802 與 Ethernet]]
- [[MAC 位址與網路設定]]
- [[Hub、Bridge、Switch]]

## 簡報補充

- 投影片把 Ethernet 訊框重點再補成下面幾個欄位：
  - `Preamble`
  - `Destination MAC`
  - `Source MAC`
  - `Type / Length`
  - `Data`
  - `FCS`
- `MAC` 位址長度是 **48 bits = 6 Bytes**。
- 廣播位址可以直接記成：`FF-FF-FF-FF-FF-FF`。
- `Data` 欄位一般記成 **46 ~ 1500 Bytes**；太短要補 `Padding`。
- `FCS` 是 **4 Bytes / 32 bits**，常用 `CRC-32` 來做錯誤檢查。
