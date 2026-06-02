# UDP Header

## UDP Header 很簡單

UDP 的 header 固定 **8 bytes**。

它只有 4 個主要欄位，每個欄位都是 16 bits。

| 欄位 | 長度 | 用途 |
|---|---:|---|
| Source Port | 16 bits | 來源應用程式的 port |
| Destination Port | 16 bits | 目的應用程式的 port |
| Length | 16 bits | UDP header + data 的總長度 |
| Checksum | 16 bits | 檢查資料在傳輸中是否可能出錯 |

## 為什麼 UDP 快

UDP 沒有 TCP 那麼多控制欄位。

它不需要：

- 三向交握。
- ACK 確認。
- Sequence number。
- 重傳機制。
- Sliding window。
- Flow control。

所以 header 小、處理流程少、延遲低。

## Checksum 的角色

UDP 雖然不保證可靠送達，但還是有 `Checksum`。

Checksum 用來判斷資料在傳輸過程中是否可能損壞。

要注意：

- Checksum 可以幫忙檢查錯誤。
- 但 UDP 本身不負責重傳。
- 如果資料壞掉或遺失，通常要由應用程式決定要不要重送。

## Wireshark 觀察重點

打開 UDP 封包時，可以觀察：

- Source Port
- Destination Port
- Length
- Checksum
- Payload / Data

這能幫助你確認：課本上的 header 欄位不是抽象名詞，而是真的會出現在封包裡。

## 一句話版本

UDP header 只有四欄：**來源 port、目的 port、長度、checksum**。

## 關聯筆記

- [[TCP 與 UDP]]
- [[Port number 與常見服務]]
