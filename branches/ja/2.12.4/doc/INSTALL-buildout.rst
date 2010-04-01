``zc.buildout`` を用いた Zope のインストール
=============================================

.. highlight:: bash

このドキュメントは ``zc.buildout`` を用いた Zope の始め方について
説明しています。

About ``zc.buildout``
---------------------

`zc.buildout <http://www.buildout.org/>`_ is a powerful tool for creating
repeatable builds of a given software configuration and environment.  The
Zope developers use ``zc.buildout`` to develop Zope itself, as well as
the underlying packages it uses.


前提
-----

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


Installing standalone Zope using zc.buildout
--------------------------------------------

In this configuration, we use ``zc.buildout`` to install the Zope software,
but then generate server "instances" outside the buildout environment.

Installing the Zope software
::::::::::::::::::::::::::::

Installing the Zope software using ``zc.buildout`` involves the following
steps:

- Zope 2 ソースコードの `PyPI`__ からのダウンロード

  __ http://pypi.python.org/pypi/Zope2

- buildout 環境の初期化 (bootstrap)

- buildout の実行

Linux では、以下のように行います::

  $ wget http://pypi.python.org/packages/source/Z/Zope2/Zope2-<Zope version>.tar.gz
  $ tar xfvz Zope2-<Zope version>.tar.gz
  $ cd Zope2-<Zope version>
  $ /path/to/your/python bootstrap/bootstrap.py
  $ bin/buildout

これで Zope はインストールされ、次は
:ref:`インスタンスの作成 <classic-instances>` を行う必要があります。


Zope インスタンス作成
::::::::::::::::::::::::

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
  ``mkzopeinstance`` は buildout 環境の外で行ってください。


buildout ベースの Zope インスタンス作成
========================================

Rather than installing Zope separately from your instance, you may wish
to use ``zc.buildout`` to create a self-contained environment, containing
both the Zope software and the configuration and data for your server.
This procedure involves the following steps:

- buildout用のディレクトリを作成します。このディレクトリには
  ``etc``, ``logs``, ``var`` の各サブディレクトリを作成します。

- Fetch the buildout bootstrap script into the environment.

- 以下の内容で buildout 設定ファイルを作成します:

.. topic:: buildout.cfg
 :class: file

 ::

   [buildout]
   parts = instance 
   extends = http://download.zope.org/Zope2/index/<Zope version>/versions.cfg

   [instance]
   recipe = zc.recipe.egg
   eggs = Zope2
   interpreter = py
   scripts = runzope zopectl
   initialization =
     import sys
     sys.argv[1:1] = ['-C',r'${buildout:directory}/etc/zope.conf']

これは最小の、しかし buildout の全ての有意な技術を使用している例です。

- Bootstrap the buildout

- Run the buildout

* Zope の設定ファイル作成します。以下は最小版の例です:

.. topic:: etc/zope.cfg
 :class: file

 ::

   %define INSTANCE <path to your instance directory>

   python $INSTANCE/bin/py[.exe on Windows]
 
   instancehome $INSTANCE


A fully-annotated sample can be found in the Zope2 egg::

   $ cat eggs/Zope2--*/Zope2/utilities/skel/etc/zope.conf.in

   <rest of the stuff that goes into a zope.conf, e.g. databases and log files.>

.. highlight:: bash

An example session::

   $ mkdir /path/to/instance
   $ cd /path/to/instance
   $ mkdir etc logs var
   $ wget http://svn.zope.org/zc.buildout/trunk/bootstrap/bootstrap.py
   $ vi buildout.cfg
   $ /path/to/your/python bootstrap.py
   $ bin/buildout
   $ cat eggs/Zope2--*/Zope2/utilities/skel/etc/zope.conf.in > etc/zope.conf
   $ vi etc/zope.conf  # replace <<INSTANCE_HOME>> with buildout directory
   $ bin/zopectl start

In the ``bin`` subdirectory of your instance directory, you will
find ``runzope`` and ``zopectl`` scripts that can be used as
normal.


``zopectl`` は引数無しで起動する事で対話モードで使用することができます。
``help`` や ``help <command>`` と入力すれば、各種コマンドを調べる
ことができます。これらのコマンドはコマンドラインからも使用できます。

Note that there are there are recipes such as `plone.recipe.zope2instance
<http://pypi.python.org/pypi/plone.recipe.zope2instance>`_ which can be
used to automate this whole process.

After installation, refer to :doc:`operation` for documentation on
configuring and running Zope.


.. rubric:: (Translated by Shimizukawa, `r110302 <http://svn.zope.org/Zope/branches/2.12/doc/INSTALL-buildout.rst?rev=110302&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/INSTALL-buildout.html>`_)
  :class: translator

