

準備中

---

## Set

> 重複要素のないコレクションです。...数学で言う集合の抽象化をモデル化します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Set.html)



![alt](./kankoreSet.png)

> [艦隊Set](http://www.dmm.com/netgame_s/kancolle/gallery/)

--

## どんなメソッドを持ってるべき？


### Response

* Collectionと同じじゃない？
* 既にあるかの判定？

--

## インターフェースのメソッド

* containsAll(Collection<?> c) //入力のコレクションの全要素が含まれているか？
* retainAll(Collection<?> c) //入力のコレクション内にある要素だけを保持する
* add(E e) //要素の追加（重複しない場合に限る）

など、Collectionとほぼ同じ。

--

## 既知のすべての実装クラス

* AbstractSet //
* ConcurrentSkipListSet
* CopyOnWriteArraySet
* EnumSet
* HashSet
* JobStateReasons
* LinkedHashSet
* TreeSet

---

基本的にはMapと同じ（MapのKeyも一意なのでSetと同じになる。）
