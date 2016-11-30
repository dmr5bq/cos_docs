=====================================
Pull Request Standards and Guidelines
=====================================

This document contains information on sending a `pull request <https://help.github.com/articles/about-pull-requests/>`_ on Github in order to merge your contributions into the OSF software.

.. _pull_requests:

Sending a Pull Request
======================

Use the following checklist to make sure your pull request can be reviewed and merged as efficiently as possible:

- For projects that use git-flow (such as the OSF): 

	- Feature branches should request to the ``develop`` branch. 
	- Hotfix branches should request to the ``master`` branch.

----

- New features must be :ref:`tested appropriately <testing>` (views, functional, and/or unit tests). Fixes should include regression tests.

----

- Your code must be sufficiently documented. Add docstrings to new classes, functions, and methods.

----

- Your code must be passing on TravisCI.

----

- On Github, rename your pull request title with the prefix ``"[feature]"``, ``"[feature fix]"`` (fixes to develop), or ``"[hotfix]"``, as appropriate.

----

- If you are sending the pull request for code review only and *not* for merge, add the ``"[WIP]"`` prefix to the pull request title.

----

- Write a descriptive pull request description (:ref:`see sample below <sample>`). Ideally, it should communicate:
    - Purpose
    	- The overall philosophy behind the changes you've made, so that, if there are questions as to whether an implementation detail was appropriate, this can be consulted before talking with the developer.
    	- Which Github issue the PR addresses, if applicable.
    - Changes. 
    	- The details of the implementation as you intended them to be. If you did front-end changes, add screenshots here.
    - Side effects. 
    	- Potential concerns, esp. regarding security, privacy, and provenance, which will requires extra care during review.

----

- Once your PR is ready, ask for code review on Flowdock.

.. note::

    Make sure to follow the :doc:`version_control` when using the Git VCS.

----

.. _sample :

Sample Pull Request Description
===============================

**Purpose**

Currently, the only way to add projects to your Dashboard's Project Organizer is from within the project organizer. There are smart folders with your projects and registrations, and you can search for projects from within the info widget to add to folders, but if you are elsewhere in the OSF, it's a laborious process to get the project into the Organizer. This PR allows you to be on a project and add the current project to the Project Organizer.

Closes Issue https://github.com/CenterForOpenScience/osf.io/issues/1186

**Changes**

Puts a button on the project header that adds the current project to the Dashboard folder of the user's Project Organizer. Disabled if the user is not logged in, if the folder is already in the dashboard folder of the user's project organizer, or if the user doesn't have permissions to view.

**Side Effects**

Also fixes a minor issue with the watch button count being slow to update.


