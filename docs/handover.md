# Cygnus-M 引き継ぎ書

> 最終更新: 2026-06-24
> 目的: 他 AI・未来の自分がプロジェクトを迷わず再開できるようにする

---

## プロジェクト概要

Cygnus-M は 51 キーの分割エルゴノミクスキーボード。
ZMK firmware を使用し、Windows / macOS / Android / iPad に対応する 11 レイヤー構成のキーマップを構築している。
VPM（Virtual Primary Modifier）設計思想により、OS間でキーバインドの体感を統一している。

---

## 正典ファイルの場所

```
config/Cygnus.keymap   ← 唯一の正典。常にこれを基準にする。
```

---

## ハードウェア仕様（確定値）

| 項目 | 値 |
|---|---|
| 総キー数 | 51キー（左26 + エンコーダー1 + 右24） |
| レイヤー数 | 11（Layer 0〜10） |
| 左親指キー | 3キー（pos43: 英数/L8, pos44: かな/Alt, pos45: Space/Sub） |
| 右親指キー | 4キー（pos46: →, pos47: BSPC, pos48: Enter/L9, pos49: DEL） |
| エンコーダー | 左手側 1個（pos18） |

> ⚠️ pos番号は 0-indexed（SVG の keypos 定義に準拠）。

---

## レイヤー構成（確定）

| Layer | 名前 | 役割 | アクセス方法 |
|---|---|---|---|
| 0 | Win_Base | Windows ベース入力 | `to 0` マクロ |
| 1 | Win_Sub | Windows ショートカット / Premiere Pro | pos45 Space 長押し |
| 2 | Mac_Base | macOS ベース入力 | `to 2` マクロ |
| 3 | Mac_Sub | macOS ショートカット / Premiere Pro | pos45 Space 長押し |
| 4 | Android_Base | Android ベース（L0 と同構成） | `to 4` マクロ |
| 5 | Android_Sub | Android 向けショートカット | pos45 Space 長押し |
| 6 | iPad_Base | iPad ベース（L2 と同構成） | `to 6` マクロ |
| 7 | iPad_Sub | iPadOS 向けショートカット | pos45 Space 長押し |
| 8 | SymNum | 記号（左手）＋テンキー（右手） | pos43 LANG2 長押し（唯一の経路） |
| 9 | Nav/Func | F1〜F12 / 矢印 / マウスクリック | pos48 RET 長押し または pos24 MINUS 長押し |
| 10 | Management | BT設定 / OS切替 / BOOTLOADER | pos0 ESC 長押し |

---

## 設計確定事項（2026-06-22〜24）

以下は確定済み。後から覆さない。

| 項目 | 内容 |
|---|---|
| L8アクセス | pos43（LANG2長押し）のみ。エルゴルール準拠 |
| L8アクセス実装 | 標準 `&lt` から専用 `hold-preferred` behavior `&l8_lt 8 LANG2` に変更済み |
| 廃止済み経路 | `lt 8 L`（pos23）と `lt 8 BSPC`（pos47）は廃止済み |
| L9アクセス | pos48（RET長押し）+ pos24（MINUS長押し）の2経路 |
| pos45の制約 | 絶対に `&mo` 系にしない（bootloaderアクセスバグ） |
| OS切替 | マクロ（win_switch / mac_switch / and_switch / pad_switch）で BT自動接続と同時実行 |
| L8 JIS対応 | L8記号はUS配列コードではなくJIS実キー操作で実装。`STAR`/`DQT`等へ戻さない |
| Windows設定 | L8を正しく使うには Windows 側を「日本語キーボード 106/109」に設定すること |

---

## L8 JIS対応の考え方（重要）

ZMK の `.keymap` では「出したい文字名」を書けばよいわけではない。
JIS環境では以下のように考える必要がある。

```
出したい記号 → JISキーボードでその記号を出す実キー操作 → ZMK keycode で書く
```

例：

| 出したい記号 | JISでの操作 | ZMK keycode |
|---|---|---|
| `!` | Shift + 1 | `&kp LS(N1)` |
| `"` | Shift + 2 | `&kp LS(N2)` |
| `(` | Shift + 8 | `&kp LS(N8)` |
| `` ` `` | Shift + [ | `&kp LS(LBKT)` |

Keymap Editor 上では `Sft+1` のように表示されるが、これは正常。
JIS環境で `!` を出すための実キー操作を表示しているだけ。

**`GRAVE` の罠**: JIS/Windows環境では `&kp GRAVE` がバッククォートではなく入力切替として動作する。バッククォートには `&kp LS(LBKT)` を使う。

---

## VPM（Virtual Primary Modifier）設計思想

- OS差異を吸収するため、論理的な「仮想修飾キー」を定義する
- Windows（Ctrl）/ macOS（Cmd）/ Android（Ctrl）/ iPad（Cmd）で同一物理キーが対応するOSの修飾キーを出力する
- Sub レイヤー（L1/L3/L5/L7）は同一物理位置が同一機能になるよう設計
- エルゴノミクスルール: 修飾キーは親指・ホームポジション付近に集中させる

---

## バックアップの取得タイミング

| タイミング | ファイル名例 |
|---|---|
| 実機書き込み前 | `Cygnus_backup_YYYY-MM-DD_pre-flash.keymap` |
| 動作確認が取れたとき | `Cygnus_backup_YYYY-MM-DD_verified.keymap` |
| 大きなレイヤー変更前 | `Cygnus_backup_YYYY-MM-DD_pre-edit.keymap` |

`verified` が付いているものが「実機で動作確認済み」の最終安定版。
`backup/` にあるファイルは参照専用。直接編集しない。

---

## 関連ファイルの役割

| ファイル | 役割 |
|---|---|
| `config/Cygnus.keymap` | 唯一の正典。ZMK ビルドに使う実装ファイル |
| `docs/README.md` | `docs/` 配下の案内板 |
| `docs/Cygnus-M-viewer.html` | keymap 正典の内容を視覚確認するための HTML Viewer |
| `docs/viewer-spec.md` | Viewer 修正・再作成時の仕様書（pos番号マッピング含む） |
| `docs/handover.md` | このファイル。他 AI・未来の自分への引き継ぎ |
| `docs/validation-report.md` | keymap と Viewer の整合性確認ログ |
| `backup/` | 過去版の退避場所（参照専用） |

---

## 他 AI に依頼するときの原則

1. 変更してよい範囲を明示する
2. 変更禁止事項を明示する（特に pos番号・物理配置・L8/L9アクセス経路）
3. 完了条件を明示する
4. 不明点は勝手に補完せず確認するよう明示する

Viewer 修正を依頼する場合は `docs/viewer-spec.md` の末尾にある指示文をそのまま使う。

---

## 現在の状態・残タスク

> ここは作業のたびに更新する

### 完了済み（2026-06-24）
- [x] L8アクセス問題の解決：`hold-preferred` behavior `l8_lt` を追加し `&l8_lt 8 LANG2` に変更
- [x] L8記号レイヤーのJIS対応：US配列コードからJIS実キー操作へ置き換え
- [x] Windows側キーボード設定を「日本語キーボード 106/109」に変更
- [x] バックアップ取得：`config/backup/Cygnus_backup_2026-06-23_pre-flash.keymap`

### 残タスク
- [ ] L8記号の実機テスト完了確認（主要記号は動作済み、未割当キー押下時の出力確認）
- [ ] Viewer の L8 表示を JIS 期待出力（`!` `` ` `` `[` `]` ...）に同期
- [ ] 動作確認後に `verified` バックアップを取得
- [ ] `\` と `¥` の表示差の確認（コード用途で問題がある場合）

---

## 更新ルール

- 大きな変更をしたあとは「現在の状態・残タスク」と「設計確定事項」を更新する
- バックアップを取ったらファイル名と理由をここに記録してもよい
- この引き継ぎ書自体も正典ではない。事実と異なる記述は修正する
