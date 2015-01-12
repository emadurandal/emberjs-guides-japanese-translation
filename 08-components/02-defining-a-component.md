# DEFINING A COMPONENT
# コンポーネントの定義

To define a component, create a template whose name starts with
`components/`. To define a new component, `{{blog-post}}` for example,
create a `components/blog-post` template.

コンポーネントを定義するために、`components/`で始まる名前を持つテンプレートを作ります。例えば、新しいコンポーネント`{{blog-post}}`を定義するために、`components/blog-post`テンプレートを作ります。

**Note:** Components must have a dash in their name. So `blog-post` is an acceptable name,
but `post` is not. This prevents clashes with current or future HTML element names, and
ensures Ember picks up the components automatically.
  
**注意：** Componentsは名前に`-`（ダッシュ）を含む必要があります。つまり、`blog-post`は正しい名前ですが、`post`は正しくありません。この規則は、名前が現在又は将来のHTML要素の名前とバッティングすることを避けることになり、また、EmberがComponentを自動的に検出できることを確実にします。

If you are including your Handlebars templates inside an HTML file via
`<script>` tags, it would look like this:

もし`<script>`タグを使ってHTMLファイルの中にあなたのHandlebarsテンプレートを含める場合、そのテンプレートは次のようになるでしょう。

```handlebars
<script type="text/x-handlebars" id="components/blog-post">
  <h1>Blog Post</h1>
  <p>Lorem ipsum dolor sit amet.</p>
</script>
```

If you're using build tools, create a Handlebars file at
`templates/components/blog-post.handlebars`.

もしビルドツールを使う場合は、`templates/components/blog-post.handlebars`にHandlebarsファイルを作ります。

Having a template whose name starts with `components/` creates a
component of the same name. Given the above template, you can now use the
`{{blog-post}}` custom element:

`components/`から始まる名前のテンプレートを持つことで、同じ名前のコンポーネントが生成されます。上述のテンプレートがあったとすると、あなたは`{{blog-post}}`カスタム要素を使うことができます。

```handlebars
<h1>My Blog</h1>
{{#each post in model}}
  {{blog-post}}
{{/each}}
```

<a class="jsbin-embed" href="http://jsbin.com/juvic/embed?js,output">JS Bin</a><script src="http://static.jsbin.com/js/embed.js"></script>

Each component, under the hood, is backed by an element. By default
Ember will use a `<div>` element to contain your component's template.
To learn how to change the element Ember uses for your component, see
[Customizing a Component's
Element](http://emberjs.com/guides/components/customizing-a-components-element).

それぞれのコンポーネントは、その中は、要素によって支えられています。標準では、EmberはあなたのComponentのテンプレートを包むために`<div>`要素を使います。Emberが使用する要素を変更する方法を学ぶためには、[Customizing a Component’s Element（Componentの要素をカスタマイズする）](http://emberjs.com/guides/components/customizing-a-components-element)を参照してください。

### Defining a Component Subclass
### コンポーネントのサブクラスの定義

Often times, your components will just encapsulate certain snippets of
Handlebars templates that you find yourself using over and over. In
those cases, you do not need to write any JavaScript at all. Just define
the Handlebars template as described above and use the component that is
created.

多くの場合、あなたのコンポーネントは、何度も何度も繰り返し使っているようなHandlebarsテンプレートのあるスニペットをただカプセル化するだけです。そのような場合、JavaScriptを書く必要は全くありません。ただ上述したようにHandlebarsテンプレートを定義し、生成されたComponentを使ってください。

If you need to customize the behavior of the component you'll
need to define a subclass of `Ember.Component`. For example, you would
need a custom subclass if you wanted to change a component's element,
respond to actions from the component's template, or manually make
changes to the component's element using JavaScript.

しかし、もしコンポーネントの振る舞いをカスタマイズする必要があるなら、`Ember.Component`のサブクラスを定義する必要があります。例えば、もしコンポーネントの要素を変更したい、またはコンポーネントのテンプレートからのアクションに対応したい、またはJavaScriptを使って手動でコンポーネントの要素を変更したいのであれば、あなたはカスタムのサブクラスを必要とすることになります。

Ember knows which subclass powers a component based on its name. For
example, if you have a component called `blog-post`, you would create a
subclass called `App.BlogPostComponent`. If your component was called
`audio-player-controls`, the class name would be
`App.AudioPlayerControlsComponent`.

EmberはComponentの名前に基づいて、どのサブクラスがComponentに力を与えるか知っています。例えば、もしあなたが`blog-post`というComponentを持っているとすると、`App.BlogPostComponent`というサブクラスをあなたは作ることになるでしょう。もしあなたのコンポーネントが`audio-player-controls`という名前だったら、クラスの名前は`App.AudioPlayerControlsComponent`になります。

In other words, Ember will look for a class with the camelized name of
the component, followed by `Component`.

言い換えれば、Emberは、キャメライズ（複合語をひと綴りとして、要素語の最初を大文字で書き表すこと）されたComponentの名前に`Component`という単語が続く名前のクラスを探すわけです。

<table>
  <thead>
  <tr>
    <th>Component Name</th>
    <th>Component Class</th>
  </tr>
  </thead>
  <tr>
    <td><code>blog-post</code></td>
    <td><code>App.BlogPostComponent</code></td>
  </tr>
  <tr>
    <td><code>audio-player-controls</code></td>
    <td><code>App.AudioPlayerControlsComponent</code></td>
  </tr>
</table>

(The original document’s commit SHA1: a574fb5a7b5b4680fb644d36d47f47021a1d2f2b)