# 新 App 的 CLAUDE.md 範本

開新 App 時,在該 repo 最上層建立 `CLAUDE.md`,內容照下面填(`< >` 換成實際內容):

```markdown
# <App 名稱> — 給 Claude 的專案說明

**先讀 https://github.com/nmymdf/READFIRST(README.md 與其他檔案):家族 App 的分工與規則。**
這份只寫這個 App 自己的事。

## 這是什麼
<一兩句話:App 做什麼、給誰用>

## 分工
- Android:爸爸與他的 Claude,本 repo `<分支>`。
- **iPhone 版:不需要**(預設。爸爸說要做時改成「需要」,並照 READFIRST 的 `iPhone交接.md` 準備文件;之後由 Jay 在 `jaykuo0619/<repo>` 的 `ios` 分支做)。

## 文件地圖
- `docs/SPEC.md`:規格與版本紀錄
- `docs/IOS_PORT.md`、`docs/IOS_TODO.md`:給 Jay 的交接(只有「iPhone 版:需要」時才有)
- <其他>

## 常用指令
<測試、打包、產生說明書等>

## 目前狀態(每次改版更新)
- Android v<版本>;iPhone <不需要/版本>
- 還沒決定的事:<…>
```
