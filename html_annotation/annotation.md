
## 目次

* アノテーションとは？
* 定義の仕方・使い方
* 標準アノテーション
* ライブラリのアノテーション

---

## アノテーションとは？

ソースコードにメタ情報を仕込むことで、開発者やコンパイラ、実行環境に対して、そのコードの意図を伝えるもの。

　

標準で組み込まれているもの

* JavaDoc
* Override
* SuppressWarnings
* Depricated

--

## 普通のコードとは違うの？


* Interfaceとの違い

* 命名規則との違い

--

## Interfaceとの違い

（マーカーとして利用する場合）

　

### Interface

* コンパイル時にチェックされる。
* 型安全が保証される
* 実装に依存する

　

### アノテーション

* 実行時にチェックされる。
* 型安全でない。
* 実装に依存しないため分離しやすい。

--

## 命名規則との違い

* 構文補助
* 宣言的プログラミング（このコードは何の分類かを明記できる。）

　

〜Java1.4では、命名パターンでチェックしたりしていた。（JUnit3.Xなど）

これでは正確じゃないし、間違ってるのかどうなのかわかりにくかったので、メタ情報として明示するようにした。

Spring4やJUnit４でも使われている。

---

## 定義の仕方・使い方

* [JUnitの例](https://github.com/junit-team/junit/blob/master/src/main/java/org/junit/Test.java)

* [Lombokの例](https://github.com/rzwitserloot/lombok/blob/master/src/core/lombok/Value.java)

ただ定義してるだけ。裏でどう動いているかは別のところを見ないといけない。

　

* [自作テストランナー](https://github.com/uryyyyyyy/JavaStudy/blob/master/src/study/annotation/execute/TestRunner.java)

裏でどう動いているかのサンプル。

---

## 標準アノテーション

たぶん知ってると思うので省略します。

* SuppressWarnings
* Depricated
* Override

（コード上ではただ定義してるだけです。裏でどう動いているかはよくわかりませんでした。）

--

## 自作用アノテーション

* Target
* Retention
* Documented
* Inherited

--

## Target

> 注釈型が適用可能なプログラム要素の種類を示します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/lang/annotation/Target.html) - [Web(openjdk-7)](http://www.docjar.com/html/api/java/lang/annotation/Target.java.html) - [Row(JDK1.7_60)](./Target.java)

[使用例](https://github.com/uryyyyyyy/JavaStudy/blob/master/src/study/annotation/marker/PlaceAnnotation.java)

Target自身にも`@Target(ElementType.ANNOTATION_TYPE)`がついてる。

--

## Retention

> 注釈付きの型を持つ注釈を保持する期間を示します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/lang/annotation/Retention.html) - [Web(openjdk-7)](http://www.docjar.com/html/api/java/lang/annotation/Retention.java.html) - [Row(JDK1.7_60)](./Retention.java)

[使用例](https://github.com/uryyyyyyy/JavaStudy/blob/master/src/study/annotation/marker/ScopeAnnotation.java)

JUnitではRuntimeで、LombokではSourceで定義されている様子。

--

## Documented

> 型を持つ注釈が javadoc および同様のツールによってデフォルトでドキュメント化されることを示します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/lang/annotation/Documented.html) - [Web(openjdk-7)](http://www.docjar.com/html/api/java/lang/annotation/Documented.java.html) - [Row(JDK1.7_60)](./Documented.java)

よくわかってない。

--

## Inherited

> 注釈型が自動的に継承されることを示します。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/lang/annotation/Inherited.html) - [Web(openjdk-7)](http://www.docjar.com/html/api/java/lang/annotation/Inherited.java.html) - [Row(JDK1.7_60)](./Inherited.java)

使い方わからない。[Sample](https://github.com/uryyyyyyy/JavaStudy/blob/master/src/study/annotation/marker/InheritedAnnotation.java)


---

## JavaDoc

Javadoc関連のアノテーション

* @author
* @version
* @deprecated
* @return
* @params
* @exception
* @see

あんまり興味ない。

---

## Any Question?

![alt](./file/kiniiranai.jpg)

> [やらおん](http://blog-imgs-64.fc2.com.yaraon-proxy2.yarareyaku.net/y/a/r/yaraon/oo_20140519004812630.jpg)

---

## reference

* EffectiveJava

* Perfect Java

* [Javaのアノテーション便利すぎワロタｗｗをしたいので総復習したけどワロエナイ](http://komaken.me/blog/2013/03/13/java%E3%81%AE%E3%82%A2%E3%83%8E%E3%83%86%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E4%BE%BF%E5%88%A9%E3%81%99%E3%81%8E%E3%83%AF%E3%83%AD%E3%82%BF%EF%BD%97%EF%BD%97%EF%BD%97%EF%BD%97%E3%82%92%E3%81%97/)

* [アノテーション作成方法](http://www.ne.jp/asahi/hishidama/home/tech/java/annotation.html)

---

## Next

JUnitかLombokのコード読みたい。


Lombokはハードル高そう。。。

http://stackoverflow.com/questions/20876101/how-to-read-the-source-of-lombok
