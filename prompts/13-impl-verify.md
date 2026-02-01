# 実装検証 プロンプト

**フェーズ**: 実装検証
**担当**: {{IMPL_ROLE}}
**目的**: テスト・検証
**推奨ツール**: Codex
**理由**: テスト実行、動作確認、デバッグに強み

---

## プロンプト

```
これは実行タスク。
実装が {{SPEC_DIR}}/** を満たしているか検証せよ。

## 前提

- **AGENTS.md の行動規範に従うこと**
- 明示的な実行指示を受けてから検証を開始

## 検証項目

### 基本検証
- Schema検証（データ構造が仕様通りか）
- Golden一致確認（出力が期待値と一致するか）
- 受け入れテスト（機能が仕様通り動作するか）

### モック乖離検証
- 実装画面がモック/ワイヤーフレームと一致しているか
- 全状態（通常、エラー、ローディング、空）が実装されているか
- レイアウト、色、フォントがモック通りか
- 乖離がある場合、モックを更新するか実装を修正するか判断

### エラー処理検証
- 空のcatchブロックがないか（grep "catch.*{\\s*}" でチェック）
- console.logのみのエラー処理がないか
- フォールバック黙殺がないか
- エラー時のユーザー通知が実装されているか
- API エラー時の挙動が仕様通りか

### ログ出力検証
- LOG_LEVEL環境変数で切り替え可能か
- API呼び出し前後にログが出力されているか
- 状態変更時にログが出力されているか
- ログフォーマットが仕様通りか

### テスト実装検証
- Logic層の単体テストが実装されているか
- カバレッジ目標（Logic層80%、Data層70%）を満たしているか
- 外部依存が適切にモック化されているか
- テスト可能な設計（DI、純粋関数分離）が実装されているか

### デバッグ支援機能検証
- 状態ダンプ関数が実装されているか
- エラー発生時の自動状態スナップショットが動作するか
- 開発者ツール連携（状態インスペクター等）が機能するか

## 判定

### 全てPASSの場合

次のみ出力せよ：

PASS: ALL TESTS PASSED
- Schema: PASS
- Golden: PASS
- Acceptance: PASS
- MockSync: PASS
- ErrorHandling: PASS
- Logging: PASS
- TestStrategy: PASS
- DebugSupport: PASS

完了フェーズ（14-impl-complete）へ進め。

### 失敗がある場合

次のみ出力せよ：

FAIL: TESTS FAILED
- Schema: [PASS/FAIL]
- Golden: [PASS/FAIL]
- Acceptance: [PASS/FAIL]
- MockSync: [PASS/FAIL]
- ErrorHandling: [PASS/FAIL]
- Logging: [PASS/FAIL]
- TestStrategy: [PASS/FAIL]
- DebugSupport: [PASS/FAIL]
- 失敗詳細: [詳細]

実装フェーズ（12-impl-execute）へ戻れ。
```

---

## 補足

### 設計問題の発見時（実装→設計フィードバック）

検証中に仕様の問題（曖昧さ、矛盾等）が見つかった場合：

```
STOP: SPEC ISSUE FOUND
- 問題: [問題の説明]
- 対象ファイル: [ファイル名]
- 問題種別: [ambiguous | undefined | inconsistent | impractical]
- 影響範囲: [影響するモジュール・機能]
- 発見経緯: [どの検証で発見されたか]
```

この場合、設計フェーズへの差し戻しが必要。

### フィードバック記録（必須）

設計問題を発見した場合、以下のファイルに記録すること：

`{{SPEC_DIR}}/feedback/impl-feedback.yml`

```yaml
feedback:
  - date: 2026-01-31
    phase: impl-verify
    issue_type: ambiguous
    file: session-spec.md
    description: セッション削除時の確認ダイアログ有無が未定義
    impact: UI実装でダイアログ有無を決定できない
    resolution: 設計担当が仕様を明確化（確認ダイアログ必須と定義）
```

**フィードバック種別:**
| 種別 | 説明 | 差し戻し先 |
|------|------|------------|
| ambiguous | 仕様が曖昧で解釈が分かれる | UX設計 or 詳細設計 |
| undefined | 必要な仕様が未定義 | UX設計 or 詳細設計 |
| inconsistent | 仕様間に矛盾がある | 詳細設計 |
| impractical | 技術的に実現困難 | アーキテクチャ設計 |

**重要:** フィードバックは握りつぶし禁止。必ず記録し、設計担当に通知すること。

---

## 履歴記録（必須）

作業完了時、`.ux-dev-history.yml` に以下を追記せよ：

PASSの場合：
```yaml
- phase: "13-impl-verify"
  status: "complete"
  comment: ""
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

FAILの場合：
```yaml
- phase: "13-impl-verify"
  status: "reject"
  target: "12-impl-execute"
  comment: "失敗した検証項目を1行で"
  timestamp: 現在時刻を取得して記録（`date -u +"%Y-%m-%dT%H:%M:%SZ"` または PowerShell `Get-Date -Format o`）
```

---

## 変数

| 変数 | 説明 |
|------|------|
| `{{SPEC_DIR}}` | 仕様書ディレクトリ（例: docs/ux） |
| `{{IMPL_ROLE}}` | 実装担当名 |
