========================================
Version Control Standards and Guidelines
========================================

Version Control
===============

General Guidelines
******************

We use `Git <https://git-scm.com/>`_ for version control. 

General Guidelines for Git VCS:
	* Follow the :ref:`git-style`, found below.
	* Use Vincent Driessen's `Successful Git Branching Model <http://nvie.com/posts/a-successful-git-branching-model/>`_ (this is also knows as Git-Flow).
	* In your **feature** branches, pull from ``develop`` frequently to keep an updated copy and to avoid potential conflicts.
	* **DO NOT** merge ``develop`` into ``hotfix`` branches, see more below.
	


Useful Tools
************

- `SourceTree <http://www.sourcetreeapp.com/>`_ - Mac OS X; GUI.
- `gitflow <https://github.com/nvie/gitflow>`_ - Cross-platform; CLI.
- `hub <https://github.com/github/hub>`_ - GitHub integration with the git CLI. Very useful for checking out pull requests.


Releases: Features
******************

- Once ``develop`` is ready for release, start a release branch with git-flow.

::

    $ git flow release start 0.17.0

- Update the CHANGELOG and bump the version where necessary. Commit changes.
- Finish the release with git-flow

::

    $ git flow release finish 0.17.0

- If prompted to add a tag message, write ``See CHANGELOG``.
- Push ``develop`` and ``master``. Push tags.


::

    $ git push origin develop
    $ git push origin master
    $ git push --tags

- Once Travis tests pass, deploy to production and staging.



Releases: Package and Application Maintainers
*********************************************

* Package and app maintainers: Use `semantic versioning <http://semver.org>`_.

Hotfix releases
---------------

- Once a hotfix pull request has been checked out locally and :ref:`code review <code_review>` is complete, rename the hotfix branch with the incremented PATCH number.

::

    # hotfix/fix-serialization-bug is currently checked out
    # rename with version number
    $ git branch -m hotfix/0.16.3

- Finish the hotfix with git-flow.

::

    $ git flow hotfix finish 0.16.3

- When prompted to add a tag message, write a brief (1-2 sentence) description of the hotfix.


.. note::

    You can also use the ``hotfix`` invoke task to automatically rename the current hotfix branch and finish it. ::

        $ invoke hotfix --finish

- Push ``develop`` and ``master``. Push tags with your commit.


::

    $ git push origin develop
    $ git push origin master
    $ git push --tags

- Once Travis tests pass, deploy to production and staging.

.. seealso::
	
	If you're unfamiliar with Travis testing, you can learn more on the `official website <https://travis-ci.org/>`_.

.. warning ::

	Many git-flow tools will try to force you to merge hotfix branches into master *before* publishing code to a hosting service (e.g. Github). Do **not** do this. Just push your hotfix branch on its own and :ref:`send a pull request <pull_requests>`. ::

	    # Push a new hotfix branch up to origin
	    $ git push -u origin hotfix/nav-header-size




.. _git-style:

Git Style Guidelines
====================

- Use the imperative mode (e.g, "Fix rendering of user logs") in commit messages.
- If your patch addresses a JIRA ticket, add the JIRA ticket ID to the commit message.

::

  Improve UI for changing names

  - Change button color for Auto-fill
  - Add help text

  [#OSF-4251]

- If your patch fixes a Github issue, you can add the issue to your commit message so that the issue will automatically be closed when the patch is merged.

::

  Fix bug in loading filetree

  [fix CenterForOpenScience/osf.io#982]

- Write `good commit messages <http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html>`_.

Here's a model message, taken from the above post: ::

    Capitalized, short (50 chars or less) summary

    More detailed explanatory text, if necessary.  Wrap it to about 72
    characters or so.  In some contexts, the first line is treated as the
    subject of an email and the rest of the text as the body.  The blank
    line separating the summary from the body is critical (unless you omit
    the body entirely); tools like rebase can get confused if you run the
    two together.

    Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
    or "Fixes bug."  This convention matches up with commit messages generated
    by commands like git merge and git revert.

    Further paragraphs come after blank lines.

    - Bullet points are okay, too

    - Typically a hyphen or asterisk is used for the bullet, followed by a
      single space, with blank lines in between, but conventions vary here

    - Use a hanging indent

