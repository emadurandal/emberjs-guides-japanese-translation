##Ember Builds
##Emberビルド

The Ember Release Management Team maintains a variety of ways to get  Ember and Ember Data builds.

Emberリリース管理チームは、EmberとEmber Dataのビルドを入手するための様々な方法をメンテナンスしています。

###Channels
###チャンネル

The latest [Release](/builds#/release), [Beta](/builds#/beta), and [Canary](/builds#/canary) builds of Ember and Ember data can be found [here](/builds). For each channel a development, minified, and production version is available. For more on the different channels read the [Post 1.0 Release Cycle](http://emberjs.com/blog/2013/09/06/new-ember-release-process.html) blog post.

最新のEmberとEmber Dataの[Release](http://emberjs.com/builds/#/release)、[Beta](http://emberjs.com/builds/#/beta)、そして[Canary](http://emberjs.com/builds/#/canary)ビルドは[ここ](http://emberjs.com/builds)で見つけることができます。それぞれのチャンネルのdevelopment、minified、そしてproductionバージョンが入手可能です。異なるチャンネルのより詳細については、ブログ記事[「Post 1.0 Release Cycle」](http://emberjs.com/blog/2013/09/06/new-ember-release-process.html)をお読みください。

###Tagged Releases
###タグリリース

Past release and beta builds  of Ember and Ember Data are available at [Tagged Releases](/builds#/tagged). These builds can be useful to track down regressions in your application, but it is recommended to use the latest stable release in production.

EmberとEmber Dataの過去のReleaseとBetaビルドは[Tagged Releases](http://emberjs.com/builds/#/tagged)から入手できます。これらのビルドはあなたのアプリケーションにおけるリグレッション（前のバージョンになかった不具合）を見つけ出すのに役立つでしょう。しかし、あなたのアプリケーションの製品リリース向けには、最新の安定版リリースを使うことをお薦めします。


##Bower

Bower is a package manager for the web. Bower makes it easy to manage dependencies in your application including Ember and Ember Data. To learn more about Bower visit [http://bower.io/](http://bower.io/).

BowerはWeb開発におけるパッケージマネージャーです。BowerはEmberとEmber Dataを含む、あなたのアプリケーションにおけるライブラリの依存性を管理することを容易にしてくれます。Bowerについてより詳しく学ぶためには、[http://bower.io/](http://bower.io/)を訪れてください。

Adding Ember to your application with Bower is easy simply run `bower install ember --save`. For Ember Data run `bower install ember-data --save`. You can also add `ember` or `ember-data` to your `bower.json` file as follows.

Bowerを使ってあなたのアプリケーションにEmberを追加するには、単純に`bower install ember --save`を実行します。Ember Dataを追加するには、`bower install ember-data --save`を実行します。また、次のようにして、`ember`と`ember-data`を`bower.json`ファイルに加えることもできます。

```json
{
	"name": "your-app",
	"dependencies": {
		"ember": "~1.5",
		"ember-data": "~1.0.0-beta.4"
	}
}

```

##RubyGems

If your application uses a Ruby based build system you can use the [ember-source](http://rubygems.org/gems/ember-source) and [ember-data-source](http://rubygems.org/gems/ember-data-source) RubyGems to access ember and ember data sources from Ruby.

もしあなたのアプリケーションがRubyベースのシステムを使っているなら、あなたはRubyからEmberとEmber Dataのソースにアクセスするために、[ember-source](http://rubygems.org/gems/ember-source) RubyGemと[ember-data-source](http://rubygems.org/gems/ember-data-source) RubyGemを使うことが出来ます。

If your application is built in rails the [ember-rails](http://rubygems.org/gems/ember-rails) RubyGem makes it easy to integrate Ember into your Ruby on Rails application.

もしあなたのアプリケーションがRailsで構築されているなら、[ember-rails](http://rubygems.org/gems/ember-rails) RubyGemを使うことで、あなたのRuby on Railsアプリケーションに容易にEmberを統合することができるでしょう。


(The original document's commit SHA1: e6ff3101e1824cd4c179a58c8410b26ffbc14a76)