========================================
Zopeのソースからのビルドとインストール
========================================

Zopeへようこそ！ このドキュメントはZopeをUnixとLinuxでビルドし
インストールする方法について記載しています。

Windows については `<WINDOWS.html>`_ を参照してください。

前提
-----

ソースからビルドするために必要な条件

- ZopeがサポートしているバージョンのPython、システムレベルのパッケージ
  からインストールしている場合は開発サポートも含みます。サポートバージョン
  は以下の通り:

  * 2.5.x, (x >= 4)

  * 2.6.x

- ZopeはPythonの ``zlib`` モジュールがインポート出来る必要があります。
  Pythonをソースからビルドするのであれば、先にシステムの ``zlib`` 
  ヘッダーをインストールしておいてください。

- Pythonの拡張モジュールをビルドできる C コンパイラー。
  (gcc を推奨).


zc.buildout を使ったZopeのビルド
---------------------------------

Zope は ``zc.buildout`` ライブラリを使用してビルドされています。
これを使う場合、あなたが使用するPythonのバージョンで
"ブートストラップ済み"の必要があります。例えば::

  $ cd /path/to/zope
  $ /path/to/your/python bootstrap/bootstrap.py

bootstrapスクリプトは ``bin`` ディレクトリに ``buildout`` スクリプトを
作成します。このスクリプトを実行してZopeのビルドを行ってください::

  $ bin/buildout

easy_install によるZopeのインストール
--------------------------------------

``easy_install`` を使用してZopeをインストールすることが出来ます。
システムでグローバルなeasy_installを使っても良いし、 ``virtualenv``
を用いて仮想Python環境のeasy_installを使用することも出来ます::

  $ virtualenv --no-site-packages my_zope
  $ cd my_zope
  $ source bin/activate
  $ bin/easy_install -i http://download.zope.org/Zope2/index/<Zope version> Zope2

これで ``mkzopeinstance`` などの関連スクリプトが、グローバル、
あるいは仮想Python環境の ``bin`` ディレクトリに作成されます。

``virtualenv`` の使用を **強く推奨します** 。そうしないと拶既に
インストールしているパッケージとの想定しない競合が発生するでしょう。


Zope インスタンスの作成
------------------------

インストールが完了したら、実際にZopeを使い始めるために
"インスタンスホーム" を作成する必要があります。 インスタンスホーム
ディレクトリには設定ファイルとZopeサーバーのデータが含まれています。
インスタンスホームを作成するには ``mkzopeinstance`` スクリプトを
使用します::

  $ bin/mkzopeinstance

もしSVNから取得したZopeを使用するのであれば、インスタンスが使用する
Pythonインタプリタを明示的に指定する必要があります::

  $ bin/mkzopeinstance --python=$PWD/bin/zopepy

``mkzopeinstance`` の実行中に管理者アカウントのユーザー名とパスワード
の入力を求められるでしょう。コマンドラインオプションの詳細を見るため
には、スクリプトを ``--help`` オプション付きで実行します::

  $ bin/mkzopeinstance --help

.. note::
  従来型の ``inplace`` ビルドはサポート外になりました。
  Zopeインスタンスの作成にbuildoutやvirtualenv環境を使用しないでください。
  Zopeインスタンスの作成には ``mkzopeinstance`` を使い、buildoutや
  virtualenv環境の外に作成してください。


Zope をデーモン起動する
-------------------------

インスタンスホームを作成したら、以下のコマンドでZopeサーバーを起動する
ことが出来ます::

  $ /path/to/zope/instance/bin/zopectl start

Zope起動中に、./log/event.logにログメッセージを出力します。
もし、Zope起動中に何らかのエラーが発生した場合には、任意のツール
(cat, more, tail)を用いて内容を確認することができます。


Zope をフォアグラウンドで実行する
----------------------------------

デフォルトでは、 ``zopectl start`` でバックグラウンドプロセス(daemon)
としてZopeを起動します。 ``zopectl stop`` でバックグラウンドプロセス
を終了させます。Zopeをコンソールから切り離さずに起動するには ``fg``
コマンド(``foreground`` の省略形)を使用します::

  $ /path/to/zope/instance/bin/zopectl fg

このモードでは、Zopeはログメッセージをコンソールに出力し、ターミナル
から切り離されません。


Zope の設定
------------

Zopeインスタンスの設定に用いるファイルは以下の方法で見つけられます::

  $ /path/to/zope/instance/bin/zopectl show
  ...
  Config file:  /path/to/zope/instance/etc/zope.conf

あるいはコマンドラインから設定ファイルを明示的に指定できます::

  $ /path/to/zope/instance/bin/zopectl -c /tmp/other.conf show
  ...
  Config file:  /tmp/other.conf

Zopeの起動中にアドレスが既に使用中であるというエラーが表示されたら、
HTTPやFTPで使用するポートを変更する必要があります。
デフォルトでは、HTTPとFTPのポートは8080と8021にそれぞれ設定されています。
このポート番号は ./etc/zope.confを編集して変更する事が出来ます。

設定ファイル内に以下のセクションがあります::

  <http-server>
    # valid keys are "address" and "force-connection-close"
    address 8080
    # force-connection-close on
  </http-server>

アドレスとして上記のようにポート番号を記載することもできるし、
host:port のペアを指定することにより特定のインターフェースでのみ
起動する事も出来ます。


システムの起動に組み込む
--------------------------

zopectl をlinuxや他のSystem V unixのrc-scriptとしてリンクして使用する
こともできます。

``zopectl`` は引数無しで起動する事で対話モードで使用することができます。
``help`` や ``help <command>`` と入力すれば、各種コマンドを調べる
ことができます。これらのコマンドはコマンドラインからも使用できます。


Zope のログ記録
------------------

Zopeを起動したら、Zopeウェブサーバーに接続することが出来ます。
ブラウザで以下のURLにアクセスしてください::

  http://yourhost:8080/manage

'yourhost' はZopeが動作しているサーバーのDNS名かIPアドレスで置き換えて
ください。もしHTTPポートを変更しているのであれば設定したポートでアクセス
してください。

ユーザー名とパスワードの入力を求められます。インスタンスの作成時に
指定したユーザー名とパスワードを入力してください。

うまくいけば、フレームで２つに分割されたZopeの管理画面が表示されます。
左のフレームはZopeのオブジェクトのナビゲーション用で、右のフレームは
は上部にタブがあり、各タブでそれぞれ異なる管理機能を提供します。

もしまだZopeを使ったことがないのであれば、ZopeのWebサイトで色々な
ドキュメントを読むことを勧めます。Zopeドキュメントセクションは
始めに読むのに適しています。以下のURLでアクセスしてください:

http://docs.zope.org/

トラブルシューティング
-----------------------

- このバージョンのZopeはPython 2.5.4以降(2.6.xを含む)が必要です。
  Python 3.x では動作しません。

- Zopeで使用するPythonは *必ず* thread対応でコンパイルしてください。
  (which is the case for a vanilla build).
  注意: Zopeは ``libpth`` を使用しているPythonでは動作しません。
  *かならず* ``libpthread`` を使用してください。

- Python拡張モジュールをビルドする場合に注意するべき情報があります。
  もしPythonをRPMでインストールしている場合、python-devel (あるいは
  python-dev)パッケージもインストールしてください。
  Pythonをソースからビルドしている場合についてはこのページの説明を
  参照してください。

- このバージョンのZopeについての重要な情報が `<CHANGES.html>`_
  にあります。参照してください。

.. rubric:: (Translated by Shimizukawa, `r104646 <http://svn.zope.org/Zope/tags/2.12.0/doc/INSTALL.rst?rev=104646&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/INSTALL.html>`_)

