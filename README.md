Code status:
------------

[![Hosted By: Cloudsmith](https://img.shields.io/badge/OSS%20hosting%20by-cloudsmith-blue?logo=cloudsmith&style=for-the-badge)](https://cloudsmith.com)

Package repository hosting is graciously provided by  [Cloudsmith](https://cloudsmith.com).
Cloudsmith is the only fully hosted, cloud-native, universal package management solution, that
enables your organization to create, store and share packages in any format, to any place, with total
confidence.

## Debian Repository

- [x] linux/s390x

### Debian bullseye (11)

```sh
# MariaDB 10.6.16
wget -qO - https://dl.cloudsmith.io/public/jumpserver/mariadb-10-6-16/gpg.B47668A04ABEE742.key | gpg --dearmor > /etc/apt/trusted.gpg.d/jumpserver-mariadb.gpg

# Debian bullseye(11)
echo "deb [arch=s390x] https://dl.cloudsmith.io/public/jumpserver/mariadb-10-6-16/deb/debian bullseye main" > /etc/apt/sources.list.d/jumpserver-mariadb.list
```

```sh
# MariaDB 10.11.6
wget -qO - https://dl.cloudsmith.io/public/jumpserver/mariadb-10-11-6/gpg.714A71B00036D18E.key | gpg --dearmor > /etc/apt/trusted.gpg.d/jumpserver-mariadb.gpg

# Debian bullseye(11)
echo "deb [arch=s390x] https://dl.cloudsmith.io/public/jumpserver/mariadb-10-11-6/deb/debian bullseye main" > /etc/apt/sources.list.d/jumpserver-mariadb.list
```

### Debian bookworm (12)
```sh
# MariaDB 10.6.16
wget -qO - https://dl.cloudsmith.io/public/jumpserver/mariadb-10-6-16/gpg.B47668A04ABEE742.key | gpg --dearmor > /etc/apt/trusted.gpg.d/jumpserver-mariadb.gpg

# Debian bookworm(12)
echo "deb [arch=s390x] https://dl.cloudsmith.io/public/jumpserver/mariadb-10-6-16/deb/debian bookworm main" > /etc/apt/sources.list.d/jumpserver-mariadb.list
```

```sh
# MariaDB 10.11.6
wget -qO - https://dl.cloudsmith.io/public/jumpserver/mariadb-10-11-6/gpg.714A71B00036D18E.key | gpg --dearmor > /etc/apt/trusted.gpg.d/jumpserver-mariadb.gpg

# Debian bookworm(12)
echo "deb [arch=s390x] https://dl.cloudsmith.io/public/jumpserver/mariadb-10-11-6/deb/debian bookworm main" > /etc/apt/sources.list.d/jumpserver-mariadb.list
```

* [![Appveyor CI status](https://ci.appveyor.com/api/projects/status/4u6pexmtpuf8jq66?svg=true)](https://ci.appveyor.com/project/rasmushoj/server) ci.appveyor.com

## MariaDB: The open source relational database 

MariaDB was designed as a drop-in replacement of MySQL(R) with more
features, new storage engines, fewer bugs, and better performance.

MariaDB is brought to you by the MariaDB Foundation and the MariaDB Corporation.
Please read the CREDITS file for details about the MariaDB Foundation,
and who is developing MariaDB.

MariaDB is developed by many of the original developers of MySQL who
now work for the MariaDB Corporation, the MariaDB Foundation and by
many people in the community.

MySQL, which is the base of MariaDB, is a product and trademark of Oracle
Corporation, Inc. For a list of developers and other contributors,
see the Credits appendix.  You can also run 'SHOW authors' to get a
list of active contributors.

A description of the MariaDB project and a manual can be found at:

https://mariadb.org

https://mariadb.com/kb/en/

https://mariadb.com/kb/en/mariadb-vs-mysql-features/

https://mariadb.com/kb/en/mariadb-versus-mysql-compatibility/

https://mariadb.com/kb/en/new-and-old-releases/

Help
-----

More help is available from the Maria Discuss mailing list
https://launchpad.net/~maria-discuss, MariaDB's Zulip
instance, https://mariadb.zulipchat.com/ 

Live QA for beginner contributors
----
MariaDB has a dedicated time each week when we answer new contributor questions live on Zulip.
From 8:00 to 10:00 UTC on Mondays, and 10:00 to 12:00 UTC on Thursdays,
anyone can ask any questions they’d like, and a live developer will be available to assist.

New contributors can ask questions any time, but we will provide immediate feedback during that interval.

Licensing
---------

***************************************************************************

NOTE: 

MariaDB is specifically available only under version 2 of the GNU
General Public License (GPLv2). (I.e. Without the "any later version"
clause.) This is inherited from MySQL. Please see the README file in
the MySQL distribution for more information.

License information can be found in the COPYING file. Third party
license information can be found in the THIRDPARTY file.

***************************************************************************

Bug Reports
------------

Bug and/or error reports regarding MariaDB should be submitted at:
https://jira.mariadb.org

For reporting security vulnerabilities, see our [security-policy](https://mariadb.org/about/security-policy/).

The code for MariaDB, including all revision history, can be found at:
https://github.com/MariaDB/server