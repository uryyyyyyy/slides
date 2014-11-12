
## Streamとは？

データを読み書きできる性質、を抜き取ったもの。
ファイルだけでなく、ネットワーク、メモリアクセスなど、データの流れは何でも取り扱えるよう抽象化されている。

（詳しくないけど、Java8のStreamAPIとは別物？）

---

### 種類

- 抽象基底クラス

|  | 入力ストリーム | 出力ストリーム |
|:-----------|------------:|:------------:|
| バイト単位 | InputStream | OutputStream |
| 文字列単位 | Reader | Writer |

- 継承関係

![alt](.img//java_stream.jpg)

> http://blog.csdn.net/sfdev/article/details/3771134

---

## 具体的な事例集


---

## Fileから文字列を読み込む場合

流れとして、

1. FileのPath、名称を取得する
2. そのFileが存在するか確認する。（isExist）
3. それがファイルか（Directoryでないか）確認する(isFile)
4. Fileが読み込み可能か確認する(canRead)

```java
class streamTest2{
  public static void main(String args[]){
    try{
      File file = new File("c:¥¥tmp¥¥test.txt");

      if (checkBeforeReadfile(file)){
        FileReader filereader = new FileReader(file);

        int ch;
        //read()はint（中身はByteコード）を返す。ファイルの終端では-1を返す。
        while((ch = filereader.read()) != -1){
        
          //int（中身はByteコード）を文字に変換する
          System.out.print((char)ch);
        }

        filereader.close();
      }else{
        System.out.println("IO Error");
      }
    }catch(FileNotFoundException e){
      System.out.println(e);
    }catch(IOException e){
      System.out.println(e);
    }
  }

  //Fileが存在するか・ディレクトリでないか・読み込み可能か確認する。
  private static boolean checkBeforeReadfile(File file){
    if (file.exists()){
      if (file.isFile() && file.canRead()){
        return true;
      }
    }

    return false;
  }
}
```

---

## Fileへ文字列を書き込む場合

```java
class streamTest4{
  public static void main(String args[]){
    try{
      File file = new File("c:¥¥tmp¥¥test.txt");

      if (checkBeforeWritefile(file)){
        FileWriter filewriter = new FileWriter(file);

        filewriter.write("一行目¥r¥n");
        filewriter.write("二行目¥r¥n");

        filewriter.close();
      }else{
        System.out.println("IO Error");
      }
    }catch(IOException e){
      System.out.println(e);
    }
  }

  private static boolean checkBeforeWritefile(File file){
    if (file.exists()){
      if (file.isFile() && file.canWrite()){
        return true;
      }
    }

    return false;
  }
}
```

---

## FileからByteを読み込む



---

## Byteコード経由で文字列を読み取る場合

なぜこのようなことをするかというと、

* 文字のEncodeが標準でない場合
* socket経由など、いきなり文字列Streamにできない場合。

---

## Fileへの書き込み（文字列）

```java
try{
  File file = new File("c:¥¥tmp¥¥test.txt");
  FileWriter filewriter = new FileWriter(file, [true]);

  filewriter.write("こんにちは");

  filewriter.close();
}catch(IOException e){
  System.out.println(e);
}
```

---

## ネットワークとのやりとり。

ネットワークとのやりとりはsocketというオブジェクトへストリームを流し込む形で行います。

ここでは概要だけにします。

* 入出力はByteコードで行う。文字列の場合は別途変換する
* Bufferd〜〜を用いると出力のタイミングが未定なので、明示的に返すメソッド`fluch()`を呼ぶ。

```java
//サーバー側の例です

// サーバーソケットの生成
ServerSocket serverSocket = new ServerSocket(80/*port_number*/);

// クライアントからの接続を待ちます
socket socket = serverSocket.accept();
system.out.println(socket.getInetAddress() + "接続受付");

// 出力ストリームを取得
OutputStream os = socket.getOutputStream();
BufferedWriter out = new BufferedWriter(new OutputStreamWriter(os));

// 入力ストリームを取得
InputStream is = socket.getInputStream();
BufferedReader in = new BufferedReader(new InputStreamReader(is));

```

---

## 便利なクラス

### Bufferd系

一文字ずつ読んでいくのではなく、Bufferとしてメモリに確保しておくので、
DiskアクセスによるIO遅延が少なくなる。

Bufferのサイズはコンストラクタ生成時に決められる。

（ネットワーク系など、即時性が必須な場合以外はこれは使っておくべし）

```java
class streamTest3{
  public static void main(String args[]){
    try{
      File file = new File("c:¥¥tmp¥¥test.txt");

      if (checkBeforeReadfile(file)){
        BufferedReader br = new BufferedReader(new FileReader(file));

        String str;
        while((str = br.readLine()) != null){
          System.out.println(str);
        }

        br.close();
      }else{
        System.out.println("ファイルが見つからないか開けません");
      }
    }catch(FileNotFoundException e){
      System.out.println(e);
    }catch(IOException e){
      System.out.println(e);
    }
  }
```

--

### PrintWriter

Printの処理をよろしくやってくれるもの。

System.outと同じことが出来る。

```java
class streamTest7{
  public static void main(String args[]){
    try{
      File file = new File("c:¥¥tmp¥¥test.txt");

      if (checkBeforeWritefile(file)){
        PrintWriter pw = new PrintWriter(new BufferedWriter(new FileWriter(file)));

        pw.println("文字列も");
        pw.print("数字も");
        pw.println(10);
        pw.println(true);
        pw.println("いろんな型を入れられる。");

        pw.close();
      }else{
        System.out.println("ファイルに書き込めません");
      }
    }catch(IOException e){
      System.out.println(e);
    }
  }

```

--

## RandomAccessFile

読み書きの途中の位置を記録しておけるため、細かな処理が行い易い。

（ただし、上手く指定してやらないと予想外の動作をするかも。）

```java

//RandomAccessFileクラスのコンストラクタの第1引数に
//Fileオブジェクトfile1を指定し、第2引数に読み込み専用モード"r"を指定します。
File file1 = new File("abc.txt");
RandomAccessFile randomfile1 = new RandomAccessFile(file1, "r");

//第2引数に読み書き両用モード"rw"を指定します
RandomAccessFile randomfile2 = new RandomAccessFile("xyz.txt", "rw");

```

--

## 標準入出力(System.in/out)

System.inはInputStreamで帰ってくる。
つまりByteコードなのですが、普通は文字列が入力されるので、いい感じにラップしてあげる。

System.outはPrintWriterで帰ってくる。
先ほど紹介したように、既にイイカンジにラップされている。


---

## 場合別の使われ方

Fileから文字列を取得


どうしても早く保存したい場合。


---

## 参考文献

* PerfectJava
* http://www.javadrive.jp/start/stream/


