Zope 2.9 Changes
==================

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については
`HISTORY.txt <http://svn.zope.org/Zope/branches/2.9/doc/HISTORY.txt?view=markup>`_
を参照してください。

Zope 2.9.11 (2009/08/06)
--------------------------

バグ修正
+++++++++

- Launchpad #373299:
  OFS.CopySupport で String で例外を投げていた問題を修正。

- Launchpad #332168:
  Connection.py で DB コネクション文字列を例外発生時に見えないようにした。

- ZEO ストレージサーバーに影響する ZEO のネットワークプロトコルの脆弱性を修正。

Zope 2.9.10 (2008/10/24)
--------------------------

バグ修正
+++++++++

- HTTP レスポンスヘッダに HTTP 仕様 (RFC2616) 違反となる CRLF
  ペアを入れないことを保証するようにした。

- Launchpad #282677:
  guarded_map と guarded_zip (RestrictedPython) の実装とテストを修正。

- ``AccessControl.ZopeGuards.guarded_import`` 関数は多くの Unauthorized
  例外を ImportError に置き換えていた。そんなことをするな!
  また、ミュータブルなデフォルト引数を削除し、テストを強化した。

- Launchpad #281156:
  ``AccessControl.SecurityInfo.secureModule`` がインポートに失敗した
  モジュールのModuleSecurityを除くため、後で同じモジュールのインポートを
  試みた際に不明瞭なエラーとなる問題を修正。

- Launchpad #142667:
  プロダクト自動リフレッシュ問題の修正のため、ZODB-3.6.4 に更新。

- Launchpad #267545:
  DateTime (DateTime()) は正しい時刻 (hour) を保持するようになった。

- Launchpad #245649:
  Products パッケージは setuptools の正則な "namespace package" ルールの下、
  配置されるようになりました。

- Launchpad #239636:
  HEAD リクエストが NotFound エラー時には空の body を返さないようにした。
  (訳注: RFC では NotFound 時にbodyを返してはいけない)

- Launchpad #234209:
  De-tabify ZPublisher/HTTPRequest.py

- Hotfix-2008-08-12 を組み込んだ。

Zope 2.9.9 (2008/05/10)
--------------------------

バグ修正
+++++++++

- Launchpad #142350:
  概要が提供されている場合に、各プロパティーの行のタイトルとして
  表示するようにした。

- Launchpad #200007:
  DateTime(anotherDateTime) がタイムゾーンを保持するようになった。

- Launchpad #143813:
  zopectl は子プロセスが失敗したときに非ゼロ終了するようになった。

- Launchpad #143748:
  Products.Five.fiveconfigure.handleBrokenProduct での
  logging モジュールの誤った利用コードを取り除いた。
  この修正のために Products.Five 1.3.11 にアップグレードした。

- Launchpad #147201:
  zope.conf に文字列で書かれたコンテナクラスを、追加した products
  ディレクティブからも型として扱うように修正した。

- Collector #2287:
  フォームの ``:record`` オブジェクトに十分な辞書インターフェースが
  implement されていない問題。

- Collector #2346:
  FCGI サーバーの、ユーザー名をログ記録する仕組みがクラッシュを
  引き起こす問題。

- Collector #2332:
  SessionDataManger: ConflictError を飲み込む問題。
  （代わりに "External session data container '%s' not found."
  というエラーが表示される）

- Launchpad #146408:
  Transience.py での logger の誤った呼び出しを修正した。

Zope 2.9.8 (2007/07/05)
--------------------------

バグ修正
+++++++++

- ZODB 3.6.3 に更新

- Zope 3.2.3 コードベース に更新

- Collector #1306:
  ローカルロールを使用している画面で獲得に失敗する問題。

- The REQUEST no longer accepts holds after it has been closed.

- Collector #2153:
  クォートされていないスペースを含むクッキーに対応。

- Collector #2295:
  PythonScripts 内のコメントがシンタックスエラーを起こす問題。

- Collector #2307:
  ObjectCopiedEvent が sublocations に配信されない問題。

- Fixed:
  ZClass のテストが壊れていた問題。 zope.interface.Implements
  が pickle 出来ない問題に関連して。 zope.interface パッケージを
  Zope 3.2 branch にアップデートして対応した。

- Collector #2260:
  Examples.zexp の問題を修正。

- Collector #2321:
  クライアントの IP アドレスを Request から展開する際に、信頼している
  Proxy をスキップする問題。

- Collector #2318:
  zopectl が使用しているコントロール用ソケットを zope.conf
  で設定できるようにした。

- Collector #2316:
  index をブラウズするときに DateTimeIndex dates を正しく Unpack
  されるようにした。

- Collector #1866:
  304 HTTP ステータスの時、コンテンツ長を持つべきではない。

- Collector #2300:
  *全ての* HTTP Response headers のデリミタは CRLF とする。

Zope 2.9.7 (2007/03/25)
--------------------------

バグ修正
+++++++++

- Collector #2298:
  webdav.Resource.COPY と webdav.Resource.MOVE が期待されている
  copy/move イベントを送信していなかった。

- Collector #2296:
  ZClass プロダクトの import を修正。 meta_type 情報がパーミッションを
  持たないオブジェクト貼り付け時の BBB サポートの削除により動作しなく
  なっていた。

- Collector #2294: Protected DOS-able ControlPanel methods with the
  same 'requestmethod' wrapper.

- Collector #2294:
  様々なセキュリティー上リスクのあるアクセスを、新しく追加したデコレータで
  防御した。デコレータはPOSTリクエストでのみアクセスを許可する。
  これは Zope 2.11 の requestmethod decorator factory をバックポートした。

- Collector #2288:
  BaseRequest と HTTPRequest で要求された URL について、 ``@`` と ``+``
  はクォートしないようにした。

- Undeprecated:
  zLOG の Deprecate を解除した。これは Python の logging module
  の後方互換性のため今後も残ろうだろう。

- Collector #2263:
  field2ulines が空文字列を正しく変換していなかった。

- Collector #2191:
  DateTime について、後方互換性が無くなっていた変更を元に戻した。

- Add Python 2.4.4:
  最適な Python のバージョンとして Python-2.4.4 を configure に追加。


Zope 2.9.6 (2006-11-22)
--------------------------

バグ修正
+++++++++

- Collector #2191:
  拡張した DateTime パーサーが ISO8601 規格に対応。

- Shared.DC.ZRDB.DA.DA の _cached_result を動作するように修正: 

  - Collector #2212 で報告されたKeyErrorを修正

  - 高負荷時に発生する2つのメモリリークを修正

  - あいまいな Shared.DC.ZRDB.DA.DA.connection_hook 使用によるキャッシュ
    Key の破損を修正。

  - キャッシュが非常に大きい場合の不正なキャッシュのソートを修正。
    (resulting in newer results being dumped)

- Collector #2237:
  make のメッセージで、 ``make instance`` する前に ``make inplace``
  するように表示していなかった問題を修正。

- Collector #2235:
  いくつかの ZCatalog メソッドがオブジェクトのブール評価行っていたため、
  None ではなく __len__ で評価されていた。いくつかの ``if not obj`` を
  ``if obj is None`` に置き換えた。 

- Collector #2218:
  OFS/Cache.py の間違った logger 引数を修正。

- Collector #2205:
  ZRDB/Connection.py の間違った logger 引数を修正。

- Collector #2208:
  HTTP ヘッダの content-type が ``text/*`` の場合のみ charset
  を書き換え/設定するように変更。

- Collector #2206:
  skel/bin/zopectl.in と skel/bin/runzope.in で、PYTHONPATH を既存の
  PYTHONPATH も含めて設定するように変更。

Zope 2.9.5 (2006/10/03)
--------------------------

バグ修正
+++++++++

- Call setDefaultSkin on new requests created as the result of
  ConflictError retries.

- Collector #2189: Fix logging of errors during product refresh.

- Collector #2185: Log username for FCGI requests.

- Collector #2152: Fixed MailHost documentation; simple_send does not
  process or validate its arguments in any way.

- Collector #2175: ZTUtils.make_hidden_input did not escape double-quotes.

- Collector #1907: Moved 'alt' property from File to Image.

- Collector #1983: Specifying session-resolution-seconds >= 1200 caused
  Zope startup to fail.

- Collector #2169: webdav.Resource.COPY did not send ObjectClonedEvent.

- Updated Five to bugfix release 1.3.7.

- Collector #2157: Expose name of broken class in SystemError raised
  from '__getstate__' of a broken instance.

- Usage of 'urljoin' in 'webdav.davcmds' could lead to wrongly
  constructed urls.

- Collector #2155: Fix wrong parameter being passed to
  logger's error() method, with tests.

- Collector #2178: Fix ZopeTestCase doctest support for layers

- included Zope 3.2.2

Zope 2.9.4 (2006/07/21)
--------------------------

バグ修正
+++++++++

- reStructuredText/ZReST:
  セキュリティー上の理由により、raw_enabled設定を0にした

- Collector #2113:
  ``zopectl test`` がCtrl-Cをマスクする問題

- OFS Application:
  deprecation warnings を更新。 ``__ac_permissions__`` と ``meta_types``
  のサポートが Zope 2.11 で終了、 ``methods`` のサポートはおそらく長く残る。

- Collector #2136:
  ResourceLockedError のレスポンス値を正しい値に修正。

- Collector #2109:
  XML-RPC が DateTime.DateTime オブジェクトをハンドリングしない問題

- Collector #2016:
  DemoStorage が ``_old`` 属性を持たずに base storages をラップ出来ない
  問題を修正を修正。

- Collector #2133:
  standard_error_messages は sync の対象外。

- Five: バグ修正リリース Five-1.3.6 に更新。

- Collector #2116: sequence.sort() が locale 関連の比較方法でうまく動作しない。

- Collector #2077: ACTUAL_URL と SiteRoot の問題を修正。

- Collector #2073: OFS.Owned.changeOwnership の間違った挙動を修正。

- Collector #2063: MailHost.sendTemplate() のコードを色々とクリーンアップ。

その他の変更
+++++++++++++

- Disabled docutils file inclusion completely, rather than trying
  to jigger it via configuration settings.

- Zope2ビルド方法を ``zpkg`` 使う方法から、"従来の"
  ``./configure && make && make install`` の手順に戻した。

- ZODB のバージョンを 3.6.2 に上げた。

Zope 2.9.3 (2006/05/13)
--------------------------

バグ修正
+++++++++
- Collector #2083: ``make clean`` で不要物を削除。

- Collector #2082: ``make intall`` の問題を修正。

- Collector #2081: ``make instance`` が適切でないディレクトリや認証を
  作成する問題。

- Collector #1447: 
  VirtualHost 化された Zope でコンテンツ編集時に AcceleratedHTTPCacheManager
  が正しいURLを取り除く問題。

- Collector #2062: manage_historyCopy が動作しない問題。
  修正してテストを書いた。

- Collector #2061: Windows の改行コードが restricted code compilers
  に渡されていた問題。

- Collector #2072: __bobo_traverse__ の制限を超えるセキュリティーの問題を
  修正するパッチを適用しテストを書いた。

- 不足していた Zope 3 packages を追加: zope.app.intid,
  zope.app.keyreference, zope.app.session, zope.contentprovider,
  zope.viewlet 

- Five をバグフィックスリリース 1.3.5 にアップデートした。

- OFS.PropertyManager: 不足していたセキュリティー設定を追加。

- Products.SiteErrorLog: SiteErrorLog は __traceback_supplement__ を除き、
  トレースバックをフォーマットせずに event.log にコピーするようにした。


Zope 2.9.2 (2006/03/27)
--------------------------

バグ修正
+++++++++

- Collector #2051:
  XMLエクスポート/インポートに関する問題を修正するための Yoshinori Okuji
  氏のパッチを適用した。

- webdav.Resource で NotFound をインポートしていなかった。

- Collector #2037:
  VHM 経由でルートにアクセスしたとき ACTUAL_URL が `http://www.mysite.com//`
  のようになっていた問題。

- Put the default skin interface on the request once it is
  created, both in ZPublisher and ZopeTestCase.

- Five を 1.3.3 にアップデートした。詳しくは Products/Five/CHANGES.txt を参照。

Zope 2.9.1  (2006/02/25)
--------------------------

バグ修正
+++++++++

- Collector #1819:
  次のシグネチャを修正: MountedObject.SimpleTrailblazer._construct()

- Collector #1991:
  ZPublisherが%20で終わっているURLを適切に対処していませんでした。

- Collector #2013:
  エラーメッセージの XHTML 適応度を高めました。いくつかの <p> タグが
  閉じていませんでした。

- Collector #1999:
  FTP における名前変更機能の不具合を修正しました。
  (RNFRはステータスコード250ではなく350を返すようになりました)

- Collector #2002:
  ls -R 機能の不具合を修正しました。
  (OFS フォルダのサブクラスで再起処理が行われていませんでした)

- Collector #2019:
  cAccessControl モジュールから validateValue() を取り除きました
  （Python 実装版の AccessControl モジュールからはすでに取り除かれていました）

- Collector #1989:
  Zope 2.9.0 では test.py をインスタンス側に置き $ZOPE_HOME/bin/test.py
  を消しましたが、再び $ZOPE_HOME/bin 側にインストールして zopectl test
  ができるようにしました。

- zope.app.introspector がソースアーカイブに含まれていませんでした。

- zLOG を正式に廃止します（Zope 2.11までには取り除かれます）。
  Python の logging モジュールがその代わりとなります。

- OFS.content_types は廃止され、 zope.app.content_types がその代わりと
  なります。

Zope 2.9.0 (2006/01/09)
--------------------------

バグ修正
+++++++++

- deprecated OFS.content_types

- Fixed ConflictError when using sessions.

Zope 2.9.0 beta 2 (2005/12/24)
-------------------------------

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

- For content-type HTTP headers starting with 'text/' or 'application/'
  the 'charset' field is automatically if not specified by the
  application. The 'charset' is determined by the content-type header
  specified by the application (if available) or from the
  zpublisher_default_encoding value as configured in etc/zope.conf

- Collector #1976: FTP STOR command would load the file being
  uploaded in memory. Changed to use a TemporaryFile.

- OFS ObjectManager: Fixed list_imports() to tolerate missing
  import directories.

- Collector #1965: 'get_transaction' missing from builtins without
  sufficient deprecation notice (ZODB 3.6 properly removed it, but
  Zope needs to keep it for another release).

- Several zope.app packages were forgotten to be included in the
  first beta due to the now zpkg-based build and release process.

機能追加
+++++++++

- The SiteErrorLog now copies exceptions to the event log by default.

- Added a 'conflict-error-log-level' directive to zope.conf, to set
  the level at which conflict errors (which are normally retried
  automatically) are logged. The default is 'info'.

Zope 2.9.0 beta 1 (2005/12/06)
--------------------------------

機能追加
+++++++++

- ObjectManager now has an hasObject method to test presence. This
  brings it in line with BTreeFolder.

- Using FastCGI is officially deprecated

- Improved logging of ConflictErrors. All conflict errors are
  logged at INFO, with counts of how many occurred and how many
  were resolved. Tracebacks for all conflicts are logged a DEBUG
  level, although these won't help anyone much. If a conflict
  error is unresolved, it will now bubble up to error_log and
  standard_error_message.

- Fixed unclear security declarations. Warn when an attempt is
  made to have a security declaration on a nonexistent method.

- updated to ZPL 2.1

- interfaces: Added 'Interfaces' tab to basic core objects.
  This is a Five feature and only available if the classes are made
  five:traversable. It allows to inspect interfaces and to assign
  marker interfaces through the ZMI.

- webdav: Added support for the z3 WriteLock interface.
  It is no longer necessary to have the WriteLockInterface in the
  __implements__ list of lockable objects. All classes inheriting from
  LockableItem inherit also the IWriteLock interface. Note that this
  enables webdav locking for all subclasses by default even if they
  don't specify the WriteLockInterface explicitly.

- App ProductContext: Made registerClass aware of z3 interfaces.
  Z2 and z3 interfaces are registered side by side in the same tuple in
  Products.meta_types. IFAwareObjectManagers like the ZCatalog work now
  with z3 interfaces as well.

- Zope now sends Zope 3 events when objects are added or removed
  from standard containers. manage_afterAdd, manage_beforeDelete
  and manage_afterClone are now deprecated. See
  lib/python/Products/Five/tests/event.txt for details.

- Zope now utilizes ZODB 3.6.  It had previously used
  ZODB 3.4.  As a result, the DBTab package was removed, as
  ZODB 3.6 has multidatabase support that makes DBTab
  unnecessary.

- Added a 'product-config' section type to zope.conf, allowing
  arbitrary key-value mappings.  Products can look for such
  confgiurations to set product-specific options.  Products mwy
  also register their own section types, extending the
  'zope.product.base' type. (see the example '<product-config>'
  section in skel/etc/zope.conf.in for sample usage).

- Collector #1490: Added a new zope.conf option to control the
  character set used to encode unicode data that reaches
  ZPublisher without any specified encoding.

- AccessControl, Acquisition, App, OFS, webdav, PluginIndexes,
  ZCatalog and ZCTextIndex: Added some Zope 3 style interfaces.
  This makes the bridged interfaces shipped with Five obsolete.

- ZConfig extension, address now also accepts symbolic port names
  from etc/services (unix) or etc\services (win32)

- ZPublisher.HTTPRequest.FileUpload now supports full file
  object interface.  This means Iterator support was added. (for
  line in fileobject: ..., as well as fileobject.next() and
  fileobject.xreadlines() ) Collector #1837

- Switched the bundled Zope 3 to release 3.2 and upgraded the
  Five product to version 1.3 (see Products/Five/CHANGES.txt).

- The PageTemplate implementation now uses Zope 3 message
  catalogs by default for translation.  Old-style translation
  services such as Localizer or PlacelessTranslationService are
  still supported as fall-backs.  See Products/Five/doc/i18n.txt
  for more information.

- Switched to the new improved test runner from Zope 3.  Run
  test.py with -h to find out more.

- Collector #1904: On Mac OS X avoid a spurious OSError when
  zopectl exits.

.. rubric:: (Translated by Shimizukawa, `r102507 <http://svn.zope.org/Zope/branches/2.9/doc/CHANGES.txt?rev=102507&view=markup>`_)
  :class: translator

