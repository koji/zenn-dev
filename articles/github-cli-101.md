---
title: "GitHub CLI 101 ghを試してみた"
emoji: "😎"
type: "tech"
topics: ["tech", "github", 'cli']
published: true
---

## install
```zsh
$ brew install gh
```
以前は(beta version)は`gh/gh`とかだったような気がします。

##　clone repo
git cloneと同じことが`gh`でも出来ます。
```zsh
$ gh repo clone owner/repo_name
```
レポジトリで`clone`を`GitHub CLI`を確認することができます。
![](https://storage.googleapis.com/zenn-user-upload/r8duci7r2lfl540hav2qfbpy04x1)

## Check issue status
現状の自分に関連するissueの状態を確認する事ができます。
自分がアサインされているもの、自分のアカウント名が言及されているもの、そして自分が開いたものが表示されます。
```zsh
$ gh issue status

Relevant issues in koji/typescript

Issues assigned to you
  #107  add docker stuff  (enhancement)  about 11 days ago
  #71   update README                    about 1 month ago

Issues mentioning you
  There are no issues mentioning you

Issues opened by you
  #107  add docker stuff  (enhancement)  about 11 days ago
  #71   update README                    about 1 month ago

```

## List issues
現在開かれているissueが表示されます。ちなみに、デフォルトで表示される数は30件です。
```zsh
$ gh issue list

Showing 2 of 2 open issues in koji/typescript

#107  add docker stuff  (enhancement)  about 11 days ago
#71   update README                    about 1 month ago
```
options
https://cli.github.com/manual/gh_issue_list

自分のレポジトリ以外に関しても表示することができます。
例：ml5jsのissueを200件表示（9月20日 issue 180件）

```zsh
$ gh issue list -R ml5js/ml5-library -L 200
```


## Open a PR
タイトル、本文（Editorを利用）を記載すると最後にPRをどうするかの選択が表示されます。下記の中から`Submit`を選ぶとPRが作成されます。ちなみ、`Continue in browser`を選択するとデフォルトブラウザでGitHubのPR作成画面が開かれます。
`Add metadata`を選択すると下記の項目を設定する事ができます。

```zsh
$ gh pr create

$ gh pr create

Creating pull request for update-readme into master in koji/typescript

? Title Update readme
? Body <Received>
? What's next?  [Use arrows to move, type to filter]
> Submit
  Continue in browser
  Add metadata
  Cancel

> Add metadata
? What would you like to add?  [Use arrows to move, space to select, <right> to all, <left> to none, type to filter]
> [x]  Reviewers
  [ ]  Assignees
  [x]  Labels
  [ ]  Projects
  [ ]  Milestone

? Reviewers  [Use arrows to move, space to select, <right> to all, <left> to none, type to filter]
> [ ]  koji

? Labels  [Use arrows to move, space to select, <right> to all, <left> to none, type to filter]
  [ ]  dependencies
  [ ]  document
  [ ]  duplicate
> [ ]  enhancement
  [ ]  good first issue
  [ ]  greenkeeper
  [ ]  help wanted

? What's next?  [Use arrows to move, type to filter]
> Submit
  Cancel

https://github.com/koji/typescript/pull/108
```

`status`を実行すると作成したPRを確認する事ができました。
```zsh
$ gh pr status

Relevant pull requests in koji/typescript

Current branch
  #108  Update Readme [update-readme]
  - Checks pending

Created by you
  #108  Update Readme [update-readme]
  - Checks pending

Requesting a code review from you
  You have no pull requests to review

```

## Review
PRのReviewも`gh`で可能です。オーソドックスな方法としてはdiffで変更箇所を確認、`review`で`Approve`, `Comment`, `Request changes`を決めるという感じではないかと思います。ただし、diffに関しては量が多いと分かりずらいので、ReviewはWebの方が良いきがします。

```zsh
$ gh pr diff
diff --git a/hello_next/.next/build-manifest.json b/hello_next/.next/build-manifest.json
new file mode 100644
index 0000000..c0da45a
--- /dev/null
+++ b/hello_next/.next/build-manifest.json
@@ -0,0 +1,34 @@
+{
+  "polyfillFiles": [
+    "static/chunks/polyfills.js"
+  ],
+  "devFiles": [
+    "static/chunks/react-refresh.js"
+  ],
+  "ampDevFiles": [
+    "static/chunks/webpack.js",
+    "static/chunks/amp.js"
+  ],
+  "lowPriorityFiles": [
+    "static/development/_buildManifest.js",
+    "static/development/_ssgManifest.js"
```

```zsh
$ gh pr review
? What kind of review do you want to give?  [Use arrows to move, type to filter]
> Comment
  Approve
  Request changes
```

## Merge
最後にPRをマージして終わりです。
```zsh
$ $ gh pr merge
? What merge method would you like to use?  [Use arrows to move, type to filter]
> Create a merge commit
  Rebase and merge
  Squash and merge
```

## オマケ
デフォルトだと`nano`がエディタとして設定されているので、使い慣れたエディタに(`code`/`vim`)に変更します。
`nano`の方がしっくりくる方は変更する必要はありません。
今回は`code`(VScode)をは見送りました、理由はVSCodeだとPRのBodyのPreviewが簡単に見られるからです。
```zsh
$ gh config set editor "code --wait"

# vim
$ gh config set editor vim

```

![](https://storage.googleapis.com/zenn-user-upload/kqpfb91uaflswmcdivs2pqp5ua1m)