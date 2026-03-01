# Migration Scope Design Model
移行スコープ設計モデル（I/O境界 × データ境界 × 構造破綻理論）

---

## 概要（実務適用ベース）

本リポジトリは、レガシーシステム移行における
「スコープ定義」を中心とした設計モデルを提示するものです。

移行の失敗要因の多くは、
機能単位ではなく「境界の誤定義」にあります。

本モデルでは、

- I/O境界によるスコープ分割
- データ境界（SoR / Write Owner）による依存制御
- 段階移行を成立させるための判断軸定義
- ハイブリッド移行（周辺ラップ＋中核リライト）の設計指針
- 移行破綻の構造パターン分類と防止原則

を体系化しています。

---

## 想定ケース（仮想適用例）

対象：
製造業向け倉庫管理システム（WMS）

規模：
- COBOL 55本（Batch 40 / Online 15）
- 画面 10
- 外部IF 6
- 帳票 8
- VSAM（業務中核）＋ 一部RDB（参照/監査）

移行方式：
ハイブリッド方式
（周辺ラップ＋状態遷移中核リライト）

---

## ドキュメント構成

本体系は `docs` ディレクトリ以下に格納されています。

```
docs/
├── 01_Project-Overview.md          ← 研究室の憲法・3層構造の定義
├── 02_Scope-Definition-Framework.md ← スコープ定義の原則・フレームワーク
├── 03_Scope-Granularity-Levels.md  ← 粒度の5段階モデル（Level 0-4）
├── 04_Migration-Decision-Mapping.md ← 構造観測から判断への写像モデル
├── 05_Case-Study-Virtual-Project.md ← 仮想WMSへの適用ケース（実証）
└── 06_Structural-Failure-Analysis.md ← 移行破綻の構造パターン（反証・防止）

log/
└── working-memo/                   ← 研究過程の作業メモ・実験ログ
```

### 論理的構造

```
理論定義（02・03・04）
    ↓ 実証
仮想適用（05）
    ↓ 反証・強化
破綻解析（06）
    ↓ 前提に還る
研究基盤（01）
```

---

## モデルの構成

### 1. I/O Boundary Scope

入出力単位でスコープを定義し、
移行境界を明確化します。

- 入荷
- 出荷
- 在庫参照
- 在庫統制
- マスタ同期
- 外部IFハブ

---

### 2. Data Ownership Control

データ更新責任を一意に定義します。

- SoR（System of Record）の明確化
- Write Ownerの固定
- 状態遷移責任点の定義

これにより段階移行中の二重更新・整合性崩壊を防止します。

---

### 3. Migration Decision Mapping

以下を判断材料として整理します。

- どこをラップするか
- どこをリライトするか
- どの順で段階移行するか
- どのリスクを観測・制御するか

---

### 4. Structural Failure Patterns

移行破綻を「失敗事例」ではなく「構造パターン」として定義します。

| パターン | 定義 |
|:---|:---|
| Boundary Permeability | 境界が契約ではなく実装詳細で結合している状態 |
| SoR Split-Brain | 同一データに複数の正本が存在する状態 |
| State Entropy | 状態遷移が新旧に断片化し決定不能になる状態 |

これにより、移行可否判断は構造的・予防的に行えます。

---

## 移行可否の構造的判断基準

本体系を通じて定義された、Go/No-Go判断の3条件：

1. **境界が閉鎖されているか？**（Boundary Permeability の排除）
2. **更新責任が単一か？**（SoR Split-Brain の排除）
3. **状態遷移が一貫しているか？**（State Entropy の排除）

これらの構造条件が満たされない状態での移行は、
リソース投入量に関わらず、構造的に破綻します。

---

## 本モデルの位置付け

本リポジトリは実案件の成果物ではなく、
移行設計能力の提示を目的とした
設計モデルおよび仮想適用ケースです。

目的は、

「実装」ではなく
「移行を成立させる設計条件を定義できること」

の明示にあります。

---

## 想定される活用領域

- レガシー刷新の事前分析
- 段階移行計画策定
- 移行リスク整理
- 移行方式比較検討
- 移行破綻の予防的診断

---

## English Summary

This repository presents a practical migration scope design model
for legacy system migration projects.

It focuses on:

- I/O-based scope definition
- Data ownership (SoR / Write owner) control
- Hybrid migration strategy (wrap peripheral / rewrite core)
- Migration decision mapping (structure → judgment)
- Structural failure pattern classification

The three structural failure patterns (Boundary Permeability,
SoR Split-Brain, State Entropy) define the conditions under which
migration inevitably fails, regardless of resource investment.

The included warehouse management case study is a virtual example
to demonstrate how migration boundaries can be defined and controlled
to enable phased migration without breaking system integrity.