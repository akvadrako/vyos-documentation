.. _documentation:

Documentation
=============

As most software projects we also have a lack in documentation. We encourage
every VyOS user to help us improve our documentation. This will not only be
beneficial for you (when reading something up) but also for the whole world.

If you are willing to contribute to our documentation this is the definite
guide how to do so.

.. note:: In contrast to submitting code patches, there is no requirement that
   you open up a Phabricator_ task prior to submitting a Pull-Request to the
   documentation.

Forking Workflow
----------------

The Forking Workflow is fundamentally different than other popular Git
workflows. Instead of using a single server-side repository to act as the
"central" codebase, it gives every developer their own server-side repository.
This means that each contributor has not one, but two Git repositories: a
private local one and a public server-side one.

The main advantage of the Forking Workflow is that contributions can be
integrated without the need for everybody to push to a single central
repository. Developers push to their own server-side repositories, and only the
project maintainer can push to the official repository. This allows the
maintainer to accept commits from any developer without giving them write
access to the official codebase.

.. note:: Updates to our documentation should be delivered by a GitHub
   pull-request. This requires you already have a GitHub account.

* Fork this project on GitHub https://github.com/vyos/vyos-documentation/fork

* Clone fork to local machine, then change to that directory``$ cd vyos-documentation``

* Install the requirements ``$ pip install -r requirements.txt`` (or something similar)

* Create new branch for your work, use a descriptive name of your work:
  ``$ git checkout -b <branch-name>``

* Make all your changes - please keep our commit rules in mind
  (:ref:`prepare_commit`). This mainly applies to proper commit messages
  describing your change (how and why). Please check out the documentation of
  Sphinx-doc_ or reStructuredText_ if you are not familiar with it. This is used
  for writing our docs. Additional directives how to write in RST can be obtained
  from reStructuredTextDirectives_.

* Check your changes by locally building the documentation ``$ make html``.
  Sphinx will build the html files in the ``docs/_build`` folder. We provide
  you with a Docker container for an easy to use user experience. Check the
  README.md_ file of this repository.

* View modified files by calling ``$ git status``. You will get an overview of
  all files modified by you. You can add individual files to the Git Index in
  the next step.

* Add modified files to Git index ``$ git add path/to/filename`` or add all
  unstaged files ``$ git add .``. All files added to the Git index will be part
  of you following Git commit.

* Commit your changes with the message, ``$ git commit -m "<commit message>"``
  or  use ``$ git commit -v`` to have your configured editor launched. You can
  type in a commit message. Again please make yourself comfortable with out
  rules (:ref:`prepare_commit`).

* Push commits to your GitHub project: ``$ git push -u origin <branch-name>``

* Submit pull-request. In GitHub visit the main repository and you should
  see a banner suggesting to make a pull request. Fill out the form and
  describe what you do.

* Once pull resquests have been approved, you may want to locally update
  your forked repository too. First you'll have to add a second remote
  called `upstream` which points to our main repository. ``$ git remote add
  upstream https://github.com/vyos/vyos-documentation.git``

  Check your configured remote repositories:

  .. code-block:: none

    $ git remote -v
    origin    https://github.com/<username>/vyos-documentation.git (fetch)
    origin    https://github.com/<username>/vyos.documentation.git (push)
    upstream  https://github.com/vyos/vyos-documentation.git (fetch)
    upstream  https://github.com/vyos/vyos-documentation.git (push)

  Your remote repo on Github is called ``origin``, while the original repo you
  have forked is called ``upstream``. Now you can locally update your forked
  repo.

  .. code-block:: none

    $ git fetch upstream
    $ git checkout master
    $ git merge upstream/master

* If you want to update your fork on GitHub, too use the following: ``$ git
  push origin master``

Style Guide
-----------

Sections
^^^^^^^^

We use the following syntax for Headlines.

.. code-block:: none

  #####
  Parts
  #####

  ********
  Chapters
  ********

  Sections
  ========

  Subsections
  -----------

  Subsubsections
  ^^^^^^^^^^^^^^

  Paragraphs
  """"""""""

Address space
^^^^^^^^^^^^^

Note the following RFCs (:rfc:`5737`, :rfc:`3849`, :rfc:`5389` and
:rfc:`7042`), which describe the reserved public IP addresses and autonomous
system numbers for the documentation:

  * ``192.0.2.0/24``
  * ``198.51.100.0/24``
  * ``203.0.113.0/24``
  * ``2001:db8::/32``
  * 16bit ASN: ``64496 - 64511``
  * 32bit ASN: ``65536 - 65551``
  * Unicast MAC Addresses: ``00-53-00`` to ``00-53-FF``
  * Multicast MAC-Addresses: ``90-10-00`` to ``90-10-FF``

Please don't use other public address space.

Custom Sphinx-doc Markup
^^^^^^^^^^^^^^^^^^^^^^^^

When writing the documentation custom commands have been developed. Please
make yourself comfortable with those commands as this eases the way we
render the documentation.

cfgcmd
""""""

When documenting CLI commands use the ``.. cfgcmd::`` directive for all
configuration mode commands. An explanation of the described command should be
added below this statement.

With those custom commands it will be possible to render them in a more
descriptive way in the resulting HTML/PDF manual.

.. code-block:: none

  .. cfgcmd:: set protocols static arp 192.0.2.100 hwaddr 00:53:27:de:23:aa

     This will configure a static ARP entry always resolving `192.0.2.100` to
     `00:53:27:de:23:aa`.

For a inline configuration level command use ``:cfgcmd:``

.. code-block:: none

  :cfgcmd:`set interface ethernet eth0`

opcmd
"""""

When documenting operational level command use the ``.. opcmd::`` directive.
An explanation of the described command should be added below this statement.

With those custom commands it will be possible to render them in a more
descriptive way in the resulting HTML/PDF manual.

.. code-block:: none

  .. opcmd:: show protocols static arp

     Display all known ARP table entries spanning across all interfaces

For a inline operational level command use ``:opcmd:``

.. code-block:: none

  :opcmd:`add system image`

cmdinclude
""""""""""

To minimize redundancy there is a special include directive. It include a txt
file and replace the ``{{ var0 }}`` - ``{{ var9 }}`` with the correct value

.. code-block:: none

   .. cmdinclude:: interface-address.txt
      :var0: ethernet
      :var1: eth1

the content of interface-address.txt looks like this

.. code-block:: none

   .. cfgcmd:: set interfaces {{ var0 }} <interface> address <address | dhcp |
      dhcpv6>

      Configure interface `<interface>` with one or more interface
      addresses.

      * **address** can be specified multiple times as IPv4 and/or IPv6
      address, e.g. 192.0.2.1/24 and/or 2001:db8::1/64
      * **dhcp** interface address is received by DHCP from a DHCP server
      on this segment.
      * **dhcpv6** interface address is received by DHCPv6 from a DHCPv6
      server on this segment.

      Example:

      .. code-block:: none

         set interfaces {{ var0 }} {{ var1 }} address 192.0.2.1/24
         set interfaces {{ var0 }} {{ var1 }} address 192.0.2.2/24
         set interfaces {{ var0 }} {{ var1 }} address 2001:db8::ffff/64
         set interfaces {{ var0 }} {{ var1 }} address 2001:db8:100::ffff/64

vytask
""""""

When referencing to VyOS Phabricator Tasks, there is a custom Sphinx Markup
command called ``vytask`` which automatically renders to a proper Phabricator
URL. This is heavily used in the :ref:`release-notes` section.

.. code-block:: none

  * :vytask:`T1605` Fixed regression in L2TP/IPsec server
  * :vytask:`T1613` Netflow/sFlow captures IPv6 traffic correctly


.. _Sphinx-doc: https://www.sphinx-doc.org
.. _reStructuredText: http://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html
.. _reStructuredTextDirectives: https://docutils.sourceforge.io/docs/ref/rst/directives.html
.. _README.md: https://github.com/vyos/vyos-documentation/blob/master/README.md

.. include:: ../common-references.rst
