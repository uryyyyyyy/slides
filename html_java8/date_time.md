
## 目次

* 日付系の難しさ
* DateTimeAPIとは？
* 〜Java1.7との違い
* 詳解
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

Locale

TimeZone

グレゴリオ暦/ユリウス暦

GMT（Greenwich Mean Time）/UTC（Universal Time, Coordinated）

---

## 〜java1.7

Date
Calender
DateFormat

日付を表すのがDateで、その計算を行うのがCalender

mutableで汎用性に欠けるらしい

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

何が不便か

* 汎用的な計算（２つの日付の期間・今月の最終日を求める etc...）がしにくい
* 月が０から始まったり、enumでなくLong定数を使ってたりとややこしい。
* スレッドセーフでない
* 状態が可変でややこしい


---


## DateTimeAPIとは？







## Joda-Time

DateTime→JavaのCalendarにあたるクラス
DateTimeMidnight→DateTimeの時間を00:00:00.000に固定したクラス
LocalDateTime→ロケールを含まず、日付と時間を扱うクラス（Dateのみ、Timeのみも）

Duration→日時の差を扱うクラス
Period→年月日時分秒毎に日時の差を扱う
Interval→2つの期間の差を扱うクラス


## 1.8

Date&Time API

LocalDateTime→タイムゾーンを持たない日時（Dateのみ、Timeのみも） // 2014-08-08T10:11:30
OffsetDateTime→UTCからの時差を持つ日時（Timeのみも） // 2014-08-08T10:11:30-09:00
ZonedDateTime→タイムゾーンを持つ日時 // 2014-08-08T10:11:30-09:00 Japan/Tokyo
Duration→時間単位の期間 // PT3600S
Period→日付単位の期間 // P1Y2M3D
JapaneseDate→和暦


OffsetDateTimeよりもZonedDateTimeを使うのが一般的っぽい

## 具体例

### 現在日時を取得する

Date now = new Date();
// Sat Apr 06 20:17:25 JST 2002

Calender cal = Calender.getInstance("Locale.??");
Date now = cal.getTime();



### 年月日を操作する



---

## Any Question?

![alt](./file/bakadana.jpg)

> [ニコニコ静画](http://seiga.nicovideo.jp/seiga/im785518)

---


## 参考文献


http://www.rsch.tuis.ac.jp/~mizutani/online/software-basic2002/date.html

http://shinsuke789.hatenablog.jp/entry/2013/08/05/120042
http://shinsuke789.hatenablog.jp/entry/2013/08/06/195436

http://acro-engineer.hatenablog.com/entry/20130213/1360691391
