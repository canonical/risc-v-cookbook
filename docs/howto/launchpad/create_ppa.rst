.. SPDX-License-Identifier: CC-BY-SA-4.0

Create Public and Private PPAs
==============================

PPAs (Personal Package Archives) can belong to either Launchpad users or Launchpad
teams. PPAs associated with Launchpad users are public by default. However, PPAs owned
by Launchpad teams can be either public or private, depending on whether the associated
Launchpad team is public or private. For details on how to create public and private
Launchpad teams, see :doc:`Create and Manage Launchpad Teams <create_team>`.

To create a PPA:

* Go to your launchpad user or team profile at https://launchpad.net/~<username or team>.
* Under the section *Personal package archives*, click *Create a new PPA*. Note that for
  private teams, only team administrators can access this option.
* Fill the *URL* and *Display name*. Try to give it a meaningful name.
  For example, it can be of the type *package-lpbug-description*:

  1. **URL**: postfix-lp1753470-segfault
  2. **Display name**: postfix-lp1753470-segfault
* Click *Activate*. The PPA is now created.
* Go to the newly-created PPA page at
  https://launchpad.net/~<username or team>/+archive/ubuntu/<PPA URL>, click *Change Details*,
  and select the architectures this PPA should build packages for.

The PPA is now ready. See :doc:`Upload to a PPA <upload_ppa>` and
:doc:`Consuming public and private PPAs <consume_ppa>` for further steps.
