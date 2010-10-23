``zc.buildout`` を用いた Zope のインストール
=============================================

.. highlight:: bash

このドキュメントは ``zc.buildout`` を用いた Zope の始め方について
説明しています。

``zc.buildout`` とは
---------------------

`zc.buildout <http://www.buildout.org/>`_ は、与えられたソフトウェアの
設定と環境について同じものを繰り返し構築することをサポートする
強力なツールです。 Zope の開発者は Zope 開発に際して ``zc.buildout``
と関連するパッケージを使っています。


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


zc.buildoutによるZopeのスタンドアロンインストール
---------------------------------------------------

ここでの設定方法では、 ``zc.buildout`` を使って Zope ソフトウェアを
インストールしますが、サーバーの "インスタンス" は buildout
環境の外に生成します。

Zope ソフトウェアのインストール
::::::::::::::::::::::::::::::::

Zope ソフトウェアのインストールは、 ``zc.buildout`` を使って、
以下の手順で行います:

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

ここまでは、 Zope 本体とインスタンスを分けて用意する方法を説明してきましたが、
もう一つの方法として、 ``zc.buildout`` を使って buildout 環境の中に
Zope ソフトウェアと、設定と、インスタンスデータを一括で用意する事も出来ます。
このためには以下の手順で環境を構築して下さい:

- buildout用のディレクトリを作成します。このディレクトリには
  ``etc``, ``logs``, ``var`` の各サブディレクトリを作成します。

- buildout の bootstrap スクリプトを buildout 環境内に取得して置きます。

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

- buildout 環境の初期化 (bootstrap)

- buildout の実行

- Zope の設定ファイル作成します。以下は最小版の例です:

.. topic:: etc/zope.cfg
 :class: file

 ::

   %define INSTANCE <インスタンスディレクトリのパス>

   python $INSTANCE/bin/py[.exe Windowsで]
 
   instancehome $INSTANCE


Zope2のeggの中には、完全なzope.cfgの設定が含まれています::

   $ cat eggs/Zope2--*/Zope2/utilities/skel/etc/zope.conf.in

   <zope.confの他の設定、例えば、データベースやログファイルなど>

.. highlight:: bash

環境を構築する例は以下のようになります::

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

インスタンスディレクトリ内の ``bin`` サブディレクトリに、
これ以降使っていくことになる ``runzope`` と ``zopectl``
スクリプトが作成されます。

``zopectl`` は引数無しで起動する事で対話モードで使用することができます。
``help`` や ``help <command>`` と入力すれば、各種コマンドを調べる
ことができます。これらのコマンドはコマンドラインからも使用できます。

なお、 `plone.recipe.zope2instance
<http://pypi.python.org/pypi/plone.recipe.zope2instance>`_
などのレシピを使うことで、前述の手順を自動化することが出来ます。

インストールが完了したら、 :doc:`operation` ドキュメントを参照して、
Zope の設定を行い、実行しましょう。


.. rubric:: (Translated by Shimizukawa, `r110522 <http://svn.zope.org/Zope/branches/2.12/doc/INSTALL-buildout.rst?rev=110522&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/INSTALL-buildout.html>`_)
  :class: translator

