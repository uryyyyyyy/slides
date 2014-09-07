
## 目次

* アノテーションとは？
* 定義の仕方
* 標準アノテーション
* ライブラリのアノテーション

---

## アノテーションとは？

ソースコードにメタ情報を仕込むことで、開発者やコンパイラ、実行環境に対して、そのコードの意図を伝えるもの。


標準で組み込まれているもの

* JavaDoc
* Override
* SupppressWarnings
* Depricated

--

### 普通のコードとは違うの？

よりストレートに開発者の意図を伝えられる。

〜Java1.4では、命名パターンでチェックしたりしていた。（JUnit3.Xなど）

これでは正確じゃないし、間違ってるのかどうなのかわかりにくかった。

Spring4やJUnit４でも使われている。

--

### 使うとどう便利になるの？

マーカーをつけておくことで、裏で対象のコードを見つけるのが容易になる。

例：[自作テストユニット](https://github.com/uryyyyyyy/JavaStudy/tree/master/src/study/annotation)

---

## 定義の仕方・使い方

コードを参照。


---

## 標準アノテーション

--

## SuppressWarnings

警告を隠す

使うべきでないが、やむを得ない場合でも理由は明記するべき。

--

## Depricated

警告を隠す

使うべきでないが、やむを得ない場合でも理由は明記するべき。

--

## Override

もしオーバーライドできてなかったときにエラーを吐いてくれる(引数が違うとか名前間違えたとか。)


---

## メタアノテーション

普通は使わないので省略（フレームワーク系か、リフレクション呼びたいときなど）

* @Retention
* @Target
* @Inherited
* @Documented

---

## マーカの使いドコロとInterfaceとの比較

Interface
コンパイル時にチェックされる。
型安全が保証される
実装に依存する


アノテーション
実行時にチェックされる。
型安全でない。
実装に依存しないため、プラグインなどと分離しやすい。


* 構文補助
* 宣言的プログラミング（このコードは何の分類かを明記できる。）

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

http://www.docjar.com/html/api/java/lang/Override.java.html


--- 

## reference

http://komaken.me/blog/2013/03/13/java%E3%81%AE%E3%82%A2%E3%83%8E%E3%83%86%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3%E4%BE%BF%E5%88%A9%E3%81%99%E3%81%8E%E3%83%AF%E3%83%AD%E3%82%BF%EF%BD%97%EF%BD%97%EF%BD%97%EF%BD%97%E3%82%92%E3%81%97/
