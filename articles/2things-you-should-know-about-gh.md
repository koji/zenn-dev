---
title: "ghについて知っておくべきこと3つ"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["tech", "github", "gh"]
published: true
---

`gh`が正式版としてリリースされました。

結構、記事も出ているので、この投稿では気をつけるべき事ということで２点ほど紹介したいと思います。


## 1. diffはあるがWebほど見やすくない
`gh`にはPRに対して変更した内容を表示するコマンド、`diff`があります。
linux,macOSで`diff`コマンドを使ったことある人はわかるかと思いますが、このghのdiffはオプションが色付けだけなので、見づらいです。
変更が数行レベルのPRならいいですが、数十行単位かつ複数ファイルにまたがる変更だとあんまりCLIでレビューする利点はないかなぁと思います。
```zsh
$ gh pr diff
```

![](https://storage.googleapis.com/zenn-user-upload/o49ddn62edu2beykac65m53hm82j)


## 2. 自分のPRはApproveできない
1点目Reviewで気をつける必要があることは`gh`は自分で作成したPRの`Approve`はできないという事です。なので、自分のPRをReviewしたい場合はWebで行う必要があります。
```zsh
$ gh pr review 1
? What kind of review do you want to give? Approve
? Review body <Received>
? Submit? Yes
failed to create review: Can not approve your own pull request
```


## 3. Default Editorは`nano`
デフォルトだと`nano`がエディタとして設定されているので、使い慣れたエディタに(`code`/`vim`)に変更します。
`nano`の方がしっくりくる方は変更する必要はありません。
今回は`code`(VScode)をは見送りました、理由はVSCodeだとPRのBodyのPreviewが簡単に見られるからです。
git同様、ファイルを閉じると変更がghの方で認識されるようです。

ちなみ、過去のコミット時のコメントが全て入力されたものが、表示されます。

```zsh
$ gh config set editor "code --wait"

# vim
$ gh config set editor vim

```

![](https://storage.googleapis.com/zenn-user-upload/kqpfb91uaflswmcdivs2pqp5ua1m)

