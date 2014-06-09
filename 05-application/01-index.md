# INTRODUCTION
# イントロダクション
## Creating an Application
## アプリケーションの作成

The first step to creating an Ember.js application is to make an
instance of `Ember.Application` and assign it to a global variable.

Ember.jsアプリケーションの作成の最初のステップは、`Ember.Application`のインスタンスを作り、それをグローバル変数に割り当てることです。

```javascript
window.App = Ember.Application.create();
```

Most people call their application `App`, but you can call it whatever
makes the most sense to you. Just make sure it starts with a capital
letter.

ほとんどの人たちはアプリケーションの名前をAppにします。しかし、あなたにとって最も意味のある名前ならば、なんとでも名付けることができます。ただ、名前が大文字で始まることだけは確実にしてください。

What does creating an `Ember.Application` instance get you?

Ember.Applicationインスタンスを生成することは、あなたに何をもたらすのでしょうか？

1. It is your application's namespace. All of the classes in your
   application will be defined as properties on this object (e.g.,
   `App.PostsView` and `App.PostsController`). This helps to prevent
   polluting the global scope.
   
   それ（Ember.Appliationインスタンスに付けた名前）はあなたのアプリケーションの名前空間になります。あなたのアプリケーションの全てのクラスはこのオブジェクトのプロパティとして定義されます（例：`App.PostsView` と `App.PostsController`）。このことはグローバルスコープを汚染することを防ぎます。
2. It adds event listeners to the document and is responsible for
   delegating events to your views. (See [The View
   Layer](/guides/understanding-ember/the-view-layer)
  for a detailed description.)
  
  それ（Ember.Applicationを生成すること）はdocumentにイベントリスナーを追加し、イベントをあなたのViewに委譲する役割を果たします。（詳細はThe View Layerを参照してください。）
3. It automatically renders the [application
   template](/guides/templates/the-application-template).
   
   それ（Ember.Application）は自動的にapplicationテンプレートをレンダリングします。
4. It automatically creates a router and begins routing, choosing which
   template and model to display based on the current URL.
   
   それ（Ember.Application）は自動的にRouterを生成し、ルーティングを開始します。そして、現在のURLに基づいて、どのテンプレートとModelを表示するか選択します。

(The original document’s commit SHA1: 5ef1bb0513231201a440d4423c3f2792a320ae06)