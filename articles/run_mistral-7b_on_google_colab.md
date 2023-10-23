---
title: "Mistral-7BをGoogleColabで試してみた"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["llm", "python", "_mistral-7b"]
published: true
---

この記事はGoogle Colabの無料版T4での動作を確認しています。  
なので、requirementはGoogleアカウントになります

## Mistral-7Bとは
https://mistral.ai/news/announcing-mistral-7b/


Google Colabで動かすにはGGUFファイルを使用します。
なので、requirementはGoogleアカウントになります。

[What is GGUF and GGML?](https://medium.com/@phillipgimmi/what-is-gguf-and-ggml-e364834d241c#:~:text=GGUF%20and%20GGML%20are%20file%20formats%20used%20for%20storing%20models,Generative%20Pre%2Dtrained%20Transformer)

>GGUFとGGMLは、特にGPT（Generative Pre-trained Transformer）のような言語モデルの文脈で、推論用のモデルを保存するために使用されるファイル形式です。


TheBlokeさんが公開しているモデルを利用します。
https://huggingface.co/TheBloke/OpenHermes-2-Mistral-7B-GGUF/tree/main


手順は以下の通りです。
1. ggufファイルをダウンロード
2. llama-cppをインストール
3. モデルをロードする プロンプトを作成
4. プロンプトでMistral-7Bモデルを呼び出す


Jupyter notebook  
https://github.com/koji/llm_on_GoogleColab/blob/main/Mistral-7B.ipynb

下記は上記のpromptを実行した結果になります。

```
I feel my heart beating faster than light  
You light up my world, you make me feel alive  
Oh, darling, I need your love to survive  
```
