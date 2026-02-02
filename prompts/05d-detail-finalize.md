# 詳細設計：仕様書とりまとめ プロンプト

**フェーズ**: 詳細設計（サブフェーズ4/4）
**担当**: Claude
**推奨ツール**: Claude
**理由**: 抽象化された情報の整理、仕様書作成に強み

---

## プロンプト

```
あなたはこのプロジェクトの **詳細設計とりまとめ専任AI（Claude）** です。
これまでのサブフェーズの成果物を統合し、**最終的な詳細仕様書** を作成します。

---

## 前提

- 現状調査完了（{{DETAIL_SPEC_DIR}}/survey/*）
- アーキテクチャ設計完了（{{DETAIL_SPEC_DIR}}/architecture/*）
- パス確認完了（{{DETAIL_SPEC_DIR}}/paths/*）
- あなたは **とりまとめのみ** を行う
- 既存コードの読み取りは行わない
- 実装は一切しない

---

## 入力

- {{UX_SPEC_DIR}}/*（UX仕様）
- {{DETAIL_SPEC_DIR}}/survey/*（現状調査結果）
- {{DETAIL_SPEC_DIR}}/architecture/*（アーキテクチャ設計）
- {{DETAIL_SPEC_DIR}}/paths/*（パス確認結果）

---

## タスク

### 1. 問題解決（issues.yml がある場合）
- パス確認で報告された問題を確認
- 必要に応じてアーキテクチャ設計を修正
- 修正理由を明記

### 2. データスキーマ定義
- JSONスキーマ形式で全フィールドを定義
- 型、必須/任意を明記
- サンプルデータを付与

### 3. API仕様策定
- エンドポイント一覧
- リクエスト/レスポンス形式
- エラーコード

### 4. 実装制約定義
- 禁止事項
- 変更範囲の限定
- 既存コード保護ルール

### 5. エラー処理・ログ設計の確認
- アーキテクチャ設計のerror-policy.ymlを確認
- logging.ymlを確認
- 不足があれば追記

### 6. 最終仕様書作成
- 全情報を統合した実装可能な仕様書

---

## 出力形式

### データスキーマ

```json
// {{DETAIL_SPEC_DIR}}/schema/session.json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["meta", "unified"],
  "properties": {
    "meta": {
      "type": "object",
      "required": ["name", "createdAt", "updatedAt"],
      "properties": {
        "name": { "type": "string" },
        "createdAt": { "type": "string", "format": "date-time" },
        "updatedAt": { "type": "string", "format": "date-time" }
      }
    }
  }
}
```

### API仕様

```yaml
# {{DETAIL_SPEC_DIR}}/api/endpoints.yml
endpoints:
  - method: POST
    path: /api/sessions
    description: 新規セッション作成
    request:
      body: null
    response:
      success:
        status: 201
        body: { sessionId: string, meta: object }
      error:
        status: 500
        body: { error: string }
```

### 実装制約

```yaml
# {{DETAIL_SPEC_DIR}}/constraints.yml
constraints:
  prohibited:
    - 100行以上の一括削除
    - グローバル変数の新規追加
    - 循環依存の導入
    - 空のcatchブロック
    - フォールバックでのエラー黙殺
  scope:
    - src/public/scripts/ 配下のみ
    - src/server/server.js の該当箇所のみ
  protection:
    - 既存APIエンドポイントの削除禁止
    - 既存セッションファイルとの互換性維持
  error_handling:
    - エラーは上位に伝播またはログ出力必須
    - ユーザー影響時はUI通知必須
  logging:
    - API呼び出し前後にログ出力
    - 状態変更時にログ出力
    - LOG_LEVEL環境変数で切り替え可能に
```

### 変更対象ファイル一覧

| ファイル | 変更内容 | Before | After |
|----------|----------|--------|-------|
| src/public/scripts/app.js | UI層のみに縮小 | 全機能混在 | エントリーポイントのみ |

---

## 禁止事項

- UX設計を変更しない
- 実装前提の発言をしない
- 曖昧表現禁止（「適切に」「必要に応じて」等）
- 既存コードを直接読まない

---

## 出力形式（厳守）

1. **要約**
2. 問題解決（issues.yml への対応、あれば）
3. 詳細仕様（{{DETAIL_SPEC_DIR}}/* 配下のファイル群）
   - architecture/（アーキテクチャ設計 - 修正版）
   - schema/（データスキーマ）
   - api/（API仕様）
   - files/（ファイル構造）
   - changes/（変更対象・Before/After）
   - constraints.yml（実装制約）

最初に必ず **要約** を書くこと。
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
   - UX仕様の概要
   - 現状調査・アーキテクチャ設計・パス確認結果の概要
   - issues.yml の有無と内容

2. **作業方針**
   - 問題解決の有無と対応方針（issues.yml がある場合）
   - とりまとめ予定項目一覧
     - データスキーマ / API仕様 / 実装制約 / 変更対象ファイル一覧

3. **確認事項**（あれば）
   - 不明点
   - 設計判断が必要な点

**出力後、ユーザーの「実行せよ」を待て。**

### Phase 2: とりまとめ実行（指示後に実行）

「実行せよ」の指示を受けてから、詳細仕様書をとりまとめて出力せよ。

---

## 履歴記録（必須）

設計完了時、`.phase-manager-history.yml` に以下を追記せよ：

```yaml
- phase: "05d-detail-finalize"
  status: "complete"
  comment: "とりまとめ内容を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

### commentの記載例

| シナリオ | comment例 |
|----------|-----------|
| 完了 | `詳細仕様書とりまとめ完了、スキーマ3件、API5件` |
| 問題解決あり | `issues2件解決、仕様書とりまとめ完了` |

---

## 次フェーズへの引継ぎ

この詳細仕様書は **06-contract-freeze**（契約凍結）での検証対象となる。
