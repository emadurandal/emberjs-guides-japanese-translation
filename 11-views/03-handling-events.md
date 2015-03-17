# HANDLING EVENTS
# イベントのハンドリング

Instead of having to register event listeners on elements you'd like to
respond to, simply implement the name of the event you want to respond to
as a method on your view.

あなたが反応したい要素にイベントリスナーを登録しなければならないかわりに、単純にビューのメソッドとして、あなたが反応したいイベントの名前を実装します。

For example, imagine we have a template like this:

例えば、次のようなテンプレートを持っていると考えましょう。

```handlebars
{{#view "clickable"}}
This is a clickable area!
{{/view}}
```

Let's implement `App.ClickableView` such that when it is
clicked, an alert is displayed:

クリックされたときにアラートが表示されるように、`App.ClikableView`を実装しましょう。

```javascript
App.ClickableView = Ember.View.extend({
  click: function(evt) {
    alert("ClickableView was clicked!");
  }
});
```

Events bubble up from the target view to each parent view in succession,
until the root view. These values are read-only. If you want to
manually manage views in JavaScript (instead of creating them using
the `{{view}}` helper in Handlebars), see the `Ember.ContainerView`
documentation below.

イベントはターゲットViewから、Root Viewに達するまで、それぞれの親のViewに連続して伝播します。これらの値は読み込みのみ可です。もしあなたが、（Handlebarsの`{{view}}`ヘルパーを使ってViewを生成するのではなく、）JavaScriptでビューを手動で管理したいのなら、下の`Ember.ContainerView`ドキュメントを参照してください。

###Sending Events
###イベントの送信

To have the click event from `App.ClickableView` affect the state of your application, simply send an event to the view's controller:

`App.ClikableView`からのクリックイベントであなたのアプリケーションの状態に影響を及ぼさせるためには、単純にViewのControllerにイベントを送信します。

````javascript
App.ClickableView = Ember.View.extend({
  click: function(evt) {
    this.get('controller').send('turnItUp', 11);
  }
});
````

If the controller has an action handler called `turnItUp`, it will be called:

もしControllerが`turnItUp`というアクションハンドラーを持っているなら、それが呼び出されます。

````javascript
App.PlaybackController = Ember.ObjectController.extend({
  actions: {
    turnItUp: function(level){
      //Do your thing
    }
  }
});
````

If it doesn't, the message will be passed to the current route:

もし、Controllerがそのアクションハンドラーを持っていなかったら、メッセージは現在のRouteに渡されます。

````javascript
App.PlaybackRoute = Ember.Route.extend({
  actions: {
    turnItUp: function(level){
      //This won't be called if it's defined on App.PlaybackController
    }
  }
});
````

To see a full listing of the `Ember.View` built-in events, see the
documentation section on [Event Names](/api/classes/Ember.View.html#toc_event-names).

`Ember.View`の内蔵イベントの完全なリストを確認するため、[Event Names](http://emberjs.com/api/classes/Ember.View.html#toc_event-names)のドキュメンテーションのセクションを参照してください。

(The original document’s commit SHA1: 184b5fdf09f7ec9e106928648f2e160066449483)