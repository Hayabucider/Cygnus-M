# Cygnus-M 検証ログ

> keymap と Viewer の整合性確認結果を記録するファイル

---

## 検証の定義

keymap を正として、Viewer が正しく反映されているかを確認する。  
Viewer が合っていなければ Viewer を修正する。keymap を修正しない。

### 確認項目

| 確認項目 | keymap側 | Viewer側 |
|---|---|---|
| pos番号 | keymap 内の pos 定義 | ViewerのposMap定義 |
| キー数 | 各レイヤーのバインド数 | 表示キー数 |
| 親指キー順序 | leftThumbs / rightThumbs の順 | Viewer の配置順 |
| エンコーダー | sensor-bindings の記述 | Viewer のencoder表示 |

---

## 記録フォーマット

```markdown
## YYYY-MM-DD

### 実施内容
（何を変更・確認したか）

### 確認結果

| 項目 | 結果 | 備考 |
|---|---|---|
| pos番号 | ✅ 一致 / ❌ 不一致 | |
| キー数 | ✅ 一致 / ❌ 不一致 | |
| 親指キー順序 | ✅ 一致 / ❌ 不一致 | |
| エンコーダー | ✅ 一致 / ❌ 不一致 | |

### 対応内容
（不一致があった場合の対応）

### 実機テスト
- [ ] 未実施
- [ ] 実施済み → 結果:

### バックアップ
- ファイル名:
- 種別: pre-flash / verified / pre-edit
```

---

## 検証記録

<!-- 以下に検証結果を追記していく -->

## 2026-06-23（初期構築）

### 実施内容
GitHub 管理体制の構築。keymap・Viewer・仕様書の初期配置。

### 確認結果
初期構築のため、keymap・Viewer ともに未配置。整合性確認は Viewer 配置後に実施。

### 対応内容
- `config/Cygnus-M.keymap` を配置後、Viewer と突き合わせ確認を行う

### 実機テスト
- [ ] 未実施

### バックアップ
- まだ取得していない
