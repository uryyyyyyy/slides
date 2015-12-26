
# 2015 done

- moved into this house.
- slack bot
- EHH hackason
- Infrared(赤外線） module with RaspberryPi

---

## moved into this house

who am I?（自己紹介）

柴田 幸輝（shibata_ko）

- Java
- Scala
- AWS
- JS(nodeJS, Typescript, ScalaJS, React, Babel)

インフラエンジニアになりたい☆彡

---

## slack bot

slack webhook ＆ AWS Lambdaで作成

- ゴミの日通知
  - 前日21時にslackへお知らせ。
  - ぶっちゃけ毎週の繰り返しだけならslackのremind機能でもいけた。
  - 月一の不燃ごみのときだけ使える？
  - 完了したらマークする or 担当者のTodoに放り込むなど改良したい。

- 天気予報お知らせ
  - 当日雨予報（50%以上）だと時間ごとの降水量と共に通知。
  - IFTTTとかでもそれっぽいのはあったけど、詳細を知れて重宝している。

---

## EHH hackason

遠隔操作できるコンセントを作った。

- ESP8266がコンセント側に埋め込まれている
- SoracomでAWS IOTへつなぎ、変更があると受け取ってコンセントの挙動を変える。
- ブラウザで操作するとAWS IoTに変更通知が飛ぶ。

---

## EHH hackason

![alt](./img/anko_chan.png)

---

## Infrared module with Ras_Pi

- 赤外線受信モジュールで、リモコンの赤外線パターンを覚える。
- 赤外線発信モジュールに先ほどのパターンを流して発振させる。
- （ちゃんと赤外線が送れてることは確認したが、パワーが弱くて反応ナシ。。。）
  - 後日トランジスタで増幅して再チャレンジ

---

# 2016 will

- to be healthy easily
- create local system(with Ad)
- Home Hack!

---

## be healthy easily

手軽に、そこそこ安く健康になりたい。

モチベーション

- 自分の体が弱い
- 常に脳がちゃんと働いてる状態にしたい。
- 意識しないと運動もしなくなる。。。
- 外食はそこそこ高い
- 自炊に時間かけたくない
- シェアハウスなので知見や食材とかシェアできるといいな。

---

## be healthy easily(food)

B塔ではいくつかシェアしようと話し合ってる
- 味噌汁
- 調味料（ウェイパー、味噌、顆粒だし）
- 食材（パックの米、ネギ、卵）

---

## be healthy easily(other)

後日ノウハウが貯まったら共有するかも。

---

## create local system(with Ad)

シェアハウス内向けのサービス作って運用したい。
（こんなの欲しいとかあればください。）

やりたいこと
- 独自ドメインの運用
- HTTPS対応
- Androidアプリの作成

など。使ってもらえるのであれば、維持費はAmazonアフィリエイトとか貼っとくからそこ経由で買ってもらうとか。

---

## Home Hack!

やりたいこと
- 表参道駅着いたらエアコンをONにしてくれる。
  - 部屋に帰る頃には快適になってる。
  - IFTTT＋赤外線モジュール

- 鍵を指定の場所に置くと明かりが付く。離すと消える。
  - 鍵忘れの防止。リモコン要らなくなる
  - 重さセンサー + 赤外線モジュール

- 自分の部屋の鍵を遠隔で操作できる
  - 便利。DIY
  - AWS IoT + トルク強めのモーター + WebSite or App

- 換気アラート
  - 温度、湿度が一定値になるとスマホへ通知が届く
  - IFTTT + 温度・湿度センサー

他にもアイデア募集。

---

みなさん、良いお年を。
