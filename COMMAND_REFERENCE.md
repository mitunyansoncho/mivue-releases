# MIvue コマンドリファレンス（検証済みの核）

MIvue（ミービュー）0.3.1 のコマンドライン版で使うコマンドの要点です。ここに載せた書式は、実装で動作を確認したものです。エンジン全体の検証範囲（どの推定・検定が公表値と照合済みか）は検証報告書が正本で、この文書はその中から日常の分析で使う核を抜き出したものです。

出力は EViews 本体の出力ではなく、互換エンジンによる再現値です。

## CLI の使い方

```
mivue --version                 版数を表示（例: MIvue 0.3.1）
mivue --help                    使い方を表示
mivue -c "コマンド"             コマンドを1つ実行
mivue program.prg               .prg プログラムを実行
mivue -c "コマンド" --json      結果を JSON で出力
mivue program.prg -o out        .prg の書き出し先フォルダを指定
```

- 終了コードは成功が 0、失敗が 1。
- 実行時のカレントディレクトリが作業フォルダになり、相対パスはそこから解決します。.prg の入力ファイルは、プログラムのあるフォルダとカレントディレクトリの両方から探します。
- `--json` の結果は `success`、`executed`、`outputs`、`warnings`、`errors`、`data` のキーを持つ JSON です。まず `success` を確認してください。
- 同じ入力への実行結果は決定的で、繰り返し実行しても出力は一致します。

## 起動中の GUI へ接続する（--live）

GUI 版が起動している間、そのセッションへ接続できます。接続先で使えるコマンドは通常の実行と同じです。

```
mivue --live state              開いているワークファイル・ページ・オブジェクト・選択を JSON で表示
mivue --live -c "コマンド"      開いているセッションでコマンドを実行（--json も併用可）
mivue --live history            エージェントが実行した操作の一覧を JSON で表示
mivue --live undo               直近のエージェント操作を取り消す（操作前の状態へ戻す）
```

- 接続は同一マシン内だけです。GUI が起動していない、または GUI 側の設定でライブ連携が無効なときは、エラーを表示して終了コード 1 で終わります。
- エージェントの操作は GUI 側の記録に「エージェント」の出所つきで残り、変更は画面へ自動で反映されます。

## ワークファイルとデータ

### wfcreate — ワークファイルを作る

```
wfcreate u 1000                    日付なし（観測数）
wfcreate a 2000 2020               年次
wfcreate q 2000q1 2005q4           四半期
wfcreate m 2020m01 2024m12         月次
wfcreate d 2024-01-01 2024-12-31   日次（ISO 形式）
```

### wfopen — ファイルを開く

```
wfopen sales.csv
wfopen data.xlsx
wfopen saved.evwf
wfopen legacy.wf1
```

CSV、Excel（.xlsx）、MIvue のワークファイル（.evwf）、EViews のワークファイル（.wf1）が開けます。CSV に date／year／quarter／month 列が無い場合は、日付なしの連番として読み込み、その旨が出力されます。

### wfsave — ワークファイルを保存する

```
wfsave "result.evwf"
```

### series / genr — 系列の作成と計算

```
series s1
genr xs = @trend
genr ys = 2*xs + 3
genr lny = log(ys)
```

`genr` は式の結果で系列を作ります。`@trend` は 0 から始まる連番です。関数と演算は EViews の式の作法に従います。

### sample / smpl — 標本の設定

```
sample s1 1 6
smpl s1
smpl @all
```

名前付き標本を定義し、`smpl` で有効化します。以後の推定はその標本で実行されます。

## 推定

### equation — 推定式

```
equation eq1.ls sales c ad
show eq1
```

`equation 名.ls 被説明変数 説明変数...` が最小二乗法です。定数項は `c` で置きます。推定すると結果表が得られ、`show` で再表示できます。

```
Dependent Variable: SALES
Method: Least Squares
Sample: 1 12
Included observations: 12
...
C                         44.37931      9.632271      4.607357    0.0010
AD                        7.892573      0.621114      12.70713    0.0000
R-squared                 0.941681
```

最小二乗法以外の推定・検定（操作変数法、ARCH／GARCH、単位根検定、共和分など）も検証済みの範囲で備えます。手法指定と出力の詳細は検証報告書と仕様の範囲に従ってください。

### forecast / fit — 予測と予測の標準誤差

```
eq1.forecast yf            動的予測（ラグ付き従属変数には予測値を再帰して使う）
eq1.forecast yf yfse       予測の標準誤差も系列 yfse へ出力する
eq1.fit yf yfse            静的予測（実績値による当てはめ）と標準誤差
eq1.forecast(e) yf yfse    予測評価表（RMSE・MAE・MAPE・Theil の不平等係数と分解）を表示する
eq1.forecast(g) yf yfse    ±2 標準誤差の帯つきグラフを出力する
```

標準誤差は EViews 公式文書（User's Guide 第25章）の定義に基づきます。標準誤差を計算できない推計法では、標準誤差なしで予測だけを出し、その旨を表示します。

## 表示

### show — オブジェクトを表示する

```
show eq1     推定結果表
show xs      系列の値（観測ラベルつき）
show g1      グラフの要約
```

### graph — グラフを作る

```
graph g1.line sales
graph g2.scat ad sales
```

グラフは7種（line、bar、area、spike、scat、xyline、xybar）です。コマンドライン版では、グラフは要約テキストとして表示されます。画像としての保存とコピーは GUI 版の機能です。

## プログラム（.prg）

.prg ファイルは、コマンドを行単位で並べたプログラムです。プログラム変数（`!` 始まりと `%` 始まり）、ループ、サブルーチンを備えます。実行履歴には、プログラムのパスとハッシュ、入力ファイルのハッシュが残り、再現性の確認に使えます。

## エラーの形式

失敗したコマンドは、次の形で報告されます（JSON では `errors` に同じ内容が入ります）。

```
<ファイル>:<行>: <理由>
対象: <失敗したコマンド>
修正: <直し方>
```

理由と修正方法が文で示されるので、コマンドを直して再実行してください。実在しない絶対パスを指定した場合は、探した場所が理由の中に示されます。
