.. SPDX-License-Identifier: CC-BY-SA-4.0

How to contribute
=================

We believe that everyone has something valuable to contribute,
whether you're a coder, a writer or a tester.
Here's how and why you can get involved:

- **Why join us?** Work with like-minded people, develop your skills,
  connect with diverse professionals, and make a difference.

- **What do you get?** Personal growth, recognition for your contributions,
  early access to new features and the joy of seeing your work appreciated.

- **Start early, start easy**:
  Improve documentation, or provide examples.
  Your presence matters,
  regardless of experience or the size of your contribution.

The guidelines below will help keep your contributions effective and meaningful.


Code of conduct
---------------

When contributing, you must abide by the
`Ubuntu Code of Conduct <https://ubuntu.com/community/ethos/code-of-conduct>`_.


License and copyright
---------------------

All contributions must use the
`Attribution-ShareAlike 4.0 International <https://creativecommons.org/licenses/by-sa/4.0/legalcode>`_
(CC BY-SA 4.0) license.

Please, add the following first line to each new document:

.. code-block:: text

   .. SPDX-License-Identifier: CC-BY-SA-4.0

All contributors must sign the `Canonical contributor license agreement
<https://ubuntu.com/legal/contributors>`_,
which grants Canonical permission to use the contributions.
The author of a change remains the copyright owner of their code
(no copyright assignment occurs).


Environment setup
-----------------

For working on the documentation you will need:

* Python 3
* make
* git
* a text editor

Submissions
-----------

If you want to address an issue or a bug in the
:doc:`index`

- Fork our Github repository
  `<https://github.com/canonical/risc-v-cookbook>`__
  and add the changes to your fork,
  properly structuring your commits,
  providing detailed commit messages
  and signing your commits.

- Make sure the updated project builds and runs without warnings or errors;
  this includes running the checks described in <checks>_.

- Submit the changes as a `pull request (PR)
  <https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork>`_.


Your changes will be reviewed in due time;
if approved, they will be eventually merged.


Describing pull requests
~~~~~~~~~~~~~~~~~~~~~~~~

To be properly considered, reviewed and merged,
your pull request must provide the following details:

- **Title**: Summarize the change in a short, descriptive title.

- **Description**: Explain the problem that your pull request solves.
  Mention any new features, bug fixes or refactoring.

Commit structure and messages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use separate commits for each logical change,
and for changes to different components.
Prefix your commit messages with names of components they affect,
using the code tree structure,
e.g. start a commit that updates the 'Create user' page with
``create_user:``.

Signing commits
~~~~~~~~~~~~~~~

To improve contribution tracking,
we use the developer certificate of origin
(`DCO 1.1 <https://developercertificate.org/>`_)
and require a "sign-off" for any changes going into each branch.

The sign-off is a simple line at the end of the commit message
certifying that you wrote it
or have the right to commit it as an open-source contribution.

To sign off on a commit, use the ``--signoff`` option in ``git commit``.


Documentation
-------------

Structure
~~~~~~~~~

We follow the `Diátaxis <https://diataxis.fr/>`_ methodology.

Formatting
~~~~~~~~~~

We use `Sphinx <https://www.sphinx-doc.org>`_ for generating the
documentation from **reStructured Text**. Refer to our `reStructuredText style guide <https://canonical-documentation-with-sphinx-and-readthedocscom.readthedocs-hosted.com/style-guide/>`_ for basic guidance.

We tend to use a maximum of 80 characters per line.

As the target format is HTML and the reStructured text only serves as source,
putting new sentences in new lines saves you from superfluous reformatting.

.. checks:

Automatic checks
~~~~~~~~~~~~~~~~

GitHub runs automatic checks on the documentation
to verify spelling, validate links and suggest inclusive language.

You can (and should) run the same checks locally:

.. prompt:: bash $ auto

   $ make spelling
   $ make linkcheck
   $ make woke

To overcome problems with the woke check an inline comment like the following
may be needed:

.. code-block:: text

    This is a forbidden word. .. wokeignore:rule=forbidden

The rules are listed in
https://raw.githubusercontent.com/canonical/Inclusive-naming/main/config.yml.
