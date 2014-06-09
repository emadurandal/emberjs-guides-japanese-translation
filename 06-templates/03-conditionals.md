# CONDITIONALS
# 条件文

Sometimes you may only want to display part of your template if a property
exists.

時には、プロパティが存在するときにだけ、テンプレートの一部を表示したいということがあるでしょう。

We can use the `{{#if}}` helper to conditionally render a block:

ブロックを条件的にレンダリングするために、`{{#if}}`ヘルパーを使うことができます。

```handlebars
{{#if person}}
  Welcome back, <b>{{person.firstName}} {{person.lastName}}</b>!
{{/if}}
```

Handlebars will not render the block if the argument passed evaluates to
`false`, `undefined`, `null` or `[]` (i.e., any "falsy" value).

もし引数が`false`、`undefined`、`null`または`[]`（空の配列）（つまり、falseと同類と見なされそうな値）と評価された場合は、Handlebarsはそのブロックをレンダリングしません。

If the expression evaluates to falsy, we can also display an alternate template
using `{{else}}`:

もし式が”falsy”（falseと同類と見なされそうな値）と評価されたら、`{{else}}`を使う事で、替わりのテンプレートを表示することもできます。

```handlebars
{{#if person}}
  Welcome back, <b>{{person.firstName}} {{person.lastName}}</b>!
{{else}}
  Please log in.
{{/if}}
```

To only render a block if a value is falsy, use `{{#unless}}`:

もし値が”falsy”の場合のみ、ブロックをレンダリングしたいなら、`{{#unless}}`を使ってください。

```handlebars
{{#unless hasPaid}}
  You owe: ${{total}}
{{/unless}}
```

`{{#if}}` and `{{#unless}}` are examples of block expressions. These allow you
to invoke a helper with a portion of your template. Block expressions look like
normal expressions except that they contain a hash (#) before the helper name,
and require a closing expression.

`{{#if}}`と`{{#unless}}`はブロック式の例です。これらを使う事で、あなたはテンプレートの一部分を伴ってヘルパーを呼び出すことができます。ブロック式は、ヘルパー名の前にハッシュ（#）を含み、終わりの式を必要とすること以外は、通常の式のように見えます。

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)