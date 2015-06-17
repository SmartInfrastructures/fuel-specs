.. -*- coding: utf-8 -*-

..
 This work is licensed under a Creative Commons Attribution 3.0 Unported
 License.

 http://creativecommons.org/licenses/by/3.0/legalcode

=============================================
Nova-Docker: Docker driver for OpenStack Nova
=============================================

We would like to develop a new Fuel plugin in order to enable the Docker hypervisor support. We will integrate  the community driver, available at [1]. 

The nova-docker environment consists of two parts:

- one or more compute nodes should be configured to support Docker. 
- the docker images are managed by Glance


Problem description
===================
Starting from Havana release, a Nova compute driver has been developed. Following its installation guide [1], we should configure one or more compute host for Docker. Due to a known issue [2] that prevents to load the image from glance, on each Docker host we should copy a Docker image (raw) on the host’s local filesystem. Docker will be installed only on hosts that contain in their name the string “docker”. Otherwise, it will be installed on all available hosts.

Nova-docker installation
------------------------
Installation flow:

- Install docker on compute node
- Install nova-docker driver on compute node
- Install docker on controller node
- Configure glance to accept Docker container format

For any further information regarding the installation process, please visit [1].

Proposed change
===============

We would like develop a new Fuel plugin in order to install nova-docker. 
Our proposal considers the following aspects:

- Install nova-docker (docker and nova-docker driver) on all compute nodes or specified ones
- Install docker on all controller nodes
- Configure glance to accept docker images


Alternatives
------------

None.  Currently, there are not other available solutions.


Data model impact
-----------------

None


REST API impact
---------------

None


Upgrade impact
--------------

None.


Security impact
---------------

The default admin user name and password for the web interface will be
configured in the setting tab of the Fuel UI.

In the Fuel UI will be possible to allow the deploy of the REST API and web
application on the public network.


Notifications impact
--------------------

There will be a deployment successful message displaying the text pointing to
the URL of the web application.

We can also add some info to the `Post Deployment Dashboard
<https://review.openstack.org/#/c/180181/>`_ once it is implemented.


Other end user impact
---------------------

None

Performance Impact
------------------

None


Other deployer impact
---------------------

None


Developer impact
----------------

None


Infrastructure impact
---------------------

The compute node change its driver and it will be no longer able to
run kvm machines.


Implementation
==============


Assignee(s)
-----------

Primary assignee:
  Alessandro Martellone <amartellone@create-net.org>

Other contributors:
  Daniel Depaoli <daniel.depaoli@create-net.org>


Work Items
----------

Dependencies
============

- Fuel 6.1.


Testing
=======

- Prepare a test plan.
- Test the plugin by deploying environments with all Fuel deployment nodes.
- Create integration tests.


Documentation Impact
====================

None.  It will be a Fuel plugin with its own documentation.


References
==========

.. [1] https://github.com/stackforge/nova-docker
.. [2] https://ask.openstack.org/en/question/55125/which-version-of-nova-docker-should-be-used-with-openstack-juno/
