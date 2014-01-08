# INTRODUCTION
# 導入

## Controllers

In Ember.js, controllers allow you to decorate your models with 
display logic. In general, your models will have properties that
are saved to the server, while controllers will have properties
that your app does not need to save to the server.

Ember.jsでは、Controllerを使うことで、表示ロジックによってあなたのModelを装飾することができます。一般的には、Modelはサーバーに保存されるプロパティを持ち、一方でControllerはサーバーに保存する必要のないプロパティを持ちます。

For example, if you were building a blog, you would have a
`BlogPost` model that you would present in a `blog_post` template.

例えば、もしあなたがブログを構築していたとすると、あなたは`blog_post`テンプレートに渡す`BlogPost`モデルを持つことになるでしょう。

Your `BlogPost` model would have properties like:

`BlogPost`モデルは次のようなプロパティを持つとします。

* `title`
* `intro`
* `body`
* `author`

Your template would bind to these properties in the `blog_post` 
template:

あなたのテンプレートは、`blog_post`テンプレートでこれらのプロパティーをバインドするとします。

```handlebars
<h1>{{title}}</h1>
<h2>by {{author}}</h2>

<div class='intro'>
  {{intro}}
</div>
<hr>
<div class='body'>
  {{body}}
</div>
```

In this simple example, we don't have any display-specific properties
or actions just yet. For now, our controller just acts as a
pass-through (or "proxy") for the model properties. (Remember that
a controller gets the model it represents from its route handler.)

このシンプルな例では、私たちは今はまだ表示固有のプロパティやActionを何も持ちません。今は、私たちのControllerはモデルプロパティへのパススルー（またはプロキシ）として振る舞うのみです。（ControllerはRoute Handlerからモデルを取得することを思い出してください。）

Let's say we wanted to add a feature that would allow the user to 
toggle the display of the body section. To implement this, we would
first modify our template to show the body only if the value of a 
new `isExpanded` property is true.

bodyセクションの表示/非表示をユーザーがトグルできるようにする機能を私たちが追加したいとしましょう。これを実装するために、まず最初に、新しい`isExpanded`プロパティの値がtrueの時に限りbodyを表示するように、私たちのテンプレートを修正します。

```handlebars
<h1>{{title}}</h1>
<h2>by {{author}}</h2>

<div class='intro'>
  {{intro}}
</div>
<hr>

{{#if isExpanded}}
  <button {{action 'toggleProperty' 'isExpanded'}}>Hide Body</button>
  <div class='body'>
    {{body}}
  </div>
{{else}}
  <button {{action 'toggleProperty' 'isExpanded'}}>Show Body</button>
{{/if}}
```

You might think you should put this property on the model, but 
whether the  body is expanded or not is strictly a display concern.

あなたはこのプロパティをModelに置きたいと思うかもしれません、しかし『bodyを展開するか否か』は厳密には表示に関わる問題です。

Putting this property on the controller cleanly separates logic
related to your data model from logic related to what you display
on the screen. This makes it easy to unit-test your model without
having to worry about logic related to your display creeping into
your test setup.

このプロパティをControllerに置くことは、画面に表示するものに関連したロジックと、データモデルに関連するロジックを綺麗に分離します。このことは、テストセットアップに忍び込んでくる、表示に関するロジックについて心配することなしに、あなたのModelをユニットテストすることを容易にします。

## A Note on Coupling
## 連結についてのメモ

In Ember.js, templates get their properties from controllers, which
decorate a model.

Ember.jsでは、テンプレートはControllerからプロパティを取得します。そしてControllerはモデルを修飾します。

This means that templates _know about_ controllers and controllers
_know about_ models, but the reverse is not true. A model knows
nothing about which (if any) controllers are decorating it, and
controller does not know which views are presenting its properties.

このことは、テンプレートはControllerのことを _知っていて_ 、ControllerはModelのことを _知っている_ ことを意味します。しかし、その逆は真ではありません。Modelは、どの（もしあればですが）ControllerがそのModelを装飾しているか知りませんし、ControllerはどのViewがそのControllerのプロパティを表示しているか知りません。

<figure>
<img src="http://emberjs.com/images/controller-guide/objects.png">
</figure>

This also means that as far as a template is concerned, all of its
properties come from its controller, and it doesn't need to know
about the model directly.

このことはまた、テンプレートに関する限り、テンプレートの全てのプロパティはそのコントローラーから来ており、また、テンプレートはモデルについて直接知る必要がないということを意味します。

In practice, Ember.js will create a template's controller once for
the entire application, but the controller's model may change
throughout the lifetime of the application without requiring that
the view knows anything about those mechanics.

実際のところ、Ember.jsはアプリケーション全体のために一度だけ、テンプレートのControllerを生成します。しかし、Controllerのモデルは、Viewがそうしたメカニズムについて知ることなく、アプリケーションの生存期間を通して変化する可能性があります。

<aside>
For example, if the user navigates from `/posts/1` to `/posts/2`,
the `PostController` will change its model from `Post.find(1)` to
`Post.find(2)`. The template will update its representations of any
properties on the model, as well as any computed properties on the
controller that depend on the model.

例えば、ユーザーが`/posts/1`から`/posts/2`に移動した場合、`PostController`はModelを`Post.find(1)`から`Post.find(2)`に切り替えます。テンプレートはそのModelの全てのプロパティーの表示、さらにModelに依存しているControllerの全てのComputed Propertiesを更新します。
</aside>

This makes it easy to test a template in isolation by rendering it 
with a controller object that contains the properties the template
expects. From the template's perspective, a **controller** is simply
an object that provides its data.

このことは、テンプレートが期待するプロパティを含んだControllerオブジェクトで、テンプレートをレンダリングすることから隔絶した状態で、テンプレートをテストすることを容易にします。テンプレートの視点からは、 **Controller** は単にそのデータを提供するオブジェクトに過ぎません。

### Representing Models
### モデルの表現

Templates are always connected to controllers, not models. This 
makes it easy to separate display-specific properties from model 
specific properties, and to swap out the controller's model as the
user navigates around the page.

テンプレートは常に、Modelではなく、Controllerに接続されています。このことは、Model固有のプロパティから表示固有のプロパティを切り離し、また、ユーザーがページを移動するのに応じて、ControlllerのModelを切り替えることを容易にします。

For convenience, Ember.js provides controllers that _proxy_ 
properties from their models so that you can say `{{name}}` in your
template rather than `{{model.name}}`. An `Ember.ArrayController` 
proxies properties from an Array, and an `Ember.ObjectController` 
proxies properties from an object.

便宜上、Ember.jsはモデルからプロパティを _プロキシする_ コントローラーを提供します。そのおかげで、あなたはテンプレートで`{{model.name}}`とせずに、`{{name}}`と書くことができるのです。

If your controller is an `ArrayController`, you can iterate directly
over the controller using `{{#each controller}}`. This keeps the
template from having to know about how the controller is implemented
and makes isolation testing and refactoring easier.

もしあなたのControllerが`ArrayController`なら、`{{#each controller}}`を使うことで、直接Controllerをイテレート（繰り返し取り出すことが）できます。このことは、Controllerがどのように実装されるかについて、テンプレートが知らなくてもいいようにし、そして隔離テストとリファクタリングをより容易にします。

### Storing Application Properties
### アプリケーションのプロパティの保存

Not all properties in your application need to be saved to the 
server. Any time you need to store information only for the lifetime
of this application run, you should store it on a controller.

あなたのアプリケーションの全てのプロパティが、サーバーに保存する必要があるというわけではありません。アプリケーションの生存時間の間だけ、情報を保存する必要があるときはいつでも、あなたはそれらの情報をControllerに保存すべきです。

For example, imagine your application has a search field that
is always present. You could store a `query` property on your
`ApplicationController`, and bind the `search` field in the `
application` template to that property.

例えば、あなたのアプリケーションが、常に表示されている検索フィールドを持っていると想像してください。あなたは`query`プロパティを`ApplicationController`に保存し、`application`テンプレートにある`search`フィールドをそのプロパティにバインドすることができます。

```handlebars
<!-- application.handlebars -->
<header>
  {{input type="text" value=search action="query"}}
</header>

{{outlet}}
```

```javascript
App.ApplicationController = Ember.Controller.extend({
  // the initial value of the `search` property
  search: '',

  actions: {
    query: function() {
      // the current value of the text field
      var query = this.get('search');
      this.transitionToRoute('search', { query: query });
    }
  }
});
```

The `application` template stores its properties and sends its 
actions to the `ApplicationController`. In this case, when the user
hits enter, the application will transition to the `search` route,
passing the query as a parameter.

`application`テンプレートはそのプロパティを保存し、`ApplicationController`にActionを送ります。この場合、ユーザーがエンターキーを押したとき、アプリケーションは`search`Routeに遷移し、パラメーターとしてqueryを渡します。

(The original document’s commit SHA1: d9dd7c341815a7182e4016e6ea015da03c0bdb2a)