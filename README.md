# Berkshelf

一部意訳。（実施中）

>Manage a Cookbook or an Application's Cookbook dependencies

Cookbookの管理、またCookbookの依存関係を解決するツールです。

>Berkshelf is maintained by the Berkshelf Core Team

BerkshelfはBerkshelf Core Team によってメンテナンスされています。

## GETTING STARTED

>Berkshelf is now included as part of the Chef-DK. This is fastest, easiest, and the recommended installation method for getting up and running with Berkshelf.

Berkshelfは現在Chef-DK(Chef Deploymant Kit)の一部として提供されています。
上記は簡単かつ、あっという間に動作させる事ができ、Berkshelfのインストールの方法として推奨しています。

>Generate a Berksfile in a pre-exisitng cookbook

既に存在するcookbook内にBerkshileを作成する場合

	$ cd my-cookbook
	$ berks init .

>Or create a new cookbook

新しくcookbookを作成する場合

	$ berks cookbook myapp

>And specify your dependencies in a Berksfile in your cookbook’s root

そしてcookbookフォルダ直下のBerksfileに必要なcookbookを記述してください。

	source "https://api.berkshelf.com"

	metadata

	cookbook "mysql"
	cookbook "nginx", "~> 2.6"

>Install the cookbooks you specified in the Berksfile and their dependencies

あなたがBerksfileに記述したcookbookとそれに依存したcookbookがインストールされます。

	$ berks install

## GETTING HELP

>If at anytime you are stuck or if you’re just curious about what Berkshelf can do, just type the help command

行き詰まった場合やBerfshlefは何をする事が出来るか確認したい場合にはhelpコマンドを利用します。

	$ berks help

>You can get more detailed information about a command, or a sub command, but asking it for help

helpコマンドを利用することでコマンドやサブコマンドの詳細を知ることが出来ます。

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

packageコマンドによって必要なcookbookを一つにまとめたアーカイブを作成することが出来ます。

	$ cd ~/code/berkshelf-api/cookbook
	$ berks package
	Cookbook(s) packaged to /Users/reset/code/berkshelf-api/cookbook/cookbooks-1397512169.tar.gz

>This archive an be given directly to Chef-Solo or extracted and uploaded to a Chef Server.

このアーカイブは直接Chef-Soloで利用でき、またChefSeverにアップロードしたり、アップロードしたものを取得する事も出来る。

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

+ Chef_soloかchef_clinetプロビジョナーを使うか。(デフォルト:chef_solo)

>ssl.verify - should we verify all SSL http connections? (default: true)

+ SSL認証を行うべきか?(デフォルト:true)

>cookbook.copyright - the copyright information should be included when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるコピーライト(デフォルト：存在すればKnife fileの値)

>cookbook.email - the email address to include when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるメールアドレス(デフォルト：存在すればKnife fileの値)

>cookbook.license - the license to use when you generate new cookbooks. (default: whatever is in your Knife file if you have one)

+ 作成した新しいcookbookに含めるライセンス(デフォルト：存在すればKnife fileの値)

>github - an array of hashes containing Github credentials to authenticate against downloading cached Github cookbooks.

+ Githubのcookbookをダウンロードするにあたって認証するためにGithubの証明書を含むハッシュの配列

>The configuration values are notated in ‘dotted path’ format. These translate to a nested JSON structure.

設定値は'dotted path'記法で書かれています。
それらはネストされたJSON形式に変換さます。

## VAGRANT WITH BERKSHELF

>Berkshelf was designed for iterating on cookbooks and applications quickly. Vagrant provides us with a way to spin up a virtual environment and configure it using a built-in Chef provisioner. If you have never used Vagrant before - stop now - read the Vagrant documentation and give it a try. Your cookbook development life is about to become 100% better.

Berkshelfは繰り返し実行されるcookboookやアプリに素早く適応出来るように設計されています。
[Vagrant](http://www.vagrantup.com/ "Vagrant")はビルトインされたChef provisionerを使って起動する仮想環境の設定を行う方法を提供します。

まだVagrantを使っていないのであれば本稿を読むのはやめて、Vagrantのドキュメントを読んで試してみてください。
きっとあなたのcookbook開発ライフは良くなる事でしょう！

>If you have used Vagrant before, READ ON!

既に使っている人も継続して使いましょう！

## INSTALL VAGRANT

>Visit the Vagrant downloads page and download the latest installer for your operating system.

[Vagrantダウンロードページ](http://www.vagrantup.com/ "Vagrantダウンロードページ")に行って、あなたのOSの最新のインストーラーをダウンロードしましょう。

## INSTALL THE VAGRANT BERKSHELF PLUGIN

	$ vagrant plugin install vagrant-berkshelf --plugin-version 2.0.1
	Installing the 'vagrant-berkshelf' plugin. This can take a few minutes...
	Installed the plugin 'vagrant-berkshelf (2.0.1)!'

## USING THE VAGRANT BERKSHELF PLUGIN

>Once the Vagrant Berkshelf plugin is installed it can be enabled in your Vagrantfile

一度Vagrant Bershelf プラグインをインストールすればVagrantfileでプラグインを有効化する事が出来ます。

	Vagrant.configure("2") do |config|
	  ...
	  config.berkshelf.enabled = true
	  ...
	end

>If your Vagrantfile was generated by Berkshelf it’s probably already enabled

もし、VagrantfileをBerkshelfコマンドを利用して作成してる場合、既にプラグインの有効化はきっとされているでしょう。

>The plugin will look in your current working directory for your Berksfile by default. Just ensure that your Berksfile exists and when you run vagrant up, vagrant provision, or vagrant destroy the Berkshelf integration will automatically kick in!

プラグインはデフォルトではあなたのカレントディレクトリから`Berksfile`を探します。
Berksfileが存在すれば、`vagrant up`、`vagrant provision`、`vagrant destroy`を実行した際に自動でBerfshlefが適応されます！

	$ vagrant provision
	[Berkshelf] Updating Vagrant's berkshelf: '/Users/reset/.berkshelf/vagrant/berkshelf-20130320-28478-sy1k0n'
	[Berkshelf] Installing nginx (2.6.0)
	...

>You can use both the Vagrant provided Chef Solo and Chef Client provisioners with the Vagrant Berkshelf plugin.

Berksheflプラグインを実行するときにVagrantのChef soloとChef Clinet provisionerを利用する事が出来ます。

#### Chef Solo provisioner

>The Chef Solo provisioner’s cookbook_path attribute is hijacked when using the Vagrant Berkshelf plugin. Cookbooks resolved from your Berksfile will automatically be made available to your Vagrant virtual machine. There is no need to explicitly set a value for cookbook_path attribute.

Vagratn Berkshlefプラグインを利用している場合、Chef Soloプロビジョナーの`cookbook_path`属性は乗っ取られます。
Berksfileから見つけられたcookbookは自動でVMに適応されます。
上記では`cookbook_path`を設定する必要はありません。

#### Chef Client provisioner

>Cookbooks will automatically be uploaded to the Chef Server you have configured in the Vagrantfile’s Chef Client provisioner block. Your Berkshelf configuration’s chef.node_name and chef.client_key credentials will be used to authenticate the upload.

VagrantfileでChef Clientプロビジョナーの設定をしている場合、Cookbookは自動的にChef Serverにアップロードされます。
Bershelf設定の`chef.node_name`と`chef.client_key`で指定した証明情報はアップロード時に利用されます。

## THE BERKSFILE

>Dependencies are managed via the file Berksfile. The Berksfile is like Bundler’s Gemfile. Entries in the Berskfile are known as sources. It contains a list of sources identifying what Cookbooks to retrieve and where to get them.

依存関係は`Berksfile`によって管理されます。
BerksfileはBundlerのGemfileのようなものです。
Berkfileの項目は情報源として知られ、何のCookbookを取得するか、またどこから取得するかの情報を含んでいます。

	source "https://api.berkshelf.com"

	metadata

	cookbook 'memcached'
	cookbook 'nginx'
	cookbook 'pvpnet', path: '/Users/reset/code/riot-cookbooks/pvpnet-cookbook'
	cookbook 'mysql', git: 'git://github.com/opscode-cookbooks/mysql.git'

>All dependencies and their dependencies (and their dependencies, etc) will be downloaded, recursively. Two keywords can be used for defining dependencies.

全ての依存するもの、またそれに依存するもの（それにまた依存しているもの）は再起的にダウンロードされます。依存関係を明確にするために、2つのキーワードを使う事ができます。

## METADATA KEYWORD

>The metadata keyword is like saying gemspec in Bundler’s Gemfile. It says, “There is a metadata.rb file within the same relative path of my Berksfile”. This allows you to resolve a Cookbook’s dependencies that you are currently working on just like you would resolve the dependencies of a Gem that you are currently working on with Bundler.

metadataキーワードは言うなれば`gemspec`がBundlerの[Gemfile](http://bundler.io/man/gemfile.5.html "Gemfile")に存在するようなものです。
"metadata.rbがBerksfileと同じ相対パス上に存在する"ことを意味してます。
これはあなたにCookbookの依存関係の解決をGemの依存関係をBundlerを使って解決する方法と同じようにする事が出来るようにします。

>Given a Berksfile at ~/code/nginx-cookbook containing:

`~/code/nginx-cookbook`に配置されたBerksfileは以下を含みます。

	metadata

>A metadata.rb file is assumed to be located at ~/code/nginx-cookbook/metadata.rb describing your nginx cookbook.

上記ではnginx cookbookについて記述された`metadata.rb`は`~/code/nginx-cookbook/metadata.rb`に配置されているでしょう。


