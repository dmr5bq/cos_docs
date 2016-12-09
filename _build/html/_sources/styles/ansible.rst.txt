========================
Ansible Style Guidelines
========================

Ansible
=======

.. seealso::
	If you are not familiar with Ansible, additional resources can be found on the `Ansible Wikipedia page <https://en.wikipedia.org/wiki/Ansible_(software)>`_, the `official website <https://www.ansible.com/>`_, or on the `official Ansible git repository <https://github.com/ansible/ansible>`_.

.. note::
	Ansible is coordinated using the YAML language, which is a human-readable data serialization language, which has similar uses to XML or JSON (which are more commonly recognized.) 

	A full YAML syntax guide can be found on the `YAML Wikipedia page <https://en.wikipedia.org/wiki/YAML#Syntax>`_ or on this `YAML quick-reference sheet <http://www.yaml.org/refcard.html>`_.

- Use `dictionary syntax <http://docs.ansible.com/ansible/YAMLSyntax.html#yaml-basics>`_ when writing YAML blocks for `Ansible Playbooks <http://docs.ansible.com/ansible/playbooks.html>`_.

.. code-block:: yaml

	# YAML dictionary syntax declaration
    - name: Ensure proper permissions on apps directory
      file:
        path: "/opt/apps/"
        mode: 0755
        group: "osf"
        owner: "www-data"


- *Note*: In opposition to the Ansible style guide, do *not* prefix task names with the name of the role. (Learn more about Ansible Playbook roles on the `Ansible docs <http://docs.ansible.com/ansible/playbooks_roles.html#roles>`_).

.. code-block:: yaml

    # YES
    - name: Make user python is installed
      apt: name="python-dev"

    # NO
    - name: uwsgi | Make user python is installed
      apt: name="python-dev"

- Prefix all default variables with the role name and an underscore.

.. code-block:: yaml

    # OSF role

    osf_virtualenv: "/opt/envs/osf/"
    osf_repo_branch: "master"


- Document all default variables using comments.

.. code-block:: yaml

    # OSF role
 
    osf_virtualenv: "/opt/envs/osf/" # directs the virtual environment path to the OSF virtualenv
    osf_repo_branch: "master" # directs Git to find the master branch of the OSF repository



