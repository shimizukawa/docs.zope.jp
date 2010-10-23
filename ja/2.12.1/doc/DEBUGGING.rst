Zopeのデバッグモード
=====================

Zope をデバッグモードで起動するには、設定ファイルの 'debug-mode' を 'on' に設定します(これがデフォルトです)。これによって以下の効果があります:

- UNIX では、 Zope がコンソールから切り離されなくなります。

- Z_DEBUG_MODE 環境変数がセットされ、これにより、 Zope の挙動が開発
  向きになります。
  設定ファイルの 'debug-mode' の説明に詳しい情報を記載しています。

'zopectl debug' を使う
-----------------------

インスタンスホームに 'zopectl' ユーティリティーが用意されています。このスクリプトに 'debug' という引数を渡すことによって、 Python の対話モードを起動し、 Zope インスタンスの実行時の状態を分析する事が出来ます。 '最上位' の Zope オブジェクト (ルートフォルダ) は 'app' という名前でインタプリタで提供されます。 Python の標準的な関数などを使って app を操作したり、返値を分析することが出来ます::

    [chrism@james Trunk]$ bin/zopectl debug
    Starting debugger (the name "app" is bound to the top-level Zope object)
    >>> app.objectIds()
    ['acl_users', 'Control_Panel', 'temp_folder', 'browser_id_manager', 'session_data_manager', 'error_log', 'index_html', 'standard_error_message']
    >>> 

.. rubric:: (Translated by Shimizukawa, `r105416 <http://svn.zope.org/Zope/tags/2.12.1/doc/DEBUGGING.rst?rev=105416&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/DEBUGGING.html>`_)
  :class: translator

