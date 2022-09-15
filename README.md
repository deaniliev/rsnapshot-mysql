# rsnapshot-mysql

This is a bash script that allows to automatically pull ALL databases from MySQL/MariaDB servers, remote or local, for backup purposes, creating "One Dump File Per Table" and a convenient restore script for each database. Plus, this script is [rsnapshot](https://github.com/rsnapshot/rsnapshot)-friendly, meaning you can use it with the "backup_script" feature of *rsnapshot*.

Features:
  - Handle dumps from local or remote MySQL hosts.
  - Allows to choose compression type for dumps (none, gzip or bzip2).
  - Automatically fetches database names from mysql host and creates a directory for each database.
  - Dump each table to its own file (.sql, .sql.gz or .sql.bz2) under a directory named as the database.
  - Handle dump of mixed database tables using MyISAM AND/OR InnoDB...
  - Ready to work with "backup_script" feature of rsnapshot, an incremental snapshot utility for local and remote filesystems.
  - Creates a convenient restore script (BASH) for each database, under each dump directory.
  - Creates backup of GRANTs (mysql permissions), and info files with the list of tables and mysql version.


Forked for the following reason:

Author used CONCAT function to produce list of database.tables.engine. 
From https://dev.mysql.com/doc/refman/5.7/en/string-functions.html#function_concat :

  CONCAT() returns NULL if any argument is NULL. 

Databases that I had to dump contained several VIEWs. The engine listed in a VIEW is NULL. Because of that the list contained several NULL values and did not dump correctly the table views. Modified it to use CONCAT_WS and if engine is empty to check if table is actually a VIEW. If it is - dump it with --no-data option.
