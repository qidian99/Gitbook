---
description: 中文莫名乱码，所以笔记全英文
---

# 创建一个解耦的Drupal 9&React应用

## Overview

{% embed url="https://medium.com/@purushotamrai/getting-started-with-reactjs-drupal-fully-decoupled-feae28a41a5d" %}

![Coupled vs. Decoupled](../.gitbook/assets/image%20%281%29.png)

We can see that in the decoupled way Drupal is exposing internal resources through its web services API. It makes me question:

1. Can we still utilize the the Field, Form API?
2. Does it change any global variable defined by Drupal \(e.g., $user\)?
3. How to dynamically/conditionally render elements in React?
4. How does access control change?
5. What's the advantages besides the boosted efficiency in front-end implementation leveraging React?

### React

* JS library for UI
* Huge ecosystem like different modules and libraries
* Nothing special

### Drupal

* Open-source CMS
* Implementing editorial workflows
* Suite of tool for modeling \(Entity API\)
* Support JSON API, and
* GraphQL \(卧槽）

## Outline

* **Configure Drupal to serve data**
* **Create React App**
* **Define React Components**
* **Fetch Data and Display**

## Step 1: Configure Drupal to serve data <a id="81ae"></a>

### **1. Setup Drupal using Composer**

> Composer, it’s a PHP dependency manager which can be used to download Drupal, Drupal contributed projects \(modules, themes, etc.\), and all of their respective dependencies.

#### The tutorial used Drupal 8.x, but I will change it to Drupal 9.

```bash
composer create-project drupal/recommended-project travel_destinations "^9.0" --stability=dev --prefer-dist
```

‘travel\_destinations’ being the directory where Drupal project will be installed.

This command downloads latest Drupal \(version 9\) in the web directory and other dependencies in the vendor directory.

 Now we need to create a virtual host \(say [http://td-drupal.local\)](http://td-drupal.local%29/) pointing to web directory so that we can access it on the web browser.

### **2. Install Drupal**

Before installing Drupal we need to create a database which Drupal will be using and corresponding user with all privileges on that database. There are multiple ways to install Drupal

1. via interactive UI provided by default by Drupal \(which can be accessed via visiting [http://td-drupal.local](http://td-drupal.local/) in the browser\) 
2. or using drush [https://www.drupal.org/docs/8/install](https://www.drupal.org/docs/8/install) [https://drushcommands.com/drush-9x/site/site:install/](https://drushcommands.com/drush-9x/site/site:install/)
3. use [Drupal Console](https://drupalconsole.com/).

**Problem encountered:**

{% embed url="https://www.drupal.org/forum/support/module-development-and-code-questions/2017-10-05/command-config-set-needs-a-higher" %}

**Where is drush install \(is there sth. like a node\_modules\)?**

{% embed url="https://drupalize.me/tutorial/install-drush-using-composer?p=1156" %}

**Composer runs out of memory:**

Fatal error: Allowed memory size of 1610612736 bytes exhausted \(tried to allocate 4096 bytes\) in phar:

Solution: increase PHP mem size: 

{% embed url="https://stackoverflow.com/questions/49212475/composer-require-runs-out-of-memory-php-fatal-error-allowed-memory-size-of-161" %}

1. sudo cp /etc/php.ini.default /etc/php.ini
2. sudo su
3. vi /etc/php.ini and change memory to -1 \(unlimited\)
4. apachectl restart
5. `php -r "echo ini_get('memory_limit').PHP_EOL;` should give you -1

Then we can finally run

```text
composer require --dev drush/drush
```

Note that the drush is installed in the local directory. If want a global drush manager, then try

{% embed url="https://github.com/drush-ops/drush-launcher" %}

#### Install MySQL

{% embed url="https://dev.mysql.com/doc/mysql-osx-excerpt/5.7/en/osx-installation-pkg.html" %}

The configuration of database when we call `./vendor/bin/drush si`

![](../.gitbook/assets/image%20%289%29.png)

Add the export in `~/.bash_profile` and restart bash \(note: NOT .bashrc\):

![](../.gitbook/assets/image%20%286%29.png)

![](../.gitbook/assets/image%20%2810%29.png)

I set the username and password in the file ~/.mysql.config and add the alias in .bash\_profile

```bash
alias mysql="/usr/local/mysql/bin/mysql --defaults-extra-file=~/.mysql.config"
```

Test MySQL connection:

![](../.gitbook/assets/image%20%284%29.png)

**Proceed with `drush si`**

Problem 2: 

SQLSTATE\[HY000\] \[2054\] The server requested authentication method unknown to the client \#1392



Need to grant permission

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-create-a-new-user-and-grant-permissions-in-mysql" %}

![](../.gitbook/assets/image.png)

Flush the privileges to make sure the user has active privileges by adding the following the mysql terminal:  
`FLUSH PRIVILEGES;`

**REMOVE MYSQL INSTALLED FROM WEB**

{% embed url="https://community.jaspersoft.com/wiki/uninstall-mysql-mac-os-x" %}

clean up the directories:

{% embed url="https://gist.github.com/vitorbritto/0555879fe4414d18569d" %}

**INSTALL MYSQL FROM HOMEBREW**

`brew install mysql`

Get the installed location: `$(brew --prefix mysql)` \($ is needed, not the bash prompt\)

Change ~/.bash\_profile

```text
$(brew --prefix mysql)
```

Unset the password \(default one in unknown\)

```text
sudo $(brew --prefix mysql)/bin/mysqladmin -u root password root
```

Install Drupal 9: `./vendor/bin/drush si`

```bash
 [notice] Starting Drupal installation. This takes a while.
 [notice] Performed install task: install_select_language
 [notice] Performed install task: install_select_profile
 [notice] Performed install task: install_load_profile
 [notice] Performed install task: install_verify_requirements
 [notice] Performed install task: install_settings_form
 [notice] Performed install task: install_verify_database_ready
 [notice] Performed install task: install_base_system
 [notice] Performed install task: install_bootstrap_full
 [notice] Performed install task: install_profile_modules
 [notice] Performed install task: install_profile_themes
 [notice] Performed install task: install_install_profile
 [notice] Performed install task: install_configure_form
 [notice] Cron run completed.
 [notice] Performed install task: install_finished
 [success] Installation complete.  User name: admin  User password: ZVQsSGhsP7
```

### **3. Download and Install Required Modules**

we need to add [JSON API module](https://www.drupal.org/project/jsonapi) and for this, we can download the module using composer and then enable it. Additionally, let’s download the [Admin Toolbar](http://drupal.org/project/admin_toolbar) module as well for faster navigation.

```text
composer require drupal/jsonapi
composer require drupal/admin_toolbar
```

This will download and place the JSON API module in travel\_destinations/web/modules/contrib directory. Enable the module either via logging into the website using the credentials used while installation in the previous step and navigating to [http://td-drupal.local/admin/module](http://td-drupal.local/admin/module) page or use drush packaged in your project.

```text
vendor/bin/drush en -y admin_toolbar admin_toolbar_tools jsonapi
```

**Question**: Where is the URL for the installed Drupal 9 site?

```text
drush runserver
```

![](../.gitbook/assets/image%20%287%29.png)

Error is thrown

```text
Parse error: syntax error, unexpected '?', expecting variable (T_VARIABLE) in /Users/qidian/git/travel_destinations/vendor/laminas/laminas-diactoros/src/functions/marshal_uri_from_sapi.php on line 84
```

{% embed url="https://www.drupal.org/docs/system-requirements/php-requirements" %}

{% embed url="https://stackoverflow.com/questions/56510272/how-to-fix-php-parse-error-syntax-error-unexpected-on-laravel-5-8" %}

![](../.gitbook/assets/image%20%282%29.png)

![](../.gitbook/assets/image%20%285%29.png)

Need to update the PHP

