.. _lin:

Debian/Ubuntu
=============

Compatibility
-------------

Debian/Ubuntu 14.04 and up - anything that's *not* end-of-life

.. warning:: Django 1.11 used in Kolibri isn't compatible with Python 3.7. You will need to downgrade to Python 3.4-3.6, set them to run in parallel, or use Python 2.7. For a workaround and more details `read this thread <https://github.com/learningequality/kolibri/issues/4182#issuecomment-419408270>`_.

.. _ppa:

Install from PPA repository
---------------------------

Use the following commands in `Terminal <https://help.ubuntu.com/community/UsingTheTerminal>`_ to add the PPA and install Kolibri:

.. code-block:: bash

    sudo apt-get install software-properties-common python-software-properties
    sudo add-apt-repository ppa:learningequality/kolibri
    sudo apt-get update
    sudo apt-get install kolibri


.. _lin_deb:

Install from a .deb file
------------------------

The advantages of downloading a ``.deb`` file is the portability: you can copy the file from device to device and install Kolibri without internet access.

#. Download the latest `.deb installer <https://learningequality.org/download/>`_ for Kolibri **version 0.12**, or have it copied to your local drive.
#. Run this command from the location where you downloaded the ``DEB`` file:

   .. code-block:: bash

       sudo dpkg -i kolibri-installer-filename.deb

#. Wait for the installation to finish and run this command to start Kolibri:

   .. code-block:: bash

       kolibri start

   .. note:: If you choose to install Kolibri as a system service, you will not need to start it manually.

#. When the command finishes, open the default browser at http://127.0.0.1:8080 and proceed with the :ref:`setup_initial` of your facility. 


Uninstall
---------

* Open **Software** on Ubuntu and locate the Kolibri. Press **Remove**.
* Or from the command line: ``sudo apt-get remove kolibri``.


Upgrade
-------

When you use the PPA installation method, upgrades to newer versions will be automatic, provided there is internet access available.

To upgrade Kolibri on a Debian device without internet access, bring the updated ``.deb`` file and follow the same steps as in :ref:`lin_deb`.


.. _changing-system-user:

Changing the owner of Kolibri system service
--------------------------------------------

The *system service* is the script that runs Kolibri in the background when your system boots on Debian-based distributions.

You may need to change the system service to run with the permissions of a different user account. Prior to v0.10, ``kolibri`` user account was the owner of the system service, while from v0.10 and later, desktop user's account is preferred, in order for Kolibri to access the local USB storage.

To change the system service owner, you need to change the configuration of the system service: move the ``.kolibri`` data folder (containing channels, databases etc.), and assign owner permissions to the new user. Follow these steps.

.. code-block:: bash

	# Stop Kolibri
	sudo systemctl stop kolibri
	# Move data to your desktop user:
	sudo mv /var/kolibri/.kolibri /home/$USER/.kolibri
	# Change ownership
	sudo chown -R $USER /home/$USER/.kolibri
	# Change the username configuration
	sudo sh -c 'sudo echo -n $USER > /etc/kolibri/username'
	# Start Kolibri again
	sudo systemctl start kolibri

.. note:: Replace the ``$USER`` in commands above with the name of the user you wish to be the new Kolibri system service owner.
