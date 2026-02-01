# UX Contract Framework

**UX駆動開発フレームワーク / UX-Driven Development Framework**

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)]()
[![Phase Manager](https://img.shields.io/badge/requires-Phase%20Manager-green.svg)]()

---

## 概要 / Overview

UX Contract Frameworkは、UX設計を起点とした開発ワークフローを提供するフレームワークです。
画面設計から詳細設計、実装までの一貫したフローを定義し、AIと人間のコラボレーションを支援します。

*UX Contract Framework provides a UX-first development workflow.
It defines a consistent flow from screen design to detailed design and implementation, supporting collaboration between AI and humans.*

---

## 特徴 / Features

- **UX駆動設計** - 画面設計を起点とした開発フロー
  *UX-Driven Design - Development flow starting from screen design*

- **契約凍結** - 設計完了後の仕様変更を防止
  *Contract Freeze - Prevents specification changes after design completion*

- **ロール分離** - Designer/Developer の明確な役割分担
  *Role Separation - Clear separation between Designer and Developer roles*

- **レビューゲート** - 各フェーズに人間レビューを組み込み
  *Review Gates - Human review integrated at each phase*

---

## ワークフロー / Workflow

```
[Setup]
  00c-project-config    プロジェクト設定 / Project Configuration
       |
[Design - Init]
  01-init-setup         構想検討 / Concept Development
  01r-init-review       構想レビュー / Concept Review
       |
[Design - UX]
  02-ux-design          UX設計 / UX Design
  02r-ux-review         UX設計レビュー / UX Design Review
  03-ux-verify          UX検証 / UX Verification
  03r-ux-verify-review  UX検証レビュー / UX Verification Review
  04-ux-fix             UX修正（任意）/ UX Fix (optional)
       |
[Design - Detail]
  05a-detail-survey     詳細設計：現状調査 / Detail: Survey
  05b-detail-architecture 詳細設計：アーキテクチャ / Detail: Architecture
  05c-detail-path-confirm 詳細設計：パス確認 / Detail: Path Confirmation
  05d-detail-finalize   詳細設計：とりまとめ / Detail: Finalization
  05r-detail-review     詳細設計レビュー / Detail Review
  06-detail-verify      詳細設計検証 / Detail Verification
  06r-detail-verify-review 詳細検証レビュー / Detail Verification Review
  07-detail-fix         詳細設計修正（任意）/ Detail Fix (optional)
       |
[Handoff]
  08-handoff-check      引継ぎ判定 / Handoff Check
  09-design-freeze      契約凍結 / Contract Freeze
       |
[Implementation]
  10-impl-start         実装開始 / Implementation Start
  11-impl-ready         実装準備 / Implementation Ready
  12-impl-execute       実装 / Implementation Execute
  13-impl-verify        実装検証 / Implementation Verification
  13r-impl-verify-review 実装検証レビュー / Implementation Verification Review
  14-impl-complete      実装完了 / Implementation Complete
```

---

## ディレクトリ構成 / Directory Structure

```
ux-contract-dev/
├── framework.yml      # フレームワークマニフェスト / Framework manifest
├── prompts/           # フェーズ別プロンプト / Phase-specific prompts
│   ├── 00c-project-config.md
│   ├── 01-init-setup.md
│   ├── ...
│   └── 14-impl-complete.md
└── templates/         # テンプレート / Templates
    └── AGENTS.md.template
```

---

## 使用方法 / Usage

### 前提条件 / Prerequisites

- [Phase Manager](https://github.com/pritzcrkers/phase-manager) がインストール済みであること
  *Phase Manager must be installed*

### セットアップ / Setup

1. Phase Managerの初期化フェーズで本フレームワークを選択
   *Select this framework during Phase Manager initialization*

2. `project-config.yml` でプロジェクト固有の設定を行う
   *Configure project-specific settings in `project-config.yml`*

3. Phase Managerを起動してワークフローを開始
   *Start Phase Manager to begin the workflow*

```powershell
# Windows
.\phase-manager.ps1

# Unix/Mac
./phase-manager.sh
```

---

## 変数 / Variables

`project-config.yml` で定義された値がプロンプト内の変数に置換されます。
*Values defined in `project-config.yml` are substituted into prompt variables.*

| 変数 / Variable | 説明 / Description |
|-----------------|---------------------|
| `{{PROJECT_NAME}}` | プロジェクト名 / Project name |
| `{{DESIGN_ROLE}}` | 設計担当ロール名 / Design role name |
| `{{IMPL_ROLE}}` | 実装担当ロール名 / Implementation role name |
| `{{SPEC_DIR}}` | UX仕様書ディレクトリ / UX spec directory |
| `{{DETAIL_SPEC_DIR}}` | 詳細仕様書ディレクトリ / Detail spec directory |
| `{{IMPL_DIR}}` | 実装ディレクトリ / Implementation directory |
| `{{TEST_DIR}}` | テストディレクトリ / Test directory |

---

## ライセンス / License

MIT License

---

## 関連 / Related

- [Phase Manager](https://github.com/pritzcrkers/phase-manager) - ワークフロー管理ツール / Workflow management tool
