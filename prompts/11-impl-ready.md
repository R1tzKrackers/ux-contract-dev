# 実装準備 プロンプト

**フェーズ**: 実装準備
**担当**: {{IMPL_ROLE}}
**目的**: 仕様読み込み確認
**推奨ツール**: Codex
**理由**: 仕様と既存コードの突合確認に強み

---

## プロンプト

```
これは実行タスク。
{{SPEC_DIR}}/** を唯一の仕様として全件読み込め。

不足・推測が1つでもあれば即停止し、
次のみ出力せよ：

STOP: UX CONTRACT INCOMPLETE
- 不足項目: [項目名]
- 理由: [理由]

問題がなければ次のみ出力せよ：

READY: UX CONTRACT COMPLETE
- 実装対象: [対象一覧]
```

---

## 補足

### STOPの場合

設計担当への差し戻しが必要。
不足項目を明確にして設計フェーズに戻す。

### READYの場合

実装対象を特定し、次のフェーズ（12-impl-execute）へ進む。

---

## 履歴記録（必須）

READY完了時、`.ux-dev-history.yml` に以下を追記せよ：

```yaml
- phase: "11-impl-ready"
  status: "complete"
  comment: "仕様読み込み完了、実装対象特定済み"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

STOPの場合：
```yaml
- phase: "11-impl-ready"
  status: "reject"
  target: "05d-detail-finalize"
  comment: "不足項目を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{SPEC_DIR}}` | 仕様書ディレクトリ（例: docs/ux） |
| `{{IMPL_ROLE}}` | 実装担当名 |
