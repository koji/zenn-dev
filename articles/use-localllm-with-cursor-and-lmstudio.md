---
title: "完全無料？CursorでローカルLLMを動かす最短ルート！LM Studio + ngrok 設定ガイド"
emoji: "💰"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['Cursor', 'localllm', 'llm']
published: true
---

## 前提条件

* **Cursor** がインストールされていること
* **LM Studio** がインストールされていること
* **ngrok** がインストールされていること
* ローカルLLMを動作させることが可能なスペックのPC

本ガイドでは、以下のモデルを使用します：`zai-org/glm-4.6v-flash`

---

### ステップ 1: LM Studio のインストール

公式サイトから LM Studio をダウンロードしてインストールします。インストール後、アプリケーションを起動してください。
[https://lmstudio.ai/](https://lmstudio.ai/)

### ステップ 2: モデルのダウンロード

LM Studio 内で、使用したいモデルをダウンロードします。
この記事では `zai-org/glm-4.6v-flash` を使用します。
次のステップに進む前に、ダウンロードが正常に完了したことを確認してください。

### ステップ 3: ngrok のインストール

システムに ngrok をインストールします。
ngrok を使うと、ローカルサーバーを公開URL経由でインターネットに公開できます。
[https://ngrok.com/](https://ngrok.com/)

`Homebrew` をお使いの場合は、以下のコマンドで簡単にインストールできます：

```shell
brew install ngrok

```

### ステップ 4: ngrok のセットアップ

ngrok 公式サイトのセットアップ手順に従い、設定を完了させます。
主な手順は以下の通りです：

* ngrok アカウントの作成
* 認証トークン（Auth Token）を使用したローカル環境の認証

```shell
ngrok config add-authtoken {your_token}

```

### ステップ 5: LM Studio でローカルサーバーを起動

1. LM Studio を開く
2. **Developer Mode**（開発者モード）を有効にする
3. ローカルサーバーを **Start**（開始）する

これで、LM Studio が OpenAI 互換の API としてローカルLLMの提供を開始します。

*(画像の各プロセス：モデルのロード、サーバーの起動確認)*

### ステップ 6: ngrok でローカルサーバーを外部公開

ターミナルを開き、以下のコマンドを実行します：

```shell
ngrok http 1234

```

> **注意:** `1234` は LM Studio のローカルサーバーで使用しているポート番号と一致させてください。

実行後、ngrok が以下のような公開 URL を表示します：
`https://yours.ngrok-free.app`
この URL は Cursor で使用するため、コピーしておいてください。

### ステップ 7: Cursor の設定を開く

Cursor を起動し、以下のメニューに移動します：
**Settings → Models / OpenAI Configuration**

### ステップ 8: OpenAI Base URL の設定

1. **OpenAI API Key** を有効（Enable）にする
2. API キーの欄に任意のプレースホルダーを入力する（例：`1234`）
3. **Override OpenAI Base URL** の欄に、先ほどコピーした ngrok の URL を貼り付ける
4. URL の末尾に `/v1` を追加する

最終的な URL の形式は以下のようになります：
`https://yours.ngrok-free.app/v1`

### ステップ 9: カスタムモデルの追加

1. **Add Custom Model** をクリックする
2. ローカルLLMの名前を入力する（例：`GLM4.6-local`）

⚠️ **重要 (Windowsユーザーの方へ):**
LM Studio が内部でレポートしている「正確なモデル名」を入力する必要があります。今回の場合は `zai-org/glm-4.6v-flash` と入力してください。

---

## 完了！ 🎉

セットアップは以上です。
これで Cursor Chat を開き、プロンプトを入力して送信できるようになりました。Cursor からのリクエストは ngrok を経由して、あなたのマシン上で動作している LM Studio のローカルLLMへとルーティングされます。

この設定により、Cursor の強力なコーディング体験を享受しながら、推論処理を完全にローカル環境で行うことができます。

### 最後に

Cursor でローカルLLMを使用するメリットは以下の通りです：

* **API コストの削減**
* **プライバシーの向上**
* **カスタムモデルやオープンソースモデルの試用**

LM Studio と ngrok を組み合わせることで、このプロセスは驚くほど簡単になります。一度設定してしまえば、クラウド上の OpenAI モデルを使っているのとほとんど変わらない感覚で、自分のマシン上のモデルを利用できます。

Happy hacking! 🚀
