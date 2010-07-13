Changelog
=========

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については http://docs.zope.jp/zope2/releases/
を参照してください。

2.13.0a2 (2010-07-13)
---------------------

バグ修正
++++++++

- Made ZPublisher tests compatible with Python 2.7.

- LP #143531: stateアクセスを許可しているbrokenオブジェクトに関する修正
  を行いました。

- LP #578326: browser:view ディレクティブに非公開のpermission属性を設定
  した場合に警告します。この属性はZope2ではサポートされません。

再構築
+++++++

- No longer use HelpSys pages from ``Products.OFSP`` in core Zope 2.

- Register OFS as a package and give it an initialize function. Moved
  registration of OFS classes there from Products.OFSP. ZopeTestCase will no
  longer install the OFSP product automatically, so you might need to change
  your test layer setup to load the OFS configure.zcml and call
  installPackage('OFS').

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

- パッケージ更新:

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

パッケージ更新
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

.. rubric:: (Translated by Shimizukawa, `r114672 <http://svn.zope.org/Zope/branches/2.13/doc/CHANGES.rst?rev=114672&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/CHANGES.html>`_)
  :class: translator

