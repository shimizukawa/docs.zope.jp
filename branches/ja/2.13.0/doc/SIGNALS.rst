シグナル (POSIX のみ)
=======================

シグナルは POSIX のプロセス間通信の仕組みです。
Windows を使っている場合、このドキュメントの内容は適用出来ません。

Zope は '$INSTANCE_HOME/var/Z2.pid' に保存されているプロセスIDに送られたシグナルに応じて動作します::

    SIGHUP  - 開いているデータベース接続を閉じて、サーバーを再起動します。
              Zope サーバーを再起動するイディオムは以下のようになります:

              kill -HUP `cat $INSTANCE_HOME/var/z2.pid`

    SIGTERM - 開いているデータベース接続を閉じて、サーバーを終了します。
              Zope サーバーを終了するイディオムは以下のようになります:

              kill -TERM `cat $INSTANCE_HOME/var/Z2.pid`

    SIGINT  - SIGTERM と同じ意味です。

    SIGUSR1 - dump a stack trace of all threads to stdout. This can help
              diagnosing `stuck` Zope processes if all threads are stuck.

    SIGUSR2 - 全ての Zope のログファイルを一度閉じて再度開きます
              (z2.log, event log, detailed log.)
              Zope ログファイルをローテートした後に以下を実行します:

              kill -USR2 `cat $INSTANCE_HOME/var/z2.pid`


.. rubric:: (Translated by Shimizukawa, `r113828 <http://svn.zope.org/Zope/branches/2.13/doc/SIGNALS.rst?rev=113828&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/SIGNALS.html>`_)
  :class: translator

