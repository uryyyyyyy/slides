
## Agenda

 - Markdownとは？
 - 何がすごい？
 - 具体的な使い方
    * メモ
    * スライド
    * ブログ
    * Github

--

## ゴールは？

 - evernoteとかより使いやすい。
 - スライドが簡単かつかっこ良く作れる。
 - ブログとかちょっとしたメモが見やすくなる。
 - 思考がシンプル・整理される。
 - エンジニアの共通言語を身につけられる。

---

## Markdownとは？

 - 軽量マークアップ言語
 - 拡張子→.md, .mkd, .markdownなど。
 - 中身はただのテキストファイル

[使い方サンプル](http://kojika17.com/2013/01/starting-markdown.html)

[公式ページ](http://daringfireball.nhttps://github.com/uryyyyyyy/slideset/projects/markdown/)

--

## 何がすごい?

 - 単体でも読み書きしやすい
 - ただのテキストなので軽い。editorを選ばない。
 - コレ以上ないくらいシンプルかつ高機能(高級言語っぽい)
 - PDFやHTMLに変換可能（そういうツールがある）

**中間ファイルとして最強**

---

具体的な使い方

 1. 簡単なメモ（各種Editor)
 2. スライドの本文（reveal.js）
 3. Blog・書き物（ハテブ・sphinx etc..）
 4. github


---

## 簡単なメモにする。

 - なんてったって軽い。
 - 余計なタグとか付かず、文字に注力できる。
 - 箇条書きベースで、簡潔にまとめられる。

markdown + DropBoxを使うとevernoteとか重たくて使えません。

（タグで検索したいって？フォルダ分けやgrepで十分じゃない？）

--

### PC

 - [ブラウザ](http://jrmoran.com/playground/markdown-live-editor/)から
 - [SublimeText](http://sonoshou.hatenablog.jp/entry/2013/12/20/Sublime_Text_%E3%81%A7_Markdown%EF%BC%8E)から
 - もちろんデフォルトのメモ帳でも（エンコードに注意）

ちょっと書くならgedit（Ubuntuのメモ帳）で十分だけど、しっかり書くならブラウザのが補完とか便利なのでオススメ。

他にも色々→[エディタ20選](http://www.find-job.net/startup/20-markdown-editors)

--

### IOS

 - [Write](http://lifehacking.jp/2014/01/write/)
 - [Plain Text](http://www.appbank.net/2014/01/09/iphone-application/730935.php)

スマホに関しては、**同期が簡単・軽い**ことが必須なので、個人的にはPlainText推しです。拡張子がtxtになりますが。
Writeは買ってみたものの、日本語入力に難があったり、同期が不十分だったりとイマイチ。

---

## スライドを簡単に作る

今の時代、WindowsやらMacやらスマホやら色々あるので、
パワポなどのプラットフォーム依存の形式は廃れていくでしょう。
代わりにPDFやHTMLが主流ですよね。

かっこよくて便利なスライドを簡単に作る方法をご紹介しましょう。


--

## reveal.js & markdown

[デモ](http://uryyyyyyy.github.io/slides/#/)

これ、実は中身はmarkdownで書かれています。

[中身](http://uryyyyyyy.github.io/slides/main_text/index.md)

--

## 使い方

[こちらから](https://github.com/uryyyyyyy/slides/tree/gh-pages)ダウンロードしてきて、index.htmlの中身をいじるだけ。

真ん中あたりのindex.mdを読んでるところへ、自分のmarkdownをそのまま貼り付ければOK。記法はサンプルを見ればわかります。

（ローカルで試すと.mdを上手く読めないので、htmlだけで完結するのが楽です。）

**PDF出力もできて便利ですよ！**

---

## Blogや書き物に。

プログラマ界隈では、markdownを読み込んで静的HTMLを作ってしまえるツールが色々あります。

また、github.ioという、静的Webページを無料で公開できるサービスがあるので、
個人ブログくらいならそれで十分という声も多いです。

--

### octpress
ブログを簡単に作れます。僕のブログも今はこれで書いています。

WordPressをブログに特化したものと思ってもらえば近いかと。

### middleman
上と違いがよくわからないですが、これもブログ生成ツール。

**言い忘れてましたが、これらは静的HTMLを作ってくれるので、表示も早いしローカルでも動かせます。**

---

## Github

Readmeを書くときに便利です。

また、github.ioという静的HTMLのホスティングサービスがあるので、
このスライドみたいに公開することもできます。

---

以上、今後はmarkdownはデファクトスタンダードになる気がしています。
サクッと覚えられるのでぜひ活用を！

![alt text][11]

*by Life Hacker*


今回のスライドの中身は[こちら][12]

  [11]: http://www.lifehacker.jp/images/2013/06/130611Markdown1.jpg
  [12]: http://uryyyyyyy.github.io/slides/main_text/markdown.md
