# INTRODUCTION
# イントロダクション

Because Handlebars templates in Ember.js are so powerful, the majority
of your application's user interface will be described using them. If
you are coming from other JavaScript libraries, you may be surprised at
how few views you have to create.

Ember.jsにおけるHandlebarsテンプレートがとてもパワフルなので、あなたの大半のアプリケーションのユーザーインターフェースはHandlebarsで記述することができるでしょう。もしあなたが他のJavaScriptライブラリから移ってきたとしたら、作らなければならないViewの少なさに、あなたは驚くかもしれません。

Views in Ember.js are typically only created for the following reasons:

Ember.jsにおけるViewは、一般的に次の理由から作ることになります。

* When you need sophisticated handling of user events
あなたが洗練されたユーザーイベントのハンドリングを必要としているとき
* When you want to create a re-usable component
あなたが再利用可能なコンポーネントを作ろうと望むとき

Often, both of these requirements will be present at the same time.

しばしば、この両者の要求は同時に発生することがあるものです。

### Event Handling
### イベントハンドリング

The role of the view in an Ember.js application is to translate
primitive browser events into events that have meaning to your
application.

Ember.jsアプリケーションにおけるViewの役割は、プリミティブなブラウザのイベントをあなたのアプリケーションにとって意味のあるイベントに変換することです。

For example, imagine you have a list of todo items. Next to each todo is
a button to delete that item:

例えば、あなたがTodo項目のリストを持っていると仮定しましょう。各Todoの隣にはそのTodo項目を削除するためのボタンがあります。

![Todo List](http://emberjs.com/guides/views/images/todo-list.png)

The view is responsible for turning a _primitive event_ (a click) into a
_semantic event_: delete this todo! These semantic events are first sent 
up to the controller, or if no method is defined there, your application's 
router, which is responsible for reacting to the event based on the 
current state of the application.

Viewは、_プリミティブなイベント（クリック）_をセマンティックなイベント（このTodoを削除する）に変換する役割を担います。これらのセマンティックなイベントは最初にControllerに送られます。そしてControllerで何もメソッドが定義されていなければ、あなたのアプリケーションのRouterに送られます。Routerはアプリケーションの現在の状態に基づいて、イベントに反応する責任を負います。

![Todo List](http://emberjs.com/guides/views/images/primitive-to-semantic-event.png)

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)