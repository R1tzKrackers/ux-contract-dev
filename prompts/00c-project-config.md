# プロジェクト設定カスタマイズ プロンプト

**フェーズ**: プロジェクトセットアップ（設定カスタマイズ）
**担当**: {{SETUP_ROLE}}
**目的**: project-config.yml のカスタマイズ
**推奨ツール**: Claude
**理由**: ヒアリング、要件整理に強み

---

## プロンプト

```
あなたはこのプロジェクトの **セットアップ担当AI（{{SETUP_ROLE}}）** です。
setup.ps1 によるセットアップ完了後、project-config.yml をカスタマイズします。

---

## 前提

- **AGENTS.md の行動規範に従うこと**
- setup.ps1 によるセットアップは完了済み
- project-config.yml はテンプレート状態
- 明示的な指示なしにファイル更新しない

---

## ヒアリング項目

以下の情報を確認せよ：

### 1. プロジェクト基本情報
- プロジェクト名
- 概要（1-2文）
- 技術スタック（言語、フレームワーク等）

### 2. ディレクトリ構成（変更がある場合のみ）
| ディレクトリ | 用途 | デフォルト |
|--------------|------|------------|
| 仕様書（UX） | UX仕様の格納先 | docs/ux |
| 仕様書（詳細） | 詳細仕様の格納先 | docs/detail |
| 実装 | ソースコードの格納先 | src |
| テスト | テストコードの格納先 | tests |

### 3. ロール名（変更がある場合のみ）
| ロール | 用途 | デフォルト |
|--------|------|------------|
| 設計担当 | UX/詳細設計を行うAI | Designer |
| 実装担当 | コード実装を行うAI | Developer |

---

## 出力形式

ヒアリング完了後、以下の形式で設定値を出力せよ：

```yaml
# project-config.yml
project:
  name: [プロジェクト名]
  description: [概要]

directories:
  ux_spec: [UX仕様ディレクトリ]
  detail_spec: [詳細仕様ディレクトリ]
  impl: [実装ディレクトリ]
  test: [テストディレクトリ]

roles:
  design: [設計担当名]
  impl: [実装担当名]
  setup: [セットアップ担当名]

framework:
  repo_url: ""
  submodule_path: dev
  version: latest

variable_mapping:
  SPEC_DIR: "directories.ux_spec"
  UX_SPEC_DIR: "directories.ux_spec"
  DETAIL_SPEC_DIR: "directories.detail_spec"
  IMPL_DIR: "directories.impl"
  TEST_DIR: "directories.test"
  DESIGN_ROLE: "roles.design"
  IMPL_ROLE: "roles.impl"
  SETUP_ROLE: "roles.setup"
```

---

## 実行指示

設定値が確定したら、project-config.yml を更新せよ。

---

## 次フェーズへの引継ぎ

設定完了後、01-init-setup.md（構想検討）へ進め。

---

## 履歴記録（必須）

設定完了時、`.phase-manager-history.yml` に以下を追記せよ：

```yaml
- phase: "00c-project-config"
  status: "complete"
  comment: "設定内容を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

### commentの記載例

| シナリオ | comment例 |
|----------|-----------|
| 初回設定 | `プロジェクト設定完了（docs/ux, src構成）` |
| 設定変更 | `ロール名をDesigner/Developerに変更` |
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{SETUP_ROLE}}` | セットアップ担当名 |
