Installing the OSF Software
***************************

.. _quick:

Easy Installation
+++++++++++++++++

After you have installed Homebrew, Python, XCode, Java, virtualenv, and virtualenvwrapper, you are ready to install the OSF.


**1.  Copy the OSF to your machine**

    Use the terminal to ``cd`` into the directory into which the OSF should be installed and enter:

    ::

        git clone  https://github.com/CenterForOpenScience/osf.io.git


**2.  From the terminal, enter your virtual environment**

    ::

        workon osf

**3.  Create local settings files**

    Navigate to the osf.io directory by entering:

    :: 

    	cd osf.io

    
    Now enter:

    ::

        cp website/settings/local-dist.py website/settings/local.py
        cp api/base/settings/local-dist.py api/base/settings/local.py

.. note::
    You may need to clear the ``WHEELHOUSE`` environmental variable for setup to function properly. The details of this are not particularly important; it just may need to be cleared.

    To do this, in the terminal, simply enter:

    ::

      unset WHEELHOUSE

**4.  Install** ``invoke`` **and then use it to start the setup process**

    ::

        pip install invoke==0.13.0
        invoke setup

.. note::
    Invoke is a Python task execution tool & library that provides a clean, high level API for running groups of shell commands and defining/organizing task functions from a ``tasks.py`` file.

If ``invoke setup`` fails for you, try the step-by-step instructions in the next section, :ref:`trad`.


----

.. _trad:

Traditional Installation
++++++++++++++++++++++++

If you haven't done so already, perform all the actions in the previous section, :ref:`quick`, omitting the ``invoke setup`` command in step 4. Although the automatic installer attempts to be helpful, it may sometimes be necessary to perform the installation steps individually — for example, if you are working on a machine running Linux, or updating an existing installation. 

.. note ::

	Te following step-by-step instructions are provided for Mac OS (using Homebrew), but other operating systems will have their own preferred package management tools.

Installing TokuMX
-----------------

TokuMX is the database that the OSF uses. It is a fork of MongoDB, which is a popular database application. Those who are familiar with PHP are more likely familiar with MySQL syntax and configurations, but databases are not language-specific.

To install TokuMX first refresh your brew installation and then use brew to install TokuMX. In the terminal, enter:

    ::

        brew tap tokutek/tokumx
        brew install tokumx-bin

Installing RabbitMQ
-------------------

To install RabbitMQ first refresh your brew install and then use brew to install RabbitMQ.  In the terminal, enter:

    ::

        brew update
        brew install rabbitmq

Installing libxml2 and libxslt
------------------------------

Enter in the terminal:

    ::

        brew install libxml2
        brew install libxslt


Install node packages with ``npm``
----------------------------------

``npm`` (node package manager) is used to install any required Node.JS packages. To install ``npm``, you have to install ``node.js``, which includes ``npm``. `official NPM documentation <http://blog.npmjs.org/post/85484771375/how-to-install-npm>`_ recommends doing this through the `Node.JS installer <https://nodejs.org/en/>`_.


Install front end dependencies with ``bower`` and ``npm``
---------------------------------------------------------

Several front end modules required by OSF are installed using bower. Bower is a front end package manager. To install bower, in the terminal, enter:

    ::

        npm install -g bower 
        # the -g flag specifies a global installation of bower
        
.. note::
        
    By default, ``npm`` installs packages locally, in the directory where you run the command (similar ``pip`` packages with ``virtualenv``). To install a package so that it's accessible from anywhere on your computer, you have to include the ``-g`` (global) flag.


Within your OSF folder install dependencies for OSF by running:

    ::

        bower install


Building Assets with WebPack
-----------------------------


``webpack`` is a module bundler that takes all of the modules (precompiler files are an example of modules, such as .scss, .less, and .coffee files) and turns them into static assets such as .css and .js files. ``webpack`` is more useful than other module bundlers in that it only loads the static assets that it needs to depending on the page visited, instead of compiling every module at the same time. 

The following command makes the modules available to webpack to be compiled whenever they're needed. In the terminal, enter:

::

    inv assets -dw

---- 

Installing Add-On Requirements
******************************

The OSF uses add-ons that provide diverse functionalities. You can decide to work with the add ons or without them. If you don't want add-ons you can turn them off. Otherwise, you will need to install the add-on requirements.

During your add-on installation some packages will be required and if you don't have them you will receive errors. To avoid errors, install the following before running any OSF software.

**Install xQuartz**

This is required for R installation. The xQuartz installation uses an installer that you can download from
`the xQuartz website <https://xquartz.macosforge.org/landing/>`_

**Install gfortran**

Gfortran will also be required for R installation and can be download as a package installer from `this website <https://gcc.gnu.org/wiki/GFortranBinaries>`_ .

**Install R**

Tap into the location where R installation exists within brew. Enter in the terminal:

    ::

        brew tap homebrew/science

Install R using Homebrew:

    ::

        brew install R

The following command will install the requirements for addons.

    ::

        invoke addon_requirements


----


**This concludes the installation of the OSF. Continue to the** :doc:`running_osf` **section to learn about running your local copy of the OSF**