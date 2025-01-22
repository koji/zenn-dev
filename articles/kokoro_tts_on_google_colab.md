---
title: 'Google Colab T4でKokoro TTSの音声合成を試してみた'
emoji: '🦀'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## Kokoro-82M とは？

Kokoro-82M とは高性能な音声を生成することが出来る TTS モデルです。
テキストから音声を簡単に生成することが出来、また音声ファイルに重みを設定することで、簡単に音声の合成が可能です。

https://huggingface.co/hexgrad/Kokoro-82M

v0.23 からは日本語もサポートされています。  
下記で簡単に試すことが出来ます。  
ただし、日本語に関してはイントネーションがやや不自然な印象です。  
https://huggingface.co/spaces/hexgrad/Kokoro-TTS

本記事では、kokoro と onnx ランタイムを使った TTS である[kokoro-onnx](https://github.com/thewh1teagle/kokoro-onnx)が stable version である 0.19v 利用しているため、音声合成に使うのは American English と British English のみになります。
タイトル通りコードは Google Golab での実行を前提としています。

### kokoro-onnx のインストール

```shell
!git lfs install
!git clone https://huggingface.co/hexgrad/Kokoro-82M
%cd Kokoro-82M
!apt-get -qq -y install espeak-ng > /dev/null 2>&1
!pip install -q phonemizer torch transformers scipy munch
!pip install -U kokoro-onnx
```

### パッケージの読み込み

```python
import numpy as np
from scipy.io.wavfile import write
from IPython.display import display, Audio
from models import build_model
import torch
from models import build_model
from kokoro import generate
```

### サンプルを動かす

音声合成を試す前に公式のサンプルを動かしてみます。  
下記を実行すると、数秒で音声が生成され、再生されます。

```python
device = 'cuda' if torch.cuda.is_available() else 'cpu'
MODEL = build_model('kokoro-v0_19.pth', device)
VOICE_NAME = [
    'af', # Default voice is a 50-50 mix of Bella & Sarah
    'af_bella', 'af_sarah', 'am_adam', 'am_michael',
    'bf_emma', 'bf_isabella', 'bm_george', 'bm_lewis',
    'af_nicole', 'af_sky',
][0]
VOICEPACK = torch.load(f'voices/{VOICE_NAME}.pt', weights_only=True).to(device)
print(f'Loaded voice: {VOICE_NAME}')

text = "How could I know? It's an unanswerable question. Like asking an unborn child if they'll lead a good life. They haven't even been born."
audio, out_ps = generate(MODEL, text, VOICEPACK, lang=VOICE_NAME[0])

display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

## 音声の合成

ここからは本題の、音声合成を試してみたいと思います。

### 音声 pack を定義

af:american female アメリカ英語　女性音声
am:american male 　アメリカン英語　男性音声
bf:british female 　イギリス英語　女性音声
bm:british male 　イギリス英語　男性音声

取りあえず、使えるモノを全部読み込んでいます。

```python
voicepack_af = torch.load(f'voices/af.pt', weights_only=True).to(device)
voicepack_af_bella = torch.load(f'voices/af_bella.pt', weights_only=True).to(device)
voicepack_af_nicole = torch.load(f'voices/af_nicole.pt', weights_only=True).to(device)
voicepack_af_sarah = torch.load(f'voices/af_sarah.pt', weights_only=True).to(device)
voicepack_af_sky = torch.load(f'voices/af_sky.pt', weights_only=True).to(device)
voicepack_am_adam = torch.load(f'voices/am_adam.pt', weights_only=True).to(device)
voicepack_am_michael = torch.load(f'voices/am_michael.pt', weights_only=True).to(device)
voicepack_bf_emma = torch.load(f'voices/bf_emma.pt', weights_only=True).to(device)
voicepack_bf_isabella = torch.load(f'voices/bf_isabella.pt', weights_only=True).to(device)
voicepack_bm_george = torch.load(f'voices/bm_george.pt', weights_only=True).to(device)
voicepack_bm_lewis = torch.load(f'voices/bm_lewis.pt', weights_only=True).to(device)
```

### 合成前の音声でテキスト生成

合成した音声との違いを確認するために取りあえず、幾つかの音声パックで音声を生成していきます。
テキストはそのままサンプルのモノを使っています。3 つ目の`voicepack_`の部分を変更すれば好きな音声パックを使って音声を生成することが可能です。

```python
audio, out_ps = generate(MODEL,
                         text,
                         voicepack_bf_emma,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

```python
audio, out_ps = generate(MODEL,
                         text,
                         voicepack_bf_isabella,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

```python
audio, out_ps = generate(MODEL,
                         text,
                         voicepack_bm_lewis,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

### 音声合成

まず最初に、bf_average(British Female を二つを使って) af の bf version を作ってみます。

```python
bf_average = (voicepack_bf_emma + voicepack_bf_isabella) / 2
audio, out_ps = generate(MODEL,
                         text,
                         bf_average,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

続いて、女性２人と男性の音声を合成してみます。

```python
weight_1 =0.25
weight_2 = 0.45
weight_3 = 0.3
weighted_voice = (voicepack_bf_emma * weight_1 + voicepack_bf_isabella * weight_2 + voicepack_bm_lewis * weight_3)
audio, out_ps = generate(MODEL,
                         text,
                         weighted_voice,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

最後に、男性のアメリカ英語とイギリス英語を合成してみます。

```python
m_average = (voicepack_am_michael + voicepack_bm_george) / 2
audio, out_ps = generate(MODEL,
                         text,
                         m_average,
                         lang=VOICE_NAME[0])
display(Audio(data=audio, rate=24000, autoplay=True))
print(out_ps)
```

Gradio を使って色々混ぜてみたら、どうなるのかもテストしてみました。
<video src="./voice_mixing.mp4" controls="controls" width="600" height="400"></video>

Ollama と組み合わせると、色々遊べそうです。
