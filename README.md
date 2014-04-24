# Berkshelf

一部意訳。（実施中）

>Manage a Cookbook or an Application's Cookbook dependencies

Cookbookの管理、またCookbookの依存関係を解決するツールです。

>Berkshelf is maintained by the Berkshelf Core Team

BerkshelfはBerkshelf Core Team によってメンテナンスされています。

## GETTING STARTED

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

## GETTING HELP

>If at anytime you are stuck or if you’re just curious about what Berkshelf can do, just type the help command

行き詰まった場合やBerfshlefは何をする事が出来るか確認したい場合にはhelpコマンドを利用します

	$ berks help

>You can get more detailed information about a command, or a sub command, but asking it for help

コマンドやサブコマンドの詳細について知りたい場合にもhelpコマンドが利用出来ます

	$ berks install help

	Usage:
	  berks install

	Options:
	  -e, [--except=one two three]  # Exclude cookbooks that are in these groups.
	  -o, [--only=one two three]    # Only cookbooks that are in these groups.
	  -b, [--berksfile=PATH]        # Path to a Berksfile to operate off of.
	                                # Default: Berksfile
	  -c, [--config=PATH]           # Path to Berkshelf configuration to use.
	  -F, [--format=FORMAT]         # Output format to use.
	                                # Default: human
	  -q, [--quiet], [--no-quiet]   # Silence all informational output.
	  -d, [--debug], [--no-debug]   # Output debug information

	Install the cookbooks specified in the Berksfile

## THE BERKSHELF

>After running berks install you may ask yourself, “Where did my cookbooks go?”. They were added to The Berkshelf.

berks install コマンド実行後、あなたは"cookbooksはどこにあるの?"と自問自動するでしょう。それらは"The Berkshelf"という棚に追加されたのです。

>The Berkshelf is a location on your local disk which contains the cookbooks you have installed and their dependencies. By default, The Berkshelf is located at ~/.berkshelf but this can be altered by setting the environment variable BERKSHELF_PATH.

The Berkshlefはローカルにcookbookと依存しているcookbookを保存します。
デフォルトでは`~/.berkshelf`に配置されますが、`BERKSHELF_PATH`という環境変数を指定する事で変更する事が出来ます。

>Berkshelf stores every version of a cookbook that you have ever installed. This is the same pattern found with RubyGems where once you have resolved and installed a gem, you will have that gem and it’s dependencies until you delete it.

Berskshelfはインストールした各cookbookをバージョンごとに保存しています。

>This central location is not the typical pattern of cookbook storage that you may be used to with Chef. The traditional pattern is to place all of your cookbooks in a directory called cookbooks or site-cookbooks within your Chef Repository. We do have all of our cookbooks in one central place, it’s just not the Chef Repository and they’re stored within directories named using the convention {name}-{version}.


