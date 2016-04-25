LMU-IuK.buildout
================

This Buildout is used at the LMU IT Division to develop and deploy a Zope/Plone Cluster.

If you are familiar with Plone and Zope setup this a normal buildout based on the concepts of the unified installer and the starzel.buildout used at plone-trainings.
It is a bit more modulized and the base directory consits of specific buildout configs for deployment and development as well as a base.cfg witch commen settings for all builds.

Buildout-Structure
------------------

Base-Scripts
............

* bootstrap.py (deprecated) - legacy support to bootstrap the buildout structure
* buildout.cfg should not exist or is a link to the actual used config

Common-Configs
..............

* secrets.cfg.tmpl - a template to generate a secrets.cfg, which is necessary for setting up the zope default admin
* secrets.cfg <-- should not exists on checkout, never add to version controll (is in .gitignores)
* bin/
* buildout.d/ explained in detail later
* develop-eggs/
* docs/
* etc/
* include/
* lib/
* parts/
* vars/

Production
..........

* zeoserver-production.cfg
* zeoserver-failover-production.cfg
* zope-clients-production.cfg

Staging
.......

* zeoserver-staging.cfg
* zeoserver-failover-staging.cfg
* zope-clients-staging.cfg

Development
...........

* develop.cfg a minimal development buildout
* develop_fullstack.cfg a buildout that contains all components from the Zope Stack so a Zeo-Cluster


Sub-Structure
.............

* buildout.d/
* bin/
* vars/
* parts/

buildout.d
~~~~~~~~~~


bin
~~~

vars
~~~~

parts
~~~~~



Setup and install Plone
-----------------------

This buildout is used by LMU to bootstrap the production and staging environment via Ansible-Playbooks (see https://github.com/loechel/lmu.ansible.playbooks).
If you use this buildout manually please follow these steps:

# download the buildout
# setup a virtualenv (python2.7) inside the buildout

.. code:: bash

    virtualenv .
    source bin activate

# update pip setuptools and wheel if necessary

.. code:: bash

    pip install -U pip setuptools wheel

# install zc.buildout via pip or easyinstall or via bootstrapping zc.buildout

**pip:**

.. code:: bash

    pip install zc.buildout

or **easy_install**

.. code:: bash

    easy_install zc.buildout

or bootstrapping **zc.buildout**

.. code:: bash

    python bootstrap.py

Attention this might require a buildout.cfg file or symlink to exists.
See following commands

# symlink your prefered buildout config to buildout.cfg, this is probably develop.cfg (for development)

.. code:: bash

    ln -s develop.cfg buildout.cfg

# create a secrets.cfg from the secrets.cfg.tmpl and supply login and password for zope default admin

for production:

.. code:: bash

    cp secrets.cfg.tmpl secrets.cfg
    vim secrets.cfg

for development you might stay on those very unsecure default passwords:

.. code:: bash

    ln -s secrets.cfg.tmpl secrets.cfg

# run buildout

.. code:: bash

    ./bin/buildout

# now you could start your Plone Instance, based on the buildout:

For develop.cfg it is:

.. code:: bash

    ./bin/instance fg

for production / staging setups it is normally supervisord via system-supervisord:

.. code:: bash

    system supervisord start
    # or
    sudo /etc/init.d/supervisord start
