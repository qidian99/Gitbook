# Docker

I’m planning to merge all the modules in develop branch and tag it as milestone 1. After that I will try to set up the docker container with our specific version of Drupal and all the modules we are currently developing in this milestone. By doing this we can minimize the deployment time of Drupal, PHP, Apache, and MySQL on their local machines.

I will make other GitHub repositories that they will be working on, and set up task management directly in GitHub.

## Other

sudo vim /etc/apache2/httpd.conf, commenting out php7

## Drupal installation

`composer create-project drupal-composer/drupal-project:7.x-dev freshd7_dir`  
 

Creates a drupal 7 site using latest version and puts the site in web directory inside of freshd7\_dir leaving drush, scripts, vendor, composer.json, composer.lock outside of document root.

**Composer error:**

constraints like "^2.0@dev" in unrelated projects

**Fix:**

{% embed url="https://github.com/composer/composer/issues/9191" %}

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p>A temporary fix for broken builds is to make the following setting in
          your pipelines:</p>
        <p>composer self-update 1.10.10</p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>



## Set up docker image

{% embed url="https://www.digitalocean.com/community/tutorials/how-to-install-drupal-with-docker-compose" %}

{% embed url="https://drupalize.me/tutorial/dockerize-existing-project?p=3040" %}



## Certbot challenge

{% embed url="https://cloud.tencent.com/developer/article/1365676" %}

## Clean up

{% embed url="https://vsupalov.com/cleaning-up-after-docker/" %}



## Credentials

`drush site:install`

Installation complete. User name: admin User password: B6USGNUxcz \[ok\]

