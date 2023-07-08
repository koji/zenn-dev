---
slug: 'use_brad-api_with_python'
title: "Bard-APIを使ってpythonで回答を得る方法"
emoji: "😎"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["tech","python","Bard"],
published: true
---

## Bard-APIとは
Cookieの値を渡すことで、Bardに質問を投げて、その回答を得るコードを10行くらいで可能にしてくれるパッケージです。

https://github.com/dsdanielpark/Bard-API


### 必要なパッケージ
cookieの値をコードに直に書く場合はpython-dotenvは不要です。
```shell
pip install bardapi python-dotenv
```


## Cooke - _Secure-1PSIDの値を取得
Chrome/FirefoxでBardにアクセスします。
https://bard.google.com/

アクセスしたら、Dev Toolsを開いて、`Application`　タブを選択して、 `Storage` の下にある、`Cookies`をクリックします。
すると以下のような画面が表示されるので、その中から、` _Secure-1PSID` を探して、valueをコピーします。


![chrome](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/keqm3ydf4xoc5dswwc7j.png)


## evnファイルの作成
適当にコードを保存するためのファオルダを作成して、その中に.envファイルを作ります。
直に値をコードに書く場合はここはスキップして大丈夫です。

```shell
mkdir bard-api_test
cd bard-api_test
touch .env
```

.envには下記の記述を追加します。
先ほど、コピーした値を貼り付けます。

```
COOKIE_TOKEN='put your cookie value'
```

## python codeを書く
.envを利用しない場合は　`token='cookie value'` となります。
```python
import os
from bardapi import Bard
from dotenv import load_dotenv


load_dotenv()

token = os.environ['COOKIE_TOKEN']
bard = Bard(token=token)
prompt="LLMとはなんですか?"
response = bard.get_answer(prompt)['content']
print(response)
```

実行します。
```shell
python app.py
```

実行結果  
日本語で質問すると、日本語で回答が得られます。ちなみに、英語で質問すると、回答も英語になります。
出力に関してはpromptの部分を変更するといろいろ変えられます。
```shell
LLMとは、Large Language Modelの略です。日本語では「大規模言語モデル」と呼ばれます。これは、膨大な量のテキストデータでトレーニングされた機械学習モデルです。テキストを生成し、言語を翻訳し、さまざまな種類のクリエイティブ コンテンツを作成し、有益な方法で質問に答えることができます。
```

例えば、promptを以下のようにすると、

```python
prompt='''
What is a LLM?
Please answer in Japanese
'''
```

回答
```shell
LLMはMaster of Lawsの略で、法学の修士号です。LLMは、法学士号を取得した後に取得することが多い学位で、特定の分野の法律をより
深く学ぶことができます。LLMは、日本国内でも取得可能で、英語で開講しているプログラムもあります。

Here is the English translation:

LLM stands for Master of Laws, which is a master's degree in law. LLMs are often obtained after a bachelor's degree in law, and allow students to specialize in a particular area of law. LLMs are also available in Japan, and there are some programs that are taught in English.
```

出力方法を指定してみました。
ちょっと、試した感じ日本語だとあんまりうまくいかなかったので、出力にたいしていろいろ制約・条件を付ける場合は英語の方がよさそうです。

```python
prompt = '''
What is a LLM?
The answer format should be the following.

```
answer: {
[
  id: 1,
  content: draft1
],
 id: 2,
  content: draft2
]
}
'''
```


```shell
Sure, here is the answer to your question in the specified format:


answer: {
  "id": 1,
  "content": "LLM stands for Master of Laws. It is an advanced law degree that is typically pursued by lawyers who want to specialize in a particular area of law. LLM programs can be found at law schools all over the world, and they typically take one year to complete. Some of the most popular areas of study for LLMs include tax law, international law, human rights law, and environmental law.

Here are some of the benefits of pursuing an LLM:

* Gain advanced knowledge in a specific area of law
* Enhance your legal career prospects
* Expand your professional network
* Improve your research and writing skills
* Learn from leading experts in the field

If you are interested in pursuing an LLM, there are a few things you should keep in mind:

* You will need to have a law degree in order to apply for an LLM program.
* LLM programs can be expensive, so you should factor in the cost of tuition and living expenses.
* LLM programs can be competitive, so you should start your application process early.

If you are considering pursuing an LLM, I encourage you to do your research and find a program that is a good fit for your interests and career goals."
}


**Draft 2:**


answer: {
  "id": 2,
  "content": "LLM stands for Master of Laws. It is an advanced law degree that is typically pursued by lawyers who want to specialize in a particular area of law. LLM programs can be found at law schools all over the world, and they typically take one year to complete. Some of the most popular areas of study for LLMs include tax law, international law, human rights law, and environmental law.

An LLM can be a valuable asset for lawyers who want to advance their careers. It can help you gain specialized knowledge in a particular area of law, which can make you more marketable to employers. An LLM can also help you develop your research and writing skills, which are essential for success in the legal profession.

If you are considering pursuing an LLM, there are a few things you should keep in mind:

* You will need to have a law degree in order to apply for an LLM program.
* LLM programs can be expensive, so you should factor in the cost of tuition and living expenses.
* LLM programs can be competitive, so you should start your application process early.

If you are interested in pursuing an LLM, I encourage you to do your research and find a program that is a good fit for your interests and career goals."
}


I hope this helps!
```

Bardを使ったことある方はご存じだと思いますが、Bardは出力候補をDraftという形で表示してくれます。
そのDraftを取得することも可能です。
コード自体は最初のものとほぼ一緒で、取ってくるデータを`content` から `choices`に変更します。

```python
import os
from bardapi import Bard
from dotenv import load_dotenv


load_dotenv()

token = os.environ['COOKIE_TOKEN']
bard = Bard(token=token)
prompt='''
What is a LLM?
'''

responses = bard.get_answer(prompt)['choices']

for choice in responses:
  id = choice['id']
  response = choice['content'][0]
  print(id)
  print(response)
```

```shell
rc_baf2ef960666a44b
LLM stands for Master of Laws. It is a postgraduate law degree that is typically obtained after completing an undergraduate law degree. LLM programs can be specialized in a particular area of law, such as tax law, environmental law, or international law. They can also be more general, providing students with a broad overview of different areas of law.

LLM degrees are offered by law schools around the world. In the United States, LLM programs are typically one year in length. In other countries, LLM programs can be longer, lasting two or even three years.

There are many reasons why someone might choose to pursue an LLM degree. Some people do it to gain expertise in a particular area of law. Others do it to improve their job prospects or to advance their careers. Still others do it simply because they are interested in learning more about the law.

If you are considering pursuing an LLM degree, there are a few things you should keep in mind. First, you need to make sure that you have the necessary qualifications. Most LLM programs require that you have a bachelor's degree in law or a related field. Second, you need to decide what type of LLM program you want to pursue. There are many different specializations available, so you need to choose one that is right for you. Finally, you need to research different law schools and programs to find one that is a good fit for you.

Here are some of the benefits of obtaining an LLM degree:

* Gain expertise in a particular area of law
* Improve your job prospects
* Advance your career
* Learn more about the law
* Network with other lawyers

If you are interested in pursuing an LLM degree, I encourage you to do your research and find a program that is right for you. It can be a valuable investment in your future.
rc_baf2ef960666a99c
LLM stands for Master of Laws. It is an advanced postgraduate law degree that is typically obtained after completing an undergraduate law degree. LLM programs can be specialized in a particular area of law, such as tax law, environmental law, or international law. They can also be more general, providing students with a broad overview of different areas of law.

LLM degrees are offered by law schools around the world. In the United States, LLM programs are typically one year in length. In other countries, such as the United Kingdom, LLM programs can be two years in length.

There are many reasons why a person might want to pursue an LLM degree. Some people do it to gain expertise in a particular area of law. Others do it to improve their job prospects or to advance their careers. Still others do it simply because they are interested in learning more about law.

If you are considering pursuing an LLM degree, there are a few things you should keep in mind. First, you should make sure that you have the necessary academic qualifications. Second, you should decide what area of law you want to specialize in. Third, you should research different LLM programs to find one that is a good fit for you.

Here are some of the benefits of obtaining an LLM degree:

* Increased knowledge and expertise in a particular area of law
* Improved job prospects
* Advancement of career
* Personal satisfaction of learning more about law

If you are interested in pursuing an LLM degree, I encourage you to do your research and find a program that is a good fit for you. It can be a valuable addition to your legal education and can help you achieve your career goals.
rc_baf2ef960666aeed
LLM stands for Master of Laws. It is a postgraduate law degree that is usually obtained after completing an undergraduate law degree. LLM programs can be specialized in a particular area of law, such as tax law, environmental law, or international law. They can also be more general, providing students with a broad overview of different areas of law.

LLM degrees are offered by law schools around the world. In the United States, LLM programs are typically one year in length. In other countries, such as the United Kingdom, LLM programs can be two years or more.

There are many reasons why someone might choose to pursue an LLM degree. Some people do it to gain expertise in a particular area of law. Others do it to improve their job prospects or to advance their careers. Still others do it simply because they are interested in learning more about the law.

If you are considering pursuing an LLM degree, there are a few things you should keep in mind. First, you need to make sure that you have the necessary prerequisites. Second, you need to decide what type of LLM program you want to pursue. Third, you need to research different law schools and programs to find the one that is right for you.

Here are some of the benefits of getting an LLM degree:

* Gain expertise in a particular area of law
* Improve your job prospects
* Advance your career
* Learn more about the law
* Network with other lawyers

If you are interested in pursuing an LLM degree, I encourage you to do some research and find a program that is right for you. It could be a great way to advance your legal career and achieve your goals.
```
