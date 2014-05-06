
### 1 コンストラクタよりstaticファクトリーメソッドを検討する。

####理由
- 特定のシグニチャを持つコンストラクタは一つだけ、の制約を逃れる。

例）

```java

public Sample(int i) {
		this.i = i;
}

```

- 同じ構造（上記なら引数がintのコンストラクタ）は作れない。

--

- 何度も必要になるものがある場合、インスタンスを使いまわせる。**flyweightパターン**

（ただし、immutableでないとどこで修正されたかわからなくてカオスになる。）

その場合は==で比較できたりもする。

- 欠点は、他のスタティックメソッドと区別がつきにくいこと。
　（うっかり呼んで、めちゃめちゃ生成コストかかったり。）

--

#### コメント
覚えるの必須。*シングルトンパターン*でよく使われます。

VOやDTOなら普通はコンストラクタだけで足りると思うけども。。

ちなみに、一般的な命名規約があるので守りましょう。

- valueOf パラメ ータと同じ値を持つインスタ ンスを返します。 
- getlnstance 型は同じだけど値は同じとは限らない
- newlnstance インスタンスが全て新しく作られる


---

### 2 数多くのパラメータのコンストラクタはBuilderパターンを検討する。

#### 理由 同じ型を大量につめ込むと間違いやすくなる

例えばこんなの

```java
public Sample(int servingSize, int servings, 
int calories, int fat, int sodium, int carbohydrate) { 
this.servingSize = servingSize;
this.servings = servings; 
this.calories = calories; 
this.fat = fat;
this.sodium = sodium; 
this.carbohydrate = carbohydrate;
 }

new Sample(servingSize, servings, 0, 0, 0, 0); 


```

--

#### コメント
一度に値を入れられない時は、そもそも1つのクラスで処理できるかを疑うべきでしょう。
その上で、同じ型が並んでわかりにくくなる時は、プリミティブをラップしたクラス（VO）を作ればいいじゃん。

例
```java
public Sample(ServingSize servingSize, Servings servings, 
calories calories, Fat fat, Sodium sodium, carbohydrate carbohydrate) { 
this.servingSize = servingSize;
this.servings = servings; 
this.calories = calories; 
this.fat = fat;
this.sodium = sodium; 
this.carbohydrate = carbohydrate;
 }

```

ServingSizeクラスとかってどう作るのかって？
[こんな感じ](https://github.com/uryyyyyyy/primeChecker/blob/master/src/com/example/vo/PositiveInt.java)


---

### 3 privateのコンストラクタかenumでSingleton特性を強制する

#### 理由
うっかりSingletonを崩してしまう可能性を排除し、一意性を確実にする。

もしくはユーティリティクラスのインスタンスを生成させない。

（Singletonを崩すと、データの不整合とか起きやすくなるため。）

--

#### コメント
前述のもの（項目１）で十分

[シリアライズ攻撃](https://www.jpcert.or.jp/java-rules/ser04-j.html)とかリフレクションは、興味があれば見て下さい。

---


### 4 privateのコンストラクタでインスタンス化不可能を強制する

#### 理由
ユーティリティクラス（フィールド変数が全てstaticなもの）はインスタンスを作る必要がないので、インスタンス化できないようにしておく。

#### コメント 
なくても害はないが、他人が自分のクラスを使う際に混乱するのを防げる。


---

### 5 不必要なオブジェク卜の生成を避ける

#### 理由：
インスタンスの生成にはコストがかかるから。（CPU・メモリ的に。）

良くない例）

```java

String s = new String("文字だよー" );  //二個インスタンスが作られる


public void method(){
  //some logic
  String moke = "定数"  //定数を何度も作るのは無駄。
  //some logic
}


```

他にも、定数なのに毎回作りなおしているものとか。


--

#### コメント：
そもそも記述としてわかりにくくなりがちなので、
不要な記述は省こう。
定数にできるものは定数にしよう。
オブジェクトがimmutableであれば安心して再利用できる。


--

### コラム ボクシング（boxing）

以下のプログラムは実行効率が悪い

```java

public static void main(String[] args) { 
Long sum = OL; //こいつが悪い
for (long i = 0; i <= Integer.MAX_VALUE; i++) { 
sum += i; 
} 
System.out.println(sum) ; 

```

ボクシングされた基本データ型よりも基本データ型を選ぶ。

--

#### Listにプリミティブが入らない理由。
Listとかは、参照型しか確か入らないようにAPIが設計されてるのではないか。
配列の場合は、物理メモリを確保しておくのでプリミティブでも大丈夫。
その代わり型安全性が確保されない（物理メモリを使いまわす）のでNG

---


### ６ 廃れたオブジェク卜参照を取り除く

####理由
メモリリークの原因になりうるから。

####コメント
配列使わなければ問題ない。
そんで配列は使うな。
Collection系の場合はきちんとremove()しよう。

---

### 7 ファイナライザを避ける
#### 理由
本当に実行される保証がないから
#### コメント
GCが走る際に実行されるメソッド。
そもそも知らなくていいので問題ない。
