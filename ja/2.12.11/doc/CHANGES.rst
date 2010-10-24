Changelog
=========

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については http://docs.zope.jp/zope2/releases/
を参照してください。

2.12.11 (2010-09-09)
--------------------

バグ修正
+++++++++

- BasicUserFolder と SiteErrorLog: string型で例外を発生させないようにした。

機能追加
+++++++++

- LP #193122: 新しいメソッド getVirtualRoot を Requestクラスに追加した。

- よく使われるモジュールなどについて Zope 2.13 で名前空間が変更されるもの
  について互換性を保てるように、Zope 2.12でも提供します。

- パッケージ更新:

  - Acquisition = 2.13.4
  - DateTime = 2.12.5
  - Products.ZSQLMethods = 2.13.3

2.12.10 (2010-07-20)
--------------------

バグ修正
+++++++++

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

- LP #143273: dtml-var の url_quote, url_unquote, url_quote_plus,
  url_unquote_plus でUnicodeオブジェクトを取り扱えるようにしました。

機能追加
+++++++++

- App.ImageFile.ImageFile がprefix無しの相対パスを受け取った場合、かつ、
  "ソフトウェアホーム"からの相対パスだと想定される場合に警告します。
  この挙動はパッケージが分割されて"ソフトウェアホーム"の相対パスというものに
  意味が無くなったため、今後は使われない方法になりました。

2.12.9 (2010-07-13)
-------------------

バグ修正
+++++++++

- Zope 2.12.8で導入されたキャッシュマネージャーの削除によって起きていた
  問題を修正しました。

- LP #143531: stateアクセスを許可しているbrokenオブジェクトに関する修正
  を行いました。

- LP #578326: browser:view ディレクティブの非公開のpermission属性サポート
  を追加しました。


機能追加
+++++++++

- `Products.ZSQLMethods` をZopeとは別の配布物に分離しました。この配布物
  には `Shared.DC.ZRDB` コードも含まれています。このコードは Zope 2.13
  からは同梱されません。もしこれが必要であれば `Products.ZSQLMethods`
  を明示的に利用するよう設定してください。このコードは Zope 2.12.9 以降
  でのみ動作します。

- `Shared` と `Shared.DC` を名前空間パッケージにしました。

- パッケージ更新:

  - DateTime = 2.12.4

2.12.8 (2010-06-25)
-------------------

バグ修正
+++++++++

- LP #597594: OFS.subscriber.maybeWarnDeprecated のパフォーマンスを
  最適化しました。

- LP #143639: コンテナ内の最後のキャッシュマネージャーが削除された時、
  そのコンテナから全てのトレースを削除する必要がありました。

- LP #143619: ZODBオブジェクトが削除されたらRAMCacheコンテンツも削除する
  ようにしました。

- LP #143403: 再帰的な所有権変更を行うとobjectValuesメソッドを持っていない
  オブジェクトの変更時に想定外の獲得が行われる問題を修正しました。

- LP #374818: ZopeTestCaseでフォルダとユーザーフォルダを作成するときに、
  古い"フォルダーメソッド"を使っているのをやめ、モジュールが提供している
  関数を使うようにしました。

- LP #143946: WebDAVでPUTに失敗したときのエラーメッセージをより親切な
  内容にしました。

- LP #142535: manage_changePropertiesメソッドのdocstringで、REQUEST引数
  にシンプルな辞書オブジェクトを渡しても良いと記述していましたが、これは
  間違っているのでdocstringを修正しました。


機能追加
+++++++++

- パッケージ更新:

  - ExtensionClass = 2.13.2
  - Missing = 2.13.1
  - Persistence = 2.13.2
  - Sphinx = 0.6.7

- ``Shared.DC.ZRDB.TM.TM`` クラスに ``setSortKey()`` メソッドを追加し、
  ``sortKey()`` メソッドをオーバーライドせずにコミット順を指定できるように
  しました。

2.12.7 (2010-06-13)
-------------------

バグ修正
+++++++++

- LP #583702: 2.12.6の#574286対応時に発生した、mailhostのキュー処理のコード
  における回帰バグを修正しました。

- xmlrpcにおけるUnauthorized例外処理を正しく行うようにしました。

- FiveのprocessInputs()が、 :list か :tuple の要素として int などの非文字列
  を含む場合にその要素を無視していました。また、 :records は完全に無視して
  おり、 :record についても何もしていませんでした。

- LP #143261: (太古の) Zope2.debug インタラクティブリクエストデバッガ
  が最上位の ``Zope`` モジュールを参照していましたが、それはだいぶ前に
  ``Zope2`` に名前変更されていたため不正な参照となっていました。

- LP #142874: ``URL`` か ``URL1`` という名前のオブジェクトを作成すると、
  ZMI画面の表示が正しく行われなくなる問題を修正。

- LP #142878: URLでAccessRuleとSiteRootオブジェクト設定の無効化を行う機能
  を削除しました。 ``os.environ`` による無効化指定は従来通り使用できるので
  こちらを使って下さい。

- LP #143144: zope.conf の ``mount-point`` ディレクティブ向けのドキュメント
  を修正しました。

- LP #142410: KeywordIndexで以下の場合においてインデックス処理を行わない
  ようにしました: インデックス対象の属性がドキュメントから無くなった場合。
  属性にアクセスした際にAttributeErrorが発生した場合。取得した属性の値
  が空だった場合。

- LP #142590: ``DTMLMethod`` と ``DTMLDocument`` の ``manage_edit``
  メソッドが ``TaintedString`` を扱えていなかった問題を修正しました。
  また ``DTMLDocument.manage_edit`` メソッドは親クラスのDTMLMethodで
  定義されていて冗長なため削除しました。

- LP #142451: ``changeOwnership`` で所有者の変更を再帰的に行わない場合は、
  子オブジェクトに一切触らないようにしました。

- LP #142750 と LP #142481: 同じフォルダに2つ以上の VirtualHostMoster
  を作成したり、新規に作成する際にIDを決める必要があったりする問題を
  避けるために、 VirtualHostMoster はデフォルトでIDを持つようにしました。
  今後はIDを指定することは出来なくなり、ID入力のための追加画面は
  無くなりました。

機能追加
+++++++++

- パッケージ更新:

  - distribute = 0.6.13
  - five.formlib = 1.0.3
  - lxml = 2.2.6
  - Sphinx = 0.6.6
  - tempstorage = 2.11.3
  - zope.app.apidoc = 3.6.6
  - zope.app.applicationcontrol = 3.5.1
  - zope.app.authentication = 3.6.1
  - zope.app.i18n = 3.6.2
  - zope.app.intid = 3.7.1
  - zope.app.wsgi = 3.6.1
  - zope.contenttype = 3.4.3
  - zope.container = 3.8.3
  - zope.deferredimport = 3.5.2
  - zope.intid = 3.7.2
  - zope.principalannotation = 3.6.1

- Fiveのテストブラウザをmechanize 0.1.x と 0.2.x の両方をサポートするよう
  更新しました。

- LP #142226: HTTPResponseのsetCookieメソッドにquotedキーワード引数が追加
  されました。これまでは、setCookieに渡すcookieの値は自動的にダブルクォート
  で括られていましたが、quoted引数の指定によってダブルクォートを付与しない
  ようにできます。


2.12.6 (2010-05-17)
-------------------

再構築
+++++++

- Zope2 自身がもっていた mkzeoinstance スクリプトを削除し、新たに
  zope.mkzeoinstance パッケージを使用するようにしました。

機能追加
+++++++++

- パッケージ更新:

  - DateTime = 2.12.2
  - distribute = 0.6.12
  - ExtensionClass = 2.13.1
  - initgroups = 2.13.0
  - Missing = 2.13.0
  - MultiMapping = 2.13.0
  - Persistence = 2.13.1
  - pytz = 2010h
  - Record = 2.13.0
  - RestrictedPython = 3.5.2
  - ThreadLock = 2.13.0
  - ZODB3 = 3.9.5
  - zope.app.locales = 3.6.1
  - zope.authentication = 3.7.1
  - zope.browser = 1.3
  - zope.cachedescriptors = 3.5.1
  - zope.deferredimport = 3.5.1
  - zope.i18n = 3.7.3
  - zope.i18nmessageid = 3.5.2
  - zope.lifecycleevent = 3.6.1
  - zope.pagetemplate = 3.5.1
  - zope.proxy = 3.6.0
  - zope.security = 3.7.3

バグ修正
+++++++++

- LP #142563:  ``AccessControl.User.NullUnrestrictedUserTests.__str__``
  を修正しました。

- SiteErrorLog(chameleon準拠)におけるいくつかのテンプレート上のエラーを
  修正しました。

- LP #267820: ``<dtml-in>`` タグの ``sequence_sort`` メソッドで例外が発生
  する問題を修正しました。

- LP #351006: ``zExceptions.ExceptionFormatter`` を利用した際に生成される
  HTMLにおいて ``<p>`` タグがネストする問題を修正しました。

- LP #411837: ``.htm`` 拡張子のリソースファイルを正しくページテンプレート
  リソースとして扱うようにしました。

- LP #435728: OFSP/help/sequence.py の docstring のインデントを修正しました。

- LP #574286: 複数のMailHostで同一のメール配信キューディレクトリを使う場合に
  配信処理用スレッドも共有することによってメールが2重に配信されないことを
  保証するようにしました。

- BaseRequest: 'traverseName' で発生したエラーの扱いを修正しました。

2.12.5 (2010-04-24)
-------------------

機能追加
+++++++++

- パッケージ更新:

  - Acquisition = 2.13.3
  - distribute = 0.6.10
  - roman = 1.4.0
  - zope.annotation = 3.5.0
  - zope.app.cache = 3.6.0
  - zope.app.dav = 3.5.1
  - zope.app.i18n = 3.6.1
  - zope.app.localpermission = 3.7.2
  - zope.app.principalannotation = 3.7.0
  - zope.app.publication = 3.8.1
  - zope.app.publisher = 3.8.4
  - zope.app.renderer = 3.5.1
  - zope.app.security = 3.7.3
  - zope.app.session = 3.6.1
  - zope.app.testing = 3.7.3
  - zope.app.wsgi = 3.6.0
  - zope.app.zapi = 3.4.1
  - zope.broken = 3.6.0
  - zope.catalog = 3.8.1
  - zope.formlib = 3.7.0
  - zope.i18nmessageid = 3.5.1
  - zope.index = 3.6.0
  - zope.keyreference = 3.6.2
  - zope.lifecycleevent = 3.6.0
  - zope.location = 3.6.0
  - zope.minmax = 1.1.2
  - zope.securitypolicy = 3.6.1
  - zope.viewlet = 3.6.1

- ``manage_* is discouraged. You should use event subscribers instead``
  という警告の表示をデバッグ時にログ出力されるようにダウングレードさせました。
  この警告が表示されたからと言って開発者がコードを修正する動機になりえない
  ためです。

- zope.testing の廃止に伴い、標準ライブラリのdoctestを使うようにしました。

- LP #143013: ConflictError発生時の最大リトライ回数を設定ファイルで変更出来る
  ようにしました。

バグ修正
+++++++++

- HTTPResponse: レスポンスにunicodeが含まれる場合、エンコードしようとして
  エラーとなる場合がある問題を修正しました。

- zExceptions: 無認証状態でのunicode関連のいくつかの問題を修正しました。

- LP #372632, comments #15ff.: 無認証状態の処理に関する回帰バグを修正しました。

- LP #563229: RFC 2965 / 2616 に違反する2重にクォートされた "邪悪な"
  JSON cookie に関する処理を修正しました。

- 派生クラスから使えるAPIとして
  ``Products.PluginIndexes.PathIndex.PathIndex.insertEntry``
  のドキュメントを記載しました。

- Zope 2.12.4 で導入されてしまったリソースディレクトリに関する回帰バグを
  修正しました。

- LP #143655: パスインデックスを用いてソート出来るように修正しました。

- LP #142478: ``PLexicon.queryLexicon`` にLexiconのパイプライン
  (例えば大文字小文字の正規化や、ストップワードの除去など)を使って
  正規化した単語を渡すように修正しました。

- 例外発生時に発行オブジェクトのcontextをラップするようにしました。


2.12.4 (2010-04-05)
-------------------

再構築
+++++++++++++

- メンテナンスされていないWindows向けのインストーラのビルドスクリプトと
  ``configure / make`` ビルドを削除しました。

- リポジトリポリシーに合うように、コピーライトとライセンス情報を更新しました。

- ZopeUndo パッケージを別の配布物に分離しました。

機能追加
+++++++++

- パッケージ更新:

  - Acquisition = 2.13.2
  - ExtensionClass = 2.13.0
  - Persistence = 2.13.0
  - pytz = 2010b

- イベント ZPublisher.interfaces.IPubBeforeStreaming がレスポンスのストリーム
  に write() メソッドを用いて最初のチャンクの出力を開始する直前に発行される
  ようになりました。これはレスポンスヘッダーを設定することが出来る最後の
  ポイントです。


バグ修正
+++++++++

- LP #142430: standard_error_message 内で使っていた title_or_id メソッド
  を使わないようにした。

- LP #257675: QUERY_STRING が環境変数に無い場合に request.form
  に含まれていた '-C':'' を取り除きました。

- Zope 3 スタイルのリソースディレクトリが Unauthorized エラーを出力していた
  問題を修正しました。この問題はリソースディレクトリのサブディレクトリに
  あるリソースに対して restrictedTraverse() を使用した場合に発生します。

- templateベースのブラウザビューで、 'macros' に対して探索が行える機能
  を元に戻しました。

- ZCTextIndex プロダクトのclearメソッドが獲得ラッパーを拒絶していました。

- LP #195761: ZMI XML export / import を修正しUIに復活させました。

- MailHost が EHLO に失敗した際に HELO を使うようにしました。


2.12.3 (2010-01-12)
-------------------

バグ修正
+++++++++

- LP #491224: エラーメッセージの表示を正しくエスケープするよう修正

- LP #246983: TALESの "string:" 表現内での unicode の衝突回避の仕組み
  を有効にしました。

- マルチパートのemail送信中に TypeError が発生する可能性があった問題
  を修正しました。

- ZEXP のインポート用のファイルはクライアントのホームディレクトリ
  からもインポート出来るようにしました。これによって buildout
  で作成されたZopeインスタンスの場合でもインポート出来るように
  なりました。

- utilities/load_site.py スクリプトの文法エラーを修正しました。


機能追加
+++++++++

- OFS.Image.File と OFS.Image.Image が IObjectModifiedEvent を発行
  するようにしました。発行タイミングは、ファクトリ経由で作成するときや、
  ZMI のフォーム経由で更新するとき (manage_edit() と manage_upload())
  です。

- zope.formlib / zope.app.form の統合を five.formlib という別パッケージ
  で行うように移動しました。


2.12.2 (2009-12-22)
-------------------

機能追加
++++++++++++++

- パッケージ更新:

  - ZODB3 = 3.9.4
  - docutils = 0.6
  - pytz = 2009r
  - zope.dottedname = 3.4.6
  - zope.i18n = 3.7.2
  - zope.interface = 3.5.3
  - zope.minmax = 1.1.1
  - zope.security = 3.7.2
  - zope.session = 3.9.2
  - zope.tal = 3.5.2

- DateRangeIndex を experimental.daterangeindexoptimisations のアイディア
  を元に内部拡張しました。ありがとう Matt Hamilton.

- ``management_page_charset`` のデフォルト値を iso-8859-1 から今日では
  一般的になっている utf-8 に更新しました。

- IPubBeforeCommit の対となる IPubBeforeAbort イベントを追加しました。
  このイベントは IPubFailure の前にのみ発行されますが、トランザクション
  がこの時はまだ開かれていることが保証されます。

- ZMI 画面のキャッシュパラメータ(訳注: /Control_Panel/Database/main の
  Cache Parametersタブ)にキャッシュサイズのbyte上限を含めるようにしました。

- 公式にサポートする Python を 2.6 のみにしました (Python 2.5 は非公式
  サポートとなります)。 Python 2.4 は完全にサポート対象から外れました。

バグ修正
++++++++++

- LP #143444: インポート / エクスポート フォーム上のチェックボックス
  とラジオボタンにラベルを追加しました。

- LP #496941: ``standard_html_header`` と ``standard_html_footer``
  の2つのデフォルトDTMLコンテンツと、これに関連する全てを削除しました。

- Products.PageTemplates のエンバグ(回帰バグ)を修正しました。
  ファイルシステム上のテンプレートが Products.Five.browser.pagetemplatefile
  の処理で TALES パス表現(python表現ではない)の処理コードがプロテクト
  されておりセキュリティーチェックに引っかかっていました。
  オリジナルの問題は以下を参照してください:
  http://codespeak.net/pipermail/z3-five/2007q2/002185.html

- LP #491249: ZRDB コネクションテストフォームの tabindex を修正しました。

- LP #490514:  DTML が ZPT から呼び出された場合の脆弱性汚染から保護する
  ようにしました。

- Products.Five.fiveconfigure の cleanUp() 関数で Products.meta_types が
  セットされていない時に、テストの tear-down 中に発生する可能性のある
  エラーを抑止しました。


2.12.1 (2009-11-02)
-------------------

機能追加
++++++++++++++

- パッケージ更新:

  - ZODB3 = 3.9.3  (バグ修正: blog 競合エラーによってコミットが停止する問題)
  - Acquisition = 2.12.4 (イテレーションサポートでの問題を修正)
  - setuptools = 0.6c11

- LP #411732: インターフェースで保護された、Viewにおける context と request
  のセキュリティー宣言の警告を静かにしました。

- ドキュメントの構成をクリーンナップし、 Windows でも HTML ドキュメント
  をビルドできるようにスクリプトを追加しました。

- Windows サービス関連機能を、インスタンスの zopeservice.py を使わずに
  提供できるようにリファクタリングしました。
  これにより、 buildout ベースのインスタンスが Windows で動作するように
  なりました。

バグ修正
++++++++++

- LP #440490: zopectl fg|adduser|run|debug がWindowsで動作するように修正しました。

- LP #443005: zopectl stop がWindowsで動作するように修正しました。

- LP #453723: zopectl start がWindows環境以外で動作しない問題を修正しました。

2.12.0 (2009-10-01)
--------------------

機能追加
++++++++++++++

- パッケージ更新:

  - ZODB3 = 3.9.0

- ``zope.app.schema`` の ``ZopeVocabularyRegistry`` をバックポートし、
  Five の初期化中に正しく登録されるようにしました。

バグ修正
++++++++++

- ``ZServer`` の代わりに Twisted HTTP サーバーを使えるようにする実験的な
  サポートの削除をバックポートしました。

- date インデックステストのタイムゾーンの問題の修正を trunk からバックポート
  しました。

- LP #414757 (Zope trunk からのバックポート):
  複製したリクエストをクリアするときに IEndRequestEvent を出力しないように
  しました。


2.12.0c1 (2009-09-04)
----------------------

機能追加
++++++++++++++

- パッケージ更新:

  - Acquisition = 2.12.3
  - pytz = 2009l
  - tempstorage = 2.11.2
  - transaction = 1.0.0
  - ZODB3 = 3.9.0c3
  - zope.app.basicskin = 3.4.1
  - zope.app.form = 3.8.1
  - zope.component = 3.7.1
  - zope.copypastemove = 3.5.2
  - zope.i18n = 3.7.1
  - zope.security = 3.7.1

バグ修正
++++++++++

- version.txt はもはや使用していないため、 pkg_resources でバージョン
  情報を取得して表示するように修正しました。


2.12.0b4 (2008-08-06)
----------------------

機能追加
++++++++++++++

- MailHost の send メソッドが unicode メッセージと
  email.Message.Message オブジェクトに対応しました。
  これにより charset と msg_type パラメータを渡すことが出来るようになり、
  文字列、ヘッダー、本文のエンコード時の助けになります。

- パッケージ更新:

  - ZODB3 = 3.9.0b5
  - zope.testing = 3.7.7

- scripts: インスタンス起動用の 'runzope' と 'zopectl' を エントリー
  ポイントとして追加しました。

バグ修正
++++++++++

- LP #418454: FTP サーバーが Python 2.6.X で動作しない問題を修正しました。

- PythonScript: 小さな Python 2.6 との互換セイン問題を修正しました。

- mkzopeinstance:
  インスタンススクリプトをより egg ベースに適した形にしました。
  カスタマイズした skel を使用している場合は、更新してください。

- Five: Zope 2.12.0a2 で追加されたパーミッション作成機能を修正しました。

- LP #399633: インタプリタのパスを修正しました。

- MailHost の管理画面は user と password のフィールドに None が設定
  されていた場合、それを文字列として扱わないようにしました。


2.12.0b3 (2009-07-15)
----------------------

機能追加
++++++++++++++

- パッケージ更新:

  - ZConfig = 2.7.1
  - ZODB = 3.9.0b2
  - pytz = 2009j
  - zope.app.component = 3.8.3
  - zope.app.pagetemplate = 3.7.1
  - zope.app.publisher = 3.8.3
  - zope.app.zcmlfiles = 3.5.5
  - zope.contenttype = 3.4.2
  - zope.dublincore = 3.4.3
  - zope.index = 3.5.2
  - zope.interface = 3.5.2
  - zope.testing = 3.7.6
  - zope.traversing = 3.7.1

- インデクシングにおいて、 datetime 値のサポートを PluginIndexes
  DataRangeIndex に追加しました。 DateIndex は既にこの機能を持っています。

再構築
+++++++++++++

- PluginIndexes: deprecated となった TextIndex を削除しました。

- HelpSys が deprecate となった TextIndex の代わりに ZCTextIndex
  を使うようになりました。データベース更新のために、 Zope の
  コントロールパネルの Product 登録から削除して、 Zope を再起動
  してください。

バグ修正
++++++++++

- LP #397861: "bin/zopectl adduser" における問題の修正のために、
  生成した 'zopectl' スクリプトで $PYTHON 環境変数を設定するように
  しました。

- PluginIndexes: IPluggableIndex に 'indexSize' を追加しました。

- HelpSys: ProductHelp は PluginIndexes の初期化に依存しなくなりました。

- App.Product: ProductHelp が Zope 2.12.0a1 から壊れていた問題を修正しました。

- ObjectManagerNameChooser を BTreeFolder2 でも動作するようにしました。

- ZPublisherExceptionHook 例外を正しく処理するようにしました。

2.12.0b2 (2009-05-27)
----------------------

再構築
+++++++++++++

- ``zope.app.pagetemplate`` の利用を全て取り除きました。利用していたコード
  はシンプルになりました。

- ``zope.app.pagetemplate.engine`` の代わりに ``zope.pagetemplate.engine`` 
  を使うようにしました。
  (update to versions 3.5.0 and 3.7.0, respectively, along with version 3.8.1
  of ``zope.app.publisher``).

- ``zope.publisher.interfaces.browser`` よりも ``zope.browser.interfaces``
  の ``IBrowserView`` インターフェースを使うようにしました。

- ``zope.app.container`` よりも ``zope.browser.interfaces`` の ``IAdding``
  インターフェースを使うようにしました。

- ``zope.processlifetime`` のイベント実装を使うようにし、
  ``zope.app.appsetup`` への依存を無くしました。

機能追加
++++++++++++++

- zExceptions.convertExceptionType:  new API, breaking out conversion of
  exception names to exception types from 'upgradeException'.

- Launchpad #374719: 新しい ZPublisher のイベントを導入:
  PubStart, PubSuccess, PubFailure, PubAfterTraversal, PubBeforeCommit.

- Testing.ZopeTestCase: Python 2.6 で DeprecationWarning が出ないように
  するために、ZODB.tests.warnhook のコピーを含めるようにしました。

- パッケージ更新:

  * python-gettext 1.0
  * pytz 2009g
  * zope.app.applicationcontrol = 3.5.0
  * zope.app.appsetup 3.11
  * zope.app.component 3.8.2
  * zope.app.container 3.8.0
  * zope.app.form 3.8.0
  * zope.app.http 3.6.0
  * zope.app.interface 3.5.0
  * zope.app.pagetemplate 3.6.0
  * zope.app.publication 3.7.0
  * zope.app.publisher 3.8.0
  * zope.browser 1.2
  * zope.component 3.7.0
  * zope.componentvocabulary 1.0
  * zope.container 3.8.2
  * zope.formlib 3.6.0
  * zope.lifecycleevent 3.5.2
  * zope.location 3.5.4
  * zope.processlifetime 1.0
  * zope.publisher 3.8.0
  * zope.security 3.7.0
  * zope.testing 3.7.4
  * zope.traversing 3.7.0

バグ修正
++++++++++

- Launchpad #374729: Firewall やセキュリティー proxy を使用すると、
  cookie の値のエンコードが無効になる問題を修正しました。

- Launchpad #373583: ZODBMountPoint のマウントの処理が壊れていた問題を
  修正し、テストを拡張しました。

- Launchpad #373621: ワーカースレッドがリークした場合に、例外を捕まえて
  ログ出力するようにしました。

- Launchpad #373577: 起動時のエラーをより詳細に分析できるようにするため、
  logging のセットアップをこれまでより早い時点で行うようにしました。

- Launchpad #373601:
  主トランザクションが閉じた後で永続データが更新されるような場合に、
  接続がリークしないように、接続を閉じる前にトランザクションを
  中止するようにしました。

- Fix BBB regression which prevented setting browser ID cookies from
  browser ID managers created before the ``HTTPOnly`` feature landed.
  https://bugs.launchpad.net/bugs/374816

- RESPONSE.handle_errors was wrongly set (to debug, should have been
  ``not debug``). Also, the check for exception constructor arguments
  didn't account for exceptions that didn't override the ``__init__``
  (which are most of them). The combination of those two problems
  caused the ``standard_error_message`` not to be called. Fixes
  https://bugs.launchpad.net/zope2/+bug/372632 .

- DocumentTemplate.DT_Raise:  'zExceptions.convertExceptionType' API
  を使用することにより、組み込み例外以外を使えるようになった。
  https://bugs.launchpad.net/zope2/+bug/372629 で、引数がない
  スクリプトの "Try" タブの表示が妨げられていた問題を修正した。

2.12.0b1 (2009-05-06)
---------------------

再構築
+++++++++++++

- ``zope.app.locales`` に依存しないようにしました。 Zope 2 は大抵は
  各パッケージが提供する翻訳を使用せず、必要ともされていません。
  この決定には、アプリケーション開発者から locales が無くなった、
  という意味を含んでいます。

- ``zope.app.testing`` の依存を取り除き、 ZopeTestCase の一部である、
  もっと小さい placeless setup を使うようにしました。

- updated to ZODB 3.9.0b1

機能追加
++++++++++++++
- zExceptions.convertExceptionType:  new API, breaking out conversion of
  exception names to exception types from ``upgradeException``.

- Extended BrowserIdManager to expose the ``HTTPOnly`` attribute for its
  cookie. Also via https://bugs.launchpad.net/zope2/+bug/367393 .

- Added support for an optional ``HTTPOnly`` attribute of cookies (see
  http://www.owasp.org/index.php/HTTPOnly).  Patch from Stephan Hofmockel,
  via https://bugs.launchpad.net/zope2/+bug/367393 .

バグ修正
++++++++++

- ZPublisher response.setBody:
  すでに header にある場合、 Accept-Encoding を破棄しないように修正。
  この問題はキャッシュ設定を難しくしていた。

2.12.0a4 (2009-04-24)
---------------------

バグ修正
++++++++++

- インデックス構造の作成のための zope.z2release で使われる、
  versions.cfg を修正しました。

2.12.0a3 (2009-04-19)
---------------------

2.12.0a2 のソースリリースのための Tarball は完全ではありませんでした。
setuptools と Subversion 1.6 の非互換性の問題を含んでいます。

再構築
+++++++++++++

- 古い Zope のバージョンで作られたデータベースを自動的にマイグレーション
  する機能を追加。 ``Control_Panel`` の ``Versions`` 画面は、自動的に
  Zope 起動時に削除されます。

- Globals.VersionNameName を含む、使われていないバージョン管理機能のコード
  を取り除きました。


2.12.0a2 (2009-04-19)
---------------------

再構築
+++++++++++++

- パーミッションを定義する <permission /> ZCML ディレクティブが無い場合、
  パーミッションを自動的に作成するようになりました。デフォルトでは、
  Manager ロールのみが許可されます。これは、新しいパーミッションが ZCML
  でのみ作成出来るという意味です。既存のパーミッションはこの方法では
  変更されません。

- <class /> ディレクティブで使われる <require set_schema="..." /> や
  <require set_attributes="..." /> が発していたエラーは、今後は警告
  になります。 Zope 2 には 'set' をプロテクトするというコンセプトは
  ありませんが、パッケージに再利用性を高めるためにも定義が書かれて
  いてもエラーにしません。

- パッケージ更新: Acquisition 2.12.1.

- パッケージ更新: DateTime 2.12.0.

- パッケージ更新: ZODB 3.9.0a12.

- バージョンを明示的には要求する ``getPackages`` ラッパーを setup.py
  から取り除きました。
  これにより、依存パッケージのより新しいバージョンを利用することが出来ます。
  今後は、このような KGS のバージョン情報は他の方法で表す必要が有ります。

- ``extras_require`` セクションを setup.py から取り除きました。
  (これは古いコードを壊す可能性がありました).

バグ修正
++++++++++

- Launchpad #348223: catalog クエリを最適化: クエリ結果が空の状態になったら、
  短時間で index 検索を抜けるようにした。

- Launchpad #344098: ``skel/etc/zope.conf.ing`` で、デフォルトでコメントアウト
  されている ``read-only-database`` オプションを削除しました。これは既に
  deprecated であり、 ZODB の ``component.xml`` で定義されています。
  ``zserver-read-only-mode`` ディレクティブの正しい書式 (suppressing log
  / pid / lock files) について説明を更新しました。
  ``read-only-database`` オプションについて、 deprecation を追加しました。
  このオプションは Zope 2.6 から設定しても効果が無いものでした。

- "Permission tab":
  ユーザーパーミッション表示の間違ったフォームパラメータを修正。

- PageTemplates: PreferredCharsetResolver を新しい種類の context でも
  動作するようにしました。この context は Acquisition ラッパーで
  ラップされていません。

- Object managers should evaluate to True in a boolean test.

2.12.0a1 (2009-02-26)
---------------------

再構築
+++++++++++++

- Switched Products.PageTemplates to directly use zope.i18n.translate and
  removed the GlobalTranslationService hook.

- Removed bridging code from Product.Five for PlacelessTranslationService
  and Localizer. Neither of the two is actually using this anymore.

- Removed the specification of ``SOFTWARE_HOME`` and ``ZOPE_HOME`` from the
  standard instance scripts.
  [hannosch]

- Made the specification of ``SOFTWARE_HOME`` and ``ZOPE_HOME`` optional. In
  addition ``INSTANCE_HOME`` is no longer required to run the tests of a
  source checkout of Zope.

- Removed the ``test`` command from zopectl. The test.py script it was relying
  on does no longer exist.

- Updated to ZODB 3.9.0a11. ZODB-level version support has been
  removed and ZopeUndo now is part of Zope2.

- The Zope2 SVN trunk is now a buildout pulling in all dependencies as
  actual released packages and not SVN externals anymore.

- Make use of the new zope.container and zope.site packages.

- Updated to newer versions of zope packages. Removed long deprecated
  layer and skin ZCML directives.

- Disabled the XML export on the UI level - the export functionality
  however is still available on the Python level.

- No longer show the Help! links in the ZMI, if there is no help
  available. The help system depends on the product registry.

- Updated the quick start page and simplified the standard content.
  The default index_html is now a page template.

- Removed deprecated Draft and Version support from Products.OFSP.
  Also removed version handling from the control panel. Versions are
  no longer supported on the ZODB level.

- Removed left-overs of the deprecated persistent product distribution
  mechanism.

- The persistent product registry is not required for starting Zope
  anymore. ``enable-product-installation`` can be set to off if you don't
  rely on the functionality provided by the registry.

- ZClasses have been deprecated for two major releases. They have been
  removed in this version of Zope.

- Avoid deprecation warnings for the md5 and sha modules in Python 2.6
  by adding conditional imports for the hashlib module.

- Replaced imports from the 'Globals' module throughout the 
  tree with imports from the actual modules;  the 'Globals' module
  was always intended to be an area for shared data, rather than
  a "facade" for imports.  Added zope.deferred.deprecation entries
  to 'Globals' for all symbols / modules previously imported directly.

- Protect against non-existing zope.conf path and products directories.
  This makes it possible to run a Zope instance without a Products or
  lib/python directory.

- Moved exception MountedStorageError from ZODB.POSExceptions
  to Products.TemporaryFolder.mount (now its only client).

- Moved Zope2-specific module, ZODB/Mount.py, to
  Products/TemporaryFolder/mount.py (its only client is
  Products/TemporaryFolder/TemporaryFolder.py).

- Removed spurious import-time dependencies from
  Products/ZODBMountPoint/MountedObject.py.

- Removed Examples.zexp from the skeleton. The TTW shopping cart isn't
  any good example of Zope usage anymore.

- Removed deprecated ZTUtil.Iterator module

- Removed deprecated StructuredText module

- Removed deprecated TAL module

- Removed deprecated modules from Products.PageTemplates.

- Removed deprecated ZCML directives from Five including the whole
  Five.site subpackage.

機能追加
++++++++++++++

- OFS.ObjectManager now fully implements the zope.container.IContainer
  interface. For the last Zope2 releases it already claimed to implement the
  interface, but didn't actually full-fill the interface contract. This means
  you can start using more commonly used Python idioms to access objects
  inside object managers. Complete dictionary-like access and container
  methods including iteration are now supported. For each class derived from
  ObjectManager you can use for any instance om: ``om.keys()`` instead of
  ``om.objectIds()``, ``om.values()`` instead of ``om.objectValues()``, but
  also ``om.items()``, ``ob.get('id')``, ``ob['id']``, ``'id' in om``,
  ``iter(om)``, ``len(om)``, ``om['id'] = object()`` instead of
  ``om._setObject('id', object())`` and ``del ob['id']``. Should contained
  items of the object manager have ids equal to any of the new method names,
  the objects will override the method, as expected in Acquisition enabled
  types. Adding new objects into object managers by those new names will no
  longer work, though. The added methods call the already existing methods
  internally, so if a derived type overwrote those, the new interface will
  provide the same functionality.

- Acquisition has been made aware of ``__parent__`` pointers. This allows
  direct access to many Zope 3 classes without the need to mixin
  Acquisition base classes for the security to work.

- MailHost: now uses zope.sendmail for delivering the mail. With this
  change MailHost integrates with the Zope transaction system (avoids
  sending dupe emails in case of conflict errors). In addition
  MailHost now provides support for asynchronous mail delivery. The
  'Use queue' configuration option will create a mail queue on the
  filesystem (under 'Queue directory') and start a queue thread that
  checks the queue every three seconds. This decouples the sending of
  mail from its delivery.  In addition MailHosts now supports
  encrypted connections through TLS/SSL.

- SiteErrorLog now includes the entry id in the information copied to
  the event log. This allowes you to correlate a user error report with
  the event log after a restart, or let's you find the REQUEST
  information in the SiteErrorLog when looking at a traceback in the
  event log.

バグ修正
++++++++++

- Launchpad #332168: Connection.py: do not expose DB connection strings
  through exceptions

- Specified height/width of icons in ZMI listings so the table doesn't
  jump around while loading.

- After the proper introduction of parent-pointers, it's now
  wrong to acquisition-wrap content providers. We will now use
  the "classic" content provider expression from Zope 3.

- Ported c69896 to Five. This fix makes it possible to provide a
  template using Python, and not have it being set to ``None`` by
  the viewlet manager directive.

- Made Five.testbrowser compatible with mechanize 0.1.7b.

- Launchpad #280334: Fixed problem with 'timeout'
  argument/attribute missing in testbrowser tests.

- Launchpad #267834: proper separation of HTTP header fields   
  using CRLF as requested by RFC 2616.

- Launchpad #257276: fix for possible denial-of-service attack
  in PythonScript when passing an arbitrary module to the encode()
  or decode() of strings.

- Launchpad #257269: 'raise SystemExit' with a PythonScript could shutdown
  a complete Zope instance

- Switch to branch of 'zope.testbrowser' external which suppresses
  over-the-wire tests.

- Launchpad #143902: Fixed App.ImageFile to use a stream iterator to
  output the file. Avoid loading the file content when guessing the
  mimetype and only load the first 1024 bytes of the file when it cannot
  be guessed from the filename.

- Changed PageTemplateFile not to load the file contents on Zope startup
  anymore but on first access instead. This brings them inline with the
  zope.pagetemplate version and speeds up Zope startup.

- Collector #2278: form ':record' objects did not implement enough
  of the mapping protocol.

- "version.txt" file was being written to the wrong place by the
  Makefile, causing Zope to report "unreleased version" even for
  released versions.

- Five.browser.metaconfigure.page didn't protect names from interface
  superclasses (http://www.zope.org/Collectors/Zope/2333)

- DAV: litmus "notowner_modify" tests warn during a MOVE request
  because we returned "412 Precondition Failed" instead of "423
  Locked" when the resource attempting to be moved was itself
  locked.  Fixed by changing Resource.Resource.MOVE to raise the
  correct error.

- DAV: litmus props tests 19: propvalnspace and 20:
  propwformed were failing because Zope did not strip off the
  xmlns: attribute attached to XML property values.  We now strip
  off all attributes that look like xmlns declarations.

- DAV: When a client attempted to unlock a resource with a token
  that the resource hadn't been locked with, in the past we
  returned a 204 response.  This was incorrect.  The "correct"
  behavior is to do what mod_dav does, which is return a '400
  Bad Request' error.  This was caught by litmus
  locks.notowner_lock test #10.  See
  http://lists.w3.org/Archives/Public/w3c-dist-auth/2001JanMar/0099.html
  for further rationale.

- When Zope properties were set via DAV in the "null" namespace
  (xmlns="") a subsequent PROPFIND for the property would cause the
  XML representation for that property to show a namespace of
  xmlns="None".  Fixed within OFS.PropertySheets.dav__propstat.

- integrated theuni's additional test from 2.11 (see r73132)

- Relaxed requirements for context of
  Products.Five.browser.pagetemplatefile.ZopeTwoPageTemplateFile,
  to reduce barriers for testing renderability of views which
  use them.
  (http://www.zope.org/Collectors/Zope/2327)

- PluginIndexes: Fixed 'parseIndexRequest' for false values.

- Collector #2263: 'field2ulines' did not convert empty string
  correctly.

- Collector #2198: Zope 3.3 fix breaks Five 1.5 test_getNextUtility

- Prevent ZPublisher from insering incorrect <base/> tags into the
  headers of plain html files served from Zope3 resource directories.

- Changed the condition checking for setting status of
  HTTPResponse from to account for new-style classes.

- The Wrapper_compare function from tp_compare to tp_richcompare.
  Also another function Wrapper_richcompare is added.

- The doc test has been slightly changed in ZPublisher to get
  the error message extracted correctly.

- The changes made in Acquisition.c in Implicit Acquisition
  comparison made avail to Explicit Acquisition comparison also.

- zopedoctest no longer breaks if the URL contains more than one
  question mark. It broke even when the second question mark was
  correctly quoted.

その他の変更
+++++++++++++

- Added lib/python/webdav/litmus-results.txt explaining current
  test results from the litmus WebDAV torture test.

- DocumentTemplate.DT_Var.newline_to_br(): Simpler, faster
  implementation.

.. rubric:: (Translated by Shimizukawa, `r114668 <http://svn.zope.org/Zope/branches/2.12/doc/CHANGES.rst?rev=114668&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/CHANGES.html>`_)
  :class: translator

