# 打包 Android APK

> 卡溜趴實際在用的做法,任何 Android App 照做。完整範例:`nmymdf/travel-ledger` 的 `app/build.gradle.kts`(簽章與指紋檢查)與 `docs/android-env-setup.sh`(雲端裝 Android SDK)。

## 0. 先確認
1. **套件名稱**(`applicationId`):給人裝過就不要再改。
2. 已經有人裝了舊版嗎?有的話新版**必須用同一把金鑰**,先找出舊金鑰;第一次就建立正式金鑰(第 3 節),之後永遠用它。
3. `./gradlew :app:assembleDebug` 能不能過。

## 1. 雲端環境
- 網路允許:`dl.google.com`、`maven.google.com`、`repo.maven.apache.org`、`plugins.gradle.org`。
- 環境的 setup script 放 travel-ledger 的 `docs/android-env-setup.sh` 內容(裝 SDK 35、build-tools 35);`local.properties` 寫 `sdk.dir=/opt/android-sdk`(不 commit)。
- 設定方式有疑問時用 `read_documentation` 查 environment.setup_script / network / secrets,再告訴爸爸去哪裡設定。

## 2. 給人用的一律是正式版
Debug 版的臨時金鑰每個容器都不同,換了環境就**無法覆蓋更新**;正式版用自己保管的金鑰,可以一直更新。

## 3. 建立正式金鑰(第一次)
```
keytool -genkeypair -v -keystore release.keystore -alias release -keyalg RSA -keysize 4096 -validity 36500 \
  -storepass <密碼> -keypass <密碼> -dname "CN=<作者>"
base64 -w0 release.keystore > release.keystore.b64
```
- 不 commit、不貼進對話。請爸爸把 `.b64` 內容存成環境的**機密環境變數**(`<APP>_KEYSTORE_B64`,密碼 `<APP>_KEYSTORE_PASSWORD`),並**另外備份金鑰檔**。金鑰遺失 = 已安裝的人永遠無法更新。
- 記下 SHA-256 指紋(`keytool -list -v -keystore release.keystore`),寫進 build 檔檢查。

## 4. build.gradle.kts
照 travel-ledger 的寫法:金鑰從環境變數(base64)或 `local.properties` 讀;`assembleRelease` 前**檢查指紋**,不符就停止。`.gitignore` 至少有 `/local.properties`、`*.keystore`、`*.jks`、`*.b64`、`build/`。

## 5. 打包、驗證、交付
```
./gradlew :app:testDebugUnitTest
./gradlew :app:assembleRelease
$ANDROID_HOME/build-tools/35.0.0/apksigner verify --print-certs app/build/outputs/apk/release/app-release.apk
```
- SHA-256 要等於記下的指紋。
- 每版 `versionCode` +1(一定要變大)、`versionName` 改新版號。
- 複製成好認的名字(`<App>-v1.0.1.apk`),用 SendUserFile 傳給爸爸。

## 6. 告訴爸爸怎麼裝
LINE 傳給自己或放雲端硬碟 → 點開 → 第一次允許「安裝不明來源的應用程式」→ 之後新版直接點開覆蓋,資料保留。不要先移除舊版(會刪資料)。

## 7. 常見問題
- 「應用程式未安裝/與現有套件衝突」:簽章不同(例如之前裝的是 debug 版),只能移除重裝。
- Gradle 下載 429/逾時:重試、檢查網路允許清單。
- 容器重開金鑰不見:正常,應該放在環境變數,build 時還原。
