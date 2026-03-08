# Browser Piano Roll Composer

ブラウザ上で動く、メロディ作成中心のピアノロール作曲ツールです。  
`index.html` 単体で完結しつつ、コードレーン、フレーズ生成、コード自動生成、Phrase Stocker、スマホ向けレイアウト、PWA 対応まで含んでいます。

この README は現在の `index.html` 実装に合わせて更新しています。  
スクリーンショット説明は含めていません。

## 動作環境

- Chromium 系ブラウザ推奨
- WebAudio / WebMIDI / MediaDevices が使える環境
- HTTPS またはローカルサーバー経由での起動を推奨

注意:
- `file://` 直開きでも一部は動きますが、PWA や一部ブラウザ機能は不安定です
- `Pitch` 入力はマイク許可が必要です
- `MIDI` 入力は Web MIDI 対応ブラウザが必要です

## 起動方法

1. `index.html` をブラウザで開く
2. 最初に `Audio Unlock` を押して音声を有効化する
3. `Play` または `Play From Cursor` で再生する

## 画面構成

### 上部トランスポート

- `Audio Unlock`
- `Play`
- `Play From Cursor`
- `Stop`
- `|< Start`
- `End >|`
- `Tempo`
- `Master`
- `Save JSON`
- `Load JSON`
- `Operations`

### タブ

- `Input`
- `Sound`
- `Scale`
- `Chord Gen`
- `Random`
- `Edit/View`

### 中央編集エリア

- Melody Piano Roll
- Pitch Sidebar
- Ruler
- Chord Lane
- Chord Picker

### 補助パネル

- Phrase Stocker
- Status
- Operations
- マスコット表示

### スマホ時

スマホでは PC レイアウトをそのまま縮小せず、専用のタッチ UI に切り替わります。

- `Melody`
- `Chords`
- `Phrase`
- `Settings`

をメインナビゲーションとして切り替えます。

## 主な機能

## 1. トランスポート

- `Play`: 先頭または現在の Transport 状態から再生
- `Play From Cursor`: 現在カーソル位置から再生
- `Stop`: 停止
- `|< Start`: カーソルを先頭へ移動
- `End >|`: カーソルを末尾へ移動
- `Tempo`: 60-240 BPM
- `Master`: 全体音量

## 2. Input

- `MIDI: ON/OFF`
- `MIDI In`: 入力デバイス選択
- `Pitch: ON/OFF`
- `Thresh`: Pitch 入力しきい値
- `Step Input: ON/OFF`

## 3. Sound

### Melody 音色

- Saw Lead
- Square Lead
- Triangle Soft
- Sine Pure
- FM Bell
- AM Soft
- Mono Bass/Lead
- Rock Drive Lead
- Rock Power Lead
- Rock Bass

### Chord 音色

- Saw Pad
- Warm Triangle
- Soft Sine
- Square Organ
- FM Glass

### 音量・トグル

- `Chord Vol`
- `Drum Vol`
- `Drums: ON/OFF`
- `Chords: ON/OFF`

## 4. Scale / Assist

### ルート

- C
- C#
- D
- D#
- E
- F
- F#
- G
- G#
- A
- A#
- B

### スケール

- Major
- Natural Minor
- Harmonic Minor
- Phrygian Dominant (Arabian)
- Dorian
- Mixolydian

### 補助機能

- `Retune to Scale`
- `AutoRetune: ON/OFF`
- `Assist: ON/OFF`
- `Chord Tones: ON/OFF`

## 5. Chord Lane / Chord Picker

コードレーンではコードブロックを配置、移動、リサイズ、削除できます。  
Chord Picker からルートと品質を分けて選択できます。

### 対応コード品質

- Major
- Minor (`m`)
- Dominant 7th (`7`)
- Major 7th (`maj7`)
- Power Chord (`5`)
- Minor 7th (`m7`)
- Suspended 4th (`sus4`)
- Diminished (`dim`)
- Half-diminished (`m7b5`)
- Augmented (`aug`)

### Chord Picker の特徴

- 12 ルート選択
- 品質別ボタン選択
- スケール適合表示
- 選択中コードの変更に使用
- 長押しやボタン経由で開ける

## 6. Chord Gen

ダイアトニック機能和声ベースでコード進行ドラフトを生成します。

### 設定

- `Key`
- `Mode`: Major / Natural Minor
- `Total Bars`
- `Steps`
- `Chord Dur (Bars)`
- `Chord Dur Steps`
- `Cadence`
  - Perfect
  - Plagal
  - Half
  - Deceptive
- `Complexity`
  - Triad
  - 7th

### 操作

- `Generate`: ドラフト生成
- `Preview`: ドラフト試聴
- `Apply`: コードレーンへ反映
- `Reroll`: 再生成

### 生成の考え方

- T / S / D の機能遷移を使う
- ダイアトニックコードへ変換する
- 最後の 2 コードは Cadence に合わせて補正する
- 複数候補を採点して最良案を採用する

## 7. Random / Generate Phrase

### `Randomize Sel Pitch`

選択ノートの高さだけを再生成します。

- ノート数は変えません
- ノート位置は変えません
- ノート長は変えません
- `Generate Phrase` で使う重みを参照します

反映される主な重み:

- `Pitch Span`
- `Intvl Near / Mid / Far`
- `Chord W`
- `Step W`
- `Leap Back W`
- `Center W`
- `Repeat W`
- `End W`

### `Generate Phrase`

選択範囲、カーソル位置、または曲全体に対して、メロディを生成します。

### 設定

- `Rhythm`
  - Auto
  - Straight 8th
  - Straight 16th
  - Syncopated A
  - Syncopated B
  - Sparse
- `Rand Pos`
  - Selection
  - Cursor + Bars
  - Whole Song
- `Rand Bars`
- `Density`
- `Dur Rand`
- `Pitch Span`
- `Intvl Near / Mid / Far`
- `Chord W`
- `Step W`
- `Leap Back W`
- `Center W`
- `Repeat W`
- `End W`

### 生成ルール

- 基本はスケール内
- コードトーン優先
- 小さい音程移動を優先
- 大きい跳躍後は戻りやすくする
- 中心音から離れすぎない
- リズムテンプレートを使用
- 一部反復を使う
- 最後は安定音へ寄せる

## 8. Edit / View

- `Bars`: 曲全体の長さ
- `Snap`: 編集単位
- `Zoom (px/16th)`: 横ズーム
- `Pitch Row Height`: 1 音階あたりの縦幅
- `Pitch Range`: 表示音域
- `Undo`
- `Redo`
- `Copy`
- `Paste`
- `Clear Melody`
- `Follow: ON/OFF`

## 9. Phrase Stocker

フレーズを曲データとは別に保存して再利用できます。

### 機能

- `Stock`: 現在のコピー内容をフレーズ化
- フレーズ一覧クリック: 選択 + プレビュー再生
- `Copy`: フレーズをアプリ内クリップボードにコピー
- `Delete`: フレーズ削除
- `Name`: 名前変更
- `Save Stocker`
- `Load Stocker`

### データ構造

`Phrase Stocker` は JSON で保存されます。

```json
{
  "format": "prc-phrase-stocker",
  "version": 1,
  "phrases": [
    {
      "id": "phr_xxx",
      "name": "Phrase 1",
      "notes": [
        { "step": 0, "dur": 4, "midi": 60, "vel": 1 }
      ],
      "lengthSteps": 16,
      "createdAt": "2026-03-09T00:00:00.000Z"
    }
  ]
}
```

## 操作方法

## 1. Piano Roll

### PC

- 空白クリック: カーソル移動
- 空白ドラッグ: 横パン
- ノートクリック: 選択
- ノートドラッグ: 移動
- ノート端ドラッグ: 長さ変更
- `Shift + 空白ドラッグ`: 範囲選択
- `Shift + クリック`: 複数選択
- ノート上ホイール: ピッチ上下
- 画面端までドラッグすると自動スクロール

### スマホ

- タップ: 選択
- 空白タップまたはモード操作: 入力
- ドラッグ: 移動
- リサイズハンドル: 長さ変更
- Box モード: 範囲選択
- 下部ドック: ズーム、行高、削除、コピーなど

## 2. Chord Lane

### PC

- クリック: コード選択
- ドラッグ: 移動
- 端ドラッグ: 長さ変更
- 長押し / ボタン: Chord Picker

### スマホ

- タップ: コード選択
- ドラッグ: 移動
- リサイズハンドル: 長さ変更
- Chord 用インスペクタから変更・削除・試聴

## 3. キーボードショートカット

- `Space`: Play / Stop
- `Ctrl/Cmd + Z`: Undo
- `Ctrl/Cmd + Y`
- `Ctrl/Cmd + Shift + Z`: Redo
- `Ctrl/Cmd + C`: Copy
- `Ctrl/Cmd + V`: Paste
- `Delete` / `Backspace`: 削除
- `ArrowLeft / ArrowRight`: カーソル移動

## 保存形式

## 1. 曲データ JSON

保存ボタンから出力される曲データは、概ね以下の情報を含みます。

- transport
- scale
- pitch range
- notes
- chords
- UI 設定
- randomize 設定
- chord gen 設定
- 音量設定

例:

```json
{
  "format": "piano-roll-song",
  "version": 7
}
```

## 2. Phrase Stocker JSON

- `format: "prc-phrase-stocker"`
- `version: 1`

## 実装メモ

- 再生は単一スケジューラ方針で動作
- Melody / Chords は `PolySynth` ベース
- 大量の `Tone.Part` を増やす設計は避けている
- 長時間再生で重くなりにくいよう調整済み
- スマホでは専用レイアウトとタッチ操作へ切り替える

## PWA

以下を含みます。

- `manifest.json`
- `sw.js`
- `icon-192.png`
- `icon-512.png`

インストール済み環境では、ホーム画面から起動できます。

## デバッグ

メモリやスケジューラ状況を見るためのデバッグログがあります。

- `localStorage.prcDebugMem = "1"` で有効
- `localStorage.prcDebugMem = "0"` で無効
- `?debugmem=1`
- `?debugmem=0`

主な監視項目:

- heap
- UI / render 更新頻度
- transport step 頻度
- late / clamp / drop
- drum rebuild

## ファイル構成

- `index.html`
- `manifest.json`
- `sw.js`
- `assets/`

## 補足

- README はユーザー向け説明を優先しています
- 実装の詳細は `index.html` 内コメントと関数名を参照してください
