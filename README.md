# はじめに

+ 本ページは[Berkshelf version3のドキュメント](http://berkshelf.com/ "Berkshelf version3のドキュメント")を**非公式に勝手**に和訳したものです。
+ 2014年5月1日時点のものを参照したため、今後変更となる箇所がある可能性もあります。
+ 意訳した箇所もあります。
+ 動作確認を行えなかった箇所も多い為、その点はご了承ください。
+ 英語が得意！という訳でもないので、すいません、その点もご了承ください。
+ 記述誤りなどは [@toshihirock](https://twitter.com/toshihirock "@toshihirock")までご連絡頂くか、pull requestして頂くなどして頂ければ幸いです。


# Berkshelf

クックブックの管理、またクックブックの依存関係を解決するツールです。

BerkshelfはBerkshelf Core Team によってメンテナンスされています。

## GETTING STARTED

Berkshelfは現在[Chef-DK](http://getchef.com/downloads/chef-dk "Chef-DK")(Chef Deploymant Kit)の一部として提供されています。
Chef-DKは簡単かつ、すぐに動作させる事ができ、Berkshelfのインストールの方法として推奨しています。

既に存在するクックブック内にBerksfileを作成する場合、以下のようにします。

	$ cd my-cookbook
	$ berks init .

新しくクックブックを作成する場合、以下のようにします。

	$ berks cookbook myapp

そしてクックブックのフォルダ直下のBerksfileに依存しているクックブックを記述してください。

	source "https://api.berkshelf.com"

	metadata

	cookbook "mysql"
	cookbook "nginx", "~> 2.6"

あなたがBerksfileに記述したクックブックとそれに依存したクックブックがインストールされます。

	$ berks install

## GETTING HELP

行き詰まった場合やBerfshelfで何をする事が出来るか確認したい場合にはhelpコマンドを利用します。

	$ berks help

helpコマンドを利用することでコマンドやサブコマンドの詳細を知ることが出来ます。

	$ berks install help

	Usage:
	  berks install

	Options:
	  -e, [--except=one two three]  # 指定したグループはインストール対象外
	  -o, [--only=one two three]    # 指定したグループのみインストールする
	  -b, [--berksfile=PATH]        # Berksfileのパス
	                                # デフォルト:Berksfile
	  -c, [--config=PATH]           # Berkshelf設定ファイルのパス
	  -F, [--format=FORMAT]         # 利用する出力フォーマット
	                                # デフォルト：human
	  -q, [--quiet], [--no-quiet]   # 全ての情報を出力しない
	  -d, [--debug], [--no-debug]   # デバック情報を出力する

	Install the cookbooks specified in the Berksfile

## THE BERKSHELF

`berks install`コマンド実行後、あなたは"クックブックはどこにあるの?"と自問自動するでしょう。それらはThe Berkshelfという棚に追加されたのです。

Berkshelfはインストールしたクックブックとそれに依存しているクックブックをローカルに保存します。
デフォルトでは`~/.berkshelf`に配置されますが、`BERKSHELF_PATH`という環境変数を指定する事で変更する事が出来ます。

Berskshelfはこれまでにインストールした各クックブックをバージョンごとに保存しています。
RubyGemsと同じように一度でもインストールしたものは削除するまで依存するものを含め、保持します。


Chefを使う場合において、クックブックを共通の場所に置くのは一般的な手法ではありません。
クックブックはChefリポジトリの`cookbooks`もしくは`site-cookbooks`と呼ばれるディレクトリに配置するのが一般的な手法となります。
Berkshelfでは全てのクックブックをChefリポジトリではない共通の場所に配置し、クックブックは{名称}-{バージョン}というルールで配置します。

以下のクックブックをインストールしたとします。

	* nginx - 2.6.4
	* mysql - 5.1.9

上記クックブックは以下のように配置されます。

	~/.berkshelf/cookbooks/nginx-2.6.4
	~/.berkshelf/cookbooks/mysql-5.1.9

クックブックのmetadataファイルにname属性の記述は必須となっています。
もし、記述がないようであれば追記する必要があります。

### PACKAGING COOKBOOKS

packageコマンドによって必要なクックブック群を一つにまとめたアーカイブを作成することが出来ます。

	$ cd ~/code/berkshelf-api/cookbook
	$ berks package
	Cookbook(s) packaged to /Users/reset/code/berkshelf-api/cookbook/cookbooks-1397512169.tar.gz

このアーカイブは直接Chef-Soloで利用でき、またChefSeverにアップロードしたり、アップロードしたものを取得する事も出来ます。

### VENDORING COOKBOOKS

パッケージは作りたくないが、Berkshelfという棚にクックブックがない状態でクックブックをインストールしたい場合にはvendorコマンドを利用します。

	$ berks vendor

上記コマンドで取得した全てのクックブックを`pwd/berks-cookbooks`に配置します。

## CONFIGURING BERKSHELF

Berkshelfは設定をしていなければデフォルトの設定で動作します。
また、デフォルトでBerkshelfはKnife Configureで設定された値を利用します。(Kinifeファイルがあれば)

あなたは設定ファイルを`~/.berkshelf/config.json`に配置する事でその設定を上書きする事が出来ます。

### CONFIGURABLE OPTIONS

+ `chef.chef_server_url` - Chef SeverのURL(デフォルト:存在すればKnifeファイルの値)
+ `chef.node_name` -Chefクライアントの名称。(デフォルト:存在すればKnifeファイルの値)
+ `chef.client_key` - Chefクライアントのキーのパス。(デフォルト:存在すればKnifeファイルの値)
+ `chef.validation_client_name` - Chef Validationの名称。(デフォルト:存在すればKnifeファイルの値)
+ `chef.validation_key_path` - Chef Validationのキーのパス。(デフォルト:存在すればknifeファイルの値)
+ `vagrant.vm.box` - Vagrantの仮想マシンとして利用するVirtualBoxのbox名称。(デフォルト:Berkshelf-CentOS-6.3-x86_64-minimal)
+ `vagrant.vm.box_url` -VirtuaBoxのboxを取得するURL(デフォルト:https://dl.dropbox.com/u/31081437/Berkshelf-CentOS-6.3-x86_64-minimal.box)
+ `vagrant.vm.forward_port` - Vagrantでホストからゲストにフォワードするポート。
+ `vagrant.vm.provision` - `chef_solo`もしくは`chef_clinet`プロビジョナーを使うか。(デフォルト:chef_solo)
+ `ssl.verify - SSL認証を行うべきか?(デフォルト:true)
+ `cookbook.copyright` - 新しくクックブックを作成した時に含めるコピーライト(デフォルト:存在すればKnifeファイルの値)
+ `cookbook.email` - 新しくクックブックを作成した時に含めるメールアドレス(デフォルト:存在すればKnifeファイルの値)
+ `cookbook.license` - 作成した新しいクックブックに含めるライセンス(デフォルト:存在すればKnifeファイルの値)
+ `github` - Githubのクックブックをダウンロードする際の認証に必要なにGithubの証明書を含むハッシュの配列

設定値は'dotted path'記法で書かれています。
それらはネストされたJSON形式に変換されます。

## VAGRANT WITH BERKSHELF

Berkshelfは繰り返し実行するクックブックやアプリに対して素早く実施出来るように設計されています。
[Vagrant](http://www.vagrantup.com/ "Vagrant")ではビルトインされたChef プロビジョナーを使って起動する仮想環境の設定を行う方法を提供します。

まだVagrantを使っていないのであれば本稿を読むのはやめて、Vagrantのドキュメントを読んで試してみてください。
きっとあなたのクックブック開発ライフは100%良くなる事でしょう！

既に使っている人も継続して使いましょう！

### INSTALL VAGRANT

[Vagrantダウンロードページ](http://www.vagrantup.com/downloads.html "Vagrantダウンロードページ")に行って、あなたのOSの最新のインストーラーをダウンロードしましょう。

### INSTALL THE VAGRANT BERKSHELF PLUGIN

	$ vagrant plugin install vagrant-berkshelf --plugin-version 2.0.1
	Installing the 'vagrant-berkshelf' plugin. This can take a few minutes...
	Installed the plugin 'vagrant-berkshelf (2.0.1)!'

### USING THE VAGRANT BERKSHELF PLUGIN

一度Vagrant Berkshelf プラグインをインストールすればVagrantfileでプラグインを有効化する事が出来ます。

	Vagrant.configure("2") do |config|
	  ...
	  config.berkshelf.enabled = true
	  ...
	end

もし、VagrantfileをBerkshelfコマンドを利用して作成している場合、既にプラグインの有効化はされているでしょう。

プラグインはデフォルトではあなたのカレントディレクトリから`Berksfile`を探します。
Berksfileが存在すれば、`vagrant up`、`vagrant provision`、`vagrant destroy`を実行した際に自動でBerkshelfのクックブックが適用されます！

	$ vagrant provision
	[Berkshelf] Updating Vagrant's berkshelf: '/Users/reset/.berkshelf/vagrant/berkshelf-20130320-28478-sy1k0n'
	[Berkshelf] Installing nginx (2.6.0)
	...

Berkshelfプラグインを実行するときにVagrantのChef soloとChef Clinet プロビジョナーを利用する事が出来ます。

#### Chef Solo provisioner

Vagratn Berkshelfプラグインを利用している場合、Chef Soloプロビジョナーの`cookbook_path`属性は乗っ取られます。
Berksfileに記述されたクックブックは自動で仮想環境に適応されます。
`cookbook_path`を設定する必要はありません。

#### Chef Client provisioner

VagrantfileのChef Clientプロビジョナーのブロックで設定したクックブック群は自動的にChef Serverにアップロードされます。
Berkshelfの設定の`chef.node_name`と`chef.client_key`で指定した証明情報はアップロード時に利用されます。

## THE BERKSFILE

依存関係は`Berksfile`によって管理されます。
BerksfileはBundlerのGemfileのようなものです。
Berksfileの項目を利用して、何のクックブックを取得するか、またどこから取得するかを決定します。

	source "https://api.berkshelf.com"

	metadata

	cookbook 'memcached'
	cookbook 'nginx'
	cookbook 'pvpnet', path: '/Users/reset/code/riot-cookbooks/pvpnet-cookbook'
	cookbook 'mysql', git: 'git://github.com/opscode-cookbooks/mysql.git'

記述された全てのクックブックとそのクックブックに依存するもの（またそれに依存しているもの）は再起的にダウンロードされます。依存関係を明確にするために、2つのキーワードを使う事ができます。

### METADATA KEYWORD

metadataキーワードは言うなれば`gemspec`がBundlerの[Gemfile](http://bundler.io/man/gemfile.5.html "Gemfile")に存在するようなものです。
これは"metadata.rbがBerksfileと同じ相対パス上に存在する"ことを意味してます。
Bundlerを使ってGemの依存関係を解決するようにクックブックの依存関係も解決する事が出来ます。

`~/code/nginx-cookbook`に配置されたBerksfileは以下の記述があります。

	metadata

nginxのクックブックについて記述された`metadata.rb`は`~/code/nginx-cookbook/metadata.rb`に配置されているはずです。

### COOKBOOK KEYWORD

Cookbookキーワードはインストールするクックブックの指定や、クックブックを探索をする場所を上書きする事が出来ます。

Cookbookキーワードは以下のフォーマットで定義されています。

	cookbook {name}, {version_constraint}, {options}

最初の`name`パラメーターは唯一の必須パラメーターです。

	cookbook "nginx"

2つ目の`version constraint`パラメーターは任意のパラメーターです。
指定しなければ最新版がインストールされるはずです。

	cookbook "nginx", ">= 0.101.2"

バージョン指定方法として以下の表記が出来ます。

+ 指定バージョンと同じ (=)
+ 指定バージョンより大きい (>)
+ 指定バージョン以上 (>=)
+ 指定バージョンより小さい (<)
+ 指定バージョン以下 (<=)
+ "~>2.0" とすると、2.x 系列で最新のものになる (~>)

最後のoptionsパラメーターは任意パラメーターでハッシュとなります。

### SOURCE OPTIONS

クックブックの探索場所やグループの指定が出来ます。

#### Locations

デフォルトでは予め設定した場所からクックブックの探索を行います。

	source "https://berks-api.vialstudios.com"
	source "https://api.berkshelf.com"

例えば上記の場合、依存する全てのクックブックが`berks-api.vialstudios.com`に存在すれば本URLのみ利用してクックブックの取得を行います。
もしなければ、次に定義されたURLから探索を行います。どのURLからも探索する事が出来なかった場合にはエラーとなります。

Locationを指定する事で、予め設定されたクックブックの探索場所を上書きする事が出来ます。

#### Path Location

Path locationを使えば、クックブックをダウンロードするのではなく、Berkshelfや他の場所からのコピーもしくは移動によって配置するので、素早く何度も実行したい場合に便利です。
ファイルパスの指定によって見つけられたクックブックはBerkshelfにあるクックブックと同じように利用されます。

	cookbook "artifact", path: "/Users/reset/code/artifact-cookbook"

`path`で指定するディレクトリでは一つのクックブックのみを含むものとし、必ず`metadara.rb`を配置する必要があります。

#### Git Location

Git Location は指定されたGitリポジトリが有効なクックブックであればBerkshelfにcloneを行います。

	cookbook "mysql", git: "https://github.com/opscode-cookbooks/mysql.git"

上記例では、Githubのプロジェクト名opscode-cookbooks/mysql のHEADリビジョンがBerkshlefにcloneされます。

任意指定の`branch`キーを利用する事で指定したbranchやtagのクックブックが取得出来ます。

	cookbook "mysql", git: "https://github.com/opscode-cookbooks/mysql.git", branch: "foodcritic"

上記例では、Githubのプロジェクト名opscode-cookbooks/mysqlの`foodcritic`ブランチがBerkshelfにcloneされます。

任意指定の`tag`キーは`branch`キーのエイリアスとなっており、branchキーと同じように利用出来ます。

	cookbook “mysql”, git: “https://github.com/opscode-cookbooks/mysql.git”, tag: “3.0.2”

上記例では、Githubのプロジェクト名opscode-cookbooks/mysqlの`3.0.2`とタグ付けされたクックブックがBerkshelfにcloneされます。

任意指定の`ref`キーはSHA-1 commit IDを指定してクックブックの取得が出来ます。

	cookbook “mysql”, git: “https://github.com/opscode-cookbooks/mysql.git”, ref: “eef7e65806e7ff3bdbe148e27c447ef4a8bc3881”

上記例では、Githubのプロジェクト名opscode-cookbooks/mysqlの3.0.2のコミットID`eef7e65806e7ff3bdbe148e27c447ef4a8bc3881`となっているクックブックがBerkshelfにcloneされます。

任意指定の`rel`キーは一つのリポジトリ内のルートディレクトリやサブディレクトリに他のクックブック含まれ、それを取得する場合に利用出来ます。

	cookbook "rightscale", git: "https://github.com/rightscale/rightscale_cookbooks.git", rel: "cookbooks/rightscale"

上記では指定リポジトリの`cookbooks`サブディレクトリから`rightscaleと`いうクックブックを取得します。

#### GitHub Location

バージョン1.0.0から、Githubリポジトリの指定方法としてより簡単に記述する事も可能となっています。

	cookbook "artifact", github: "RiotGames/artifact-cookbook", tag: "0.9.8"

上記例では、`RiotGames`organizationの`artifact-cookbook`リポジトリの`0.9.8`としてタグ付けされているものを`artifact`というクックブック名としてBerkshlefにcloneします。

指定がない場合、`Git`プロトコルを利用して取得を行いますが、プライベートリポジトリにアクセスする場合には`https`もしくは`ssh`プロトコルを利用する事でアクセスが可能となります。

	cookbook "keeping_secrets", github: "RiotGames/keeping_secrets-cookbook", protocol: :ssh

プライベートリポジトリを参照する際に`https`もしくは`ssh`プロトコルを利用しないとnot foundエラーとなってしまいます。

### GROUPS

groupを追記する事でインストールやアップロードの際に特定のクックブックや特定のクックブック郡を対象外とする事が出来ます。

groupはブロックで指定出来ます。

	group :solo do
	  cookbook 'riot_base'
	end

groupは１行表記の場合でも記述が出来ます。

	cookbook 'riot_base', group: 'solo'

コマンド実行時に`--without`を用いてグループを指定する事でインストールやアップデートの対象外とする事が出来ます。

	$ berks install --without solo

## AUTHORS

+ Jamie Winsor (jamie@vialstudios.com)
+ Josiah Kiehl (jkiehl@riotgames.com)
+ Michael Ivey (michael.ivey@riotgames.com)
+ Justin Campbell (justin.campbell@riotgames.com)
+ Seth Vargo (sethvargo@gmail.com)

[全てのコントリビューターの方々](https://github.com/berkshelf/berkshelf/graphs/contributors "全てのコントリビューターの方々")
