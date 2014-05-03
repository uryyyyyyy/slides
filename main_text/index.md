# 目次

- いつか作る
- やっぱ作らないかもしれない。

---

ヨコダヨー

--

シタダヨー


---

## code

```java:sample.java
public static void main(String[] args) {
		try{
			final long startTime = System.currentTimeMillis();

			System.out.println("入力値");
			List<Integer> inputs = load(args);
			List<Integer> outputs = execute(inputs);
			System.out.println("出力値");
			write(outputs);

			final long endTime = System.currentTimeMillis();
			System.out.println(endTime - startTime + "ms かかりました。");
		}catch(Exception e){
			System.out.println("エラーです。ログを確認してください。");
		}
	}

```

## quote

> quoted words

---

## Image

### ガゾウダヨー

#### ミクジャナイヨー
![Alt Text](img/hako_shiba_bigger.png)

## link

[ﾘﾝｸﾀﾞﾖｰ](https://github.com/uryyyyyyy)