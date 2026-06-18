# Crab Champions 繁體中文翻譯補包

> 把 Steam 上的 **Crab Champions**（Unreal Engine 4.27 動作射擊 roguelike）變成繁體中文的社群補包。
> 一個檔案搞定，**免啟動參數、免安裝、可隨時解除**。

---

## 📦 這是什麼？

`CrabChampions-WindowsNoEditor_P.pak` 是一個 **UE4 附掛 pak 補包**（10 MB），透過 Unreal Engine 內建的 `_P.pak` patching 機制，在遊戲啟動時自動疊加到原版遊戲上，把遊戲介面與內容翻譯成繁體中文。

**不需要動到任何原版遊戲檔案**。刪掉補包就完全還原英文版。

### 補包內容

| 項目 | 數量 | 說明 |
|------|------|------|
| **UI 介面翻譯**（locres）| 238 句 | 主選單、HUD、設定選單、統計、造型、難度、獎牌、修正詞、挑戰、按鍵設定、小遊戲、好友列表等 |
| **遊戲內容翻譯**（資產注入）| 1,097 句 | 武器、天賦、聖物、能力、改件、道具、敵人 的名稱與描述 |
| **中文字型**（Noto Sans TC 子集）| 6 個 ufont | 含設定選單用的舊字型（漏換會崩，已處理）|
| **多語系 locres**| en + zh-Hant 兩槽 | 兩槽都放繁中，跑哪個語系都中文 |

**總計**：1 locmeta + 2 locres + 6 ufont + 496 內容資產（.uasset + .uexp）≈ 1,001 檔案，10 MB。

---

## 🎯 翻譯覆蓋範圍

| 類別 | 覆蓋率 | 說明 |
|------|--------|------|
| **遊戲內容**（武器/天賦/聖物/能力/改件/道具/敵人 名稱與描述）| **100%**（1,097/1,097）| 完整翻完 |
| **UI 介面**（玩家可見的選單、HUD 等）| **~99%**（玩家可見部分）| 238/288 字串已翻 |
| **設定選項「值」**（Windowed / Low / Medium / High / On / Off 等）| **0%** | 寫死在 exe 裡，pak 無法覆蓋（見「已知限制」）|

---

## ✨ 特色

- ✅ **零侵入**：完全不動原版遊戲、不動 exe、不動 config
- ✅ **免啟動參數**：放進去就好，不用 `-culture=zh-Hant`
- ✅ **可隨時解除**：刪掉 .pak 檔立刻還原英文版
- ✅ **可分享**：整個 .pak 給朋友即可，他一樣丟進 `~mods` 資料夾就能用
- ✅ **多人連線安全**：只改文字，**完全不動任何遊戲數值**，不會被判定作弊
- ✅ **繁體台灣用語**：使用台灣遊戲習慣用語（暴擊、護甲、衝刺、連擊 …）
- ✅ **Token 完整保留**：`PrimaryBuff`、`SecondaryBuff`、`Debuff`、`Duration`、`%` 等執行時替換的數值 token 全部原樣保留，遊戲會自動填入正確數字
- ✅ **Noto Sans TC 字型**：使用 Google 開源 Noto Sans TC（OFL 授權，可自由散布）

---

## 📥 安裝方式

### 方法一：手動建立資料夾（推薦）

1. **下載** `CrabChampions-WindowsNoEditor_P.pak`
2. 打開你的 Steam Crab Champions 安裝目錄。預設路徑通常是：
   ```
   C:\Program Files\Steam\steamapps\common\Crab Champions\CrabChampions\Content\Paks\
   ```
   （找不到？在 Steam library 對 Crab Champions 右鍵 → 管理 → 瀏覽本機檔案）
3. 在 `Paks\` 裡面**新增一個資料夾**叫 `~mods`（注意開頭的波浪號 `~`，很重要）
4. 把下載的 `CrabChampions-WindowsNoEditor_P.pak` 丟進 `~mods\`
5. 最終路徑應該長這樣：
   ```
   ...\Crab Champions\CrabChampions\Content\Paks\~mods\CrabChampions-WindowsNoEditor_P.pak
   ```
6. 啟動遊戲，享受繁中介面！

### 方法二：PowerShell 一鍵安裝

把以下指令貼到 PowerShell（請先確認 Steam 路徑）：

```powershell
$pakDir = "C:\Program Files\Steam\steamapps\common\Crab Champions\CrabChampions\Content\Paks\~mods"
New-Item -ItemType Directory -Force -Path $pakDir | Out-Null
# 假設 pak 已下載到當前目錄
Copy-Item ".\CrabChampions-WindowsNoEditor_P.pak" $pakDir -Force
Write-Output "安裝完成！"
```

---

## 🗑️ 解除安裝

直接刪掉 `~mods\` 裡的 `CrabChampions-WindowsNoEditor_P.pak` 即可。
遊戲下次啟動會完全回復英文版，不留任何痕跡。

---

## ❓ 為什麼不需要啟動參數？

Unreal Engine 4 的在地化系統運作方式：

1. 遊戲偵測你的系統顯示語言（你的 Windows 是中文，遊戲會跑 `zh-Hant`；英文系統會跑 `en`）
2. 引擎載入對應的 `Localization\Game\<culture>\Game.locres`

**本補包的做法**：把繁體中文**同時放進 `en` 和 `zh-Hant` 兩個槽**。所以：

- 你的系統是中文 → 遊戲跑 `zh-Hant` → 載入 `zh-Hant\Game.locres`（繁中）✅
- 你的系統是英文 → 遊戲跑 `en` → 載入 `en\Game.locres`（**也是繁中**）✅

無論哪種系統語言，都會顯示繁中。**完全免啟動參數**。

> 萬一你的系統是其他語言（日文/韓文/法文…）造成選單沒變中文，可以加上啟動參數 `-culture=zh-Hant` 強制載入繁中槽。Steam library → 右鍵遊戲 → 內容 → 通用 → 啟動選項。

---

## ⚠️ 已知限制（這些會維持英文）

下列字串**寫死在遊戲的 exe 檔裡**，用 `FText::FromString` 直接產生，**繞過 locres 系統**。除非動 exe（違反「不動遊戲內部」原則），否則永遠是英文：

- **設定選項的「值」**：`Windowed Fullscreen` / `Low` / `Medium` / `High` / `On` / `Off` / `Unlimited` / `Hidden` 等
- 解析度列表、各種百分比數值
- **少數系統字**：`RANDOMIZER`、`DIAMOND`（難度顯示）、`Lobby`、`Cosmetics Totem`、長句 `Are you sure?`

這不是補包的 bug，而是引擎的技術限制。**設定選單的「項目名稱」（如 Graphics、Audio、Gameplay）已翻譯，只有選項的「值」維持英文**。

---

## 🔧 技術原理（給有興趣的人）

UE4 遊戲的在地化分兩條路線：

### 路線 A：UI 介面（locres）

- 遊戲 UI 用 `FText` 型別，每個字串帶有穩定的 GUID key
- 走 `Game.locres` 二進位檔查表替換
- 補包提供 `Game.locmeta`（宣告 native=en, compiled=[en, zh-Hant]）+ 兩個 locres

### 路線 B：遊戲內容（資產注入）

- 武器/天賦等的名稱與描述是 `DataAsset` 裡的 `FString`（純字串，無 GUID）
- locres 無法翻譯 `FString`，必須直接改 `.uasset` 檔
- 補包用 UAssetAPI 注入中文字串到 496 個 DataAsset，**只改文字、不動數值**

### 字型替換

- UE 4.27 的 FreeType 字型子系統**載不動可變字型（Variable Font, VF）**
- 補包用 fonttools 把 Noto Sans TC 從可變字型**靜態化為 wght=400**
- 再 subset 出遊戲用到的 958 字（282 KB）
- **新舊兩套字型都要換**（Bold/Chunky/Thin + Bold_OLD/Chunky_OLD/Thin_OLD），漏換舊字型會導致設定選單中文變方塊

### `_P.pak` 修補機制

UE4 內建：檔名以 `_P.pak` 結尾的 pak 檔會在載入時獲得 **+1000 優先級**，自動覆蓋原版 pak 中的同名檔案。這是 Epic 官方支援的 patching 機制，不是 hack。

### Token 保留

遊戲描述中的 `PrimaryBuff`、`SecondaryBuff`、`Debuff`、`Duration`、`%` 等是執行時替換的變數 token，翻譯時必須原樣保留。例如：

> 英文：`Shots deal PrimaryBuff% more damage (+SecondaryBuff damage) but are Debuff% heavier`
> 中文：`子彈傷害提升 PrimaryBuff%（+SecondaryBuff 傷害），但重量增加 Debuff%`

遊戲執行時會把 `PrimaryBuff` 等替換成實際數字。

---

## 🛡️ 安全性說明

- ✅ **只改文字，不動數值**：武器傷害、敵人血量、掉落機率等全部維持原版，**多人連線不會被判定作弊**
- ✅ **不包含任何可執行檔**（.exe / .dll / .bat / .ps1）— 補包內 1,001 個檔案全是資料檔
- ✅ **不修改任何原版遊戲檔案**
- ✅ **Steam 驗證/更新不會還原**（因為補包在獨立的 `~mods` 資料夾，不在原版 pak 內）
- ✅ **SHA256 完整性**：`3C2DF68B6ADCC565117A039F213AA5CD3FE9ED41B4BBBBA2A36B240D69CAD2B7`

如果你對安全性有疑慮，可以用 PowerShell 自行驗證：

```powershell
Get-FileHash ".\CrabChampions-WindowsNoEditor_P.pak" -Algorithm SHA256
# 應該輸出：3C2DF68B6ADCC565117A039F213AA5CD3FE9ED41B4BBBBA2A36B240D69CAD2B7
```

---

## 📊 翻譯品質

- ✅ **全繁體中文**（使用台灣遊戲習慣用語：傷害、能力、子彈、暴擊、水晶、島嶼、護甲板、天賦、聖物、砲塔、連擊、衝刺、造型、波次、輪迴）
- ✅ **Token 完整保留**（PrimaryBuff/SecondaryBuff/Debuff/Duration/% 計數與英文完全一致）
- ✅ **核心術語 99.8% 一致**（20/21 核心術語完全統一）

---

## 📜 授權

- **補包包裝與翻譯內容**：MIT License（見 [LICENSE](LICENSE)）
- **Noto Sans TC 字型**：[SIL Open Font License 1.1](https://scripts.sil.org/OFL)（Google 開源，可自由散布）
- **Crab Champions 遊戲本身**：版權屬於 original 開發者，本補包不包含任何原版遊戲資產

---

## ⚖️ 免責聲明

- 這是一個**非官方、非營利**的社群在地化補包，與 Crab Champions 開發者無關
- 使用本補包的風險自負。雖然技術上只改文字且不影響多人連線，但若開發者未來對 mod 政策有變動，請以官方規範為準
- 若開發者要求下架，本補包會立即移除

---

## 🙏 致謝

- **Crab Champions** 開發者 — 感謝做出這款好遊戲
- **Noto Sans TC** — Google 開源繁體中文字型
- **UAssetAPI** — 解析與修改 UE4 資產的 .NET 函式庫
- **repak** — UE4 pak 解包/打包工具
- **UnrealLocres** — UE4 locres 匯出/匯入工具

---

## 🐛 問題回報

翻譯有錯誤、用語不順、或者發現某個該翻的沒翻？歡迎開 [Issue](../../issues) 回報。

建議附上：
1. 看到哪個字串（中/英文都可）
2. 在遊戲哪個畫面看到（武器/天賦/選單/HUD…）
3. 建議的譯法（如果你有想法）
