# 株価分析＆類似チャート照合アプリ (Web版)

銘柄コードを入力すると、直近の値動きと過去の類似チャートパターンをブラウザだけで分析します。
元はStreamlit(Python)アプリでしたが、GitHub Pagesなどの静的ホスティングでそのまま動くように、
ロジックを丸ごとJavaScriptに移植した単一HTMLファイルです。サーバーやビルドは不要です。

## 使い方 (GitHub Pagesで公開)

1. このリポジトリに `index.html` を置く
2. Settings → Pages → 「Deploy from branch」を選択して公開
3. 発行されたURL (`https://ユーザー名.github.io/リポジトリ名/`) を開く
4. 銘柄コード(例: `7203.T`, `AAPL`)を入力して「分析を実行する」

## 仕組み

- 株価データは Yahoo! Finance の公開チャートAPI (`query1.finance.yahoo.com/v8/finance/chart/...`) から取得します。
- このAPIはブラウザから直接叩くとCORS(オリジン制限)でブロックされるため、
  複数の公開CORS回避プロキシ(`allorigins.win` → `codetabs.com` → `killcors.com` → `cors.lol` → 予備で再度`allorigins.win`)を順番に試す形にしています。1つが混雑・停止していても他の経路に自動で切り替わります。
- 分析ロジック(トレンド判定・類似チャート照合の相関計算など)は、元のPythonコードと
  数値が一致するよう検証済みです。

## 既知の制限

- 無料の公開プロキシを使っているため、混雑時や仕様変更時にデータ取得が失敗することがあります。
  失敗した場合は少し時間を置いて再度お試しください。
- より安定させたい場合は、`index.html` 内の `PROXIES` の配列に、ご自身で用意した
  CORSプロキシやサーバーレス関数のURLを追加/差し替えてください。
