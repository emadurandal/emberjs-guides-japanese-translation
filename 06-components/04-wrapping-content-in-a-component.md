# WRAPPING CONTENT IN A COMPONENT
# Componentの中でコンテンツを内包する

Sometimes, you may want to define a component that wraps content
provided by other templates.

ときには、他のテンプレートの内容を内包するComponentを定義したいと思う事があるでしょう。

For example, imagine we are building a `blog-post` component that we can
use in our application to display a blog post:

例えば、ブログの投稿を表示するためにアプリケーションで使うことのできる、`blog-post`Componentを構築することを考えましょう。

```handlebars
<script type="text/x-handlebars" id="components/blog-post">
  <h1>{{title}}</h1>
  <div class="body">{{body}}</div>
</script>
```

Now, we can use the `{{blog-post}}` component and pass it properties
in another template:

さて、これで私たちは`{{blog-post}}`Componentを使い、またそれに他のテンプレートのプロパティーを渡すことができます。

```handlebars
{{blog-post title=title body=body}}
```

<a class="jsbin-embed" href="http://jsbin.com/obogub/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

(See [Passing Properties to a
Component](http://emberjs.com/guides/components/passing-properties-to-a-component/) for
more.)

（詳細は、[Passing Properties to a Component（Componentにプロパティーを渡す）](http://emberjs.com/guides/components/passing-properties-to-a-component/)（[日本語版](https://github.com/emadurandal/emberjs-guides-japanese-translation/blob/master/06-components/03-passing-properties-to-a-component.md)）を参照してください）

In this case, the content we wanted to display came from the model. But
what if we want the developer using our component to be able to provide custom
HTML content?

この場合、私たちが表示したい内容はモデルから来ます。しかし、私たちのComponentを使っている開発者が、カスタムのHTMLコンテンツを供給できようにしてあげたい場合はどうすればよいでしょう？

In addition to the simple form you've learned so far, components also
support being used in **block form**. In block form, components can be
passed a Handlebars template that is rendered inside the component's
template wherever the `{{yield}}` expression appears.

あなたが今まで学んできたシンプルな形式に加えて、Componentは **ブロック形式** のなかでの利用もサポートしているのです。ブロック形式では、`{{yield}}`式が現れている場所ならどこでも、Componentに、Componentのテンプレートの中にレンダリングされるHandlebarsテンプレートを渡す事ができます。

To use the block form, add a `#` character to the
beginning of the component name, then make sure to add a closing tag.
(See the Handlebars documentation on [block expressions](http://handlebarsjs.com/#block-expressions) for more.)

ブロック形式を使うために、コンポーネント名の最初に`#`を付けます。それから、確実に閉じタグを付けます。（詳細は、[block expressions](http://handlebarsjs.com/#block-expressions)のHandlebarsドキュメントを参照してください）

In that case, we can use the `{{blog-post}}` component in **block form**
and tell Ember where the block content should be rendered using the
`{{yield}}` helper. To update the example above, we'll first change the component's
template:

そのような場合では、ブロック形式で`{{blog-post}}`Componentを使うことができ、`{{yield}}`ヘルパーを使って、どこにブロック内容がレンダリングされるべきか、Emberに伝えることができます。

```handlebars
<script type="text/x-handlebars" id="components/blog-post">
  <h1>{{title}}</h1>
  <div class="body">{{yield}}</div>
</script>
```

You can see that we've replaced `{{body}}` with `{{yield}}`. This tells
Ember that this content will be provided when the component is used.

`{{body}}`を`{{yield}}`で置き換えたのがわかりますね。この記述の変更はEmberに、このComponentが使われたとき、このコンテンツが供給されるということを伝えます。

Next, we'll update the template using the component to use the block
form:

次に、コンポーネントを使っているテンプレートを、ブロック形式を使うように更新します。

```handlebars
{{#blog-post title=title}}
  <p class="author">by {{author}}</p>
  {{body}}
{{/blog-post}} 
```

<a class="jsbin-embed" href="http://jsbin.com/osulic/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

It's important to note that the template scope inside the component
block is the same as outside. If a property is available in the template
outside the component, it is also available inside the component block.

重要なことは、Componentブロックの中のテンプレートスコープは、外側のものと同じであるということです。もし、プロパティがコンポーネントの外のテンプレートで得られるなら、そのプロパティはComponentブロックの中でも得られるのです。

This JSBin illustrates the concept:

このJSBinデモが、そのコンセプトを解説します。

<a class="jsbin-embed" href="http://jsbin.com/iqocuf/1/embed?live">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

(The original document’s commit SHA1: 4f561e7df140363fba06f06281138caa10582110)