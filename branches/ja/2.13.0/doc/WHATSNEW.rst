Zope 2.13 の新機能
====================

このページでは、このバージョンの Zope 2 の新しい高度な機能と変更点について
説明します。

新機能やバグ修正の細目については、 `変更の詳細 <CHANGES.html>`_
を参照してください。


ZODB 3.10
---------

This version of Zope includes ZODB 3.10 - a new major version of the ZODB.
Among the notable changes are a variety of performance improvements. The ZEO
server process is now multi-threaded. If the underlying file system and disk
storage can handle concurrent disk I/O efficiently a throughput increase by a
factor of up to four has been seen. On a related note using solid state disks
for the ZEO server has a similar effect and can increase throughput by the
same factor. Both of these effects combined can lead to an increase of up to
sixteen times the throughput in high load scenarios.

File storage indexes use a new format, which is both smaller in size and can
be read much faster. The repozo backup script now also backs up the index files
in addition to the actual data, so in a restore scenario the index doesn't have
to be recreated. For large databases this can bring down the total downtime in
a restore scenario by a significant amount of time.

The ZODB has added support for wrapper storages that transform pickle data.
Applications for this include compression and encryption. A storage using
standard zlib compression is available as a new package called
`zc.zlibstorage <http://pypi.python.org/pypi/zc.zlibstorage>`_. In content
management scenarios where strings constitute the most of the non-blob data,
this can reduce the Data.fs size by a factor of two or more. The overhead of
compressing and uncompressing is negligible. This saves both network I/O and
disk space. More importantly the database has better chances of fitting into
the operating systems disk cache and thus into memory. The second advantage is
less important when using solid state disks.

Databases now warn when committing very large records (> 16MB). This is to try
to warn people of likely design mistakes. There is a new option
(large_record_size/large-record-size) to control the record size at which the
warning is issued. This should help developers to better understand the storage
implications of their code, which has been rather transparent so far.

The mkzeoinst script has been moved to a separate project
`zope.mkzeoinstance <http://pypi.python.org/pypi/zope.mkzeoinstance>`_ and is
no-longer included with ZODB. You will need to use this new package to set up
ZEO servers or use the
`plone.recipe.zeoserver <http://pypi.python.org/pypi/plone.recipe.zeoserver>`_
recipe if you use `buildout <http://www.buildout.org/>`_.

More information can be found in the detailed
`change log <http://pypi.python.org/pypi/ZODB3/3.10.0b1.>`_.


WSGI
----

This Zope release comes with native WSGI support. First pioneered in the
repoze.zope2 project, this capability finally found its way back into the core
and obsoletes the externally managed project. With WSGI Zope 2 can natively talk
to a variety of web servers and isn't restricted to its own ZServer anymore. It
also opens up new possibilities for writing or reusing middleware in Zope 2 or
factoring out capabilities into WSGI endware. It's expected that this new
deployment model will over time become the default and the old ZServer
implementation will be deprecated. There's no concrete timeline for this yet.

NOTE: There's no setup documentation nor streamlined instance creation logic
for a WSGI setup yet. This will be provided in a later alpha release.


Zope Toolkit
------------

Zope 2.13 has neither direct nor indirect ``zope.app.*`` dependencies anymore.
This finishes the transition from the hybrid Zope 2 + 3 codebase. Zope 3 itself
has been split up into two projects, the underlying Zope Toolkit consisting of
foundation libraries and the application server part. The application server
part has been renamed BlueBream. Zope 2 only depends and ships with the Zope
Toolkit now.

Large parts of code inside Zope 2 and specifically Products.Five have been
refactored to match this new reality. The goal is to finally remove the Five
integration layer and make the Zope Toolkit a normal integral part of Zope 2.


リファクタリング
----------------

There's an ongoing effort to refactor Zope 2 into more independent modularized
distributions. Zope 2.12 has already seen a lot of this, with the use of zope.*
packages as individual distributions and the extraction of packages like
Acquisition, DateTime or tempstorage to name a few. Zope 2.13 continues this
trend and has moved all packages containing C extensions to external
distributions. Among those are AccessControl, DocumentTemplate and
Products.ZCTextIndex.


Formlibサポートのオプション化
------------------------------

Zope 2 made a number of frameworks available through its integration layer
Products.Five. Among these has been direct support for an automated form
generation framework called zope.formlib with its accompanying widget library
zope.app.form.

This form generation framework has seen only minor adoption throughout the Zope
community and more popular alternatives like z3c.form exist. To reflect this
status Zope 2 no longer directly contains formlib support.

If you rely on formlib, you need to add a dependency to the new five.formlib
distribution and change all related imports pointing to Products.Five.form or
Products.Five.formlib to point to the new package instead.

In order to ease the transition, five.formlib has been backported to the 2.12
release series. Starting in 2.12.3 you can already use the new five.formlib
package, but backwards compatibility imports are left in place in Products.Five.
This allows you to easily adopt your packages to work with both 2.12 and 2.13.


.. rubric:: (Translated by Shimizukawa, `r113828 <http://svn.zope.org/Zope/branches/2.13/doc/WHATSNEW.rst?rev=113828&view=markup>`_, `original-site <http://docs.zope.org/zope2/releases/2.13/WHATSNEW.html>`_)
  :class: translator

