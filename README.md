# Berkshelf

一部意訳。

>Manage a Cookbook or an Application's Cookbook dependencies

Cookbookの管理、またCookbookの依存関係を解決する

>Berkshelf is maintained by the Berkshelf Core Team

BerkshelfはBerkshelf Core Team によってメンテナンスされています。

## Getting Started

>Berkshelf is now included as part of the Chef-DK. This is fastest, easiest, and the recommended installation method for getting up and running with Berkshelf.

Berkshelfは現在Chef-DK(Chef Deploymant Kit)の一部として提供されています。
上記は高速かつ、簡単に動作させる事ができ、Berkshelfのインストールの方法として推奨しています。

>Generate a Berksfile in a pre-exisitng cookbook

既にcookbookが存在する時のBerksfile作成する場合

	$ cd my-cookbook
	$ berks init .

>Or create a new cookbook

新しくcookbookを作成し、Berksfileを作成する場合

	$ berks cookbook myapp

>And specify your dependencies in a Berksfile in your cookbook’s root

そしてcookbookフォルダ直下のBerksfileに必要なcookbookを記述してください

	source "https://api.berkshelf.com"

	metadata

	cookbook "mysql"
	cookbook "nginx", "~> 2.6"

>Install the cookbooks you specified in the Berksfile and their dependencies

あなたがBerksfileに記述したcookbookとそれに依存したcookbookがインストールされます。

	$ berks install

