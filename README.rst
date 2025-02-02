fake-useragent
==============

:info: Up to date simple useragent faker with real world database

.. text:: Original Repository
    :target: https://github.com/hellysmile/fake-useragent


Features
********

* grabs up to date ``useragent`` from `useragentstring.com <http://useragentstring.com/>`_
* randomize with real world statistic via `w3schools.com <https://www.w3schools.com/browsers/browsers_stats.asp>`_

Installation
------------

.. code-block:: shell

    pip install fake-useragent

Usage
-----

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent()

    ua.ie
    # Mozilla/5.0 (Windows; U; MSIE 9.0; Windows NT 9.0; en-US);
    ua.msie
    # Mozilla/5.0 (compatible; MSIE 10.0; Macintosh; Intel Mac OS X 10_7_3; Trident/6.0)'
    ua['Internet Explorer']
    # Mozilla/5.0 (compatible; MSIE 8.0; Windows NT 6.1; Trident/4.0; GTB7.4; InfoPath.2; SV1; .NET CLR 3.3.69573; WOW64; en-US)
    ua.opera
    # Opera/9.80 (X11; Linux i686; U; ru) Presto/2.8.131 Version/11.11
    ua.chrome
    # Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.2 (KHTML, like Gecko) Chrome/22.0.1216.0 Safari/537.2'
    ua.google
    # Mozilla/5.0 (Macintosh; Intel Mac OS X 10_7_4) AppleWebKit/537.13 (KHTML, like Gecko) Chrome/24.0.1290.1 Safari/537.13
    ua['google chrome']
    # Mozilla/5.0 (X11; CrOS i686 2268.111.0) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.57 Safari/536.11
    ua.firefox
    # Mozilla/5.0 (Windows NT 6.2; Win64; x64; rv:16.0.1) Gecko/20121011 Firefox/16.0.1
    ua.ff
    # Mozilla/5.0 (X11; Ubuntu; Linux i686; rv:15.0) Gecko/20100101 Firefox/15.0.1
    ua.safari
    # Mozilla/5.0 (iPad; CPU OS 6_0 like Mac OS X) AppleWebKit/536.26 (KHTML, like Gecko) Version/6.0 Mobile/10A5355d Safari/8536.25

    # and the best one, random via real world browser usage statistic
    ua.random

Notes
-----

``fake-useragent`` store collected data at your os temp dir, like ``/tmp``

If You want to update saved database just:

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent()
    ua.update()

If You don't want cache database or no writable file system:

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent(cache=False)

Sometimes, `useragentstring.com <http://useragentstring.com/>`_ or `w3schools.com <https://www.w3schools.com/browsers/browsers_stats.asp>`_ changes their html, or down, in such case
``fake-useragent`` uses `heroku <https://fake-useragent.herokuapp.com/browsers/0.1.8>`_ fallback

If You don't want to use hosted cache server (version 0.1.5 added)

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent(use_cache_server=False)

In very rare case, if hosted cache server and sources will be
unavailable ``fake-useragent`` wont be able to download data: (version 0.1.3 added)

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent()

    # Traceback (most recent call last):
    #   ...
    # fake_useragent.errors.FakeUserAgentError

    # You can catch it via

    from fake_useragent import FakeUserAgentError

    try:
        ua = UserAgent()
    except FakeUserAgentError:
        pass

If You will try to get unknown browser: (version 0.1.3 changed)

.. code-block:: python

    from fake_useragent import UserAgent
    ua = UserAgent()
    ua.best_browser
    # Traceback (most recent call last):
    #   ...
    # fake_useragent.errors.FakeUserAgentError

You can completely disable ANY annoying exception with adding ``fallback``: (version 0.1.4 added)

.. code-block:: python

    import fake_useragent

    ua = fake_useragent.UserAgent(fallback='Your favorite Browser')
    # in case if something went wrong, one more time it is REALLY!!! rare case
    ua.random == 'Your favorite Browser'

Want to control location of data file? (version 0.1.4 added)

.. code-block:: python

    import fake_useragent

    # I am STRONGLY!!! recommend to use version suffix
    location = '/home/user/fake_useragent%s.json' % fake_useragent.VERSION

    ua = fake_useragent.UserAgent(path=location)
    ua.random

If you need to safe some attributes from overriding them in UserAgent by ``__getattr__`` method
use ``safe_attrs`` you can pass there attributes names.
At least this will prevent you from raising FakeUserAgentError when attribute not found.

For example, when using fake_useragent with `injections <https://github.com/tailhook/injections>`_ you need to:

.. code-block:: python

    import fake_useragent

    ua = fake_useragent.UserAgent(safe_attrs=('__injections__',))

Please, do not use if you don't understand why you need this.
This is magic for rarely extreme case.

Experiencing issues???
----------------------

Make sure that You using latest version!!!

.. code-block:: shell

    pip install -U fake-useragent

Check version via python console: (version 0.1.4 added)

.. code-block:: python

    import fake_useragent

    print(fake_useragent.VERSION)

And You are always welcome to post `issues <https://github.com/hellysmile/fake-useragent/issues>`_

Please do not forget mention version that You are using

Tests
-----

.. code-block:: console

    pip install tox
    tox

Changelog
---------

* 0.1.11 October 4, 2018
    - moved `s3 + cloudfront` fallback to `heroku.com`, cuz someone from Florida did ~25M requests last month

* 0.1.10 February 11, 2018
    - Minor fix docs `cloudfront` url

* 0.1.9 February 11, 2018
    - fix ``w3schools.com`` renamed `IE/Edge` to `Edge/IE`
    - moved `heroku.com` fallback to `s3 + cloudfront`
    - stop testing Python3.3 and pypy

* 0.1.8 November 2, 2017
    - fix ``useragentstring.com`` ``Can't connect to local MySQL server through socket``

* 0.1.7 April 2, 2017
    - fix broken README.rst

* 0.1.6 April 2, 2017
    - fixes bug ``use_cache_server`` do not affected anything
    - `w3schools.com <https://www.w3schools.com/browsers/browsers_stats.asp>`_ moved to ``https``
    - ``verify_ssl`` options added, by default it is ``True`` (``urllib.urlopen`` ssl context for Python 2.7.9- and 3.4.3- is not supported)

* 0.1.5 February 28, 2017
    - added ``ua.edge`` alias to Internet Explorer
    - w3schools.com starts displaying ``Edge`` statistic
    - Python 2.6 is not tested anymore
    - ``use_cache_server`` option added
    - Increased ``fake_useragent.settings.HTTP_TIMEOUT`` to 5 seconds

* 0.1.4 December 14, 2016
    - Added custom data file location support
    - Added ``fallback`` browser support, in case of unavailable data sources
    - Added alias ``fake_useragent.FakeUserAgent`` for ``fake_useragent.UserAgent``
    - Added alias ``fake_useragent.UserAgentError`` for ``fake_useragent.FakeUserAgentError``
    - Reduced ``fake_useragent.settings.HTTP_TIMEOUT`` to 3 seconds
    - Started migration to new data file format
    - Simplified a lot 4+ years out of date code
    - Better thread/greenlet safety
    - Added verbose logging
    - Added ``safe_attrs`` for prevent overriding by ``__getattr__``

* 0.1.3 November 24, 2016
    - Added hosted data file, when remote services is unavailable
    - Raises ``fake_useragent.errors.FakeUserAgentError`` in case when there is not way to download data
    - Raises ``fake_useragent.errors.FakeUserAgentError`` instead of ``None`` in case of unknown browser
    - Added ``gevent.sleep`` support in ``gevent`` patched environment when trying to download data

* X.X.X xxxxxxx xx, xxxx
    - xxxxx ?????

Authors
-------

You can visit `authors page <https://github.com/hellysmile/fake-useragent/blob/master/AUTHORS>`_
