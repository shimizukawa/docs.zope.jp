Zope 開発者ガイド
======================

背景
----------

"Zope 開発者ガイド" は長い間メンテナンスされていませんでした。
これは "Zope 開発者ガイド" を生き返らせようという試みです。

技術
----------

今回、オリジナルのコンテンツを `Sphinx`_ ベースに切り替えました。

Sphinx をインストールするには単純に以下のようにします::

  easy_install Sphinx

もしグローバルなPython環境に色々入れたくないのであれば、 `virtualenv`_
を使うこともできます。

Sphinx は一つのソースからいろいろな種類のフォーマットを出力する
事が出来ます。HTMLを生成したいのであれば以下のようにします::

  make html

これが Windows でも動作するかはわかりません。

.. note:: 訳注

  Windows 標準では make コマンドがないため、
  ``sphinx-build -b html -d build/doctrees source build/html``
  のようにsphinxコマンドを直接使用してください。コマンドのオプション等
  については Makefile を参照してください。

連絡
-------

この本についての最も良い議論の場は以下です:

http://mail.zope.org/mailman/listinfo/zope-dev


.. _Sphinx: http://sphinx.pocoo.org/
.. _virtualenv: http://pypi.python.org/pypi/virtualenv/


TODO
----

- テストとデバッグの章を分割する
- exampleに `始めよう` のコードを追加する


.. rubric:: (Translated by Shimizukawa, `r104830 <http://svn.zope.org/zope2docs/trunk/zdgbook/README.txt?rev=104830&view=markup>`_)
  :class: translator

