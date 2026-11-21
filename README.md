# MIvue

EViews 互換の計量経済ソフトウェア **MIvue** の公式配布ページです。このリポジトリは配布専用で、ソースコードは含まれていません。

## ダウンロード

最新の ZIP は [Releases](https://github.com/mitunyansoncho/mivue-releases/releases) から取得してください。

- `MIvue-0.1.0-windows-x64.zip`（Windows 10/11、64bit）

ZIP を展開し、`MIvue\MIvue.exe` を起動します。インストールは不要です。

## 初回起動と SmartScreen

署名のない実行ファイルのため、初回起動時に Windows SmartScreen の警告が出ることがあります。その場合は「詳細情報」→「実行」を選んでください。

## 使い方の要点

- 起動時に作業フォルダを選びます。ワークファイル（.evwf）の中に系列・方程式・グラフなどのオブジェクトを作成します。
- EViews 風のコマンド（`wfcreate` / `series` / `equation ....ls` / `graph` / `wfsave` など）で操作でき、推定ダイアログからコマンドなしでも推定できます。
- 詳しい操作は、配布 ZIP 内の `README_DIST.md` を参照してください。

## 利用について

- 学生・研究者への無償配布を目的としています。
- EViews は IHS Global Inc. の商標です。本ソフトは EViews の提供元とは関係のない独立した実装であり、「互換」は機能と計算結果の同一性を指します。

## 数表と出典

p 値・臨界値の計算には、次の公表資料の数値を使用しています（出典はプログラム内にも記録されています）。

- MacKinnon (1996)、MacKinnon–Haug–Michelis (1999): 単位根・共和分検定の応答曲面。配布については著者 James G. MacKinnon の同意を得ています（学生・研究者への無償配布）。
- Levin–Lin–Chu (2002)、Im–Pesaran–Shin (2003)、Pedroni (1999, 2004)、Pesaran (2007): パネル検定の調整項・臨界値。

## 更新の確認

MIvue は起動時と「ヘルプ → 更新を確認」で、このリポジトリの最新リリースを確認します。自動でダウンロードや上書きはしません。
