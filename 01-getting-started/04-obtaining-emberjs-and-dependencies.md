# OBTAINING EMBER.JS AND DEPENDENCIES
# Ember.jsと依存ファイルの取得
TodoMVC has a few dependencies:

TodoMVCはいくつかのファイルに依存しています。
  
  * [jQuery](http://code.jquery.com/jquery-1.10.2.min.js)
  * [Handlebars](http://builds.handlebarsjs.com.s3.amazonaws.com/handlebars-1.0.0.js)
  * [Ember.js 1.3](http://builds.emberjs.com/tags/v1.3.0/ember.js)
  * [Ember Data 1.0 beta](http://builds.emberjs.com/tags/v1.0.0-beta.5/ember-data.js)

For this example, all of these resources should be stored in the folder `js/libs` located in the same location as `index.html`. Update your `index.html` to load these files by placing `<script>` tags just before your closing `</body>` tag in the following order:

この例題のために、これらの全てのリソースは`index.html`と同じ階層に`js/lib`というフォルダを作り、そこに保存してください。`</body>`タグの直前に、次の順番で`<script>`タグを記述することで、これらのファイルを読み込むよう、`index.html`を更新してください。

```html
<!-- ... additional lines truncated for brevity ... -->
  <script src="js/libs/jquery-1.10.2.min.js"></script>
  <script src="js/libs/handlebars-1.0.0.js"></script>
  <script src="js/libs/ember.js"></script>
  <script src="js/libs/ember-data.js"></script>
</body>
<!-- ... additional lines truncated for brevity ... -->
```

Reload your web browser to ensure that all files have been referenced correctly and no errors occur.

ウェブブラウザをリロードして、全てのファイルが正しく参照され、エラーが何も発生していないことを確認してください。

If you are using a package manager, such as [bower](http://bower.io), make sure to checkout the [Getting Ember](/guides/getting-ember) guide for info on other ways to get Ember.js.

もし、あなたが[bower](http://bower.io)のようなパッケージマネージャーを使っているなら、Ember.jsを入手する他の方法について、[Getting Ember](/guides/getting-ember)ガイドをチェックしてください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/ijefig/2/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>
 
### Additional Resources
### 追加リソース
  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/0880d6e21b83d916a02fd17163f58686a37b5b2c)
  
(The original document’s commit SHA1: 20cb79fa337481e66f9d5888034d4bd6b695e847)
