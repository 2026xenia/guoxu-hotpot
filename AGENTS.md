# AGENTS.md — 鍋序 guoxu-hotpot

手寫靜態行銷網站：HTML + 單一 `styles.css`，**沒有建置、套件管理、測試或 CI**。部署＝把 `main` 推到 GitHub，GitHub Pages 直接從 repo 根目錄供檔（`https://2026xenia.github.io/guoxu-hotpot/`）。不要引入 npm／bundler／框架，除非使用者明確要求。

本檔只談這個 repo。

## 先讀：兩份唯一真實來源

- `PRODUCT.md` — 頁面／關鍵字、價格、營業時間、店家事實、文案紅線。改文案先看它。
- `DESIGN.md` — 色彩、字體、硬陰影、元件規格。視覺爭議以它為準。
- 文件與程式碼衝突時，以程式碼為事實並回頭修文件。

## 檔案地圖

- `index.html` 首頁（`Restaurant` JSON-LD、`#hours` 逐日表）
- `menu.html` 菜單（逐項價格唯一來源：`<td class="price">`）
- `pot.html`／`guide.html` 鍋品介紹、吃鍋指南（`WebPage` + `about: Restaurant` JSON-LD）
- `visit.html` 交通（地址、電話、停車、地圖）
- `404.html`（`noindex`，不進 sitemap）；`styles.css`；`assets/`；`robots.txt`；`sitemap.xml`
- `measure.html` 本機量測用臨時頁（目前未納入 git）：不連首頁、不進 sitemap、不要部署

## 沒有樣板引擎：共用區塊是複製貼上的

每頁各有一份 header、nav、footer、skip link，以及同一段 inline JS（漢堡選單＋`.open-now` 開鍋狀態，含 `HOURS` 常數）。**改導覽／頁首／頁尾／營業狀態必須逐頁改**：index、menu、pot、guide、visit（404 另有簡化版 header/footer）。

## 改資料的連動清單（最容易漏）

改價格：

1. `menu.html` 的 `<td class="price">`
2. 首頁 hero「170 元起・多數 190」與價目牌 `board-row` 區間
3. `title`／`meta description`／`og:*`（index、menu）
4. JSON-LD `priceRange`
5. `pot.html`／`guide.html` 內文寫死的價位帶

→ 一律保留「170 元起，多數 190」的誠實寫法，不可只寫「170 元起」。

改營業時間（以週六為例）：

1. 5 頁 header 的 `.open-now-text` 靜態字串
2. 5 頁 footer `.footer-note`
3. `index.html` `table.hours-table`（`data-day="6"`）
4. `index.html` JSON-LD `openingHoursSpecification`（週六單獨一條）
5. 5 頁 inline JS 的 `HOURS` 物件
6. `visit.html` 摘要句、`guide.html` FAQ

→ 少一處就會出現「靜態字說 17:00、徽章說 11:00」的矛盾。

## 內容紀律（E-E-A-T 就是本專案的目的）

- 不編品牌故事、不寫「招牌」、不寫評分／評論數／媒體報導、不自創品名（沒有「沙茶豬肉鍋」）。
- 未確認資料留「待補」chip，不得代填。
- 繁體中文、`lang="zh-Hant"`；每頁只有一個 `<h1>`，h1 保留地名關鍵字。
- 免 JS 可讀：菜單價格是純 HTML，不要在菜單頁加價格腳本；關 JS 仍要看得到營業時間。

## 樣式規則

- 顏色只取 `styles.css` 第 1 行 `:root` 變數；不新增 hex、不引入第二個強調色（紅＝唯一行動色）。
- 不用漸層、不用模糊陰影；深度只用實色偏移 2/4/6px。吉祥物 `drop-shadow` 是唯一明文例外。
- 字型只有 Noto Sans TC 400/700/900。
- `styles.css` 已是壓縮長行，且慣例是**在檔尾按日期追加新區塊**（`2026-11-03`、`11-07`、`11-09`…）。照這樣加，不要重排或展開整份檔案。
- 斷點只有 1100／920／680px；觸控目標 ≥44px。

## 新增頁面

複製任一內頁當模板（head meta + canonical + og + JSON-LD + header/footer + inline JS），`aria-current="page"` 指到自己，補進所有頁面的 nav 與 footer，更新 `sitemap.xml` 的 `lastmod`。全站用絕對網址 `https://2026xenia.github.io/guoxu-hotpot/...`；換網域要同步 canonical／`og:url`／`og:image`／sitemap／JSON-LD。

## 驗證（沒有測試，只能手動）

- repo 根目錄跑 `python -m http.server 8000` → `http://localhost:8000/`（頁面全用相對路徑，直接開檔也可）。
- 檢查：五頁導覽／footer 一致、單一 `<h1>`、關 JS 可讀菜單與時間、360px 與 920px 寬不爆版。
- 不要為了「補測試」新增工具鏈。
- commit 慣例（沿用 git log）：`feat:`／`fix:`／`style:`／`content:`／`docs:`／`refactor:` + 中文描述。
