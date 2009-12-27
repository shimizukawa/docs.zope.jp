============================
Zope のビルドとインストール
============================

.. highlight:: bash

このドキュメントは Zope の始め方について説明しています。

前提
=====

Zope を使うためには、以下の前提条件をそろえる必要があります:

- ZopeがサポートしているバージョンのPython、システムレベルのパッケージ
  からインストールしている場合は開発サポートも含みます。サポートバージョン
  は以下の通り:

  * 2.6.x

- ZopeはPythonの ``zlib`` モジュールがインポート出来る必要があります。
  Pythonをソースからビルドするのであれば、先にシステムの ``zlib`` 
  ヘッダーをインストールしておいてください。

- Pythonの拡張モジュールをビルドできる C コンパイラー。 (gcc を推奨). 
  この項目は Windows では必須ではありません。 Windows の場合、コンパイル
  が必要な部分については常にバイナリでのリリースが提供されています。

- Windows で Zope をサービス登録したい場合、 `pywin32`__ パッケージを
  インストールしておいてください。

  __ https://sourceforge.net/projects/pywin32/


Zope のインストール
====================

:ref:`後述するような <buildout-instances>`  buildout を使用した
Zope インスタンスの作成を行わない場合、 Zope のインストールを個別
に行う必要があります。もし buildout ベースの Zope インスタンスを
作成したいのであれば、以下のセクションを飛ばして当該セクション
を読んでください。

virtualenv を使用した Zope のインストール
------------------------------------------

Zope を ``virtualenv`` を用いた仮想 Python 環境にインストールする場合、
以下のように行います::

  $ virtualenv --no-site-packages my_zope
  $ cd my_zope
  $ source bin/activate
  $ bin/easy_install -i http://download.zope.org/Zope2/index/<Zope version> Zope2

``virtualenv`` の使用を **強く推奨** します。そうしない場合、既に
インストール済みのパッケージとの望ましくない競合が発生するでしょう。

Zope をインストールしたら次に :ref:`インスタンスの作成 <classic-instances>`
を行ってください。

zc.buildout を使用した Zope のインストール
-------------------------------------------

`Zope の開発`__ を行わないのであれば、おそらくこのセクションで記載している
:ref:`buildout ベースの Zope インスタンス <buildout-instances>`
の作成を行うのがよいでしょう。

__ http://docs.zope.org/developer/

もし buildout で作成した環境において ``mkzopeinstance`` コマンドに
よる従来の Zope インスタンス作成を行いたいのであっても、まずは以下
の手順を行ってください:

- Zope 2 ソースコードの `PyPI`__ からのダウンロード

  __ http://pypi.python.org/pypi/Zope2

- buildout 環境の初期化 (bootstrap)

- buildout の実行

Linux では、以下のように行います::

  $ wget http://pypi.python.org/packages/source/Z/Zope2/Zope2-<Zope version>.tar.gz
  $ tar xfvz Zope2-2.12.0.tar.gz
  $ cd Zope2-2.12.0
  $ /path/to/your/python bootstrap/bootstrap.py
  $ bin/buildout

これで Zope はインストールされ、次は
:ref:`インスタンスの作成 <classic-instances>` を行う必要があります。


easy_install によるZopeのインストール
--------------------------------------

Zope は ``easy_install`` でもインストール出来ますが、前述の
``virtualenv`` を用いた上で行うことをお勧めします。これにより
期待しないインストール済みパッケージとの競合を避けることができます。

easy_install を使ってグローバル環境にインストールするには以下のようにします::

  $ easy_install -i http://download.zope.org/Zope2/index/<Zope version> Zope2

これによって、 ``mkzopeinstance`` のような関連スクリプトが Python の
スクリプトフォルダに生成されます。これらのスクリプトを使用したインスタンス
の作成方法については以下を参照してください。

.. _classic-instances:

従来型の Zope インスタンス作成
================================

Zope のインストールが終わったら、次は "インスタンスホーム" の作成を
行います。これは Zope サーバープロセスが使用する設定とデータファイルを
置くディレクトリです。インスタンスホームは ``mkzopeinstance`` スクリプト
で以下のようにして作成します::

  $ bin/mkzopeinstance

なお、必要であれば Python インタプリタを明示的に指定することも出来ます::

  $ bin/mkzopeinstance --python=$PWD/bin/zopepy

``mkzopeinstance`` の実行中に管理者アカウントのユーザー名とパスワード
の入力を求められるでしょう。コマンドラインオプションの詳細を見るため
には、スクリプトを ``--help`` オプション付きで実行します::

  $ bin/mkzopeinstance --help

.. note::
  従来のような"同一ディレクトリ(inplace)"でのビルドはサポートされません。
  ``mkzopeinstance`` は buildout/virtualenv 環境の外で行ってください。
  buildout を使用して Zope インスタンスを管理したい場合、次のセクション
  を参照してください。

.. _buildout-instances:


buildout ベースの Zope インスタンス作成
========================================

buildout を使った Zope インスタンス管理を行いたい場合、以下の手順で
インスタンスを作成します:

* インスタンス用のディレクトリを作成します。このディレクトリには
  ``etc``, ``logs``, ``var`` の各サブディレクトリを作成します。

* インスタンスディレクトリに以下のファイルをダウンロードして置きます:

  `http://svn.zope.org/*checkout*/zc.buildout/trunk/bootstrap/bootstrap.py`__
    
  __ http://svn.zope.org/*checkout*/zc.buildout/trunk/bootstrap/bootstrap.py

.. highlight:: none

* 以下の内容で buildout 設定ファイルを作成します:

.. topic:: buildout.cfg
 :class: file

 ::

   [buildout]
   parts = instance 
   extends = http://svn.zope.org/*checkout*/Zope/tags/<Zope version>/versions.cfg

   [instance]
   recipe = zc.recipe.egg
   eggs = Zope2
   interpreter = py
   scripts = runzope zopectl
   initialization =
     import sys
     sys.argv[1:1] = ['-C',r'${buildout:directory}/etc/zope.conf']

これは最小の、しかし buildout の全ての有意な技術を使用している例です。

* Zope の設定ファイルを以下の内容で作成します:

.. topic:: etc/zope.cfg
 :class: file

 ::

   %define INSTANCE <path to your instance directory>

   python $INSTANCE/bin/py[.exe on Windows]
 
   instancehome $INSTANCE

.. highlight:: bash

* 最後に以下のコマンドを実行します::

    $ /path/to/your/python bootstrap.py
    $ bin/buildout

  インスタンスの ``bin`` サブディレクトリに ``runzope`` と ``zopectl``
  スクリプトが作成されるでしょう。


Zope インスタンスを使う
========================

新しく作成したインスタンスから Zope を実行するには、いくつかの異なる
手順が考えられます。以下に記載します。

Zope をフォアグラウンド起動する
--------------------------------

Zope をコンソールから切り離さずに実行するには、 ``fg`` (``foreground``
の短縮形) コマンドを使用します::

  $ /path/to/zope/instance/bin/zopectl fg

このモードでは、 Zope のログメッセージがコンソールに出力され、ターミナル
からは切り離されません。


Zope をデーモン起動する
-------------------------

インスタンスホームを作成したら、以下のコマンドでZopeサーバーを起動する
ことが出来ます::

  $ /path/to/zope/instance/bin/zopectl start

Zope起動中に、./log/event.logにログメッセージを出力します。
もし、Zope起動中に何らかのエラーが発生した場合には、任意のツール
(cat, more, tail)を用いて内容を確認することができます。

.. highlight:: none
.. note::

  Windows でこのモードを使用する場合、 Zope インスタンスをサービス
  として登録する必要があります。以下のように実行します::

    bin\zopectl install

  サービスから削除する場合は以下のようにします::

    bin\zopectl remove

  Windowsサービスとしての全てのオプションを表示するには以下のように
  実行します::

    bin\zopectl install --help

.. highlight:: bash


システムの起動に組み込む
--------------------------

zopectl をlinuxや他のSystem V unixのrc-scriptとしてリンクして使用する
こともできます。

``zopectl`` は引数無しで起動する事で対話モードで使用することができます。
``help`` や ``help <command>`` と入力すれば、各種コマンドを調べる
ことができます。これらのコマンドはコマンドラインからも使用できます。

.. note::

  Windows では以下の方法でサービス登録と自動起動を同時に設定出来ます:

  .. code-block:: none

    bin\zopectl install --startup=auto

Zope の設定
================

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


Zope のログ記録
================

Zopeを起動したら、Zopeウェブサーバーに接続することが出来ます。
ブラウザで以下のURLにアクセスしてください::

  http://yourhost:8080/manage

'yourhost' はZopeが動作しているサーバーのDNS名かIPアドレスで置き換えて
ください。もしHTTPポートを変更しているのであれば設定したポートでアクセス
してください。

ユーザー名とパスワードの入力を求められます。インスタンスの作成時に
指定したユーザー名とパスワードを入力してください。

If you are using a buildout-based Zope instance, you will need to
create a user as follows::

  $ bin/zopectl adduser username password

うまくいけば、フレームで２つに分割されたZopeの管理画面が表示されます。
左のフレームはZopeのオブジェクトのナビゲーション用で、右のフレームは
は上部にタブがあり、各タブでそれぞれ異なる管理機能を提供します。

もしまだZopeを使ったことがないのであれば、ZopeのWebサイトで色々な
ドキュメントを読むことを勧めます。Zopeドキュメントセクションは
始めに読むのに適しています。以下のURLでアクセスしてください
http://docs.zope.org/

トラブルシューティング
=======================

- このバージョンのZopeはPython 2.6.4以降が必要です。
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

- このバージョンのZopeについての重要な情報が :doc:`CHANGES`
  にあります。参照してください。

.. rubric:: (Translated by Shimizukawa, `r105416 <http://svn.zope.org/Zope/tags/2.12.1/doc/INSTALL.rst?rev=105416&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/INSTALL.html>`_)
  :class: translator

