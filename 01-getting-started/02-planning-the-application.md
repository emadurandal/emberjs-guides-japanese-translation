#PLANNING THE APPLICATION
#アプリケーションの設計

TodoMVC, despite its small size, contains most of the behaviors typical in modern single page applications. Before continuing, take a moment to understand how TodoMVC works from the user's perspective.

TodoMVCは小さいサイズながらも、モダンなシングルページアプリケーションとして典型的なほとんどの動作を含んでいます。進める前に、ユーザーの観点から、TodoMVCがどのように動作するのかを理解しましょう。

TodoMVC has the following main features:

TodoMVCは次のような主な特徴を持っています。

<img src="http://emberjs.com/guides/getting-started/images/todo-mvc.png" width="680">

  1. It displays a list of todos for a user to see. This list will grow and shrink as the user adds and removes todos.
  ユーザーが確認できるようにTodoのリストを表示します。このリストはユーザーがTodoを追加したり削除するのに伴い、大きくなったり小さくなったりします。

  1. It accepts text in an `<input>` for entry of new todos. Hitting the `<enter>` key creates the new item and displays it in the list below.
  新しいTodo項目のために、`<input>`要素でテキストを受け取ります。`<enter>`キーを押すことで、新しい項目が作られ、リストの下に表示されます。

  1. It provides a checkbox to toggle between complete and incomplete states for each todo. New todos start as incomplete.
  各Todoに対して、完了と未完了を切り替えるためのチェックボックスを提供します。新しいTodoは未完了の状態になります。

  1. It displays the number of incomplete todos and keeps this count updated as new todos are added and existing todos are completed.
  未完了のTodoの数を表示します。そして、新しいTodoが追加されたり既存のTodoが完了するのに応じて、このカウントは最新の状態に更新されます。

  1. It provides links for the user to navigate between lists showing all, incomplete, and completed todos.
  ユーザーが、全てのTodo、未完了のTodo、完了したTodoのリスト表示を切り替えられるようにするリンクを提供します。

  1. It provides a button to remove all completed todos and informs the user of the number of completed todos. This button will not be visible if there are no completed todos.
  全ての完了済みのTodoを削除するボタンを提供し、ユーザーに完了済みTodoの数を通知します。

  1. It provides a button to remove a single specific todo. This button displays as a user hovers over a todo and takes the form of a red X.
  単一の特定のTodoを削除するボタンを提供します。このボタンはユーザーがTodoの上をマウスでホバーすることで表示され、赤いXの形をしています。

  1. It provides a checkbox to toggle all existing todos between complete and incomplete states. Further, when all todos are completed this checkbox becomes checked without user interaction.
  全ての既存のTodoについて、完了と未完了の状態を切り替えるチェックボックスを提供します。さらに、全てのTodoが完了済みのときは、このチェックボックスはユーザーの操作なくチェック状態になります。

  1. It allows a user to double click to show a textfield for editing a single todo. Hitting the `<enter>` key or moving focus outside of this textfield will persist the changed text.
  単一のTodoを編集するために、ユーザーがTodoをダブルクリックするとテキストフィールドが表示されるようにします。`<enter>`キーを押すか、このテキストフィールドの外にフォーカスを移動したら、変更したテキストを保存します。

  1. It retains a user's todos between application loads by using the browser's `localstorage` mechanism.
  ブラウザのローカルストレージ機構を使うことで、アプリケーションの読み込みを繰り返しても、ユーザーのTodoのデータは引き続き保持されます。

You can interact with a completed version of the application by visiting the [TodoMVC site](http://todomvc.com/architecture-examples/emberjs/).

TodoMVCウェブサイトを訪れて、このアプリケーションの完成バージョンを操作することができます。

(The original document’s commit SHA1: 5451c52cde72009797b21292f6838f5b10a5a858)