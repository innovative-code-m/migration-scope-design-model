# 2026-03-01_05_Create_Granularity_Levels

## 1. 目的・背景
- スコープの「大きさ」を科学的に扱うため、`03_Scope-Granularity-Levels.md` を作成する。
- 「粗すぎる／細かすぎる」という感覚的な議論を排除する。

## 2. 検討内容
- **5段階モデル**:
  - Level 0: 資産（プログラム）
  - Level 1: 機能（画面）
  - Level 2: データ責任（推奨最小単位）
  - Level 3: 状態遷移（並行稼働前提）
  - Level 4: 構造ドメイン
- **粒度誤りと破綻**:
  - 粗すぎ → Boundary Permeability
  - 細かすぎ → SoR Split-Brain
  という因果関係をMermaid図で体系化した。

## 3. 暫定結論・Next Action
- [x] Level 2以上を必須とする基準の策定
- [x] 粒度誤り診断チェックリストの作成
