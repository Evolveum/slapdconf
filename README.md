Slapdconf Project
=================

OpenLDAP configuration utilities. 
These utilities make configuration of OpenLDAP easier - especially after the 
switch to the OLC-stype configuration which uses the cn=config LDAP suffix.

Synopsis
========

$ sudo slapdconf list-suffixes
dc=evolveum,dc=com
dc=example,dc=com

$ sudo slapdconf get-suffix-prop dc=example,dc=com
olcDatabase : {2}mdb
olcDbDirectory : /var/lib/ldap/example
.... (shorted for clarity) ....

$ sudo slapdconf set-server-prop idle-timeout:120

$ sudo slapdconf get-server-prop
olcIdleTimeout : 120
olcLogLevel :
  stats
  stats2

Description
===========

There are two utilities in this project:

slapdconf - Command-line tool to configure a running OpenLDAP.
slapdadm  - Command-line tool to configure stopped OpenLDAP.

slapdconf
=========

This command-line tool is used to configure a runnint OpenLDAP server instance.
It uses LDAP protocol to change the cn=config subtree of an OpenLDAP server.
The configuration changes are applied without a server restart.

It can reconfigure the sever, create new directory suffixes, setup
replication, etc.

Examples:
---------
```
slapdconf -h myserver.example.com -D "uid=admin,ou=people,dc=example,dc=com" -w secret get-server-prop
slapdconf -Y EXTERNAL list-suffixes
slapdconf -Y EXTERNAL create-suffix dc=example,dc=com --dbDir /var/lib/ldap/dc=example,dc=com --rootPassword supersecret
```


slapdadm
========

This command-line tool is used to configure a stopped OpenLDAP instance. The configuration
is done by direct manipulation of files in C</etc/ldap/slapd.d> directory and the database
files.

It can be used for operations that slapdconf cannot do. E.g. it can claen-up the OpenLDAP
configuration that is provided by your Linux distribution and that somehow never quite
fits. Then a slapdconf tool can be used to replace it with a proper setup.

Examples:
---------
```
slapdadm delete-suffix dc=example,dc=com
slapdadm delete-all
```

Notes
=====

This is work in progress. Some commands are not yet implemented or implemented partially. Any kind of
contribution to make these tools a more complete solution is greatly appreciated. The primary source
code repository for these tools is at Github:

[https://github.com/Evolveum/slapdconf]

The tools are written in Perl. The slapdconf uses Net::LDAP module which is easy to use LDAP client.
The Perl was chosen because of its flexibility. It also looks like it is kind of a tradition to use
Perl for LDAP server administration tools.
