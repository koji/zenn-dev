---
title: "GradioでCSSを使う方法"
emoji: "✨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ['gradio', 'python', 'css']
published: false
---

最近仕事で、 Gradioベースのアプリケーションの見た目をデザイナーのデザインしたものに寄せる作業が発生して、Gradioに無理やりCSSを当てるということをやってました。

本記事では、Gradioサイトに乗っている`Interface`のサンプルを使いますが、`Block`でも全く同じことが可能です。

```py
import gradio as gr

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text")
demo.launch()
```

![original](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*Mn12JuSb64jje2nnw8MDqA.png)


## CSSを当てる方法
方法は非常にシンプルで GradioアプリのコードでCSSを読み込んで、それを`css`に渡すことで GradioでCSSを利用することができます。


```py
import gradio as gr

def load_css():
    with open('style.css', 'r') as file:
        css_content = file.read()
    return css_content

def greet(name):
    return "Hello " + name + "!"

demo = gr.Interface(fn=greet, inputs="text", outputs="text", css=load_css())
demo.launch()
```

```css
button {
  background: red;
  color: #ffffff;
}
```

![styled](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*QSlq1XW5usPNdZ8nZRwNIA.png)


:::message
 Gradio自体がフロントエンドにSveletを使用しているので、結構な頻度で!importantを利用する必要があります。
:::


### class / id を追加する
Gradioのelementにclassやidを追加して、CSSからstyleを当てることも可能です。

`class`を追加
```py
with gr.Blocks(css=css) as demo:
    box1 = gr.Textbox(value="Good Job", elem_classes="feedback")
    box2 = gr.Textbox(value="Failure", elem_id="warning", elem_classes="feedback")
```

`id`を追加
```py
with gr.Blocks(css=css) as demo:
    box1 = gr.Textbox(value="Good Job", elem_classes="feedback")
    box2 = gr.Textbox(value="Failure", elem_id="warning", elem_classes="feedback")
```


## まとめ
GradioでCSSを当てる方法を紹介しました。
ちなみに、HTMLの読み込みも同じよう要領で`gradio.HTML(···)`を利用することで簡単に Gradioに追加できます。jsについてはも似たような感じで利用可能です。詳しくは下記をご確認ください。  
個人的には、あまりの`!important`の利用頻度が高くて、やはり Gradioにスタイルを外部ファイルから当てるのはあんまり良い方法ではない気がしています。 Gradio自体は素晴らしいライブラリだと思いますが、Pythonがメインのチームという場合を除けば、素直にFrontendとBackendを分けて、アプリケーションを作る方が良いと思います。


https://www.gradio.app/guides/custom-CSS-and-JS
