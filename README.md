restore-db-and-files
=========

This role picks up from the stancel.restore-bareos-backup role where files are restored to a computer or server that were backed up from some server or computer onto a Bareos or Bacula backup system. The role restores a Gzipped MySQL dump file into a MySQL/Percona/MariaDB DBMS and copies one or more file paths into the correct/final location paths needed to fully restore a working backup to the server you are running this role against. An example of a file path to copy over might be the web application files being copied over into the webserver's document root, but could be any path.


Requirements
------------

Need to already have MySQL / MariaDB / Percona Server and your webserver (Apache or Nginx) already setup and configured. The defaults assume a Debian based Linux (Ubuntu, Debian, etc.) with a default webserver document root of `/var/www/html`. You can override those default variables if that is not the case.

Role Variables
--------------

The Document Root or file path where the files will be stored and served up by your webserver. The default path is `/var/www/html` and assumes you are running Apache2 on Debian or Ubuntu.

First part => *restore_db_and_files_web_files_path:* is the root directory of your webserver

Second part =>  *restore_db_and_files_web_directory_for_application:* is the application directory inside the root directory

!Be aware of the starting / !

```
    restore_db_and_files_web_files_path: "/var/www"
    restore_db_and_files_web_directory_for_application: "/html"
```
The linux username used by your webserver. The default value is `www-data` which assumes Apache is used on a Debian or Ubuntu linux.

```
	restore_db_and_files_web_user: "www-data"
```
The linux group used by your webserver. The default value is `www-data` which assumes Apache is used on a Debian or Ubuntu linux.

```
	restore_db_and_files_web_group: "www-data"
```
The root password for your MySQL, MariaDB or Percona Server DB instance to create the DB and user.
```
	restore_db_and_files_mysql_root_password: "your MySQL root password"
```
The name of the database to restore
```
	restore_db_and_files_db_name: "my-db-name-to-restore"
```
Absolute file path where Bareos files were restored to
```
	restore_db_and_files_path_to_restored_files: "/tmp/bareos-restores"
```
File paths of Bareos restored files that need to be copied to some other path
```
	restore_db_and_files_filesets_to_restore:
  	  - "{{ restore_db_and_files_web_files_path }}{{ restore_db_and_files_web_directory_for_application }}"
```
File path where DB dump file to restore is located
```
	restore_db_and_files_db_backup_file_path: "{{ restore_db_and_files_path_to_restored_files }}/backups/mysql/current"
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
		restore_db_and_files_mysql_root_password: "your-super-secure-pw"
		restore_db_and_files_db_name: "your-db"
		restore_db_and_files_path_to_restored_files: "/tmp/bareos-restores"
  	    restore_db_and_files_db_backup_file_path: "{{ path_to_restored_files }}/backups/mysql/current"
	  roles:
	    - { role: stancel.restore-db-and-files }

License
-------

GPLv3

Author Information
------------------

[Brad Stancel](https://github.com/stancel)


