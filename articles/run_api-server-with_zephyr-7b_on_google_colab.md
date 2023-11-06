---
title: 'zephyr-7bをGoogleColabで動かしてAPIサーバとして使う方法'
emoji: '🤖'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['python', 'llm', 'googlecolab']
published: true
---

## これは何ですか？

これは、エンドポイントを介して LLM を使用する Jupyter ノートブックです。  
これは llama.cpp、ngrok、そして TheBloke からのモデルを使用しています。基本的な Jupyter ノートブックは zephyr-7b を使用しています。

## 使い方 [必要条件]

- Google アカウント（Google Colab 用）https://colab.google.com/
- ngrok アカウント https://ngrok.com

### ステップ 0. 上記のアカウントを作成する

### ステップ 1. Jupyter ノートブックをコピーする

### ステップ 2. シークレットキーを作成する

Google Colab のサイドバーにキーアイコンがあります。あなたのトークンをシークレットキーとして追加する必要があります。Jupyter ノートブックでは、私は NGROK と名付けました。あなたはそれを好きなものに変えることができます。

シークレットキー[この記事](https://note.com/npaka/n/n79bb63e17685)が参考になります。

### ステップ 3. Jupyter ノートブックを実行する

新しいシークレットキーを設定した後、API サーバーを実行するために Jupyter ノートブックを実行することができます。

### ステップ 4. API サーバーを確認する

すべてが正常に動作していれば、https://ngrok_address/docs にアクセスすることができ、以下のようなものが表示されます。 利用可能なすべてのエンドポイントを見ることができます。
![Screenshot 2023-11-05 193747](https://github.com/koji/llm_api_template/assets/474225/561830f6-f3a7-4c71-bc39-c857c8eb7ad9)

#### 最後の 2 行は実行しないでください

最初のものは FastAPI サーバーを停止するもので、2 つ目は ngrok を停止するものです。

```shell
!pkill uvicorn
!pkill ngrok
```

### ステップ 5. エンドポイントを呼び出す

今、あなたは好きな言語でエンドポイントを呼び出すことができます。このリポジトリでは、私はサンプルとして Python コードを置いています。 あなたがする必要があるのは、URL を変更することだけです。
https://github.com/koji/llm_api_template/blob/main/send_request.py

## 別のモデルを使いたい場合は？

もし、あなたが llama2-7b、mistral-7b などの別のモデルを使いたい場合は、https://huggingface.co/TheBlokeからモデルをダウンロードし、Jupyterノートブックを少し修正する必要があります。

```py
from llama_cpp import Llama
llm = Llama(model_path="zephyr-7b-alpha.Q5_K_M.gguf") # 👈 このパスを変更.
output = llm("Q: can you write a python script for fizz buzz? A: ", max_tokens=2048, stop=["Q:", "\n"], echo=True)
print(output)
```
