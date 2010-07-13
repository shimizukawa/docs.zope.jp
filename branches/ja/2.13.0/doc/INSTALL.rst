Zope のインストール
====================

.. highlight:: bash

このドキュメントは Zope の始め方について説明しています。

前提
-----

Zope を使うためには、以下の前提条件をそろえる必要があります:

- ZopeがサポートしているバージョンのPython、システムレベルのパッケージ
  からインストールしている場合は開発サポートも含みます。サポートバージョン
  は以下の通り:

  * 2.6.x
  * 2.7.x

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
--------------------

推奨される Zope のインストール方法は ``virtualenv`` を用いた仮想 Python
環境にインストールする方法で、以下のように行います::

  $ virtualenv --no-site-packages my_zope
  $ cd my_zope
  $ bin/easy_install -i http://download.zope.org/Zope2/index/<Zope version> Zope2


もし、システムにまだ ``virtualenv`` がインストールされていないのであれば、
最新版を `virtualenv PyPI page <http://pypi.python.org/pypi/virtualenv>`_
から取得して以下のようにしてインストールしてください::

  $ wget http://pypi.python.org/packages/source/v/virtualenv/virtualenv-1.4.6.tar.gz
  $ tar xzf virtualenv-1.4.6.tar.gz
  $ cd virtuaenv-1.4.6
  $ /path/to/python2.7 setup.py install

virtualenv ではなく buildout を使って Zope のインスタンスを管理したいので
あれば、 :doc:`INSTALL-buildout` を参照して下さい。


Zope インスタンス作成
----------------------

Zope のインストールが終わったら、次は "インスタンスホーム" の作成を
行います。これは Zope サーバープロセスが使用する設定とデータファイルを
置くディレクトリです。インスタンスホームは ``mkzopeinstance`` スクリプト
で以下のようにして作成します::

  $ bin/mkzopeinstance

``mkzopeinstance`` の実行中に管理者アカウントのユーザー名とパスワード
の入力を求められるでしょう。コマンドラインオプションの詳細を見るため
には、スクリプトを ``--help`` オプション付きで実行します::

  $ bin/mkzopeinstance --help

.. note::
   従来のような”同一ディレクトリ(inplace)”でのビルドはサポートされません。
   virtualenv の環境外で ``mkzopeinstance`` を使用してインスタンスを作成
   してください。


.. rubric:: (Translated by Shimizukawa, `r113828 <http://svn.zope.org/Zope/branches/2.12/doc/INSTALL.rst?rev=113828&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/INSTALL.html>`_)
  :class: translator

