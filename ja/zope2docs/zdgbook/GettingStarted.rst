###############
始めよう
###############

はじめに
============

本章では、インストールと簡単なアプリケーションを題材とした、開発の
始め方についてカバーします。
このガイドでは、 `Buildout <http://www.buildout.org>`_ という
ビルドシステムを用いて、アプリケーションのビルドを行います。
また、アプリケーションの一部として開発された Python パッケージ
は `Python eggs <http://peak.telecommunity.com/DevCenter/setuptools>`_
という形式で配布することが出来ます。


ディレクトリ構成
===================

アプリケーション開発を始めるに当たり、 Python パッケージを配置する
ディレクトリや、ビルド関係のファイルが置かれるディレクトリを
作成します。

::

  $ mkdir poll
  $ mkdir poll/poll_build
  $ mkdir poll/poll.main

ビルド関連の全てのファイルは `poll_build` ディレクトリに格納されます。
主となる Python パッケージは `poll.main` ディレクトリに格納されます。
``poll`` は名前空間パッケージとして使うことが出来、これには setuptools
に含まれる `pkg_resources` モジュールが提供する機能を使用します。


ビルド環境を整える
======================

まずシステムにPython 2.5 か 2.6 がインストールされている必要があります。
ビルドを始めるには、 `bootstrap.py` をダウンロードして実行します。
`bootstrap.py` によって、 `setuptools` と `zc.buildout` パッケージが
ダウンロード & インストールされます。
また同時に、いくつかのディレクトリが作成され、 `bin` ディレクトリに
`buildout` スクリプトが生成されます。

::

  $ cd poll/poll_build
  $ touch buildout.cfg
  $ wget -c http://svn.zope.org/repos/main/zc.buildout/trunk/bootstrap/bootstrap.py
  $ python2.6 bootstrap.py

Zope 2 のインストール
======================

Zope 2.12 から Zope 2 の配布が egg 形式になりました。
Zope 2 egg のインストールとインスタンスの作成のために、 buildout
設定ファイル (``buildout.cfg``) の各パーツととレシピを以下のように
設定します。

::

  [buildout]
  parts = zope2
          instance
  extends = http://download.zope.org/Zope2/index/2.12.0/versions.cfg

  [zope2]
  recipe = zc.recipe.egg
  eggs = Zope2
  interpreter = zopepy

  [instance]
  recipe = plone.recipe.zope2instance
  user = admin:admin
  http-address = 8080
  eggs = ${zope2:eggs}

``[zope2]`` パートでは `zc.recipe.egg` を用いて `Zope2` egg と
それらが依存している egg をダウンロードします。また同時に、
いくつかのコンソールスクリプトが `bin` ディレクトリに作られます。
コンソールスクリプトには ``zopepy`` という名前のカスタム Python
インタプリタも作成されます。

``[instance]`` パートでは、アプリケーション開発のための Zope 2
アプリケーションインスタンスが作成されます。また同時に、
``instance`` スクリプトが `bin` ディレクトリに作られます。
このスクリプトを使ってアプリケーションインスタンスを実行する
事が出来ます。

buildout 設定ファイルを更新したら、 `buildout` コマンドを実行して
ビルドを始めてください。

::

  $ ./bin/buildout

最初のビルドは、完了するまでいくらか時間がかかります。

インスタンスの実行
===================

ビルドが完了したら、 Zope 2 インスタンスを以下のようにして起動します。

::

  $ ./bin/instance fg


画面に、 Zope が 8080 ポートで起動している旨が表示されます。
Zope 管理画面 (Zope Management Interface = ZMI) にアクセスしてください。

::

  http://localhost:8080/manage

`[instance]` パートに上記ページにアクセスする際のユーザー名とパスワード
の設定があります。

インストール出来るアプリケーションの一覧をドロップダウンメニューで
見ることが出来ます。あるいは、 "Control_Panel" -> "Products" と
辿ることでも確認出来ます。

::

  http://localhost:8080/Control_Panel/Products/manage_main

次の節で、 `poll.main` を作成すればここに表示されるようになります。
さらに節が進めば、それをインストール出来るようになるでしょう。


メインパッケージの開発
===========================

それでは `poll.main` パッケージの開発に移りましょう。
全てのアプリケーションを `poll.main` パッケージ内で開発することも
出来ますが、パッケージは論理的な単位、かつ依存関係などの
保守のしやすさを目安に分割することをお勧めします。

::

  $ cd ../poll.main

ここでまた、基本的なディレクトリ構造の作成と、 egg で配布するための
`setup.py` を作成する必要があります。 Python パッケージは
`src` ディレクトリに置く事にします。

::

  $ touch setup.py
  $ mkdir src
  $ mkdir src/poll
  $ mkdir src/poll/main
  $ touch src/poll/__init__.py
  $ touch src/poll/main/__init__.py
  $ touch src/poll/main/configure.zcml

上記の最後のファイルは Zope Configuration Markup Language (ZCML)
と呼ばれる設定ファイルです。すこししたら、いくつかの定型的な
コードを ZCML ファイルに記載することになります。

`poll` は名前空間パッケージとして定義されます。 `src/poll/__init__.py`
には、名前空間としての定型コードとして以下の内容を記載してください。

::

  __import__('pkg_resources').declare_namespace(__name__)

次に、 `setup.py` に最低限のパッケージ情報を記載します。

::

  from setuptools import setup, find_packages

  setup(
      name="poll.main",
      version="0.1",
      packages=find_packages("src"),
      package_dir={"": "src"},
      namespace_packages=["poll"],
      install_requires=["setuptools",
                        "Zope2"],
      )

Zope から認識されるようにするため、2つのファイルを追加します。
まずは `src/poll/main/__init__.py` に以下のコールバック関数を
定義します。

::

  def initialize(registrar):
      pass

そして、 ZCML ファイルに以下の行を追加します。

::

  <configure
      xmlns="http://namespaces.zope.org/five">

      <registerPackage package="." initialize=".initialize" />

  </configure>

インストール出来るアプリケーションの作成
==========================================

インストール出来るアプリケーションを作成するには、以下の3つが必要です。

- オブジェクト作成時に使用されるフォーム用の ZPT (manage_addPollMain)
- フォームのアクションを定義する関数 (addPollMain)
- 最上位のアプリケーションオブジェクトを定義するクラス (PollMain)

そして、 `initialize` 関数に渡される `register` オブジェクトを用いて、
これらのクラスと関数を登録する必要があります。

これら全てを `app.py` で定義し、フォームのテンプレートは
`manage_addPollMain_form.zpt` として用意します。

::

  $ touch src/poll/main/app.py
  $ touch src/poll/main/manage_addPollMain_form.zpt

`app.py` のコードは以下のようになります。

::

  from OFS.Folder import Folder
  from Products.PageTemplates.PageTemplateFile import PageTemplateFile

  class PollMain(Folder):
      meta_type = "POLL"

  manage_addPollMain = PageTemplateFile("manage_addPollMain_form", globals())

  def addPollMain(context, id):
      """ """
      context._setObject(id, PollMain(id))
      return "POLL Installed: %s" % id

`manage_addPollMain_form.zpt` は以下のようになります。

::

  <html xmlns="http://www.w3.org/1999/xhtml"
        xmlns:tal="http://xml.zope.org/namespaces/tal">
    <body>

      <h2>Add POLL</h2>
      <form action="addPollMain" method="post">
        Id: <input type="text" name="id" /><br />
        Title: <input type="text" name="title" /><br />
        <input type="submit" value="Add" />
      </form>
    </body>
  </html>

最後に、これらを登録するために `__init__.py` を以下のように更新します::

  from poll.main.app import PollMain, manage_addPollMain, addPollMain

  def initialize(registrar):
      registrar.registerClass(PollMain,
                              constructors=(manage_addPollMain, addPollMain))

これでアプリケーションはインストール出来るようになりました。しかし、
Zope 2 パッケージとして認識されるようにするためには、まだ `poll_build`
にいくつかの変更を行う必要があります。

poll.main をビルド対象に追加
==============================

まず `[buildout]` パートに `poll.main` がローカルで開発されている
ものだということを表記します。これを書かないと、 buildout は
パッケージをインデックスサーバー (標準では http://pypi.python.org/pypi)
から取得しようとするでしょう。

::

  [buildout]
  develop = ../poll.main
  ...

また、 `poll.main` egg を `[zope2]` パートの `eggs` オプションに追加
してください。

::

  ...
  eggs = Zope2
         poll.main
  ...

最後に、 ZCML ファイルを含めるように指示する新しいオプションを追加します。
これによって、 Zope はこのパッケージを認識できるようになります。

::

  ...
  zcml = poll.main

最終的に `buildout.cfg` は以下のようになりました。

::

  [buildout]
  develop = ../poll.main
  parts = zope2
          instance

  [zope2]
  recipe = zc.recipe.egg
  eggs = Zope2
         poll.main
  interpreter = zopepy

  [instance]
  recipe = plone.recipe.zope2instance
  user = admin:admin
  http-address = 8080
  eggs = ${zope2:eggs}
  zcml = poll.main

この変更を有効にするために、 buildout を再度実行してください。

::

  $ ./bin/buildout

そしてアプリケーションインスタンスを再度実行してください。

::

  $ ./bin/instance fg

アプリケーションインスタンスの追加
===================================

ZMI を開いて、 `POLL` をドロップダウンメニューから選ぶと、先ほど作成
した追加フォームが画面に表示されます。 ID に `poll` を指定してフォーム
を送信してみてください。画面に "POLL Installed: poll" という
メッセージが表示されると思います。


POLL のメインページを追加する
==============================

この節では、 POLL アプリケーションのメインページを追加してみましょう。
POLL アプリケーションには以下のURLでアクセス出来ます:
http://localhost:8080/poll

まず、 `index_html.zpt` というファイルを `src/poll/main` ディレクトリに
以下の内容で作成します::

  <html>
  <head>
    <title>Welcome to POLL!</title>
  </head>
  <body>

  <h2>Welcome to POLL!</h2>

  </body>
  </html>

そして `index_html` という属性を PollMain クラスに以下のように追加します::

  class PollMain(Folder):
      meta_type = "POLL"

      index_html = PageTemplateFile("index_html", globals())

Zope を再起動してください。追加したメインページを以下のURLで確認する
事が出来ます: http://localhost:8080/poll

まとめ
=======

本章では、インストールと、 Zope 2 での簡単なプロジェクトの始め方について
説明しました。

.. rubric:: (Translated by Shimizukawa, `r104989 <http://svn.zope.org/zope2docs/trunk/zdgbook/GettingStarted.rst?rev=104989&view=markup>`_, `original-site <http://docs.zope.org/zope2/zdgbook/GettingStarted.html>`_)
  :class: translator

