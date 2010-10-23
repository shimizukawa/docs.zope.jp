WindowsでZopeをソースからビルドしインストールする方法
--------------------------------------------------------

* 使用する Python で使われているものと同じ MSVC バージョンをインストールします。

* Python をインストールします(あるいはソースからビルドします):

  http://www.python.org

* Python for Windows extensions をインストールします(あるいはソースからビルドします):

  http://sourceforge.net/projects/pywin32/

* ソース配布版の Zope を展開し、そのディレクトリへ移動します。

* 実行::

    % python.exe inst\configure.py

  これにより以下のように表示されます::

    > - Zope top-level binary directory will be c:\Zope-2.12.
    > - Makefile written.
    >
    > Next, run the Visual C++ batch file "VCVARS32.bat" and then "nmake".

  ('configure.py --help' とすると他の引数を確認出来ます)

* 'makefile' が作成されます。指示されているように、 'nmake' を実行します。  
  ビルドが成功すれば、以下のようにメッセージが表示されます::

    > Zope built.  Next, do 'nmake install'.

* 指示されているように、 'nmake install' を実行します。
  いくつかの警告が表示されるかもしれませんが、無視してかまいません。
  ビルドが成功すれば、以下のようにメッセージが表示されます::

    > Zope binaries installed successfully.

* Zope がインストールされました。次はインスタンスを作成する必要があります。
  次のように実行してください::

    % python.exe {install_path}\bin\mkzopeinstance.py
  
  コンソールにプロンプトが表示され、インスタンスのディレクトリや、
  管理者のユーザー名/パスワードなどの入力が求められます。

* Zope を起動する準備が出来ました。次のように実行してください::

    % {zope_instance}\bin\runzope.bat

  Zope が起動し、ログメッセージを標準出力に表示します。
  Zope の起動処理が完了すれば以下のようなメッセージが表示されます::

    > ------
    > 2004-10-13T12:27:58 INFO(0) Zope Ready to handle requests
  
  Ctrl+C を押してサーバーを停止することが出来ます。

* Windows サービスとして登録する事も出来ます。
  次のように実行してください::

    % python {zope_instance}\bin\zopeservice.py

  指定するべきオプションが表示されます。例えば以下のように入力してください::

    % python {zope_instance}\bin\zopeservice.py --startup=auto install

  サービス登録が完了しました。起動にはいくつかの異なる方法があります:

  - % {zope_instance}\bin\zopectl.bat start
  - % python {zope_instance}\bin\zopeservice.py start
  - Control Panel
  - % net start service_short_name (例: `net start Zope_-1227678699`)

.. rubric:: (Translated by Shimizukawa, `r104646 <http://svn.zope.org/Zope/tags/2.12.0/doc/WINDOWS.rst?rev=104646&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.12/WINDOWS.html>`_)
  :class: translator

