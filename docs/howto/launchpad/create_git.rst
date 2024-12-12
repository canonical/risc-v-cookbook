-.. SPDX-License-Identifier: CC-BY-SA-4.0

.. _create_git:

Create Git Repository in Launchpad
==================================

Git repositories can be created in Launchpad for users or teams.

For using Launchpad git repositories you must
:ref:`add an SSH key to your user <add-ssh-key>`.

Initialize the local git repository with

.. prompt:: bash $ auto

    $ git init

Add the upstream repository with

.. prompt:: bash $ auto

    $ git remote add origin \
      git+ssh://<user>@git.launchpad.net/~<team_or_user>/+git/<repo>

user
    your Launchpad user used for login

team_or_user
    the Launchpad user or team for which the repository is created

repo
    name of the repository

Add the default branch

.. prompt:: bash $ auto

    $ git checkout -b main

Add some commit(s), e.g.

.. prompt:: bash $ auto

    $ vi README.rst
    $ git add README.rst
    $ git commit -asm 'Initial commit'

Push to upstream with

.. prompt:: bash $ auto

    $ git push -u origin main

Pushing is enough to create a repository.

Browse git repositories
-----------------------

The git repositories are listed at
`https://code.launchpad.net/~<team_or_user>/+git`.

You can navigate from the team or user home page by opening the `Code` tabe
and clicking the `View Git repositories` link.
