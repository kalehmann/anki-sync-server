ankisyncd
=========

A personal Anki sync server (so you can sync against your own server rather than
AnkiWeb). This version has been modified to remove the REST API, which makes it
possible to drop some dependencies.

Installing
----------

### Manual installation

1. First, you need to install "virtualenv".  If your system has easy_install,
this is just a matter of:

        $ easy_install virtualenv

    If your system doesn't have easy_install, I recommend getting it!

2. Next, you need to create a Python environment for running ankisyncd and
install some of the dependencies we need there:

        $ virtualenv ankisyncd.env
        $ ankisyncd.env/bin/easy_install webob simplejson

3. Download and install libanki.  You can find the latest release of Anki here:

    http://code.google.com/p/anki/downloads/list

    Look for a *.tgz file with a Summary of "Anki Source".  At the time of this
    writing that is anki-2.0.11.tgz.

    Download this file and extract.

    Then either:

    a. Run ```make install```, or

    b. Copy the entire directory to /usr/share/anki

4. Copy the example.ini to production.ini and edit for your needs.

5. Create authentication database:

        $ sqlite3 auth.db 'CREATE TABLE auth (user VARCHAR PRIMARY KEY, hash VARCHAR)'

6. Create user:

        $ ./ankisyncctl.py adduser <username>

7. Then we can run ankisyncd like so:

        $ ./ankisyncctl.py start

    To stop the server, run:

        $ ./ankisyncctl.py stop

### Via AUR

For Arch Linux, ankisyncd is also available on AUR: https://aur.archlinux.org/packages/ankisyncd-git/

Use your AUR wrapper to install or run the commands below to install manually from PKGBUILD.

    $ wget https://codeload.github.com/jdoe0/ankisyncd-pkgbuild/zip/master
    $ unzip master
    $ cd ankisyncd-pkgbuild-master
    $ makepkg -s
    $ sudo pacman -U *.xz

Configuration file is at /etc/ankisyncd/ankisyncd.conf

To start the server run:

    $ sudo systemctl start ankisyncd

Setting up Anki
---------------

To make Anki use ankisyncd as its sync server, create a file (name it something
like ankisyncd.py) containing the code below and put it in ~/Anki/addons.

    import anki.sync

    anki.sync.SYNC_BASE = 'http://127.0.0.1:27701/'

Replace 127.0.0.1 with the IP address or the domain name of your server if
ankisyncd is not running on the same machine as Anki.