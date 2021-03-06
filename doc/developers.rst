.. _Sphinx: http://www.sphinx-doc.org
.. _Read the Docs: http://bro-package-manager.readthedocs.io/en/latest
.. _GitHub: https://github.com/bro/package-manager
.. _Google Style Docstrings: http://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html
.. _bro-aux: https://github.com/bro/bro-aux
.. _PyPi: https://pypi.python.org/pypi

Developer's Guide
=================

This a guide for developers working on the Bro Package Manager itself.

Versioning/Releases
-------------------

After making a commit to the *master* branch, you can use the
:program:`update-changes` script in the `bro-aux`_ repository to automatically
adapt version numbers and regenerate the :program:`bro-pkg` man page.  Make sure
to install the `documentation dependencies`_ before using it.

Releases are hosted at PyPi_.  To build and upload a release:

#. Finalize the git repo tag and version with  ``update-changes -R <version>``
   if not done already.

#. Get an account at PyPi_ if you don't have one already.

#. Download :program:`twine`, which is used for uploading the release
   distribution:

   .. code-block:: console

      $ pip install twine

#. Build the :program:`bro-pkg` distribution:

   .. code-block:: console

      $ python setup.py bdist_wheel

#. If :program:`bro-pkg` metadata in :file:`setup.py` has changed, register it:

   .. code-block:: console

      $ twine register -u <username> dist/<package_file>

#. Upload the distribution:

   .. code-block:: console

      $ twine upload -u <username> dist/<package_file>

Documentation
-------------

Documentation is written in reStructuredText (reST), which Sphinx_ uses to
generate HTML documentation and a man page.

.. _documentation dependencies:

Dependencies
~~~~~~~~~~~~

To build documentation locally, find the minimal requirements in
:file:`requirements.doc.txt`:

  .. literalinclude:: ../requirements.doc.txt

They can be installed like:

  pip install -r requirements.doc.txt

Local Build/Preview
~~~~~~~~~~~~~~~~~~~

Use the :file:`Makefile` targets ``make html`` and ``make man`` to build the
HTML and man page, respectively.  To view the generated HTML output, open
:file:`doc/_build/index.html`.  The generated man page is located in
:file:`doc/man/bro-pkg.1`.

If you have also installed :program:`sphinx-autobuild` (e.g. via
:program:`pip`), there's a :file:`Makefile` target, ``make livehtml``, you can
use to help preview documentation changes as you edit the reST files.

Remote Hosting
~~~~~~~~~~~~~~

The GitHub_ repository has a webhook configured to automatically rebuild the
HTML documentation hosted at `Read the Docs`_ whenever a commit is pushed.

Style Conventions
~~~~~~~~~~~~~~~~~

The following style conventions are (generally) used.

========================== =============================== ===========================
Documentation Subject      reST Markup                     Preview
========================== =============================== ===========================
File Path                  ``:file:`path```                :file:`path`
File Path w/ Substitution  ``:file:`{<replace_me>}/path``` :file:`{<replace_me>}/path`
OS-Level Commands          ``:command:`cmd```              :command:`cmd`
Program Names              ``:program:`prog```             :program:`prog`
Environment Variables      ``:envvar:`VAR```               :envvar:`VAR`
Literal Text (e.g. code)   ````code````                    ``code``
Substituted Literal Text   ``:samp:`code {<replace_me>}``` :samp:`code {<replace_me>}`
Variable/Type Name         ```x```                         `x`
INI File Option            ```name```                      `name`
========================== =============================== ===========================

Python API docstrings roughly follow the `Google Style Docstrings`_ format.
