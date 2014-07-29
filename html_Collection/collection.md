### Collection

> コレクションは、その要素であるオブジェクトのグループを表します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Collection.html)

![alt](./kankore.jpg)

> [艦隊Collection](http://www.dmm.com/netgame_s/kancolle/gallery/)

--

## どんなメソッドを持ってるべきか？

要素に対して行う処理とは？

### Response

* CRUD操作的なものかな。
* 追加・変更・参照・削除とか？
* Immutableにしたいな


--

### Collectionのメソッド

* add(E e) //要素の追加
* contains(Object o) //要素の存在確認
* isEmpty() //空かどうか
* remove(Object o) //要素の削除
* size() //リストの要素数確認
* toArray() //配列に直す
* iterator() //iteratorを返す。

--

### Q

- containsやremoveはどうやって対象のオブジェクトを探す？
- Integer.MAX_VALUE以上の要素をaddしたらどうなる？

### Tips

- Itarate中に要素を追加したり削除したりするとErrorになる。
- addとかclearとかを実装したくない場合は実行時に`UnsupportedOperationException`を返す。
	* 実行時まで気付かない。すごくイケてない。

--

### 既知の実装クラス:

全ては多すぎますが、Set, List, Deque(Queue)など。(Mapは別)

![alt](./overview.jpg)

> [Learn with Harsha](http://learnwithharsha.com/day-5-collections-framework/)

---

## Collections

> このクラスは、コレクションに作用する、またはコレクションを返す static メソッドだけで構成されます。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Collections.html) - [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/Collections.java.html) - [Row(JDK1.7_60)](./Collections.java)

--

### Collectionsのメソッド

* emptySet() (MAPやLISTも)
	- 空のSetを持ってくる。
* sort(List&lt;T&gt; list, [Comparator<? super T> c])
	- `mergesort`する。 O(n log(n)) のパフォーマンス
* binarySearch(List<? extends T> list, T key, [Comparator<? super T> c])
	- `sort`してから使うと探索が速くなる。 O(n) -> O(log (n))
	- というより、`sort`してないで使うのは想定されていない
* reverse shuffle rotate
	- Listの順番を入れ替える。O(n)
* disjoint(Collection<?> c1, Collection<?> c2)
	- 共通の要素があるかのチェック
* max (min)
	- 要素の順序付けに従って、指定されたコレクションの最大の要素を返す。

--

### Q

- `Comparator`, `Comparable`ってどう違うの？
- sortのアルゴリズムはどんなの？
- disjointって単語の意味は？

---

## Any Question?

![alt](./bakanisinaide.jpg)

> [hatena](http://f.hatena.ne.jp/pema/20140126003617)

---

- [List](list.html)
- [Set](set.html)
- [Map](map.html)

---

## Reference

- [艦これ](http://www.dmm.com/netgame_s/kancolle/)
