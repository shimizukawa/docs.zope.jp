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

The short technical version of this change is: "Acquisition has been made aware
of __parent__ pointers". What sounds like a small change is actually a major
step in the integration story for Zope components based technology into Zope 2.

While integrating the Zope component architecture and its many concepts into
Zope 2 an integration layer called Five (Zope 2 + 3) has been created. One of
the major reasons for the necessity of an integration layer has been in the way
Zope 2 was tightly coupled to the concept of Acquisition. The entire security
machinery, object traversal and publication has been relying on this.

All classes, which wanted to interact with Zope 2 in any non-trivial way, had
to inherit from the Acquisition base classes. As a result almost no external
package could directly work inside Zope 2 but required an integration layer.

With this version of Zope 2, objects have a new option of providing location
awareness to Zope APIs. This new option is to provide an explicit parent
pointer in the ``__parent__`` attribute, much like specified by the ILocation
API from `zope.location <http://pypi.python.org/pypi/zope.location>`_. Browser
views and other location-dependent components implement ILocation already.

Classes adhering to this convention need to get `__parent__` pointers set to
their container object, when being put into the container. Code that operates
on such objects can then walk up the containment hierarchy by following the
pointers. In Acquisition based classes no information would be stored on the
objects, but Acquisition wrappers are constructed around the objects instead.
Only those wrappers would hold the container references. The Acquisition
wrapping relies on the objects to provide an `__of__` method as done by the
Acquisition base classes.

The most common way of getting the container of an instance is to call::

  from Acquisition import aq_parent
  
  container = aq_parent(instance)

For instances providing the ILocation interface the common way is::

  container = instance.__parent__

There are various `aq_*` methods available for various other tasks related to
locating objects in the containment hierarchy. So far virtually all objects in
Zope 2 would participate in Acquisition. As a side-effect many people relied on
Acquisition wrappers to be found around their objects. This caused code to rely
on accessing the `aq_*` methods as attributes of the wrapper::

  container = instance.aq_parent

While all the existing API's still work as before, Acquisition now respects
`__parent__` pointers to find the container for an object. It will also not
unconditionally try to call the `__of__` method of objects anymore, but protect
it with a proper interface check::

  from Acquisition.interfaces import IAcquirer

  if IAcquirer.providedBy(instance):
      instance = instance.__of__(container)

In addition to this check you should no longer rely on the `aq_*` methods to be
available as attributes. While all code inside Zope 2 itself still supports
this, it does no longer rely on those but makes proper use of the functions
provided by the Acquisition package.

To understand the interaction between the new and old approach here is a
little example::

  >>> class Location(object):
  ...     def __init__(self, name):
  ...         self.__name__ = name
  ...     def __repr__(self):
  ...         return self.__name__

  # Create an Acquisition variant of the class:

  >>> import Acquisition
  >>> class Implicit(Location, Acquisition.Implicit):
  ...     pass

  # Create two implicit instances:

  >>> root = Implicit('root')
  >>> folder = Implicit('folder')

  # And two new Acquisition-free instances:

  >>> container = Location('container')
  >>> item = Location('item')

  # Provide the containment hints:

  >>> folder = folder.__of__(root)
  >>> container.__parent__ = folder
  >>> item.__parent__ = container

  # Test the containtment chain:

  >>> from Acquisition import aq_parent
  >>> aq_parent(container)
  folder

  >>> from Acquisition import aq_chain
  >>> aq_chain(item)
  [item, container, folder, root]

  # Explicit pointers take precedence over Acquisition wrappers:

  >>> item2 = Implicit('item2')
  >>> item2 = item2.__of__(folder)
  >>> item2.__parent__ = container

  >>> aq_chain(item2)
  [item2, container, folder, root]

For a less abstract example, you so far had to do::

  >>> from Acquisition import aq_inner
  >>> from Acquisition import aq_parent
  >>> from Products import Five

  >>> class MyView(Five.browser.BrowserView):
  ...
  ...     def do_something(self):
  ...         container = aq_parent(aq_inner(self.context))

Instead you can now do::

  >>> import zope.publisher.browser

  >>> class MyView(zope.publisher.browser.BrowserView):
  ...
  ...     def do_something(self):
  ...         container = aq_parent(self.context)

As the zope.publisher BrowserView supports the ILocation interface, all of this
works automatically. A view considers its context as its parent as before, but
no longer needs Acquisition wrapping for the Acquisition machinery to
understand this. The next time you want to use a package or make your own code
more reusable outside of Zope 2, this should be of tremendous help.


ObjectマネージャとIContainer
------------------------------

One of the fundamental parts of Zope 2 is the object file system as implemented
in the `OFS` package. A central part of this package is an underlying class
called `ObjectManager`. It is a base class of the standard `Folder` used
for many container-ish classes inside Zope 2.

The API to access objects in an object manager or to add objects to one has
been written many years ago. Since those times Python itself has gotten
standard ways to access objects in containers and work with them. Those Python
API's are most familiar to most developers working with Zope. The Zope
components libraries have formalized those API's into the general IContainer
interface in the zope.container package. In this version of Zope 2 the standard
OFS ObjectManager fully implements this IContainer interface in addition to its
old API.

 >>> from zope.container.interfaces import IContainer
 >>> from OFS.ObjectManager import ObjectManager
 >>> IContainer.implementedBy(ObjectManager)
 True

You can now write your code in a way that no longer ties it to object managers
alone, but can support any class implementing IContainer instead. In
conjunction with the Acquisition changes above, this will increase your chances
of being able to reuse existing packages not specifically written for Zope 2 in
a major way.

Here's an example of how you did work with object managers before::

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

Instead of this special API, you can now use::

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

