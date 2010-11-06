Changelog
=========

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については http://docs.zope.jp/zope2/releases/
を参照してください。

2.13.0 (2010-11-05)
-------------------

- 変更ありません。

2.13.0c1 (2010-10-28)
---------------------

バグ修正
++++++++

- LP #628448:  Windows以外の環境での ``zopectl start`` の動作を修正しました。

機能追加
+++++++++

- Zope Toolkit 1.0 に更新。

- 配布物の更新:

  - DateTime = 2.12.6
  - mechanize = 0.2.3
  - ZODB3 = 3.10.1
  - zope.sendmail = 3.7.4
  - zope.testbrowser = 3.10.3

2.13.0b1 (2010-10-09)
---------------------

バグ修正
++++++++

- Avoid iterating over the list of packages to initialize while it is being
  mutated, which was skipping some packages.

- 速いWindowsマシンで失敗していたユニットテスト2つを修正しました。

- Windowsの64bit版PythonにおけるProducts.ZCatalog.LazyのOverflowErrorを
  修正しました。

- 最近のZODBのセマンティックに合うようにZopeTestCase内の ``testZODBCompat``
  テストを修正しました。

- LP #634942: ``nt_svcutils`` をWindowsでのみ必須としました。

機能追加
+++++++++

- Avoid conflict error hotspot in PluginIndexes' Unindex class by using
  IITreeSets instead of simple ints from the start. Idea taken from
  ``enfold.fixes``.

- DateRangeIndex を experimental.daterangeindexoptimisations のアイディア
  を元に内部拡張しました。

- ``Products`` 内のZCMlをパースする際に発生した例外の処理方法についての
  ポリシーを変更しました。この変更により、今後は非デバッグモードでは
  どのような例外も捕捉しません。

- 標準のPluginIndexesに新たにBooleanIndexを追加しました。

- Zope Toolkit 1.0c3 に更新。

- zopectlにsetuptoolsのentry-pointsを使用してコマンドを追加定義する機能を
  追加しました。

- 配布物の更新:

  - Acquisition = 2.13.5
  - Products.MailHost = 2.13.1
  - Products.ZCTextIndex = 2.13.1
  - repoze.retry = 1.0
  - tempstorage = 2.12.1
  - ZODB3 = 3.10.0
  - zope.testbrowser = 3.10.1

2.13.0a4 (2010-09-09)
---------------------

再構築
+++++++

- イベントハンドラ
  ``Products.Five.security.create_permission_from_permission_directive``
  への依存を削除しました。このコードはZope2の ``AccessControl.security``
  パーミッションディレクティブに移動しました。

機能追加
+++++++++

- LP #193122: 新しいメソッド getVirtualRoot を Requestクラスに追加しました。

- ユニットテストの ``assert*`` メソッドを使うようにテストを修正しました。
  これまでは廃止予定の `fail*` エイリアスを使用していました。

- Zope Toolkit 1.0a3 に更新。

- 配布物の更新:

  - AccessControl = 2.13.3
  - Acquisition = 2.13.4
  - ZODB3 = 3.10.0b6

2.13.0a3 (2010-08-04)
---------------------

バグ修正
++++++++

- DateIndexとDateRangeIndexが最新のZODB 3.10.0b4で動作するようにオーバーフロー
  のロジックを調整しました。

- Zope2で独自に実装済みの、 ``zope.*`` パッケージ由来のいくつかのメタZCML
  ハンドラーを除外するようにしました。

- LP #599378: accumulated_headers が正しくヘッダを追加出来ていない問題を修正
  しました。

- browser:view ディレクティブの非公開のpermission属性について、これらの
  属性がallowed_interfaceかallowed_attributesに含まれておらず、かつ、
  親クラスのセキュリティー情報としては定義されているがそれが非公開に
  オーバーライドされていない状況についても修正しました。

- LP #143755: オブジェクトに対して PluginIndexes.common.UnIndex を行い
  値のインデクシングを行う際に、TypeErrorについても対処するようにしました。

- LP #143533: HTTPサーバーの設定でIPアドレスが設定されていない場合に、
  requestオブジェクトのSERVER_NAMEに"0.0.0.0"が設定されていました。
  これをsocketモジュールからFQDNを取得して設定するようにしました。

- LP #143722: ObjectManager.manage_hasId のパーミッション設定が無かった
  ため、FTP経由でファイル名やフォルダ名を変更できなかった問題を修正
  しました。

- LP #143564: Request.resolve_url がパスの探索中に例外が発生すると、
  その例外を正しく再発行していなかった問題を修正しました。

再構築
+++++++

- Removed catalog length migration code. You can no longer directly upgrade a
  Zope 2.7 or earlier database to Zope 2.13. Please upgrade to an earlier
  release first.

- Deprecated the ``Products.ZCatalog.CatalogAwareness`` and
  ``CatalogPathAwareness`` modules.

- Removed deprecated ``catalog-getObject-raises`` zope.conf option.

- Removed unmaintained HelpSys documents from ZCatalog and PluginIndexes.
  Useful explanations are given inside the form templates.

- Deprecate Products.ZCatalog's current behavior of returning the entire
  catalog content if no query restriction applied. In Zope 2.14 this will
  result in an empty LazyCat to be returned instead.

- Deprecate acquiring the request inside Products.ZCatalog's searchResults
  method if no explicit query argument is given.

- Cleaned up the Products.ZCatalog search API's. The deprecated support for
  using `<index id>_usage` arguments in the request has been removed. Support
  for overriding operators via the `<index id>_operator` syntax has been
  limited to the query value for each index and no longer works directly on
  the request. The query is now brought into a canonical form before being
  passed into the `_apply_index` method of each index.

- Factored out the `Products.MailHost` package into its own distributions. It
  will no longer be included by default in Zope 2.14 but live on as an
  independent add-on.

機能追加
+++++++++

- Merged the query plan support from both ``unimr.catalogqueryplan`` and
  ``experimental.catalogqueryplan`` into ZCatalog. On sites with large number of
  objects in a catalog (in the 100000+ range) this can significantly speed up
  catalog queries. A query plan monitors catalog queries and keeps detailed
  statistics about their execution. Currently the plan keeps track of execution
  time, result set length and support for the ILimitedResultIndex per index for
  each query. It uses this information to devise a better query execution plan
  the next time the same query is run. Statistics and the resulting plan are
  continuously updated. The plan is per running Zope process and not persisted.
  You can inspect the plan using the ``Query Plan`` ZMI tab on each catalog
  instance. The representation can be put into a Python module and the Zope
  process be instructed to load this query plan on startup. The location of the
  query plan is specified by providing the dotted name to the query plan
  dictionary in an environment variable called ``ZCATALOGQUERYPLAN``.

- Various optimizations to indexes _apply_index and the catalog's search
  method inspired by experimental.catalogqueryplan.

- Added a new ILimitedResultIndex to Products.PluginIndexes and made most
  built-in indexes compatible with it. This allows indexes to consider the
  already calculated result set inside their own calculations.

- Changed the internals of the DateRangeIndex to always use IITreeSet and do
  an inline migration from IISet. Some datum tend to have large number of
  documents, for example when using default floor or ceiling dates.

- Added a new reporting tab to `Products.ZCatalog` instances. You can use this
  to get an overview of slow catalog queries, as specified by a configurable
  threshold value.

- Warn when App.ImageFile.ImageFile receives a relative path with no prefix,
  and then has to assume the path to be relative to "software home". This
  behaviour is deprecated as packages can be factored out to their own
  distribution, making the "software home" relative path meaningless.

- 配布物の更新:

  - AccessControl = 2.13.2
  - DateTime = 2.12.5
  - DocumentTemplate = 2.13.1
  - Products.BTreeFolder2 = 2.13.1
  - Products.OFSP = 2.13.2
  - ZODB3 = 3.10.0b4

2.13.0a2 (2010-07-13)
---------------------

バグ修正
++++++++

- Made ZPublisher tests compatible with Python 2.7.

- LP #143531: stateアクセスを許可しているbrokenオブジェクトに関する修正
  を行いました。

- LP #578326: browser:view ディレクティブに非公開のpermission属性を設定
  機能を追加しました。

再構築
+++++++

- No longer use HelpSys pages from ``Products.OFSP`` in core Zope 2.

- No longer create an `Extensions` folder in the standard instance skeleton.
  External methods will become entirely optional in Zope 2.14.

- Avoid using the ``Products.PythonScripts.standard`` module inside the
  database manager ZMI.

- Factored out the `Products.BTreeFolder2`, `Products.ExternalMethod`,
  `Products.MIMETools`, `Products.OFSP`, `Products.PythonScripts` and
  `Products.StandardCacheManagers` packages into their own distributions. They
  will no longer be included by default in Zope 2.14 but live on as independent
  add-ons.

- `Products.ZSQLMethods` をZopeとは別の配布物に分離しました。この配布物
  には `Shared.DC.ZRDB` コードも含まれています。このコードは Zope 2.13
  からは同梱されず自動的に使えるようにはなりません。もしこれが必要であれば
  `Products.ZSQLMethods` を明示的に利用するよう設定してください。
  この移行措置としてZope 2.12.9にバックポートしているので、そのZope2を
  使っていればこの配布物への依存設定は行われている状態になっています。

- `Shared` と `Shared.DC` を名前空間パッケージにしました。

- Removed fallback code for old Python versions from
  `ZServer.FTPServer.zope_ftp_channel.push`.

- Removed fallback code for old `ZCatalog.catalog_object` function signatures
  from `Products.ZCatalog.ZCatalog.reindexIndex`.

機能追加
+++++++++

- Python 2.7 を公式サポートに追加しました。

- Added a new API ``get_packages_to_initialize`` to ``OFS.metaconfigure``.
  This replaces any direct access to ``Products._packages_to_initialize``.
  The OFS.Application.install_package function takes care of removing entries
  from this list now.

- Added notification of ``IDatabaseOpenedWithRoot``.

- Added a new API's ``get_registered_packages, set_registered_packages`` to
  ``OFS.metaconfigure`` which replace any direct access to
  ``Products._registered_packages``.

- Changed product install so it won't write persistent changes only to abort
  them. Instead we don't make any database changes in the first place.

- Disabled persistent product installation in the default test configuration.

- Directly extend and use the Zope Toolkit KGS release 1.0a2 from
  http://download.zope.org/zopetoolkit/index/.

- 配布物の更新:

  - DateTime = 2.12.4
  - nt_svcutils = 2.13.0

2.13.0a1 (2010-06-25)
---------------------

このリリースには `Zope 2.12.8 <http://pypi.python.org/pypi/Zope2/2.12.8>`_
リリースに含まれるバグ修正と機能追加が全て含まれています。

配布物の変更
++++++++++++

- Moved AccessControl, DocumentTemplate (incl. TreeDisplay) and
  Products.ZCTextIndex to their own distributions. This removes the last direct
  C extensions from the Zope2 distribution.

- Moved the ``zExceptions`` package into its own distribution.

- Drop the dependency on the ThreadLock distribution, by using Python's thread
  module instead.

- Integrated the Products.signalstack / z3c.deadlockdebugger packages. You can
  now send a SIGUSR1 signal to a Zope process and get a stack trace of all
  threads printed out on the console. This works even if all threads are stuck.

インスタンススケルトン
++++++++++++++++++++++

- Changed the default for ``enable-product-installation`` to off. This matches
  the default behavior of buildout installs via plone.recipe.zope2instance.
  Disabling the persistent product installation also disabled the ZMI help
  system.

.. - Removed Zope2's own mkzeoinstance script. If you want to set up ZEO instances
..   please install the zope.mkzeoinstance and use its script.

- Zope2 自身がもっていた mkzeoinstance スクリプトを削除しました。もし、
  ZEOインスタンスを作成したい場合は zope.mkzeoinstance パッケージをインストール
  してそのスクリプトを使って下さい。

- Removed deprecated ``read-only-database`` option from zope.conf.

- LP #143232: Added option to 'zope.conf' to specify an additional directory to
  be searched for 'App.Extensions' lookups. Thanks to Rodrigo Senra for the
  patch.

- LP #143604: Removed top-level database-quota-size from zope.conf, some
  storages support a quota option instead.

- LP #143089: Removed the top-level zeo-client-name option from zope.conf, as it
  had no effect since ZODB 3.2.

- Removed no longer maintained ``configure, make, make install`` related
  installation files. Zope2 can only be installed via its setup.py.

- Removed the unmaintained and no longer functioning ZopeTutorialExamples from
  the instance skeleton.

非推奨と削除
++++++++++++

- Finished the move of five.formlib to an extra package and removed it from Zope
  2 itself. Upgrade notes have been added to the news section of the release
  notes.

- ZPublisher: Removed 'Main' and 'Zope' wrappers for Test.publish. If anybody
  really used them, he can easily use ZPublisher.test instead. In the long run
  ZPublisher.test and ZPublisher.Test might also be removed.

- ZPublisherExceptionHook: Removed ancient backwards compatibility code.
  Customized raise_standardErrorMessage methods have to implement the signature
  introduced in Zope 2.6.

- Removed ancient App.HotFixes module.

- Removed the deprecated ``hasRole`` method from user objects.

- Removed deprecated support for specifying ``__ac_permissions__``,
  ``meta_types`` and ``methods`` in a product's ``__init__``.

- Remove remaining support classes for defining permissions TTW.

- Removed the deprecated ``five:containerEvents`` directive, which had been a
  no-op for quite a while.

- Removed Products.Five.fivedirectives.IBridgeDirective - a leftover from the
  Interface to zope.interface bridging code.

- Marked the ``<five:implements />`` as officially deprecated. The standard
  ``<class />`` directive allows the same.

リファクタリング
++++++++++++++++

- Completely refactored ``ZPublisher.WSGIResponse`` in order to provide
  non-broken support for running Zope under arbitrary WSGI servers. In this
  (alternate) scenario, transaction handling, request retry, error handling,
  etc. are removed from the publisher, and become the responsibility of
  middleware.

- Moved the code handling ZCML loading into the ``Zope2.App`` package. The
  component architecture is now setup before the application object is created
  or any database connections are opened. So far the CA was setup somewhat
  randomly in the startup process, when the ``Five`` product was initialized.

- Moved Products.Sessions APIs from ``SessionInterfaces`` to ``interfaces``,
  leaving behind the old module / names for backward compatibility.

- Centralize interfaces defined in Products.ZCTextIndex, leaving BBB imports
  behind in old locations.

- Moved ``cmf.*`` permissions into Products.CMFCore.

- Moved ``TaintedString`` into the new AccessControl.tainted module.

- Testing: Functional.publish now uses the real publish_module function instead
  of that from ZPublisher.Test. The 'extra' argument of the publish method is no
  longer supported.

- Moved ``testbrowser`` module into the Testing package.

- Moved general OFS related ZCML directives from Products.Five into the OFS
  package.

- Moved the ``absoluteurl`` views into the OFS package.

- Moved ``Products/Five/event.zcml`` into the OFS package.

- Moved ``Products/Five/security.py`` and security related ZCML configuration
  into the AccessControl package.

- Moved ``Products/Five/traversing.zcml`` directly into the configure.zcml.

- Moved ``Products/Five/i18n.zcml`` into the ZPublisher package.

- Moved ``Products/Five/publisher.zcml`` into the ZPublisher package.

- Ported the lazy expression into zope.tales and require a new version of it.

その他全般
++++++++++

- Updated copyright and license information to conform with repository policy.

- LP #143410: Removed unnecessary color definition in ZMI CSS.

- LP #374810: ``__bobo_traverse__`` implementation can raise
  ``ZPublisher.interfaces.UseTraversalDefault`` to indicate that there is no
  special casing for the given name and that standard traversal logic should
  be applied.

- LP #142464: Make undo log easier to read. Thanks to Toby Dickinson for the
  patch.

- LP #142401: Added a link in the ZMI tree pane to make the tree state
  persistent. Thanks to Lalo Martins for the patch.

- LP #142502: Added a knob to the Debug control panel for resetting profile
  data. Thanks to Vladimir Patukhov for the patch.

- ZCTextIndex query parser treats fullwidth space characters defined in Unicode
  as valid white space.

配布物の更新
++++++++++++++

- Jinja2 = 2.5.0
- RestrictedPython = 3.6.0a1
- Sphinx = 1.0b2
- transaction = 1.1.0
- ZConfig = 2.8.0
- ZODB3 = 3.10.0b1
- zope.annotation = 3.5.0
- zope.broken = 3.6.0
- zope.browsermenu = 3.9.0
- zope.browserpage = 3.12.2
- zope.browserresource = 3.10.3
- zope.component = 3.9.4
- zope.configuration = 3.7.2
- zope.container = 3.11.1
- zope.contentprovider = 3.7.2
- zope.contenttype = 3.5.1
- zope.event = 3.5.0-1
- zope.exceptions = 3.6.0
- zope.filerepresentation = 3.6.0
- zope.i18nmessageid = 3.5.0
- zope.interface = 3.6.1
- zope.location = 3.9.0
- zope.lifecycleevent = 3.6.0
- zope.ptresource = 3.9.0
- zope.publisher = 3.12.3
- zope.schema = 3.6.4
- zope.sendmail = 3.7.2
- zope.site = 3.9.1
- zope.structuredtext = 3.5.0
- zope.tales = 3.5.1
- zope.testbrowser = 3.9.0
- zope.testing = 3.9.3
- zope.traversing = 3.12.1
- zope.viewlet = 3.7.2

バグ修正
++++++++

- LP #143391: Protect against missing acl_users.hasUsers on quick start page.

.. rubric:: (Translated by Shimizukawa, `r118225 <http://svn.zope.org/Zope/branches/2.13/doc/CHANGES.rst?rev=118225&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/CHANGES.html>`_)
  :class: translator

