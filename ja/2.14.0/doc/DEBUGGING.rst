Zopeのデバッグモード
=====================

..  A utility known as 'zopectl' is installed into generated instance homes.

インスタンスホームに 'zopectl' ユーティリティーコマンドがあります。

.. If you wish to run Zope in debug mode, run zopectl in foreground mode::

もしZopeをデバッグモードで起動したければ、以下のようにzopectlを
フォアグラウンドで起動して下さい::

  $ bin/zopectl fg

.. You can also use it to inspect a Zope instance's running state via an
.. interactive Python interpreter by passing zopectl the 'debug' parameter on the
.. command line.
.. The 'top-level' Zope object (the root folder) will be bound to the name 'app'
.. within the interpreter. You can then use normal Python method calls against app
.. and use the Python interpreter normally to inspect results::

他にも、コマンドラインでzopectlに 'debug' という引数を渡すことによって、
Python の対話モードを起動し、 Zope インスタンスの実行時の状態を分析する事が
出来ます。 '最上位' の Zope オブジェクト (ルートフォルダ) は 'app' という
名前でインタプリタで提供されます。 Python の標準的な関数などを使って app
を操作したり、返値を分析することが出来ます::

  $ bin/zopectl debug
  Starting debugger (the name "app" is bound to the top-level Zope object)
  >>> app.keys()
  ['acl_users', 'Control_Panel', 'temp_folder', 'browser_id_manager', 'session_data_manager', 'error_log', 'index_html', 'standard_error_message']
  >>>

.. rubric:: (Translated by Shimizukawa, `r117404 <http://svn.zope.org/Zope/trunk/doc/DEBUGGING.rst?rev=117404&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.14/DEBUGGING.html>`_)
  :class: translator

