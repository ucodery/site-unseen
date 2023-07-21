==========================
 Site Unseen
==========================

hidden python customization

.. include:: ./trim.rst

site
====

- Python is a customizable language
- So too is its interpreter startup

.. include:: ./trim.rst

.. note::
   “consenting adults”
   -  monkey-patching; magic methods (+, -, isinstance)
   unseen customization can sometimes be referred to as “magic”.
   -  decorators; context manager
   There is nothing mysterious about it though, it is just unseen complexity.
   If we look at it it won’t be so magical - the illusion will be revealed
   Especially lookup paths
   Much of this is done through `site` in stdlib

site
====

- Extend Module search path
- Enable interactive features
- Execute user customizations

.. include:: ./trim.rst

site
====

- :code:`import site`
- Done for you before ``__main__`` starts
   - Can be turned off with ``-S`` option
   - Explicit import will still turn it on

.. include:: ./trim.rst

.. note::
   you’ve never imported site, but have always benefited from it
   If that’s surprising to you: you chose the right talk
   done out-of-site (magically)

site
====

- :code:`python -m site`
- Module search paths diagnostics

.. include:: ./trim.rst

.. note::
   Can also be executed

site
====

.. code-block::

   $ python -m site
   sys.path = [
       '/Users/ucodery/unseen',
       '/Users/ucodery',
       '/Users/ucodery/.local/py311/lib/python311.zip',
       '/Users/ucodery/.local/py311/lib/python3.11',
       '/Users/ucodery/.local/py311/lib/python3.11/lib-dynload',
       '/Users/ucodery/.local/py311/lib/python3.11/site-packages',
       '/tmp/EuroPython/python/packages',
       '/Users/ucodery/.local/lib/python3.11/site-packages',
       '/Users/ucodery/.local/py311/lib/python3.11/site-packages',
   ]
   USER_BASE: '/Users/ucodery/.local' (exists)
   USER_SITE: '/Users/ucodery/.local/lib/python3.11/site-packages' (exists)
   ENABLE_USER_SITE: True

.. include:: ./trim.rst

site
====

.. code-block::

   $ python -S -m site
   sys.path = [
       '/Users/ucodery/unseen',
       '/Users/ucodery',
       '/Users/ucodery/.local/py311/lib/python311.zip',
       '/Users/ucodery/.local/py311/lib/python3.11',
       '/Users/ucodery/.local/py311/lib/python3.11/lib-dynload',
   ]
   USER_BASE: '/Users/ucodery/.local' (exists)
   USER_SITE: '/Users/ucodery/.local/lib/python3.11/site-packages' (exists)
   ENABLE_USER_SITE: None

.. include:: ./trim.rst

.. note::
   We’re asking Python not to import site, but executing site.
   A little weird. It "just works"

site
====

.. code-block::

   python -S -c 'import site; site.main(); site._script()'
   sys.path = [
       '/Users/ucodery/unseen',
       '/Users/ucodery',
       '/Users/ucodery/.local/py311/lib/python311.zip',
       '/Users/ucodery/.local/py311/lib/python3.11',
       '/Users/ucodery/.local/py311/lib/python3.11/lib-dynload',
       '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages',
       '/tmp/EuroPython/python/packages',
       '/Users/ucodery/.local/lib/python3.11/site-packages',
       '/Users/ucodery/.local/py311/lib/python3.11/site-packages',
   ]
   USER_BASE: '/Users/ucodery/.local' (exists)
   USER_SITE: '/Users/ucodery/.local/lib/python3.11/site-packages' (exists)
   ENABLE_USER_SITE: True

.. include:: ./trim.rst

==========================
 Extend Module Search Path
==========================

.. include:: ./trim.rst

.. note::
   Everyone probably knows PYTHONPATH can be used to extend the module search path
   but it is not the only way

Extend Module Search Path
=========================

- site-packages

.. include:: ./trim.rst

.. note::
   probably know the directory site-packages from installing 3rd party packages. But NOT guaranteed to be in sys.path
   -  THIS is why pip installs into “site-packages”
   possibly many site packages
   co-located with stdlib but has no __init__.py

Extend Module Search Path
=========================

- site-packages
- user site-packages

.. include:: ./trim.rst

.. note::
   You might know USER_SITE from pip.
   installs associated with user (HOME) not the install (prefix)

Extend Module Search Path
=========================

.. code-block::

   $ pip install example

.. code-block::

   error: could not create '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages
   /example': Permission denied
   ----------------------------------------
   ERROR: Command errored out with exit status 1: /Users/ucodery/unseen/.venv/bin/py
   thon -u -c 'import sys, setuptools, tokenize; sys.argv[0] = '"'"'/private/var/fol
   ders/kz/fz2z89ws6rx5_0vn64nrwpb40000gn/T/pip-install-kuiz530q/example/setup.py'"'
   "'; __file__='"'"'/private/var/folders/kz/fz2z89ws6rx5_0vn64nrwpb40000gn/T/pip-in
   stall-kuiz530q/example/setup.py'"'"';f=getattr(tokenize, '"'"'open'"'"', open)(__
   file__);code=f.read().replace('"'"'\r\n'"'"', '"'"'\n'"'"');f.close();exec(compil
   e(code, __file__, '"'"'exec'"'"'))' install --record /private/var/folders/kz/fz2z
   89ws6rx5_0vn64nrwpb40000gn/T/pip-record-3h15zlul/install-record.txt --single-vers
   ion-externally-managed --compile --install-headers /Users/ucodery/unseen/.venv/in
   clude/site/python3.11/example Check the logs for full command output.

.. include:: ./trim.rst

..
   Extend Module Search Path
   =========================

   .. code-block::

      $ sudo pip install example

   .. include:: ./trim.rst

Extend Module Search Path
=========================

.. code-block::

   $ pip install example --user
   Collecting example
     Using cached https://files.pythonhosted.org/packages/c9/c9/6122a974f5b611b16396
   c918722ea75945db7dcd3069c477a7c608a405a0/example-0.1.0.tar.gz
   Requirement already satisfied: six in /Users/ucodery/.local/lib/python3.11/site-p
   ackages (from example) (1.16.0)
   Installing collected packages: example
       Running setup.py install for example ... done
   Successfully installed example-0.1.0

..
   $ python -m pip install example
   Defaulting to user installation because normal site-packages is not writeable
   Collecting example
     Downloading example-0.1.0.tar.gz (860 bytes)
     Installing build dependencies ... done
     Getting requirements to build wheel ... done
     Preparing metadata (pyproject.toml) ... done
   Collecting six (from example)
     Using cached six-1.16.0-py2.py3-none-any.whl (11 kB)
   Building wheels for collected packages: example
     Building wheel for example (pyproject.toml) ... done
     Created wheel for example: filename=example-0.1.0-py3-none-any.whl size=1220 sha256=532c87496256322224f50ed997446b62eaa2496807e4d690a1c16bb7b4159cee
     Stored in directory: /Users/ucodery/Library/Caches/pip/wheels/14/de/af/940922ac9dc43faded2bb30a1049c2fe4e2728e372183626f5
   Successfully built example
   Installing collected packages: six, example
   Successfully installed example-0.1.0 six-1.16.0

.. include:: ./trim.rst

.. note::
   since pip v20.0 (2020-01-21) pip will fall back on –user (if permission issues)

Extend Module Search Path
=========================

- site-packages
- user site-packages
- virtualenv

.. include:: ./trim.rst

.. note::
   Setting up of virtual environments is entirely down to `site` being run
   add virtual site-packages
   still add system “real” site-packages (sometimes)

Extend Module Search Path
=========================

- site-packages
- user site-packages
- virtualenv
- pth files

.. include:: ./trim.rst

.. note::
   Every site-packages dir that is added to sys.path is searched for for .pth
   Allow installed packages to participate in the site customization
   These dirs also add to sys.path, if exist
   They also allow other imports to happen before the main import, just like `site`
   like metaclasses: they are an advanced answer
   editable installs; code coverage injection (pytest-cov)

Extend Module Search Path
=========================

.. code-block::

 # mytelemetry.pth
 /Users/ucodery/repos/mycov
 import mycov.pth_setup

.. include:: ./trim.rst

Extend Module Search Path
=========================

.. code-block::

   python -m site
   sys.path = [
       '/Users/ucodery/unseen',                                     # cwd
       '/Users/ucodery',                                            # PYTHONPATH
       '/Users/ucodery/.local/py311/lib/python311.zip',             # stdlib
       '/Users/ucodery/.local/py311/lib/python3.11',                # stdlib
       '/Users/ucodery/.local/py311/lib/python3.11/lib-dynload',    # stdlib
       '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages',  # venv site
       '/tmp/EuroPython/python/packages',                           # pth
       '/Users/ucodery/.local/lib/python3.11/site-packages',        # user site
       '/Users/ucodery/.local/py311/lib/python3.11/site-packages',  # system site
   ]
   USER_BASE: '/Users/ucodery/.local' (exists)
   USER_SITE: '/Users/ucodery/.local/lib/python3.11/site-packages' (exists)
   ENABLE_USER_SITE: True

.. include:: ./trim.rst

============================
 Enable Interactive Features
============================

.. include:: ./trim.rst

Enable Interactive Features
===========================

- configure readline module

.. include:: ./trim.rst

.. note::
   TAB-completion, command history (including arrow-up, reverse search), and inputrc 

Enable Interactive Features
===========================

.. code-block::

   >>> improt^[[D^[[D^[[D^[[C^[[C^[[A^[[A^[[A^[[B

.. include:: ./trim.rst

.. note::
   you’ll know readline wasn’t configured if the buffer starts filling with garbage like this

Enable Interactive Features
===========================

- configure readline module
- add new builtins

.. include:: ./trim.rst

.. note::
   help, exit, quit, copyright, credits, license

Enable Interactive Features
===========================

.. code-block::

   $ python -c "exit()"

.. include:: ./trim.rst

Enable Interactive Features
===========================

.. code-block::

   $ python -S -c "exit()"
   Traceback (most recent call last):
     File "<string>", line 1, in <module>
   NameError: name 'exit' is not defined

.. include:: ./trim.rst

.. note::
   just always use sys.exit()
   nice to not need an import to leave REPL

===========================
Execute User Customizations
===========================

.. include:: ./trim.rst

.. note::
   more hooks for other code to participate in interpreter startup customization

Execute User Customizations
===========================

- sitecustomize

.. include:: ./trim.rst

.. note::
   allows administrator to alter default Python behavior for any users on the machine
   telemetry

Execute User Customizations
===========================

.. code-block:: python

   # sitecustomize.py
   import logging.handlers
   handler = logging.handlers.SMTPHandler(
      'mailserver.example.com',
      '"Python Error Monitor" <host123@example.com>'
      'python-errors@example.com'
      'Errors from host123'
   )
   handler.setLevel(logging.ERROR)
   logging.getLogger('').addHandler(handler)

.. include:: ./trim.rst

Execute User Customizations
===========================

- sitecustomize
- usercustomize

.. include:: ./trim.rst

.. note::
   customize Python when you run it. tune it with sys vars
   you change or add to python
   remember: slows all python startup

Execute User Customizations
===========================

.. code-block:: python

   # usercustomize.py
   import pretty_traceback
   pretty_traceback.install()

.. include:: ./trim.rst

Execute User Customizations
===========================

- sitecustomize
- usercustomize
- PYTHONSTARTUP

.. include:: ./trim.rst

.. note::
   exece'd not imported. shared global scope
   WHY: interactive-only customizations, similar to `site` enabling interactive features
   always be careful to not crash the interpreter loop. Not version specific

Execute User Customizations
===========================

.. code-block:: python

   # startup.py
   from __future__ import annotations
   import numpy as np
   import sys
   sys.set_int_max_str_digits(0)

.. include:: ./trim.rst

=====================
 When Things Go Wrong
=====================

.. include:: ./trim.rst

.. note::
   For reducing attack vectors
   Also for when your world is burning down
   either way less "magic"

Too Many Paths
==============

- :code:`python -v`
- :code:`python -X importtime`

.. include:: ./trim.rst

.. note::
   cpython specific option
   these and many of the other options have env vars

Too Many Paths
==============

.. code-block::

   $ python -v -m site 2>&1 | grep 'Processing|Adding'
   Processing global site-packages
   Adding directory: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages'
   Processing .pth file: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages/unseen.pth'
   Processing user site-packages
   Adding directory: '/Users/ucodery/.local/lib/python3.11/site-packages'
   Processing global site-packages
   Adding directory: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages'
   Processing .pth file: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages/unseen.pth'
   Adding directory: '/Users/ucodery/.local/py311/lib/python3.11/site-packages'
   Processing global site-packages
   Adding directory: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages'
   Processing .pth file: '/Users/ucodery/unseen/.venv/lib/python3.11/site-packages/unseen.pth'

.. include:: ./trim.rst

.. note::
   Without filtering, around 300 lines of log
   Python 3.10+

Too Many Paths
==============

- :code:`PYTHONPATH="" python`
- :code:`python -P`

.. include:: ./trim.rst

.. note::
   stop pesky shaddowing bugs
   Must use 3.11+

Too Much Customization
======================

- :code:`PYTHONSTARTUP="" python`
- :code:`python -s`
- :code:`python -S`

.. include:: ./trim.rst

.. note::
   NO venvs!
   no sitecustomize

Untrusted User
==============

- :code:`python -E`
   - ``PYTHONPATH``, ``PYTHONSTARTUP``
- :code:`python -I`
   - shortcut for ``-E``, ``-s``

.. include:: ./trim.rst

.. note::
   "no user-trust"
   Can disable a lot of other options: PYTHONBREAKPOINT, PYTHONHASHSEED…
   This is admin-trusted python - nothing set by the user is influencing python

Safe Python
===========

- :code:`python -SIP`

.. include:: ./trim.rst

.. note::
   “Safe mode” of Python
   -  no relative path imports
   -  no influence from environment variables
   -  virtual environments don’t engage
   -  no 3rd party modules available
   -  no custom python code executed prior to __main__
   Ultimately, less “magic”

===============
 Go, Customize!
===============

:ref:`https://gist.github.com/ucodery/23f5de4f40b3e88fd9f927dc8f0d4a42`

:ref:`https://github.com/ucodery/site-unseen`

.. include:: ./foot.rst

.. note::
   Be Safe when you need to be.
   Python is a tool, make it work for you
   personal and productive for you
   a joy to use every day
