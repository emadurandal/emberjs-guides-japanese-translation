# THE APPLICATION TEMPLATE
# Applicationテンプレート

The application template is the default template that is rendered when
your application starts. 

applicationテンプレートはあなたのアプリケーションが起動した時にレンダリングされるデフォルトのテンプレートです。

You should put your header, footer, and any other decorative content
here. Additionally, you should have at least one `{{outlet}}`:
a placeholder that the router will fill in with the appropriate template,
based on the current URL.

あなたはヘッダーやフッター、そして他の装飾的なコンテンツをここ（Applicationテンプレート）に置いてください。加えて、あなたはそこに少なくとも一つの`{{outlet}}`を置いてください。`{{outlet}}`は、現在のURLに基づいて、ルーターによって適切なテンプレートで埋められるプレースホルダーです。

Here's an example template:

例となるテンプレートを以下に示します。

```handlebars
<header>
  <h1>Igor's Blog</h1>
</header>

<div>
  {{outlet}}
</div>

<footer>
  &copy;2013 Igor's Publishing, Inc.
</footer>
```

The header and footer will always be displayed on screen, but the
contents of the `<div>` will change depending on if the user is
currently at `/posts` or `/posts/15`, for example.

ヘッダーとフッターは画面に常に表示されます。しかし、`<div>`の内容は、例えばユーザーが現在`/posts`に居るか、または`/posts/15`に居るかによって、変化します。

For more information about how outlets are filled in by the router, see
[Routing](/guides/routing).

アウトレットがルーターによって、どのように（コンテンツを）埋め込まれるのかについて、より多くの情報を得るには、Routingのセクションを参照してください。

If you are keeping your templates in HTML, create a `<script>` tag
without a template name. It will automatically be compiled and appended
to the screen.

もしあなたがテンプレートをHTMLの中に保持しているなら、`<script>`タグをテンプレート名なしで作成してください。そのテンプレートは自動的にコンパイルされ、スクリーンに追加されます。

```html
<script type="text/x-handlebars">
  <div>
    {{outlet}}
  </div>
</script>
```

If you're using build tools to load your templates, make sure you name
the template `application`.

もしあなたがテンプレートを読み込むためのビルドツールを使っているなら、テンプレートの名前を`application`にすることを確実にしてください。

(The original document’s commit SHA1: 5ef1bb0513231201a440d4423c3f2792a320ae06)