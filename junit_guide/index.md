
# 目次

## なぜテスト駆動開発か。

なぜテストを尽くさないのか。

## テストを行うか行わないか
進研ゼミ方式
テストを怠ったAさん
「普段の業務で忙しくてテストしている時間がなくて、
　そんで後回しにしていたら前よりもヒドイものが出来てしまった。
　おまけに女の子にも『もっとテストしとけばよかった』とふられました」

テストを頑張ったBさん
「思い切ってTDDにチャレンジしたおかげで実装の効率も上がったし、
　テストの点数も上がったし上司にも褒められるし、スキルも上がっていますし、
　仕事でもレギュラーに選ばれるし、さらには彼女も出来ました。」 


### そもそもの目的

- 本来の理想は保守の効率化。
- そのためには、疎結合やレイヤー分けをして拡張性を確保すること、
- 過去の実装を作り変えていくこと、かつデグレが発生しないようにすること、

これらを必然的に満たしてくれるのがTDD


## 当面の目標。

- テストをササッと書けるようになる。
- 実装の前に、テストを意識した設計とゴールの明確化ができるようになる。
- 自分の実装の出来や想定の足りなさがよくわかってPDCAが回せる。
- 実装後、修正後にテストを見せることで、周囲の人の評価アップ
- 転職する際もテストの経験は活きるはず。

### 実業務の想定。
- 設計に時間をかけられるようになる。
- 全てにテストを書けとは言わない
- 実装が不安なところはテストを書いて安心する。
- コメントを添えるよりもテストで示したほうがわかりやすい。
- テストする必要がないようなシンプルな記述を心がけるようになる。


## 目指すコードのカタチ

- グローバル変数の駆逐
- StateLessなオブジェクト
- 疎結合- 依存関係がすっきりしている。
- 関数的（いつ実行しても結果が変わらない）
- 関数的２（再利用できる小さなメソッドを作る）
- 仕様書、動作説明書となる


## テスト駆動開発のサイクル
- 設計する
- テストコード書く
- プロダクトコード書く（テストをクリアする）
- リファクタリングする

このプロセスを通じて保守性の高いコードを身につける。


## もしかして→”めんどくさい”?
- 工数が増えては意味がない。長期的に考えて楽を目指す。
- テストをしなくてもわかるような設計- 記述が理想
- 評価- 保守における単純作業は自動化してしまおう。
- 慣れないのは、プログラマ脳になりきれていないから。
- 汚い- 地雷だらけのコードの中で仕事したくない。

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
 

真似たほうが早いでしょう
- Sample1
```java


    public int multiply(int x, int y) {
        return x * y;
    }
```

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

ひたすらにこのカタチを真似ましょう。

次に例外を。

```java
    public float divide(int x, int y) {
        if (y == 0) throw new IllegalArgumentException("divide by zero.");
        return (float) x / (float) y;
    }


@Test
    public void divideで3と2の除算結果が取得できる() {
        Calculator calc = new Calculator();
        float expected = 1.5f;
        float actual = calc.divide(3, 2);
        assertThat(actual, is(expected));
    }

    @Test(expected = IllegalArgumentException.class)
    public void divideで5と0のときIllegalArgumentExceptionを送出する() {
        Calculator calc = new Calculator();
        calc.divide(5, 0);
    }
```

@Testについてるexpectedがメソッドの期待値です。

テストに相応しい値の探し方（C0や境界値など）はみんな理解してると思うので割愛）

---

さて、改めて確認です。

良い設計
- 細かく分割されている→問題が局所化されること
- 関数的である→入力に対しての出力が不変
- 関数的である２→オブジェクトが状態を持たない。
- 依存関係が少ない。簡潔である。

--

### 例えばこんなのは辛いよね。

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

### テストは明確に
- ドキュメントの代わりとして
- テスト失敗時に問題を特定しやすいこと
- 可読性の低いテストは避け、何をしてるかわかりやすくすること。
- 何がゴールなのかめいかくにする（×正しく出力される）

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


---

### テストの記述- 構造化の仕方。
機能ベースでまとめていくプロダクトコードと違い、
初期化が一般化できるかどうかで記述するのがテストコード。
初期状態によってインナークラスを作って分けていくなど（p97）
コンストラクタメソッドも別クラスとして設けておく

- テストランナー
バグが起きやすいところ
- 境界値
- ２つまでの組み合わせ（PairWise）
全ての組み合わせ、パターンを実行することは不可能
テストケースの選択が品質を左右する。 

--

### ※SlowTest問題
毎回初期化してテストをするのが一番独立していて有効なのだが、それだと時間がかかるorマシンスペックを要求される。そのため、一度作ったフィクスチャを共有することがある。（もちろんテストの独立性は犠牲になる。） 

テストは一般にそれぞれ独立していて、
初期化処理も個別に行うのが望ましい。
が、複数抱えると途端にパンクする。
から個別に分けていくことが必要らしいのだけど、
現状は必要性を感じないので省略。
@ignoreだけ覚えておけばいいのでは。

---

### 初期値のセットアップがめんどくさいとき。

初期値のセットアップ、
ものによっては複雑なオブジェクトを組むことになります。
その時には外部ファイルに記述するか、
Helperクラスを使いましょう。

```java
public class BookStoreTest {
@Test
public void getTotalPrice() throws Exception {
/ / SetUp
BookStore sut = new BookStore ( );
Book book = BookHelper.createNewBook( );
sut .addToCart( book, 1);
/ / Exercise & Verify
assertThat ( sut.getTotalPrice( ),  is (4500)); 


public class BookStoreTestHelper {
pub lic static createNewBook() {
Book book = new Book () ;
book. setTi tle( "Refactoring") ;
book. setPrice(4500) ;
Author author = new Author( ) ;
author. setFirstName( "Martin");
author. setLastName(" Fowler") ;
book. setAuthor(author) ;
return book; 
```

---

### パラメータ化テスト

例えばこんなカタチ

```java
/材
* 優待会員かどうかを返す
* @param age 年齢
備考
18以上の値
18未満の値
* @param isRegisterMailMagazine メ ールマガジンに登録 している場合にtrue
* @param usePastMonth 前月の利用回数
* @return 20歳以上であ り、 メールマガジンに登録 しであ り 、
本 かつ前月の利用回数がl回以上ならtrue
*/
public static boolean isSpecialMember(
int age, boolean isRegisterMailMagazine, int usePastMonth) {
if ( age < 20) return false;
if (! isRegisterMailMagazine) return false;
if (usePastMonth < 1) return false;
return true; 

```

というとき、テストを複数書くのはめんどくさい
最低でもこんなテストケースがありそう。

キャプチャ

（ここで、テストケースを元に例外処理を考えるのも安全かもしれない。）

```java
public class Age {
public final int value;
public Age(int value) {
if (value < 0 II 150 <= value)
throw new IllegalArgumentException ( ) ;
this. value = value; 
```


### 複数データに対応したテスト

```java

@Datapoint

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
"テストメソッド(" + intParam + " I " + strParam + ")");
}
```

result:

```java

テストメソッド(3 I Hello)
テストメソッド(3 I World)
テストメソッド(4 I Hello)
テストメソッド(4 I World)

```

void型の場合、オブジェクトが状態を持ってしまっているので、
それを確認するようのメソッドなどを呼んであげる必要があります。
（確認のしようがない？じゃぁそのvoid型のメソッドは無意味といういことになるよ。

--

## テストダブル

---

さて、これで基本は完了です。
これで何でもいけるとおもいきや、そんな簡単なプログラムだけではありません。

大きく２つ難関があります。

- 内部のprivateメソッドなどを呼んでいる場合
- 外部オブジェクト- メソッドを使っている場合。


順番に行きましょう。

---

### 内部のメソッドを呼んでいる場合

例えば
```java
public class MethodExtractExample {
Date date = newDate();
public void doSomething() {
this. date = newDate();
// 何らかの処理 (省略)
Date newDate() {
return new Date( ) ;
```

内部のメソッドをオーバーライド（書き換え）することができます

```java
public class MethodExtractExampleTest {
@Test
public void doSomethingTest ( )
throws Exception {
final Date current = new Date( );
MethodExtractExample sut = new HethodExtractExample() {
@Override
Date newDate() {
return current;
sut.doSomething( );
assertThat ( sut.date, is(samelnstance(current)))i
```

---

### 外部オブジェクト- メソッドを使っている場合。

Mock/stubオブジェクトを使う（Mockitoライブラリ使用）
例えばこんな感じに。

```java
List<String> stub = mock (List . class) ;
when{ stub. get (0) ).thenReturn("Hello") ;
when( stub.get( l) ) . thenReturn("World" ) ;
when( stub. get(2) ) .thenThrow(new IndexOutOfBoundsException());

//Verify
stub.get(0) //Helloが返ってくる。
stub.get(2); //例外が送出される
```

---

### 省略した内容
Spy
DBUnit
カバレッジ
自動ビルド

---

オフライン勉強会で行うもの（予定）

- テストコード記述の実践
- 振る舞い駆動開発（シナリオテスト）
cucumber- Jnario


http://www.slideshare.net/shuji_w6e/junit-xunittestpatterns

---

### 実務におけるTips
→クラスを設計する際

http://www.slideshare.net/shuji_w6e/junit-xunittestpatterns
QuickJUnitで簡単にテストクラスが作れる（義務化しましょう。） 


---

アノテーション対応した4

ヘロク

大文字プリミティブは必要か？ボクシングしてくれるのでは？

１８６P
