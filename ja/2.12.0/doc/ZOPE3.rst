Zope 2 アプリケーション内でのZopeコンポーネント利用
====================================================

背景
-----

Zope 3 は Zope コミュニティー発の、 Web 開発を目的とした別プロジェクトです。より 'プログラマ中心' にデザインされ、学習が容易で、プログラマによって使われ拡張されています。 Zope 3 にはインターフェースを中核としたコンポーネントアーキテクチャが採用され、これによって開発者が Zope のフレームワーク全体を把握していなくても、開発およびデプロイが容易になっています。

Zope 2.8 で Zope コアに "Five" プロジェクトが統合されました。 "Five" プロジェクトは Zope 3 のコンポーネントとパターンを既存の、そして新しい Zope 2 アプリケーションから使用するために導入された互換レイヤです。

特徴
-----

Five 統合レイヤは Zope 3 のインターフェース、 Zope Configuration Markup Language (ZCML) 、アダプター、ビュー、ユーティリティー、そしてスキーマによるコンテンツ型を提供します。

注意点として、 Five レイヤは Zope 3 コンポーネントの ZMI ユーザーインターフェースを提供 *しません* 。

Zope 2 には Zope 3 のパッケージの要素が同梱されており、別途 Zope 3 をインストールする必要はありません。


.. rubric:: (Translated by Shimizukawa, `r104646 <http://svn.zope.org/Zope/tags/2.12.0/doc/ZOPE3.rst?rev=104646&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/ZOPE3.html>`_)
  :class: translator

