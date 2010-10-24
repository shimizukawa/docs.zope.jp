Changelog
=========

このファイルには現在の Zope リリースでの変更点を記載しています。
これ以前の変更については http://docs.zope.jp/zope2/releases/
を参照してください。

2.14.0a1 (unreleased)
---------------------

Bugs Fixed
++++++++++


Features Added
++++++++++++++


Restructuring
+++++++++++++

- 各種パッケージへの直接的な依存を無くし、各パッケージをZope2パッケージの外
  に分割しました。あなたのアプリケーションが以下のパッケージを利用するには、
  配布時の設定で依存の記述を行って下さい: ``Products.BTreeFolder2``,
  ``Products.ExternalMethod``, ``Products.MailHost``, ``Products.MIMETools``,
  ``Products.PythonScripts``, ``Products.StandardCacheManagers``.


.. rubric:: (Translated by Shimizukawa, `r117404 <http://svn.zope.org/Zope/trunk/doc/CHANGES.rst?rev=117404&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.14/CHANGES.html>`_)
  :class: translator

