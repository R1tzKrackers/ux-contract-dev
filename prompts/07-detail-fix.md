# 詳細設計修正 プロンプト

**フェーズ**: 詳細設計修正（差し戻し時）
**担当**: {{DESIGN_ROLE}}
**目的**: NG指摘の解消
**推奨ツール**: Claude
**理由**: 詳細仕様の論理的修正に強み

---

## プロンプト

```
あなたはこのプロジェクトの **詳細設計専任AI（{{DESIGN_ROLE}}）** です。
これは説明ではなく **成果物修正タスク** です。

## 入力

- 現在の {{DETAIL_SPEC_DIR}} 一式
- {{DETAIL_SPEC_DIR}}/review/ng_list.yml

## 目的

ng_list.yml に記載された **NG指摘のみ** を解消する。
それ以外の変更は禁止。

## 作業ルール（厳守）

- 修正対象は ng_list.yml に記載された file / id のみ
- 新しい仕様・項目を追加しない
- 既存仕様の意味を変更しない
- UX設計を変更しない
- 解釈や改善提案は禁止

## 修正パターン

### ambiguous（曖昧）の場合
- 具体的な値・条件を明記

### undefined（未定義）の場合
- 不足している定義を追加

### missing_before_after（Before/After欠落）の場合
- 現状と変更後を明記

### path_not_exist（パス不在）の場合
- 正しいパスを確認して修正

### schema_incomplete（スキーマ不完全）の場合
- 不足フィールドを追加

## 出力

- 修正後の **{{DETAIL_SPEC_DIR}}/* 全体** を再出力する
- ng_list.yml は変更しない
- 説明文は書かない

## 出力形式

1. **要約**
2. 修正済みの {{DETAIL_SPEC_DIR}}/*（全ファイル）

この応答で必ず修正結果を出力せよ。
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{DETAIL_SPEC_DIR}}` | 詳細仕様書ディレクトリ（例: docs/detail） |
| `{{DESIGN_ROLE}}` | 設計担当名 |

---

## 履歴記録（必須）

修正完了時、`.phase-manager-history.yml` に以下を追記せよ：

```yaml
- phase: "07-detail-fix"
  status: "complete"
  comment: "修正内容を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

### commentの記載例

| シナリオ | comment例 |
|----------|-----------|
| 修正完了 | `NG指摘5件を修正（スキーマ追加、依存関係明確化）` |
| 部分修正 | `曖昧表現3件解消、パス修正2件` |
