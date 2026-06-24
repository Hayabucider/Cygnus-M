# Cygnus-M Viewer 仕様書

> 最終更新: 2026-06-23  
> 対象ファイル: `docs/Cygnus-M-viewer.html`  
> 正典ファイル: `config/Cygnus.keymap`

---

## 目的

```
このHTML Viewer は、config/Cygnus.keymap の内容を視覚的に確認するための補助ツールである。
正典はあくまで .keymap ファイルであり、Viewer は正典ではない。
Viewer はデザイン成果物ではなく、keymap 正典の可視化ツールである。
```

この一文を忘れると、別AIが見た目重視で作り替えてしまう。

---

## ハードウェア仕様（固定値）

| 項目 | 値 | 備考 |
|---|---|---|
| 総キー数 | 51キー | 左26 + エンコーダー1（左手側）+ 右24 |
| レイヤー数 | 11レイヤー | Layer 0〜10 |
| 左親指キー | 3キー | pos43（英数/L8）, pos44（かな/Alt）, pos45（Space/Sub） |
| 右親指キー | 4キー | pos46（→）, pos47（BSPC）, pos48（Enter/L9）, pos49（DEL） |
| 左端キー | 1キー | pos40（LCTL） |
| 右端キー | 1キー | pos50（RCTL） |
| エンコーダー | 1個 | 左手側。各レイヤーで機能が異なる（下記参照） |

> ⚠️ 上記の値は `config/Cygnus.keymap` の実装から抽出した確定値。
> 変更する場合は keymap 正典を先に更新し、その後ここを修正する。

---

## レイヤー構成（確定）

| Layer | 名前 | 役割 | アクセス方法 |
|---|---|---|---|
| 0 | Win_Base | Windows ベース入力 | `to 0`（mac_switch マクロ経由） |
| 1 | Win_Sub | Windows ショートカット / Premiere Pro | pos45 `lt 1 SPACE` 長押し |
| 2 | Mac_Base | macOS ベース入力 | `to 2`（mac_switch マクロ経由） |
| 3 | Mac_Sub | macOS ショートカット / Premiere Pro | pos45 `lt 3 SPACE` 長押し |
| 4 | Android_Base | Android ベース入力（L0 と同構成） | `to 4`（and_switch マクロ経由） |
| 5 | Android_Sub | Android 向けショートカット | pos45 `lt 5 SPACE` 長押し |
| 6 | iPad_Base | iPad ベース入力（L2 と同構成） | `to 6`（pad_switch マクロ経由） |
| 7 | iPad_Sub | iPadOS 向けショートカット | pos45 `lt 7 SPACE` 長押し |
| 8 | SymNum | 記号（左手）＋テンキー（右手） | pos43 `lt 8 LANG2` 長押し |
| 9 | Nav/Func | F1〜F12 / 矢印 / マウスクリック | pos48 `lt 9 RET` 長押し または pos24 `lt 9 MINUS` 長押し |
| 10 | Management | BT設定 / OS切替 / BOOTLOADER | pos0 `lt 10 ESC` 長押し |

---

## pos番号マッピング（確定）

### 上段（Row 0）: pos 0〜11

| pos | キー（L0基準） |
|---|---|
| 0 | lt 10 ESC |
| 1 | Q |
| 2 | W |
| 3 | E |
| 4 | R |
| 5 | T |
| 6 | Y |
| 7 | U |
| 8 | I |
| 9 | O |
| 10 | P |
| 11 | BSPC |

### 中段（Row 1）: pos 12〜25（+ encoder pos26）

| pos | キー（L0基準） | 備考 |
|---|---|---|
| 12 | TAB | |
| 13 | A | |
| 14 | S | |
| 15 | D | |
| 16 | F | |
| 17 | G | |
| 18 | MCLK（中クリック） | encoder左側インナー |
| 19 | UP | encoder右側インナー |
| 20 | H | |
| 21 | J | |
| 22 | K | |
| 23 | L | |
| 24 | lt 9 MINUS | L9アクセス経路の1つ |
| 25 | RET | |

### 下段（Row 2）: pos 26〜39

| pos | キー（L0基準） | 備考 |
|---|---|---|
| 26 | LSHFT | |
| 27 | Z | |
| 28 | X | |
| 29 | C | |
| 30 | V | |
| 31 | B | |
| 32 | DEL | |
| 33 | DOWN | |
| 34 | N | |
| 35 | M | |
| 36 | COMMA | |
| 37 | DOT | |
| 38 | FSLH | |
| 39 | RSHFT | |

### 最下段（Row 3 / 親指段）: pos 40〜50

| pos | キー（L0基準） | 備考 |
|---|---|---|
| 40 | LCTL | |
| 41 | LGUI | Win=LGUI / Mac=LALT |
| 42 | LALT | Win=LALT / Mac=LGUI |
| 43 | l8_lt 8 LANG2 | tap=英数 / hold=L8(SymNum) ← L8唯一の経路（hold-preferred） |
| 44 | mt LALT LANG1 | tap=かな / hold=Alt(Option) |
| 45 | lt X SPACE | tap=Space / hold=Subレイヤー（OSによりL1/L3/L5/L7） |
| 46 | RIGHT（→） | |
| 47 | BSPC | |
| 48 | lt 9 RET | tap=Enter / hold=L9 ← L9アクセス経路の1つ |
| 49 | DEL | |
| 50 | RCTL | |

---

## エンコーダー動作（レイヤー別）

| Layer | 時計回り | 反時計回り |
|---|---|---|
| 0, 2, 4, 6（Base系） | 音量上 | 音量下 |
| 1, 3, 5, 7（Sub系） | Ctrl+Tab（次タブ） | Ctrl+Shift+Tab（前タブ） |
| 8（SymNum） | Page Up | Page Down |
| 9（Nav/Func） | = （Premiereズームイン） | - （Premiereズームアウト） |
| 10（Management） | 定義なし | 定義なし |

---

## 重要な設計確定事項（変更禁止）

以下は 2026-06-22〜24 に確定した内容。Viewer 修正時も変えない。

- **L8アクセス**: pos43（LANG2長押し）のみ。`lt 8 L`（pos23）と `lt 8 BSPC`（pos47）は廃止済み
- **L8 behavior**: 標準 `&lt` から `hold-preferred` の専用 behavior `&l8_lt 8 LANG2` に変更済み（L8長押し安定化）
- **L8 JIS対応**: L8記号は US配列コード（`STAR`/`DQT` 等）ではなく JIS 実キー操作で実装。元に戻さない
- **Windows設定**: L8正常動作には Windows 側「日本語キーボード 106/109」設定が必須
- **L9アクセス**: pos48（RET長押し）とpos24（MINUS長押し）の2経路
- **pos45**: 絶対に `&mo` 系にしない（bootloaderアクセス可能になるバグ）
- **pos49（DEL）**: `lt 8 BSPC` から `kp BSPC` を経て現在は `kp DEL`

---

## デザイン方針

- 実機の物理配置に近い見た目を優先する
- 装飾よりも pos番号・レイヤー・tap/hold の確認しやすさを優先する
- 左手・右手・親指キー・エンコーダーの位置関係を崩さない
- キーの大きさ・余白・色分けは視認性を優先する
- 大幅な UI 刷新は行わず、既存 Viewer の構造を維持する

---

## 変更してはいけないもの

- 物理キー数（51）
- pos番号の定義・順序（上記マッピング参照）
- 左右キーの並び順
- 親指段のキー配置（pos40〜50）
- 重要キーの位置: SPACE(pos45), 英数(pos43), かな(pos44), BSPC(pos47), ENTER(pos48)
- エンコーダーの存在と位置（左手側中段インナー）

---

## 更新ルール

1. `config/Cygnus.keymap` を先に更新する
2. Viewer は keymap に合わせて同期する
3. Viewer だけを根拠に keymap を変更しない
4. 修正後は pos番号・キー数・レイヤー内容を確認する（検証の定義参照）

---

## 検証の定義

keymap を正として、Viewer が正しく反映されているかを確認する。

| 確認項目 | keymap側 | Viewer側 | 期待値 |
|---|---|---|---|
| 総キー数 | 各レイヤーのバインド数 | 表示キー数 | 51 |
| pos番号 | keymap 内の pos 定義 | ViewerのposMap定義 | 上記マッピングと一致 |
| 親指段順序 | pos40〜50 の順 | Viewer の配置順 | LCTL〜RCTLの順 |
| エンコーダー | sensor-bindings の記述 | Viewer のencoder表示 | レイヤー別動作と一致 |

**keymap を正として、Viewer が合っているかを確認する。**
Viewer が合っていなければ Viewer を修正する。keymap を修正しない。

---

## 他 AI への依頼時の必須指示文

```
このプロジェクトでは config/Cygnus.keymap を唯一の正典として扱います。
docs/Cygnus-M-viewer.html は正典ではなく、keymap 内容を確認するための HTML Viewer です。

Viewer を修正する場合は docs/viewer-spec.md の方針に従い、
物理配置・pos番号・左右の並び・親指キー配列を勝手に変更しないでください。
見た目の刷新よりも、keymap 正典との整合性を優先してください。

修正後は、keymap と Viewer の pos番号・キー数・レイヤー内容・tap/hold・
encoder / sensor-bindings の整合性を確認してください。

不明点・判断できない変更がある場合は、作業を止めてユーザーに確認してください。
勝手な補完・推測による修正は行わないでください。
```
