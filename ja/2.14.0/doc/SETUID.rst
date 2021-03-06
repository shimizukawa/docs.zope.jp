Zopeが動作する際のユーザー権限を設定
======================================

.. note:: 
  Apache, Squid, Varnish のような リバースProxy の背後で Zope
  を稼働させるのは良い手法です。この場合、 Zope を root
  権限でインストールや起動する必要はなく、 リバースProxy
  が80番ポートを提供してくれ、全てのリクエストを特権の必要ない
  ポートで起動している Zope に中継してくれます。

Zope はネットワークサービスとして、 21 (FTP) や 80 (HTTP) などの低い
番号のポートを使用することが出来ます。低い番号のポートを使う場合、
Zope は root 権限で起動する必要があります。とはいえ、 Zope が root
権限を必要とするのはそのような低い番号のポートを開くところまでで、
その後は特権の無いユーザーに setuid しようと試みます。

Zope が setuid に使用するユーザーを zope.conf の 'effective-user'
パラメータに設定してください。ユーザーは存在するユーザー名か、 UID
を指定してください。全ての実行中のファイルはそのユーザーの権限で
書き込まれます。もし 'effective-user' を設定せずに Zope を起動しようと
した場合、起動することは出来ないでしょう。

もし 'nobody' を指定した場合、 Zope は警告メッセージを表示します。
この警告メッセージの表示には根拠があります。歴史的に多くの UNIX
サービスが root での起動後に実行権限を 'nobody' アカウントに落とした
ため、そのようなサービスはどれでも 'nobody' 権限を得ることが出来、
システム内で 'nobody' 権限としてアクセス権を得てしまうという
セキュリティー上の欠陥があります。
もし誰かに 'nobody' アカウントを奪われた場合、彼らは Zope のファイルを
自由に出来てしまいます。

'effective-user' サポートについて覚えておいて欲しい最も重要なことは、
低い番号のポート (ポート 1024 以下) を使わなければ Zope を root 権限
で起動する必要はないと言うことです。必要ないのであれば、 Zope を
専用のユーザーアカウントで起動するのがより良いでしょう。

.. rubric:: (Translated by Shimizukawa, `r117404 <http://svn.zope.org/Zope/trunk/doc/SETUID.rst?rev=117404&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.14/SETUID.html>`_)
  :class: translator

