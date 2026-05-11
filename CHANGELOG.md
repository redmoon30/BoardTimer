# BoardTimer 更新日誌 (Changelog)

所有關於本專案的重大變更將記錄於此文件。

## [v2.1.0] - Vertical Mode & Export Enhancements - 2026-02-04

#### 新增 (Added)
- **垂直顯示模式 (Vertical Focus Mode)**: 
  - 專為長版圖片設計，主預覽區改為「上下貼合」顯示。
  - 預覽列可切換至左側垂直排列，支援垂直自動對焦捲動。
  - 在 View 選項下方新增切換開關。

#### 修正 (Fixed)
- **XML 匯出檔名修正**: 
  - 改善匯入邏輯，內部儲存完整的原始檔名（含副檔名）。
  - 匯出 XML 時的 `<pathurl>` 現在包含副檔名，徹底解決 Premiere Pro 掉檔問題。
- **Grid Image 匯出強化**:
  - **動態標題**: 匯出圖片的標題現在會自動帶入目前的場景 (Section) 名稱。
  - **縮圖裁切優化**: 縮圖改為「等比縮放至寬度、頂部對齊、裁切底部」，不再因比例不同而拉伸變形。

## [v2.0.2] - XML Export Fix - 2026-02-02

#### 修正 (Fixed)
- **XML 相容性修復**: 針對 Premiere Pro Import 失敗問題進行重大修正。
  - **路徑編碼**: 實作標準 URI Encoding，解決中文路徑與空白字元導致無法讀取的問題。
  - **結構調整**: 改用巢狀 `<file>` 結構 (Nested inside `<clipitem>`)，符合 Final Cut Pro XML (xmeml) 標準規範。
  - **版本校正**: XML 標頭降回 `version="4"` 並補上遺失的 `DOCTYPE` 宣告。

## [v2.0.1] - UX Refinement - 2026-01-27

#### 修正與優化 (Fixed & Optimized)
- **檔名自動去副檔名**: 導入圖片時自動移除檔案副檔名（如 `.jpg`, `.png`），保持介面簡潔。
- **Grid Export 強化**: 匯出網格圖片時，現在會顯示「分鏡檔名」而非僅顯示序號，方便離線校對。
- **Focus 模式修復**: 修正因 Session 重構導致的渲染作用域錯誤。
- **介面互動**: 實作 Grid 模式下的雙擊重新命名 (Rename) 功能。

#### 待優化 (Upcoming)
- **Grid 秒數編輯**: 規劃於未來版本實作 Grid 模式下的直接秒數編輯與微調功能。

## [v2.0.0] - Section Management & Persistence - 2026-01-27

#### 新增 (Added)
- **多場景管理 (Section Management)**: 加入右側邊欄，支援建立多個獨立場景 (Section)，每一段都有其獨立的分鏡序列與計時。
- **資料持久化 (Persistence)**: 串接 IndexedDB，分鏡圖片與計時資訊現在會永久儲存於瀏覽器中，重新整理或重開頁面後會自動載入。
- **可調整側邊欄 (Resizable Sidebars)**: 左右兩側邊欄現在都可以手動拖拉調整寬度，中央區域具備流暢的響應式佈局。
- **介面修正 (UI Defense)**: 針對第三方工具可能的 DOM 注入 (如 Figma `#always-on-top-app`) 增加防護，確保佈局不被破壞。

## [v1.5.0] - Grid Export & Sort Update - 2026-01-23

#### 新增 (Added)
- **Grid Image Export**: 新增圖片網格匯出功能。產生一張包含所有分鏡縮圖、鏡號、秒數與檔名的 PNG 圖檔，適合用於快速預覽或與團隊溝通。
- **鍵盤排序 (Shortcuts Reordering)**: 支援 `Ctrl + ←/→` 快速移動分鏡順序，具備即時預覽與 Undo 支援。
- **拖拉排序 (Drag & Drop Sort)**: 實作視覺化的拖拉排序功能。
- **操作限制 (UX Restriction)**: 在播放模式進行中，自動鎖定左右方向鍵與 Enter 鍵，避免錄製過程意外中斷。

#### 修正 (Fixed)
- **Import Bug**: 修正因事件監聽器衝突導致的圖片拖入匯入失敗問題。
- **Code Integrity**: 修正 `moveSlide` 函數內出現字面 `\n` 字元的語法錯誤。

#### 已知問題 (Known Issues)
- **XML Export**: Premiere Pro XML 匯入仍有相容性問題 (Generic Error)，目前暫時擱置，改以 Grid Image Export 作為替代方案。

## [v1.4.0] - Zero-Jitter Update - 2026-01-23
#### 新增 (Added)
- **版本控制系統**: 新增 `versions/` 資料夾與 `CHANGELOG.md`。
- **操作手感優化**:
  - **Zero-Jitter Rendering**: 重構渲染邏輯，保留 DOM 結構，解決預覽列捲動閃爍問題，實現原生般平滑的捲動體驗。
  - **Grid 導航**: 支援 `ArrowUp` / `ArrowDown` 上下移動，演算法自動計算列數。
  - **Shift + Space**: 全局快捷鍵，強制切換至播放模式並從頭開始播放。
  - **Undo Duration**: 現在 `Ctrl+Z` 可以復原 "錄製秒數" 的操作 (例如錄太長可以單獨復原該卡秒數)。
- **視覺優化**:
  - **High Contrast HUD**: 專注模式的秒數顯示改為金色 (#FFD700) 並增加陰影，大幅提升在不同圖片背景下的可讀性。
  - **Next Slide Hint**: 強化「下一卡」的視覺提示 (不透明度 100% + 微放大 + 藍色光暈)。

#### 修正 (Fixed)
- **Layout Bug**: 修正從 Grid 視圖切換回 Focus 視圖時，舞台 (Stage) 區塊消失的問題。
- **Playback Logic**: 修正播放模式邏輯，現在遇到秒數為 0 的分鏡會自動停止，避免播放未錄製的內容。
- **Grid Selection**: 在 Grid 模式下現在允許選取並預覽「已隱藏 (Hidden)」的分鏡。

### [v1.3.0] - Refinement Phase
#### 新增 (Added)
- **UI 改版**: 左側側邊欄設計，深色專業介面。
- **Timeline UX**: 實作「永遠置中」的時間軸。
- **編輯功能**:
  - `Backspace`: 按一下隱藏，對隱藏卡片再按一下刪除。
  - `Ctrl+Z`: 基礎 Undo/Redo 系統。
- **快捷鍵**: `R`/`T` 切換模式，`F`/`G` 切換視圖。

### [v1.0.0] - MVP Release
- 基礎功能發布：拖放圖片、空白鍵計時、匯出 Premiere Pro XML。
