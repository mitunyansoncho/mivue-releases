# クイックスタート（動作確認用）

MIvue のコマンドライン版が正しく動くかを、短い手順で確認するためのセットです。

## 手順

1. このフォルダ（`quickstart`）の中で、コマンドライン版を実行します。

   ```
   mivue quickstart.prg
   ```

   Python 版の場合は `python -m mivue quickstart.prg` です。

2. 標準出力が `expected.txt` と一致すれば正常です。終了コードは 0 です。
   あわせて、同じフォルダに `quickstart_result.evwf` が作られます。

## 中身

- `sales.csv` — サンプルデータ（12 行、sales と ad の2系列）。
- `quickstart.prg` — CSV を開き、`sales` を `ad` で最小二乗推定して結果表を表示し、ワークファイルを保存するプログラム。
- `expected.txt` — 正常時の標準出力の確定形。推定値は C = 44.37931、AD = 7.892573、R-squared = 0.941681 です。

出力が一致しない場合は、版数（`mivue --version`）と、実行したフォルダがこのフォルダであるかを確認してください。
