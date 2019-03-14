============
Adding Tests
============

Generally, tests live near the code they are testing, however Mozmill tests live in two
particular directories.

This document doesn't cover actually writing tests, for that see these MDN pages:

- `Writing xpcshell-based unit tests`__
- `Mochitest`__

(Just note that they're Firefox-centric and include some ancient ideas and practices.)

__ https://developer.mozilla.org/en-US/docs/Mozilla/QA/Writing_xpcshell-based_unit_tests
__ https://developer.mozilla.org/en-US/docs/Mozilla/Projects/Mochitest

XPCShell & Mochitest
====================

Tests should be added to a directory near the code they are located. For example, code in
``mail/components/extensions`` is tested by tests in ``mail/components/extensions/test``. Inside
the ``test`` directory is a subdirectory named after the type of test: ``browser`` for Mozmill
tests (as in Firefox terms they are "browser-chrome" Mozmill tests), and ``xpcshell`` or ``unit``
for XPCShell tests.

A new directory needs some standard files: an ESLint configuration file if the directory is linted
(and it should be), and a test manifest.

.eslintrc.js (ESLint configuration)
-----------------------------------

.. code-block:: javascript

  module.exports = {
    // For XPCShell:
    "extends": "plugin:mozilla/xpcshell-test",
    // Or for Mochitest:
    "extends": "plugin:mozilla/browser-test",

    "rules": {
      // If you want to name your test functions, which can be useful.
      "func-names": "off",
      // Automatically import globals from any head file, just ignore that this is set to "error". ;-)
      "mozilla/import-headjs-globals": "error",
    },
  };

xpcshell.ini (XPCShell test manifest)
-------------------------------------

The default section isn't even necessary here, but you probably want to add a ``head.js`` file if
you're going to have more than one test.

.. code-block:: text

  [default]

  [test_firstTest.js]

browser.ini (Mochitest manifest)
--------------------------------

Mochitest needs some prefs set, or automated testing will fail. 

.. code-block:: text

  [default]
  prefs =
    ldap_2.servers.osx.description=
    ldap_2.servers.osx.dirType=-1
    ldap_2.servers.osx.uri=
    mail.provider.suppress_dialog_on_startup=true
    mail.spotlight.firstRunDone=true
    mail.winsearch.firstRunDone=true
    mailnews.start_page.override_url=about:blank
    mailnews.start_page.url=about:blank
  subsuite = thunderbird

  [browser_firstTest.js]

Linking to manifests
--------------------

The next thing you need to do is tell mach about your new test manifest. In the nearest
``moz.build`` file, add these lines as appropriate:

.. code-block:: python

  BROWSER_CHROME_MANIFESTS += [
      'test/browser/browser.ini',
  ]
  XPCSHELL_TESTS_MANIFESTS += [
      'test/xpcshell/xpcshell.ini',
  ]

Mozmill
=======

It's not really ideal to be adding new Mozmill tests. The platform is obsolete and now that
Mochitest runs on Thunderbird it's preferred over Mozmill. But sometimes, you've gotta do what
you've gotta do.

All Mozmill tests live in ``mail/test/mozmill``, except for calendar tests, which live in
``calendar/test/mozmill``. The test files themselves general live one directory deeper than that
and are named ``test-something.js`` (or ``testSomething.js``). To add a new test, put it in one of
the existing directories, or if you must, create a new directory and add its name to the file
``mozmilltests.list``.

.. note::

  If you add a calendar Mozmill test, you'll need to build again before you can use it.
