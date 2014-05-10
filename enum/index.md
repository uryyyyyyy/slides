
## enumのキホン

 - enumの子はenum
 - enumは**ほぼ**文字列
 - static finalなインスタンス
 - 値を限定できる。（null許容）
 - EnumSetとは
 - EnumMapとは

---

## enumの子はenum

```java
public enum SampleEnum {
	        Item1,
	        Item2;
    }

```
ここで定義されたitem1とかって、
enumの要素みたいに思ってませんでしたか？

実はこいつらもenum型（ここではSampleEnum型）です。だからこんなこともできる。

```java

    System.out.println(SampleEnum.Item1.Item2.Item1.Item1);
```

（eclipseさんに注意されますが特に問題ないはず。**まぁ、こんな書き方する人はいないでしょうが**）

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
これは、Item1のインスタンスを作るときに、引数である１がコンストラクタに入る形です。フィールド変数を増やすことももちろん可能です。


---

## enumは**ほぼ**文字列

```java

    System.out.println(CheckEnum.OK);
    System.out.println(CheckEnum.OK.toString());

```
これらは同じ値を指します。
型は違いますが、内部的には文字列への参照を持ってるんだと思います。

ちなみに、Stringでも同じ文字列は同じ領域を参照しますが、enumも実体は一つなので非常に省エネです。




---

## static finalなインスタンス

enumは実行時に即座に生成され、またインスタンスは増減しないし不変です。

以降は同じ実体への参照が使われていくだけになります。

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

この例では、実行時にコンストラクタが呼ばれ、JPY USDを生成します。
その結果、フィールド変数numberをもったSampleEnum型のインスタンスが2つ生成されるわけです。

**※ただしフィールド変数自体は、finalを付けないと当然可変になってしまいます**

---

### 使い方

 - 特定のパターンだけ列挙したい
 - 過去の実装でintがあるけどなんとかenumにしたい

---

## 特定のパターンだけ列挙したい

例えば、

 - 日付系（1~12月、 干支）
 - 金額系（￥, ＄, €）
 - フラグ系（○○区分、 ○○flag）

などなど。



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

---

## 過去の実装でintがあるけどなんとかenumにしたい

主に区分とかでしょうか。

昔のコード

```java

public static final int CALC_KUBUN_NORMAL = 1;
public static final int CALC_KUBUN_NEW_NORMAL = 2;
public static final int CALC_KUBUN_2014_NORMAL = 3;


```

みたいな。（例が良くないですかね。。。）

--

```java

    public enum CalcDivision {
	        Normal(1);
                NewNormal(2);
                Normal2014(3);
	        
	        //CalcDivisionのフィールド変数
	        public final int divionNum;

                //コンストラクタ
	        private CalcDivision (int divionNum) {
	            this.divionNum = divionNum;
	          }

    public static CalcDivision getEnum(int divionNum) {
	 
	            // enum型全てを取得しループします。
	            for(CalcDivision div : values()) {
	                // 引数intとenum型の文字列部分を比較します。
	                if (divionNum == div.divionNum ){
	                    return div;
	                }
	            }
    //null帰ってきた場合の処理をLogic部で忘れないこと。
	            return null;
	        }
    }

```
