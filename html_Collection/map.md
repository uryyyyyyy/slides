### Map

準備中

メソッド群

* ContainsKey //Keyの存在チェック
* ContainsValue //Valueの存在チェック
* entrySet //要素の集合を取る。
* get //要素の取得
* keySet //Keyの集合を取る。
* put //要素の追加
* remove //要素の削除
* values //Valueの集合を取る。

--

実装クラス

* HashMap
* LinkedHashMap
* TreeMap

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
