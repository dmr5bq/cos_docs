Preparing to Install the OSF
****************************

This section contains information on installations and configurations that must be made on your machine in order to develop for and run the Open Science Framework on your machine. 

.. note::
	These instructions are prepared for **researchers, technical staff, or developers new to Python**, and primarily focus on Mac OS (10.7 or later).

	If you are already familiar with Python, more compact instructions can be found in the OSF `README <https://github.com/CenterForOpenScience/osf.io/blob/develop/README.md>`_ file.

Preparing your Machine for Installation
+++++++++++++++++++++++++++++++++++++++

Before you can install the OSF, you will first need to install several pre-requisite programs. You may already have these installed, but instructions are provided to ensure that things are up-to-date.

Installing Homebrew
-------------------

`Homebrew <http://brew.sh/>`_ is a package manager that allows you to install many varieties of software easily, and it will greatly ease the process of installing OSF requirements. To see if Homebrew is already installed, open a new window in your terminal and type

    ::

        brew

If you see a list of options you already have homebrew. If not you will want to install homebrew globally, not just in your osf environment. To install it, open a new terminal window and run the following command.

For Mac OS users:

    ::

        ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Or for Linux users:

    ::

        ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/linuxbrew/go/install)"


Homebrew installation will ask you to press ``ENTER`` to continue and enter your password. When it's done installing type

    ::

        brew doctor

This will show any errors or other tasks that need to be done to complete installation. Homebrew is quite clear about what to do in these cases; usually you just need to copy and paste the provided commands and run them.

Installing Python
-----------------

Now that we have Homebrew, we can install Python.  Python is the programming language in which much of the OSF is written. Even if it is already on your computer, installing the newest version via Homebrew will avoid many common problems for new developers.  It will also automatically install pip, which is a common tool to manage Python packages.  

To install Python, go to your terminal and run:

    ::

        brew install Python


Updating the Path
------------------

Now that you have installed Homebrew, you will need to make a change to help your computer find the newly installed software.
This is done by editing the variable ``PATH`` in a file loaded whenever you open a new terminal window.

The file will usually be in your home directory (such as ``/Users/<your_username>``, commonly abbreviated as ``~``). If you are using bash this file could be .bash_profile, .bashrc, or .profile.
If you are using another shell like zsh you will need to add this section to the file .zshrc. Further tools installed later like virtualenvwrapper will work with bash, zsh or ksh.

.. note::

   You most likely have bash, and if you don't know what this means, `this article <http://natelandau.com/my-mac-osx-bash_profile/>`_  can explain its utility and purpose.

.. _bash:

Updating the Bash Profile
+++++++++++++++++++++++++

You can open your .bash_profile from the command line with:

    ::

        open -e ~/.bash_profile

If you get the error "The file .bash_profile does not exist," then run

    ::

        touch ~/.bash_profile
        open -e ~/.bash_profile

In your text editor, add the following line to your .bash_profile:

    ::

        PATH=/usr/local/bin:/usr/local/sbin:$PATH


Save the bash profile and close it.

To load the bash profile changes, enter:

    ::

        source ~/.bash_profile



Installing XCode and Java
-------------------------

You will also need the XCode command line tools and Java to install the OSF.  To install Xcode, in the terminal, execute:

    ::

        xcode-select --install

In order to install Java (via Homebrew), in the terminal, enter:

    ::

        brew install Caskroom/cask/java


Virtual Environments
--------------------

A common problem for software developers is dealing with different projects that all require different settings or library versions.
If your computer had only a single work environment shared by multiple projects, you would not be able to use two versions of a library simultaneously,
and updating a library for one project could break other things that depend on an older version or on legacy versions.

To avoid this problem, Python provides a tool called `virtual environments <http://docs.python-guide.org/en/latest/dev/virtualenvs/>`_ (called a 'virtualenv'). Since it is not always possible to avoid conflicts, and not often practical
to use a different computer for each thing you work on, it's best to use Python virtual environments. This lets you have separate, individually customized
Python setups for each thing you are working on.

You can install this tool using ``pip``, which is a tool for installing and managing Python packages. This seems like an extra layer of complexity, but working with libraries in a package format makes it much easier to manage and update your applications without having to handle low-level issues and complex integrations.


Installing a Python Virtual Environment with Virtualenv
+++++++++++++++++++++++++++++++++++++++++++++++++++++++

Virtualenv is the tool we use to isolate the Python environments for each project you need to run, helping to separate configurations and settings.

Open your Linux/Mac OS terminal and enter:

    ::

        pip install virtualenv


Installing Virtualenvwrapper
++++++++++++++++++++++++++++

Now that you installed virtualenv itself, we will also install a virtual environment management tool called virtualenvwrapper. Virtualenvwrapper wraps the virtual environments so that you can easily manage them and work with multiple environments at once without the headache of tracking each down separately. 

To install virtualenvwrapper, enter into the terminal:

    ::

        pip install virtualenvwrapper


To conclude the installation you need to add the following lines to the end of your bash profile. 

For how to open the bash profile file, see above in the :ref:`bash` section.

  ::

    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/Devel
    source /usr/local/bin/virtualenvwrapper.sh

The first line shows where the virtual environments are. If you installed virtualenv normally you shouldn't need to adjust this setting. The second line is the folder that has your development projects, this folder should exist before you do anything with virtualenvwrapper. Finally the third file is the location of the virtualenvwrapper.sh file.

.. note::

    If you don't know where a certain file is on your computer you can use the ``find`` command in the terminal. To search for ``virtualenvwrapper.sh`` file anywhere on your computer type the following:
    ::

        find / -name "virtualenvwrapper.sh"

Once you made the changes remember to load the changed file by typing:

    ::

        source ~/.bash_profile

Creating a Virtual Environment
++++++++++++++++++++++++++++++

You now have a development environment framework you can use for any of your projects. To start using OSF we will create a virtual environment for it.

First lets see which virtual environments you already have by using the command to show the short version of your existing environments.

    ::

        lsvirtualenv -b

You'll see that there isn't anything there yet. Let's create a virtual environment titled "try"

    ::

        mkvirtualenv try

When you make a virtual environment it will automatically enter that environment so to get out of that virtual environment type:

    ::

        deactivate

now when you run the ``lsvirtualenv`` command above you will see that "try" is listed. To start working on this virtual environment type

    ::

        workon try

The terminal lines will change to reflect that you are currently in that environment and should look something like ``(try)$ ...``.

You can switch environments by typing the name of another existing environment

    ::

        workon another

These commands work from within other environments. To get out of the virtual environment again type:

    ::

        deactivate

To delete a virtual environment enter:

   ::

        rmvirtualenv try

To create the OSF virtual environment and work on it. This will create and start the virtual environment.

    ::

        mkvirtualenv osf

Next time you need to start ``osf`` you will have to type:

    ::

        workon osf

Remember that the reason we created these environments is that next time we need to install something just for OSF we will go to the ``osf`` virtual environment we just created.

.. warning::
	
	The remainder of these instructions should be executed within the new ``osf`` virtual environment, unless explicitly stated that it should not take place in the virtualenv.

