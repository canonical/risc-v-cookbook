.. SPDX-License-Identifier: CC-BY-SA-4.0

Create and manage PPA
=====================

Private package archives (PPAs) are where users and teams build Debian packages.

PPAs can belong to either Launchpad users or Launchpad teams.
PPAs associated with Launchpad users are public by default.
PPAs owned by private Launchpad teams are always private.

For details on creating public and private Launchpad teams, see
:doc:`create_team`.

To create a new PPA

* open your user or team page in Launchpad
* click the *Create a new PPA* link

  .. note::

     In private teams, only team administrators can create PPAs.
* enter the URL and display names
* press the *Activate* button

In the list *Existing PPAs* you will see you new PPA.

Click the provided link to show the overview page of your PPA.

Change build architecture
-------------------------

By default only amd64 and i386 are selected as build architectures.

To change this

* Click the *Change details* link on the PPA overview page.
* In the section *Processors* choose the relevant architectures.

If amd64 is enabled, architecture independent packages will be build on amd64.
So it is often reasonable to keep amd64 enabled even if working for another
architecture.

Edit PPA dependencies
---------------------

When building a package the build dependencies will be taken by default from
the current PPA and from the Ubuntu archive.

If you have other PPAs which should also be searched you can add them:

* Click the *Edit PPA dependencies* link on the PPA overview page
* In the add PPA dependency field add the team or user and the PPA name
  separated by a slash, e.g. *user/ppa* and press the search button.
* In the search box press the search button and choose the PPA.
* Press the *Save* button.

To delete a PPA dependency mark the checkbox in its line and press the *Save*
button.

Change repository size
----------------------

If the repository size of the PPA is insufficient for your needs, please, open
a question on https://answers.launchpad.net/launchpad/+addquestion.
