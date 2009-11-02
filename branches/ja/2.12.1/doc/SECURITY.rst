ファイルシステム パーミッション
=================================

Zope の データ格納ディレクトリのパーミッションを設定する必要があり、
このディレクトリは通常ではインスタンスHomeの `var` ディレクトリです。
Zope はこのディレクトリに読み書き両方を行う必要があります。
Zope を起動する前に Zope の実行ユーザー権限でこのディレクトリに
アクセスする必要十分な設定をしておいてください。

Zope をどのように実行するかによっては、 このディレクトリに異なる
パーミッションを設定する必要があるでしょう。既存のサーバーに
入っている Zope を使用する場合などには、おそらく Zope は 'nobody'
権限で動作するでしょうから、この場合は 'nobody' ユーザーに対して
読み書きを許可するようにこのディレクトリに設定します。

Zope の実行方法を変更した場合、それに合わせて var ディレクトリと
中のファイルのアクセス権限を設定して、 Zope がそれらのファイルに
新しく設定した userid で読み書き出来るようにしてください。

.. rubric:: (Translated by Shimizukawa, `r105416 <http://svn.zope.org/Zope/tags/2.12.1/doc/SECURITY.rst?rev=105416&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/SECURITY.html>`_)
  :class: translator

