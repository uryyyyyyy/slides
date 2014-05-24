
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

if you use one sentence, like this `int main(void)` dayo-.

## quote

> ぽっぴっぽー by 初音ミク

---

## table

| Left align | Right align | Center align |
|:-----------|------------:|:------------:|
| This       |        This |     This     |
| column     |      column |    column    |
| will       |        will |     will     |
| be         |          be |      be      |
| left       |       right |    center    |
| aligned    |     aligned |   aligned    |

## escape

\# hoge
\- escaped

---

## Image

### ガゾウダヨー

#### ミクダヨー
![Alt Text](../img/miku.png)

## link

[ﾘﾝｸﾀﾞﾖｰ](https://github.com/uryyyyyyy)

[index.md](index.md)

---

## embeded
ビバハピ♪
<iframe width="560" height="315" src="//www.youtube.com/embed/WiUjG9fF3zw" frameborder="0" allowfullscreen></iframe>
