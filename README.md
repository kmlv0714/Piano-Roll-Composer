# Browser Piano Roll Composer

ブラウザ上で動作する、ステップベースの作曲ツールです。  
メロディ入力、コードレーン編集、フレーズストック、メロディ自動生成、コード自動生成、JSON保存/読込、MIDI入力、マイクピッチ補助、PWA対応を備えています。

この README は `index.html` の現行実装に合わせて記載しています。

## 1. 起動方法

1. `index.html` をブラウザで開く。
2. 音を出す前に `Audio Unlock` を押す。
3. 必要に応じて `Play` / `Stop` で再生。

注意:
- Service Worker を有効に使う場合は `file://` ではなくローカルサーバー配信が推奨です。
- Tone.js は CDN (`https://unpkg.com/tone@14.8.49/build/Tone.js`) を参照しています。

## 2. 画面構成（全要素）

### 2.1 上部固定トランスポート

- `Audio Unlock`: WebAudioのユーザー操作アンロック
- `Play`: 再生開始
- `Stop`: 停止 + 先頭へ戻る
- `Tempo`: 60〜240 BPM
- `Master`: マスターボリューム（0〜100%）

### 2.2 タブ群

- `Input`
- `Sound`
- `Scale`
- `Chord Gen`
- `Random`
- `Edit/View`

### 2.3 右上ユーティリティ

- `Save JSON`: 曲データ保存
- `Load JSON`: 曲データ読み込み

### 2.4 左サイド: Phrase Stocker

- `Stock`: 現在のコピー内容（ノート）をフレーズ登録
- `Copy`: 選択フレーズをアプリ内クリップボードへコピー
- `Delete`: 選択フレーズ削除（確認あり）
- `Name`: フレーズ名編集
- `Save Stocker` / `Load Stocker`: フレーズライブラリ単体保存/読込
- フレーズ一覧クリック: 選択 + プレビュー再生

### 2.5 中央: Piano Roll 本体

- 上段 `Melody`:
  - ルーラー
  - ピッチサイドバー
  - メロディキャンバス
- 下段 `Chords`:
  - コードレーンキャンバス

### 2.6 下部

- `Operations`（操作ヒント）
- `Status`（状態メッセージ）

### 2.7 モーダル

- `Chord Picker`（コード長押しで表示）
  - Root選択
  - Quality選択
  - Scale fit表示
  - `Apply` / `Close`

## 3. タブごとの機能

## 3.1 Input

- `MIDI: ON/OFF`
- `MIDI In` デバイス選択
- `Pitch: ON/OFF`（マイク入力ピッチ補助）
- `Thresh`（Pitch ON時のみ表示）
- `Step Input: ON/OFF`

## 3.2 Sound

- `Melody` 音色:
  - Saw Lead
  - Square Lead
  - Triangle Soft
  - Sine Pure
  - FM Bell
  - AM Soft
  - Mono Bass/Lead
- `Drums: ON/OFF`
- `Chords: ON/OFF`

## 3.3 Scale

- `Scale Root`（C〜B）
- `Scale Type`:
  - Major
  - Natural Minor
  - Harmonic Minor
  - Phrygian Dominant
  - Dorian
  - Mixolydian
- `Retune to Scale`
- `AutoRetune: ON/OFF`
- `Assist: ON/OFF`
- `Chord Tones: ON/OFF`

## 3.4 Chord Gen（コード自動生成）

入力:
- `Key`
- `Mode`（Major / Natural Minor）
- `Bars`（totalSteps = bars * 16）
- `Steps`（表示のみ）
- `Chord Dur`（ハーモニックリズム）
- `Cadence`（Perfect / Plagal / Half / Deceptive）
- `Complexity`（Triad / 7th）

操作:
- `Generate`: ドラフト進行を生成
- `Preview`: ドラフトを現テンポでプレビュー
- `Apply`: ドラフトをコードレーンへ確定（Undo 1操作）
- `Reroll`: 直前条件で再生成

補足:
- 生成結果はコードレーン上に `draft` スタイルで仮表示されます。
- `Apply` 後に実コードデータへ反映されます。

## 3.5 Random（メロディ自動生成）

- `Randomize Sel Pitch`: 選択ノートの高さ再配置
- `Rhythm`: リズムテンプレ選択
- `Rand Pos`: 生成位置（Selection / Cursor+Bars / Whole Song）
- `Rand Bars`: Cursorモード時の長さ
- `Density`: ノート密度
- `Dur Rand`: ノート長のランダム性
- `Pitch Span`: 生成音域幅（中心音まわり）
- `Intvl Near / Mid / Far`: 音程距離バケット重み
- `Chord W`: コードトーン優先重み
- `Step W`: 近接進行優先重み
- `Leap Back W`: 跳躍後の反行補正重み
- `Center W`: 重心への引力重み
- `Repeat W`: 反復（モチーフ再利用）重み
- `End W`: 終止安定（Root/3rd/5th）重み
- `Generate Phrase`: ルールベースでノート構造を生成

## 3.6 Edit/View

- `Bars`: 小節数（1〜128）
- `Snap`: グリッド量子化
- `Zoom (px/16th)`: 横倍率
- `Pitch Range`: Cオクターブ下限/上限
- `Undo` / `Redo`
- `Copy` / `Paste`
- `Follow: ON/OFF`

## 4. マウス/キーボード操作

## 4.1 共通ショートカット

- `Space`: Play / Stop トグル（入力欄フォーカス時は無効）
- `Ctrl/Cmd + Z`: Undo
- `Ctrl/Cmd + Y` または `Ctrl/Cmd + Shift + Z`: Redo
- `Ctrl/Cmd + C`: コピー
- `Ctrl/Cmd + V`: 貼り付け
- `Delete` / `Backspace`: 選択削除（ノート/コード）

## 4.2 メロディレーン

- 左クリック空白: ノート追加
- 左ドラッグノート本体: 移動
- 左ドラッグノート右端: 長さ変更
- `Shift + ドラッグ(空白)`: 矩形選択
- `Shift + クリック`: 複数選択の追加/解除
- ホイール（ノート上）: ピッチ上下
- ホイール（空白）: 表示音域スクロール
- 右クリックノート: 削除
- 空白ドラッグ: 横パン

## 4.3 ルーラー

- クリック: カーソル移動
- ドラッグ: 横スクロール

## 4.4 ピッチサイド

- クリック: その音を短く試聴 + ハイライト
- ホイール: 表示音域スクロール

## 4.5 コードレーン

- 左クリック空白: コード追加（既定 `Em`）
- 左ドラッグコード本体: 移動
- 左ドラッグコード右端: 長さ変更
- `Shift + クリック`: 複数選択
- 左クリックコード: 試聴
- 長押しコード: Chord Picker 表示
- ホイール（コード上）: コード種類を循環変更
- 右クリック/ダブルクリック: 削除

## 4.6 Step Input モード

- 停止時のみ利用可能
- MIDIノート押下で入力開始、カーソル前進でノート長が伸長
- `ArrowLeft / ArrowRight`: ステップカーソル移動
- ON解除またはノートオフで確定

## 5. 自動生成ロジック概要

## 5.1 メロディ自動生成（Generate Phrase）

内部処理:
1. 対象範囲決定（Selection/Cursor/Song）
2. リズムテンプレートから候補イベント生成
3. `Density` と `Dur Rand` で採用率/長さを調整
4. スケール制約内で候補ピッチを列挙
5. `Chord W / Step W / Leap Back W / Center W / Repeat W / End W` で重み評価
6. `Pitch Span` で中心音周辺へ制限
7. 最終音を安定音へ補正

## 5.2 コード自動生成（Chord Gen）

内部処理:
1. `totalSteps` を `chordDur` で分割（余りは最後へ加算）
2. T/S/D 機能遷移で機能列生成
3. 機能別重みで度数選択
4. Mode別ダイアトニックコードへ変換（Triad/7th）
5. Cadenceテンプレートで末尾2コードを強制
6. 複数候補（既定20回）を採点して最大スコア採用

採点の主軸:
- Cadence一致
- T→S→D→T の流れ
- 同一コード連打ペナルティ
- 度数偏りの抑制

## 6. 内部データ構造

## 6.1 曲本体

- `notes`: `{ step, dur, midi, vel }[]`
- `chords`: `{ start, dur, chord }[]`
- `TOTAL_BARS`, `TOTAL_STEPS`

## 6.2 クリップボード

- `clipboard`: `{ minStep, notes, chords }`  
  コピー時は選択範囲の先頭を基準に正規化

## 6.3 Phrase Stocker

- `phraseStore`: `{ version, phrases[] }`
- `phrase`: `{ id, name, notes[], lengthSteps, createdAt }`

## 6.4 Chord Gen ドラフト

- `chordGenState.draft`:  
  `{ startStep, durSteps, chordSymbol, degree, func, label }[]`

## 7. 保存形式（JSON）

## 7.1 曲データ

- `format: "piano-roll-song"`
- `version: 7`
- 主要フィールド:
  - `transport`（bpm, bars, stepsPerBar）
  - `scale`
  - `pitchRange`
  - `notes`
  - `chords`
  - `ui`（表示設定、randomize設定、chordGen設定 など）

`ui.randomize` には以下が保存されます:
- `posMode`, `rhythmTemplate`, `barsSpan`, `density`, `durVar`, `pitchSpan`
- `intervalWeights`（near/mid/far）
- `ruleWeights`（chordTone/stepwise/leapRecover/centerPull/repeat/endStable）

`ui.chordGen` には以下が保存されます:
- `key`, `mode`, `bars`, `chordDurSteps`, `cadence`, `complexity`

## 7.2 Phrase Stocker 保存

- `format: "prc-phrase-stocker"`
- `version: 1`
- `phrases[]`（id, name, notes, lengthSteps, createdAt）

## 8. 音源・再生エンジン

- 再生は **単一スケジューラ**（`Tone.Transport.scheduleRepeat(..., "16n")`）
- メロディ/コードは `PolySynth` を再利用
- `maxPolyphony`:
  - melody: 8
  - chord: 12
- 遅延時の保護:
  - 最低先行スケジュール
  - 過度に遅れたステップのドロップ
- 背景タブでの自動一時停止/復帰ロジックあり

## 9. デバッグ/診断

- `window.__prcDebug` 経由の診断ログ
- `localStorage.prcDebugMem`:
  - `"1"` で有効
  - `"0"` で無効
- クエリ:
  - `?debugmem=1`
  - `?debugmem=0`

出力例:
- heap使用量
- UI/render/pitchループ頻度
- transport step頻度
- late/clamp/drop カウント
- drum rebuild カウント

## 10. PWA / オフライン

- `manifest.json` あり
- `sw.js` を登録
- キャッシュ戦略:
  - インストール時に `./` をキャッシュ
  - GETリクエストは cache-first

## 11. 既知の仕様メモ

- コードレーン確定データは内部的に `{start, dur, chord}` を使用
- Chord Gen のドラフトは `{startStep, durSteps, chordSymbol}` 系を使用
- Snap設定により編集/生成の量子化単位が変わる

## 12. 変更履歴管理のおすすめ

- 大機能追加後は `Save JSON` でスナップショットを残す
- Phrase Stocker も別ファイル保存しておくと再利用しやすい


## 13. クイックスタート（スクリーンショット付き）

このセクションは、初見ユーザー向けの最短手順です。  
画像ファイルは `assets/readme/` に配置する想定です（必要に応じてパス変更してください）。

### 13.1 5分で鳴らす手順

1. `index.html` を開く  
   ![Step 1: 起動画面](assets/readme/01_open.png)

2. `Audio Unlock` を押す  
   ![Step 2: Audio Unlock](assets/readme/02_audio_unlock.png)

3. `Play` を押して既定データを再生する  
   ![Step 3: Play](assets/readme/03_play.png)

4. メロディレーンの空白をクリックしてノートを追加する  
   ![Step 4: Add Note](assets/readme/04_add_note.png)

5. コードレーンをクリックしてコードを追加する  
   ![Step 5: Add Chord](assets/readme/05_add_chord.png)

6. `Random` タブで `Generate Phrase` を実行する  
   ![Step 6: Generate Phrase](assets/readme/06_generate_phrase.png)

7. `Chord Gen` タブで `Generate -> Preview -> Apply` を実行する  
   ![Step 7: Chord Gen](assets/readme/07_chord_gen.png)

8. `Save JSON` で保存する  
   ![Step 8: Save JSON](assets/readme/08_save_json.png)

### 13.2 スクリーンショット撮影ガイド

- 推奨解像度: `1920x1080`（または `1600x900`）
- 1枚目は全体、以降は対象UIが分かる範囲をトリミング
- 再生中UIを撮る場合は `pos` と再生ハイライトが見える状態にする
- ファイル名は上記の `01_...08_...` に合わせる

### 13.3 画像が未配置でもREADMEを読む場合

画像がない場合でも、手順本文だけで操作は可能です。  
後から `assets/readme/*.png` を置けば、そのまま表示されます。
