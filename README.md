# Tutum Stackfile for Magento 2

Please see blog post at <a href="http://mageinferno.com/blog/deploy-magento-2-digital-ocean-tutum">http://mageinferno.com/blog/deploy-magento-2-digital-ocean-tutum</a> for more details about this Stackfile.

```
app:
  image: mageinferno/magento2-nginx:1.9.9-0
  ports:
    - '80:80'
  links:
    - php-fpm
    - db
  volumes_from:
    - appdata

appdata:
  image: tianon/true
  volumes:
    - /src

php-fpm:
  image: mageinferno/magento2-php:7.0.0-fpm-0
  links:
    - db
  volumes_from:
    - appdata

db:
  image: mariadb:10.0
  ports:
    - '3306:3306'
  volumes_from:
    - dbdata
  environment:
    - MYSQL_DATABASE=magento2
    - MYSQL_PASSWORD=magento2
    - MYSQL_ROOT_PASSWORD=magento2
    - MYSQL_USER=magento2

dbdata:
  image: tianon/true
  volumes:
    - /var/lib/mysql

setup:
  image: mageinferno/magento2-setup:2.0.0-archivesd-0
  links:
    - db
  volumes_from:
    - appdata
  environment:
    - M2SETUP_DB_HOST=db
    - M2SETUP_DB_NAME=magento2
    - M2SETUP_DB_USER=magento2
    - M2SETUP_DB_PASSWORD=magento2
    - M2SETUP_BASE_URL=http://PROJECTID-username.node.tutum.io/
    - M2SETUP_ADMIN_FIRSTNAME=Admin
    - M2SETUP_ADMIN_LASTNAME=User
    - M2SETUP_ADMIN_EMAIL=dummy@gmail.com
    - M2SETUP_ADMIN_USER=magento2
    - M2SETUP_ADMIN_PASSWORD=magento2
```
