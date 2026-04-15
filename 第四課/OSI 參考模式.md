# OSI 參考模式

## 重點

- `OSI` = Open Systems Interconnection。
- 是 `ISO` 在 1984 年提出的參考模型。
- 作用是把網路技術拆成七層，方便設計、學習與溝通。

## 七層

1. 應用層 Application
2. 呈現層 Presentation
3. 會議層 Session
4. 傳輸層 Transport
5. 網路層 Network
6. 資料連結層 Data Link
7. 實體層 Physical

## 核心觀念

- 越低層越接近硬體。
- 越高層越接近軟體。
- 資料往下傳時會逐層加上控制資訊，這就是**封裝**。

## 各層資料單位

- Message
- Data
- Segment
- Packet
- Frame
- Signal

## 例子

- 傳輸層：`TCP`、`UDP`
- 網路層：`IP`
- 資料連結層：網路卡、橋接器、交換器
- 實體層：線材、連接頭、中繼器

## 怎麼記

- 背七層不是目的，重點是知道：
  - 資料如何一層一層往下包
  - 各設備大概工作在哪一層

## 連到其他主題

- [[協定與標準組織]]
- [[TCP-IP 與 RFC]]
