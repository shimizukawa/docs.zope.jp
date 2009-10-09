Zope 2.11 Changes
==================

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については
`HISTORY.txt <http://svn.zope.org/Zope/branches/2.11/doc/HISTORY.txt?view=markup>`_
を参照してください。


Zope 2.11.5 (Unreleased)
--------------------------

バグ修正
+++++++++

- LP #414757 (Zope trunk からのバックポート):
  複製したリクエストをクリアするときに IEndRequestEvent を出力しないようにした。

- ZODB を 3.8.4 にアップデート。

Zope 2.11.4 (2009/08/06)
--------------------------

再構築
+++++++

- MountedStorageError 例外の定義を ZODB.POSExceptions から
  Products.TemporaryFolder.mount へ移動した
  (他では使われていないため)。

- LP #177513: Zope 2 特有のモジュールである ZODB/Mount.py を
  Products/TemporaryFolder/mount.py に移動した
  (Products/TemporaryFolder/TemporaryFolder.py が唯一使用しているため).

- Products/ZODBMountPoint/MountedObject.py が持っていた import 時の
  てきとうな依存を削除した。

バグ修正
+++++++++

- ZEO脆弱性:
  ZEOストレージサーバーに影響するZEOのネットワークプロトコルの脆弱性を修正。

Zope 2.11.3 (2009/05/04)
--------------------------

機能追加
+++++++++

- SiteErrorLog はエントリの ID をイベントログにも書き出すようになりました。
  これにより、ユーザーからのエラー報告とイベントログとを、 Zope の再起動後
  にも対応づけて見たり、イベントログの traceback と対応づけて SiteErrorLog
  の REQUEST 情報を見る事が出来るようになります。

再構築
+++++++

- 未公開の Zope 3.4.1 相当の新しいバージョンのパッケージにアップデート
  しました。
  http://svn.zope.org/zope.release/branches/3.4/releases/controlled-packages.cfg?rev=99659

- Zope 3.4.0 最終版に含まれる全ての最新版パッケージにアップデートしました。
  http://download.zope.org/zope3.4/3.4.0/

- 'App.class_init' に 'InitializeClass' エイリアスを追加しました。
  これは今後のマイグレーションを簡単にするためのものです。
  Zope 2.12 では 'InitializeClass' は 'Globals' からではなく、
  'App.class_init' から import するようにしてください。

- 'ApplicationDefaultPermissions' を 'App.class_init' から
  'AccessControl.Permissions' に移動しました。
  'Globals' から import することを避けることにより、
  サードパーティー製コードによる import サイクルを断つためです。
  以前の場所からも deprecation 警告無しで import することも出来ます。

- configure スクリプトで ZOPE_VERS に '2.11' を設定するようになりました。

- Products.PluginIndexes.PathIndex: 
  trunk から、 doc の修正と、最適化の修正をバックポート
  (ExtendedPathIndex も).

- 'pytz' を '2007f' から '2008i' にアップデート

- 'mechanize', 'ClientPath', 'pytz' のための svn:externals を
  Zope3 trunk の外のバージョン管理に移動。

- Testing.ZopeTestCase: DemoStorage から quota 引数を削除。これは
  ZODB 3.9 の前処理から呼ばれています。

バグ修正
+++++++++

- Launchpad #373299:
  OFS.CopySupport で String で例外を投げていた問題を修正。

- ZPublisher response.setBody:
  すでに header にある場合、 Accept-Encoding を破棄しないように修正。
  この問題はキャッシュ設定を難しくしていた。 (r99493 からマージ)

- Launchpad #267834:
  CRLF を使って HTTP ヘッダフィールドを正しく分割するように修正。
  RFC 2616 でリクエストされている。 (merged 90980, 92625)

- Launchpad #348223:
  catalog クエリを最適化: クエリ結果が空の状態になったら、
  短時間で index 検索を抜けるようにした。

- "Permission tab":
  ユーザーパーミッション表示の間違ったフォームパラメータを修正。

- Launchpad #290254, DateTime/DateTime.py:
  古い pickle が '_micros' 属性を見逃すのを防ぐために '__setstate__'
  を追加した。
  Python の pickle サポートにより '_micros' 属性付きの新しいインスタンス
  が作られるが、属性が更新される前にインスタンスの dict を unpickled
  ステートでクリアしていないため。

- Launchpad #332168, Shared/DC/RDBMS/Connection.py:
  DB 接続文字列を例外発生時に表示しないようにした。

- Launchpad #324876:
  meta-equiv ヘッダの charset 認識のために厳密な regex に修正

- Launchpad #174705:
  エラー情報オブジェクトが、 'tal:on_error' ハンドラーに制限コード
  (restricted code) を表示してしまう問題を修正。

- Acquisition ラッパーが__iter__を正しくproxyするように修正


Zope 2.11.2 (2008/10/24) 
--------------------------

バグ修正
+++++++++

- fix RFC2616:
  HTTP レスポンスヘッダに HTTP 仕様 (RFC2616) 違反となる CRLF
  ペアを入れないことを保証するようにした。

- Launchpad #282677:
  guarded_map と guarded_zip (RestrictedPython) の実装とテストを修正。

- ZODB を 3.8.1 にアップデート。

- Launchpad #143736, #271395:
  TempStorage の _ltid が AttributeError となっていた問題を修正。

- fix gurded_import:
  ``AccessControl.ZopeGuards.guarded_import`` 関数は多くの Unauthorized
  例外を ImportError に置き換えていた。そんなことをするな!
  また、ミュータブルなデフォルト引数を削除し、テストを強化した。

- Launchpad #281156:
  ``AccessControl.SecurityInfo.secureModule`` がインポートに失敗した
  モジュールの ModuleSecurity を除くため、後で同じモジュールのインポートを
  試みた際に不明瞭なエラーとなる問題を修正。

- fix DateTime:
  DateTime が pytz ではない tzinfo の datetime オブジェクトに変換していた。
  Timezones() は timezone のリストのコピーを返す (allows tests to run).
  (Backport of r89373 から trunk へのバックポート).

- Launchpad #253362:
  不正な HTTP_ACCEPT_CHARSET ヘッダを受け取った場合により良い振る舞いを
  するよう修正。

- Hotfix-2008-08-12 を組み込んだ。

- Launchpad #267545:
  DateTime(DateTime()) は正しい時刻 (hour) を保持するようになった。

- Launchpad #262313:
  ZMIのページテンプレート編集画面における "Expand macros" 設定に配慮。

- Testing.ZopeTestCase: installPackage was tied to the ZopeLite layer.

Zope 2.11.1 (2008/07/17)
--------------------------

バグ修正
+++++++++

- DeprecationWarning:
  ZPublisher.Iterators をインポートする際に表示される DeprecationWarning
  を抑止。このモジュールは BBB ではあるものの、 Zope 2.11 では Zope 2
  スタイルのインターフェースを使用するために Interface モジュールの
  インポート時に表示される問題のため。

- Launchpad #246748:
  メールを zope.sendmail 配信メカニズムを通して即時送信するために
  sendXXX() 系メソッドに 'immediate' オプションを追加。

- Launchpad #246290:
  後方互換性問題を修正。

- zope.testing を 3.5.3 に更新。

- Launchpad #245649:
  Products パッケージは setuptools の正則な "namespace package" ルールの下、
  配置されるようになりました。

- zope.viewlets を 3.4.2 に更新。

- zope.sendmail を 3.5.0 に更新(Launchpad #230831 の修正)

- Launchpad #239636:
  HEADリクエストがNotFoundエラー時には空のbodyを返さないようにした。(訳注:RFCではNotFound時にbodyを返してはいけない)

- fix ZODBMountPoint:
  ZODBMountPoint.SimpleTrailblazer の古い transaction.commit(1) という呼び出しを行っていたのを修正。


Zope 2.11.0 (2008/06/15)
--------------------------

再構築
+++++++

- 長らく残っていた著名な、しかし表に現れていなかった Zope 2 スタイル
  のインターフェース（これらは Interface パッケージの import で
  使われる）は既に trunk から取り除かれました。

バグ修正
+++++++++

- Launchpad #229549:
  PageTemplate を描画中に 'debug' フラグを無視しないようにした。
  (thanks to Eric Steele for the patch).

- zope.conf のルールに従って、'fast_listen' を 'fast-listen' に
  修正した。(ダッシュが正しい。アンダースコアではない)


Zope 2.11 rc 1 (2008/05/08)
----------------------------

バグ修正
+++++++++

- Launchpad #142350:
  概要が提供されている場合に、各プロパティーの行のタイトルとして
  表示するようにした。

- Launchpad #200007:
  DateTime(anotherDateTime) がタイムゾーンを保持するようになった。

- Launchpad #213311:
  ページ発行時のURLトラバース中に 'unsubscriptable object' エラーを
  ハンドリングする様にした。

- Products.Five: 
  vocabulary検索機能が2.11 beta 1で壊れていたのを修正。
  ZopeVocabularyRegistryが起動時にフックされていなかった。

- Launchpad #143813:
  zopectl は子プロセスが失敗したときに非ゼロ終了するようになった。

- Products.Five: 
  browser.addingの実装を再度zope.app.containerに合わせ調整した。
  この修正で多くのマイナーバグの修正と、古くなったコードの除去を
  行った。

- Launchpad #173658:
  使用されていないコード OFS.Traversable の unrestrictedTraverse を取り除いた。
  (NameErrorとなっていた).

- Launchpad #198274:
  '空の' ZopePageTemplate をunpickleすることが出来ない問題を修正。


Zope 2.11 beta 1 (2007/12/29)
-------------------------------

再構築
+++++++

- メソッド manage_afterAdd, manage_beforeDelete, manage_afterClone の
  deprecation 警告を discouraged 警告に変更した。これらのメソッドは
  Zope 2.11 では削除されないことになったが、近い将来無くなるだろう。イベ
  ントの仕組みを使うことを強く推奨する。

- 2つの宣言の実装を Five から実クラスへ移動した。

- Document.sequence: zope.sequencesort に置き換えた。

- 全ての Products フォルダ (zopeやzope.appフォルダ) は setuptools
  名前空間パッケージで定義されるようになった。詳しくは以下のURLを参照。
  http://peak.telecommunity.com/DevCenter/setuptools#namespace-packages

- ZPT: ZPT 警告の画面表示を削除。 zope.pagetemplate の実装から削除された
  ため。

- パッチ当て版ではない標準の docutils 0.4 を Zope に同梱した。Both 
  trusted and untrusted code are stillprotected against unwanted file 
  inclusion.

- ZGadflyDA を削除した (Zope 2.9からdeprecated)。コードは以下から
  取得可能。 http://svn.zope.org/Products.ZGadflyDA

- OFS.content_types を削除した (Zope 2.9からdeprecated)。

- zLOG の deprecated を解除。まだ後方互換性のために必要。(which will 
  remain a backward-compatibilityshim for the Python logging module.)

- Indexes: 使用されていないパラメータを '_apply_index' メソッドから削除

- '__ac_permissions__' と 'meta_types' 属性によるプロダクトの初期化の推奨
  されないサポートを削除。

- reStructuredText/ZReST: セキュリティー上の理由により、raw_enabled を
   0 に設定。

- OFS Image: 画像とファイルで isinstance(data, str) を使うように更新し、
  unicode オブジェクトに遭遇した場合は TypeError を raise するようにした。

- OFS Application: deprecation warnings (推奨しないことを表す警告)
  を更新した。 '__ac_permissions__' と 'meta_types' サポートを Zope 2.11
  で削除し、 'methods' サポートはまだ残す。


機能追加
+++++++++

- Zope2 startup: Zope は DatabaseOpend と ProcessStarting イベントを
  起動時に送るようになった。

- Testing.ZopeTestCase: "ZopeLite" テストレイヤーを導入した。これは
  ZTC と非 ZTC テストをより手軽に混在させることが可能とする。

- Testing/custom_zodb.py: DemoStorage 以外のストレージ使用のサポートを追
  加した。 FileStorage は $TEST_FILESTORAGE 環境変数によってカスタム
  Data.fs をマウントできる。 ZEO サーバーは $TEST_ZEO_HOSTと$TEST_ZEO_PORT
  環境変数で設定できる。この新しい機能により、標準の Zope テストランナー
  で既存の Zope インストール環境のためのテストを書き、実行することが出来
  るようになる。

- ZPublisher の HTTP リクエストに、 Zope 3 に相当する debug と locale
  の属性を持つようになった。 debug 属性は今までのところ、 Zope 3 ZPT
  エンジンを働かせるように zope.* 名前空間からコードに制限されました。
  locale 属性は zope.i18n.interfaces.locales.ILocale オブジェクトへの、
  locale に関連した情報(日時のフォーマット情報、言語変換、国名など)
  付きでのアクセスを提供する。
  Form variables of both debug and locale will shadow
  these two attributes and their use is therefor discouraged.

- MailHost: メールの配信に zope.sendmail を使うようになりました。これによ
  り、 MailHost が Zope のトランザクションシステム(コンフリクトエラーでの
  送信 email の複製を除く)に対応しました。追加で、 MailHost が非同期メール
  配信サポートに対応しました。 'Use queue' コンフィグオプションにより、
  ファイルシステム上に ('Queue directory' 以下に) メールキューが作成され、
  queue スレッドが起動し3秒ごとに queue をチェックします。これにより、
  メール送信時の衝撃を吸収します。また、 MailHost に TLS/SSL による暗号通信
  サポートが追加されました。

- ZODB 3.8 にインテグレートしました (BLOBサポート対応)

- 最新の Zope 3 コンポーネントをインテグレート (Zope 3.4)

- Windows で zopectl を使えるようになりました。全てのコマンドがサポートさ
  れています。また、 Windows 専用に2つのコマンド install と remove が追加
  されています。これらは Windows Service への登録と解除を行います。
  start, stop, restart の各コマンドは Windows サービスを操作します。これ
  らのコマンドを使用する前に 'bin\zopectl install' を一度行う必要があり
  ます。

- ZCatalog の返値となるobject (catalog brains) は
  ZCatalog.interfaces.ICatalogBrains インターフェースを持つようになりま
  した。

- 新しいモジュール, AccessControl.requestmethod は一つのリクエスト利用
  にのみメソッドの利用を制限するデコレータファクトリーを提供します。例
  えば、メソッドを @requestmethod("POST") のようにマーキングすると、
  publish 時に POST リクエストでのみ利用できるよう制限されます。いくつかの
  セキュリティーに関連したメソッドは POST のみに制限されます。

- PythonScripts: Pythonの sets モジュールを使えるようになりました。

- 'fast_listen' ディレクティブを etc/zope.conf の http-server と 
  webdav-source-server セクションに追加しました。これにより、起動フェー
  ズでソケットを開く順番を遅らせます。これは Zope がロードバランサーの背
  後で動作している場合などの特定の状況で使用します。
  (patch by Patrick Gerken)

- ZopePageTemplate の内部実装に unicode を使用するようにしました。非
  unicode インスタンスは on-the-fly で unicode に変換されます。ところでこれ
  が正しく働くのは ZPT インスタンスが utf-8 か ISO-8859-15 でエンコードされて
  いる場合のみです。他のエンコーディングの場合は、環境変数
  ZPT_REFERRED_ENCODING の値の utf-8 と ISO-8859-15 の前に使用する
  エンコーディングを設定してください。

  'output_encodings' プロパティーが、 WebDAV/FTP 操作での各エンコーディング
  と unicode との相互変換をコントロールします。

- ZPT の実装は UnicodeDecodeError 時の振る舞いについてコンフィグ出来るよう
  になりました。カスタム UnicodeEncodingConflictResolver を ZCML で設定す
  ることが出来ます。詳しくは Products/PageTemplates/(configure.zcml, 
  unicodeconflictresolver.py, interfaces.py) を参照のこと。

- AccessControl.Role: 新しいメソッド 
  manage_getUserRolesAndPermissions() が追加されました。

- AccessControl: "Security" タブのフォームに新しくユーザーに関連したパ
  ーミッションとロールのフォームを追加しました。

- Zope 3 ベースの、 Zope が起こしたいくつかの例外のための例外 view を ZCML で
  登録できるようになりました。例外 View を以下のように登録できます::

    <browser:page
      for="zope.publisher.interfaces.INotFound"
      class=".view.SomeView"
      name="index.html"
      permission="zope.Public" />

  これに関連する、 View を持っている例外は:

  - zope.interface.common.interfaces.IException

  - zope.publisher.interfaces.INotFound

  - zope.security.interfaces.IForbidden

  - zope.security.interfaces.IUnauthorized

  注意として、例外 view が動作するためには name は 'index.html' でなけれ
  ばならない。(patch by Sidnei da Silva from Enfold,
  integration by Martijn Faassen (Startifact) for Infrae)

- DateTime のタイムゾーンデータに pytz を使うようになりました。これによ
  って多くのタイムゾーン追加と夏時間情報の更新されました。


バグ修正
+++++++++

- Collector #2113:
  'zopectl test' が Ctrl-C をマスクしていて効かない問題.

- Collector #2190:
  zope.security.management.checkPermission 呼び出しが Zope 2
  のセキュリティーポリシーに迂回されていなかった。

  注意: もしあなたがすでに Zope 2.10 のインスタンスを使用しているなら、
  インスタンスを作り直すか、以下の数行をetc/site.zcmlファイルに追加する
  必要がある::

    <securityPolicy
          component="Products.Five.security.FiveSecurityPolicy" />

- Collector #2223:
  TALES における boolean 評価時のdefaultの扱いについて。

- Collector #2213:
  "古い" ZopePageTemplate を編集できない問題を修正。

- Collector #2235:
  いくつかの ZCatalog メソッドがオブジェクトのブール評価行っていたため、
  Noneではなく __len__ で評価されていた。いくつかの ``if not obj`` を
  ``if obj is None`` に置き換えた。

.. rubric:: (Translated by Shimizukawa, `r104363 <http://svn.zope.org/Zope/branches/2.11/doc/CHANGES.txt?rev=104363&view=markup>`_)

