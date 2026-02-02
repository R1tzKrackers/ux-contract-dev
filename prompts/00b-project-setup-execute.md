# プロジェクトセットアップ実行 プロンプト

**フェーズ**: プロジェクトセットアップ（実行）
**担当**: {{SETUP_ROLE}}
**目的**: プロジェクト初期構成の作成
**推奨ツール**: Codex
**理由**: ファイル操作、コマンド実行に特化

---

## プロンプト

```
これは実行タスク。
00a-project-setup-plan.md で確定した設定に基づき、プロジェクトをセットアップせよ。

---

## 前提

- **AGENTS.md の行動規範に従うこと**
- 明示的な実行指示を受けてからセットアップを開始
- 設定値は project-config.yml に基づく

---

## 入力

```yaml
{{PROJECT_CONFIG}}
```

---

## 実行手順

### 1. Git初期化（未初期化の場合）

```bash
git init
```

### 2. フレームワークをサブモジュールとして追加

```bash
git submodule add {{FRAMEWORK_REPO_URL}} dev
```

**注意:**
- サブモジュール内のファイルは変更禁止
- フレームワーク更新は `git submodule update --remote` で行う

### 3. ディレクトリ構成作成

```bash
mkdir -p {{UX_SPEC_DIR}}
mkdir -p {{DETAIL_SPEC_DIR}}
mkdir -p {{IMPL_DIR}}
mkdir -p {{TEST_DIR}}
```

### 4. プロジェクト設定ファイル作成

`project-config.yml` を作成（入力値をそのまま使用）

### 5. .gitignore 作成/更新

以下を追加：

```
# プロジェクト固有設定（機密情報を含む可能性）
project-config.yml

# 環境ファイル
.env
.env.local
.env.*.local

# 依存関係
node_modules/
vendor/
__pycache__/

# ビルド成果物
dist/
build/
*.log

# IDE
.idea/
.vscode/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db
```

### 6. 初期コミット

```bash
git add .
git commit -m "Initial project setup with dev framework"
```

---

## 検証項目

セットアップ完了後、以下を確認：

| 項目 | 確認内容 |
|------|----------|
| サブモジュール | `dev/` が正しく追加されているか |
| AGENTS.md | `dev/AGENTS.md` が存在するか |
| プロンプト | `dev/prompt/*.md` が存在するか |
| 設定ファイル | `project-config.yml` が作成されているか |
| .gitignore | `project-config.yml` が除外されているか |
| ディレクトリ | 指定したディレクトリが作成されているか |

---

## 出力

セットアップ完了後、以下を出力せよ：

```
SETUP COMPLETE

プロジェクト: [プロジェクト名]
フレームワーク: dev/ (submodule)

ディレクトリ構成:
├── dev/                  ← フレームワーク（変更不可）
│   ├── AGENTS.md
│   ├── prompt/
│   └── project-config.yml.template
├── {{UX_SPEC_DIR}}/
├── {{DETAIL_SPEC_DIR}}/
├── {{IMPL_DIR}}/
├── {{TEST_DIR}}/
├── project-config.yml    ← プロジェクト固有設定
└── .gitignore

次のステップ:
1. project-config.yml を確認・編集
2. 01-init-setup.md で構想検討を開始
```

---

## 禁止事項

- dev/ 配下のファイル変更
- project-config.yml.template の直接編集（コピーして使用）
- フレームワーク外へのプロンプトファイル配置

---

## clone 時の注意

このリポジトリを clone する際は、サブモジュールも取得すること：

```bash
git clone --recursive [リポジトリURL]
```

または clone 後に：

```bash
git submodule update --init --recursive
```
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{PROJECT_CONFIG}}` | 00a で確定した設定値（YAML） |
| `{{FRAMEWORK_REPO_URL}}` | フレームワークリポジトリURL |
| `{{UX_SPEC_DIR}}` | UX仕様ディレクトリ |
| `{{DETAIL_SPEC_DIR}}` | 詳細仕様ディレクトリ |
| `{{IMPL_DIR}}` | 実装ディレクトリ |
| `{{TEST_DIR}}` | テストディレクトリ |
| `{{SETUP_ROLE}}` | セットアップ担当名 |
