# DISPLAYING A LIST OF ITEMS
# 項目のリストの表示

If you need to enumerate over a list of objects, use Handlebars' `{{#each}}` helper:

もし、オブジェクトのリストを列挙する必要があるなら、Handlebarsの`{{#each}}`ヘルパーを使ってください。

```handlebars
<ul>
  {{#each people}}
    <li>Hello, {{name}}!</li>
  {{/each}}
</ul>
```

The template inside of the `{{#each}}` block will be repeated once for
each item in the array, with the context of the template set to the
current item.

`{{#each}}`ブロックの中のテンプレートは、配列の中の項目ごとに一度、現在の項目が設定されたテンプレートのコンテキストで繰り返されます。

The above example will print a list like this:

上記の例は次のようなリストをプリントします。

```html
<ul>
  <li>Hello, Yehuda!</li>
  <li>Hello, Tom!</li>
  <li>Hello, Trek!</li>
</ul>
```

Like everything in Handlebars, the `{{#each}}` helper is bindings-aware.
If your application adds a new item to the array, or removes an item,
the DOM will be updated without having to write any code.

Handlebarsのすべてに言えるように、`{{#each}}`ヘルパーはバインディングを意識します。もしあなたのアプリケーションが、配列に新しい項目を追加したり、あるいは項目を削除したら、何のコードを書くこともなしに、DOMが更新されます。

There is an alternative form of `{{#each}}` that does not change the
scope of its inner template. This is useful for cases where you need to
access a property from the outer scope within the loop.

内側のテンプレートのスコープを変更しない、`{{#each}}`の代替形式があります。これは、ループの中で、外側のスコープからプロパティにアクセスする必要がある場合に便利です。

```handlebars
{{name}}'s Friends

<ul>
  {{#each friend in friends}}
    <li>{{name}}'s friend {{friend.name}}</li>
  {{/each}}
</ul>
```

This would print a list like this:

これは次のようにリストをプリントします。

```html
Trek's Friends

<ul>
  <li>Trek's friend Yehuda</li>
  <li>Trek's friend Tom!</li>
</ul>
```

The `{{#each}}` helper can have a matching `{{else}}`.
The contents of this block will render if the collection is empty:

`{{#each}}`ヘルパーは対応する`{{else}}`を持つことができます。もしコレクションが空だったときに、このブロックのコンテンツがレンダリングされます。

```handlebars
{{#each people}}
  Hello, {{name}}!
{{else}}
  Sorry, nobody is here.
{{/each}}  
```

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)
