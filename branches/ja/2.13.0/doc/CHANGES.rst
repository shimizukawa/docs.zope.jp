Changelog
=========

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については http://docs.zope.jp/zope2/releases/
を参照してください。

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

- Integrated zLOG package back into this distribution.

- Integrated the Products.signalstack / z3c.deadlockdebugger packages. You can
  now send a SIGUSR1 signal to a Zope process and get a stack trace of all
  threads printed out on the console. This works even if all threads are stuck.

インスタンススケルトン
++++++++++++++++++++++

- Changed the default for ``enable-product-installation`` to off. This matches
  the default behavior of buildout installs via plone.recipe.zope2instance.

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

.. rubric:: (Translated by Shimizukawa, `r113828 <http://svn.zope.org/Zope/branches/2.13/doc/CHANGES.rst?rev=113828&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/CHANGES.html>`_)
  :class: translator

