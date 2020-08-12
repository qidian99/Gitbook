# Prestashop

## PHP zip

```text
brew install libzip
```

{% embed url="https://stackoverflow.com/questions/58290566/install-ext-zip-for-mac/58300437\#58300437" %}

  
Previous php location:

/usr/local/bin/php

```text
 brew link --overwrite php@7.3
 echo 'export PATH="/usr/local/opt/php@7.3/bin:$PATH"' >> /Users/qidian/.bash_profile
 echo 'export PATH="/usr/local/opt/php@7.3/sbin:$PATH"' >> /Users/qidian/.bash_profile
 sudo apachectl restart
```

## Create DB



```text
$ mysql -u adminusername -p
```

Create the database and give it a name like “prestashop”:

```text
> CREATE DATABASE prestashop COLLATE utf8mb4_general_ci;
```

Grant privileges to that database to a new user \(the one that PrestaShop will use to connect to the database\). Let’s call it “prestashopuser”.

```text
> CREATE USER "prestashopuser"@"hostname" IDENTIFIED BY "somepassword";
> GRANT ALL PRIVILEGES ON prestashop.* TO "prestashopuser"@"hostname";
```

### PHP extensions

{% embed url="https://affinitybridge.com/blog/adding-php-extensions-system-php-under-os-x-1015-catalina" %}

* **CURL**. The [Client URL extension](https://php.net/manual/en/book.curl.php) is used to download remote resources like modules and localization packages.
* **DOM**. The [DOM extension](https://php.net/manual/en/book.dom.php) is needed to parse XML documents. PrestaShop uses it for various functionalities, like the Store Locator. It is also used by some modules, as well as the pear\_xml\_parse library.
* **Fileinfo**. The [File information extension](https://php.net/manual/en/book.fileinfo.php) is used to find out the file type of uploaded files.
* **GD**. The [GD extension](https://php.net/manual/en/book.image.php) is used to create thumbnails for the images that you upload.
* **Intl**. The [Internationalization extension](https://php.net/manual/en/book.intl.php) is used to display localized data, such as amounts in different currencies.
* **Mbstring**. The [Multibyte string extension](https://www.php.net/manual/en/book.mbstring.php) to perform string operations everywhere.
* **Zip**. The [Zip extension](https://php.net/manual/en/book.zip.php) is used to expand compressed files such as modules and localization packages.

## Enable Apache Mode\_Write \(2.4\)

{% embed url="https://stackoverflow.com/questions/869092/how-to-enable-mod-rewrite-for-apache-2-2" %}

Remove the **\#** in `#LoadModule rewrite_module libexec/apache2/mod_rewrite.so`

## DB

prestashopuser:password

