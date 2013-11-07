# INTRODUCTION
# イントロダクンション

HTML was designed in a time when the browser was a simple document
viewer. Developers building great web apps need something more.

HTMLは、Webブラウザーがシンプルなドキュメントビューワーだったころに設計されました。素晴らしいWebアプリケーションを作る開発者たちは、もっと何かを必要としています。

Instead of trying to replace HTML, however, Ember.js embraces it, then adds
powerful new features that modernize it for building web apps.

HTMLを置き換えることを試みるのではなく、Ember.jsはHTMLを受け入れ、その上で、Webアプリケーションを構築するためにHTMLを近代的にするパワフルな新しい機能を付け加えます。

Currently, you are limited to the tags that are created for you by the
W3C. Wouldn't it be great if you could define your own,
application-specific HTML tags, then implement their behavior using
JavaScript?

現在、あなたはW3Cによって作られたタグに制約されています。もし、アプリケーション特有のHTMLタグをあなた自身で定義することができ、それらの動作をJavaScriptで実装することができるなら、すばらしいと思いませんか？

That's exactly what components let you do. In fact, it's such a good
idea that the W3C is currently working on the [Custom
Elements](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/custom/index.html)
spec.

それこそ、まさにあなたがComponentsでできることなのです。実際、このアイデアはW3Cが [Custom Elements](https://dvcs.w3.org/hg/webcomponents/raw-file/tip/spec/custom/index.html) として現在策定を進めているほど良いアイデアなのです。

Ember's implementation of components hews as closely to the Web
Components specification as possible. Once Custom Elements are widely
available in browsers, you should be able to easily migrate your Ember
components to the W3C standard and have them be usable by other
frameworks.

Emberのコンポーネントの実装は、できる限り緊密にWeb Components仕様に従うようにしています。いったん、Custom Elementsがブラウザーで広く利用可能になったら、あなたが簡単にEmberコンポーネントからW3C標準に移行できるようにしますし、他のフレームワークからもそれらが利用可能になるようにします。

This is so important to us that we are working closely with the
standards bodies to ensure our implementation of components matches the
roadmap of the web platform.

私たちのコンポーネントの実装がWebプラットフォームのロードマップに合致するよう、標準化団体と緊密に連携することは私たちにとってとても重要です。

To highlight the power of components, here is a short example of turning a blog post into a reusable
`blog-post` custom element that you could use again and again in your
application. Keep reading this section for more details on building
components.

コンポーネントの力をお見せするため、ブログの投稿を、あなたがご自分のアプリケーションで何度も何度も使えるような、再利用可能な`blog-post`カスタム要素に変化させる短いサンプルを作りました。Componentsを構築するにあたってのさらなる詳細について、このセクションを引き続きお読みください。

<a class="jsbin-embed" href="http://jsbin.com/ifuxey/2/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

(The original document’s commit SHA1: e3ad91db2f2726d1118adac87b2f7e488da03ab6)
