## StreamAPI

> 順次および並列の集約操作をサポートする要素のシーケンスです。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/util/stream/Stream.html)

![alt](./stream_1.png)

> [1st8](http://www.first8.nl/presentations/java8/#/3/1)

（primitive型用のIntStreamなどもありますが、まぁ同じものです。）

--

> Streamの値の持ち方はjava.util.Listのようなイメージ。

> * なお、Streamはコレクションフレームワーク（List・Queue・Set・MapやCollectionsクラス等）の一員ではない。
> * また、java.io.InputStreamやjava.io.OutputStreamやPrintStream等とは（名前が似ている以外の）何の関係も無い。

> [hishidama](http://www.ne.jp/asahi/hishidama/home/tech/java/stream.html)

--

## ラムダ式

関数型インターフェースを実装した無名クラスの宣言のsyntax-sugar

めっちゃ簡単に書けるよ

```java


```

--

## 何が嬉しいの？

処理を宣言的に（簡単に）書けるため、

* 最適化（並列化）しやすい。（内部で勝手にやってくれる）
* バグりにくい
* 読みやすい
* 書くのが楽

などの利点がある。

--

## 目次

* 関数型インターフェース
	- Predicate
	- Function
	- Supplier etc...
* streamクラスのメソッド
	- map
	- filter
	- flatMap
	- forEach etc...
* Collect処理
	- groupingBy
	- toList etc...

---

## 関数型インターフェース

メソッド定義をひとつだけ持ったインターフェース。
ラムダ式やメソッド参照の代入先になる。

Javaだと関数が単体で（ファーストクラスとして）存在できないため、
メソッドを一つだけ持ったクラスで対応している。

**@FunctionalInterface**がついてるハズ。

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

* 関数型言語では `func(element, func2)` などのように、関数を引数として扱えるが、
Javaだとできないので、 `func(element, Predicate)` のようになる。

* 例ではラムダ式を使ったが、使わずに書くとこんな感じ。

```java

Predicate<String> p = new Predicate<String>(){
	
	@override
	public boolean test(String t){
		return t.isEmpty();
	}
}

```

---

## streamクラスのメソッド




---

## Any Question?

![alt](./doraemon.jpg)

> [http://livedoor.4.blogimg.jp/](http://livedoor.4.blogimg.jp/chihhylove/imgs/6/f/6f0e791f.jpg)

---
