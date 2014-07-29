## StreamAPI

> 順次および並列の集約操作をサポートする要素のシーケンスです。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/util/stream/Stream.html)

![alt](./stream_1.png)

> [1st8](http://www.first8.nl/presentations/java8/#/3/1)

--

> Streamの値の持ち方はjava.util.Listのようなイメージ。

> * なお、Streamはコレクションフレームワーク（List・Queue・Set・MapやCollectionsクラス等）の一員ではない。
> * また、java.io.InputStreamやjava.io.OutputStreamやPrintStream等とは（名前が似ている以外の）何の関係も無い。

> [hishidama](http://www.ne.jp/asahi/hishidama/home/tech/java/stream.html)

--

## 何が嬉しいの？

処理を宣言的に（簡単に）書けるため、

* 最適化（並列化）しやすい。勝手によろしくやってくれる
* バグりにくい
* 読みやすい
* 書くのが楽

などの利点がある。

--

## 学ぶ内容

* streamクラスのメソッド
	- map
	- filter
	- flatMap
	- forEach
* Collect処理
	- 
* 関数型インターフェース
	- Predicate
	- Function
	- Supplier

## インターフェースのメソッド

* add(int index, E element) //要素の追加
* get(int index) //要素の取得
* indexOf(E element) //要素の検索
* remove(int index) //要素の削除
* set(int index, E element) //要素の代入
* toArray(T[] a) //配列を返す

など、要素の順番（index）に対してアクセスする。

--

### Q

* indexOfの実装は？

### Tips

* 順番を入れ替える系はCollections, Arraysに入っている
	- 例えば、`sort`の実装を読んでみるとわかる

--




---

## Any Question?

![alt](./doraemon.jpg)

> [http://livedoor.4.blogimg.jp/](http://livedoor.4.blogimg.jp/chihhylove/imgs/6/f/6f0e791f.jpg)

---
