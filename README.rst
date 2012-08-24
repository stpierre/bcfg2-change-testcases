===========================
 Bcfg2 - Sample Repository
===========================

This repo contains test cases for a change-tracking tool for Bcfg2
that is currently being written.  Each branch is a separate test case.

The ``master`` branch contains a sparse, but working, Bcfg2
specification that provides base files for all of our test cases.

Each other branch should be named ``<plugin>-<n>``, where ``<plugin>``
is the name of the Plugin whose data is being modified for the test
case, and ``<n>`` is a unique number.  The number is required, even if
a given plugin only has a single test case. Only one plugin's data
should be modified per test case, where possible.

Each test case should also have two files:

* ``README`` should contain a brief description of the test
* ``results`` should contain a list of clients that should match the
  output of the change-tracking tool.  Regardless of the final format
  of the output from the tool, ``results`` should be a simple list of
  clients, one per line, as listed in ``Metadata/clients.xml``.  The
  testing tool will handle any format conversion necessary.

For each branch, a test will be run against the patch created by::

    git checkout <branch>
    git diff master

Once each branch has been individually tested, each 2-combination of
branches testing different plugins will be tested.  That is, assuming
the following::

    $ git branch
    * master
      metadata-1
      metadata-2
      bundler-1
      bundler-2
      bundler-3

Tests will be run on the union of patches produced by ``metadata-1``
and ``bundler-1``, ``metadata-2`` and ``bundler-1``, ``metadata-1``
and ``bundler-2``, and so on, but **not** on ``metadata-1`` and
``metadata-2``.

In these cases, the expected output will be the union of the
``results`` files from the two branches.

Subsequently, each 3-combination of branches testing different plugins
will be tested, and on up to each N-combination, where N is the number
of different plugin branches.

Additionally, branches that are not named after plugins and do not
have numbers appended can be created; these will only be run
individually, and can be used to test specific changes that modify
data for multiple plugins.  E.g., adding a file to ``Cfg/`` might be
done in a branch called ``add-etc-foo.conf``, since that would involve
both adding a file to ``Cfg`` and adding it in ``Bundler``.
(Conversely, a test that **just** added the file, and did not add it
to Bundler, could be in a branch called ``cfg-1``, and ``results`` for
this would be empty.)
