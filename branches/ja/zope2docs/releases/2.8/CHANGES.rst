Zope 2.8 Changes
==================

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については
`HISTORY.txt <http://svn.zope.org/Zope/branches/Zope-2_8-branch/doc/HISTORY.txt?view=markup>`_
を参照してください。


Zope 2.8.11 (2009/08/06)
--------------------------

バグ修正
+++++++++

- Launchpad #373299: OFS.CopySupport でStringで例外を投げていた問題を修正。

- ZEOストレージサーバーに影響する ZEO のネットワークプロトコルの脆弱性を修正。

Zope 2.8.10 (2008/10/24)
--------------------------

バグ修正
+++++++++

- HTTP レスポンスヘッダに HTTP 仕様 (RFC2616) 違反となる CRLF ペアを
  入れないことを保証するようにした。

- `AccessControl.ZopeGuards.guarded_import` 関数は多くの Unauthorized
  例外を ImportError に置き換えていた。そんなことをするな!
  また、ミュータブルなデフォルト引数を削除し、テストを強化した。

- LP #281156: `AccessControl.SecurityInfo.secureModule` がインポートに
  失敗したモジュールの ModuleSecurity を除くため、後で同じモジュールの
  インポートを試みた際に不明瞭なエラーとなる問題を修正。

- LP #142667: プロダクト自動リフレッシュ問題の修正のため、 ZODB-3.4.5
  に更新。

- Collector #2318: zope.conf で zopectl のコントロールソケットを上書き
  できるようにした。

- Hotfix-2008-08-12 を組み込んだ。

Zope 2.8.9 (2006/03/25)
--------------------------

バグ修正
+++++++++

- Collector #2294: Protected DOS-able ControlPanel methods with the
  same 'requestmethod' wrapper.

- Collector #2294: 様々なセキュリティー上リスクのあるアクセスを、
  新しく追加したデコレータで防御した。デコレータはPOSTリクエストでのみ
  アクセスを許可する。これは Zope 2.11 の requestmethod decorator factory
  をバックポートした。

- Collector #2263: `field2ulines` が空文字列を正しく変換していなかった。

- Collector #2237: `make` のメッセージで、 `make instance` する前に
  `make inplace` するように表示していなかった問題を修正。

- Collector #2235: いくつかの ZCatalog メソッドがオブジェクトのブール
  で評価していたため、 None ではなく __len__ で評価されていた。
  いくつかの `if not obj` を `if obj is None` に置き換えた。 

- Fix yet another resTructuredText glitch, and add tests (test
  backported from 2.9, which was not in fact vulnerable).

- Collector #2157: Expose name of broken class in SystemError raised
  from '__getstate__' of a broken instance.

Zope 2.8.8 (2006/07/14)
--------------------------

バグ修正
+++++++++

- OFS Application: Zope 2.8.5 で追加された Deprecation Warning
  (今後廃止されるコード利用の警告) が出ないようにした。
  警告はZope 2.9.0.から開始する。

- Collector #2136: ResourceLockedError の response 値を正しい値に修正。

- Collector #2016: DemoStorage が `_old` 属性を持たずに base storages
  をラップ出来ない問題を ZODB 3.4.4 で修正。

- Collector #2116: sequence.sort() が locale 関連の比較方法でうまく動作しない。

- Collector #2077: ACTUAL_URL と SiteRoot の問題を修正。

- Collector #2073: OFS.Owned.changeOwnership の間違った挙動を修正。

- Collector #1944: HTTPRequest.resolve_url にあるエラーの問題

- Collector #2063: MailHost.sendTemplate() のコードを色々とクリーンアップ。

その他の変更
+++++++++++++

- ZopeX3 バージョンを 3.0.2 に上げました。

- ZODB のバージョンを 3.4.4 に上げました。

- Disabled docutils / ReST file inclusion completely, rather than
  trying to jigger it via configuration settings.

Zope 2.8.7 (2006/05/29)
--------------------------

バグ修正
+++++++++

- Collector #1429: ZSQL method において、 name/value による traversal
  (URL解決) の問題を修正 (backport from 2.9).

- Collector #1447: VirtualHost 化されたZopeでコンテンツ編集時に
  AcceleratedHTTPCacheManager が 正しいURLを取り除く問題。

- Collector #2072: __bobo_traverse__ の制限を超えるセキュリティーの
  問題を修正するパッチを適用しテストを書いた。

- XMLエクスポート/インポートに関する問題を修正するための Yoshinori Okuji
  氏のパッチを適用した。

- Collector #2037: VHM 経由でルートにアクセスしたとき ACTUAL_URL が
  `http://www.example.com//` のようになっていた問題

- Collector #2039: パスワードにコロンが含まれていた場合の
  ZPublisher.HTTPRequest.HTTPRequest._authUserPW が詰まる問題。

- webdav.Resource モジュールが NotFound をインポートしていなかった。

- OFS.Image: 'Image.update_data' が Etag を更新しない。

- Collector #940: PageTemplateFile: python expressions に含まれる、
  改行コードが混在したファイルを開いた際の問題をサポート。

- OFS.PropertyManager: 不足していたセキュリティー設定を追加。

その他の変更
+++++++++++++

- Updated to ZODB 3.4.3.

Zope 2.8.6 (2006/02/25)
--------------------------

バグ修正
+++++++++

- Collector #1819: 次のシグネチャを修正:
  MountedObject.SimpleTrailblazer._construct(

- ZPublisher.BaseRequest: The publisher would happily publish attributes
  of type 'bool' and 'complex', as well as Python 2.4's 'set' and
  'frozenset'.

- Collector #1991: ZPublisherが%20で終わっているURLを適切に対処
  していませんでした。

- Collector #2013: エラーメッセージのXHTML適応度を高めました。
  いくつかの<p>タグが閉じていませんでした。

- Collector #1999: FTPにおける名前変更機能の不具合を修正しました。
  (RNFRはステータスコード250ではなく350を返すようになりました)

- Collector #2002: ls -R 機能の不具合を修正しました。
  (OFSフォルダのサブクラスで再起処理が行われていませんでした)

Zope 2.8.5 (2005/12/19)
--------------------------

バグ修正
+++++++++


- Collector #1939: When running as a service, Zope could
  potentially collect too much log output filling the NT Event
  Log. When that happened, a 'print' during exception handling
  would cause an IOError in the restart code causing the service
  not to restart automatically.

  Problem is that a service/pythonw.exe process *always* has an
  invalid sys.stdout.  But due to the magic of buffering, small
  "print" statements would not fail - but once the file actually
  got written to, the error happened.  Never a problem when
  debugging, as the process has a console, and hence a valid
  stdout.

- Collector #1976: FTP STOR command would load the file being
  uploaded in memory. Changed to use a TemporaryFile.

- Collector #1904: On Mac OS X avoid a spurious OSError when
  zopectl exits.

- CopySupport: Reverted workaround in '_verifyObjectPaste'.
  'checkPermission' now respects proxy roles, so the warkaround
  introduced to fix http://www.zope.org/Collectors/Zope/78 is no longer
  needed. Meta types listed in all_meta_types need a 'permission' key.

- Collector #1774:  Harmonize the implementation of
  AccessControl.ZopeSecurityPolicy.checkPermission
  with 'validate', checking ownership and proxy roles if an
  executable object is on the stack.

- AccessControl.SecurityInfo: Fixed problem with
  setPermissionDefault when the permission wasn't used anywhere
  else in the class to protect methods.

- Collector #1957:  Made ZPublisher.HTTPResponse._error_html
  return conformant XHTML.

- Collector #1891:  Backported changes to ZCatalog regression
  tests, removing use of 'whrandom' (and its 'seed' function).

- Collector #1621, #1894:  Added BBB alias for 'whrandom'
  in AccessControl/DTML.py and RestrictedPython/Utilities.py.  The
  alias will be removed in Zope 2.10.

- Collector #1954: DocumentTemplate.DT_String:  remove non-XHTML
  wart from error message.

- Fixed unclear security declarations. Warn when an attempt is
  made to have a security declaration on a nonexistent method.

- OFS Application: While deprecated since years, old-style product
  metadata in the __init__.py did not show deprecation warnings. Added
  warnings and converted ZGadflyDA/__init__.py and
  ZSQLMethods/__init__.py to use registerClass instead.

その他の変更
+++++++++++++

- The SiteErrorLog now copies exceptions to the event log by default.

- ObjectManager now has an hasObject method to test presence. This
  brings it in line with BTreeFolder.

- Made 'zopectl test' work for software homes which do not have
  an "inplace" build (it used to require that test.py be in
  $ZOPE_HOME/bin/;  now it will use $ZOPE_HOME as a fallback).

- Improved logging of ConflictErrors. All conflict errors are
  logged at INFO, with counts of how many occurred and how many
  were resolved. Tracebacks for all conflicts are logged a DEBUG
  level, although these won't help anyone much. If a conflict
  error is unresolved, it will now bubble up to error_log and
  standard_error_message.

- Suppressed output of deprecation warning in 'filepath' test
  for PythonScripts.

Zope 2.8.4 (2005/10/26)
--------------------------

バグ修正
+++++++++

- Collector #1927:  Modified ZReST not to store rendered / warnings
  as persistent attributes, using volatile attributes instead as
  a cache.

- Collector #1926: fixed a typo in _doAddUser when password
  encryption is enabled.

- If a content object implemented any in-place numeric operators, 
  untrusted code could call them, thus modifying the content.

- If Python 2.4 is used, despite the fact that Python 2.4 is
  unsupported, untrusted code could use generator expressions to
  gain access to container items.

Zope 2.8.3 (2005/10/18)
--------------------------

その他の変更
+++++++++++++

- ZSQLMethod.manage_main: Moved the error message that warns of a 
  non-existing or closed database connection next to the Connection ID
  dropdown and present it using red to increase its visibility.

- Update to Docutils 0.3.9 (forgotten in Zope 2.8.2)

Zope 2.8.2 (2005/10/13)
--------------------------

機能追加
+++++++++

- Collector #1118: Added syntax to dtml-sqlgroup to support flexible
  generation of 'UPDATE' statements (bounty sponsored by Logicalware).

- The '@' character is now allowed in object ids (RFC 1738 allows it).

バグ修正
+++++++++

- OFS.Image.manage_FTPget() would str() it's .data attribute,
  potentially loading the whole file in memory as a
  string. Changed to use RESPONSE.write() iterating through the
  Pdata chain, just like index_html().

- When PageTemplates have a syntax error, show the traceback output
  in the rendered error message.

- Collector #1914: Hardened 'call_with_ns' (in
  'Products.PageTemplates.ZRPythonExpr') against namespaces from other
  callers than page templates.

- Collector #1490: Added a new zope.conf option to control the
  character set used to encode unicode data that reaches
  ZPublisher without any specified encoding.

- Collector #1895: omit 'var' folder from recursive traversal causing
  trouble with DirectoryStorage.

- disabled ".. include" directive for all the ZReST product and the
  reStructuredText package

- Collector #1888: Adjust call to 'engine.translate' to accomodate
  change in its signature.

- Collector #1863: Prevent possibly sensitive information to leak via
  the TransientObject's __repr__ method.

- Repaired 'handle_errors' usage for doctests, along with the
  supporting 'debug' argument passed to
  'ZPublisher.Test.publish_module'.

- Collector #1879: applied patch by Dieter Maurer to fix a bug in
  ac_aquire() ignoring the default argument

- Collector #1864, #1906: fixed header normalization in appendHeader()

- Collector #1899: fixed migration issue when using export/import for
  ZCatalog instances

- 'ZPublisher.Test.publish' now takes a 'done_string' argument, which
  is written to standard error when the request completes (forward
  ported from Zope 2.7 branch).

- Collector #556:  <dtml-sqlvar> now returns 'null' instead of 'None'
  for values which are None in Python (sponsored by a bounty from
  Logicalware).

- Collector #1182: BBB Forward port fix from 2.7 branch (19 months
  ago!), reverting 'guarded_getitem' to pass the 'index' argument as
  the name to 'validate'.  This change is *not* propagated to the
  trunk, because the resolution of #1182 specifies that the reverted
  behavior (i.e., passing None for item accces) is to become the
  standard implementation as of 2.9.

- Collector #1877: skel/Products/README.txt inappropriately copied
  from CMF.

- Collector #1871: Applied patch to support lists with records using
  ZTUtils.make_query()

- AccessControl: creating a new user through "zpasswd inituser" did not
  work properly with a top-level user folder with enabled password
  encryption.

- ZCatalog: refreshCatalog() could not be called safely from a ZEO
  client script

- Catalog.clear(): fixed handling of _length attribute (caused import
  problems for some .zexp files e.g. Squishdot instances)

- DateIndex now properly removes documents from both indexes if
  the value is None

- Collector #1888: Some parts of the TALInterpreter would not pass a
  default when  translating, yet expect a string back. This would cause
  an error (usually "NoneType has no attribute 'replace'") in the case
  the message was not translated.

Zope 2.8.1 (2005/08/11)
--------------------------

機能追加
+++++++++

- Interface: Added Z3 -> Z2 bridge utilities.
  This allows to migrate interfaces to Zope 3 style interfaces and
  bridge them back to oldstyle interfaces for backwards compatibility.

バグ修正
+++++++++

- Zope2.Startup.zopectl: fork before execv when running unit tests
  (don't exit the shell, if run from there).

- TAL: MassageIDs are now handled the same way as in zope.tal.

- DocumentTemplate: ustr no longer mangles MassageIDs.
  Custom string types are now returned unchanged.

- As developed in a long thread starting at
  http://mail.zope.org/pipermail/zope/2005-July/160433.html
  there appears to be a race bug in the Microsoft Windows socket
  implementation, rarely visible in ZEO and/or in
  ZServer/medusa/thread/select_trigger.py when multiple processes try
  to create an "asyncore trigger" simultaneously, most often (in
  stress tests) manifesting as a hung process.  Windows-specific
  trigger code in both changed to work around this bug when it occurs.

- Collector #1807: fixed memory leak in cAccessControl.guarded_getattr()


Zope 2.8.1 b1 (2005/07/28)
--------------------------

機能追加
+++++++++

- PluginIndexes, ZCTextIndex and ZCatalog: Added some z3 interfaces.

- Verbose security exception reporting has been folded into Zope,
  removing the need for the VerboseSecurity product.  See the
  documentation for the "verbose-security" option in zope.conf.

- "TemporaryStorage" (the storage that is used mainly to back the
  default sessioning database) is now MVCC capable, which essentially
  means that its usage will no longer generate ZODB ReadConflictErrors.

バグ修正
+++++++++

- Collector #1852: fixed wrong URL construction in webdav.davcmds

- Collector #1844: fixed whitespace handling in the ZMI "Find" tab

- Collector #1813: removed spurious inclusion of CMFBTreeFolder.
  in Products/BTreeFolder2 (CMFCore will include it after 1.5, with
  an appropriate module alias for backward compatibility).

- Replaced all transaction.commit(1) calls by  transaction.savepoint()

- Collector #1832: UnIndex swallowed ConflictErrors.

- Collector #1815: ZCTextIndex accepts (again) sequences of strings to
  be indexed.

- Collector #1812: Fixed key error in ZSQL ZMI/Test

- Fixed CMFBTreeFolder for CMF 1.5+

- WebDAV COPY and MOVE did not call '_notifyOfCopyTo' and '_postCopy'
  hooks like it was done in OFS.CopySupport. Additionally added
  'manage_changeOwnershipType' to make MOVE behave even closer to
  OFS.CopySupport.

- Collector #1548: Fix 'httplib' usage in ZPublisher.Client.

- Collector #1808: manage_convertIndexes no longer tries to change the
  index types causing some trouble with CMF.

- manage_convertIndexes did not treat DateRangeIndexes and PathIndexes
  properly.

- Updated Zope X3 to bugfix release 3.0.1

- Updated Five to bugfix release 1.0.2 (see Products/Five/CHANGES.txt)

Zope 2.8.0 (2005/06/11)
--------------------------

バグ修正
+++++++++

- Collector #1792: applied patch for broken ZClasses

- doc/FAQ.txt updated: should bear some resemblance to reality now.
  (PCGI stuff removed; error information updated; PID information
  updated; upgrade procedure added; some common version questions added.)

- Collector #1770: Fixed RestructuredText subtitle

- Collector #1803: Fixed InitializeClass for some corner case.

- Collector #1798, issue 1: ZopeTestCase no longer tries to
  install products that were installed by Zope during startup.

- Collector #1799: Avoid lying about parent's refcount when
  calling back into Python code.

- Collector #889:  made 'and' operator for KeywordIndexes actually
  restrict results as expected (thanks to 'aroda' for the patch!).

- Collector #1323: applied patch to fix umask problem in zdctl

- Updated Five to bugfix release 1.0.1 (see Products/Five/CHANGES.txt)

Zope 2.8.0 b2 (2005/05/22)
--------------------------

機能追加
+++++++++

- Made WebDAV server distinguishable from the default HTTP
  server both in the ZMI and in event.log.

- Made Acquisition and derived classes play inside Python's acyclic
  garbage collection framework.

- Included BTreeFolder2

バグ修正
+++++++++

- Collector #1507/1728: Server addresses are now handled the same way on
  all platforms. This fixes the default binding on Windows.

- Collector #1781: made 'create_mount_points' ZConfig option actually
  work (thanks to Dieter Maurer for the patch).

- Collector #1780: DateTime.strftime() now handles dates <= 1900 or
  >= 2038

- Collector #1775: turning off debug mode by default

- Collector #1784: fixed handling of multiple attributes in ZCTextIndex

- Don't copy '.svn' directories from skeleton into an instance
  (thanks to Dale Hirt for the patch).

- Collector #1776: Improved setup.py.
  The Finder class is now used for the complete lib/python tree and has
  a blacklist instead of a whitelist for file extensions. So there
  should no longer be a need to update setup.py if modules or files are
  added or removed in lib/python.

- Collector #1751: Improved error reporting reporting during the
  startup phase

- Collector #1745: Fixed ZSQL error KeyError 'query'

- Collector #1735: fixed UnicodeDecodeError in Loader.py

Zope 2.8b1 (2005/04/24)
--------------------------

機能追加
+++++++++

- Added lazy: TAL expression and fixed defer: expression for python
  expression

- ZCatalog.CatalogBrains: An _unrestrictedGetObject method has
  been added.

- ZODB transactions now support savepoints. See
  transaction/savepoint.txt.  These will replace
  subtransactions.

バグ修正
+++++++++

- Collector #1754: Fixed import of 'transaction' in
  'zopectl adduser' (which wasy dying with a NameError).

- Collector #1750: StructuredText: fixed handling of image URLs
  with query string

- Collector #1748: Fixed SIGSEGV in Acquisition

- Hotfix_20050405:  classes defined in untrusted code could shadow
  the roles of methods defined as protected by their bases.

- Collector #1656: Fixed enumeration within untrusted code
  (forward-port from 2.7 branch).

- Collector #1721: Fixed handling of an empty indexed_attrs parameter


Zope 2.8a2 (2005/04/02)
--------------------------

機能追加
+++++++++

- ZCatalog.CatalogBrains:  'getObject' now raises errors, rather than
  returning None, in cases where the path points either to a nonexistent
  object (in which case it raises NotFound) or to one which the user
  cannot access (raising Unauthorized).  Sites which rely on the old
  behavior can restore setting a new zope.conf option,
  'catalog-getObject-raises', to "off".

  This compatibility option will be removed in Zope 2.10.

- PluginIndexes: the ZCatalog's "Indexes" tab now show the number of
  distinct values indexed by each index instead of a mixture of indexed
  objects versus number of distinct values. Indexes derived from UnIndex
  show both values within their own ZMI screen. In addition most indexes
  have now a "Browse" tab to browse through the list of indexed
  values and their occurrences.

- FTPServer: a RNFR (rename from) request is now being responded
  with a 550 error code if the source file does not exist

- Fixed ObjectManager to not swallow exceptions during object
  deletion (in debug mode and if the user is not Manager). This
  allows for better debugging, while still keeping the possibility
  for a Manager to delete buggy objects.

- Added a ZConfig directive 'large-file-threshold' to control
  the request content-size threshold at which a temporary file
  gets created. Use the same value for deciding between reading
  the whole request in memory or just a chunk inside
  webdav.NullResource.PUT().

- RAMCacheManager: Allow invalidation of a cache entry from the
  Statistics view in the ZMI

- Collector #1454/OFS.File: Accept content types ending with
  "javascript" as editable through the File edit form, just like
  text/<foo> types

- Zope X3 3.0.0's 'src/zope' package is included now.

- Five (Zope 3 integration technology for Zope 2) is included
  now in Products/Five.

バグ修正
+++++++++

- Collector #1460: guarded_apply was too restrictive.

- OFS.Traversable still used a string 'NotFound' exception.

- ZPublisher would fail to recognize a XML-RPC request if the
  content-type header included a 'charset' parameter.

- Forward-ported 'aq_acquire'-related fix and associated tests
  from Zope 2.7.4.

- Collector #1730: XML page templates couldn't call aq_parent in
  path expressions.

- Fixed brain.getObject() to correctly traverse to an object even
  if one of its parents is not accessible, to be close to what the
  Publisher does.

- Forward ported fix for OFS.CopySupport tests which corrected
  signature of a faux security policy's 'validate' method.

- 'setup.py' did not install the 'Zope' compatibility module
  (the old 'Zope' package has been renamed to 'Zope2').

- Fixed Shared.DC.ZRDB.Results to behave with the new-style
  ExtensionClass. Added a test.

- 'setup.py' did not install the new 'Zope' compatibility module
  (the 'Zope' package has been renamedd to 'Zope2').

- Collector #1507: Zope now binds again to all available IP addresses if
  ip-address is unset

- Use 'del' instead of 'list.remove()' in
  Catalog.delColumn(). There can be only one column with the
  same name, and it could potentially break catalog metadata as
  remove() may remove more than one element from the list if
  they have the same value. Also, we already have the list index
  we are interested in deleting so it doesn't make sense to look
  up the value and call 'list.remove()' on it.

- Collector #1628: FTP server has been broken (directory
  listings did not work)

- Collector #1705: CopySource._postCopy is never called

- Collector #1617: Fixed crash in ZPT code (caused by improper
  checks in cAccessControl)

- Collector #1683: fixing batching in the DA "Test" tab

- Collector #1648: Fix bug in Medusa FTP

- Collector #1667: allow 'max-number-of-session-objects 0' to have
  the same effect as setting the value via the web interface (i.e.,
  make the number of session objects unlimited, rather than falling
  back to the default).

- Collector: #1651: removed compiler warning

- Collector #1661: make 'python-check-interval' setting in zope.conf
  actually work as documented.  This setting allows for important
  tuning opportunities for production Zope servers.

- Collector #1657:  Don't break host-based virtual hosting when
  purging an HTTP accelerator.

- DTML Methods were not interoperable with the new filestream_iterator
  and caches based on it (FileCacheManager).

- Collector #1655: fixed severe memory leak in TemporaryStorage

- Collector #1407: fixed XML escaping problem introduced in 2.7.4 b1

- Collector #1151: HTTP compression was broken on error pages

- The REQUEST now contains a new entry ACTUAL_URL which contains the
  full URL without query string as it appears within the location bar of
  the browser. The key has been added to provide a single key that is
  available for vhosted and non-vhosted installations.

- Collector #1605: VHM did not quote URLs

- webdav.Resource: during COPY, manage_afterClone was called way
  too early, thus the object wasn't bound to the database and
  couldn't find a context. Changed to behave the same way as
  CopySupport.

- RAMCacheManager: opimized performance by using cPickle instead
  of pickle and by using the highest pickle protocol available
  instead of using ASCII pickles (patch by Dieter Maurer)

- Collector #631: Image URLs in StructuredText containing port
  numbers were not rendered correctly

- Collector #1498: Don't choke on malformed cookies. Cookies of
  the form "foo=bar; hmm; baz=gee" will give an empty value for
  'hmm' instead of silently discarding it and the rest of the
  string. (Thanks to 'sirilyan' for the patch.)

- bin/zopectl test now uses os.execv, instead os os.system,
  so that options with characters that needs shell quoting
  doesn't break the command.

- Collector #1219:  Make XML export sane again.

- Collector #945:  Allow adding empty PythonScript instances
  programmatically.

- Updated doc/UNITTEST.txt and lib/python/Testing/README.txt to
  reflect progress made since UNITTEST.txt was originally written.

- Removed Version objects from the add menu. Versions are agreed to be a
  feature that should not be used as it is not well implemented and
  allows for data loss.

- Collector #1510: Allow encoding of application/xhtml+xml pages
  according to the charset specified in the Content-Type header
  (thanks to Jacek Konieczny for the patch).

- Collector #1599: made sqltest work with unicode strings (thanks
  to Peter Sabaini for the patch).

- zopectl: fixed handling of child processes (patch by Dieter Maurer)

- Collector #1593: fixed dumb _get_id() implementation in
  OFS.CopySupport that produced copy_of_copy_of....files (thanks
  to Alexandre Boeglin for the patch).

- Collector #1450: files in utilities/ZODBTools are now installed
  during the installation process in the 'bin' directory

- Collector #1003: added new 'http-header-max-length' directive
  to zope.conf to specific the maximum length of a HTTP request
  header before it is considered as a possible DoS attack and
  discarded.

- Collector #1371: added new 'cgi-maxlen' directive to zope.conf
  to limit the amount of form data being processed by Zope
  to prevent DoS attacks

- Collector #1407: changed WebDAV display name for objects
  to title_or_id()

- the 'trusted-proxy' directive in zope.conf now also accepts
  hostnames instead of IP addresses only (patch by Dieter Maurer)

- Fixed test.py to not over-resolve symbolic links. Needed to run
  tests when the Products directory and a product are symlinks.

- Collector #1583/ZReST: Fixed handling of the title attribute
  for non-ascii characters.

- Collector #1577: Fixed cryptic error message in ZPublisher if a
  non-ASCII string is passed to a date, int, long or float property.

- Collector #1576: Fixed Z Search Interface to use proper HTML.

- Collector #1127: strftime did not take timezone into account.

- Collector #1569/DateTime: Added a new ISO8601-method that will
  return correctly formatted ISO 8601-representations to augment
  the ISO method which isn't compliant with ISO 8601.

- ZPublisher: changed some hardcoded 'latin1' arguments to 'iso-8859-15'
  since latin1 is obsolete.

- Collector #1566: Installation of Zope on some older Solaris versions
  could fail due to a broken "echo" implementation causing the
  creation of a borked version.txt file.

- Collector #934: Image and File objects are now always internally
  split into small chunks even when initialized from a string.

- docutils: updated to V 0.3.5. The Zope core now contains a full copy of
  the docutils package except some GPLed files which can not be included
  with the Zope distribution due to license constraints on svn.zope.org.

- docutils: moved from lib/python/docutils to
  lib/python/third_party/docutils

- Collector #1557/OFS.Image: Introducing new 'alt' property. The 'alt'
  attribute is no longer taken from the 'title' property but from the new
  'alt' property.  The border="0" attribute is no longer part of the HTML
  output except specified otherwise.

- Set a default value of '' for the new 'alt' property as not to
  break existing content.

- Collector #1511: made IPCServer show up in the Control Panel under
  "Network Services"

- Collector #1443: Applied patch by Simon Eisenmann that reimplements
  the XML parser used in WebDAV fixing a memory leak.

- Always unescape element contents on webdav.xmltools

- Use saxutils to escape/unescape values for/from
  PROPFIND/PROPPATCH.

- Make OFS.PropertySheet use the escaping function from
  webdav.xmltools.

- Escape/unescape &quot; and &apos;

- Don't escape properties stored as XML (ie: having a
  __xml_attrs__ metadata set by PROPPATCH) when building a
  PROPFIND response.

- If a PROPPATCH element value contains only a CDATA section,
  store the CDATA contents only.

- Catch AttributeErrors and KeyErrors raised from
  __bobo_traverse__ and convert them to NotFound. In debug mode
  a more verbose error message is issued, the same way it's done
  on attribute/item traversal.

- Collector #1523: replace the text field for importing .zexp/.xml
  files with a selection list

- Stitch newly-created object into it's container *before*
  calling it's PUT() method. This fixes an issue with
  OFS.File/OFS.Image that would result into reading the whole
  file in memory and wrapping it into a *single* Pdata object.

- Import ZServer.CONNECTION_LIMIT variable *inside* the method
  that uses it. Before this, the variable was imported at the
  module level, thus binding it too early which would cause the
  ZConfig handler to have no real effect.

Zope 2.8a1 (2004/10/17)
--------------------------

機能追加
+++++++++

- Included Stefan Holek's ZopeTestCase 0.9

- The SiteErrorLog allows you to acknowledge (or delete) exceptions,
  so you can reduce or clear the list without restarting your
  Zope server. Additionally the SiteErrorLog is covered by unit tests
  now.

- Unit tests added for the SiteErrorLog.

- UI improvement for the ZCatalog. The "catalog contents" allow
  you to filter the cataloged objects by path now.

- Made test.py follow symbolic links on POSIX systems.

- added utilities/reindex_catalog.py to perform ZCatalog maintenance
  operations from the command line (through zopectl)

- RESPONSE.setBody and RESPONSE.setStatus now accept lock
  parameters in the same way as RESPONSE.redirect. These prevent
  further calls to the methods from overwriting the previous value.
  This is useful when writing http proxies.

- DateTime: new DateTime instance can be constructed from a given
  DateTime instance: d_new = DateTime(d_old)

- The DateTime parser now throws a SyntaxError upon any parsing errors.

- ZCatalog: added a new configuration option in the "Advanced" tab
  to provide optional logging of the progress of long running
  reindexing or recataloging operations.

- made Zope.configure return the starter instance to enable other
  methods to be called, such as starter.setupConfiguredLoggers()

- Improved Unicode handling in Page Templates. Template contents
  and title will now be saved as a Unicode string if
  the management_page_charset variable can be acquired and is true.
  The character set of an uploaded file can now be specified.

- zopectl now accepts the -m argument to set a umask for files created
  by the managed process (e.g. -m 002 or --umask 002).

- AccessControl/permission_settings() now has a new optional parameter
  'permission' to retrieve the permission settings for a particular
  permission.

- The obsolete 'SearchIndex' package has been removed

- Traversal now supports a "post traversal hook" that get's run
  after traversal finished and the security context is established.

- Using "_usage" parameters in a ZCatalog query is deprecated and
  logged as DeprecationWarning.

- MailHost now has two additional properties, a user id and a
  password. These are used to attempt ESMTP authentication
  before sending a mail.

- Folder listings in FTP now include "." as well as "..".

- When a VHM is activated, it adds the mapping
  'VIRTUAL_URL_PARTS': (SERVER_URL, BASEPATH1, virtual_url_path)
  to the request's 'other' dictionary.  If BASEPATH1 is empty, it
  is omitted from the tuple.  The joined parts are also added
  under the key 'VIRTUAL_URL'.  Since the parts are evaluated
  before traversal continues, they will not reflect modifications
  to the path during traversal or by the addition of a default
  method such as 'index_html'.

- Extension Classes, a key Zope foundation, have been totally
  rewritten based on Python new-style classes.

  This change provides a number of advantages:

  - Use of new-style class features (e.g. slots, descriptors,
    etc.) in Zope objects. Support for object protocols (special
    __ methods) added since Python 1.4.

  - Support for cyclic garbage collection.

  - Ability to use new-style classes as base classes of Zope objects.

  - Pave the way for sharing code between Zope 2 and Zope 3.

  Note -- Extension classes with __of__ methods are made into
  Python read descriptors.

  If an extension classes is used to implement a descriptor,
  indirectly by implementing __of__ or directly by implementing
  __get__, the behavior of the descriptor will differ from
  ordinary descriptors in an important way. The descriptors
  __get__ method will be called *even if* the descriptor is
  stored on an instance of an extension class.  Normally
  descritor __get__ methods are called only of the descriptor
  is stored in a class.

- ZODB 3.3

  This is the first version of ZODB that does not require
  ExtensionClass.

- Add 'parity' method to ZTUtils Iterators.

- Allow untrusted code to mutate ZPublisher record objects.

- Added a "mime-types" configuration value which names a file
  giving additional MIME type to filename extension mappings.
  The "mime-types" setting may be given more than once in the
  configuration file; the files have the same format at the
  mime.types file distributed with Apache.

- Changed the ZEO server and control process to work with a
  single configuration file; this is now the default way to
  configure these processes.  (It's still possible to use
  separate configuration files.)  The ZEO configuration file can
  now include a "runner" section used by the control process and
  ignored by the ZEO server process itself.  If present, the
  control process can use the same configuration file.

- ZConfig was updated to version 2.0.  The new version includes
  two new ways to perform schema extension; of particular
  interest in Zope is the ability for a configuration file to
  "import" new schema components to allow 3rd-party components
  (such as storages, databases, or logging handlers) to be used.

- The testrunner.py script has been replaced with test.py which
  is now installed into the 'bin' folder.

バグ修正
+++++++++

- Removed Python 2.3.3 as valid option. ZODB 3.3 requires Python
  2.3.4 or later.

- Collector #1332: Added in-place migration of the Catalog.__len__
  attribute to avoid new-style class caching problems. Instances of
  ZCatalog or instances of classes with ZCatalog as base class will be
  migrated automatically. Instances of Catalog or classes with Catalog
  as base class must be migrated manually by calling the migrate__len__()
  method on the every instance. In addition old BTree migration code
  (for pre-Zope 2.5 instances) has been removed. If you want to migrate
  from such an old version to Zope 2.8, you need to clear and reindex
  your ZCatalog).

- Collector #1595: same as in Collector #1132 for indexes derived from
  UnIndex. Exisiting ZCatalog instances must be converted manually
  by calling the "manage_convertIndexes" method through-the-web for
  every single ZCatalog instance. See also doc/FAQ.txt (Installation,
  question #4)

- Collector #1457: ZCTextIndex's QueryError and ParseError
  are now available for import from untrusted code.

- Collector #1473: zpasswd.py can now accept --username
  without --password

- Collector #1491: talgettext.py did not create a proper header
  for the generated .pot file if multiple pagetemplate files
  were processed.

- Collector #1477: TaintedString.strip() now implements the
  same signature as str.strip()

- TAL: tal:on-error does not trap ConflictError anymore.

- OFS.CopySupport: Enforced "Delete objects" permission during
  move (CMF Collector #259).

- Removed DWIM'y attempt to filter acquired-but-not-aceessible
  results from 'guarded_getattr'.

- Collector #1267: applied patch to fix segmentation faults on
  x86_64 systems

- ZReST: the charset used in the rendered HTML was not set to the
  corresponding output_encoding property of the ZReST instance. In addition
  changing the encodings through the Properties tab did not re-render
  the HTML.

- Collector #1234: an exception triple passed to LOG() was not
  propagated properly to the logging module of Python

- Collector #1441: Removed headers introduced to make Microsoft
  webfolders and office apps happy, since they make a lot of
  standards-compliant things unhappy AND they trick MS Office
  into trying to edit office files stored in Zope via WebDAV even
  when the user isn't allowed to edit them and is only trying to
  download them.

- Collector #1445: Fixed bad interaction between -p and -v(v)
  options to test.py that resulted in exceptions being printed
  when they shouldn't have been.

- Collector #729: manage_main doesn't display the correct page title
  most of the time. It is not completely fixed but using title_or_id
  makes folders display the correct id as a fallback.

- Collector #1370: Fixed html generated by Z Search interface.

- Collector #1295: Fixed minor niglet with the Elvis tutorial.

- added "version.txt" to setup.py to avoid untrue "unreleased version"
  messages within the control panel

- Collector #1436: applied patch to fix a memory leak in
  cAccessControl.

- Collector #1431: fixed NetBSD support in initgroups.c

- Collector #1406: fixed segmentation fault by acquisition

- Collector #1392: ExternalMethod ignored management_page_charset

- unrestrictedTraverse() refactored to remove hasattr calls (which mask
  conflict errors) and for greater readability and maintainability.

- Zope can now be embedded in C/C++ without exceptions being raised
  in zdoptions.

- Collector #1213: Fixed wrong labels of cache parameters

- Collector #1265: Fixed handling of orphans in ZTUtil.Batch

- Collector #1293: missing 'address' parameters within one of the server
  sections raise an exception.

- Collector #1345: AcceleratedHTTPCacheManager now sends the
  Last-Modified header.

- Collector #1126: ZPublisher.Converters.field2lines now using
  splitlines() instead of split('\n').

- Collector #1322: fixed HTML quoting problem with ZSQL methods
  in DA.py

- Collector #1124: The ZReST product now uses the same reST encoding
  parameters from zope.conf as the low-level reStructuredText
  implementation.

- Collector #1259: removed the "uninstall" target from the Makefile
  since the uninstall routine could also remove non-Zope files. Because
  this was to dangerous it has been removed completely.

- Collector #1299: Fixed bug in sequence.sort()

- Collector #1159: Added test for __MACH__ to initgroups.c so the
  initgroups method becomes available on Mac OS X.

- Collector #1004: text,token properties were missing in
  PropertyManager management page.

- Display index name on error message when index can't be used as
  'sort_on'.

- PUT would fail if the created object had a __len__ = 0 (eg:
  BTreeFolder2) and fallback to _default_put_factory. Fix by
  checking if the returned object is None instead.

- Collector #1160: HTTPResponse.expireCookie() potentially didn't
  when an 'expires' keyword argument was passed.

- Collector #1289: Allow ZSQL methods to be edited via WebDAV.

- WebDAV property values were not being properly escaped on
  'propstat'.

- WebDAV 'supportedlock' was not checking if the object did
  implement the WriteLockInterface before returning it's
  value.

- reStructuredText ignored the encoding settings in zope.conf

- ObjectManager no longer raises string exceptions

- Collector #1260: Testing/__init__.py no longer changes the
  INSTANCE_HOME.

- App.config.setConfiguration() did not update the legacy source
  for debug_mode, Globals.DevelopmentMode.

- Script (Python) objects now have a _filepath attribute, also
  used as the '__file__' global at runtime.  This prevents an
  import problem caused by the fix to #1074.

- Minor usability tweaks:

  * Increased FindSupport meta type selection widgets
    height to 8 lines

- The DateTime module did not recognize the settings for
  "datetime-format".

- Stop testrunner.py from recursing into the 'build-base' directory
  created by setup.py.

- Collector #1074: Change Scripts' __name__ to None

- Range searches with KeywordIndexes did not work with record-style
  query parameters

- Item_w__name__ now has a working getId() method

- PageTemplateFile now using Item_w__name__ mixin, fixing
  its getId() and absolute_url() methods.

- Only one VirtualHostMonster is allowed per container.

- Collector #1133: TreeTag choked on Ids of type long.

- Collector #1012: A carefully crafted compressed tree state
  could violate size limit.  Limit is no longer hardcoded.

- Collector #1139: tal:attributes didn't escape double quotes.

- Management interface of TopicIndexes has been completely broken

- Collector #1129: Improper parsing of ISO8601 in DateTime.

- Removed pervasive use of string exceptions (some may still be
  hiding in the woodwork, but all raise's with string literals are
  gone).

- AccessControl.User used a misleading string exeception,
  'NotImplemented', which shadowed the Python builtin.

- Collector #426: Inconsistent, undocumented error() method.

- Collector #799: Eliminate improper uses of SCRIPT_NAME.

- Collector #445: Add internal global declaration for Script bindings.

- Collector #616: Make CONTEXTS available to TALES Python expressions.

- Collector #1074: Give Script execution context a __name__

- Collector #1095: Allow TAL paths starting with '/varname' as a
  preferred spelling for 'CONTEXTS/varname'.

- Collector #391: Cut and paste now requires delete permissions.

- Collector #331: Referenses to URL in manage_tabs was changed
  to REQUEST.URL to prevent accidental overriding.

- Made the control panel properly reflect the cache-size setting
  of ZODB's object cache once again.

- ConflictError was swallowed in ObjectManager by
  manage_beforeDelete and _delObject. This could break code
  expecting to do cleanups before deletion.

- Python 2.3 BooleanType wasn't handled properly by ZTUtils
  marshalling and ZPublisher's converters.

- Collector #1065: bin/ scripts didn't export HOME envars.

- Collector #572: WebDAV GET protected by 'FTP Access' permission.
  Two new methods have been added to WebDAV resources, "manage_DAVget"
  and "listDAVObjects". These are now used by WebDAV instead of the
  earlier "manage_FTPget" and "objectValues". This separates the
  permissions, and allows WebDAV specific overriding of these methods.

- Collector #904: Platform specific signals in zdaemon/Daemon.py
  (fixed by removing the "fossil" module from 2.7 branch and head).

- Workaround for Collector #1081: The 'title' property for objects
  derived from OFS.Folder or PropertyManager can now be
  removed and replaced with a ustring property. This allows the usage
  of non-ISO-8859-1 or ASCII charsets

- Collector #951: DateTime(None) is now equal to DateTime()

- Collector #1056: aq_acquire() ignored the default argument

- Collector #1087: ZPT: "repeat/item/length" did not work as documented
  in the Zope Book.

- Collector #721: Entities in tal:attribute values weren't
  properly escaped.

- Collector #851: Traversable.py: A bare try..except shadowed
  conflict errors

- Collector #1058: Several fixes for PropertySheets when used
  outside ZClasses

- Collector #1053: parseIndexRequest turned empty sequence of search
  terms into unrestricted search.

- manage_tabs had a namespace problem with the acquisition of names from
  the manage_options variable resulting to acquire "target" and "action"
  from objects above in the hierachy.

- PathIndex and TopicIndex are now using a counter for the number
  of indexed objects instead of using a very expensive calculation
  based on the keys of their indexes.

- Collector #1039: Whitespace problem in Z2.log fixed

- changed some bare try: except:'s in Shared.DC.ZRDB.Connection
  so that they now log exceptions that occur.

- ObjectManager will now attempt to set Owner local role keyed
  to the user's id, rather than username.

