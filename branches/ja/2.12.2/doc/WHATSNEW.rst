Zope 2.12 の新機能
====================

このページでは、このバージョンの Zope 2 の新しい高度な機能と変更点について
説明します。

新機能やバグ修正の細目については、 `変更の詳細 <CHANGES.html>`_
を参照してください。


新しいPythonバージョンのサポート
---------------------------------

Zope 2 は2006年の夏頃のバージョン2.9から Python 2.4 をサポートし、また
必須としてきました。Pythonのそれ以降のバージョンは長い間 Zope 2 では
サポート外でした。

このバージョンの Zope 2 は Python 2.5 と 2.6 を同時にサポートに加えました。
Python 2.4 はすでにメンテナンスされておらず、 Zope 2 の公式サポート
からも外すことになりました。とはいえ、 Zope 2 はまだ Python 2.5 を必須
とするようなコードを持っていないので、 Python 2.4 でもまだビルドして
動かすことが出来ます。

Python 3 は後方互換性を持たないリリースです。 Python 3 を採用するための
明確なロードマップはまだありません。
Zope 2 が複数のメジャーバージョンをリリースするか、あるいは何年もかかる
問題だと予想しています。


完全なegg化
--------------

Zope 2 は完全なegg化が完了し、 `setuptools
<http://pypi.python.org/pypi/setuptools>`_ との互換性を手に入れました。
今後は `easy_install <http://peak.telecommunity.com/DevCenter/EasyInstall>`_
や `zc.buildout <http://pypi.python.org/pypi/zc.buildout>`_ といった
一般的なツールを使用して Zope 2 をインストールすることが出来ます。

Zope 2 のリリース版やインストールファイルは Python package index 
http://pypi.python.org/pypi/Zope2 にあります。

Zope 2 のegg化やファイルシステム上でのレイアウト変更に合わせてパッケージング
にいくつかの変更を行いました。環境変数 `SOFTWARE_HOME` と `ZOPE_HOME` は
コントロールスクリプト内で使用されなくなりました。 Zope 2 パッケージ内で
データにアクセスする必要がある場合、例えば
`import os, OFS; os.path.dirname(OFS.__file__)` といった方法で OFS
パッケージ内のファイルにアクセスしてください。

一般的には、これまでのように `lib/python` や `Products` ディレクトリに
コードがあることを期待してはいけません。これらの仕組み自体はこれからも
機能しますが、 distutils や setuptools を使用してパッケージを管理し、それら
の仕組みで `sys.path` にパスが追加され、 Python の標準の仕組みで動作する
ようにすることを勧めます。独立した Python 環境を作成するために
`zc.buildout <http://pypi.python.org/pypi/zc.buildout>`_ と `virtualenv
<http://pypi.python.org/pypi/virtualenv>`_ が広く使われています。


Zope Toolkit
------------

このバージョンの Zope 2 は Zope Toolkit 上で動作しています。 Zope Toolkit
は再利用可能でよく使われているZope 3のパッケージから抽出したものです。
Zope Toolkit はそれ自体で完結するのではなく、フレームワークと
アプリケーションにフォーカスしています。
Zope Toolkit の一部は、 Zope 2, Plone, Grok, Repoze.bfg, その他多くの
アプリケーションやフレームワークで使用されています。

Zope Toolkit は、パッケージの依存関係をもっと保守しやすくし、良い構造の
コードにすることに、最もフォーカスしています。この取り組みにより、 
Zope 2 に含まれていたパッケージ数は120以上から60程度まで劇的に改善されました。
Zope 2 の総コードサイズは、依存も含めて 200,000 行以上が削減されました。

Zope Toolkit の変更についての詳しい情報は、
http://docs.zope.org/zopetoolkit/ にあります。
Zope 3 から Zope Toolkit へのアップグレード情報は
http://docs.zope.org/zopetoolkit/migration/index.html.
にあります。


ZODB 3.9
--------

このバージョンの Zope 2 は最新の `ZODB (3.9)
<http://pypi.python.org/pypi/ZODB3>`_ が含まれています。
これには多くの新しい設定オプションと、不具合修正が含まれています。
ファイルストレージにはblobサポートが追加され、demo ストレージは
大きく拡張されました。 大規模環境におけるZEOサーバーとクライアントを
チューニングしたり、キャッシュの無効化のコントロール、広範囲での
パッケージングの度合いなど、数多くのオプションが追加されました。

バージョン 3.9 の変更の詳細については `ZODB3 change log
<http://pypi.python.org/pypi/ZODB3>`_ を参照してください。


モジュールのクリーンナップ
---------------------------

Zope 2 のこれまでのリリースと同様に、このバージョンでも、前のバージョンで
Deprecation 扱いとなった様々なモジュールを削除しています。

特に ZClasses とそれに関連するモジュールが Zope 2 から完全に削除されました。
これによって、永続プロダクト登録(persistent product registry)はオプション
扱いとなりましたが、デフォルトではまだ有効です。
もしアプリケーションがこの機能に依存していないのであれば、 `zope.conf` で
以下のようにして設定を無効化できます::

  enable-product-installation off

このオプションをオフにすることで、 Zope は今後多くの場合において、起動中に
データベースへの書き込みを行わないようになります。

ZODB 3.9 にアップグレードしたことにより、データベースレベルでのバージョン
機能がサポートされなくなりました。 結果として、 `Products.OFSP` 内の多くの
モジュールが削除されました。また、データベースからオブジェクトをロードする
ための低レイヤの API からバージョン指定の引数が削除されました。


ドキュメントの更新
---------------------

Zope 2 は `Sphinx <http://sphinx.pocoo.org/>`_ を使用して HTML による
ドキュメントページの生成を行うようになりました。
公式のドキュメントを Sphinx で作成して http://docs.zope.org/ で公開
する取り組みが進行中です。

これまでのところ、 Zope 2 Book, Zope Developers Guide, その他多くの
細かな文章が reStructuredText に書き換えられ、更新されています。


Acquisition redux
-----------------

この変更を技術的に一言で説明すると、 "獲得 (Acquisition) の機能として
__parent__ ポインタを導入した" です。小さな変更のように聞こえますが、
これは Zope コンポーネント技術を Zope 2 に統合するための主要な１ステップです。

Zope コンポーネントアーキテクチャと、付随する多くのコンセプトを
Zope 2 の Five (Zope 2 + 3) と呼ばれる統合レイヤに組み込んでいます。
このような統合レイヤが必要である主な理由の一つが、 Zope 2 が獲得
(Acquisition) のコンセプトと密接に関係していることです。
全てのセキュリティーの仕組みや、 object traversal (訳注:URL階層に合わせて
オブジェクトを辿る仕組み)、 publication などがこの技術に依存しています。

Zope 2 の中で動作する全てのクラスは Acquisition クラスまたはその派生クラス
を継承する必要がありました。その結果、ほとんどどんな外部パッケージも
統合レイヤなしでは Zope 2 の中で直接動かすことが出来ませんでした。

このバージョンの Zope 2 では、オブジェクトは Zope の API にロケーション
を示すための新しいオプションを持つようになりました。このオプションは
``__parent__`` 属性で明示的に親オブジェクトを指し示し、 
`zope.location <http://pypi.python.org/pypi/zope.location>`_ の ILocation
API とほぼ同じ仕様です。 Browser views (訳注: Zope コンポーネント
アーキテクチャの一つ) や、他のロケーションに依存したコンポーネント
は既に ILocation を実装しています。

この規約を守っているクラスがコンテナの中に格納されると、 `__parent__` にそのコンテナオブジェクトへのポインタが設定されるようになります。そしてそのように実装されたオブジェクトは、 __parent__ を使ってオブジェクト階層を親方向に辿っていくことができます。 Acquisition クラスをベースとしているクラスの場合、これに関する情報をオブジェクトに格納していなくても、 Acquisition ラッパーがオブジェクトの代わりにやってくれるため、コンテナへの参照はラッパーに保持されることになります。 Acquisition ラッパーが `__of__` メソッドを提供し、 Acquisition を基底とするクラスはこれで行います。

インスタンスの親コンテナを取得する最も一般的な方法は以下の呼び出しです::

  from Acquisition import aq_parent
  
  container = aq_parent(instance)

ILocation インターフェースを実装しているインスタンスの場合は以下の方法です::

  container = instance.__parent__

コンテナ階層の位置を表すための、様々な目的に合わせた、様々な `aq_*` メソッドがあります。今のところ、 Zope 2 の全てのオブジェクトは仮想的に Acquisition クラスに属しています。副作用として、多くの人たちが彼らのオブジェクトに Acquisition ラッパーがあることを期待しています。このため、コードが `aq_*` メソッドに属性としてアクセスできることに依存しています::

  container = instance.aq_parent

既存の全ての API はこれで動作していましたが、 Acquisition は `__parent__` ポインタを使ってオブジェクトが格納されているコンテナにアクセスするようになります。そしてオブジェクトの `__of__` メソッド呼び出しを無条件に呼び出す処理は、今後行われなくなり、まずインターフェースの実装があるかを確認します::

  from Acquisition.interfaces import IAcquirer

  if IAcquirer.providedBy(instance):
      instance = instance.__of__(container)

これは、今後は `aq_*` メソッドがあることを前提としてはいけない、と言うことでもあります。 Zope 2 内の全てのコードはまだこれをサポートしていますが、今後はこれに依存せず、 Acquisition パッケージが提供している機能を使うことになります。

以下は、新旧両方のアプローチの相互作用を理解するための小さな例です::

  >>> class Location(object):
  ...     def __init__(self, name):
  ...         self.__name__ = name
  ...     def __repr__(self):
  ...         return self.__name__

  # Acquisition 型のクラスを作成:

  >>> import Acquisition
  >>> class Implicit(Location, Acquisition.Implicit):
  ...     pass

  # ２つの Implicit のインスタンスを作成:

  >>> root = Implicit('root')
  >>> folder = Implicit('folder')

  # そして２つの Acquisition と関連しないインスタンスを作成:

  >>> container = Location('container')
  >>> item = Location('item')

  # 包含状態を各インスタンスに設定:

  >>> folder = folder.__of__(root)
  >>> container.__parent__ = folder
  >>> item.__parent__ = container

  # 包含状態の連鎖を確認:

  >>> from Acquisition import aq_parent
  >>> aq_parent(container)
  folder

  >>> from Acquisition import aq_chain
  >>> aq_chain(item)
  [item, container, folder, root]

  # 明示的なポインタが Acquisition ラッパーよりも優先される:

  >>> item2 = Implicit('item2')
  >>> item2 = item2.__of__(folder)
  >>> item2.__parent__ = container

  >>> aq_chain(item2)
  [item2, container, folder, root]

もう少し具体的な例として、今までは以下のように実装していました::

  >>> from Acquisition import aq_inner
  >>> from Acquisition import aq_parent
  >>> from Products import Five

  >>> class MyView(Five.browser.BrowserView):
  ...
  ...     def do_something(self):
  ...         container = aq_parent(aq_inner(self.context))

今後は以下のように出来ます::

  >>> import zope.publisher.browser

  >>> class MyView(zope.publisher.browser.BrowserView):
  ...
  ...     def do_something(self):
  ...         container = aq_parent(self.context)

このように zope.publisher の BrowserView は ILocation インターフェースをサポートしており、自動的に動作します。このように親の取得はこれまで同様に動作していますが、今後は Acquisition の仕組みを使うために Acquisition ラッパーを使う必要はなくなりました。このことは、パッケージを再利用したり、自分のコード Zope 2 の外でも再利用できるようにするのに非常に効果があります。


ObjectマネージャとIContainer
------------------------------

Zope 2 の基本的な要素として、 `OFS` パッケージとして実装されているオブジェクトファイルシステムがあります。このパッケージの中心的な要素に `ObjectManager` クラスがあります。これは標準の `Folder` クラスのベースクラスとして使用され、Folderクラスは多くのコンテナ型クラスとして Zope 2 内部で使用されています。

Object マネージャの、オブジェクトにアクセスするための API や、オブジェクトを追加するための API は、書かれてから何年持ったっています。これまでの間、 Python がコンテナ内のオブジェクトにアクセスするための標準的な実装を提供するたびに、協調動作するようにしてきました。こういった Python の API は多くの Zope を扱う開発者たちにとっても、親しみやすいものでした。 Zope コンポーネントライブラリは、そういった API を正式なものとして zope.container パッケージの IContainer インターフェースで規定しました。このバージョンの Zope 2 の標準の OFS ObjectManager は従来の API の他に IContainer インターフェースを完全に実装しています。

 >>> from zope.container.interfaces import IContainer
 >>> from OFS.ObjectManager import ObjectManager
 >>> IContainer.implementedBy(ObjectManager)
 True

このため、コードを書くときに Object マネージャに合わせた実装を行うのではなく、 IContainer を実装した任意のクラスを実装することが出来ます。この変更は、前述の Acquisition の変更と合わせると、とても、既存のパッケージを再利用しやすく、 Zope 2 のために特化した実装をせずに済むようにしてくれます。

ここに、Objectマネージャを使う際の以前の実装例があります::

  >>> from OFS.Folder import Folder
  >>> from OFS.SimpleItem import SimpleItem

  >>> folder = Folder('folder')
  >>> item1 = SimpleItem('item1')
  >>> item2 = SimpleItem('item2')

  >>> result = folder._setObject('item1', item1)
  >>> result = folder._setObject('item2', item2)

  >>> folder.objectIds()
  ['item1', 'item2']

  >>> folder.objectValues()
  [<SimpleItem at folder/>, <SimpleItem at folder/>]

  >>> if folder.hasObject('item2')
  ...     folder._delObject('item2')

このような専用APIを使わずに、今後は以下のように書くことが出来ます::

  >>> from OFS.Folder import Folder
  >>> from OFS.SimpleItem import SimpleItem

  >>> folder = Folder('folder')
  >>> item1 = SimpleItem('item1')
  >>> item2 = SimpleItem('item2')

  >>> folder['item1'] = item1
  >>> folder['item2'] = item2

  >>> folder.keys()
  ['item1', 'item2']

  >>> folder.values()
  [<SimpleItem at folder/>, <SimpleItem at folder/>]

  >>> folder.get('item1')
  <SimpleItem at folder/>

  >>> if 'item2' in folder:
  ...     del folder['item2']

  >>> folder.items()
  [('item1', <SimpleItem at folder/>)]


.. rubric:: (Translated by Shimizukawa, `r105416 <http://svn.zope.org/Zope/tags/2.12.1/doc/WHATSNEW.rst?rev=105416&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/WHATSNEW.html>`_)
  :class: translator

