## List

> 順序付けられたコレクションです。シーケンスとも呼ばれます。
> [JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/List.html)

どんなメソッドを持ってるべきか考えてみる。

![alt](./kankoreList.png)

> [艦隊List](http://www.dmm.com/netgame_s/kancolle/gallery/)

--

## インターフェースのメソッド

* add(int index, E element) //要素の追加
* get(int index) //要素の取得
* indexOf(E element) //要素の検索
* remove(int index) //要素の削除
* set(int index, E element) //要素の代入
* toArray(T[] a) //配列を返す

など、要素の順番（index）に対してアクセスする。

### Q

* indexOfの実装は？

### Tips

* 順番を入れ替える系はCollections, Arraysに入っている

--

ListのAPIにこんな記述が。

> List インタフェースは、iterator、add、remove、equals、および hashCode の各メソッドの規約に対して、Collection インタフェースで指定されているものに加えてさらに条項を追加します。便宜上、ほかの継承メソッドの宣言もここに含まれます。

### Q

* どういう意味？


--

## 既知のすべての実装クラス:

* AbstractList // Listのスケルトン実装（RandamAccsess）
* AbstractSequentialList // Listのスケルトン実装（Sequential）
* ArrayList // 定番
* CopyOnWriteArrayList // 同期させたい時などに役立つ(?)
* LinkedList // List + Deque
* RoleList・RoleUnresolvedList・AttributeList // 誰？
* Stack // LIFO(Dequeで良くない？)
* Vector // スレッドセーフなArrayList。過去の遺産

### Q

- スケルトン実装って？
- スレッドセーフな実装って？
- SortedListがないのは何で？

---

## Arrays

> このクラスには、ソートや検索など、配列を操作するためのさまざまなメソッドがあります。また、配列をリストとして表示するための static ファクトリもあります。
> [JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Arrays.html) -  [source(openjdk-7)](http://www.docjar.com/html/api/java/util/Arrays.java.html)

- asList(T... a)
	* 指定された配列に連動する固定サイズのリストを返します。
- binarySearch(Object[] a, Object key)

- copyOf(T[] original, int newLength)


### Tips

- 今や使うとしたらasListくらい？これには落とし穴が。。。
- Collectionsクラスは中でArraysを呼んでたりする。(sortとか)

---

## ArrayList

> List インタフェースのサイズ変更可能な配列の実装です。
> [JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/ArrayList.html) -  [source(openjdk-7)](http://www.docjar.com/html/api/java/util/ArrayList.java.html)

![alt](./arrayList.png)

> [www.wakhok.ac.jp](http://www.wakhok.ac.jp/~tomoharu/j2ee1-tokyo-2005/text/index_c9.html)

### Feature

* インデックスを指定してのget/setが速い
* 先頭からすべての要素を取っていくのが速い。

### Q

* add, remove, get, setのオーダーは？
* Capacityの取り扱いは？

--

## Implement

---

## LinkedList

> List および Deque インタフェースの二重リンクリスト実装です。
> [JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/LinkedList.html) - [source(openjdk-7)](http://www.docjar.com/html/api/java/util/LinkedList.java.html)

![alt](./linkedList.png)

> [www.wakhok.ac.jp](http://www.wakhok.ac.jp/~tomoharu/j2ee1-tokyo-2005/text/index_c9.html)

### Feature

* 要素のadd/removeが速い。

### Q

* add, remove, get, setのオーダーは？
* Dequeって何？

--

## Implement


---

### sortとsearch

Collectionsクラスに以下のメソッドが用意されている。。

* sort(list, [options])
* binarySearch(list, target, Comparator)

sortしてからbinarySearch（二分探索）すれば探索の効率が良くなる。(O(n) -> O(logn))

---

## Any Question?

![alt](./omaeha.jpg)

> [livedoor.blogimg.jp](http://livedoor.4.blogimg.jp/chihhylove/imgs/6/f/6f0e791f.jpg)
