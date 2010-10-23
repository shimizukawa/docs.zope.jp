How to build and install Zope from source code on Windows.
----------------------------------------------------------

* Ensure you have the correct MSVC version installed for the
  version of Python you will be using.

* Install (or build from sources) Python
  http://www.python.org

* Install (or build from sources) the Python for Windows extensions
  http://sourceforge.net/projects/pywin32/

* Unpack the Zope source distribution. Change to that directory.

* Execute:
  % python.exe inst\configure.py
  It should say something like:
  >
  > - Zope top-level binary directory will be c:\Zope-2.12.
  > - Makefile written.
  >
  > Next, run the Visual C++ batch file "VCVARS32.bat" and then "nmake".

  (run 'configure.py --help' to see how to change things)

* 'makefile' will have ben created.  As instructed, execute 'nmake'.  
  If the build succeeds, the last message printed should be:
  > Zope built.  Next, do 'nmake install'.

* As instructed, execute 'nmake install'.  A few warnings will be generated, 
  but they can be ignored.  The last message in the build process should be:
  > Zope binaries installed successfully.

* Zope itself has now been installed.  We need to create an instance.  Run:
  % python.exe {install_path}\bin\mkzopeinstance.py
  
  We will be prompted, via the console, for the instance directory and 
  username/password for the admin user.

* We are now ready to start zope.  Run:
  % {zope_instance}\bin\runzope.bat
  Zope should start with nice log messages being printed to
  stdout.  When Zope is ready, you should see:
  > ------
  > 2004-10-13T12:27:58 INFO(0) Zope Ready to handle requests
  
  Press Ctrl+C to stop this instance of the server.

* Optionally, install as a Windows service.  Execute:
  % python {zope_instance}\bin\zopeservice.py
  to see the valid options.  You may want something like:
  % python {zope_instance}\bin\zopeservice.py --startup=auto install

  Once installed, it can be started any number of ways:
  - % {zope_instance}\bin\zopectl.bat start
  - % python {zope_instance}\bin\zopeservice.py start
  - Control Panel
  - % net start service_short_name (eg, `net start Zope_-1227678699`)
