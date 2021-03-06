..
 This work is licensed under a Creative Commons Attribution 3.0 Unported
 License.

 http://creativecommons.org/licenses/by/3.0/legalcode

==================
Backup Master Node
==================

https://blueprints.launchpad.net/fuel/+spec/backup+master+node


Because Fuel Master HA is viewed as a waste of a user's resources, we need
to provide value by allowing for backup/recovery for disaster recovery
scenarios. Now that Fuel Master is running on Docker containers, backup and
recovery are quite painless and simple.

Problem description
===================

A detailed description of the problem:

* Fuel Master currently cannot be backed up or restored

* Reconfiguration of the Fuel Master requires significant manual input

Proposed change
===============

Fuel Master backup and recovery can be simplified by use of scripts and a
simple mechanism to compress and save the archive wherever the user requests.

Backup will take place with powered down containers to ensure consistent state.

Recovery in its first stage of implementation will be simplified. It will not
include astute.yaml settings (IP addresses, DHCP settings, DNS, NTP, etc). It
will simply restart the Docker containers to the backed up state, plus restore
all configurations, Puppet manifests, and package repositories.

Alternatives
------------

Backup and restore can be done with docker-0.10 without freezing running
containers, but it may result in inconsistent data.

Using docker-0.12 will allow freezing containers to save running state without
any destructive risks.

Data model impact
-----------------

Changes which require modifications to the data model often have a wider impact
on the system.  The community often has strong opinions on how the data model
should be evolved, from both a functional and performance perspective. It is
therefore important to capture and gain agreement as early as possible on any
proposed changes to the data model.

Questions which need to be addressed by this section include:

* None

REST API impact
---------------

Each API method which is either added or changed should have the following

* None

Upgrade impact
--------------

This will impact upgrades. The scope of this feature is not so extensive to
cover restoring an old version of Fuel onto a newer installed Fuel Master. In
most cases, this would downgrade every component on the system except Fuel
Master base host packages. A workaround could be devised in a future blueprint,
but certainly not in the time frame of 5.1.

Security impact
---------------

None

Notifications impact
--------------------

None

Other end user impact
---------------------

The user will interact with backup and restore via the dockerctl command
line utility. All containers will be shut down during the backup process.
The backup will fail to start if there are any incomplete tasks present
when running *fuel task --list*.

Performance Impact
------------------

Minimal. There will be performance hits during backup process, resulting in
downtime.

Other deployer impact
---------------------

Discuss things that will affect how you deploy and configure Fuel
that have not already been mentioned, such as:

* What config options are being added? Should they be more generic than
  proposed? Are the default values ones which will work well in
  real deployments?

Default backup path /var/backup/fuel
The backup ID will be generated from a timestamp of YYYY-MM-DD-hh_ss.
The backup will be 'tar'ed then compressed with lrzip due to its efficiency
in handling deduplication across large archives.

* Is this a change that takes immediate effect after its merged, or is it
  something that has to be explicitly enabled?

Yes. Immediate effect, but no backups are automatic.

Developer impact
----------------

Discuss things that will affect other developers working on Fuel,
such as:

* There will be an impact on dockerctl config dependency on container names.

Implementation
==============

Assignee(s)
-----------

Primary assignee:
  raytrac3r

Feature Lead: raytrac3r
Mandatory Design Reviewers: vkuklin
Developers: raytrac3r
QA: ykotko

Other contributors:
  None

Work Items
----------

Work items or tasks -- break the feature up into the things that need to be
done to implement it. Those parts might end up being done by different people,
but we're mostly trying to understand the timeline for implementation.

* Add backup feature to create archive of all containers, repositories, logs,
  and puppet manifests.
* Add restore feature to overwrite all containers, repositories, logs,
  and puppet manifests.
* User documentation on how to backup and restore.
* (Nice to have) backup via rsync, ftp, or http.


Dependencies
============

None.

Testing
=======

Automated tests for backup/save need to be added to current Fuel system tests.

Acceptance criteria:
* User can deploy multinode OpenStack and run a backup.

* User can deploy HA OpenStack and run a backup.

* User can install Fuel Master on a new host with the same network
  configuration and then restore the backup.

* User can manage all existing environments (delete node, add node).

* User can deploy new OpenStack environments.

Documentation Impact
====================

User-facing docs are required to show users the different ways to perform
the back up and restore.

References
==========

None
