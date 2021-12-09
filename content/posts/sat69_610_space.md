---
title: "Satellite 6.9 to 6.10 Space Requirement's"
date: 2021-12-06T16:19:14Z
draft: false
---

With the release of [Satellite 6.10](https://access.redhat.com/documentation/en-us/red_hat_satellite/6.10) there are some considerations for space for the migration.  This is due to the removal of Mongo DB with the data being migrated into the postgres database and the migration of the pulp system from Pulp 2 to Pulp 3.

If you are currently running Satellite 6.9 and wish to check the space requirements they are:
* Ensure you have double the currently used space in /var/lib/pulp/published/ available
* Ensure you have free space in /var/lib/pulp/media equivalent to the currently used space in /var/lib/pulp/content
* Ensure the mount for postgresql12 has the free space available equivalent to the currently used space in /var/lib/mongodb/

To help you check and understand these requirments I have created a simple bash script to check these requirements.

The script can be run on a Satellite 6.9 Server to let you know the required space requirements and if your server currently meets these requirements.

To run the script:
* Log On to your Satellite Server
* ``` sudo -i # to gain root ```
* ``` curl -o 610_upgrade_space_check.sh https://people.redhat.com/~bmccaffe/scripts/610_upgrade_space_check.sh ```
* ``` chmod +x 610_upgrade_space_check.sh ```
* ``` ./610_upgrade_space_check.sh ```
* Review outputs

A sample of the output from a test machine is below:

```
root@sat6 ~]# ./610_upgrade_space_check.sh
Checking Migration State for ETA of migration
Running Retrieve Pulp 2 to Pulp 3 migration statistics
================================================================================
Retrieve Pulp 2 to Pulp 3 migration statistics:
============Migration Summary================
Migrated/Total RPMs: 0/151398
Migrated/Total errata: 0/27391
Migrated/Total repositories: 0/34
Estimated migration time based on yum content: 1 hours, 34 minutes

Note: ensure there is sufficient storage space for /var/lib/pulp/published to double in size before starting the migration process.
Check the size of /var/lib/pulp/published with 'du -sh /var/lib/pulp/published/'

Note: ensure there is sufficient storage space for postgresql.
You will need additional space for your postgresql database.  The partition holding '/var/opt/rh/rh-postgresql12/lib/pgsql/data/'
   will need additional free space equivalent to the size of your Mongo db database (/var/lib/mongodb/).
                                                                      [OK]
--------------------------------------------------------------------------------


##################################################

Checking Size of /var/lib/pulp/published/
9.8G

Upgrade Requires Double This Space to be avail for upgrade
19.6 G

Checking Avail Space In Mount Point /var/lib/pulp/published/
460G

[OK] The Pulp published mount has the required free space.

##################################################

Checking Size of /var/lib/pulp/content
11G

Upgrade Requires Double This Space to be avail for upgrade
22 G

Checking Avail Space In Mount Point /var/lib/pulp/media
460G

[OK] The Pulp media mount has the required free space.

This Install has the pulp media & published directorys on the same mount
Checking combined requirements for space are ok

Combined requirement is 41.6 G

[OK] The combined Pulp media & published mount has the required free space.

##################################################

Checking Size of /var/lib/mongodb/
8.5G

Checking Avail Space In Mount Point /var/opt/rh/rh-postgresql12/lib/pgsql/data/
460G

[OK] The postgresql12 mount has the required free space.

##################################################

[OK] All space checks passed.

##################################################

Done!

```

This script is provided for your convenience only and comes with no support from Red Hat in anyway.

You can review the script before running by using cat on the file or opening in your favorite text editor.