# SGD 收斂理論互動網頁設計規格

## 概要

將 SGD 收斂理論研究（readme.md）做成一頁式互動網頁。使用 D3.js 製作動畫，遵循 oklch design tokens 配色與字型規範。內容面向有微積分基礎的讀者，以清晰脈絡讓高中生也能跟上主線，同時保留完整數學深度。

## 目標

- **互動論文摘要 + 展示型作品集**：兼具學術深度與視覺吸引力
- **全部內容放入 + 分層展開**：主線用白話敘事 + D3 動畫，數學細節放在展開面板
- **每段至少 2 個 D3 互動動畫**
- **中英混合**：說明用中文，數學符號和專有名詞保留英文

## 交付分層

| 階段 | 範圍 | 動畫數 | 說明 |
|------|------|--------|------|
| **MVP** | Hero + B + C1 + C2 + D | 17 | 核心論證線：模型→兩大定理→四類反例 |
| **P1** | E + F | 12 | 概念深化：五個概念問答 + 噪聲四重角色 |
| **P2** | G + H | 4 | 延伸：可驗證預測 + 未解決問題 |

每階段獨立可交付、可測試。MVP 完成後即可展示完整故事線。

## 技術規格

### 輸出格式

- 純靜態單一 `index.html` 檔案
- 所有自撰 CSS、JavaScript 內嵌
- 需網路連線載入 CDN 依賴，透過瀏覽器直接開啟即可，無需本地 server
- 預估檔案大小：自撰程式碼 < 300KB（不含 CDN）

### CDN 依賴（鎖定版本）

| 套件 | 版本 | CDN URL | 用途 |
|------|------|---------|------|
| D3.js | 7.9.0 | `https://cdn.jsdelivr.net/npm/d3@7.9.0/dist/d3.min.js` | 動畫與資料視覺化 |
| KaTeX | 0.16.11 | `https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.js` + CSS + auto-render | 數學公式渲染 |

不使用 Tailwind CSS CDN——佈局用手寫 CSS（grid/flex/padding/rounded 皆為基礎 CSS 屬性，無需框架）。

### 瀏覽器支援

Chrome 90+, Firefox 90+, Safari 15+, Edge 90+。

### Design Tokens

遵循 `~/.claude/skills/design-tokens/` 規範：

**色彩（oklch + hex fallback）**：

| Token | OKLCH | Hex | 用途 |
|-------|-------|-----|------|
| `--bg-base` | `oklch(0.97 0.005 250)` | `#f5f6f8` | 頁面底色 |
| `--bg-surface` | `oklch(0.94 0.005 250)` | `#ecedf0` | 卡片、動畫容器 |
| `--bg-overlay` | `oklch(0.90 0.008 250)` | `#dfe0e5` | 展開面板標題 |
| `--bg-hover` | `oklch(0.92 0.005 250)` | `#e5e6ea` | Hover 狀態 |
| `--text-primary` | `oklch(0.20 0.01 250)` | `#1e2030` | 主要文字、標題 |
| `--text-secondary` | `oklch(0.45 0.01 250)` | `#5e6070` | 副標題、白話導言 |
| `--text-muted` | `oklch(0.60 0.008 250)` | `#8a8c98` | 操作提示、時間戳 |
| `--color-pass` | `oklch(0.48 0.16 150)` | `#1e8050` | 穩定/好解/收斂/✓ |
| `--color-fail` | `oklch(0.55 0.22 25)` | `#c93135` | 不穩定/壞解/發散/✗/紅字警告 |
| `--color-high` | `oklch(0.55 0.16 55)` | `#b86a2a` | 未解決問題（橘色虛線框） |
| `--color-warn` | `oklch(0.52 0.14 80)` | `#8a7020` | 臨界值/強調框（暗琥珀，替代金色） |
| `--color-info` | `oklch(0.52 0.13 240)` | `#2a6bb8` | 進度條、資訊標示 |
| `--color-link` | `oklch(0.48 0.15 250)` | `#1e5ab8` | 所有連結（navbar、footer） |
| `--border-subtle` | `oklch(0.85 0.005 250)` | `#d5d6da` | 邊框 |

CSS 使用 `oklch()` 搭配 `@supports not (color: oklch(0 0 0))` hex fallback。

**動畫語意色彩映射**：

| 動畫中的語意 | 對應 Token |
|-------------|-----------|
| 好解 / 穩定 / 收斂 / λ<0 | `--color-pass` |
| 壞解 / 不穩定 / 發散 / λ>0 | `--color-fail` |
| 臨界值 / 分界線 / 不變量強調框 | `--color-warn` |
| 未解決問題 / 開放 | `--color-high` |
| 資訊 / 中性標示 | `--color-info` |
| 警告文字 / 紅字結論 | `--color-fail` |

**字型**：

| Token | px | 用途 |
|-------|----|------|
| `--text-xs` | 18px | 最小字（標籤、動畫操作提示） |
| `--text-sm` | 20px | 表格內文、動畫標註 |
| `--text-base` | 24px | 正文、白話導言 |
| `--text-lg` | 28px | 強調文字（H3 等級） |
| `--text-xl` | 32px | 區段標題（H2 等級） |
| `--text-2xl` | 48px | 數值強調（即時計算數字） |
| `--text-3xl` | 56px | 頁面主標題（Hero H1） |

最小字級 18px，無例外。正文行高 1.6。

字型堆疊：`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif`

### 佈局

- 最大寬度 72rem，水平置中，左右 padding 1rem
- 佈局用手寫 CSS：CSS Grid / Flexbox / padding / border-radius
- 響應式斷點：768px

## 頁面結構

### 總覽

```
頂部 navbar（固定）→ Hero（涵蓋 readme §A）→ 形式模型 → 定理 I → 定理 II
→ 四類反例（F1-F4）→ 概念問答（E1-E5）→ 噪聲的四重角色
→ 可驗證預測 → 未解決的關口 → Footer
```

Hero 區涵蓋 readme Section A「最終立場」的核心結論，包括四條件等價式和「前三個是演算法的條件，第四個是問題本身的條件」。

### 全域元件

**固定 Navbar**：
- 一行式錨點連結，使用描述性短名：模型｜定理 I｜定理 II｜反例｜概念｜噪聲｜預測｜開放
- 連結色彩 `--color-link`，hover 加底線
- 捲動時高亮當前段落（IntersectionObserver，threshold 0.3）
- 窄螢幕（≤768px）收為漢堡選單：純 CSS checkbox hack，展開時 overlay 背景 `--bg-overlay` + opacity 0.95，不需 JavaScript

**捲動進度條**：
- 頂部 navbar 下方一條 3px 細進度條
- 色彩 `--color-info`，寬度隨 `window.scrollY / (document.body.scrollHeight - window.innerHeight)` 變化
- scroll 事件用 `requestAnimationFrame` 節流

**動畫共通行為**：
- IntersectionObserver 監控（threshold 0.1）：進入 viewport 時初始化 + 播放，離開時暫停 `requestAnimationFrame`
- 已初始化的動畫標記 `data-initialized="true"`，避免重複初始化
- 每個動畫容器有標題（`--text-lg` 28px）和操作提示（`--text-xs` 18px，`--text-muted`）
- 動畫容器背景 `--bg-surface`，圓角 0.5rem，padding 1rem，border 1px `--border-subtle`
- `prefers-reduced-motion: reduce` 時停止所有 `requestAnimationFrame`，顯示靜態初始幀

**動畫容器高度分類**：

| 類型 | 高度 | 適用動畫 |
|------|------|---------|
| 簡單圖表 | 300px | E1-2, E2-1, E2-2, E4-1, E4-2, E5-2, D1-2, D2-2, D3-2 |
| 帶滑桿互動 | 360px | B1, C1-1, C2-1, D1-1, D2-1, E1-1, E5-1, G2 |
| 複雜面板 / 多元素 | 400px | B2, C1-2, C1-3, C2-2, C2-3, D3-1, D4-1, D4-2, E3-1, E3-2 |
| 2x2 矩陣 | 500px | F1-four-roles |
| 全寬地圖 | 400px | F2-regime-map, H1-research-map, H2-discrete-continuous |
| Hero | 350px | hero-potential |
| Dashboard | 300px | G1-dashboard |

**展開面板**：
- 使用原生 `<details>/<summary>`
- `<summary>` 背景 `--bg-overlay`，padding 0.75rem 1rem，font-weight 600，`--text-sm`
- 展開時 `<details>[open] > .detail-content` 有 `max-height` CSS 過渡動畫
- KaTeX 公式在 `<details>` 的 `toggle` 事件觸發時渲染（lazy rendering），用 `data-katex-rendered` 標記避免重複
- 證明排版格式：numbered list，每步一行 KaTeX 公式 + 一行中文說明

**動畫聯動機制**：
- 同一段落內需要聯動的動畫（如 D2-1 和 D2-2）共享一個 state object
- 滑桿變動時透過 state object 的 setter 觸發所有訂閱者更新
- 非可見的聯動動畫在下次進入 viewport 時同步最新 state

### SVG vs Canvas 判定規則

- 同時移動的元素 ≤ 20 個：使用 SVG（大多數動畫）
- 同時移動的元素 > 20 個（粒子系統）：使用 Canvas（C1-1, D1-1, D3-1, D4-1）
- 所有 SVG 動畫使用 `viewBox` 自適應寬度

### 每段結構模板

```html
<section id="section-id">
  <h2>段落標題</h2>
  <p class="intro">白話導言（1-3 句）</p>
  <div class="animation-grid">
    <div class="animation-container" id="anim-id-1">
      <h3>動畫標題</h3>
      <div class="d3-canvas"></div>
      <p class="hint">操作提示</p>
    </div>
    <div class="animation-container" id="anim-id-2">
      ...
    </div>
  </div>
  <details class="math-detail">
    <summary>數學細節</summary>
    <div class="detail-content">...</div>
  </details>
</section>
```

### 數學渲染

- KaTeX CDN 引入（鎖定版本 0.16.11）
- 行內公式用 `\(...\)`，展示公式用 `\[...\]`（避免 `$` 與 JavaScript 衝突）
- `renderMathInElement()` 配置 delimiters 為 `\(...\)` 和 `\[...\]`
- 主內容區在頁面載入時渲染一次
- 展開面板內的公式在 `toggle` 事件觸發時 lazy 渲染，掃描範圍限定在該 `<details>` 元素內
- 預估公式數量：主內容 ~30 個行內公式，展開面板 ~80 個公式（C1 約 15 個、C2 約 20 個、其餘段落各 5-10 個）

### 響應式設計

- 寬螢幕（>768px）：動畫並排（2 欄 CSS Grid，`grid-template-columns: 1fr 1fr`，gap 1rem）
- 窄螢幕（≤768px）：動畫堆疊（1 欄），navbar 收為漢堡選單
- 動畫容器寬度 100%，高度依分類表固定，D3 用 `viewBox` 自適應

## 各段落內容與動畫設計

### 合成資料與模擬參數

所有動畫使用合成分析函數，不依賴真實資料集。以下為共用參數：

**雙井勢能函數**（用於 Hero, C1-1, D1-1, D3-1, D4-1）：

```
V(θ) = a·θ⁴ - b·θ² + c
```

各動畫的 a, b, c 及 Σ(θ) 函數在各段獨立定義。

**粒子模擬**（Euler-Maruyama 離散化）：

```
θ_{t+1} = θ_t - V'(θ_t)·dt + √(2·τ·Σ(θ_t)·dt)·ξ_t,  ξ_t ~ N(0,1)
```

| 參數 | 預設值 | 說明 |
|------|--------|------|
| dt | 0.01 | 時間步長 |
| 粒子數 | 50 | 上限，Canvas 渲染 |
| Burn-in | 500 步 | 穩態判定前的暖機步數 |
| 動畫幀率 | 30 fps | requestAnimationFrame 每 2 幀更新一次 |

### 滑桿規格表

| 動畫 ID | 滑桿參數 | min | max | step | default | 說明 |
|---------|---------|-----|-----|------|---------|------|
| B1-sgd-iter | η | 0.01 | 0.5 | 0.01 | 0.1 | 學習率 |
| C1-1-veff | τ = η/B | 0.01 | 2.0 | 0.01 | 0.5 | 溫度 |
| C2-1-eta-screening | η | 0.01 | 0.3 | 0.005 | 0.05 | 學習率 |
| D1-1-kramers | η | 0.001 | 0.5 | 0.001 | 0.01 | 學習率 |
| D1-2-mixing | T_train | 10² | 10⁸ | 對數 | 10⁵ | 訓練步數預算 |
| D2-1-diverge | η | 0.01 | 0.25 | 0.005 | 0.05 | 學習率，臨界 0.111 |
| D3-2-selectivity | Σ_A/H_A | 0.1 | 5.0 | 0.1 | 0.5 | 噪聲比 A |
| D3-2-selectivity | Σ_B/H_B | 0.1 | 5.0 | 0.1 | 2.0 | 噪聲比 B |
| E1-1-reparam | g 非線性度 | 0 | 3.0 | 0.1 | 0 | 座標變換程度 |
| E4-2-temperature | K | 2 | 10 | 1 | 3 | 極小值數量 |
| E5-1-control-panel | η | 0.01 | 0.5 | 0.01 | 0.1 | 學習率 |
| E5-1-control-panel | B | 1 | 128 | 1 | 32 | batch size |
| E5-1-control-panel | d/n | 1 | 100 | 1 | 10 | 過參數化比 |
| G2-phase-transition | η | 0.01 | 0.3 | 0.002 | 0.01 | 學習率 |

---

### Hero 區（涵蓋 readme §A）

- **標題**：「SGD 為何收斂到好解？」（`--text-3xl` 56px）
- **副標題**：「其普遍成立的充分條件是什麼？」（`--text-xl` 32px，`--text-secondary`）
- **核心結論**：「不存在單一的萬能條件，但存在四道關卡——全部通過，SGD 就能找到好解。前三個是演算法的條件，第四個是問題本身的條件。」
- **等價式**（KaTeX 渲染）：SGD 收斂到好解 ⟺ ¬F1 ∧ ¬F2 ∧ ¬F3 ∧ ¬F4
- **四條件簡述**：
  1. 學習率夠大使盆地間混合（¬F1）
  2. 學習率不至於太大使 SGD 發散（¬F2）
  3. 好解與壞解的 per-sample 靈敏度有差異（¬F3）
  4. 低靈敏度確實對應好泛化（¬F4）

**Hero 動畫 (hero-potential)**：350px
- 勢能函數：V(θ) = 0.25θ⁴ - 2θ² + 4，雙井位於 θ ≈ ±2
- 單個粒子做 Euler-Maruyama 隨機遊走，τ = 0.3
- Σ(θ) = 1 + 0.5·sin²(θ)（簡單的位置相依噪聲）
- 右側四個條件標記：靜態顯示 ✓（`--color-pass`），hover 時展開一句話說明
- 向下箭頭提示（CSS animation bounce）

---

### B. 形式模型與符號

**白話導言**：「在開始之前，先建立共同語言。SGD 就是每次隨機拿一筆資料來更新參數，我們關心的是：它最後停在哪裡？那個位置好不好？」

**動畫 B1-sgd-iter：SGD 迭代視覺化**（360px）
- 損失曲面：L(θ₁,θ₂) = 0.5(θ₁² + 5θ₂²)（橢圓等高線，簡單可理解）
- n = 5 個合成資料點，每個 ℓ_i 是 L 加上不同方向的擾動
- 單個點代表 θ，每步隨機選 i∈{1,...,5}（高亮對應資料點，`--color-info`）
- 梯度箭頭：`--text-muted`，更新後位置用 `--text-primary` 圓點
- 雙路徑對比：GD 軌跡（`--color-info`，平滑）vs SGD 軌跡（`--color-fail`，鋸齒）
- 互動：η 滑桿（見滑桿規格表）

**動畫 B2-good-bad：「好解」vs「壞解」四種定義**（400px）
- 2×2 grid 四個小圖（G1-G4），每個 180×150px：
  - G1（低噪聲比）：兩根柱狀圖比較解 A 和解 B 的 Σ/H，A 低（`--color-pass`）B 高（`--color-fail`）
  - G2（低 Jacobian 複雜度）：n=5 個色塊熱力圖，顏色深淺代表 ||J_i||²
  - G3（穩定解）：小動畫，移除資料點後解的位移距離條形圖
  - G4（泛化解）：train loss vs test loss 雙柱對比
- 底部 SVG 箭頭連接圖：SGD 偏好 G1/G2 → G1→G3→G4，G2→G4

**展開面板**：
- ▶ SGD 更新公式與噪聲方差：θ_{t+1} = θ_t - η∇ℓ_{z_t}(θ_t) 和 Σ(θ) 公式
- ▶ 連續時間 SDE 近似（B.2）：dθ = -∇L dt + √(τΣ) dW_t
- ▶ 離散線性化（B.3）：δ_{t+1} = (I - ηJ_{z_t}J_{z_t}ᵀ)δ_t 和 h_i = ||J_i||² 定義

---

### C1. 定理 I：非插值域閉環

**白話導言**：「當模型還沒有完美擬合所有資料時，SGD 的隨機噪聲就像溫度——它讓參數在不同的盆地之間流動，最終更常停留在『每筆資料梯度都很小』的安靜盆地裡。而安靜的盆地，恰好是泛化更好的地方。」

**動畫 C1-1-veff：有效勢能景觀 V_eff vs 原始損失 L**（360px，Canvas）
- 上層曲線：L(θ) = 0.25θ⁴ - 2θ² + 4（等高雙井）
- Σ(θ)：在 θ_A=-2 處 Σ_A=1，θ_B=+2 處 Σ_B=4（用插值函數 Σ(θ) = 2.5 - 1.5·tanh(θ)）
- 下層曲線：V_eff(θ) = ∫L'/Σ dθ + (τ/2)lnΣ（數值積分，grid N=500）
- 50 個粒子在 V_eff 上做 Euler-Maruyama（dt=0.01），burn-in 500 步後顯示穩態分佈
- 互動：τ 滑桿，即時更新 V_eff 曲線和粒子分佈
- 右側數值框：p(θ_A)/p(θ_B) = Σ_B/Σ_A（`--text-2xl`，`--color-warn` 框）

**動畫 C1-2-loo：LOO 泛化直覺**（400px）
- 左半：1D 擬合場景，n=8 個資料點，二次多項式擬合曲線
- 動畫循環：依序灰掉每個資料點（j=1→8），曲線用 CSS transition 平滑偏移
- 分為兩組對比（左右或上下）：
  - 解 A（低 Σ）：偏移量小，曲線幾乎不動（`--color-pass`）
  - 解 B（高 Σ）：偏移量大，曲線明顯移動（`--color-fail`）
- 底部即時計算框：Gap_A ≈ Σ_A/(H_A·(n-1)) vs Gap_B（`--text-lg`）

**動畫 C1-3-fpe：FPE 穩態推導視覺化**（400px）
- 不做即時 PDE 求解，改為預計算 keyframe 動畫
- 預計算 10 個時間快照的 p(θ,t)（從均勻分佈到穩態），用 D3 interpolation 平滑過渡
- 右側推導步驟面板（10 步），當前步驟高亮（`--color-info` 背景），其餘灰色
- 步驟切換：每 2 秒自動推進一步，或點擊手動跳轉
- 最終定格時穩態公式 p ∝ (1/Σ)exp(-(2/τ)V_eff) 放大顯示

**展開面板**：
- ▶ Part I 完整證明：FPE → 穩態 → 積分（Step 1-10，numbered list）
- ▶ Part II 對稱情形比值證明（Step 1-5）
- ▶ Part III 泛化間隙推導（Step 1-9）

---

### C2. 定理 II：插值域閉環

**白話導言**：「當模型已經完美擬合所有資料時，連續的 SDE 近似失效——因為噪聲是零。但 SGD 的離散本性此時反而成為篩選器：它用學習率 η 當作門檻，把太敏感的解震走，只留下簡單的解。」

**動畫 C2-1-eta-screening：η 篩選機制（Lyapunov 指數）**（360px）
- 合成場景：n=5 個解，h_max 值分別為 [8, 15, 25, 40, 60]
- 橫軸：h 值（0-70），縱軸：λ = (1/n)ln|1-η·h|
- 紅色虛線（`--color-fail`）：λ = 0 分界線
- 分界線右側背景漸變為淡紅（badge-critical 背景），左側淡綠
- 互動：η 滑桿，5 個解的點隨 η 移動
  - λ > 0 的點標紅（`--color-fail`），旁邊迷你動畫：小線段振幅增長
  - λ < 0 的點標綠（`--color-pass`），旁邊迷你動畫：小線段振幅衰減
- 底部文字即時更新：「η = {val} → 排斥 h_max > {2/η} 的解」

**動畫 C2-2-jacobian：Jacobian 複雜度與泛化**（400px）
- 左右對比兩個示意網路：
  - 簡單網路：3 層小圖示，下方 n=5 資料點
  - 複雜網路：5 層大圖示，下方 n=5 資料點
- 每個資料點引出一個 J_i 向量箭頭（長度 ∝ √h_i）
- 簡單：箭頭短且長度均勻（`--color-pass`），C = ||J||_F/√n 小
- 複雜：箭頭長且長度差異大（`--color-fail`），C 大
- 右側柱狀圖：Rademacher 複雜度 R̂ ≤ ρC/√n + B₂ρ²/2
- 底部結論：「η 增大 → 排斥高 C 的解 → 泛化更好」

**動畫 C2-3-closed-loop：閉環流程圖**（400px）
- 四步流程水平排列，依序動畫亮起（每步 1 秒）：
  1. 「SGD 執行」（齒輪圖示）
  2. 「排斥 h_max > 2/η」（紅色 ✗，`--color-fail`）
  3. 「保留 C ≤ √(2/η)」（綠色 ✓，`--color-pass`）
  4. 「泛化界 O(ρ/√(ηn))」（藍色盾牌，`--color-info`）
- 步驟間有動畫箭頭連接
- 點擊任一步驟：下方展開該步的核心公式（KaTeX，lazy render）

**展開面板**：
- ▶ Part A 壞解排斥證明（Lyapunov 指數推導，Step 1-5）
- ▶ Part B 好解穩定性證明
- ▶ Part C Rademacher 複雜度界推導（Step 1-5）
- ▶ 主定理閉環論證（(I)-(IV)）

---

### D. 四類反例（F1-F4）

**段落導言**：「前面說了 SGD 什麼時候成功。現在反過來問：什麼時候會失敗？恰好有四種方式讓 SGD 找到壞解，每一種對應一個必要條件被違反。」

#### F1：η 太小（動力學陷阱）

**白話**：「學習率太小，SGD 像一個只敢小碎步走的人，永遠翻不過兩個盆地之間的山丘。」

**動畫 D1-1-kramers：Kramers 逃逸時間**（360px，Canvas）
- 勢能：V(θ) = 0.25θ⁴ - 2θ² + 4（同 Hero，ΔV = 4）
- 井 A（θ≈-2）：H=2, Σ=1（好解）；井 B（θ≈+2）：H=8, Σ=4（壞解）
- 有效壁壘 ΔU ≈ 2ΔV/(τ·Σ_B) = 2·4/(τ·4) = 2/τ → Kramers 時間 τ_escape ~ (1/τ)exp(2/τ)
- 單個粒子從 B 出發，Euler-Maruyama 模擬
- 互動：η 滑桿（τ = η 因 B=1）
  - η 大（如 0.5）→ 粒子幾秒內跳到 A
  - η 小（如 0.01）→ 粒子困在 B 震盪
- 右側計數器：顯示理論 Kramers 時間和已模擬步數

**動畫 D1-2-mixing：混合時間 vs 訓練時間**（300px）
- 橫軸 η（對數刻度 0.001-1），縱軸對數時間（10⁰-10¹²）
- 紅色曲線（`--color-fail`）：τ_mix(η) ~ exp(C/η)
- 藍色水平線（`--color-info`）：T_train（可拖動）
- 交叉點左側區域填充淡紅背景：「訓練結束前還沒混合」
- 互動：垂直拖動 T_train 線

#### F2：η 太大（發散/混沌）

**白話**：「學習率太大，SGD 每一步跨太遠，不但找不到好解，連停都停不下來。」

**動畫 D2-1-diverge：離散穩定性崩潰**（360px）
- 損失：L(θ) = (1/2)·10·θ²，由兩個樣本組成：ℓ₁ = (1/2)·18·θ²、ℓ₂ = (1/2)·2·θ²
- 臨界 η_c = 2/18 ≈ 0.111
- 動畫：θ_t 軌跡隨時間演化（折線圖，橫軸 t，縱軸 θ_t）
- 互動：η 滑桿
  - η < 0.111：振盪收斂到 0（`--color-pass` 軌跡）
  - η > 0.111：振幅指數增長（`--color-fail` 軌跡），超出範圍時截斷並標示「→ ∞」
- η_c 處用虛線標示（`--color-warn`）

**動畫 D2-2-lyapunov-diverge：Lyapunov 指數符號翻轉**（300px）
- 橫軸 η（0-0.25），縱軸 Λ = (1/2)(ln|1-18η| + ln|1-2η|)
- 曲線穿越零點處（η ≈ 0.111）標紅色圓點
- 零點右側背景淡紅，左側淡綠
- **聯動 D2-1**：共享 η state object，D2-1 滑桿變動時 D2-2 的 η 指示線同步移動

#### F3：無差異化（選擇力退化）

**白話**：「如果好解和壞解在 SGD 眼中長得一模一樣，SGD 就像蒙眼選擇，有一半機率停在壞解。」

**動畫 D3-1-equal-wells：等深雙井**（400px，Canvas）
- V_eff 兩個盆地深度完全相同（Σ_A/H_A = Σ_B/H_B = 1）
- 50 個粒子隨機遊走，穩態時左右各約 50%
- 上方分割畫面對比 C1-1 的截圖/縮圖（V_eff 不等深），強調差異

**動畫 D3-2-selectivity：選擇力儀表板**（300px）
- 兩個旋鈕滑桿：Σ_A/H_A 和 Σ_B/H_B（見滑桿規格表）
- 中央大數字：偏好比 p(A)/p(B) = (Σ_B/H_B)/(Σ_A/H_A)（`--text-2xl`）
- 兩者差距大 → 大數字 + 綠色（`--color-pass`）
- 兩者趨近 → 數字→1.0 + 黃色閃爍（`--color-warn`）→ 「選擇力消失」警告文字（`--color-fail`）

#### F4：噪聲-泛化反轉

**白話**：「最致命的情況：好解因為用了多樣的特徵，反而梯度噪聲很大；壞解因為死記答案，噪聲反而很小。SGD 偏好低噪聲，結果選了壞解。」

**動畫 D4-1-reversal：反轉景觀**（400px，Canvas）
- 勢能同 C1-1 結構，但 Σ 反轉：Σ_A=20（好解），Σ_B=1（壞解）
- 有效壁壘標示：ΔU_A（低，虛線 `--color-warn`）、ΔU_B（高，實線 `--color-warn`）
- 50 個粒子模擬：絕大多數被困在壞井 B
- 泛化品質標注：A 旁 ✓（`--color-pass`）、B 旁 ✗（`--color-fail`），形成視覺矛盾

**動畫 D4-2-diversity：特徵多樣性 vs 記憶**（400px）
- 左右分割：
  - 好解 A：n=6 個資料點，每個引出梯度箭頭，方向散開（60° 間隔）→ Σ 大
  - 壞解 B：n=6 個資料點，梯度箭頭幾乎平行（5° 偏差）→ Σ 小
- 動畫：向量逐一出現，然後計算 Σ = (1/n)Σ||∇ℓ_i||² 的加總過程
- 底部紅字結論（`--color-fail`）：「SGD 的低噪聲偏好 ≠ 泛化偏好」

**段落結尾：四張總結卡片**
- 四張卡片水平排列（窄螢幕 2×2 grid），每張 `--bg-surface` 背景
- 內容對應 readme 第 229-232 行的最小失敗條件：
  1. F1：ηT ≫ τ_mix（充分混合）
  2. F2：η < 2/λ_max（穩定）
  3. F3：Σᵢ/Hᵢ 有差異（有選擇力）
  4. F4：Corr(Σ, Gap) > 0（方向正確）
- 每張卡片頂部 ✓（`--color-pass`），標題一句話，與 Hero 四個標記呼應
- 純 HTML/CSS 靜態元件，不計入動畫總數

---

### E. 概念問題的嚴格回答（E1-E5）

**導言**：「前面的定理回答了『什麼時候成功、什麼時候失敗』。這一段回答五個常見的概念困惑。」

#### E1. Simplicity vs Flatness

**白話**：「人們常說 SGD 偏好『平坦的解』，但平坦度可以被座標變換隨意改變。真正不變的量是 Σ/H。」

**動畫 E1-1-reparam：Reparameterization 不變性**（360px）
- 左：θ 空間的 1D 損失曲線，標示 H 和 Σ 數值（`--text-sm`）
- 右：ϕ = g(θ) 空間的損失曲線，標示 H̃ 和 Σ̃
- g(θ) = θ + α·sin(θ)，α 由滑桿控制（見規格表）
- α 增大 → 右側曲線變形，H̃ 和 Σ̃ 劇變
- 中央不變量框（`--color-warn` 邊框，粗 3px）：Σ/H = {fixed value}，始終不變

**動畫 E1-2-three-quantities：三量比較柱狀圖**（300px）
- 三根柱：H（Flatness⁻¹）、Σ（Simplicity⁻¹）、Σ/H（不變量）
- 切換座標系（按鈕或自動循環 3 組不同的 g）
- 前兩根柱高度劇變（CSS transition），第三根紋絲不動
- 第三根柱用 `--color-warn` 填色強調

#### E2. 最小範數投影

**白話**：「在線性模型中，SGD 從零出發，不管怎麼隨機抽樣，最終一定走到和 GD 一樣的最小範數解。這是代數性質，與噪聲無關。」

**動畫 E2-1-rowspace：Row space 約束**（300px）
- 3D→2D 投影視覺化：row(X) 子空間畫成灰色半透明平面
- w₀ = 0 在平面上（藍點），每次 SGD 增量箭頭（`--color-info`）都在平面內
- 軌跡始終在平面上，最終收斂到 X†y 點（`--color-pass` 大圓點）

**動畫 E2-2-sgd-vs-gd：SGD vs GD 路徑對比**（300px）
- 同一平面俯視圖，起點 w₀ = 0
- GD 路徑：`--color-info`，平滑曲線
- SGD 路徑：`--color-fail`，鋸齒折線
- 兩者收斂到同一點 X†y（`--color-pass`）
- 旁邊標注：「線性模型中 SGD = GD（隱式偏差相同）」

#### E3. 多維 Detailed Balance

**白話**：「在一維中穩態有簡潔公式。多維時可能存在旋轉流，需要解更難的方程。」

**動畫 E3-1-rotation：2D 向量場與旋轉流**（400px）
- 2D 平面上向量場 + 粒子軌跡
- Toggle 按鈕：「Detailed Balance 成立 / 破缺」
  - 成立：Σ = I，粒子沿梯度下降，無旋轉
  - 破缺：Σ = diag(1+θ₂², 1)，粒子出現旋轉分量
- 向量場用箭頭網格顯示（`--text-muted`），粒子軌跡用 `--color-info`

**動畫 E3-2-quasipotential：準勢 vs 真勢**（400px）
- 使用 readme 給出的解析近似，硬編碼此特定 2D 案例：
  - 左：真損失 L = (θ₁²+θ₂²)/2 等高線
  - 右：準勢 V ≈ θ₁²+θ₂² - (1/2)θ₁²θ₂² 等高線
- 修正項 -(1/2)θ₁²θ₂² 的區域用色彩漸層標示（`--color-info` 到 `--color-warn`）
- 底部標注：「本圖為特定 2D 例子的解析近似」

#### E4. 三層條件分類

**白話**：「SGD 收斂到好解的條件分三層——必要、充分、近似充分——對應不同信心等級。」

**動畫 E4-1-concentric：三層同心圓**（300px）
- 三個同心圓環：
  - 外圈（`--border-subtle`）：必要條件
  - 中圈（`--color-info` 填充淡色）：近似充分（容許 δ 誤差）
  - 內圈（`--color-pass` 填充淡色）：嚴格充分（唯一最小值 + 能隙 Δ > 0）
- Hover 每層：右側彈出框顯示數學表述和直覺解釋

**動畫 E4-2-temperature：溫度掃描**（300px）
- 橫軸 τ（0-2），縱軸 P(好解)（0-1）
- 三條曲線：Δ = 0.5, 1.0, 2.0（不同色）
- P = 1 - (K-1)exp(-2Δ/τ)
- 互動：K 滑桿（見規格表），曲線隨 K 變化

#### E5. 各因素的嚴格影響

**白話**：「學習率、batch size、過參數化程度、曲率——每個因素如何影響 SGD 的選擇？」

**動畫 E5-1-control-panel：四因素控制面板**（360px）
- 左側四個滑桿（見規格表）：η、B、d/n、H 譜形狀（下拉選單：均勻/指數衰減/冪律）
- 中央：1D 有效勢能景觀，隨參數即時變形
- 右側三個即時數值（`--text-lg`）：|M_η|、C 上界、泛化界

**動畫 E5-2-deff：有效維度 d_eff**（300px）
- d=20 個特徵值的柱狀圖（λ₁ ≥ λ₂ ≥ ...）
- 前 k 個大值用 `--color-info` 亮色，後面的用 `--text-muted` 灰色
- 右側動態計算：d_eff = (tr H)²/tr(H²) = {value}（`--text-2xl`）
- 提供三種預設譜：均勻 / 指數衰減 / 冪律，點擊按鈕切換

**展開面板**：
- ▶ 各因素數學細節（η 離散修正、B 對 τ 的影響、過參數化的流形漂移）
- ▶ 對稱性與軌道權重：V_eff(g·θ) = V_eff(θ)，物理解權重 p_phys([θ]) = |G/G_θ|·p(θ)，穩定化子小的解權重更大，效應量級 O(τ)

---

### F. 噪聲的四重角色

**導言**：「SGD 中的噪聲不只是干擾——它同時扮演四個角色，在不同的機制域中用不同的數學方式發揮作用。」

**動畫 F1-four-roles：四重角色互動矩陣**（500px）
- 2×2 grid 佈局，每格 ~240×220px：
  - 探索：小球翻越壁壘的迷你動畫（Kramers 逃逸），標注「連續+離散」
  - 選擇：天秤圖示偏向低 Σ 端，標注「連續（乘性噪聲 SDE）」
  - 篩選：η 門檻線 + 紅/綠分區，標注「純離散」
  - 鎖定：∇ℓ_i = 0 文字 + 不動點示意，標注「純離散」
- 點擊任一格：overlay 放大顯示完整機制說明和對應定理引用

**動畫 F2-regime-map：機制域地圖**（400px）
- 橫軸：訓練損失（高→0），縱軸：d/n（1→100）
- 左上區域（非插值）：標記「探索+選擇」，背景 `--color-info` 淡色
- 右下區域（插值）：標記「篩選+鎖定」，背景 `--color-pass` 淡色
- 動畫軌跡：一個點從左上慢慢移到右下（代表訓練過程），經過的區域依序高亮
- 底部核心洞見（`--color-warn` 框）：「現代深度學習主要在插值域，正則化完全是離散效應」

---

### G. 可驗證預測

**導言**：「理論的價值在於可否被實驗推翻。這裡列出五個具體預測，任何一個被推翻都意味著理論需要修正。」

**動畫 G1-dashboard：預測 Dashboard**（300px）
- 五張小卡片水平排列（窄螢幕折行），每張 ~130×200px，`--bg-surface`
  - P1：h_max vs 測試誤差散點圖示意（正相關）
  - P2：η 相變跳躍的小折線圖
  - P3：各向同性 vs 結構化噪聲的雙柱對比
  - P4：η↑ → C↓ 的下降曲線
  - P5：過擬合 Σ < 泛化 Σ 的柱狀圖
- 點擊卡片：下方展開區顯示詳細說明和實驗設計建議

**動畫 G2-phase-transition：臨界相變模擬（P2 詳細版）**（360px）
- 模擬：固定 n=5 個解的 h_max = [8, 15, 25, 40, 60]
- 橫軸 η（滑桿），縱軸訓練損失
- 動畫：η 從 0.01 緩慢增加
  - η < 2/60 ≈ 0.033：所有解穩定，loss = 0
  - η > 0.033：解依序失穩（h_max 大的先），loss 跳升
  - 每次失穩處加閃爍標記（`--color-fail`）

---

### H. 未解決的關口

**導言**：「這個理論走到哪裡了？還有五道關卡尚未突破。」

**動畫 H1-research-map：研究地圖**（400px）
- 節點圖（D3 force layout）：
  - 已解決（定理 I、定理 II）：`--color-pass` 實線框
  - 五個未解決問題：`--color-high` 虛線框（橘色）
    1. 多維 V_eff
    2. Jacobian 正交假設
    3. 全局軌跡
    4. F4 邊界精確化
    5. 離散-連續統一
  - 依賴箭頭連接
- Hover 節點：tooltip 顯示問題描述和難點

**動畫 H2-discrete-continuous：離散-連續統一鴻溝**（400px）
- 左半：連續 SDE 世界的 V_eff 景觀（定理 I），標注 Σ > 0
- 右半：離散 Lyapunov 世界的穩定性圖（定理 II），標注 Σ → 0
- 中間：Σ 值的滑桿或動畫，從正數漸漸趨向零
  - Σ > 0：左側景觀清晰
  - Σ → 0：左側景觀漸漸消失/退化，右側分析接手
  - 過渡處視覺斷裂（裂縫效果），標注「統一需要處理此極限」

---

### Footer

- 來源：「基於 sgd-convergence-theory 研究筆記」（`--text-secondary`）
- 引用格式建議
- 原始檔案連結（`--color-link`）：readme.md、sgd_interpolation_theorem_final.tex

## 動畫總覽

| 段落 | 動畫數 | 動畫 ID | 渲染方式 |
|------|--------|---------|---------|
| Hero | 1 | hero-potential | SVG |
| 形式模型 | 2 | B1-sgd-iter, B2-good-bad | SVG |
| 定理 I | 3 | C1-1-veff, C1-2-loo, C1-3-fpe | Canvas, SVG, SVG |
| 定理 II | 3 | C2-1-eta-screening, C2-2-jacobian, C2-3-closed-loop | SVG |
| 反例 F1 | 2 | D1-1-kramers, D1-2-mixing | Canvas, SVG |
| 反例 F2 | 2 | D2-1-diverge, D2-2-lyapunov-diverge | SVG |
| 反例 F3 | 2 | D3-1-equal-wells, D3-2-selectivity | Canvas, SVG |
| 反例 F4 | 2 | D4-1-reversal, D4-2-diversity | Canvas, SVG |
| 概念 E1 | 2 | E1-1-reparam, E1-2-three-quantities | SVG |
| 概念 E2 | 2 | E2-1-rowspace, E2-2-sgd-vs-gd | SVG |
| 概念 E3 | 2 | E3-1-rotation, E3-2-quasipotential | SVG |
| 概念 E4 | 2 | E4-1-concentric, E4-2-temperature | SVG |
| 概念 E5 | 2 | E5-1-control-panel, E5-2-deff | SVG |
| 噪聲角色 | 2 | F1-four-roles, F2-regime-map | SVG |
| 預測 | 2 | G1-dashboard, G2-phase-transition | SVG |
| 開放問題 | 2 | H1-research-map, H2-discrete-continuous | SVG |
| **合計** | **33** | | Canvas: 4, SVG: 29 |

D 段結尾「四張總結卡片」為靜態 HTML/CSS 元件，不計入動畫。

## 效能策略

- **Lazy init**：所有動畫用 IntersectionObserver（threshold 0.1），進入 viewport 時才初始化 D3，標記 `data-initialized`
- **暫停/恢復**：離開 viewport 時暫停 `requestAnimationFrame`
- **粒子上限**：Canvas 粒子系統最多 50 個粒子
- **SVG vs Canvas**：同時移動元素 ≤ 20 用 SVG，> 20 用 Canvas。Canvas 動畫：C1-1, D1-1, D3-1, D4-1（均為 50 粒子系統）
- **防抖**：滑桿互動用 `requestAnimationFrame` 節流，不用 setTimeout
- **KaTeX lazy render**：展開面板內的公式在 `toggle` 事件時渲染
- **動畫幀率**：30 fps（每 2 個 rAF 幀更新一次模擬）

## 無障礙

- 所有互動元件有 `aria-label`
- `prefers-reduced-motion: reduce`：停止所有 `requestAnimationFrame`，顯示靜態初始幀
- 展開面板使用原生 `<details>/<summary>`
- 色彩對比符合 WCAG AA（design tokens 已保證，嚴重度 L 0.45-0.55 確保 4.5:1+）
- 滑桿使用原生 `<input type="range">` + `aria-valuemin/max/now`
