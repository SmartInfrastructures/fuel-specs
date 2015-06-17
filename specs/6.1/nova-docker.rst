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
A compute node is able to support multiple drivers [4]. 
Docker is an operating-system-level virtualization software and, opposite of the others
nova drivers, abstract the Linux Kernel. Docker aims to simplify the applications provisioning
and to avoid programmer to configure the environment.
Adding nova-docker to openstack permits to load a specific container already configured for
a specific application.

Some proposal has presented ([2] and [3]), but no implementations was developed.
These proposals doesn't use the plugin concept. Using a plugin it's possible
to choose the node configured for docker in a simple way, mantaining the compute
concept.


Nova-docker installation
------------------------
For more details about installation please refer to README present in [1]

Flow:

- Install docker on compute node
- Install nova-docker driver on compute node
- Install docker on controller node
- Configure glance to accept Docker container format


Proposed change
===============

We would like develop a new Fuel plugin in order to install nova-docker. 
Our proposal considers the following aspects:

- Install nova-docker (docker and nova-docker driver) on all compute nodes or specified ones
- Install docker on all controller nodes
- Configure glance to accept docker images


Alternatives
------------

None.  The solution presented in [2] and [3] are not implemented and don't 
consider the plugin architecture


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
.. [2] https://blueprints.launchpad.net/fuel/+spec/nova-docker-driver
.. [3] https://blueprints.launchpad.net/fuel/+spec/enable-nova-docker-driver
.. [4] https://wiki.openstack.org/wiki/HypervisorSupportMatrix