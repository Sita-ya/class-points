# 班級經營與集點系統

單一網頁（`index.html`），用瀏覽器開啟即可使用。資料存在瀏覽器；登入 Google 後可雲端同步。

## 一、發佈到 GitHub Pages（約 10 分鐘）

1. 註冊／登入 https://github.com
2. 右上角 `+` → **New repository**
   - Repository name：`class-points`（可自訂，英文小寫）
   - 選 **Public**（GitHub Pages 免費版需要公開；學生資料不在程式碼裡，是存在你的瀏覽器／Firebase，所以公開沒關係）
   - **不要**勾 Add a README → 按 **Create repository**
3. 在新頁面點 **uploading an existing file**
4. 把本資料夾裡的 `index.html`（和 `README.md`）拖進去 → 下方按 **Commit changes**
5. 上方 **Settings** → 左側 **Pages**
   - Source：**Deploy from a branch**
   - Branch：**main**，資料夾 **/(root)** → **Save**
6. 等 1～2 分鐘，重新整理 Pages 頁面，頂端會出現網址：
   `https://你的帳號.github.io/class-points/`

之後要更新：repository 首頁 → `index.html` → 右上鉛筆 → 貼上新內容 → Commit；
或 **Add file → Upload files** 覆蓋同名檔案。

## 二、設定雲端同步（Firebase）

1. https://console.firebase.google.com → **建立專案**（名稱例：class-points；Google Analytics 可關閉）
2. 左側 **Build → Authentication** → **開始使用** → 登入方式 → **Google** → 啟用、選支援電子郵件 → 儲存
3. Authentication → **設定** → **已授權的網域** → **新增網域**：填 `你的帳號.github.io`（只填網域，不含 /class-points）
4. 左側 **Build → Firestore Database** → **建立資料庫** → 選 **正式版模式** → 地區選 `asia-east1`（台灣）→ 啟用
5. Firestore → **規則** 分頁，整段貼上並按 **發布**：

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /users/{uid}/{document=**} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```
6. 專案總覽旁齒輪 → **專案設定** → **一般** → 下方「您的應用程式」→ 點 `</>`（網頁）→ 取個暱稱 → 註冊應用程式（**不用**勾 Hosting）
7. 複製畫面中的 `firebaseConfig` 內容（apiKey、authDomain、projectId、storageBucket、messagingSenderId、appId）
8. 用文字編輯器（記事本即可）打開 `index.html`，找到檔案最上方 `window.FIREBASE_CONFIG = { ... }`，把每個空白 `""` 換成你複製的值，存檔
9. 重新上傳 `index.html` 到 GitHub（見上方「之後要更新」）
10. 等 1～2 分鐘，開啟 GitHub Pages 網址 → 右上角 **Google 登入**
    - 第一次登入若這台瀏覽器已有舊資料，會問你是否上傳到雲端 → 選「上傳到雲端」
    - 右上角出現綠點「已同步」就完成了

> firebaseConfig 的 apiKey 公開在網頁裡是正常的，不是密碼；真正保護資料的是第 5 步的安全規則。

## 三、日常使用提醒

- 換電腦或平板：開同一個網址、用同一個 Google 帳號登入，資料會自動出現。
- 教室網路不穩時照常操作，恢復連線後自動上傳（燈號：綠＝已同步、黃＝同步中、紅＝離線）。
- 仍建議定期在首頁按「匯出全部資料（JSON 備份）」。
- 不想用雲端：不填 firebaseConfig 即可，維持本機模式。
