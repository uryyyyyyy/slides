
準備中

---

## Map

> キーを値にマッピングするオブジェクトです。...インタフェースというよりむしろ完全に抽象クラスであった`Dictionary`クラスに代わるものです。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Map.html)


![alt](./kankoreMap.png)

> [艦隊Map](http://www.dmm.com/netgame_s/kancolle/gallery/)

--

## どんなメソッドを持ってるべき？


### Response

* KeyからオブジェクトをCRUDする操作
* 

--

## インターフェースのメソッド

* put(K key, V value)
* get(Object key)
* remove(Object key)
* keySet()


--

## 既知のすべての実装クラス

数が多いので省略して。

---

### HashMap

![alt](./hashMap.jpg)

要素を追加する際に、Keyにhash()をかけて得られた出力値をindexとして配置する。
普通はHash値は被らないはずだが、同じになった場合はLiskedListで追加されていく。

getする際は、同様にKeyにhash()をかけて得られたindexの先にValuesがある。
もし複数の要素があった場合は、順番にKeyでequals()比較を行う。一致したらそのValueを取ってくる。


`new HashMap(initialCapacity)`で初期サイズを定義できる。

*要素の追加・取得はかなり高速だが、hashの再計算が走ると非常に遅くなるため注意が必要。*

---

### LinkedHashMap
HashMapとLinkedListを合わせた感じ。

最後に追加・もしくは参照された値が始めに来るようになっているため、順序も含めて保持しておきたいときに用いる。

欠点として、別でLinkedListも作るため、要素の追加には時間がかかる。

---

### TreeMap
Keyの比較をして、二分木でMapを作成する。

（比較可能（Comparable）を実装していること。）

![alt](./treeMap.jpg)

Keyのsort順に並べたい場合はこれを使うとよいでしょう。
