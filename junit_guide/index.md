
# 目次

## なぜテスト駆動開発か。

なぜテストを尽くさないのか。
gazou

---

テストをしなかった場合
忙しい、とテストを怠ったAさん
「普段の業務で忙しくてテストしている時間がなくて後回しにしていたら前よりもヒドイものが出来てしまった。
不具合の対応に追われて新機能が作れない。不具合の特定にも時間がかかる
技術的負債がたまり、周りには触れてはいけないコードばかりで怖くて動けません
※あくまで個人の感想で個人差があります

---

テストをした場合
テストを頑張ったBさん
「思い切ってTDDにチャレンジしたおかげでテストの点数も上がって上司にも褒められるし、実装スキルも上がって新規案件でもレギュラーに選ばれるし、憧れの先輩と付き合うことも出来ました。
※あくまで個人の感想で個人差があります

---

冗談はこのくらいにして

### そもそもの目的

- 本来の理想は保守の効率化。
- そのためには、疎結合やレイヤー分けをして拡張性を確保すること、
- 過去の実装を作り変えていくこと、かつデグレが発生しないようにすること、

これらを必然的に満たしてくれるのがTDD

---

## 当面の目標。

- テストをササッと書けるようになる。
- 実装の前に、テストを意識した設計とゴールの明確化ができるようになる。
- 自分の実装の出来や想定の足りなさがよくわかってPDCAが回せる。
- 実装後、修正後にテストを見せることで、周囲の人の評価アップ
- 転職する際もテストの経験は活きるはず。

---

### 実業務の想定。
- 設計に時間をかけられるようになる。
- 全てにテストを書けとは言わない
- 実装が不安なところはテストを書いて安心する。
- コメントを添えるよりもテストで示したほうがわかりやすい。
- テストする必要がないようなシンプルな記述を心がけるようになる。

---

## 目指すコードのカタチ

- グローバル変数の駆逐
- StateLessなオブジェクト
- 疎結合- 依存関係がすっきりしている。
- 関数的（いつ実行しても結果が変わらない）
- 関数的２（再利用できる小さなメソッドを作る）
- 仕様書、動作説明書となる

---

## テスト駆動開発のサイクル
- 設計する
- テストコード書く
- プロダクトコード書く（テストをクリアする）
- リファクタリングする

このプロセスを通じて保守性の高いコードを身につける。

---

## もしかして→”めんどくさい”?
- 工数が増えては意味がない。長期的に考えて楽を目指す。
- テストをしなくてもわかるような設計- 記述が理想
- 評価- 保守における単純作業は自動化してしまおう。
- 慣れないのは、プログラマ脳になりきれていないから。
- 汚い- 地雷だらけのコードの中で仕事したくない。

TDDは保守などの単純作業を知的労働に変える思想なのだ。

---

## JUnitテストの基本

---

## 基礎知識


テストには大きく４つの流れがある。
- SetUp
→初期化- 入力値と期待結果の設定
- Execute
→メソッドの実行（最小限の範囲で）
- Verify
→比較する
- TearDown
→次のテストに影響がないか確認。
 
---

よく使われる単語
単語リスト
SUT → System Under Test。自分が今テストしているもの
Mock → 暫定的に用意しておく仮オブジェクト
Stab → テスト用に、「こう動いて欲しい」と記述するオブジェクト
Coverage → 網羅率。コードやメソッド（C0・C1）の網羅の割合
TestSuite → テストのグループ。（今回は割愛）
※StubとMockの違いは実際には理解しきれていませんが、Test特化かそうでもないかの違いっぽいです。


---


習うより慣れろ
Calculator.java

- Sample1
```java


    public int multiply(int x, int y) {
        return x * y;
    }
```
このプロダクトコードに対して

---

習うより慣れろ
CalculatorTest.java

- Test1
```java


@Test
    public void multiplyで3と4の乗算結果が取得できる() {
     //SetUp 
        Calculator calc = new Calculator();
        int expected = 12;
     //Execute
        int actual = calc.multiply(3, 4);
     //Verify
        assertThat(actual, is(expected));
     //TearDwon
    }

@Test
    public void multiplyで5と7の乗算結果が取得できる() {
        Calculator calc = new Calculator();
        int expected = 35;
        int actual = calc.multiply(5, 7);
        assertThat(actual, is(expected));
    }
```

テストコードはこのようなカタチ。 
まずはこれを真似るところから。

---



次に例外を。

習うより慣れろ２
Calculator.java
```java
          public class Calculator{

            public float divide(int x, int y) {
        if (y == 0) throw new IllegalArgumentException("divide by zero.");
        return (float) x / (float) y;
    }
          }
```
        
今度は例外を


習うより慣れろ２
CalculatorTest.java

```java
          public class CalculatorTest{
    @Test
    public void divide_3_2Test() {
        Calculator calc = new Calculator();
        float expected = 1.5f;
        float actual = calc.divide(3, 2);
        assertThat(actual, is(expected));
    }

    @Test(expected = IllegalArgumentException.class)
    public void divide_5_0Test() {
        Calculator calc = new Calculator();
        calc.divide(5, 0);
    }
```

@Testについてる(expected = ??)がTestの期待値です。

---

さて、改めて確認です。

良い設計
- 細かく分割されている→問題が局所化されること
- 関数的である→入力に対しての出力が不変
- 関数的である２→オブジェクトが状態を持たない。
- 依存関係が少ない。簡潔である。

※ここで言う「良い」はテスタブルなコードという意味合いで使っています。

--

### テストが難しいメソッド
例えばこんなのは辛いよね。
返り値がvoid->何を比較すればいい？
オブジェクトが状態を持つ→いつからいつの変化を測ればいい？
DBや複数のオブジェクトに依存している→エラーの原因はどこ？SetUpがめんどくさい
日付・乱数などが入り込んでる。→テストの度に結果が違う
例えば次のテストは大変そうだ。

---

- void

- オブジェクトが状態を持つ
@Test
pub lic void addで要素を追加するとサイ ズがlとなりgetで取得できる()
throws Exception {
ArrayList<String> sut = new ArrayList<String>( );
sut . add{ "Hello") ;
assertThat ( sut . size( )， iS(l) );
assertThat( sut . get(Ð) ， is( "Hello" ));



- 複数のオブジェクトに依存している

- 日付が入り込んでる。
→後述するMockを作成する。

- 外部ファイル- DBに依存している
→Test書けるんですが、今回は省略します。
なのでそれらを使ったメソッドは別に分けて、Mockなどを使うようにしましょう。


---

### Testは簡潔に。
Test自体は目的じゃない
仕様書の代わりとして使える
テスト失敗時に問題を特定しやすくする
何がゴールなのか明確にする（×「正しく」出力される）
Test自体の可読性が悪かったら、何のTestかわからない

---

## 色々あるアノテーション（注釈）
アノテーション
＠Test
　timeout→一定時間を超えたら失敗（速度検証ができる。）
　expected→例外送出の期待値
＠ignore→テストの実行から除外する
＠Before→SetUp用、毎回行われる
＠After→リソースの解放- 毎回行われる
＠BeforeClass→クラス定義時だけ行われる（Statefulになるので使うべきでない）
＠AfterClass→同様 

--

### 色々なメソッド

fail→強制失敗
MatcharAPI→ググれ
hasItems→リストなどにその要素が含まれているか
is
not
nullValue
sameInstance（==と同じ）
instanceOf

他にも色々ありますが（hamcrest-libraryで検索）
基本的にAssertThatだけで良いと思います。
→Collectionの操作（hasKeyとかhasSizeなど）とか、
isEmptyStringとかgreaterThanとか細かくあるけど、
assertThatで事足りるでしょう。


---

## Date型のTest
よく使いそうなので先に作ってしまいましょう。
IsDate.java


日付型
```java
public class IsDate extends BaseMatcher<Date> {
private final int yyyy;
private final int mm;
private final int dd;
IsDate(int yyyy,  int mm,  int dd) {
this.yyyy = yyyy;
this.mm = mm;
this.dd = dd;
}

//compare
@Override
public boolean matches(Object actuál) {
this. actual = actual;
if ( ! (actual instanceof Date ) ) return false;
Calendar cal = Calendar .getInstance( ) ;
cal.setTime( (Date) actual) ;
if ( yyyy != cal. get ( Calendar .YEAR) ) retu rn false;
if ( mm != cal . get(Calendar .MONTH) + 1) return false;
if ( dd != cal . get(Calendar.DATE)) return false;
return t rue; 

//ErrerMassage
@Override
public void describeTo( Description desc) {
desc.appendValue(String.format ( “%d/%02d/%02d” ,  yyyy,  mm, dd));
if ( actual != null) {
desc.appendText( “ but actual is ");
desc.appendValue(
new SimpleDateFormat ( “yyyy/MM/dd").format( (Date) actual ) ); 
}

public static Matcher<Date> dateOf(int yyyy, int mm, int dd) {
return new IsDate(yyyy, mm, dd) ;
} 
```

をまず作成する。

次にこんな感じにしたい
`assertThat(hogeDate,  is(dateOf(2612, 1, 12)));`

dateOfのところが先ほど作ったメソッドです。インスタンスを作って、matchsを元に比較してdescribeToでエラーを吐きます。

---

Testを書くときに
バグが起きやすいところ
境界値
変数の組み合わせのうちの２つまででの不具合（PairWise）
※SlowTest問題
毎回初期化してテストをするのが一番独立していて有効なのだが、それだと時間がかかるorマシンスペックを要求される。そのため、一度作ったフィクスチャを共有することがある。（もちろんテストの独立性は犠牲になる。） テストケースの選択が品質を左右する。

---

応用編

---

### 初期値のセットアップ
メソッドによっては複雑なオブジェクトが入力値に入ることになります。 その値は一つ一つset()などするのは大変だし見にくい。メインの処理が埋もれてしまう など問題があります。
その時には、外部ファイルに記述するかHelperクラスを使いましょう。

---

```java
初期値のセットアップ
BookStoreTest .java

public class BookStoreTest {
  @Test
  public void getTotalPrice() throws Exception {
    // SetUp
    BookStore sut = new BookStore();
    Book book = BookHelper.createNewBook();
    sut.addToCart(book, 1);
    // Exercise & Verify
    assertThat (sut.getTotalPrice(), is(4500)); 
  }
}
```

ここで、BookHelperのメソッドを呼んでオブジェクトを生成しています。 そのおかげでメインのTestメソッドがわかりやすくなっています。

---

初期値のセットアップ
BookStoreTest .java

```java
public class BookStoreTestHelper {
  public static createNewBook() {
    Book book = new Book();
    book.setTitle("Refactoring");
    book.setPrice(4500);
    Author author = new Author();
    author.setFirstName("Martin");
    author.setLastName("Fowler");
    book.setAuthor(author);
    return book; 
  }
}
```
        
HelperMethodはこんなカンジで定義したりです。（オブジェクトによっては外部ファイルへの定義の方が楽そうですがここでは割愛します。）


---

### 複数データに対応したテスト
例えばこんなメソッドがあったとします。


```java
//**
* 優待会員かどうかを返す
* @param age 年齢
* 備考
* 18以上の値
* 18未満の値
* @param isRegisterMailMagazine メ ールマガジンに登録 している場合にtrue
* @param usePastMonth 前月の利用回数
* @return 20歳以上であ り、 メールマガジンに登録 しであ り 、
* かつ前月の利用回数がl回以上ならtrue
*/
public static boolean isSpecialMember(int age,
  boolean isRegisterMailMagazine, int usePastMonth) {
  if ( age < 20) return false;
  if (! isRegisterMailMagazine) return false;
  if (usePastMonth < 1) return false;
  return true;
}
```

これのテストを一つずつ書くのはめんどくさいですよね。複数パターンをまとめて書きたい。

---

複数データに対応したテスト
（先ほどの例と関係なくて恐縮ですが、わかりやすいので別例で） こんな風にパラメータを設定できます。

```java


@RunWith(Theories. class)
public class CalculatorTest {

  @DataPoint
  public static int INT_PARAM1 = 3;
  @DataPoint
  public static int INT_PARAM2 = 4;
  @DataPoint
  public static String STRING_PARAM1 = "Hello" ;
  @DataPoint
  public static String STRING_PARAM2 = "World";
  @Theory
  public void test( int intParam, String strParam)
  throws Exception {
    System.out.println(
    "テストメソッド(" + intParam + " | " + strParam + ")"); 
  }
}
```

```
----result(console)----
テストメソッド(3 | Hello)
テストメソッド(3 | World)
テストメソッド(4 | Hello)
テストメソッド(4 | World)
```

同じ型のパラメータはこんなふうにまとめられ、全組み合わせが実行されます。

---

## テストダブル

さて、これで基本は完了です。
これで何でもいけるとおもいきや、そんな簡単なプログラムだけではありません。

大きく２つ難関があります。

- 内部のprivateメソッドなどを呼んでいる場合
- 外部オブジェクト- メソッドを使っている場合。


順番に行きましょう。

---

### 内部のメソッドを呼んでいる場合


MethodExtractExample.java

```java
public class MethodExtractExample {
  Date date = newDate();
  
  public void doSomething() {
    this.date = newDate();
    // 何らかの処理 (省略)
  }

  private Date newDate() {
    return new Date();
  }
}
```

        
ここでbewDate()が使われています。Test死体メソッドはこの部分の挙動は関与していないため、これでは意図した結果になるか不安が残ります。

---

内部のメソッドを呼んでいる場合
MethodExtractExampleTest.java

```java
public class MethodExtractExampleTest {
  @Test
  public void doSomethingTest() throws Exception {
    final Date current = new Date();
    
    MethodExtractExample sut = new HethodExtractExample() {
      @Override
      Date newDate() {
        return current;
      }
    }
    sut.doSomething();
    assertThat(sut.date, is(samelnstance(current)));
  }
}
```

---

## 外部オブジェクト・メソッドを使っている場合

Mock/stubオブジェクトを使う（Mockitoライブラリ使用） 例えばこんな感じに。

```java
static import ... Mock;
...

List＜String＞ stub = mock(List.class);
when(stub.get(0)).thenReturn("Hello");
when(stub.get(1)).thenReturn("World");
when(stub.get(2)).thenThrow(new IndexOutOfBoundsException());

//Verify
stub.get(0); //Helloが返ってくる。
stub.get(2); //例外が送出される。
```

        
Mockオブジェクトを使うと、そのオブジェクトがどのメソッドを呼んだら 何が返ってくるのか、というのが定義できます（実際にStringを持ってるわけではないハズ）


newでコンストラクタを呼んでますが、そこでnewDate()の書き換えを行っています。 ここで作られたsutのnewDate()メソッドは 元のオブジェクトと違う振る舞いを見せることができるわけです。

---

