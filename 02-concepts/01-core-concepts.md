# CORE CONCEPTS
# コア・コンセプト

To get started with Ember.js, there are a few core concepts you
should understand. 

Ember.jsを始めるために、理解すべきいくつかのコア・コンセプトがあります。

Ember.js is designed to help developers build ambitiously large web
applications that are competitive with native apps. Doing so requires
both new tools and a new vocabulary of concepts. We've spent a lot of
time borrowing ideas pioneered by native application frameworks like
Cocoa and Smalltalk.

Ember.jsはネイティブアプリケーションに匹敵するほどの大きなWebアプリケーションを野心的に作ろうとする開発者を助けるように設計されています。そうすることは、新しいツールとコンセプトについての新しい語彙を必要とします。私たちは、CocoaとSmalltalkのようなネイティブアプリケーション・フレームワークによって開拓されたアイデアを借りることに、多くの時間を費やしました。

However, it's important to remember what makes the web special. Many
people think that something is a web application because it uses
technologies like HTML, CSS and JavaScript. In reality, these are just
implementation details.

しかし、何がウェブを特別なものにしているかを、覚えていることが重要です。多くの人たちは、HTMLやCSS、JavaScriptのような技術を使っていれば、それがWebアプリケーションだと考えています。実際には、これらは単に実装の詳細に過ぎません。

Instead, **the web derives its power from the ability to bookmark and
share URLs.** URLs are the key feature that give web applications
superior shareability and collaboration. Today, most JavaScript
frameworks treat the URL as an afterthought, instead of the primary
reason for the web's success.

むしろ、 **Webは、ブックマークして、URLを共有する能力からその力を得ます。** URLは、Webアプリケーションに優れた共有性とコラボレーションを与える鍵となる特徴です。
今日、ほとんどのJavaScriptフレームワークは、URLをウェブの成功の一番の要因ではなく、後付の補足的なものとして扱っています。

Ember.js, therefore, marries the tools and concepts of native
GUI frameworks with support for the feature that makes the web so
powerful: the URL.

Ember.jsは、したがって、ツールとネイティブGUIフレームワークのコンセプトを、Webをこれほどまでに強力にしている機能、つまり「URL」、をサポートすることに結びつけました。

### Concepts
### コンセプト

#### Templates
#### テンプレート

A **template**, written in the Handlebars templating language, describes
the user interface of your application. Each template is backed by a
model, and the template automatically updates itself if the model changes.

テンプレート（Handlebersテンプレート言語で書かれます）：アプリケーションのユーザーインターフェースを表現します。それぞれのテンプレートはモデルに支えられており、モデルが変更されれば、テンプレートは自身を自動的に更新します。

In addition to plain HTML, templates can contain:

普通のHTMLに加え、テンプレートは以下のものを含むことが出来ます。

* **Expressions**, like `{{firstName}}`, which take information from
  the template's model and put it into HTML.
  
  式（{{firstName}}のような）：テンプレートのモデルから情報を取得し、それをHTML内に埋め込みます。
* **Outlets**, which are placeholders for other templates. As users
  move around your app, different templates can be plugged into the
  outlet by the router. You can put outlets into your template using the
  `{{outlet}}` helper.
  
  アウトレット：他のテンプレートのためのプレースホルダです。ユーザーがあなたのアプリの中を移動すると、異なるテンプレートがルータによってアウトレットに挿入されます。あなたは`{{outlet}}`ヘルパーを使って、テンプレートにアウトレットを配置できます。  
* **Components**, custom HTML elements that you can use to clean up
  repetitive templates or create reusable controls.
  
  コンポーネント：反復的なテンプレートを整理するため、あるいは、再使用可能なコントロールをつくるために、使用できるカスタムHTML要素です。

#### Router
#### ルーター

The **router** translates a URL into a series of nested templates, each
backed by a model. As the templates or models being shown to the user
change, Ember automatically keeps the URL in the browser's address bar
up-to-date.

ルーターはURLを一連のネストされたテンプレートに変換します。テンプレートはそれぞれモデルによって支えられています。ユーザーに表示されているテンプレートまたはモデルが変化すると、EmberはブラウザのアドレスバーのURLを自動的に最新に保ちます。

This means that, at any point, users are able to share the URL of your
app. When someone clicks the link, they reliably see the same content as
the original user.

これは、いかなる点でも、ユーザーがあなたのアプリケーションのURLを共有することができることを意味します。誰かがリンクをクリックするとき、彼らは最初のユーザーと同じ内容を確実に見ることができるのです。

#### Components
#### コンポーネント

A **component** is a custom HTML tag whose behavior you implement using
JavaScript and whose appearance you describe using Handlebars templates.
They allow you to create reusable controls that can simplify your
application's templates.

コンポーネントは、あなたがJavaScriptを使ってその動作を実装でき、また、あなたがHandlebarsテンプレートを使ってその外観を表現できる、カスタムのHTMLタグです。コンポーネントを使うことで、あなたは再利用可能なコントロールを作成でき、それによってアプリケーションのテンプレートをシンプルにすることができます。

#### Models
#### モデル

A **model** is an object that stores _persistent state_. Templates are
responsible for displaying the model to the user by turning it into
HTML. In many applications, models are loaded via an HTTP JSON API,
although Ember is agnostic to the backend that you choose.

モデルは永続的な状態を保存するオブジェクトです。テンプレートは、モデルをHTMLに変換することによって、そのモデルをユーザーに表示する役割を果たします。多くのアプリケーションでは、モデルはHTTP JSON APIを通じて読み込まれます。しかし、Emberはあなたが選ぶバックエンドについては関知しません。

#### Route
#### ルート

A **route** is an object that tells the template which model it should
display.

ルートは、テンプレートにどのモデルを表示すべきかを伝えるオブジェクトです。

#### Controllers
#### コントローラー

A **controller** is an object that stores _application state_. A
template can optionally have a controller in addition to a model, and
can retrieve properties from both.

コントローラーはアプリケーションの状態を保存するオブジェクトです。テンプレートはモデルに加えてコントローラを任意に持つことができ、両方からプロパティを読み出すことができます。

---

These are the core concepts you'll need to understand as you develop
your Ember.js app. They are designed to scale up in complexity, so that
adding new functionality doesn't force you to go back and refactor major 
parts of your app.

これらが、あなたがEmber.jsアプリケーションを開発する際に理解しておかなければならないコア・コンセプトです。これらは複雑さを必要に応じて増加させることができるように設計されており、そのため、新しい機能を追加することによって、あなたのアプリケーションの大部分が後戻りになったり、リファクタリングしなければならなくなる、ということがありません。

Now that you understand the roles of these objects, you're equipped to
dive deep into Ember.js and learn the details of how each of these
individual pieces work.

これらのオブジェクトの役割を理解した今、あなたは、Ember.jsの中に深く飛び込んで、これらの個々のピースのそれぞれがどのように働くか、という詳細を学ぶ準備ができました。

(The original document’s commit SHA1: 72b1b974db9270404a692a340440464205341a4e)
