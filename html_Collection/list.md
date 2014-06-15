### List

Listとは？
順序付けられたコレクション

[List-JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/List.html)

#### インターフェースのメソッド

* add(int index, E element) //要素の追加
* contains(E element) //要素の存在確認
* get(int index) //要素の取得
* indexOf(E element) //要素の検索
* remove(E element) //要素の削除
* set(int index, E element) //要素の代入

など、要素の順番（index）に対してアクセスする。

List インタフェースは、iterator、add、remove、equals、および hashCode の各メソッドの規約に対して、Collection インタフェースで指定されているものに加えてさらに条項を追加します。便宜上、ほかの継承メソッドの宣言もここに含まれます。

--

既知のすべての実装クラス:

* AbstractList
* AbstractSequentialList
* ArrayList
* AttributeList
* CopyOnWriteArrayList
* LinkedList
* RoleList
* RoleUnresolvedList
* Stack
* Vector

---

### AbstractList




---


### ArrayList

* インデックスを指定してのget/setが速い
* 先頭からすべての順番を取っていくのが速い。

![alt](./arrayList.jpg)

---

### LinkedList

* 要素の挿入・削除が速い。

![alt](./linkedList.jpg)

---

### sortとsearch

Collectionsクラスに以下のメソッドが用意されている。。

* sort(list, [options])
* binarySearch(list, target, Comparator)

sortしてからbinarySearch（二分探索）すれば探索の効率が良くなる。(O(n) -> O(logn))

---
