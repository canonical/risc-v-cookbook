.. SPDX-License-Identifier: CC-BY-SA-4.0

Create Launchpad user
=====================

To create a new Launchpad user

* open the Launchpad homepage https://launchpad.net in a browser.
* click on `Log In / Register <https://login.launchpad.net/+login>`_.
* select *I don’t have an Ubuntu One account*
* fill in your credentials

Add public key certificate
--------------------------

To upload code to Launchpad your code must be signed by a public key
certificate.

You could use an existing certfificate or create a new OpenPGP key pair:

.. prompt:: bash $ auto

    $ gpg --default-new-key-algo rsa4096 --gen-key

Now list your keys:

.. prompt:: text $ auto

    $ gpg --list-secret-keys
    ------------------------------
    sec   rsa4096/0E5187163D609A71 2024-11-22 [SC] [expires: 2027-11-22]
          Key fingerprint = E673 1759 54C6 F670 0DA0  3CB0 0E51 8716 3D60 9A71
    uid                 [ultimate] John Doe <john.doe@example.com>

Take note of the *Key fingerprint* line.

Publish the public key.
Keyid is the *0E5187163D609A71* string in the example above.

.. prompt:: text $ auto

    $ gpg --send-keys --keyserver keyserver.ubuntu.com <keyid>

Now add your public key to your user in Launchpad.
On your user page
:samp:`https://launchpad.net/~<username>`
you can find a button labeled *OpenPGP keys:* or you can directly navigate to
:samp:`https://launchpad.net/~<username>/+editpgpkeys`.

Enter your fingerprint and press the *Import Key* button.

.. _add-ssh-key:

Add SSH key
-----------

For uploading code to a private PPA or to Git repository on Launchpad you need
an SSH key pair.
You can generate one with:

.. prompt:: text $ auto

    $ ssh-keygen -t rsa -b 4096

To avoid abuse add a passphrase.

The key pair is stored in the .ssh directory of your home directory, e.g.

.. prompt:: text $ auto

    $ find ~/.ssh
    /home/user/.ssh
    /home/user/.ssh/id_rsa.pub
    /home/user/.ssh/id_rsa

Now add your public key to your user in Launchpad.
On your user page
:samp:`https://launchpad.net/~<username>`
you can find a button labeled *SSH keys:* or you can directly navigate to
:samp:`https://launchpad.net/~<username>/+sshkeys`.

Enter the content of the public key (id_rsa.pub in the example) and
press the *Import Public Key* button.
