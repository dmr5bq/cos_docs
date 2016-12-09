===================================
Common Problems and Troubleshooting
===================================

Common Installation Problems
****************************

**1. Mongodb path /data/db does not exist**

    ::

        sudo mkdir -p /data/db/
        sudo chown `id -u` /data/db

**2. Unable to execute clang: No such file or directory**

Xcode Command Line Tools installation is missing or launch was not successful. Install (or reinstall) XCode.

**3. Unable to lock file: /data/db/mongod.lock**

If the TokuMX server is still running or if you turn off the computer without stopping the server the TokuMX lock file will cause errors. If you see an error like the one below:

    ::

        ...exception in initAndListen: 10310 Unable to lock file: /data/db/mongod.lock. Is a mongod instance already running?, terminating...

first check other terminals to see if TokuMX is running. If it isn't go to the folder  /data/db/mongod.lock and delete the file.

**4. RuntimeError: Broken toolchain: cannot link a simple C program OR clang: error: unknown argument: '-mno-fused-madd'**

Add the following to your bash profile document

    ::

        export CFLAGS=-Qunused-arguments
        export CPPFLAGS=-Qunused-arguments


**5. ImportError: No module named kombu.five**
This error is related to Celery and not part of OSF. Until the source code is improved what you can do is uninstall celery and reinstall using:

    ::

        pip uninstall celery
        pip install celery

**6. Incompatible library version: etree.so requires 12.0.0 or later**

If you have pip and conda installed, make sure remove lxml from conda and from pip. Then install again using conda.

    ::

        conda remove lxml
        pip uninstall lxml
        conda install lxml

**7.  Fatal error: 'libxml/xmlversion.h' file not found #include "libxml/xmlversion.h"**

Problem: Libxml installation fails with the error.

Solution: Xcode Command Line Tools installation is missing or was not successful.Install (or reinstall) XCode.

**8.  High disk watermark [10%] exceeded on ..., shards will be relocated away from this node**

Problem: Mongodb needs 10% of your total disk space to function with the OSF, and more then that to not throw a lot of warnings.

Solution:  Free up space on your disk.

**9. Import Error: cannot import name URITemplate**

Problem: github3.py needs uritemplate.py but conflicting package uritemplate is installed instead

Solution: Uninstall uritemplate and install uritemplate.py.

    ::

        pip uninstall uritemplate.py
        pip install uritemplate.py==0.3.0

**10. Error: Cannot write to /usr/local/Cellar**

Problem: Can't install packages because homebrew complains about permissions.

Solution: Take control, Gotham! Homebrew prefers to be run by one user, so you'll need to take ownership of it and homebrew-cask.  This assumes you have admin privileges.

    ::

        sudo chown -R <your username> /usr/local
        sudo chown -R <your username> /Library/Caches/Homebrew
        sudo chown -R <your username> /opt/homebrew-cask

**11. Error: Failed to download resource "tokumx-bin"**

Problem: The 2.0.0 version of "tokumx-bin" may not be available.

Solution: Manually update tokumx-bin.rb:

    ::

        brew edit tokumx-bin

Replace

    ::

        version "2.0.0"
        url "https://s3.amazonaws.com/tokumx-2.0.0/tokumx-2.0.0-osx-x86_64-main.tar.gz"
        sha1 "ad575f0868a778bca45eea404346e9823d6d5ef2"

with

    ::

        version "2.0.1"
        url "https://s3.amazonaws.com/tokumx-2.0.1/tokumx-2.0.1-osx-x86_64-main.tar.gz"
        sha1 "26f77ce6faa10c774d32a1a85aebc838c36b7e22"

Miscellaneous Runtime Problems
*******************************

**1. "Emails not working on my development server"**


Solution: You may not have a celery worker running. If you have Celery and RabbitMQ installed (see the `README <https://github.com/CenterForOpenScience/osf>`_ for installation instructions), run ``invoke celery``.

Less ideally, you can turn Celery off and send emails synchronously by adding ``USE_CELERY = False`` to your ``website/settings/local.py`` file.

**2. "My view test keeps failing"**


Solution: You have to reload the database record.

.. code-block:: python

    def test_change_name_view(self):
        user = UserFactory()
        # Hit some endpoint that updates the user's database record
        res = self.app.post_json('/{}/changename/'.format(user._primary_key),
            {'name': 'Freddie Mercurial'})
        user.reload()  # Make sure object is up to date
        assert_equal(res.status_code, 200)


**3. ImportError: No module named five**


Celery may raise an exception when attempting to run the OSF tests. A partial
traceback:

::

    Exception occurred:
      File "<...>", line 49, in <module>
        from kombu.five import monotonic
    ImportError: No module named five

error: [Errno 61] Connection refused` is raised in ampq/transport.py


Solution: You may have to start your Rabbitmq and Celery workers.

::

    $ invoke rabbitmq
    $ invoke celery_worker

**4. Error when importing uritemplate**


If invoking assets or server commands throw an error about uritemplate, run the following to resolve the conflict:

    ::

        pip uninstall uritemplate.py --yes
        pip install uritemplate.py==0.3.0

and then re run the command that failed.


Other Helpful Tips
******************

Using PyCharm's Remote Debugger

Some debugging tasks make it difficult to use the standard debug tools (i.e. pdb, ipdb, or PyCharm's debugger). 
Usually this is becuase you're running code in a way where you don't have ready access to the process's 
standard in/out. Examples of this include:

- celery tasks
- local testing/debugging using uWSGI

One way to debug code running in these kinds of enviornments is to use the PyCharm remote debugger. Follow the 
JetBrains documentation for creating a new run configuration for the remote debugger: https://www.jetbrains.com/pycharm/help/remote-debugging.html. At some point you may be required to add pycharm-debug.egg to your system's PYTHONPATH. The 
easist way to do this is to modify your ~/.bash_profile to automatically append this module to the python path. This looks like:

:: 
    
    export PYTHONPATH="$PYTHONPATH:<your_path_to>/pycharm-debug.egg"

To apply these changes to the current bash session, simply

::

   source ~/.bash_profile

When you start the remote debug server in PyCharm you will get some console output like:

::

    Use the following code to connect to the debugger:
    import pydevd
    pydevd.settrace('localhost', port=54735, stdoutToServer=True, stderrToServer=True)

So to use, simple copy paste the bottom two lines wherever you need to run a debugger. In celery tasks for example,
this often means inside a task definition where it would be otherwise impossible to step into the code. Trigger 
whatever is needed to queue the celery task, and watch the PyCharm console to see when a new connection is initiated. 


