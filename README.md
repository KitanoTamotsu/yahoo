#### 開発メモ
ワークフロー
<br><img width="600" src="https://user-images.githubusercontent.com/40127279/126856670-b705deb1-6ce7-4633-8eff-345b743c9567.png">

### 1.まずは『ヤフーゆるネタ』でScriptFilterを試作してみる
　このワークフローには7つのScriptFilterがありますが作りは全て同じ
<br>　まずは『(ヤフー　ゆるネタ)』のコードを見てください
<br>
<br>　変数queryにアクセスするURLを格納しています
<br>　後で、この部分変えれば他にも使い回しができるかなという魂胆です
<br>
<br>　続いて配列変数title,desc,linkを作ります
<br>　cURLでソースを取得して、grepとsedで抽出・整形しています
<br>　
<br>　初めはxmllintで解析する方法を試したのですが、うまく行きませんでした
<br>　空白があってパースが途切れる感じです
<br>　どうせ空白を取る必要があるのであればと思い、sedとgrepを選択しました
<br>　インターネットからのコピペなのでオプションやパラメータは吟味していません
<br>　そう、動いたからいいやのノリです
<br>　
<br>　URLからRSS取得、空白削除、タグ抽出、タグの中身の取り出しという4段階を
<br>　パイプで繋いでいます
<br>　テスト中に気が付いたのですが、descriptionタグには＆lt;や＆gt;でaタグが
<br>　埋め込まれていたので、空白削除の後に&lt;から&gt;に囲まれた部分も削除しています
<br>　
<br>　ちょっと気になるのが、それぞれのタグごとにcURLを利用しているので、無駄なコードに
<br>　なっている点。色々と試してみたのですが、うまくいきませんでした（力量不足です）
<br>　
<br>　最後はJSONフォーマットの記述
<br>　はじめはサンプル通りEOBで書いてみました
<br>　ただ、せっかく配列を利用したので、ループ処理で文字列をつなげてみました
<br>　（線が繋がっているScriptFilterを見てくださいな）
<br>　こちらはEOBではないのですが、最後にechoをしてみたらうまくAlfredに表示させる
<br>　ことができました
<br>　
<br>　兎にも角にも、これでコア部分の完成です
<br>　
<br>　RunScript
<br>　<img width="600" src="https://user-images.githubusercontent.com/40127279/126856694-5a135691-32e4-48e4-86a6-ae9dedc043a9.png">


### 2.初めにRSSのカテゴリーを選択させたい
　ヤフーニュース『みんなの意見』にはカテゴリがあるので、
<br>　はじめにカテゴリーを選択させて、次に詳細のタイトルを選ばせたいと思い
<br>　試行錯誤した結果、複数のScriptFilterを並列するという方法をとりました
<br>　具体的にはScriptFilterのキーワードを『ヤフー　〇〇〇〇』とするだけ
<br>　こうすることで、『ヤフー』と入力すると、全てのカテゴリーが表示できます
<br>　実際に動かしてみてください。[サンプル動画もどうぞ](https://user-images.githubusercontent.com/40127279/125182947-5a489f00-e24d-11eb-823d-9ffe309f65d7.mp4)

#### 背景
　AlfredのJSONフォーマットのサンプルとしてScriptFilterが準備されていたので、
<br>　何かに利用できないかと考えたらすぐにRSSフィードが思い浮かびました
<br>　よくあるニュースサイトのRSSなどをみていたら、ヤフーのRSS一覧を発見
<br>　　https://news.yahoo.co.jp/rss
<br>　下の方にあったみんなの意見という一連のRSSをインタラクティブにAlfredで表現してみました
<br>　もし皆さんがRSSをよく利用するのであれば、RSS xxxをキーワードとしてAlfredで
<br>　まとめてみてはいかかでしょう

#### 取扱説明
### 機能:
　ヤフーニュースの『みんなの意見』を表示させる
### インストール:
　1.[alfredworkflow](https://github.com/KitanoTamotsu/yahoo/releases/download/1.2/yahoo.alfredworkflow.zip)をダウンロード 
<br>　2.ファイルをダブルクリックしてワークフローに登録
### 使い方:
　Alfredからキーワード起動『ヤフー』


#### 修正履歴
### ver1.1(2021-03-28)：
・ScriptFilterのJSONフォーマットの編集をワンライナーから1行1プロパティーに変更
<br>　修正前
```
　json=$json'{"title":"'${title[i]}'","subtitle":"'${desc[i]}'","arg":"'${link[i]}'"}'  
``` 
<br> 修正後
```
  json=$json'{"title":"'${title[i]}'",'
  json=$json'"subtitle":"'${desc[i]}'",'
  json=$json'"arg":"'${link[i]}'"}'  
```
### ver1.2(2021-04-04)：
 ・シェルスクリプトをbashからzshに変更

<br>
<br>
[トップページに戻る](https://kitanotamotsu.github.io/)

