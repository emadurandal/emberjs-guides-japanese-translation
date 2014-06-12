# Bindings, Observers, Computed Properties: What Do I Use When?
# バインディング、Observers、Computed Properties：どれをいつ使えばいい？

Sometimes new users are confused about when to use computed properties,
bindings and observers. Here are some guidelines to help:

ときどき、新しいユーザーはComputed Properties、バインディング、そしてObserversをいつ使えばいいのか混乱するようです。助けになるいくつかのガイドラインがあります：

1. Use *computed properties* to build a new property by synthesizing other
properties. Computed properties should not contain application behavior, and
should generally not cause any side-effects when called. Except in rare cases,
multiple calls to the same computed property should always return the same
value (unless the properties it depends on have changed, of course.)
*Computed properties*は、他のプロパティを総合することで新しいプロパティを構築するために使ってください。Computed Propertiesにはアプリケーションの動作を含めるべきではありません。また、一般的に、呼び出したときにどのような副作用も起こすべきではありません。まれなケースを除いて、同じComputed Propetyに対する複数の呼び出しは常に同じ値を返すべきです（もちろん、それが依存するプロパティが変わらない限り）。

2. *Observers* should contain behavior that reacts to changes in another
property. Observers are especially useful when you need to perform some
behavior after a binding has finished synchronizing.
*Observers*は他のプロパティの変化に反応するような動作を含めるようにすべきです。Observersは、バインディングが同期を終えた後に何らかの動作を実行する必要があるときに、特に便利です。

3. *Bindings* are most often used to ensure objects in two different layers
are always in sync. For example, you bind your views to your controller using
Handlebars.
*バインディング（Bindings）*は、二つの異なるレイヤー間のオブジェクトが常に同期されることを確実にするために最も良く使われます。例えば、Handlebarsを使い、ビューをコントローラーにバインドする、などです。

(The original document’s commit SHA1: 63f655a0f8b9ec99192c26d8d0478af5f84d7c72)