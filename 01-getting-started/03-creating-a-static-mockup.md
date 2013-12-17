# CREATING A STATIC MOCKUP
# 静的なモックアップの作成

Before adding any code, we can roughly sketch out the layout of our application. In your text editor, create a new file and name it `index.html`. This file will contain the HTML templates of our completed application and trigger requests for the additional image, stylesheet, and JavaScript resources.

コードを追加する前に、私たちはアプリケーションのレイアウトを（静的なモックアップを作ることで）粗くスケッチすることができます。あなたのテキストエディタで、新しいファイルを作り、名前を`index.html`としてください。このファイルは完成されたアプリケーションのHTMLテンプレートを含み、また追加の画像、スタイルシート、そしてJavaScriptリソースのリクエストをトリガします。

To start, add the following text to `index.html`:

手始めとして、次のテキストを`index.html`に追加してください。

```html
<!doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Ember.js • TodoMVC</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <section id="todoapp">
      <header id="header">
        <h1>todos</h1>
        <input type="text" id="new-todo" placeholder="What needs to be done?" />
      </header>

      <section id="main">
        <ul id="todo-list">
          <li class="completed">
            <input type="checkbox" class="toggle">
            <label>Learn Ember.js</label><button class="destroy"></button>
          </li>
          <li>
            <input type="checkbox" class="toggle">
            <label>...</label><button class="destroy"></button>
          </li>
          <li>
            <input type="checkbox" class="toggle">
            <label>Profit!</label><button class="destroy"></button>
          </li>
        </ul>

        <input type="checkbox" id="toggle-all">
      </section>

      <footer id="footer">
        <span id="todo-count">
          <strong>2</strong> todos left
        </span>
        <ul id="filters">
          <li>
            <a href="all" class="selected">All</a>
          </li>
          <li>
            <a href="active">Active</a>
          </li>
          <li>
            <a href="completed">Completed</a>
          </li>
        </ul>

        <button id="clear-completed">
          Clear completed (1)
        </button>
      </footer>
    </section>

    <footer id="info">
      <p>Double-click to edit a todo</p>
    </footer>
  </body>
</html>
```

The associated [stylesheet](http://emberjs.com.s3.amazonaws.com/getting-started/style.css) and [background image](http://emberjs.com.s3.amazonaws.com/getting-started/bg.png) for this project should be downloaded and placed in the same directory as `index.html`

このプロジェクトの関連するスタイルシートと背景画像はダウンロードして、`index.html`と同じディレクトリに保存してください。

Open `index.html` in your web browser to ensure that all assets are loading correctly. You should see the TodoMVC application with three hard-coded `<li>` elements where the text of each todo will appear.

すべてのアセットが正常にロードされていることを確認するために、`index.html`をウェブブラウザで開いてください。あなたは、三つのハードコードされた（各Todoのテキストが表示される場所である）`<li>`要素を持ったTodoMVCアプリケーションを確認できるはずです。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/uduyip/2/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script> 

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/4d91f9fa1f6be4f4675b54babd3074550095c930)
  * [TodoMVC stylesheet](http://emberjs.com.s3.amazonaws.com/getting-started/style.css)
  * [TodoMVC background image](http://emberjs.com.s3.amazonaws.com/getting-started/bg.png)
  
(The original document’s commit SHA1: 59426fa314bd33f017a8443c83c47856b6b0e714)