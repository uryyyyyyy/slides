
## Index

* Elasticsearchとは？
* 全文検索の仕組み
* Analyzer
* 概念・単語
* サンプル

---

## Elasticsearchとは？

Apache v2ライセンスで公開されているオープンソースソフトウェアであり、全文検索エンジンであるLuceneを使用した、全文検索システムです。

特徴として

* RESTfulなAPIが使える
* InputもOutputもJSON
* スキーマレスで面倒な定義無しにデータを登録可能

等があります。

from: http://dev.classmethod.jp/cloud/aws/use-elasticsearch-1-use-kuromoji/

--

### 何が嬉しいのか？

* Nodeを複数立てることでスケールアウト可能
* インクリメンタルサーチ
* 集計系のクエリが扱える
* ネストしたデータ構造を扱える
* 日本語の形態素解析ソフト「Kuromoji」が使える
* Kibanaというデータのグラフィカルツールが使える

etc...

--

## どんな用途に向いてる？

* 大量データの全文検索（複数Nodeで分散処理できる・精度が高い）
* あいまい検索・サジェストなどのリッチな検索全般
* ログ統計（Kibana）

--

## できないこと

* Transactionの管理（rollbackなどの仕組み）
* joinなど、複数データ構造の合成

（あくまで検索専用というのが無難か）

---

## 全文検索の仕組み

* データの登録
	- 文字列→Analyzer→格納

* データの検索
	- 検索文字列→Analyzer→一致するtermを取得

---

## 構文解析

構文解析（analyze）は、文字列を意味のある単位（term）に分割する。

* 文字列ストリーム（Imput）→
* 事前フィルタリング（Character filters）→
* 単語に分割（Tokenizer）→
* 事後処理（TokenFilter）→
* 保存 or 検索(Output)

という流れで処理される。

![alt](./image/Signatures.svg)

https://www.found.no/foundation/text-analysis-part-1/

--

### analyzer

analyzerは、文字列の分割方法を定義するtokenizerと、分割後の文字列の整形処理を定義するfilterによって構成されます。

（先の流れ全体をまとめたものをanalyzerと呼ぶ。）

例えば、tokenizerがngramで文字列を分割し、filterで大文字小文字を小文字に統一してしまうなどといった定義をすることが出来ます。

analyzerはいくつでも定義することが出来き、Field毎にどのanalyzerを利用するか決めることが出来ます。

http://engineer.wantedly.com/2014/02/25/elasticsearch-at-wantedly-1.html

--

### Example

![alt](./image/custom_analyzers_diag.png)

https://www.found.no/foundation/text-analysis-part-1/

--

### kuromoji

日本語の形態素解析ソフト。

elasticsearch向けにAnalyzerやtokenizerを提供しているみたい。

[動作チェック](https://github.com/uryyyyyyy/elasticsearchSample/tree/master/script/kuromoji)

### Q

形態素解析ソフトの比較？（MeCab・ChaSen他）


