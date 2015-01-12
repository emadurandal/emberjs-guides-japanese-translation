# PASSING PROPERTIES TO A COMPONENT
# コンポーネントにプロパティーを渡す

By default a component does not have access to properties in the
template scope in which it is used.

標準では、Componentはそれが使われているテンプレートスコープのプロパティへのアクセス手段を持っていません。

For example, imagine you have a `blog-post` component that is used to
display a blog post:

例えば、ブログの投稿を表示するのに使われる`blog-post`コンポーネントがあると想像してください。

```handlebars
<script type="text/x-handlebars" id="components/blog-post">
  <h1>Component: {{title}}</h1>
  <p>Lorem ipsum dolor sit amet.</p>
</script>
```

You can see that it has a `{{title}}` Handlebars expression to print the
value of the `title` property inside the `<h1>`.

このComponentが、`<h1>`タグの内側に、`title`プロパティの値を表示するための`{{title}}`Handlebars式を持っていることがわかりますね。

Now imagine we have the following template and route:

さて、ここで次のようなテンプレートとRouteがあると想像してください。

```js
App.IndexRoute = Ember.Route.extend({
  model: function() {
    return {
      title: "Rails is omakase"
    };
  }
});
```

```handlebars
{{! index.handlebars }}
<h1>Template: {{title}}</h1>
{{blog-post}}
```

Running this code, you will see that the first `<h1>` (from the outer
template) displays the `title` property, but the second `<h1>` (from
inside the component) is empty.

このコードを実行すると、最初の（外側のテンプレートの）`<h1>`タグが`title`プロパティを表示し、しかし二つ目の（コンポーネントの中の）`<h1>`が空であることがわかると思います。

<a class="jsbin-embed" href="http://jsbin.com/ufedet/2/embed?live">JS Bin</a>

We can fix this by making the `title` property available to the
component:

この動作は、titleプロパティをComponent側で利用可能にすることで修正できます。

```handlebars
{{blog-post title=title}}
```

This will make the `title` property in the outer template scope
available inside the component's template using the same name, `title`.

<a class="jsbin-embed" href="http://jsbin.com/ufedet/3/embed?live">JS Bin</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

If, in the above example, the model's `title` property was instead
called `name`, we would change the component usage to:

上の例でもし、Modelの`title`プロパティが代わりに`name`という名前だったら、次のようにComponentの使い方を変えます。

```
{{blog-post title=name}}
```

<a class="jsbin-embed" href="http://jsbin.com/ufedet/4/embed?live">JS Bin</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

In other words, you are binding a named property from the outer scope to
a named property in the component scope, with the syntax
`componentProperty=outerProperty`.

言い換えれば、`componentProperty=outerProperty`という構文を使って、外側のスコープのプロパティ名をコンポーネントスコープのプロパティ名にバインドするのです。

It is important to note that the value of these properties is bound.
Whether you change the value on the model or inside the component, the
values stay in sync. In the following example, type some text in the
text field either in the outer template or inside the component and note
how they stay in sync.

これらのプロパティの値はバインドされていることに留意することが重要です。あなたが変更するのがモデルの値かコンポーネントの値かにかかわらず、それらの値は同期されるのです。次の例で、外側のテンプレートのテキストフィールド、またはコンポーネントのテキストフィールドに何らかのテキストを入力し、これらが同期される様子を確認してください。

<a class="jsbin-embed" href="http://jsbin.com/ufedet/5/embed?live">JS Bin</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

You can also bind properties from inside an `{{#each}}` loop. This will
create a component for each item and bind it to each model in the loop.

`{{#each}}`ループの中からでも、プロパティをバインドすることができます。このコードは、それぞれの項目ごとにComponentを作成し、ループの中のそれぞれのモデルに、プロパティをバインドします。

```handlebars
{{#each post in model}}
  {{blog-post title=post.title}}
{{/each}}
```
<a class="jsbin-embed" href="http://jsbin.com/japiv/1/embed?live">JS Bin</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

(The original document’s commit SHA1: ec0c79f78dad7b464efc5cb009ecf045a51f6a44)