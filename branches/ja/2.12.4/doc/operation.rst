Configuring and Running Zope
============================

.. highlight:: bash


Whichever method you used to install Zope and create a server instance (see
:doc:`INSTALL` and :doc:`INSTALL-buildout`), the end result is configured
and operated the same way.


Configuring Zope
----------------

Your instance's configuration is defined in its ``etc/zope.conf`` file.
Unless you created the file manually, that file should contain fully-
annotated examples of each directive.

You can also pass an explicit configuration file on the commandline::

  $ /path/to/zope/instance/bin/zopectl -c /tmp/other.conf show
  ...
  Config file:  /tmp/other.conf

When starting Zope, if you see errors indicating that an address is in
use, then you may have to change the ports Zope uses for HTTP or FTP. 
The default HTTP and FTP ports used by Zope are
8080 and 8021 respectively. You can change the ports used by
editing ./etc/zope.conf appropriately.

The section in the configuration file looks like this::

  <http-server>
    # valid keys are "address" and "force-connection-close"
    address 8080
    # force-connection-close on
  </http-server>

The address can just be a port number as shown, or a  host:port
pair to bind only to a specific interface.

After making any changes to the configuration file, you need to restart any
running Zope server for the affected instance before changes are in effect.


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

During startup, Zope emits log messages into
.  You can examine it with the usual
tools (``cat``, ``more``, ``tail``, etc) and see if there are any errors
preventing Zope from starting.

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

.. rubric:: (Translated by Shimizukawa, `r110302 <http://svn.zope.org/Zope/branches/2.12/doc/operation.rst?rev=110302&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/operation.html>`_)
  :class: translator

