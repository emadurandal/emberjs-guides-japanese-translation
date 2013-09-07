# INDICATING WHEN ALL TODOS ARE COMPLETE
# すべてのTodoが完了したことを示す

Next we'll update our template to indicate when all todos have been completed. In `index.html` replace the static checkbox `<input>` with an `{{input}}`:

次に、すべてのTodoが完了したときにそれを示すよう、テンプレートを更新します。`index.html`にて、静的なチェックボックス`<input>`要素を`{{input}}`ヘルパで置き換えます。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<section id="main">
  {{outlet}}
  {{input type="checkbox" id="toggle-all" checked=allAreDone}}
</section>
<!--- ... additional lines truncated for brevity ... -->
```

This checkbox will be checked when the controller property `allAreDone` is `true` and unchecked when the property `allAreDone` is `false`.

このチェックボックスは、コントローラーの`allAreDone`プロパティが`true`の時はチェックされ、`false`の時はチェックがはずされます。

In `js/controllers/todos_controller.js` implement the matching `allAreDone` property:

`js/controllers/todos_controller.js`にて、対応する`allAreDone`プロパティを実装します。

```javascript
// ... additional lines truncated for brevity ...
allAreDone: function (key, value) {
  return !!this.get('length') && this.everyProperty('isCompleted', true);
}.property('@each.isCompleted')
// ... additional lines truncated for brevity ...
```

This property will be `true` if the controller has any todos and every todo's `isCompleted` property is true. If the `isCompleted` property of any todo changes, this property will be recomputed. If the return value has changed, sections of the template that need to update will be automatically updated for us.

このプロパティは、コントローラーが一つ以上のTodoを持っており、かつ、すべてのTodoの`isCompleted`プロパティがtrueである場合に、`true`になります。各Todoの`isCompleted`プロパティが変更されると、このプロパティが再計算されます。このプロパティの戻り値が変化すると、更新すべきテンプレートのセクションが自動的に更新されます。

Reload your web browser to ensure that there are no errors and the behavior described above occurs. 

ウェブブラウザーをリロードし、何もエラーが起きないことと、上述の動作が行われることを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/IcItARE/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/9bf8a430bc4afb06f31be55f63f1d9806e6ab01c)
  * [Ember.Checkbox API documentation](/api/classes/Ember.Checkbox.html)

(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)