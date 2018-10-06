restore-db-and-files
=========

This role picks up from the stancel.restore-bareos-backup role where files are restored to a computer or server that were backed up from some server or computer onto a Bareos or Bacula backup system. The role restores a Gzipped MySQL dump file into a MySQL/Percona/MariaDB DBMS and copies one or more file paths into the correct/final location paths needed to fully restore a working backup to the server you are running this role against. An example of a file path to copy over might be the web application files being copied over into the webserver's document root, but could be any path.


Requirements
------------

Need to already have MySQL / MariaDB / Percona Server and your webserver (Apache or Nginx) already setup and configured. The defaults assume a Debian based Linux (Ubuntu, Debian, etc.) with a default webserver document root of `/var/www/html`. You can override those default variables if that is not the case.

Role Variables
--------------

The Document Root or file path where Mantis files will be stored and served up by your webserver. The default path is `/var/www/html` and assumes you are running Apache2 on Debian or Ubuntu.

```
	web_files_path: "/var/www/html"
```
The linux username used by your webserver. The default value is `www-data` which assumes Apache is used on a Debian or Ubuntu linux.

```
	web_user: "www-data"
```
The linux group used by your webserver. The default value is `www-data` which assumes Apache is used on a Debian or Ubuntu linux.

```
	web_group: "www-data"
```
The root password for your MySQL, MariaDB or Percona Server DB instance to create the DB and user.
```
	mysql_root_password: "your MySQL root password"
```
The name of the database to restore
```
	db_name: "suitecrm"
```
Absolute file path where Bareos files were restored to
```
	path_to_restored_files: "/tmp/bareos-restores"
```
File paths of Bareos restored files that need to be copied to some other path
```
	filesets_to_restore:
  	  - "/var/www/html"
```
File path where DB dump file to restore is located
```
	db_backup_file_path: "{{ path_to_restored_files }}/backups/mysql/current"
```


Dependencies
------------

None

Example Playbook
----------------

	- hosts: your_new_server
	  vars_files:
	    - vars/main.yml
	  roles:
	    - { role: stancel.restore-db-and-files }


or 


	- hosts: your_new_server 
	  vars:
		mysql_root_password: "your-super-secure-pw"
		db_name: "your-db"
		path_to_restored_files: "/tmp/bareos-restores"
		filesets_to_restore:
  	      - /var/www/html
  	    db_backup_file_path: "{{ path_to_restored_files }}/backups/mysql/current"
	  roles:
	    - { role: stancel.restore-db-and-files }

License
-------

GPLv3

Author Information
------------------

Brad Stancel