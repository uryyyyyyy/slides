## 目次

* ラムダ式とは
* 関数型インターフェース
* streamAPI
* Collect処理

---

## ラムダ式

関数型インターフェースを実装した無名クラスの宣言のsyntax-sugar

（厳密には、より最適化されるらしい。→[@IT](http://www.atmarkit.co.jp/ait/articles/1403/17/news105_2.html), [社内Java8勉強会 by池添さん](http://www.slideshare.net/zoetrope/java8-lambdaandstream)）

簡単に書けるし効率良いので、とりあえずラムダ式使うべし。

```java

// こんなのが
Predicate<String> p = new Predicate<String>(){
	@override
	public boolean test(String s){
		return s.isEmpty();
	}
}

// こう書ける！
Predicate<String> p = s -> s.isEmpty();

```

---

## 関数型インターフェース

メソッド定義をひとつだけ持ったインターフェース。**@FunctionalInterface**がついてる。
ラムダ式やメソッド参照の代入先になる。

Javaだと関数が単体で（オブジェクトとして）存在できないため、
メソッドを一つだけ持ったクラスで対応している。

java.util.function以下にたくさん入ってる。

--

## Supplier

値を返す（供給する）関数。
引数なしで何らかの値を返す。

[source(JDK1.8_11)](./Supplier.java)

```java

Supplier<String> s = () -> "abc";

```

--

## Consumer

引数を受け取り、それを使って処理を行う関数。
値を返さないで副作用を起こす。

[source(JDK1.8_11)](./Consumer.java)

```java

Consumer<String> c = s -> System.out.println(s);

```

引数を２つ受け取る`BiConsumer`もある。

--

## Predicate

引数の判定を行う関数。
判定結果をbooleanで返す。

[source(JDK1.8_11)](./Predicate.java)

```java

Predicate<String> p = s -> s.isEmpty();

```

--

## Function

値を変換する関数。
引数を別の値に変換して返す。

[source(JDK1.8_11)](./Function.java)

```java

Function<String, File> f = s -> new File("/tmp", s);

```

--

## UnaryOperator

単項演算子を表す関数。
引数に演算を行い、同じ型の値を返す。（特殊なFunction）

[source(JDK1.8_11)](./UnaryOperator.java)

```java

UnaryOperator<String> op = s -> s.toUpperCase();

```

--

## BinaryOperator

二項演算子を表す関数。
同じ型の2つの引数を受け取り、同じ型の値を返す。

[source(JDK1.8_11)](./BinaryOperator.java)

```java

BinaryOperator<String> op = (s1, s2) -> s1 + s2;

```

--

### Tips

昔からあるComparatorにも@FunctionalInterfaceがついてる。

[source(JDK1.8_11)](./Comparator.java)

### Q

* 何かありますか？
* ここについては覚えるだけだと思います。

---

## StreamAPI

> 順次および並列の集約操作をサポートする要素のシーケンスです。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/util/stream/Stream.html)

（primitive型用のIntStreamなどもあります。）

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

* 自動で最適化（並列化）される。
	- （内部で勝手にやってくれる）
* バグりにくい
* 読みやすい
* 書くのが楽

などの利点がある。

マルチコア時代になり、プログラムの方でも並列化できるように書く必要性が出てきた。

--

## streamAPIのメソッド

全体が知りたければココを読むべし。これだけで十分かと。

[hishidama](http://www.ne.jp/asahi/hishidama/home/tech/java/stream.html)

--

## 汎用メソッド

* `allMatch(Predicate<? super T> predicate)`
	- すべての要素においてPredicateの結果がtrueになるかどうか。

* `concat(Stream<? extends T> a, Stream<? extends T> b)`
	- ２つのStreamを合成する。両方ともTを継承している型であること。

* `distinct()`
	- 要素の重複を除いたStreamを返す（Object#equals()で比較）。

* `filter(Predicate<? super T> predicate)`
	- Predicateの結果がtrueになる要素だけで構成されるStreamを返す。

--

* `forEach(Consumer<? super T> action)`
	- 各要素をCounsumerに流し込む（Counsumerの返り値はvoid）

* `map(Function<? super T,? extends R> mapper)`
	- 各要素をFunctionに入れて、出てきた要素でStreamを作る。

* `flatMap(Function<? super T,? extends Stream<? extends R>> mapper)`
	- Streamの各要素のStreamを一つのStreamにまとめる。

--

### Tips

* 引数の関数型インターフェースを理解してれば、各メソッドの動作は想像つくと思う。
* Primitive型は`<T>`に入らないので別メソッドで区別している。（mapToInt()とか）
* これらを使いこなせばfor文は駆逐できる！（らしい）

### Q

* for文じゃなきゃ対応できない例を挙げよ。
* それに対して反論せよ。


---

## Collect処理

[Collectorを征す者はStream APIを征す（部分的に）](http://blog.exoego.net/2013/12/control-collector-to-rule-stream-api.html)

### なぜ必要か。

関数型であれば標準でできることがJavaだとやりにくい（最適化されず遅い）ので、
Collectの中で上手いことやってくれる（実装は知らなくてもいい）。


--

## collect

末端処理（Streamから普通のオブジェクトに戻す処理）を行う。

`Stream#collect(Supplier<R> supplier, BiConsumer<R,? super T> accumulator, BiConsumer<R,R> combiner) <R> R`

中の処理は、Supplierで作られた`<R>`のインスタンスに、Streamから来た要素`<T>`を
accumulatorで処理していくようです。

（たぶん並行処理したいときに`combiner`で、あるスレッドで作られた`<R>`と、別のスレッドで作られた`<R>`を合成している。）

--

## 例

```java

	List<String> list = Stream.of("a", "b", "c").collect(
				() -> new ArrayList<>(10), 
				(l, t) -> l.add(t), 
				(l1, l2) -> l1.addAll(l2));
	System.out.println(list); 
	//a
	//b
	//c

```

supplierでインスタンス(`List<String>`）に、Streamから来た要素(String)をAccumulatorで処理している。

（(l, t)のlがList, tが要素(String)。返り値はないがlistに蓄積）

もしListが複数出来たら、combinerでまとめる。

--

これでバッチリ！

といっても、一般的な処理を毎回書くのはダルい。。。

そこで、

--

## Collectors

> 要素をコレクションに蓄積したり、さまざまな条件に従って要素を要約するなど、有用な各種リダクション操作を実装したCollector実装。

> [JavaSE8-API](http://docs.oracle.com/javase/jp/8/api/java/util/stream/Collectors.html)

[使い方](http://www.ne.jp/asahi/hishidama/home/tech/java/collector.html)

--

### 汎用メソッド

* groupingBy(Function<? super T,? extends K> classifier)
	- classifierの結果が同じものをgroup化したMapを返す。追加処理も書けるよ。

* joining()
	- 流れてきた要素を全てStringで連結して一つにする。

* ToCollection(Supplier<C> collectionFactory)
	- StreamをCollectionに変換する（toList, toMapなどもある。）

--

## その他

（代替できるので覚えなくていいかも。
最適化されてるっぽいので、どうしても遅いなら検討する程度で。）

* collectingAndThen → Stream#collect()・Function。
* summingLong → Stream#mapToLong()・LongStream#sum()
* summarizingLong() → Stream#mapToLong()・LongStream#summaryStatistics()
* maxBy() → Stream#max()
* reducing() → Stream#reduce()
* counting() → Stream#count()


---

### Tips

* 無限リスト・遅延評価(stream#generate(), stream#limit())

### Q

---

## Any Question?

![alt](./doraemon.jpg)

> [http://livedoor.4.blogimg.jp/](http://livedoor.4.blogimg.jp/chihhylove/imgs/6/f/6f0e791f.jpg)

---

* [Date & Time API](./date_time.html)
