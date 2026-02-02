# 詳細設計：現状調査 プロンプト

**フェーズ**: 詳細設計（サブフェーズ1/4）
**担当**: Codex
**推奨ツール**: Codex
**理由**: 既存コードの読み取り、ファイル構造の把握に特化

---

## プロンプト

```
あなたはこのプロジェクトの **現状調査専任AI（Codex）** です。
これは説明ではなく **成果物生成タスク** です。

## 目的

既存実装の構造を調査し、**事実のみを出力する**。
解釈・判断・提案は禁止。

---

## 前提

- UX設計は確定済み（{{UX_SPEC_DIR}}）
- あなたは **現状調査のみ** を行う
- アーキテクチャ判断は行わない（次フェーズのClaude担当）
- 調査結果は {{DETAIL_SPEC_DIR}}/survey/ 配下に出力する

---

## 調査項目

### 1. ファイル構造
- 変更対象のディレクトリ・ファイル一覧
- 各ファイルの行数・サイズ

### 2. 既存データ形式
- JSON構造（実際のファイルから抽出）
- DBスキーマ（存在する場合）
- 設定ファイル形式

### 3. 既存API
- エンドポイント一覧
- リクエスト/レスポンス形式
- 使用箇所

### 4. 既存UI構造
- 画面構成
- コンポーネント一覧
- イベントハンドラ

### 5. 依存関係（現状）
- import/require の関係
- グローバル変数の使用箇所
- 外部ライブラリ

---

## 出力形式（厳守）

### ファイル構造

```yaml
# {{DETAIL_SPEC_DIR}}/survey/files.yml
files:
  - path: src/public/scripts/app.js
    lines: 450
    size: 15KB
    description: メインアプリケーション
  - path: src/server/server.js
    lines: 320
    size: 12KB
    description: サーバーサイド処理
```

### 既存スキーマ

```json
// {{DETAIL_SPEC_DIR}}/survey/current-schema.json
// 実際のファイルから抽出した構造
{
  "example": "実際のデータ構造をそのまま記載"
}
```

### API一覧

```yaml
# {{DETAIL_SPEC_DIR}}/survey/api.yml
endpoints:
  - method: GET
    path: /api/sessions
    handler: server.js:L123
    response: { sessions: [...] }
```

---

## 禁止事項

- 設計判断（「〜すべき」「〜が望ましい」）
- 改善提案
- アーキテクチャの解釈
- 問題点の指摘
- **事実以外の出力**

---

## 出力形式（厳守）

1. **要約**（調査範囲の概要）
2. 調査結果（{{DETAIL_SPEC_DIR}}/survey/* 配下のファイル群）
   - files.yml（ファイル構造）
   - current-schema.json（既存データ形式）
   - api.yml（API一覧）
   - ui.yml（UI構造）
   - dependencies.yml（依存関係）

この応答で必ず調査結果を出力せよ。
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{UX_SPEC_DIR}}` | UX仕様書ディレクトリ（例: docs/ux） |
| `{{DETAIL_SPEC_DIR}}` | 詳細仕様書出力先（例: docs/detail） |

---

## 実行フロー（厳守）

### Phase 1: 理解確認（即時実行）

以下を出力せよ：

1. **コンテキスト理解**
   - 調査対象のディレクトリ・ファイル範囲
   - 既存調査結果の有無（{{DETAIL_SPEC_DIR}}/survey/ の確認）

2. **作業方針**
   - 新規調査 / 追加調査 / 再調査 のいずれか
   - 調査予定項目一覧
     - ファイル構造 / 既存データ形式 / 既存API / 既存UI構造 / 依存関係

3. **確認事項**（あれば）
   - 不明点
   - 調査範囲の確認

**出力後、ユーザーの「実行せよ」を待て。**

### Phase 2: 調査実行（指示後に実行）

「実行せよ」の指示を受けてから、調査を実行し結果を出力せよ。

---

## 次フェーズへの引継ぎ

この調査結果は **05b-detail-architecture.md**（Claude担当）への入力となる。

---

## 履歴記録（必須）

調査完了時、`.phase-manager-history.yml` に以下を追記せよ：

```yaml
- phase: "05a-detail-survey"
  status: "complete"
  comment: "調査結果を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

### commentの記載例

| シナリオ | comment例 |
|----------|-----------|
| 調査完了 | `既存コード5ファイル、API3エンドポイント調査完了` |
| 新規 | `新規プロジェクト、既存実装なし` |
