
## 目次

* 日付系の難しさ
* 〜java7
* DateTimeAPIとは？
* おまけ：JodaTime紹介

---

## 日付系の難しさ

* そもそも一貫性がない(月ごとに日数が異なる)
* 様々な基数(12進数、60進数、7進数?(曜日))
* 何の法則もない祝日
* 何の法則もない年号
* 複雑さに輪をかけるタイムゾーン
* 複雑さに輪をかけるうるう年
* 複雑さに輪をかける夏時間

[by 井上さん](http://dev.ariel-networks.com/wp/archives/4186)

--

## 知っておくべき概念

* Locale
	- 時刻に限らず、その地域・国独特の表現などに対応させる指標。
* TimeZone
	- UTCからの時差がどのくらいか（日本なら＋９）
* グレゴリオ暦/ユリウス暦
	- ユリウス暦はうるう年がざっくり。グレゴリオ歴はもっと正確。
* GMT/UTC
	- GMTはグリニッジの時間。UTCはもっと厳密なやつ。普通はUTC


---

## 〜java7

* Date
* Calender
* DateFormat

日付を表すのがDateで、その計算を行うのがCalender。parseなどはDateFormat

--

### Date

> クラス Date は、特定の時点を表すもので、その精度はミリ秒です。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Date.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/Date.java.html) - [Row(JDK1.7_60)](./Date.java)

--

## Calendar

> Calendar クラスは、特定の時点と YEAR、MONTH、〜〜間の変換、および次週の日付の取得など〜〜を行うための abstract クラスです。〜〜(グレゴリオ暦) を元期とするミリ秒単位のオフセットで表現できます。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/util/Calendar.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/util/Calendar.java.html) - [Row(JDK1.7_60)](./Calendar.java)

--

## DateFormat

> DateFormat は、言語に依存しない方法で日付または時刻をフォーマットおよび解析する、日付/時刻フォーマットサブクラスの abstract クラスです。〜〜日付フォーマットは同期化されません。
[JavaAPI](http://docs.oracle.com/javase/jp/7/api/java/text/DateFormat.html) -  [Web(openjdk-7)](http://www.docjar.com/html/api/java/text/DateFormat.java.html) - [Row(JDK1.7_60)](./DateFormat.java)

--

## 何が不便か

* 年月日などを指定したインスタンス生成ができない。
* ちょっとした計算（２つの日付の期間・今月の最終日を求める etc...）がめんどくさい
* 月が０から始まったり、enumでなくLong定数を使ってたりとややこしい。
* DateFormatがスレッドセーフでない
* 状態が可変でややこしい

　

などなど。

---

## DateTimeAPIとは？

> いままでのJavaでは、日付時刻を扱おうとするとめんどくさい割に非常に低機能でした。
Java8では、新たに日付時刻APIが導入され、めんどくささが増しつつ非常に高機能になりました。
[きしだのはてな](http://d.hatena.ne.jp/nowokay/20130917)

　

日時の生成・計算・表示が高機能になった

--

## Date&Time API

* LocalDateTime→タイムゾーンを持たない日時// 2014-08-08T10:11:30
* OffsetDateTime→UTCからの時差を持つ日時// 2014-08-08T10:11:30-09:00
* ZonedDateTime→タイムゾーンを持つ日時 // 2014-08-08T10:11:30-09:00 Japan/Tokyo
* Duration→時間単位の期間 // PT3600S
* Period→日付単位の期間 // P1Y2M3D
* JapaneseDate→和暦

　

とりあえず〜〜DateTimeを使えばいいと思うよ。

OffsetDateTimeよりもZonedDateTimeを使えばいいじゃん。

--

## LocalDateTime

> LocalDateTimeは、日付/時間〜〜を表す不変の日付/時間オブジェクトです。〜〜。時間は、ナノ秒の精度まで表されます。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/time/LocalDateTime.html) -  [Web(openjdk-8)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/time/LocalDateTime.java?av=f) - [Row(JDK1.8_11)](./LocalDateTime.java)


--

## Duration

> 時間ベースの時間(「34.5秒」など)。
このクラスは、秒とナノ秒に基づいて時間量をモデル化します。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/time/Duration.html) -  [Web(openjdk-8)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/time/Duration.java?av=f) - [Row(JDK1.8_11)](./Duration.java)


--

## Period

> ISO-8601暦体系における日付ベースの時間の量(「2年3か月と4日」など)。
このクラスは、時間の量を年、月、および日単位でモデル化します。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/time/Period.html) -  [Web(openjdk-8)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/time/Period.java?av=f) - [Row(JDK1.8_11)](./Period.java)

--

## イメージ図

Duration Periodの計算はこんなことができる。

![./file/duration.png]


--

## JapaneseDate

> この日付は和暦を使用して、運用されます。この暦体系は主に日本で使用されています。
[JavaAPI](http://docs.oracle.com/javase/jp/8/api/java/time/chrono/JapaneseDate.html) -  [Web(openjdk-8)](http://grepcode.com/file/repository.grepcode.com/java/root/jdk/openjdk/8-b132/java/time/chrono/JapaneseDate.java?av=f) - [Row(JDK1.8_11)](./JapaneseDate.java)

### Tips（by大嶋さん）

* 日本は明治6年からグレゴリオ歴を採用した模様。[リンク](http://www.kumamotokokufu-h.ed.jp/kumamoto/bungaku/wa_seireki.html)
	- そのため、明治６年以前の日付は正確に取れないためエラーになる。

* 昭和64年と平成１年とかをちゃんと管理している（〜1989/1/7が昭和）

--

## どう便利か？

* immutableでスレッドセーフなので安心して使える。
* メソッドチェーン的なこともできる。
* 日付だけ、日時だけ、とかを選べる。
* ZonedDateTimeは、サマータイムとかも管理してるっぽい。

　

実例はコードの方で。

---

## オマケ：Joda-Time

--

* DateTime→JavaのCalendarにあたるクラス
* DateTimeMidnight→DateTimeの時間を00:00:00.000に固定したクラス（@deprecated）
* LocalDateTime→ロケールを含まず、日付と時間を扱うクラス（Dateのみ、Timeのみも）

* Duration→日時の差を扱うクラス
* Period→年月日時分秒毎に日時の差を扱う
* Interval→2つの期間の差を扱うクラス


---

## Any Question?

![alt](./file/bakadana.jpg)

> [ニコニコ静画](http://seiga.nicovideo.jp/seiga/im785518)

---

