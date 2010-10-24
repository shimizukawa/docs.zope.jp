Zope の設定と実行
==================

.. highlight:: bash


Zope のインストールとサーバーのインスタンスの作成をどの方法で行っても、
最後に設定する内容と手順は同じになります。インストール方法については、
:doc:`INSTALL` や :doc:`INSTALL-buildout` を参照して下さい。


Zope の設定
------------

インスタンスの設定はインスタンス内の ``etc/zope.conf`` ファイルで行います。
このファイルは手動で作成しなくても、各設定の例がすべて含まれています。

また、別に用意した設定ファイルをコマンドラインで明示的に指定することも
出来ます::

  $ /path/to/zope/instance/bin/zopectl -c /tmp/other.conf show
  ...
  Config file:  /tmp/other.conf

Zope を起動したときに、ポートが使用されているというエラーが表示された場合は、
Zope が使用する HTTP や FTP のポート設定を変更する必要があります。
Zope がデフォルトで使用する HTTP と FTP のポートは8080と8021です。
このポート番号は ./etc/zope.conf を編集して変更する事が出来ます。

ポート番号を設定するセクションは以下の箇所です::

  <http-server>
    # valid keys are "address" and "force-connection-close"
    address 8080
    # force-connection-close on
  </http-server>

上記の address の部分にはポート番号のみを書くほかに、 host:port という
形式で記載する事で特定のインターフェースとのみバインドすることが出来ます。

設定ファイルの記述を変更したら、変更した内容を反映するために Zope
サーバーを再起動してください。


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

Zope起動中に、 `/path/to/zope/instance/log/event.log` にログメッセージを
出力します。もし、Zope起動中に何らかのエラーが発生した場合には、任意のツール
(``cat``, ``more``, ``tail``, 他)を用いて内容を確認することができます。

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


Zope のログ記録
----------------

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
始めに読むのに適しています。以下のURLでアクセスしてください
http://docs.zope.org/

トラブルシューティング
----------------------

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



Zopeにコマンドを追加する
-------------------------

``setup.py`` に *エントリーポイント* を定義することで ``zopectl`` にコマンドを
追加することができるようになりました。コマンドはは以下のように
``zopectl.command`` グループに定義して下さい:

.. code-block:: python

   setup(name="MyPackage",
         ....
         entry_points="""
         [zopectl.command]
         init_app = mypackage.commands:init_application
         """)

.. note::

   ``zopectl`` の実装においてマイナス記号(``-``)をコマンド名に使用する
   ことは出来ません。

これで ``init_app`` コマンドを以下のようにコマンドラインから使えるように
なります::

    bin\zopectl init_app

コマンドはPythonの呼び出し可能オブジェクトを指定します。これによって、
その呼び出し可能オブジェクトは2つのパラメータを引数として呼び出されます。
1つはZope2アプリケーションオブジェクト、もう一つはコマンドライン引数
のタプルです。基本的な例は以下のコードのようになります:

.. code-block:: python

   def init_application(app, args):
       print 'Initialisating the application'



.. rubric:: (Translated by Shimizukawa, `r117184 <http://svn.zope.org/Zope/branches/2.12/doc/operation.rst?rev=117184&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/operation.html>`_)
  :class: translator

