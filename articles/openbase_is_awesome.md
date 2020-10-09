---
title: "jsライブラリ選定はレビューが載ってるopenbaseが超絶便利"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: [github, library, js]
published: false
---
![](https://storage.googleapis.com/zenn-user-upload/8ytbhrkpue1zx3z9c6ng8ifgfsy4)

## openbaseとは
https://openbase.io
Find and compare open-source packages with user reviews, categorization, and unparalleled insights about packages' popularity, reliability, activity, and more.

手を抜いてdeeplでw
>ユーザーレビュー、カテゴリ分け、パッケージの人気、信頼性、アクティビティなどについての他の追随を許さない洞察力で、オープンソースパッケージを見つけて比較してください。

現在、多くのプログラミング言語はパッケージマネージャがあって、何らかの方法でパッケージについての情報を取得することができると思います。

## パッケージの選び方
自分の場合はjs系が殆どなので、ライブラリを選ぶ方法は以下のような感じです

1. npmで検索  
2. descriptionを読んで良さそうなもの、ダウンロード数がある程度あるもの、  
最終更新が数ヶ月以内のものピックアップ  
3. GitHubページのReadmeと公式ドキュメントがどの程度整備されているか確認  
issueとPRを軽くチェックし、デモがあれば、それもチェック  
   ちなみにドキュメントの有無だけでは、決めないようにしていますreact-draggableのようにドキュメントはあっさりしているけれど、定期的にリリースされていて、issue boardで開発者やコントリビュータが意見を頻繁に交換しているケースもあるため

その上で、数個に絞った上で簡単に試してみて、決めています。
激しく、外すことはないのですが、時々ざっと、Readme読んでああこれなら良さそうと思って入れて、使い始めたら勝手にあると思い込んでいた機能が実はサポートされてなくて、自分で実装するくらいならちょっとあれだけど、もう一個の最終候補に残ったやつに切り替えるかという事があります。
大抵の場合、凄い特殊な使い方というわけでは、何ので自分の他にも同じような経験の人いるだろうから、レビューとかあれば助かるなぁと思っていました。

## openbaseのいいところ Review
そんな時に見つけたのが、openbase.ioです。
先にあげた選ぶ時の指標が全部入っている上に、ユーザーからのReviewも載っているという至れり尽くせりのサイトです。　
個人的には、もう少し明るい色のサイトだといいなぁとか思ったりしますが、機能面は本当に大満足です。

Reviewは投稿者のレーティング星の数と使った感想が掲載されています。
また、Review自体にLikeを付けられるので、Reviewを見た人の視点でReviewそのものをチェックしても良さそうという判断の助けになるかと。
ちなみに、Reactだと570件以上のReviewがついています

[基本情報]
![](https://storage.googleapis.com/zenn-user-upload/s1clltyy6rfmycn2g6t2krslpn2d)

[レビュー]
![](https://storage.googleapis.com/zenn-user-upload/vdamn7blticatjme0y08vjw4k2kx)


## openbaseのいいところ tutorialをすぐ確認できる
サイドバーの`tutorial`をクリックすると簡単にチュートリアルにアクセスできます。Youtubeのビデオも含まれているので、ざっくりと使い方を掴むのに非常に便利です。
![](https://storage.googleapis.com/zenn-user-upload/s44khn5923fqmqpkn4nzw1brmikh)

![](https://storage.googleapis.com/zenn-user-upload/h1ryt2fszm22kg502v1slkgd5rv3)


## openbaseのいいところ 代替パッケージの情報
サイドバーのAlternativesをクリックすると、代替パッケージの情報が表示されます。下記はReactの結果なので、まぁそんなに新しい発見みたいなものはないですが、パッケージによっては面白い発見がある可能性もあるかと思います。
この機能の良いところは自分でリサーチした時の漏れをカバーすることができる点だと思います。あとは、有名なパッケージだけ、検索かけて最初から候補をAlternativesからピックアップするという方法もありではないかと。
![](https://storage.googleapis.com/zenn-user-upload/7iwligihz98uwfzs8acz32kfe9wc)