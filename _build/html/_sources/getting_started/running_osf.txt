Running the OSF
===============

Quick Start
-----------
To run the OSF, each of the following terminal commands must be run. Many of these will run in the background and log output to the console.

1. ``cd`` into your OSF installation's root directory in a terminal window.
2. Open six new terminal tabs in the window; for Mac users, this can be done with ``⌘`` + ``T``.
3. Enter ``workon osf`` in **every tab** to start using your OSF-specific virtualenv.
4. Enter each of the following commands in a separate terminal tab:
    ::

            invoke mongo -d  # Runs mongodb as a daemon
            invoke mailserver
            invoke rabbitmq
            invoke celery_worker
            invoke elasticsearch
            invoke assets -dw
            invoke server


You now have both the database and application running. You will see the application address in the terminal window where you entered ``invoke server``. It will most likely be **http://localhost:5000/**. Navigate to this url in your browser to verify that it works.

To enable log-in to the OSF, you will also need to run an authentication server.

To do so, consult `the fakeCAS repository <https://github.com/CenterForOpenScience/fakeCAS>`_.

First download the binary file and run the commands specified to run the server.

.. seealso ::

	**To develop authentication-related features**, instructions for set-up and execution of a full CAS server ` are available here <https://github.com/CenterForOpenScience/docker-library/tree/master/cas>`_.

The Modular File Renderer (MFR) is used to render uploaded files to HTML via an inline frame (``<iframe>``) so that they can be
viewed directly on the OSF. Files will not be rendered if the MFR is not running. Consult the
MFR `repository <https://github.com/CenterForOpenScience/modular-file-renderer>`_ for information on how to install
and run the MFR.

The API Server can be run using:
    ::

        invoke apiserver

Navigate to **localhost:8000/v2/** in your browser to go to the root of the browsable API. 
If the page looks strange, run python manage.py collectstatic to ensure that CSS files are deposited in the correct location.

.. warning::

	There are common documented problems that occur when running the OSF; if any errors occur or something is not right, see our :doc:`../help` section.



