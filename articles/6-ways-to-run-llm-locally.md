---
title: "ローカルでLLMを動かす6つの簡単な方法 + α"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['LLM', 'ローカル開発', 'AI']
published: true
---

# ローカルでLLMを実行する6つの簡単な方法 + α

## 1. LM Studio
https://lmstudio.ai/  
対応OS：Windows、Linux、MacOS
![LM studio](https://baxin.netlify.app/_astro/lm-studio.DN-jYVW1_Z2sTHhQ.webp)

LM Studioは、大規模言語モデルをローカルで実行・管理するために設計されたパワフルなデスクトップアプリケーションです。様々なオープンソースLLMのダウンロード、実行、チャットのための使いやすいインターフェースを提供しています。LM Studio 0.3.5の新機能：ヘッドレスモード、オンデマンドモデルローディング、MLX Pixtralサポート！これらの機能により、GUIなしでモデルを実行し、必要に応じてモデルをロードしてリソースを節約し、最新のAI技術を活用できるようになりました。

> LM Studio 0.3.5の新機能：ヘッドレスモード、オンデマンドモデルローディング、MLX Pixtralサポート！

## 2. GPT4ALL ⭐70.6K
https://github.com/nomic-ai/gpt4all  
対応OS：Windows、Linux、MacOS
![gpt4all](https://baxin.netlify.app/_astro/gpt4all.sB5O4Ung_2tvCRm.webp)

GPT4ALLは、一般的なデスクトップやラップトップで大規模言語モデル（LLM）をプライベートに実行できます。
APIコールやGPUは不要です。
GPT4ALLは、消費者向けハードウェアでローカルに実行できるオープンソースのチャットボットと言語モデルのエコシステムです。強力なGPUやクラウドサービスを必要とせずに、高度な言語モデルを実行できる独自のアプローチを提供します。GPT4ALLには、使いやすいチャットクライアント、開発者向けのPython API、さまざまなタスクや言語に最適化された事前学習済みモデルが含まれています。

## 3. Ollama ⭐97.3K
https://ollama.com/  
対応OS：Windows、Linux、MacOS  
![ollama](https://baxin.netlify.app/_astro/ollama.5Zz8So48_1Ls2NE.webp)

Ollamaは、大規模言語モデルをローカルで実行するためのパワフルなツールです。個人のコンピュータで様々なLLMをダウンロード、インストール、実行するプロセスを簡略化します。Ollamaは使いやすさと豊富なモデルライブラリにより人気を集めています。PythonとJavaScriptのライブラリを提供しており、アプリケーションにLLMを統合したい開発者にとって優れた選択肢となっています：

- https://github.com/ollama/ollama-python
- https://github.com/ollama/ollama-js

Ollamaはカスタムモデルの作成とファインチューニングもサポートしており、ユーザーは特定のユースケースに合わせてモデルを調整できます。

## 4. Jan ⭐23.3K
https://jan.ai/  
対応OS：Windows、Linux、MacOS  
![jan](https://baxin.netlify.app/_astro/jan.BEGeCCqD_A6GuL.webp)

Janは、デスクトップ上で完全にオフラインで動作するChatGPTのオープンソース代替です。ユーザーのプライバシーとコントロールを重視しながら、AIを誰もが利用できるようにすることを目指しています。Janには組み込みのモデルライブラリがあり、リモートAI APIへの接続をサポートし、OpenAI相当の機能を持つローカルAPIサーバーを提供します。また、カスタマイズのための拡張システムも含まれており、Llama、Gemma、Mistralなど、人気のあるLLMを幅広くサポートしています。

## 5. llamafile ⭐20.4K
https://github.com/Mozilla-Ocho/llamafile  
対応OS：Windows、Linux、MacOS  
Llamafileは、大規模言語モデルを単一のファイルとして簡単に配布・実行できる革新的なプロジェクトです。CPUとGPUの両方での実行をサポートし、エンドユーザーにとってAI LLMをより身近なものにしています。Llamafileは使いやすさと効率性を重視して設計されており、最近のアップデートでは様々なハードウェアアーキテクチャのパフォーマンス最適化に焦点を当てています。また、リクエストを処理するための組み込みサーバーを含み、他のAIツールやフレームワークとの統合も優れています。

## 6. NextChat ⭐76.5K
https://github.com/ChatGPTNextWeb/ChatGPT-Next-Web  
対応OS：Windows、Linux、MacOS  
![nextchat](https://baxin.netlify.app/_astro/nextchat.DbcXC9z5_Z1EAoW9.webp)

NextChat（ChatGPT Next Webとしても知られる）は、標準のChatGPT体験を追加機能で強化するオープンソースのチャットボットです。カスタマイズ可能なインターフェースを提供し、テンプレートプロンプトにすぐアクセスできる「Awesome Prompts」や、特殊なChatGPTインスタンス用の「Masks」などの機能が含まれています。NextChatはOpenAI APIを使用した従量制システムをサポートしており、ユーザーにとってコスト効率の良いオプションとなっています。簡単にデプロイとカスタマイズが可能で、ユーザーは特定のニーズに合わせてパーソナライズされたAIアシスタントを作成できます。

## オプション：llama.cpp ⭐67.6K
https://github.com/ggerganov/llama.cpp

llama.cppはC++で書かれており、LLaMaの最速の実装です。他のローカルアプリケーションやウェブベースのアプリケーションでも使用されています。
しかし、上記のアプリケーションほど簡単にインストールできないため、ローカルでLLMを実行するオプションの方法として紹介しています。
