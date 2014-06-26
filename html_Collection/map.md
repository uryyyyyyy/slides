
## Map

> キーを値にマッピングするオブジェクトです。(略)インタフェースというよりむしろ完全に抽象クラスであった Dictionary クラスに代わるものです。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Map.html)


![alt](./kankoreMap.png)

> [艦隊Map](http://www.dmm.com/netgame_s/kancolle/gallery/)

--

## どんなメソッドを持ってるべき？




--

## インターフェースのメソッド

* put(K key, V value) // 要素の追加
* get(Object key) // 要素の取得
* remove(Object key) // 要素の削除
* entrySet() // マッピングのSetを返す

など、Keyを引数にしてValueに処理をするメソッドが多い。

--

### Q

* get, removeはなぜObjectで受けてるの？
* Map.Entryというクラスがある？

--

## 既知のすべての実装クラス

数が多いので省略して。

* AbstractMap //Mapのスケルトン実装
* ConcurrentHashMap // 同期化されたHashMap
* ConcurrentSkipListMap // 同期化されたSort済みのMap
* EnumMap // Enum用の軽量Map
* HashMap // Hash値で管理するMap
* Hashtable // 性能でHashMapに劣る？
* IdentityHashMap // Keyの比較が、同値でなく同一の場合とするMap
* LinkedHashMap // HashMapをextendsしてLinkをつけたMap
* TreeMap // SortedMapを実装したMap

---

## SortedMap

> そのキーに対して全体順序付けを提供する Map です。マップの順序付けは、キーの自然順序付けに従って行われるか、ソートマップ構築時に通常提供される Comparator を使って行われます。[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/SortedMap.html)

--

## どんなメソッドを持ってるべき？


--

## インターフェースのメソッド

* comparator() // このマップ内のキーを順序付けするのに使うコンパレータを返す。
* firstKey() // Map内の最初のKeyを返す
* headMap(K toKey) // toKeyよりも小さいキーを持つ部分のMapを返す。
* lastKey() // Map内の最後のKeyを返す
* tailMap(K fromKey) // fromKeyよりも大きいキーを持つ部分のMapを返す。

など、順番を元にした処理を行う。

--

## 既知のすべての実装クラス:

* ConcurrentSkipListMap
* TreeMap

### Q

* どういうときに使うの？


---

## HashMap

> Map インタフェースのハッシュテーブルに基づく実装です。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/HashMap.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/HashMap.java.html) - [Row(JDK1.7_60)](./HashMap.java)

--

## Feature

* put - Keyにhash()をかけて得られた出力値をindexとして配置する。
* get - Keyにhash()をかけるだけで要素の場所がわかる。(理論上、O(１))。

`new HashMap(initialCapacity)`で初期サイズを定義できる。

**要素の追加・取得はかなり高速だが、hashの再計算が走ると非常に遅くなるため注意が必要。**

--

### Q

* get, put のオーダーは？
* 複数のKeyでhash値が被った場合は？
* なんで内部でもhash()かけるの？
* `HashMap(int initialCapacity, float loadFactor)`のloadFactorとは？
* tableの再計算（resize）が走るタイミングは？

### Tips

* Keyになるオブジェクトには暗黙の前提条件がある。
	- equals, hashCodeの実装。Immutable。

--

## Implementation

```

    transient Entry<K,V>[] table = (Entry<K,V>[]) EMPTY_TABLE;

    public V get(Object key) {
        if (key == null)
            return getForNullKey();
        Entry<K,V> entry = getEntry(key);

        return null == entry ? null : entry.getValue();
    }
    
    final Entry<K,V> getEntry(Object key) {
        if (size == 0) {
            return null;
        }

        int hash = (key == null) ? 0 : hash(key);
        for (Entry<K,V> e = table[indexFor(hash, table.length)];
             e != null;
             e = e.next) {
            Object k;
            if (e.hash == hash &&
                ((k = e.key) == key || (key != null && key.equals(k))))
                return e;
        }
        return null;
    }
    
    final int hash(Object k) {
        int h = hashSeed;
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }

        h ^= k.hashCode();

        // This function ensures that hashCodes that differ only by
        // constant multiples at each bit position have a bounded
        // number of collisions (approximately 8 at default load factor).
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }
    
    static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : "length must be a non-zero power of 2";
        return h & (length-1);
    }
    
    void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }

        Entry[] newTable = new Entry[newCapacity];
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }

```

---

## LinkedHashMap
> 予測可能な反復順序を持つ Map インタフェースのハッシュテーブルとリンクリストの実装です。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/LinkedHashMap.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/LinkedHashMap.java.html) - [Row(JDK1.7_60)](./LinkedHashMap.java)

--

## Feature

追加された順番も保持しているため、Iterator（拡張For）で回すのが速い。

欠点として、別でLinkedListも作るため、要素の追加には時間がかかる。

--

### Q

* 全要素をなめるとき、なんで速くなるの？

### Tips

---

## TreeMap
> 赤 - 黒ツリーに基づく NavigableMap 実装です。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/TreeMap.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/TreeMap.java.html) - [Row(JDK1.7_60)](./TreeMap.java)

赤黒ツリーとは。

> 赤黒木は、要素の挿入・削除・検索などの操作が、 いかなる場合でも O(logn) の計算量で実行出来る平衡木です[これで分かった赤黒木](http://www.moon.sannet.ne.jp/okahisa/rb-tree/rb-tree.html)

--

## Feature

Keyを順番にsortして保持する。
（Keyが比較可能（`Comparable`）を実装していること。）

Iterator（拡張for）で回すときにKeyのsort順で取ってきたいときに使う。

--

### Q


### Tips

* Java７で仕様が変わった。
	- ComparableでないKeyを使うとNPE

--

## Implementation

```
    public V get(Object key) {
        Entry<K,V> p = getEntry(key);
        return (p==null ? null : p.value);
    }

    final Entry<K,V> getEntry(Object key) {
        // Offload comparator-based version for sake of performance
        if (comparator != null)
            return getEntryUsingComparator(key);
        if (key == null)
            throw new NullPointerException();
        Comparable<? super K> k = (Comparable<? super K>) key;
        Entry<K,V> p = root;
        while (p != null) {
            int cmp = k.compareTo(p.key);
            if (cmp < 0)
                p = p.left;
            else if (cmp > 0)
                p = p.right;
            else
                return p;
        }
        return null;
    }
```

---

## Any Question?

![alt](./kininarimasu.jpg)

> [かる日記](http://karu.blog.so-net.ne.jp/2010-08-24)

---

## Reference

- [艦これ](http://www.dmm.com/netgame_s/kancolle/)
