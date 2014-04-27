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

The Berkshlefはインストールしたcookbookとそれに依存しているcookbookをローカルに保存します。
デフォルトでは`~/.berkshelf`に配置されますが、`BERKSHELF_PATH`という環境変数を指定する事で変更する事が出来ます。

>Berkshelf stores every version of a cookbook that you have ever installed. This is the same pattern found with RubyGems where once you have resolved and installed a gem, you will have that gem and it’s dependencies until you delete it.

Berskshelfはこれまでにインストールした各cookbookをバージョンごとに保存しています。
RubyGemsと同じように一度でもインストールしたものは削除するまで依存するものを含め、保持します。

>This central location is not the typical pattern of cookbook storage that you may be used to with Chef. The traditional pattern is to place all of your cookbooks in a directory called cookbooks or site-cookbooks within your Chef Repository. We do have all of our cookbooks in one central place, it’s just not the Chef Repository and they’re stored within directories named using the convention {name}-{version}.

Chefを使う場合において、cookbookを共通の中心的な場所に置くのは一般的な手法ではありません。
cookbookはChefリポジトリの`bookbooks`もしくは`site-cookbooks`と呼ばれるディレクトリに配置するのが一般的な手法となります。
Berfshlefでは全てのCookbookは共通の中心的な場所に置かれます。そこはChefリポジトリではなく、cookbookは{名称}-{バージョン}というルールで保持されています。

>Given you have the cookbooks installed:

例えば以下のようになります。

	* nginx - 2.6.4
	* mysql - 5.1.9

>These cookbooks will be located at:

上記cookbookは以下のように配置されます。

	~/.berkshelf/cookbooks/nginx-2.6.4
	~/.berkshelf/cookbooks/mysql-5.1.9

>It is now REQUIRED for the name attribute to be set in your cookbook’s metadata. If you have a cookbook which does not specify this, it will need to be added.

Cookbookのmetadataにname属性の記載は必須となっています。
もし、cookbookに記載がないようであれば追記する必要があります。

## PACKAGING COOKBOOKS

>A single archive containing all of your required cookbooks can be created with the package command

packageコマンドであなたが必要なcookbookを一つのアーカイブにする事が出来ます。

	$ cd ~/code/berkshelf-api/cookbook
	$ berks package
	Cookbook(s) packaged to /Users/reset/code/berkshelf-api/cookbook/cookbooks-1397512169.tar.gz

>This archive an be given directly to Chef-Solo or extracted and uploaded to a Chef Server.

このアーカイブは直接Chef-Soloで利用でき、ChefServerへのアップロードも出来る。

## VENDORING COOKBOOKS

>If you don’t want to create a package but you want to install the cookbooks to a location on disk that is not the berkshelf, you can use the vendor command

パッケージは作りたくないが、berkshlefがない環境にインストールしたcookbookを利用する場合にはvendorコマンドを利用する事が出来る。

	$ berks vendor

>This will output all of the cookbooks to pwd/berks-cookbooks

上記コマンドで全てのcookbookを`pwd/berks-cookbooks`に配置します。

## CONFIGURING BERKSHELF

>Berkshelf will run with a default configuration unless you explicitly generate one. By default, Berkshelf uses the values found in your Knife configuration (if you have one).

Berkshlefは設定をしなければデフォルトで設定で動作します。
デフォルトではBerkshlefはKnifes Configureで設定された値を利用します。(Kinife configureを設定していれば)

>You can override this default behavior by create a configuration file and placing it at ~/.berkshelf/config.json

あなたは設定ファイルを`~/.berkshelf/config.json`に配置する事でその設定をオーバーライドする事が出来ます。


### CONFIGURABLE OPTIONS

>chef.chef_server_url - URL to a Chef Server API endpoint. (default: whatever is in your Knife file if you have one)

+ Chef SeverAPIのURL(デフォルト：存在すればKnife fileの値)

>chef.node_name - your Chef API client name. (default: whatever is in your Knife file if you have one)

+ ChefクライアントAPIの名称。(デフォルト：存在すればKnife fileの値)

>chef.client_key - filepath to your Chef API client key. (default: whatever is in your Knife file if you have one)

+ ChefAPIクライアントのキーのパス。(デフォルト：存在すればKnife fileの値)

>chef.validation_client_name - your Chef API’s validation client name. (default: whatever is in your Knife file if you have one)

+ Chef Validationの名称。(デフォルト：存在すればKnife fileの値)

>chef.validation_key_path - filepath to your Chef API’s validation key. (default: whatever is in your Knife file if you have one)

+ Chef Validationのキーのパス。(デフォルト：存在すればKnife fileの値)

>vagrant.vm.box - name of the VirtualBox box to use when provisioning Vagrant virtual machines. (default: Berkshelf-CentOS-6.3-x86_64-minimal)

+ VagrantのVMの名称。(デフォルト:Berkshelf-CentOS-6.3-x86_64-minimal)

>vagrant.vm.box_url - URL to the VirtualBox box (default: https://dl.dropbox.com/u/31081437/Berkshelf-CentOS-6.3-x86_64-minimal.box)

+ VagrantVMのURL(デフォルト: https://dl.dropbox.com/u/31081437/Berkshelf-CentOS-6.3-x86_64-minimal.box)

>vagrant.vm.forward_port - a Hash of ports to forward where the key is the port to forward to on the guest and value is the host port which forwards to the guest on your host.

+ Vagrantでホストからゲストにフォワードするポート。

>vagrant.vm.provision - use the chef_solo or chef_client provisioner? (default: chef_solo)

+ プロビジョナーとしてChef_soloかchef_clinetを使うか。(デフォルト:chef_solo)

>ssl.verify - should we verify all SSL http connections? (default: true)

+ SSL認証を行うべきか?(デフォルト:true)

>cookbook.copyright - the copyright information should be included when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるコピーライト(デフォルト：存在すればKnife fileの値)

>cookbook.email - the email address to include when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるメールアドレス(デフォルト：存在すればKnife fileの値)

>cookbook.license - the license to use when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるライセンス(デフォルト：存在すればKnife fileの値)

>github - an array of hashes containing Github credentials to authenticate against downloading cached Github cookbooks.

+ Githubの認証証明書???

>The configuration values are notated in ‘dotted path’ format. These translate to a nested JSON structure.

設定値は'dotted path'記法です。
それらはネストされたJSON形式に変換されます。
