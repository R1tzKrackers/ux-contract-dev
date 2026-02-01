# 実装 プロンプト

**フェーズ**: 実装
**担当**: {{IMPL_ROLE}}
**目的**: コード実装
**推奨ツール**: Codex
**理由**: コード生成、ファイル操作に特化

---

## プロンプト

```
これは実行タスク。
{{SPEC_DIR}}/** を逐語的に満たす実装を行え。

## 前提

- **AGENTS.md の行動規範に従うこと**
- 明示的な実行指示（「実装せよ」「開始」等）を受けてから実装を開始

## 実装ルール

- 差分単位で変更
- 仕様外の挙動禁止
- 追加仕様禁止
- 実装領域: {{IMPL_DIR}}

## エラー処理ルール（必須）

**原則: fail-fast（エラーは即時報告、握りつぶし禁止）**

### 禁止パターン
```javascript
// NG: 空のcatch
try { ... } catch (e) {}

// NG: console.logのみ
try { ... } catch (e) { console.log(e) }

// NG: フォールバック黙殺
try { ... } catch (e) { return defaultValue }
```

### 必須パターン
```javascript
// OK: 上位に伝播
try { ... } catch (e) { throw e }

// OK: ログ出力 + ユーザー通知
try { ... } catch (e) {
  logger.error('操作失敗', { error: e, context: ... })
  showErrorToUser('操作に失敗しました')
}
```

## ログ出力ルール（必須）

- API呼び出し前後にログ出力
- 状態変更時にログ出力
- LOG_LEVEL環境変数で切り替え可能に
- フォーマット: `[{timestamp}] [{level}] [{module}] {message}`

## コードコメントルール（必須）

**AGENTS.md セクション7 に従うこと**

- Why（なぜ）はコメント必須、What（何）はコード自体で表現
- ファイルヘッダに `@see` で対応する設計書を記載
- 設計判断・制約・非自明なロジックにはコメント必須
- 自明な処理にはコメント不要

## 実装対象

{{IMPL_TARGETS}}

## 禁止事項

- {{SPEC_DIR}}/** の変更
- 仕様にない機能の追加
- 「改善」「最適化」の名目での仕様変更

## 出力

実装コードを出力せよ。
実装後、検証フェーズ（13-impl-verify）へ進め。

---

## 履歴記録（必須）

実装完了時、`.ux-dev-history.yml` に以下を追記せよ：

```yaml
- phase: "12-impl-execute"
  status: "complete"
  comment: "実装内容を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{SPEC_DIR}}` | 仕様書ディレクトリ（例: docs/ux） |
| `{{IMPL_DIR}}` | 実装ディレクトリ（例: src） |
| `{{IMPL_TARGETS}}` | 実装対象の一覧（08-impl-readyで特定） |
| `{{IMPL_ROLE}}` | 実装担当名 |
