# CHANGING SCOPE
# スコープの変更

Sometimes you may want to invoke a section of your template with a
different context.

ときには、異なるコンテキストでテンプレートの部分を呼び出したいことがあるでしょう。

For example, instead of repeating a long path, like in this example:

たとえば、この例のような、長いパスを繰り返す代わりに……。

```handlebars
Welcome back, <b>{{person.firstName}} {{person.lastName}}</b>!
```

We can use the `{{#with}}` helper to clean it up:

`{{#with}}`ヘルパーを使って、コードを綺麗にすることができます。

```handlebars
{{#with person}}
  Welcome back, <b>{{firstName}} {{lastName}}</b>!
{{/with}}
```

`{{#with}}` changes the _context_ of the block you pass to it. The
context, by default, is the template's controller. By using the `{{#with}}`
helper, you can change the context of all of the Handlebars expressions
contained inside the block.

`{{#with}}`はあなたが通るブロックのコンテキストをそれ（withで指定したもの）に変えます。標準では、コンテキストはテンプレートのコントローラーです。`{{#with}}`ヘルパーを使うことで、ブロックの中に含まれる全てのHandlebarsの式のコンテキストを変更することができます。

Note: it's possible to store the context within a variable for nested 
usage using the "as" keyword:

補足："as"キーワードを使うことで、入れ子の中で使うために、変数の中にコンテキストを保存することができます。

```handlebars
{{#with person as user}}
  {{#each book in books}}
    {{user.firstName}} has read {{book.name}}!
  {{/each}}
{{/with}}
```

(The original document’s commit SHA1: dbb88f88104f40d1233ee9abd999aec97cbd2ada)