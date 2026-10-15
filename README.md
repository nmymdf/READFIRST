# READFIRST:家族 App 開發流程

> **給 Claude:每個專案開工前先讀這份**(以及同一個 repo 裡的其他檔案)。讀完就照這裡的分工與規則做,不用再問使用者這些事。
> 各 App 自己的細節寫在該 App repo 的 `CLAUDE.md`;這裡是所有 App 共用的規則。

## 1. 分工

| 誰 | 做什麼 | 工具 |
|---|---|---|
| **爸爸**(GitHub `nmymdf`,本 repo 擁有者) | 想功能、試用;**Android 版**與電腦網頁版,由他的 Claude(雲端 Claude Code)開發與打包 APK | 沒有 iPhone、沒有 Mac |
| **Jay**(兒子,GitHub `jaykuo0619`) | **iPhone / iPad 版**:由他 Mac 上的 Claude Code 照交接文件移植、用 Xcode 編譯、TestFlight / App Store 上架 | 有 Mac、有 Apple 開發者帳號 |

- **預設只做 Android,不做 iPhone 版。** 只有在該 App 的 `CLAUDE.md` 寫「iPhone 版:需要」,或爸爸說要做 iPhone 版時,才處理 iPhone 的事(讀 `iPhone交接.md`、準備給 Jay 的文件)。沒寫的 App,不要提 iPhone、不要產生 iPhone 文件。
- **爸爸不需要、也不能編譯 iPhone 版**。不要叫他裝 Xcode、不要說「你沒辦法編譯 iPhone app」;iPhone 的事一律變成**給 Jay 的交接文件**(見 `iPhone交接.md`)。
- Android 這邊的 Claude **不寫 iOS 程式**、不動 `ios/`;Jay 那邊的 Claude 不動 Android 的程式。兩邊靠 repo 裡的文件溝通。

## 2. 每次 Android 出新版,自動要做的事

1. 打包正式版 APK、驗證簽章,傳給爸爸(方法見 `打包APK.md`)。
2. **(只限「iPhone 版:需要」的 App)** 只要有使用者看得到的行為改變,就同步更新給 Jay 的交接文件:該 App repo 的 `docs/IOS_PORT.md`(規格)與 `docs/IOS_TODO.md`(待辦勾選清單),一起 push。不用等爸爸開口。
3. (同上)回報時用一兩句話說「iPhone 要跟著改的已經寫進 IOS_TODO.md」,爸爸轉告 Jay 即可。
4. 多個版本(Android、iPhone、網頁)之間互傳的**檔案格式**:能不改就不改;要改先問爸爸,並在 IOS_TODO.md 標明。

## 3. 跟爸爸溝通

- 一律**繁體中文、白話**,少用術語;他常用語音輸入,字可能有錯,依上下文理解。
- 他說「**先討論**」「**先回答**」「**不動手**」:只說明與提建議,**不改程式**;等他說「**好**」才動手。
- 給建議,不要丟一大串選項;牽涉畫面的改動,先做預覽截圖給他看。
- 回報要說清楚:做了什麼、怎麼驗證、哪些要他在手機上試。
- 需要他設定東西(例如金鑰、環境變數)時,告訴他在哪個畫面、按哪裡。

## 4. 一定要遵守

- repo 多半是**公開的**:簽章金鑰、`.p12`、密碼、token **不能 commit**,也**不要請爸爸把它們貼進對話**;放在雲端環境的機密環境變數。
- 沒有被要求就**不要開 PR**;commit 訊息不放模型名稱。
- 介面:**任何畫面都不能出現直排字**(窄欄把中文擠成一字一行)、大字體下不能切字;字不縮小,靠減少留白。
- 改了功能,同步更新該 App 的規格、說明書與 iPhone 交接文件。

## 5. 本 repo 的檔案

| 檔案 | 什麼時候讀 |
|---|---|
| `README.md` | 每次開工(就是這份) |
| `打包APK.md` | 要做 Android APK 給人安裝時 |
| `iPhone交接.md` | **只在需要 iPhone 版時**:該 App 的 `CLAUDE.md` 寫「iPhone 版:需要」,或爸爸說要做 |
| `新App的CLAUDE.md範本.md` | 開新 App 時,照這個建立該 repo 的 `CLAUDE.md` |

## 6. 目前的 App

| App | Repo | iPhone 版 | 狀態 |
|---|---|---|---|
| 卡溜趴(旅遊記帳+行程) | `nmymdf/travel-ledger`(分支 `claude/confident-volta-6rsegv`);iPhone 在 `jaykuo0619/travel-ledger` 的 `ios` 分支 | 需要 | Android v0.10.6、iPhone 1.0.0 |

新增 App 時在這張表加一行。
