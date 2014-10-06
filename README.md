# Getting Started
WordPress, with a git workflow. Deploys are currently done by manual git pulls from the production server.

## Pre-requirements
* Git
* MySql
* PHP
* Apache (might move to nginx)

## WordPress installation
*Most of the instructions are borrowed from the [WordPress installation page](http://codex.wordpress.org/Installing_WordPress), as well as [this site](http://blog.g-design.net/post/60019471157/managing-and-deploying-wordpress-with-git) and [this site](http://efeqdev.com/website-development/this-is-how-we-version-control-and-deploy-our-wordpress-websites-with-git/).*

* Clone this repository. Make sure to use the recursive flag because the WordPress git repository was added as a submodule: 
```
sudo git clone --recursive https://github.com/Addison-Huff/music-blog.git
```
* Create a MySQL database for our local WordPress instance. Remember to save this information for later, when setting up wp-config.php (change `databasename`, `wordpressusername` and `password`):

```
$ mysql -u"root" -p

Enter password:

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 5340 to server version: 3.23.54
 
Type 'help;' or '\h' for help. Type '\c' to clear the buffer.
 
mysql> CREATE DATABASE databasename;
Query OK, 1 row affected (0.00 sec)
 
mysql> GRANT ALL PRIVILEGES ON databasename.* TO "wordpressusername"@"localhost"
    -> IDENTIFIED BY "password";
Query OK, 0 rows affected (0.00 sec)

mysql> EXIT
Bye
$ 
```
* At this point we need to create the wp-config.php file before moving on. From the main directory, copy the sample config to a new file: `cp wordpress/wp-config-sample.php wordpress/wp-config.php`
* Edit this new file and set the values of:
	* `DB_NAME`
		* The name of the database you created for WordPress in Step 2 .
	* `DB_USER` 
		* The username you created for WordPress in Step 2.
	* `DB_PASSWORD` 
		* The password you chose for the WordPress username in Step 2.
	* `DB_HOST` 
		* The hostname you determined in Step 2 (usually localhost, but not always; see some possible `DB_HOST` values). If a port, socket, or pipe is necessary, append a colon (:) and then the relevant information to the hostname.
* Fill out the [secret keys](http://codex.wordpress.org/Editing_wp-config.php#Security_Keys) section.
* After the section where WP_DEBUG is defined, insert these several lines to account for the fact that the wordpress folder is in a subdirectory: 
```
define('WP_SITEURL', 'http://' . $_SERVER['SERVER_NAME'] . ':' . $_SERVER['SERVER_PORT'] . '/wordpress');
define('WP_HOME',    'http://' . $_SERVER['SERVER_NAME'] . ':' . $_SERVER['SERVER_PORT']);
define('WP_CONTENT_DIR', $_SERVER['DOCUMENT_ROOT'] . '/wp-content');
define('WP_CONTENT_URL', 'http://' . $_SERVER['SERVER_NAME'] . ':' . $_SERVER['SERVER_PORT'] . '/wp-content');
```
