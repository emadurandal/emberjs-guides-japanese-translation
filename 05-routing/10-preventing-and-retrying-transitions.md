# PREVENTING AND RETRYING TRANSITIONS
# 遷移を止めて、そして再開する

During a route transition, the Ember Router passes a transition
object to the various hooks on the routes involved in the transition.
Any hook that has access to this transition object has the ability
to immediately abort the transition by calling `transition.abort()`, 
and if the transition object is stored, it can be re-attempted at a 
later time by calling `transition.retry()`.

Routeの遷移の間、EmberのRouterは遷移先のRouteの様々なフックに遷移オブジェクト（transition object）を渡します。この遷移オブジェクトにアクセスするフックは`transition.abort()`を呼ぶことで直ちに遷移を中止する能力を持ち、もし遷移オブジェクトが保存されたら`transition.retry()`を呼ぶ事であとで遷移を再び試みることができます。

### Preventing Transitions via `willTransition`
### `willTransition`を通じて遷移を止める

When a transition is attempted, whether via `{{link-to}}`, `transitionTo`,
or a URL change, a `willTransition` action is fired on the currently
active routes. This gives each active route, starting with the leaf-most
route, the opportunity to decide whether or not the transition should occur.

（`{{link-to}}`からであろうと`transitionTo`からであろうと）遷移が試みられるとき、あるいはURLが変化したとき、現在アクティブなRouteの`willTransition`アクションが起動されます。このことは、各アクティブRoute（一番末端のRouteから始まります）に遷移が起こるべきか否かを決定する機会を与えます。

Imagine your app is in a route that's displaying a complex form for the user
to fill out and the user accidentally navigates backwards. Unless the
transition is prevented, the user might lose all of the progress they
made on the form, which can make for a pretty frustrating user experience.

あなたのアプリケーションが、ユーザーに入力させる複雑なフォームを表示しているRouteにおり、そしてユーザーが不意に「戻る」操作をしてしまったと想像してください。遷移が防がれないと、ユーザーがフォームで入力した内容がすべて失われてしまいます。これはかなりフラストレーションのたまるユーザー体験になりえます。

Here's one way this situation could be handled:

この状況を扱う、一つの例をお見せします。

```js
App.FormRoute = Ember.Route.extend({
  actions: {
    willTransition: function(transition) {
      if (this.controllerFor('form').get('userHasEnteredData') &&
          !confirm("Are you sure you want to abandon progress?")) {
        transition.abort();
      } else {
        // Bubble the `willTransition` action so that
        // parent routes can decide whether or not to abort.
        return true;
      }
    }
  }
});
```

### Aborting Transitions Within `model`, `beforeModel`, `afterModel`
### `model`, `beforeModel`, `afterModel`フックの中で遷移を中止する

The `model`, `beforeModel`, and `afterModel` hooks described in
[Asynchronous Routing](http://emberjs.com/guides/routing/asynchronous-routing)
each get called with a transition object. This makes it possible for
destination routes to abort attempted transitions.

[Asynchronous Routing（非同期ルーティング）](http://emberjs.com/guides/routing/asynchronous-routing)で説明した`model`、`beforeModel`、`afterModel`フックはそれぞれ、遷移オブジェクトを伴って呼び出されます。このことは、目標のRouteが、試みられた遷移を中止することを可能にします。

```js
App.DiscoRoute = Ember.Route.extend({
  beforeModel: function(transition) {
    if (new Date() < new Date("January 1, 1980")) {
      alert("Sorry, you need a time machine to enter this route.");
      transition.abort();
    }
  }
});
```

### Storing and Retrying a Transition
### 遷移を保存し、再開する

Aborted transitions can be retried at a later time. A common use case
for this is having an authenticated route redirect the user to a login
page, and then redirecting them back to the authenticated route once
they've logged in. 

中止された遷移はあとで再開できます。これの最も一般的なケースは、認証制のRouteに、ユーザーをログインページにリダイレクトさせるというものです。そして、一度ユーザーがログインしたら、ユーザーをまたリダイレクトしてその認証制のRouteに戻します。

```js
App.SomeAuthenticatedRoute = Ember.Route.extend({
  beforeModel: function(transition) {
    if (!this.controllerFor('auth').get('userIsLoggedIn')) {
      var loginController = this.controllerFor('login');
      loginController.set('previousTransition', transition);
      this.transitionTo('login');
    }
  }
});

App.LoginController = Ember.Controller.extend({
  actions: {
    login: function() {
      // Log the user in, then reattempt previous transition if it exists.
      var previousTransition = this.get('previousTransition');
      if (previousTransition) {
        this.set('previousTransition', null);
        previousTransition.retry();
      } else {
        // Default back to homepage
        this.transitionToRoute('index');
      }
    }
  }
});
```

(The original document’s commit SHA1: b49ef810880af7a834ce6b83737b06d47115fb93)