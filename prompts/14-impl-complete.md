# 実装完了 プロンプト

**フェーズ**: 実装完了
**担当**: {{IMPL_ROLE}}
**目的**: 完了確認
**推奨ツール**: Codex
**理由**: 完了確認、最終チェックに強み

---

## プロンプト

```
これは実行タスク。
未対応要件がないことを確認せよ。

## 確認項目

- {{SPEC_DIR}}/** の全要件が実装されているか
- 全テストがPASSしているか
- 未実装・未対応の項目がないか

## 判定

### 全て完了の場合

次のみ出力せよ：

DONE: IMPLEMENTATION COMPLETE
- 実装ファイル数: [数]
- テスト結果: ALL PASS
- 未対応要件: なし

### 未対応がある場合

次のみ出力せよ：

INCOMPLETE: PENDING ITEMS EXIST
- 未対応項目: [一覧]

実装フェーズ（09-impl-execute）へ戻れ。
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{SPEC_DIR}}` | 仕様書ディレクトリ（例: docs/ux） |
| `{{IMPL_ROLE}}` | 実装担当名 |

---

## 履歴記録（必須）

実装完了時、`.phase-manager-history.yml` に以下を追記せよ：

```yaml
- phase: "14-impl-complete"
  status: "complete"
  comment: "完了内容を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

### commentの記載例

| シナリオ | comment例 |
|----------|-----------|
| 完了 | `実装完了: 8ファイル、テスト全件PASS` |
| 未完了 | `未対応項目あり: ログ出力機能` |
