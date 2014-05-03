# BUILT-IN VIEWS
# ビルトインView

Ember comes pre-packaged with a set of views for building a few basic controls like text inputs, check boxes, and select lists.

Emberには、テキスト入力やチェックボックス、選択リストのようないくつかの基本的なコントロールを構築するための、ビューのセットが同封されています。

They are:
それらは、以下のようなものです。

#### Ember.Checkbox

```handlebars
<label>
  {{view Ember.Checkbox checked=model.isDone}}
  {{model.title}}
</label>
```

#### Ember.TextField

```javascript
App.MyText = Ember.TextField.extend({
  formBlurred: null, // passed to the view helper as formBlurred=controllerPropertyName
  change: function(evt) {
    this.set('formBlurred', true);
  }
});
```

#### Ember.Select

```handlebars
{{view Ember.Select viewName="select"
                    content=people
                    optionLabelPath="content.fullName"
                    optionValuePath="content.id"
                    prompt="Pick a person:"
                    selection=selectedPerson}}
```

#### Ember.TextArea

```javascript
var textArea = Ember.TextArea.create({
  valueBinding: 'TestObject.value'
});
```


(The original document’s commit SHA1: b678a545c3e5a8cca522f9e80b6eb564493a7d64)