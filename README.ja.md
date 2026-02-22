<p align="right">
  <a href="README.md">English</a> &nbsp;·&nbsp; <a href="README.ko.md">한국어</a>
</p>

<div align="center">

# MySonar

リアルタイム字幕対応のデスクトップ音楽プレイヤー。

<br>

![Windows](https://img.shields.io/badge/Windows-Release-0078D4?style=flat-square)&nbsp;&nbsp;![macOS](https://img.shields.io/badge/macOS-QA_Testing-lightgrey?style=flat-square)

<br>

**v1.0.1** &nbsp;·&nbsp; リリース日 2026. 02. 22 &nbsp;·&nbsp; 開発 Hustlyn

<br>

<img src="preview/icon.png" width="72">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="preview/snrpack_icon.png" width="72">

</div>

<br>

<video src="preview/demo.mp4" controls width="100%"></video>

![MySonar](preview/00-page/00-track.png)

---

## 対応フォーマット

| 種類 | フォーマット |
|---|---|
| オーディオ | WAV · FLAC · MP3 · OGG · OPUS · AAC · M4A · AIFF · WebM |
| 字幕 | SRT · VTT · ASS / SSA · SMI · LRC · TXT |

---

## 字幕ファイルの命名ルール

字幕ファイルはオーディオファイルと同じフォルダに置くと自動的に認識されます。

### RAW（言語なし）

| パターン | 例（オーディオ: `rain.flac`） |
|---|---|
| `<ファイル名>.<字幕拡張子>` | `rain.srt` |
| `<ファイル名>.<オーディオ拡張子>.<字幕拡張子>` | `rain.flac.srt` |

RAW 字幕は字幕セレクターに **RAW** と表示されます。

### 言語コード付き

| パターン | 例（オーディオ: `rain.flac`） |
|---|---|
| `<ファイル名>.<言語>.<字幕拡張子>` | `rain.en.srt` · `rain.ja.vtt` |
| `<ファイル名>.<オーディオ拡張子>.<言語>.<字幕拡張子>` | `rain.flac.ko.srt` |

`<言語>` は小文字 2〜3 文字の ISO 639-1 コードを使用してください（例: `en`, `ko`, `ja`, `fra`）。

**例 — `rain.flac` のフォルダ構成:**

```
rain.flac
rain.srt            ← RAW
rain.en.srt         ← 英語
rain.ja.vtt         ← 日本語（別フォーマット）
rain.ko.ass         ← 韓国語
rain.flac.zh.srt    ← 中国語（フルファイル名形式）
```

1つのトラックに複数の言語とフォーマットを同時に使用できます。同じ言語に複数の字幕ファイルがある場合、言語セレクターの隣にフォーマットセレクター（SRT / VTT / …）が表示されます。

---

## パッケージフォーマット (.snrpack)

`.snrpack` ファイルは、コレクション全体（オーディオ・字幕・カバー画像・メタデータ）を1つのファイルにまとめたものです。共有やバックアップに使用します。内部は ZIP アーカイブ形式です。

**内部構造:**

```
collection.snrpack  (ZIP アーカイブ)
├── manifest.json        フォーマットバージョンとアプリ識別情報
├── collection.json      名前・作者・説明・タグ・トラックリスト・グループ
├── audio/               オーディオファイル（シーク対応のため非圧縮で保存）
│   ├── 0/  song.flac
│   └── 1/  ...  （ファイル名が重複する場合はインデックスを増加）
├── images/              カバー画像（圧縮保存）
│   └── 0/  cover.jpg
└── subtitles/           字幕ファイル（オーディオとインデックス対応・圧縮保存）
    └── 0/  song.en.srt
             song.ja.vtt
```

`collection.json` にはコレクション名・作者・説明・タグ・カバー画像パス・トラックリストが格納されます。各トラックにはオーディオのメタデータ（タイトル・アーティスト・アルバム・再生時間・フォーマット・ビットレートなど）・評価・タグ・言語コード付きの字幕ファイル一覧が含まれます。トラックグループと順序も保存されます。

**エクスポート:** コレクションを右クリック → **パッケージをエクスポート**
**インポート:** `.snrpack` ファイルをアプリウィンドウにドラッグ、またはコレクションを右クリック → **パッケージをインポート**

`.snrpack` ファイルはカスタムアイコン付きでシステムに登録されます。ファイルをダブルクリックするか、**プログラムから開く → MySonar** を使用すると直接開けます。

<table>
  <tr>
    <td width="50%" align="center"><img src="preview/03-packing/30_snrpack.png" width="100%"><br>エクスプローラーでの .snrpack ファイル</td>
    <td width="50%" align="center"><img src="preview/03-packing/31_snrpack-open.png" width="100%"><br>ダブルクリックまたは「プログラムから開く」で直接起動</td>
  </tr>
</table>

---

## 機能

### 再生
- リアルタイムアナライザーを重ねた波形シークバー
- トラック間クロスフェード（時間調整可能）
- ファイルごとに最後の再生位置を記憶

### オーディオ処理
- 10バンドイコライザー（32 Hz – 16 kHz）— シンプル / プロモード、プリセット内蔵
- ダイナミクスコンプレッサーおよびベースブースト
- 音量ノーマライゼーション（ReplayGain）(BETA)
- 空間オーディオ — HRTF バイノーラルポジショニング（X / Y / Z 軸）
- モノラルテストトーンを使った左右の聴力バランス補正

### 字幕
- オーバーレイ表示 — フォント・サイズ・色・アウトライン・影・位置を調整可能
- 現在のキューをハイライト表示し自動スクロールするスクリプトパネル
- インラインカラータグのレンダリング（SRT / VTT / SMI）
- 前後の字幕キューへのジャンプ

### プレイリストとコレクション
- ファイルやフォルダをウィンドウのどこにでもドラッグ＆ドロップ
- 複数選択：Ctrl+クリック · Shift+クリック · マウスドラッグ
- プレイリスト内の折りたたみ可能なトラックグループ、ドラッグで並び替え可能
- 名前・再生時間・ファイルサイズで並び替え（自然数順）
- コレクション — `.msc` ファイルとして保存される名前付きプレイリスト
  - コレクションごとに複数のカバー画像、ドラッグで並び替え可能
  - 評価（0〜5 星）およびカスタムタグ
  - 1 ページあたりのトラック数：5 / 10 / 20
- タグフィルターと最低評価フィルターを使った検索
- 全メタデータを確認できるトラック情報モーダル

### 外観
- 7 種類のテーマ：Dark · Light · Dark Rose · Light Rose · Light Marine · Dark Marine · Pink
- 角丸の透過ウィンドウ
- アルバムアート：埋め込みメタデータ・同名画像ファイル・ドラッグ＆ドロップで上書き
- キー操作時に画面に表示されるアクションオーバーレイ
- ステータスバー — リアルタイム再生情報

---

## スクリーンショット

<table>
  <tr>
    <td align="center"><img src="preview/00-page/00-track.png"><br>トラック画面</td>
    <td align="center"><img src="preview/00-page/01-collection.png"><br>コレクション</td>
    <td align="center"><img src="preview/00-page/02-edit_collection.png"><br>コレクション編集</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/00-page/03-import-srnpack.png"><br>パッケージのインポート</td>
    <td align="center"><img src="preview/00-page/04-eq.png"><br>イコライザー</td>
    <td align="center"><img src="preview/00-page/05-password.png"><br>パスワードロック</td>
  </tr>
</table>

### テーマ

<table>
  <tr>
    <td align="center"><img src="preview/02-theme/20_dark.png"><br>Dark</td>
    <td align="center"><img src="preview/02-theme/21_light.png"><br>Light</td>
    <td align="center"><img src="preview/02-theme/22_dark-rose.png"><br>Dark Rose</td>
    <td align="center"><img src="preview/02-theme/23_light-rose.png"><br>Light Rose</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/02-theme/24_dark-marine.png"><br>Light Marine</td>
    <td align="center"><img src="preview/02-theme/25_dark-marine.png"><br>Dark Marine</td>
    <td align="center"><img src="preview/02-theme/26_pink.png"><br>Pink</td>
    <td></td>
  </tr>
</table>

### 設定

<table>
  <tr>
    <td align="center"><img src="preview/01-settings/10_setting.png"><br>一般</td>
    <td align="center"><img src="preview/01-settings/11_setting.png"><br>オーディオ</td>
    <td align="center"><img src="preview/01-settings/12_setting.png"><br>字幕</td>
    <td align="center"><img src="preview/01-settings/13_setting.png"><br>コントロール</td>
  </tr>
  <tr>
    <td align="center"><img src="preview/01-settings/14_setting.png"><br>タグ</td>
    <td align="center"><img src="preview/01-settings/15_setting.png"><br>言語</td>
    <td align="center"><img src="preview/01-settings/16_setting.png"><br>セキュリティ</td>
    <td align="center"><img src="preview/01-settings/17_setting.png"><br>About</td>
  </tr>
</table>

---

## キーボードショートカット

| キー | 動作 |
|---|---|
| `Space` | 再生 / 一時停止 |
| `←` / `→` | シーク ±1 秒 |
| `Shift` + `←` / `→` | シーク ±5 秒 |
| `Ctrl` + `←` / `→` | シーク ±0.2 秒 |
| `↑` / `↓` | 音量 ±5% |
| `Z` / `X` | 前 / 次の字幕行 |
| `Alt` + `←` / `→` | 前 / 次の字幕キューへジャンプ |
| `[` / `]` | 字幕フォントサイズ −1 / +1 px |

`Ctrl+A`（全選択）を除くすべてのショートカットは **設定 → コントロール** で変更できます。
シーク量（±1 秒 / ±5 秒 / ±0.2 秒）もカテゴリごとに調整できます。

---

## 多言語対応

標準搭載：**英語 · 韓国語 · 日本語**

他の言語を追加するには、[sample_local.json](sample_local.json) をテンプレートとして使用してください。
ファイル名を `<言語コード>.json`（例: `fr.json`）に変更し、内容を翻訳したうえで以下のフォルダに置いてください：

```
<実行ファイルのフォルダ>/locales/<言語コード>.json
```

同じ言語コードのファイルがある場合、ユーザーファイルが標準ファイルより優先されます。

---

## ロードマップ

| | |
|---|---|
| Mac ビルドと配布 | 署名済み macOS リリースのビルドと配布 |
| 字幕の自動生成 | Whisper モデルを使ったオーディオからの VTT 字幕自動生成 |
| カスタムテーマ | JSON ファイルまたはアプリ内エディターによるユーザー定義テーマ |
| マニュアルとドキュメント | アプリ内ヘルプおよびオンラインドキュメントの整備 |

---

## リリースノート

<details>
<summary><strong>v1.0.1</strong> &nbsp;— 2026. 02. 22</summary>

<br>

**バグ修正**

- **ネットワークパス（UNC）の再生不具合を修正** — `\\server\share\...` 形式の UNC パス（SMB 共有・NAS など）にあるオーディオファイルが再生できなかった問題を修正しました。内部の URL 変換処理が UNC パスを正しく扱えておらず、該当ファイルが読み込まれない状態でした。
- **ネットワーク上の snrpack 内画像を修正** — ネットワーク共有に保存された `.snrpack` パッケージ内の画像が正しく表示されない問題を修正しました。
- **snrpack エクスポート時のカバー画像の同梱** — `.snrpack` パッケージ作成時に、音声ファイルと同名の画像ファイル（例：`music.flac` と同じフォルダの `music.png`）が自動で検出され、パッケージに含まれるようになりました。
- **並び順の設定を保持** — プレイリストで設定した並び替えの項目と方向がアプリ再起動後も維持されるようになりました。

</details>
