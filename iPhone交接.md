# 讓 Jay 做 iPhone 版:交接方式

> 爸爸沒有 Mac 和 iPhone 開發環境,iPhone 版一律由 Jay 用他 Mac 上的 Claude Code 完成。Android 這邊的 Claude 負責**把 Jay 需要的資訊寫進 App 的 repo**,爸爸只要轉告一句話。

## 新 App 第一次要做 iPhone 版
在該 App repo 建立以下文件並 push(卡溜趴的範例:`nmymdf/travel-ledger` 的 `docs/`):

1. **`docs/IOS_PORT.md`(交接規格)**,給 Jay 的 Claude 讀:
   - 先讀這裡:目標、程式放 `ios/`、不要動 Android 程式、公開 repo 不放憑證、繁中溝通、Jay 可能是第一次。
   - App 是什麼、功能總覽。
   - 技術建議(SwiftUI、資料庫、需要的套件)。
   - 資料模型(欄位意義、日期時間的存法)。
   - **跨平台檔案格式**(若 App 之間要互傳):逐欄位寫清楚、匯入規則、錯誤訊息。
   - 要照搬的邏輯與它的測試案例(能共用的測試資料放 JSON,兩邊都跑)。
   - 開發順序(每階段能跑、有測試、commit)。
   - **逐畫面細節**:畫面上的原文、預設值、限制、特殊情況,附 Android 檔案位置。建議派一個助手逐檔比對 Android 程式,避免漏掉。
   - Android 已知的小問題(iPhone 不用照抄)。
2. **`docs/IOS_TODO.md`(待辦勾選清單)**:之後每次 Android 改版,iPhone 要跟著改的項目都加在這裡,每項指回 IOS_PORT.md 的段落與 Android 的程式/測試;最上面放合併指令(見下)。
3. **相容測試檔**(若有互傳檔案):`compat/fixtures/` 放 Android 匯出的範例檔與說明;`compat/from-ios/` 讓 Jay 放 iPhone 匯出的檔,Android 端測試讀得懂。
4. **`docs/iPhone版_給家人的步驟.md`**(第一次才需要):Mac、Xcode、Claude Code、TestFlight 的操作步驟。
5. 該 App 的 `CLAUDE.md` 寫明分工(見 `新App的CLAUDE.md範本.md`)。

## 之後每次 Android 改版
- 有影響 iPhone 的改動:更新 `docs/IOS_PORT.md` 的對應段落,在 `docs/IOS_TODO.md` 加一行(版本、項目、規格段落、Android 參考),push。
- 回報爸爸:「iPhone 要跟著改的已寫進 IOS_TODO.md」。

## 爸爸轉告 Jay 的話(固定這一句)
> 「`<App repo>` 有更新,請合併後讀 `CLAUDE.md` 和 `docs/IOS_TODO.md`,照清單處理 iPhone 版。」

## Jay 那邊合併的指令(寫在 IOS_TODO.md 最上面)
```
git remote add upstream https://github.com/nmymdf/<repo>.git   # 第一次才需要
git fetch upstream <爸爸的分支>
git checkout ios
git merge upstream/<爸爸的分支>
```
Jay 的 iPhone 程式在他 fork 的 `ios` 分支、`ios/` 資料夾。iPhone 版做完後,他把 iPhone 匯出的相容檔放 `compat/from-ios/` 並 push;Android 這邊再跑相容測試確認。
