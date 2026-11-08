# Impeccable Critique — Waive

Detector 例外（waive）記錄。以下兩項皆為 DESIGN.md 明定，故意為之，視為誤報，不計為瑕疵。

## hard-offset-shadow — WAIVED

- 偵測訊息：`box-shadow` 為實色偏移陰影（verdict: advisory，4 筆）。
- Waive 理由：本站深度只來自硬陰影，沒有模糊、沒有漸層，屬 DESIGN.md **Hard-Shadow Rule** 明定（陰影三級：徽章 2px、元件 4px、面板 6px，皆為 `box-shadow: Npx Npx 0 var(--ink)`，無 blur）。四筆分別對應 `.open-now` 徽章級 2px、`.menu-shortcut`/`.visit-card` 等元件級 4px、`.visit-card`/`.map` 面板級 6px，全部符合該規則。保留現狀，不得以「移除偏移陰影」作為修復方向；若未來需降噪，應調整陰影級別而非改用模糊柔光。

## cream-palette — WAIVED

- 偵測訊息：奶油底色（verdict: warning，1 筆）。
- Waive 理由：`#faf6ec` 奶油底為 DESIGN.md 色簿 **奶油底** 明定（全站頁面底色，與紙白 `#fffdf7` 卡片靠 2px 墨棕邊分明），是「門口價目牌」紙感設計的刻意選色，非盲式低飽和配色。保留現狀。

## cramped-padding — WAIVED（參考）

- 偵測訊息：hero/menu-section/contact 等「邊界貼圖」警告（warning，4 筆）。
- Waive 理由：經人工核對皆為誤報——`.container` 本身帶 44px 內縮（`width:min(1140px,calc(100% - 44px))`），且 `.section` 另有 102px（手機 70px）上下留白，內容並未貼邊。此項記錄以免未來異動時誤判。

## 吉祥物輪廓陰影 — WAIVED（DESIGN.md 例外）

- 偵測訊息：`.hero-right .mascot` 使用 `filter: drop-shadow(10px 12px 0 color-mix(in srgb,var(--deep) 14%,transparent))`（若 detector 對非 `box-shadow` 實色陰影或半透明陰影色提出 advisory，1 筆）。
- Waive 理由：首頁 Hero 大吉祥物是透明底 PNG，陰影須貼鍋體輪廓，屬 DESIGN.md「**吉祥物輪廓陰影例外**」（2026-11-08 登記）：`drop-shadow` blur 恆為 0，仍是實色偏移而非柔光；顏色用既有 `--deep`（#622027，即 HEAD 原始 `rgba(98,32,39,.11/.14)`）的低透明 `color-mix` 取樣，未新增色簿 hex，`design-system-color` 不應觸發。吉祥物保持 `rotate(-4deg)`、`width:min(430px,89%)`（≤920px `300px`、≤680px `260px`），為 2026-11-08 視覺還原決策（還原舊透明圖原始視覺，使用者已確認）。不得以「改回不透明硬陰影 box-shadow／放正／放大」作為修復方向。

## 備註

- 本檔僅記錄 waive 理由，不放寬 PRODUCT.md 紅線（門牌/電話/評分/招牌/共鍋費/IG/LINE 未提供不得代填；價格一律「170 元起，多數 190」）。
- `design-system-color` 漂移（`#744541`、`#765747`、`#846c5e`、`#f7ca70`）已於本次 P1/P3 修復中收斂至既有 token（`--ink` / `--yellow`），不再適用 waive。
- `design-system-color`（2026-11-08 審查，9 筆）已透過把湯頭三色（`--soup-gold`／`--soup-brown`／`--soup-red`）、看板紙 `--board`、聯絡米 `--contact`、虛線 `--dash`／`--dash-soft`、腳註棕 `--note`，以及紙白／沙米／亮黃紙／鍋標內底／反紅奶白／深紅陰影／開鍋綠／待機點／hero 弧形全部寫入 `styles.css` 的 `:root` token，並同步收錄進 `DESIGN.md` 色簿（Soup 與「區塊面與低強度色」兩節）。CSS 內不再有 token 以外的扁平色硬編碼；`pot-icon` 的 `rgba(56,38,32,.15)` 半透明柔影同時改為 `4px 4px 0 var(--ink)` 實色偏移（Hard-Shadow Rule）。
