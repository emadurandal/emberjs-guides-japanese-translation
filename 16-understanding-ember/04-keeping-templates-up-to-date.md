## KEEPING TEMPLATES UP-TO-DATE
## テンプレートを最新に保つ

In order to know which part of your HTML to update when an underlying property changes, Handlebars will insert marker elements with a unique ID. If you look at your application while it's running, you might notice these extra elements:

依存しているプロパティが変化したとき、あなたのHTMLのどの部分が更新されるのか知るために、HandlebarsはユニークなIDで目印となる要素を挿入します。あなたの稼働中のアプリケーションを見てみると、これらの要素に気付くことでしょう。

```html
My new car is
<script id="metamorph-0-start" type="text/x-placeholder"></script>
blue
<script id="metamorph-0-end" type="text/x-placeholder"></script>.
```

Because all Handlebars expressions are wrapped in these markers, make sure each HTML tag stays inside the same block. For example, you shouldn't do this:

すべてのHandlebars式はこれらの目印要素でラップされるため、各HTMLタグが同じブロックの中に存在するようにしてください。たとえば、あなたは次のようなことはすべきではありません。

```handlebars
{{! Don't do it! }}
<div {{#if isUrgent}}class="urgent"{{/if}}>
```

If you want to avoid your property output getting wrapped in these markers, use the `unbound` helper:

もし、プロパティの出力がこれらの目印要素でラップされるのを避けたいのなら、`unbound`ヘルパーを使ってください。

```handlebars
My new car is {{unbound color}}.
```

Your output will be free of markers, but be careful, because the output won't be automatically updated!

あなたの出力は目印要素から開放されます。しかし、気をつけてください。その出力はもう自動的に更新されなくなります。

```html
My new car is blue.
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)