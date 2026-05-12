# 田口最佳化 Skill — Claude Code / Taguchi Optimization Skill / 田口メソッド Skill

---

## 中文

一個可重複使用的 Claude Code 斜線指令，引導你完成**田口實驗設計（Taguchi Method）**，用 9 組實驗取代 27 種組合，系統化找出最佳參數。

利用田口最佳化找出實驗組合的最佳因子。

### 安裝方式

**macOS / Linux**
```bash
mkdir -p ~/.claude/commands
cp SKILL_zh.md ~/.claude/commands/taguchi.md
cp SKILL.md ~/.claude/commands/taguchi-en.md
cp SKILL_ja.md ~/.claude/commands/taguchi-ja.md
```

**Windows（PowerShell）**
```powershell
mkdir "$env:USERPROFILE\.claude\commands"
Copy-Item "SKILL_zh.md" "$env:USERPROFILE\.claude\commands\taguchi.md"
Copy-Item "SKILL.md" "$env:USERPROFILE\.claude\commands\taguchi-en.md"
Copy-Item "SKILL_ja.md" "$env:USERPROFILE\.claude\commands\taguchi-ja.md"
```

### 使用方式

```
/taguchi        # 中文引導流程
/taguchi-en     # English guided flow
/taguchi-ja     # 日本語ガイド
```

### 適用情境

| 情境 | 建議用田口？ |
|------|------------|
| 有 3～4 個可控參數需要調整 | ✅ |
| 全因子實驗成本太高（時間、資源） | ✅ |
| 想知道哪個因子影響最大 | ✅ |
| 因子之間有強烈交互作用 | ⚠️ 改用全因子實驗 |
| 只有 1～2 個參數 | ⚠️ A/B Test 就夠了 |

**常見應用場域：** AI Prompt 調校、製造製程優化、UI/UX 參數測試、機器學習超參數篩選、配方開發。

### 實際案例：Web API 回應時間最佳化

**目標：** 最小化 API 平均回應時間（ms）— 望小。

| 因子 | 水準 1 | 水準 2 | 水準 3 |
|------|--------|--------|--------|
| A — 資料庫查詢快取 | 不啟用 | 256 MB | 1 GB |
| B — Worker 數量 | 2 | 4 | 8 |
| C — 連線逾時設定 | 5 秒 | 15 秒 | 30 秒 |

| 組別 | A | B | C | 平均回應 (ms) |
|------|---|---|---|--------------|
| 1 | 不啟用 | 2 | 5s | 420 |
| 2 | 不啟用 | 4 | 15s | 310 |
| 3 | 不啟用 | 8 | 30s | 280 |
| 4 | 256 MB | 2 | 15s | 390 |
| 5 | 256 MB | 4 | 30s | 195 |
| **6** | **256 MB** | **8** | **5s** | **140 ← 最佳** |
| 7 | 1 GB | 2 | 30s | 370 |
| 8 | 1 GB | 4 | 5s | 160 |
| 9 | 1 GB | 8 | 15s | 175 |

**最重要因子：B（Worker 數量）** — 增加並行處理數量對降低排隊延遲效果最顯著。

---

## English

A reusable Claude Code slash command that guides you through **Taguchi Method** experimental design — systematically finding the optimal parameter combination with only 9 experiments instead of 27.

Use Taguchi Method to identify the optimal factor combination from a structured experiment design.

### Installation

**macOS / Linux**
```bash
mkdir -p ~/.claude/commands
cp SKILL_zh.md ~/.claude/commands/taguchi.md
cp SKILL.md ~/.claude/commands/taguchi-en.md
cp SKILL_ja.md ~/.claude/commands/taguchi-ja.md
```

**Windows (PowerShell)**
```powershell
mkdir "$env:USERPROFILE\.claude\commands"
Copy-Item "SKILL_zh.md" "$env:USERPROFILE\.claude\commands\taguchi.md"
Copy-Item "SKILL.md" "$env:USERPROFILE\.claude\commands\taguchi-en.md"
Copy-Item "SKILL_ja.md" "$env:USERPROFILE\.claude\commands\taguchi-ja.md"
```

### Usage

```
/taguchi        # Chinese guided flow (中文引導)
/taguchi-en     # English guided flow
/taguchi-ja     # 日本語ガイド
```

### When to Use

| Situation | Taguchi? |
|-----------|----------|
| 3–4 controllable parameters to tune | ✅ |
| Full factorial is too expensive | ✅ |
| Want to rank factor importance | ✅ |
| Strong factor interactions expected | ⚠️ Use full factorial |
| Only 1–2 parameters | ⚠️ A/B test is enough |

**Typical use cases:** AI prompt engineering, manufacturing process tuning, UI/UX testing, ML hyperparameter screening, formulation optimization.

### Example: Web API Response Time Optimization

**Goal:** Minimize average API response time (ms) — smaller-the-better.

| Factor | L1 | L2 | L3 |
|--------|----|----|-----|
| A — DB query cache | Disabled | 256 MB | 1 GB |
| B — Worker processes | 2 | 4 | 8 |
| C — Connection timeout | 5s | 15s | 30s |

| Exp | A | B | C | Avg Response (ms) |
|-----|---|---|---|-------------------|
| 1 | Disabled | 2 | 5s | 420 |
| 2 | Disabled | 4 | 15s | 310 |
| 3 | Disabled | 8 | 30s | 280 |
| 4 | 256 MB | 2 | 15s | 390 |
| 5 | 256 MB | 4 | 30s | 195 |
| **6** | **256 MB** | **8** | **5s** | **140 ← Best** |
| 7 | 1 GB | 2 | 30s | 370 |
| 8 | 1 GB | 4 | 5s | 160 |
| 9 | 1 GB | 8 | 15s | 175 |

**Most impactful factor: B (Worker count)** — scaling workers reduced queuing delay more than caching alone.

### Methodology Notes

- **L9 orthogonal array** covers 9 of 27 combinations while maintaining balance
- **S/N ratio:** Smaller-the-better: `-10 × log10(mean²)` / Larger-the-better: `-10 × log10(mean(1/y²))`
- Always run a **confirmation experiment** after identifying the predicted optimum

---

---

## 日本語

Claude Code で使える再利用可能なスラッシュコマンドです。**田口メソッド（タグチメソッド）**の実験設計をステップバイステップでガイドします。27通りの組み合わせの代わりに9回の実験で最適パラメータを体系的に発見できます。

田口最適化を活用して、実験の最適因子組み合わせを特定します。

### インストール

**macOS / Linux**
```bash
mkdir -p ~/.claude/commands
cp SKILL_ja.md ~/.claude/commands/taguchi-ja.md
```

**Windows（PowerShell）**
```powershell
mkdir "$env:USERPROFILE\.claude\commands"
Copy-Item "SKILL_ja.md" "$env:USERPROFILE\.claude\commands\taguchi-ja.md"
```

### 使い方

```
/taguchi-ja     # 日本語ガイド
```

### 適用場面

| 状況 | 田口法を使う？ |
|------|--------------|
| 3〜4つの制御可能なパラメータを調整したい | ✅ |
| 全要因実験はコストが高すぎる | ✅ |
| どの因子が最も重要か知りたい | ✅ |
| 因子間の強い交互作用が予想される | ⚠️ 完全要因実験を使用 |
| パラメータが1〜2つだけ | ⚠️ A/Bテストで十分 |

**主な適用分野：** AIプロンプト最適化、製造プロセス改善、UI/UXパラメータテスト、機械学習ハイパーパラメータ探索、配合最適化。

---

## Files

| File | Description |
|------|-------------|
| `SKILL.md` | Claude Code skill — English |
| `SKILL_zh.md` | Claude Code skill — 中文版 |
| `SKILL_ja.md` | Claude Code skill — 日本語版 |
