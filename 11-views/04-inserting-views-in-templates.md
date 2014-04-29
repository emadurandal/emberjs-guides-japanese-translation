# INSERTING VIEWS IN TEMPLATES
# テンプレートでのViewの挿入

So far, we've discussed writing templates for a single view. However, as your application grows, you will often want to create a hierarchy of views to encapsulate different areas on the page. Each view is responsible for handling events and maintaining the properties needed to display it.

今まで、単一のViewのためのテンプレートを書くことについて議論してきました。しかし、アプリケーションが大きくなるにつれて、あなたはしばしば、ページの異なるエリアをカプセル化するために、Viewの階層を作りたいと思うようになるでしょう。
それぞれのViewは、イベントをハンドリングし、表示されるべきプロパティを保持する責任を負います。

### {{view}}

To add a child view to a parent, use the `{{view}}` helper, which takes a path to a view class.

親Viewに子Viewを追加するには、`{{view}}`ヘルパーを使います。`{{view}}}`ヘルパーはViewクラスを経由します。

```javascript
// Define parent view
App.UserView = Ember.View.extend({
  templateName: 'user',

  firstName: "Albert",
  lastName: "Hofmann"
});

// Define child view
App.InfoView = Ember.View.extend({
  templateName: 'info',

  posts: 25,
  hobbies: "Riding bicycles"
});
```

```html
<script type="text/x-handlebars" data-template-name="user">
  User: {{view.firstName}} {{view.lastName}}
  {{view App.InfoView}}
</script>
```

```html
<script type="text/x-handlebars" data-template-name="info">
  <b>Posts:</b> {{view.posts}}
  <br>
  <b>Hobbies:</b> {{view.hobbies}}
</script>
```

If we were to create an instance of `App.UserView` and render it, we would get
a DOM representation like this:

もし、`App.UserView`のインスタンスを生成し、それをレンダリングしたら、私たちは次のようなDOM表現を得るでしょう。

```html
User: Albert Hofmann
<div>
  <b>Posts:</b> 25
  <br>
  <b>Hobbies:</b> Riding bicycles
</div>
```

#### Relative Paths
#### 相対パス

Instead of specifying an absolute path, you can also specify which view class
to use relative to the parent view. For example, we could nest the above view
hierarchy like this:

絶対パスを指定する代わりに、あなたは親のViewに対してどのViewクラスを使うべきかを指定することもできます。例えば、あなたは上述したViewを次のように階層的に入れ子にすることができます。

```javascript
App.UserView = Ember.View.extend({
  templateName: 'user',

  firstName: "Albert",
  lastName: "Hofmann",

  infoView: Ember.View.extend({
    templateName: 'info',

    posts: 25,
    hobbies: "Riding bicycles"
  })
});
```

```handlebars
User: {{view.firstName}} {{view.lastName}}
{{view view.infoView}}
```

When nesting a view class like this, make sure to use a lowercase
letter, as Ember will interpret a property with a capital letter as a
global property.

このようにViewクラスを入れ子にするとき、小文字を使うようにしてください。Emberは大文字のプロパティをグローバルプロパティとして解釈します。

### Setting Child View Templates
### 子Viewのテンプレートのセッティング

If you'd like to specify the template your child views use inline in
the main template, you can use the block form of the `{{view}}` helper.
We might rewrite the above example like this:

もしあなたがメインテンプレートの中にインラインで、子Viewが使うテンプレートを指定したいのなら、ブロック形式の`{{view}}`ヘルパーを使うことができます。上の例は次のように書き直すことができます。

```javascript
App.UserView = Ember.View.extend({
  templateName: 'user',

  firstName: "Albert",
  lastName: "Hofmann"
});

App.InfoView = Ember.View.extend({
  posts: 25,
  hobbies: "Riding bicycles"
});
```

```handlebars
User: {{view.firstName}} {{view.lastName}}
{{#view App.InfoView}}
  <b>Posts:</b> {{view.posts}}
  <br>
  <b>Hobbies:</b> {{view.hobbies}}
{{/view}}
```

When you do this, it may be helpful to think of it as assigning views to
portions of the page. This allows you to encapsulate event handling for just
that part of the page.

このやり方については、Viewをページの部分に割り当てることと考えると分かりやすいでしょう。このやり方により、ちょうどページのその部分のためにイベントハンドリングをカプセル化できます。


(The original document’s commit SHA1: 1b78086df60d5866d42c80df09e3a7df7eaf9618)