Zope 2.13 の新機能
====================

このページでは、このバージョンの Zope 2 の新しい高度な機能と変更点について
説明します。

新機能やバグ修正の細目については、 `変更の詳細 <CHANGES.html>`_
を参照してください。


Python 2.7
----------

.. This release of Zope 2 adds support for
.. `Python 2.7 <http://www.python.org/download/releases/2.7/>`_. Please refer to
.. the `What's new in Python 2.7 <http://docs.python.org/dev/whatsnew/2.7.html>`_
.. document, if you want to know more about the changes.

このZope2リリースには `Python 2.7 <http://www.python.org/download/releases/2.7/>`_
サポートが追加されています。Python 2.7 で何が変わったのかを詳しく知りたい場合は、
`What's new in Python 2.7 <http://docs.python.org/dev/whatsnew/2.7.html>`_
を参照してください。

.. Zope 2.13 is continuing to support Python 2.6.4 or any later maintenance release
.. of it. There's currently no support for any Python 3.x version. Work has begun
.. in the Zope Toolkit to port some of the lower level packages to Python 3.

Zope 2.13 はこれまでの Python 2.6.4 またはそれ以降のメンテナンスリリースバージョンを
サポートしていきます。そして Python 3 以降のバージョンについては、現時点では
サポートしません。Zope2の下層レイヤで使用している Zope Toolkit は Python 3
への対応を開始しています。


ZODB 3.10
---------

.. This version of Zope includes ZODB 3.10 - a new major version of the ZODB.
.. Among the notable changes are a variety of performance improvements. The ZEO
.. server process is now multi-threaded. If the underlying file system and disk
.. storage can handle concurrent disk I/O efficiently a throughput increase by a
.. factor of up to four has been seen. On a related note using solid state disks
.. for the ZEO server has a similar effect and can increase throughput by the
.. same factor. Both of these effects combined can lead to an increase of up to
.. sixteen times the throughput in high load scenarios.

このバージョンのZopeにはZODB 3.10というZODBの新しいメジャーバージョンを
同梱しています。大きな変化として様々な性能改善が行われています。
ZEOサーバーのプロセスはマルチスレッドになりました。このため、利用している
ファイルシステムとDISKストレージが並列DISK I/Oを効率的に処理出来るので
あれば最大4倍までスループットが増加するでしょう。これに関連して、
ソリッドステートディスク(SSD)をZEOサーバーに使うことで同様にスループット
が向上するでしょう。両方を組み合わせることで16倍までのスループットを
得ることも可能です。

.. File storage indexes use a new format, which is both smaller in size and can
.. be read much faster. The repozo backup script now also backs up the index files
.. in addition to the actual data, so in a restore scenario the index doesn't have
.. to be recreated. For large databases this can bring down the total downtime in
.. a restore scenario by a significant amount of time.

ファイルストレージのインデックスについてフォーマットが新しくなりました。
これにより、サイズが小さくなり読み込みが非常に速くなりました。repozoの
バックアップスクリプトがデータだけでなくインデックスファイルもバックアップスクリプト
するようになったため、復元した際にインデックスを再作成しなくてよくなりました。
大きなデータベースではインデックスの復元にかかる時間を省略できるため、
ダウンタイムを削減することができます。

.. The ZODB has added support for wrapper storages that transform pickle data.
.. Applications for this include compression and encryption. A storage using
.. standard zlib compression is available as a new package called
.. `zc.zlibstorage <http://pypi.python.org/pypi/zc.zlibstorage>`_. In content
.. management scenarios where strings constitute the most of the non-blob data,
.. this can reduce the Data.fs size by a factor of two or more. The overhead of
.. compressing and uncompressing is negligible. This saves both network I/O and
.. disk space. More importantly the database has better chances of fitting into
.. the operating systems disk cache and thus into memory. The second advantage is
.. less important when using solid state disks.

ZODBにpickleデータを変換するためのラッパーストレージ機能が追加されました。
これには圧縮と暗号化の機能が含まれています。このストレージにzlibでの圧縮機能を
提供している `zc.zlibstorage <http://pypi.python.org/pypi/zc.zlibstorage>`_
という新しいパッケージを使用できます。コンテンツ管理に使っている状況で、
データの多くがblobではなく文字列なのであれば、Data.fsサイズを半分以下に削減
させる効果を見込めます。圧縮や展開のオーバーヘッドコストは無視できる程度です。
この仕組みはネットワークI/Oとディスクスペースの両方に効果があります。
そしてデータベースにとってより重要なのは、OSのディスクキャッシュやメモリに
乗る可能性が高くなったことです。SSDを使っている場合は、2つめのアドバンテージは
それほど重要ではありません。

.. Databases now warn when committing very large records (> 16MB). This is to try
.. to warn people of likely design mistakes. There is a new option
.. (large_record_size/large-record-size) to control the record size at which the
.. warning is issued. This should help developers to better understand the storage
.. implications of their code, which has been rather transparent so far.

Databaseに16MB以上の大きなデータを保存しようとすると警告されるようになりました。
これは設計の間違いなどを警告する試みです。これに関連して新しいオプション
(large_record_size/large-record-size) で警告するレコードサイズを指定します。
こういった部分はこれまでは透過的に開発者には見えないようにしていましたが、
これによって開発者がストレージ周りの実装コードについてより理解する助けに
なると思います。


.. The mkzeoinst script has been moved to a separate project
.. `zope.mkzeoinstance <http://pypi.python.org/pypi/zope.mkzeoinstance>`_ and is
.. no-longer included with ZODB. You will need to use this new package to set up
.. ZEO servers or use the
.. `plone.recipe.zeoserver <http://pypi.python.org/pypi/plone.recipe.zeoserver>`_
.. recipe if you use `buildout <http://www.buildout.org/>`_.

mkzopeinst スクリプトは
`zope.mkzeoinstance <http://pypi.python.org/pypi/zope.mkzeoinstance>`_
という別プロジェクトに分割され、ZODBパッケージには含まれなくなりました。
このためZEO環境を作る必要がある場合にはこのパッケージをインストールするか、
`buildout <http://www.buildout.org/>`_ を使っているのであれば
`plone.recipe.zeoserver <http://pypi.python.org/pypi/plone.recipe.zeoserver>`_
レシピを使って下さい。

.. More information can be found in the detailed
.. `change log <http://pypi.python.org/pypi/ZODB3/3.10.0b1.>`_.

より詳細な情報を知りたい場合は
.. `change log <http://pypi.python.org/pypi/ZODB3/3.10.0b1.>`_.
を参照して下さい。


WSGI
----

.. This Zope release comes with native WSGI support. First pioneered in the
.. repoze.zope2 project, this capability finally found its way back into the core
.. and obsoletes the externally managed project. With WSGI Zope 2 can natively talk
.. to a variety of web servers and isn't restricted to its own ZServer anymore. It
.. also opens up new possibilities for writing or reusing middleware in Zope 2 or
.. factoring out capabilities into WSGI endware. It's expected that this new
.. deployment model will over time become the default and the old ZServer
.. implementation will be deprecated. There's no concrete timeline for this yet.

このバージョンのZopeからWSGIを標準サポートします。WSGIの先駆者は
repoze.zope2プロジェクトでしたが、ここで最終的に得られた技術をZopeのコア
に取り込むことが出来たため、外部プロジェクトを使わなくてもよくなりました。
WSGIをZope2標準で使えるようになったため、様々なWebサーバーと連携するために
ZServerの制約を受ける事がなくなりました。これはさらにZope2用のミドルウェア
を作成したり再利用したり、Zope2をWSGIのパーツとして提供するなどの、
新しい可能性を開きます。将来的にはこのデプロイ方式が標準となっていき、
古いZServerでの方式は廃止されるでしょう。これについての具体的なスケジュール
はまだ決まっていません。

.. NOTE: There's no setup documentation nor streamlined instance creation logic
.. for a WSGI setup yet. This will be provided in a later alpha release.

ノート: WSGI用のセットアップドキュメントや現代的なインスタンス作成手順はまだ
ありません。これらは今後のαリリースで提供していく予定です。



Zope Toolkit
------------

.. Zope 2.13 has neither direct nor indirect ``zope.app.*`` dependencies anymore.
.. This finishes the transition from the hybrid Zope 2 + 3 codebase. Zope 3 itself
.. has been split up into two projects, the underlying Zope Toolkit consisting of
.. foundation libraries and the application server part. The application server
.. part has been renamed BlueBream. Zope 2 only depends and ships with the Zope
.. Toolkit now.

Zope 2.13 には直接的にも間接的にも ``zope.app.*`` への依存はもうありません。
これでZope2と3のコードべース融合への移行は完了しました。Zope3自体も2つの
プロジェクトに分割され、片方はZope Toolkitという基本ライブラリ部分となり、
もう片方はアプリケーションサーバー部分です。アプリケーションサーバー部分は
BlueBreamと名付けられました。Zope2はZope Toolkitにのみを同梱し依存しています。

.. Large parts of code inside Zope 2 and specifically Products.Five have been
.. refactored to match this new reality. The goal is to finally remove the Five
.. integration layer and make the Zope Toolkit a normal integral part of Zope 2.

Zope2のコードの多くの部分を占めていたProducts.FiveはZope Toolkitの利用に
向けてリファクタリングしていきます。最終的なゴールはFiveインテグレーション層
を取り除き、Zope ToolkitをZope2から直接使うようにすることです。


リファクタリング
----------------

.. There's an ongoing effort to refactor Zope 2 into more independent modularized
.. distributions. Zope 2.12 has already seen a lot of this, with the use of zope.*
.. packages as individual distributions and the extraction of packages like
.. Acquisition, DateTime or tempstorage to name a few. Zope 2.13 continues this
.. trend and has moved all packages containing C extensions to external
.. distributions. Among those are AccessControl, DocumentTemplate and
.. Products.ZCTextIndex.

Zope2を独立したモジュールにするためにさらなるリファクタリングの努力が続けられて
います。Zope2.12で既に多くのリファクタリングが行われ、分割されたzope.*パッケージ
を使い、Zope2自身もAcquisition, DateTime, tempstorageなどのパッケージを分割
しました。Zope2.13はこの流れを進め、C拡張モジュールを含むパッケージを配布物
から分割しました。これに該当するのが AccessControl, DocumentTemplate,
Products.ZCTextIndex です。


Formlibサポートのオプション化
------------------------------

.. Zope 2 made a number of frameworks available through its integration layer
.. Products.Five. Among these has been direct support for an automated form
.. generation framework called zope.formlib with its accompanying widget library
.. zope.app.form.

Zope2はいくつかのフレームワークをProducts.Fiveというインテグレーション層で
統合して構成されています。このうちの一つに、zope.formlibというフォームを
自動生成するフレームワークがあり、これにzope.app.form Widgetライブラリが
付いてきます。

.. This form generation framework has seen only minor adoption throughout the Zope
.. community and more popular alternatives like z3c.form exist. To reflect this
.. status Zope 2 no longer directly contains formlib support.

このフォーム生成フレームワークはZopeコミュニティーの中ではあまりメジャーではなく、
より一般的な選択肢としてz3c.formなどがあります。これを反映して、Zope2はformlib
を直接同梱するのをやめました。

.. If you rely on formlib, you need to add a dependency to the new five.formlib
.. distribution and change all related imports pointing to Products.Five.form or
.. Products.Five.formlib to point to the new package instead.

もしformlibが必要であれば、新しいfive.formlibへの依存設定を追加し、他にも
Products.Five.formやProducts.Five.fromlibをimportしている箇所をこの新しい
パッケージをimportするように変更して下さい。

.. In order to ease the transition, five.formlib has been backported to the 2.12
.. release series. Starting in 2.12.3 you can already use the new five.formlib
.. package, but backwards compatibility imports are left in place in Products.Five.
.. This allows you to easily adopt your packages to work with both 2.12 and 2.13.

five.formlibは既に2.12リリースとの後方互換性があるので、簡単に移行を始められます。
Zope2.12.3でfive.formlibパッケージを使うとProducts.Fiveが無効化されます。
このため、あなたのパッケージを2.12と2.13の両方に簡単に対応させることができる
ようになります。

.. rubric:: (Translated by Shimizukawa, `r114672 <http://svn.zope.org/Zope/branches/2.13/doc/WHATSNEW.rst?rev=114672&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/WHATSNEW.html>`_)
  :class: translator

