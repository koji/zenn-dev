---
title: 'Ollamaを使ってCursorでLoc LLMを使う方法'
emoji: '✨'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: ['Cursor', 'Ollama', 'LocalLLM']
published: true
---

## 要件

- `Cursor` がマシンにインストールされている
- `Ollama` がマシンにインストールされ、モデルを保持している
- `ngrok` がマシンにインストールされており、ngrok アカウントを持っている

### Step1. `Cursor` をインストール

[https://www.cursor.com/](https://www.cursor.com/) にアクセスして Cursor をダウンロードし、マシンにインストールしてください。

### Step2. `Ollama` をインストール

[https://ollama.com/](https://ollama.com/) にアクセスして Ollama をダウンロードし、 マシンにインストールしてください。

### Step3. ngrok アカウントを作成し、ngrok をインストール

[https://ngrok.com/](https://ngrok.com/) にアクセスして ngrok をダウンロードし、マシンにインストールしてください。  
その後、ngrok のセットアップを行います。

### Step4. モデルをダウンロード（pull）

この記事では `deepseek-r1` モデルを使用します。
[https://ollama.com/library/deepseek-r1](https://ollama.com/library/deepseek-r1) を参照してください。ターミナルアプリを開き、以下を実行します：

```shell
# 7Bモデルの取得
ollama pull deepseek-r1:latest
```

### Step5. CORS を有効化して ngrok を起動

```shell
# macOS または Linux の場合
export OLLAMA_ORIGINS="*"

# Windows を使用している場合
set OLLAMA_ORIGINS="*"

ngrok http 11434 --host-header="localhost:11434"
```

### Step6. OpenAI API キーの設定

1. 取得したモデル名（ここでは `deepseek-r1:latest`）を入力し、「Add model」をクリック
2. API キーの欄には `Ollama` を入力
3. ngrok コマンドで得られた URL + `/v1` を入力  
   例: `https://ngrok_something/v1`

   ![config](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/juwee414s4n1d3tuqzgo.png)

4. 「Save」をクリック

### Step7. Ollama 設定を検証

もう少しで完了です。
「Verify」ボタンをクリックする前に、ローカル以外のモデルはすべて選択解除してください。
この場合、`deepseek-r1:latest` のみを選択して、「Verify」をクリックします。

### Step8. ローカルモデルを使う

いよいよ最終ステップです。
Cursor を開き、チャット（Ctrl/Cmd + l）を起動します。
Step6 で追加したモデルが選択されていることを確認し、プロンプトを送信してください。
