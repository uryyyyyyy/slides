
## アジェンダ

* 例外の種類
* 実行時例外
* 検査例外
* 使い方

---

### 例外の種類

![alt](./img/exception_types.png)

Errorは無視していいです。（自分で実装する機会があるとは思えないから。）

---

### 実行時例外

java.lang.RuntimeException以下の例外

コンパイル時にひっかからないため、
通常は対処不可能な場合に用いる。

---

### 検査例外

java.lang.Exceptionのうち、Runtimeでないもの。

コンパイル時にひっかかるため、プログラマの方できちんと定義してやる必要がある。

逆に実行時にこれらの例外を気にしなくて良い。
（もう処理が記述されているはずだから。）

---


### 使い方

* 「例外」にだけ例外を用いる
* 回復可能な例外はチェック例外を、それ以外は実行時例外を。
* 例外を全て記述する。
* 詳細メッセージにエラー情報を詰める。
* エラーアトミック性に努める。
* 例外を無視しない
* アサートを使う。


---

### 「例外」にだけ例外を用いる

処理ロジックの中に例外を混ぜると、最適化が行われず実行効率が落ちることが多い。

また、他の例外と区別がつかなくなって読む人が混乱する。

```java
	try {
		int i = 0;
		while(true){
			range[i++].climb();
		}
	} catch (ArrayIndexOutOfBoundsException e) {
	//Nothing
	}

```

`if-else`で代替できるならその方が理解しやすい（JVMも人も）

---

### 回復可能な例外はチェック例外を、それ以外は実行時例外を。

Errorは無視。回復可能だろうと考えるものは、チェック例外にして処理の記述を強制されるべき。

逆に復旧できない例外については、処理を強制されてもどうしようもないので、無視できる実行時例外に流す。

---

### 適切な例外にして返す。
メソッド内で何をやっているかに興味はなく、そのメソッドを呼び出して想定される例外を投げる。

例：
```java
String get(int index){
	try{
	//
	} catch (NoSuchElementsException e) {
		throw new IndexOutOfBoundsException("Reason");
	} 
}

```

このメソッドだったら、そのindexの場所に値が入ってるかを知りたいので、
「要素がない」、ではなくて「そのIndexは範囲外」の方が意味が通っている。。

---


### 例外を全て記述する。

実行時例外もJavaDocに書いてあげると、
「あ、このメソッドにこんな値渡すとヤバそうだな」とかが事前にわかる。

メソッドの中身を読まなくても済むようにするのがオブジェクト志向の原則。

---

### 詳細メッセージにエラー情報を詰める。

コンストラクタに入れておくと強制できる。

`SampleException.java`
```java
public class SampleException extends Exception{
	private static final long serialVersionUID = -9029774938234791120L;
	private final String msg;
	
	SampleException(String msg){
		super(msg);
		this.msg = msg;	
	}

	public String getMsg() {
		return msg;
	}
}
```

`Main.java`
```java
public static void main(String[] args) {
		try {
			throw new SampleException("Error dayo-");
		} catch (SampleException e) {
			e.printStackTrace();
			System.out.println(e.getMsg());
		}
	}
```

--

この場合、スタックトレースでもprintlnでも`Error dayo-`が出力されます。

このSampleExceptionはコンストラクタに文字列の引数を強制しているので、呼び出し側は
必ずErrorMsgの記述をしなければいけない。

標準の例外でももちろんメッセージや変数の値は添えるておくべき
---

### エラーアトミック性に努める。

エラー後にデータ不整合にならないのが望ましい。

実現方法として、事前にチェックを済ませておくのが良い。**最悪の場合書き戻すことが必要**

※DBとのやりとりはTransactionで制御して、フィールド変数はImmutableならほぼ問題なし。

---

### 例外を無視しない

チェック例外を握りつぶすのは９９％アンチパターン。

きちんとエラー処理をするか、もしどうしても記述できないなら、

- 上に伝播させる
- チェック例外でなく実行時例外になるようにメソッドを修正する。

---

### アサートを使う。

おまけですが、assertというのがあります。
他にも、

- guavaのCheckNotNull()
- lombokの@NotNull

などもあります。**（詳しくはググれ）**

これらは例外の検知というより、「当然この値だよね」というドキュメントの機能を果たします。


