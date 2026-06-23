# Cygnus-M docs/

このディレクトリは、Cygnus-M キーマッププロジェクトの補助資料置き場です。  
ZMK ビルドに使う正典ファイルは `config/Cygnus-M.keymap` です。ここには含まれません。

---

## 重要な原則

```
正典は config/Cygnus-M.keymap である。
Viewer・Trainer はいずれも正典ではなく、確認・練習用の派生ツールである。
Viewer や Trainer だけを見て keymap を修正しない。
```

---

## ファイルの派生関係

```
config/Cygnus-M.keymap             ← 唯一の正典
        ↓
docs/Cygnus-M-viewer.html          ← 正典の可視化ツール
        ↓
docs/cygnus_m_typing_trainer.html  ← キーマップ習得のための練習ツール
```

---

## ファイル一覧

| ファイル | 役割 |
|---|---|
| `Cygnus-M-viewer.html` | keymap 正典の内容を視覚確認するための HTML Viewer |
| `viewer-spec.md` | Viewer を修正・再作成する際の仕様書。デザイン基準・変更禁止事項を記載 |
| `cygnus_m_typing_trainer.html` | Cygnus-M 専用タイピング練習ブラウザ |
| `trainer-spec.md` | Trainer を修正・拡張する際の仕様書。デザイン基準・実装済み機能・変更禁止事項を記載 |
| `handover.md` | 他 AI・未来の自分に渡すための引き継ぎ書 |
| `validation-report.md` | keymap と Viewer の整合性確認結果を残す検証ログ |

---

## 更新時の順序

1. `config/Cygnus-M.keymap` を先に更新する
2. Viewer を keymap に合わせて同期する（`viewer-spec.md` 参照）
3. Trainer を keymap・Viewer に合わせて同期する（`trainer-spec.md` 参照）
4. 整合性を確認し `validation-report.md` に結果を記録する
5. 問題なければ動作確認済みバックアップを `backup/` に保存する

---

## 注意事項

- Viewer・Trainer の見た目だけを根拠に keymap を変更しない
- 他 AI に Viewer 修正を依頼するときは必ず `viewer-spec.md` を渡す
- 他 AI に Trainer 修正を依頼するときは必ず `trainer-spec.md` を渡す
- `backup/` にあるファイルは直接編集しない（参照専用）
