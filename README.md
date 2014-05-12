Moodle Development Kit
======================

A collection of tools meant to make developers' lives easier.

Requirements
============

- Linux or Mac OS
- Python 2.7
- MySQL or PostgreSQL
- Git v1.7.7 or greater

Most of the tools work on Moodle 1.9 onwards, but some CLI scripts required by MDK might not be available in all versions.

Usage
=====

The commands are called using that form:

    mdk <command> <arguments>

Get some help on a command using:

    mdk <command> --help

Also check the [wiki](https://github.com/FMCorz/mdk/wiki).

Installation
============

Ubuntu package
--------------

_This method is currently not recommended, the package is outdated. Maintainer wanted!_

    sudo apt-add-repository ppa:2x1cq-fred-7nqa6/ppa
    sudo apt-get update
    sudo apt-get install moodle-sdk
    # Most settings are probably good as they are, just configure your remote and database engines.
    sudo mdk init
    # The next line prevents you from logging out and in again.
    sudo su `whoami`

You're done!
Try the following command to create a typical Stable Master instance (this will take some time because the cache is still empty):

    mdk create
    mdk list

Now you should be able to access it from http://moodle-sdk/stable_master.

Mac OS
------

Using [Homebrew](http://brew.sh/), please refer to this [formula](https://github.com/danpoltawski/homebrew-mdk).

Manual installation
-------------------

### 1. Clone the repository

    cd /opt
    sudo git clone git://github.com/FMCorz/mdk.git moodle-sdk

### 2. Install the dependencies

You will need the tool [pip](http://www.pip-installer.org/en/latest/installing.html) to install the packages required by Python.

    sudo pip install -r /opt/moodle-sdk/requirements.txt

### 3. Make executable and accessible

    sudo chmod +x /opt/moodle-sdk/mdk.py
    sudo ln -s /opt/moodle-sdk/mdk.py /usr/local/bin/mdk

### 4. Set up the basics

Assuming that you are using Apache, which is set up to serve the files from /var/www, leave the default values as they are in `mdk init`, except for your remote and the database passwords.

    mkdir ~/www
    sudo ln -s ~/www /var/www/m
    sudo mdk init

### 5. Done

Try the following command to create a typical Stable Master instance (this will take some time because the cache is still empty):

    mdk create
    mdk list

Now you should be able to access it from http://localhost/m/stable_master.

Command list
============

alias
-----

Set up aliases of your Moodle commands.

**Example**

This line defines the alias 'upall', for 'moodle update --all'
    
    mdk alias add upall "update --all"

backport
--------

Backport a branch to another instance of Moodle.

**Examples**

Assuming we are in a Moodle instance, this backports the current branch to the version 2.2 and 2.3

    mdk backport --version 22 23

Backports the branch MDL-12345-23 from the instance stable_23 to the instance stable_22, and pushes the new branch to your remote

    mdk backport stable_23 --branch MDL-12345-23 --version 22 --push

backup
------

Backup a whole instance so that it can be restored later.

**Examples**

Backup the instance named stable_master

    mdk backup stable_master

List the backups

    mdk backup --list

Restore the second backup of the instance stable_master

    mdk backup --restore stable_master_02


behat
-----

Get the instance ready for acceptance testing (Behat), and run the test feature(s).

**Examples**

    mdk behat -r --tags=@core_completion


create
------

Create a new instance of Moodle. It will be named according to your config file.

**Examples**

Create a new instance of Moodle 2.1

    mdk create --version 21

Create an instance of Moodle 2.2 using PostgreSQL from the integration remote, and run the installation script.

    mdk create --version 22 --engine pgsql --integration --install

config
------

Set your MDK settings from the command line.

**Examples**

Show the list of your settings
     
    mdk config list

Change the value of the setting 'dirs.storage' to '/var/www/repositories'

    mdk config set dirs.storage /var/www/repositories


css
---

CSS related functions.

**Example**

Compile the LESS files from Bootstrapbase

    mdk css --compile


doctor
------

Perform some checks on the environment to identify possible problems, and attempt to fix them automatically.


fix
---

Create a branch from an issue number on the tracker (MDL-12345) and sets it to track the right branch.

**Examples**

In a Moodle 2.2 instance, this will create (and checkout) a branch named MDL-12345-22 which will track upstream/MOODLE_22_STABLE.

    mdk fix MDL-12345
    mdk fix 12345


info
----

Display information about the instances on the system.

**Examples**

List the instances

    mdk info --list

Display the information known about the instance *stable_master*

    mdk info stable_master


install
-------

Run the command line installation script with all parameters set on an existing instance.

**Examples**

    mdk install --engine mysqli stable_master


js
--

JS related functions.

**Example**

Compile the JS modules in Atto

    mdk js shift --plugin editor_atto


phpunit
-------

Get the instance ready for PHPUnit tests, and run the test(s).

**Examples**

    mdk phpunit -u repository/tests/repository_test.php


plugin
------

Look for a plugin on moodle.org and downloads it into your instance.

**Example**

    mdk plugin download repository_evernote


purge
-----

Purge the cache.

**Example**

To purge the cache of all the instances

    mdk purge --all


pull
----

Pulls a patch using the information from a tracker issue.

**Example**

Assuming we type that command on a 2.3 instance, pulls the corresponding patch from the issue MDL-12345 in a testing branch

    mdk pull --testing 12345


push
----

Shortcut to push a branch to your remote.

**Examples**

Push the current branch to your repository

    mdk push

Force a push of the branch MDL-12345-22 from the instance stable_22 to your remote

    mdk push --force --branch MDL-12345-22 stable_22


rebase
------

Fetch the latest branches from the upstream remote and rebase your local branches.

**Examples**

This will rebase the branches MDL-12345-xx and MDL-56789-xx on the instances stable_22, stable_23 and stable_master. And push them to your remote if successful.

    mdk rebase --issues 12345 56789 --version 22 23 master --push
    mdk rebase --issues MDL-12345 MDL-56789 --push stable_22 stable_23 stable_master


remove
------

Remove an instance, deleting every thing including the database.

**Example**

    mdk remove stable_master


run
---

Execute a script on an instance. The scripts are stored in the scripts directory.

**Example**

Set the instance stable_master ready for development

    mdk run dev stable_master


tracker
-------

Gets some information about the issue on the tracker.

**Example**

    $ mdk tracker 34543
    ------------------------------------------------------------------------
      MDL-34543: New assignment module - Feedback file exists for an
        assignment but not shown in the Feedback files picker
      Bug - Critical - https://tracker.moodle.org/browse/MDL-34543
      Closed (Fixed) 2012-08-17 07:25
    -------------------------------------------------------[ V: 7 - W: 7 ]--
    Reporter            : Paul Hague (paulhague) on 2012-07-26 08:30
    Assignee            : Eric Merrill (emerrill)
    Peer reviewer       : Damyon Wiese (damyon)
    Integrator          : Dan Poltawski (poltawski)
    Tester              : Tim Barker (timb)
    ------------------------------------------------------------------------


uninstall
---------

Uninstall an instance: removes config file, drops the database, deletes dataroot content, ...


update
------

Fetch the latest stables branches from the upstream remote and pull the changes into the local stable branch.

**Examples**

This updates the instances stable_22 and stable_23

    mdk update stable_22 stable_23

This updates all your integration instances and runs the upgrade script of Moodle.

    mdk update --integration --upgrade


upgrade
-------

Run the upgrade script of your instance.

**Examples**

The following runs an upgrade on your stable branches

    mdk upgrade --stable

This will run an update an each instance before performing the upgrade process

    mdk upgrade --all --update

Scripts
=======

You can write custom scripts and execute them on your instances using the command `mdk run`. MDK looks for the scripts in the _scripts_ directories and identifies their type by reading their extension. For example, a script called 'helloworld.php' will be executed as a command line script from the root of an installation.

    # From anywhere on the system
    $ mdk run helloworld stable_master

    # Is similar to typing the following command
    $ cp /path/to/script/helloworld.php /path/to/moodle/instances/stable_master
    $ cd /path/to/moodle/instances/stable_master
    $ php helloworld.php

Scripts are very handy when it comes to performing more complexed tasks.

Shipped scripts
---------------

The following scripts are available with MDK:

* `dev`: Changes a portion of Moodle settings to enable development mode
* `enrol`: Enrols users in any existing course
* `undev`: Reverts the changes made by `dev`
* `users`: Creates a set of users
* `webservices`: Does all the set up of webservices for you

License
=======

Licensed under the [GNU GPL License](http://www.gnu.org/copyleft/gpl.html)
