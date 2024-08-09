---
title: 'FLUX.1をM3 Macで試してみた'
emoji: '✨'
type: 'tech' # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## FLUX.1 とは

https://blackforestlabs.ai/announcing-black-forest-labs/

日本語の方が良い場合はこちらのページをご覧ください。  
https://note.com/npaka/n/n4ca3fae6ce31

M3 Mac で FLUX.1 を試すのに Diffusers を使いました。
https://huggingface.co/docs/diffusers/index

## pre-requisite

python 3.10.x がインストール済  
Huggingface アカウントが作成済み  
下記のページでモデルへのアクセス Grant を付与していること  
https://huggingface.co/black-forest-labs/FLUX.1-dev

## Step1. Virtualenv を作成

仕事で利用している python 環境に影響が出るような変更を加えたくないので、Virtualenv を作成し、  
そこに必要なパッケージをインストールします。
この記事では Python 3.10.13 を使っています。

```shell
python3 -m venv fluxtest
source fluxtest/bin/activate
```

## Step2. Huggingface CLI をインストール

FLUX.1 のモデルをダウンロードするためには Huggingface CLI が必要になるため、インストールして、CLI 経由でログインします。

```shell
pip install -U "huggingface_hub[cli]"
huggingface-cli login
```

## Step3. 必要なパッケージをインストール

```shell
pip install torch==2.3.1
pip install git+https://github.com/huggingface/diffusers.git
pip install transformers==4.43.3 sentencepiece==0.2.0 accelerate==0.33.0 protobuf==5
```

## Step4. Script を実行

`test.py`

```python
import torch
from diffusers import  FluxPipeline
import diffusers

_flux_rope = diffusers.models.transformers.transformer_flux.rope
def new_flux_rope(pos: torch.Tensor, dim: int, theta: int) -> torch.Tensor:
    assert dim % 2 == 0, "The dimension must be even."
    if pos.device.type == "mps":
        return _flux_rope(pos.to("cpu"), dim, theta).to(device=pos.device)
    else:
        return _flux_rope(pos, dim, theta)

diffusers.models.transformers.transformer_flux.rope = new_flux_rope

pipe = FluxPipeline.from_pretrained("black-forest-labs/FLUX.1-schnell", revision='refs/pr/1',  torch_dtype=torch.bfloat16).to("mps")

prompt = "japanese girl, photo-realistic"
out = pipe(
     prompt=prompt,
     guidance_scale=0.,
     height=1024,
     width=1024,
     num_inference_steps=4,
     max_sequence_length=256,
).images[0]
out.save("image.png")
```

```shell
python test.py
```

`output`
36GB M3 Mac で大体 250−330 秒くらい掛かりました。  
![output](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*BpQAztQ6WzGzvlADLh-rEA.png)
