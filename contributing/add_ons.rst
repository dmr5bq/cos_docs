======================================
Developing Add-On Features for the OSF
======================================

This document lays out the process for developing an add-on feature to the OSF.

.. warning::

	**In Progress**: This document is still under construction. Check out :doc:`../documentation/todo` if you would like to contribute.


.. note::
	
	**Important Standards and Considerations**
		- The words SHALL, MUST, MAY, etc are to be interpreted as defined `here <https://tools.ietf.org/html/rfc2119>`_.
		- The add-on system is module-based not class-based.
		- Anything edited by a developer should be in the ``website/addons/`` directory.

Installing Add-ons
******************


Open terminal and ``cd`` into to the folder where your OSF installation is located. We will install the addons to the website folder. So navigate to:

::

    cd website/addons

During your installation you created a virtual environment for OSF — If this is unfamiliar, see :doc:`../getting_started/installing_osf`. Switch to the environment by typing ``workon`` followed by the name of your virtual environment

::

    # If you use virtualenvwrapper
    $ workon osf


Minimum Requirements
********************

-  ``__init__.py`` declares all views/models/routes/hooks for your add-on

**An add-on MUST declare the following in** ``__init__.py``:


-  ``SHORT_NAME`` (string)

   -  The name that will be used to refer to your add-on on the back end.
   -  Examples:

      -  Amazon Simple Storage Service is ``"s3"``
      -  Google Drive is ``"googledrive"``
----

-  ``FULL_NAME`` (string)

   -  The display name of your add-on. Whenever the user is
      interacting with your add-on this is the name they will see.
----

-  ``ROUTES`` (list of `routes`_ dictionaries)

   -  A list containing all `routes`_ defined by your add-on.
   -  Maps URLs to views in the add-on.

----

-  ``MODELS`` (list of `StoredObjects`_)

   -  A list of all `ODM objects <http://www.cdisc.org/standards/foundational/odm>`_ defined by your add-on.
   -  If a model is not in this list, it will not be usable in the add-on.
----

-  ``ADDED_DEFAULT`` (list of strings)

   -  A list of ``AddonMixin`` models that your add-on SHALL be added to
      when they are created
   -  Valid options are ``user`` and ``node``
   -  *Example:* The Wiki addon is added by default for nodes.
----

-  ``ADDED_MANDATORY`` (list of strings)

   -  A list of ``AddonMixin`` models that your add-on MUST be
      attached/connected to at all times.
   -  Valid options are ``user`` and ``node``
   -  *Example:* ``OsfStorage`` is a required add-on for nodes
----

-  ``VIEWS`` (list of strings)

   -  Additional builtin views for your add-on
   -  Valid options are ``page`` and ``widget``
   -  EX: The wiki defines both a ``page`` view and a ``widget`` view

----

-  ``CATEGORIES`` (list of strings)

   -  A list of categories this add-on should be displayed under when
      the user is “browsing” add-ons
   - SHOULD be ``documentation``, ``storage``, ``citations``, ``security``, ``bibliography``, or ``other``

       - Additional categories can be added to ``ADDON_CATEGORIES`` in ``website.settings.defaults``

----

-  ``INCLUDE_JS`` and ``INCLUDE_CSS``

   -  Deprecated field, define as empty dict (``{}``)

----

-  ``OWNERS`` (list of strings)

   -  Valid options are ``user`` and ``node``
----

-  ``CONFIGS`` (list of strings)

   -  Valid options are ``accounts`` and ``node``


.. note ::

	**Miscellaneous Technical Standards**
		- An `AddonSettings` object MUST NOT be instantiated by the contributor.
		- Don't forget to do error handling! This includes handling errors that might occur if 3rd party HTTP APIs cause a failure and any exceptions that a client library might raise
		- Any static assets that you put in ``website/addons/<addon_name>/static/`` will be served from ``/static/addons/<addon_name>/``. This means that ``<link>`` and ``<script>`` tags should always point to URLs that begin with ``/static/``.
		- `to_json` returns the mako context for the settings pages

Optional Fields
***************

**An add-on MAY define the following fields**

-  ``HAS_HGRID_FILES`` (boolean)

	-  A boolean that indicated that this add-on’s ``GET_HGRID_DATA`` function should be used to populate the files grid

----

-  ``GET_HGRID_DATA`` (function)

	-  A function that returns HGrid/Treebeard formatted data to be included in a project’s files grid

----

-  ``USER_SETTINGS_MODEL`` (`StoredObject`_)

	-  MUST inherit from ``website.addons.base.AddonUserSettingsBase``
	-  A model that will be used to store settings for users
	-  Will be returned when ``User.get_addon('YourAddon')`` is called
   	-  *Example:* Amazon S3’s User settings is used to store the user’s AWS access keys

----

-  ``NODE_SETTINGS_MODEL`` (`StoredObject`_)
	-  MUST inherit from ``website.addons.base.AddonNodeSettingsBase``
	-  A model that will be used to store settings for nodes
	-  Will be returned when ``Node.get_addon('YourAddon')`` is called

----

-  ``NODE_SETTINGS_TEMPLATE`` (string to directory)
	-  A `mako`_ template for configuring your add-on’s node settings object

----

-  ``USER_SETTINGS_TEMPLATE`` (string to directory)
	-  A `mako`_ template for configuring your add-on’s user settings object

----

-  ``MAX_FILE_SIZE``
	-  This maximum size, in MB, that can be uploaded to your add-on (supposing it supports files.)


Add-on Structure
***************

**An add-on SHOULD have the following folder structure**

::

    website/addons/addonshortname/
    ├── __init__.py
    ├── model.py
    ├── requirements.txt
    ├── routes.py
    ├── settings
    │   ├── __init__.py
    │   └── defaults.py
    ├── static
    │   ├── comicon.png
    │   ├── node-cfg.js*
    │   ├── tests
    │   │   └── ...
    │   └── user-cfg.js*
    ├── templates
    │   ├── log_templates.mako
    │   ├── addonshortname_node_settings.mako*
    │   └── addonshortname_user_settings.mako*
    ├── tests
    │   ├── __init__.py
    │   ├── test_model.py
    │   └── test_views.py
    └── views
        └── ...

        * optional



StoredObject
************

All models SHOULD be defined as subclasses of ``framework.mongo.StoredObject``.


Routes
******

Routes are defined in a dictionary containing ``rules`` and an optional ``prefix``.

Add-on URL templating works the same way that `flask’s`_ does.

.. code:: python

    my_route = {
      'rules': [
        Rule(
          [
            '/my/<templated>/path/',  # Note all routes SHOULD end with a forward slash (/)
            '/also/my/<templated>/path/'
          ],
          ('get', 'post'),  # Valid HTTP methods
          view.my_view_function,  # The view method this route maps to
          json_renderer  # The renderer used for this view function, either OsfWebRenderer or json_renderer
        )
      ]
    }

Routes SHOULD be defined in ``website.addons.youraddon.routes`` but could be defined anywhere

Views
*****

Add-on views are implemented the same way that `flask’s`_ are.

Any value matched by url templating (``<value_name>``) will be passed to
your view function as a keyword argument

Our framework supplies many python decorators to make writing view
functions more pleasant.

Below are a few examples that are commonly used in our code base.

More can be found in ``website.project.decorators``.

``framework.auth.decorators.must_be_logged_in`` ensures that a user is logged in and imputes ``auth`` into keyword
arguments

``website.project.decorators.must_have_addon`` is a decorator factory meaning you must supply
arguments to it to get a decorator.

.. code:: python

    @must_have_addon('myaddon', 'user')
    def my_view(...):
      pass


    @must_have_addon('myaddon', 'node')
    def my_node_view(...):
      pass

The above code snippet will only run the view function if the specified
model as the requested addon.

.. note::
    Routes whose views are with decorated ``must_have_addon('addon_short_name', 'node')`` MUST start with ``/project/<pid>/...``.

``website.project.decorators.must_have_permission`` is another decorator factory that takes a ``permission`` argument (may be 'write','read', or 'admin'). This prevents the decorated view function from being called unless the
user issuing the request has the required permission.


Logs
****

Some common log examples

- ``dropbox_node_authorized``
- ``dropbox_node_authorized``
- ``dropbox_file_added``
- ``dropbox_file_removed``
- ``dropbox_folder_selected``, ``github_repo_linked``, etc.

Use the ``NodeLog`` class's named constants when possible,

.. code-block:: python

    'dropbox_' + NodeLog.FILE_ADDED

Every log action requires a template in ``youraddon/templates/log_templates.mako``. Each template's id corresponds to the name of the log action.


Static files for add-ons
************************

.. todo:: Add detail.


First make sure your add-on's short name is listed in ``addons.json``.

**addons.json**

.. code-block:: json

    {
        "addons": [
            ...
            "dropbox",
            ...
        ]
    }

 This adds the proper entry points for webpack to build your add-on's static files.

The following files in the ``static`` folder of your addon directory will be built by webpack:

- user-cfg.js : Executed on the user addon configuration page.
- node-cfg.js : Executed on the node addon configuration page.
- files.js : Executed on the files page of a node.

**You do not have to include these files in a ``<script>`` tag in your templates.** They will dynamically be included when your addon is enabled.

Rubeus and the FileBrowser
**************************

For an addon to be included in the files view they must first define the following in the addon's ``__init__.py``:

.. code-block:: python

    HAS_HGRID_FILES = True
    GET_HGRID_DATA = views.hgrid.{{addon}}_hgrid_data


Has hgrid files is just a flag to attempt to load files from the addon.
get hgrid data is a function that will return FileBrowser formatted data.


Rubeus
------

Rubeus is a helper module for filebrowser compatible add ons.

``rubeus.FOLDER,KIND,FILE`` are rubeus constants for use when defining filebrowser data.

``rubeus.build_addon_root``:

Builds the root or "dummy" folder for an addon.

::

    :param AddonNodeSettingsBase node_settings: Addon settings

    :param str name: Additional information for the folder title

        eg. Repo name for Github or bucket name for S3

    :param dict or Auth permissions: Dictionary of permissions for the add-on's content or Auth for use in node.can_X methods

    :param dict urls: Hgrid related urls

    :param str extra: Html to be appended to the addon folder name

        eg. Branch switcher for github

    :param dict kwargs: Any additional information to add to the root folder

    :return dict: Hgrid formatted dictionary for the addon root folder

Addons using OAuth and OAuth2
-----------------------------

There are utilities for add-ons that use OAuth or Oauth2 for authentication. These include:

- ``website.oauth.models.ExternalProvider`` : a helper class for managing and acquiring credentials (see ``website.addons.mendeley.model.Mendeley`` as an example)
- ``website.oauth.models.ExternalAccount`` : abstract representation of stored credentials; you do not need to implement a subclass of this class
- ``website.addons.base.AddonOAuthUserSettingsBase`` : abstract interface to access user credentials (see ``website.addons.mendeley.model.MendeleyUserSettings`` as an example)
- ``website.addons.base.AddonOAuthUserSettingsBase`` : abstract interface for nodes to manage and  access user credentials (see ``website.addons.mendeley.model.MendeleyNodeSettings`` as an example)
- ``website.addons.base.serializer.AddonSerializer`` & ``website.addons.base.serializer.OAuthAddonSerializer``: helper classes to facilitate serializing add-on settings


Deselecting and Deauthorizing
-----------------------------

Many add-ons will have both user and node settings. It is important to ensure that, if a user's add-on settings are deleted or authorization to that add-on is removed, every node authorized by the user is deauthorized, which includes resetting all fields including its user settings.

It is necessary to override the ``delete`` method for ``MyAddonUserSettings`` in order to clear all fields from the user settings.

.. code-block:: python

    class MyAddonUserSettings(AddonUserSettingsBase):

        def delete(self):
            self.clear()
            super(MyAddonUserSettings, self).delete()

        def clear(self):
            self.addon_id = None
            self.access_token= None
            for node_settings in self.myaddonnodesettings__authorized:
                node_settings.deauthorize(Auth(self.owner))
                node_settings.save()
            return self

You will also have to override the ``delete`` method for ``MyAddonNodeSettings``.

.. code-block:: python


    class MyAddonNodeSettings(AddonNodeSettingsBase):

        def delete(self):
            self.deauthorize(Auth(self.user_settings.owner), add_log=False)
            super(AddonDataverseNodeSettings, self).delete()

        def deauthorize(self, auth, add_log=True):
            self.example_field = None
            self.user_settings = None

            if add_log:
                ...

IMPORTANT Privacy Considerations
********************************

Every add-on will come with its own unique set of privacy considerations. There are a number of ways to make small errors with a *large* impact.

General

- **Using** ``must_be_contributor_or_public``, ``must_have_addon``, **etc. is not enough.** While you should make sure that you correctly decorate your views, that does not ensure that *non-OSF*-related permissions have been handled.
- For file storage add-ons, make sure that contributors can only see the folder that the authorizing user has selected to share.
- Think carefully about security when writing the node settings view ({{addon}}_node_settings.mako / {{addon}}NodeConfig.js}}. For example, in the GitHub add-on, the user should only be able to see the list of repos from the authenticating account if the user is the authenticator for the current node. Most add-ons will need to tell the view (1) whether the current user is the authenticator of the current node and (2) whether the current user has added an auth token for the current add-on to her OSF account.

Example: When a Dropbox folder is shared on a project, contributors (and the public, if the project is public) should only perform CRUD operations on files and folders that are within that shared folder. An error should be thrown if a user tries to access anything outside of that folder.

.. code-block:: python

    @must_be_contributor_or_public
    @must_have_addon('dropbox', 'node')
    def dropbox_view_file(path, node_addon, auth, **kwargs):
        """Web view for the file detail page."""
        if not path:
            raise HTTPError(http.NOT_FOUND)
        # check that current user was the one who authorized the Dropbox addon
        if not is_authorizer(auth, node_addon):
            # raise HTTPError(403) if path is a not a subdirectory of the shared folder
            abort_if_not_subdir(path, node_addon.folder)
        ...

Make sure that any view (CRUD, settings views...) that accesses resources from a 3rd-party service is secured in this way.


.. _routes: #routes
.. _StoredObjects: #storedobject
.. _StoredObject: #storedobject
.. _mako: http://www.makotemplates.org/
.. _flask’s: http://flask.pocoo.org/docs/0.10/views/
