
## enumのキホン


 - 何が嬉しいのか？
 - enumの子はenum
 - enumは**ほぼ**文字列
 - static finalなインスタンス
 - EnumSetとは
 - EnumMapとは

---

## 何が嬉しいのか？

enumを使うと、あるグループで列挙される値を限定できます。

例えば、
```java
public enum Fruits {
	        Apple,
	        Orange,
	        Peach;
    }
```

なら、この三種類以外はFruitsと認めません。

限定しないと、

- 「俺はスイカは果物でいいと思う」
- 「いちごは果物じゃないと聞いたけど」

なんて個々人で好きなように定義してしまい、収集がつかなくなってしまうためです。

（※ただし、nullが入らないようにできないのがJavaの残念なところ。）

---

## enumの子はenum

```java
public enum SampleEnum {
	        Item1,
	        Item2;
    }
```
ここで定義されたitem1らは、
SampleEnumのインスタンスです。

型が同じだから、例えばこんなこともできる。

```java
    System.out.println(SampleEnum.Item1.Item2.Item1.Item1);
```

（eclipseさんに注意されますが特に問題ないはず。まぁ、こんな書き方する人はいないでしょうが。。）

--

また、こんなこともできます。

```java
    public enum SampleEnum {
            //enum型のインスタンスを羅列
	        Item1(1),
	        Item2(2);

            //SampleEnumのフィールド変数
	        public final int number;

            //コンストラクタ
	        private SampleEnum (int number) {
	            this.number = number;
	          }
    }

```
これは、Item1のインスタンスを作るときに、
引数である１がコンストラクタに入って、
フィールド変数に格納される形です。
Item1もItem2もそれぞれ別のフィールド変数を持ちます。

フィールド変数を増やすことももちろん可能です。
`item1(1, "1dayo")`みたいな。


---

## enumは**ほぼ**文字列

```java
    System.out.println(CheckEnum.Item1);
    System.out.println(CheckEnum.Item1.toString());
```
これらは同じ値（Item1）を指します。
型は違いますが、内部的には文字列への参照を持ってるんだと思います。

ちなみに、Stringは同じ文字列は同じ領域を参照するようにコンパイルされますが、
enumも実体は一つなので非常に省エネです。


---

## static finalなインスタンス

enumは実行時に即座に生成され、
またインスタンスの数は増減しません。

実行中は同じ実体への参照が使われていくだけになります。

```java
    public enum SampleEnum {
	     JPY("￥", 1.08),
		 USD("＄", 0.00);
	        
	     public final String mark;
	     public final BigDecimal taxRate;
	        
	     private CheckEnum (String mark, double taxRate) {
	            this.rate = BigDecimal.valueOf(taxRate);
	            this.mark = mark;
	     }
    }
```

この例では、実行時にコンストラクタが呼ばれ、JPY, USDを生成します。
繰り返しですが、その結果、フィールド変数をもった`SampleEnum`型のインスタンスが2つ生成されるわけです。

**※ただしフィールド変数自体は、finalを付けないと当然可変になってしまいます**

---

## EnumSetとは

setをEnumで扱いやすいように実装したもの。

setにはないいろんな便利メソッドが定義されている。

例：
```java
enum Flag { FLAG0, FLAG1, FLAG2, FLAG3 }

		EnumSet<Flag> flag = EnumSet.of(FLAG0, FLAG1, FLAG3);
		if (flag.contains(FLAG0)) {
			System.out.println("フラグ0が立っている");
		}
```

EnumでSetを使いたいときはHashSetなどより
実行効率がよく、メソッドが追加されていて便利なようです。

---

## EnumMapとは

EnumをキーにしたMap。HashMapより実行効率が良い。
<small>（列挙子の個数が固定なので、序数を使った配列で実装されている）</small>

```java
Map<Flag, String> map = new EnumMap<Flag, String>(Flag.class);
	map.put(FLAG0, "000");
	...
	map.get(FLAG0); //"000"
```

---


## 使い方の例

 - 一般的なパターン
 - 過去の実装でintがあるけどなんとかenumにしたい

---

## 一般的なパターン

例えば、

 - 日付系（1~12月、 干支、 曜日）
 - 金額系（￥, ＄, €）
 - フラグ系（○○区分、 ○○flag）

などなど。


例：
```java
    public enum Currency {
	     JPY("￥", 1.08),
		 USD("＄", 0.00);
	        
	     public final String mark;
	     public final BigDecimal taxRate;
	        
	     private CheckEnum (String mark, double taxRate) {
	            this.rate = BigDecimal.valueOf(taxRate);
	            this.mark = mark;
	     }
    }

```

使い方：
```java
    System.out.println(Currency.JPY.mark + (price * Currency.JPY.rate)
    //\108
```

---

## 過去の実装でintがあるけどなんとかenumにしたい

主に区分とかFlagでしょうか。

昔のコード

```java
public static final int CALC_KUBUN_NORMAL = 1;
public static final int CALC_KUBUN_NEW_NORMAL = 2;
public static final int CALC_KUBUN_2014_NORMAL = 3;
```

みたいな。（例が良くないですかね。。。）

--

上記のをEnumに改良したVersion

```java
   public enum CheckEnum {
    	Normal(1),
    	NewNormal(2),
    	Normal2014(3);

    	//フィールド変数
    	public final int kubunInt;

    	private CheckEnum (int kubunInt) {
    		this.kubunInt = kubunInt;
    	}

    	//昔のIntからEnumを取ってくるときに使うMap。実行時に生成される。
    	private static final Map<Integer, CheckEnum> checkMap = init();
    	private static Map<Integer, CheckEnum> init() {
    		Map<Integer, CheckEnum> init = new HashMap<Integer, CheckEnum>();
    		init.put(CheckEnum.Normal.kubunInt, CheckEnum.Normal);
    		init.put(CheckEnum.NewNormal.kubunInt, CheckEnum.NewNormal);
    		init.put(CheckEnum.Normal2014.kubunInt, CheckEnum.Normal2014);
    		return init;
    	}

    	//これでintでEnumが釣れる
    	public static CheckEnum getEnum(int kubunInt) {
    		return checkMap.get(kubunInt);
    	}
    }

}
```

使い方：
```java
        //Enumからintへの変換
        int i = CheckEnum.NewNormal.kubunInt;
        
        //intからEnumへの変換
        CheckEnum.getEnum(i);
```
