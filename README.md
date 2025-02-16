### 🐳 **Docker Image: Nginx PHP-FPM**

Copyright note: This repo is inspired and partially copied from [jkaninda/nginx-php-fpm](https://github.com/jkaninda/nginx-php-fpm) with some differences:
* Support for more PHP libraries
* Different Docker configuration
* Engineered to run mainly Drupal applications

This Docker image provides **Nginx** and **PHP-FPM** is the base for application container Docker image which can run in a stack with other services (MariaDB, Varnish, Redis, Apache Solr etc.). See this project: (eaudeweb/drupal-docker-stack)[https://github.com/eaudeweb/drupal-docker-stack].

Scenario 1 - Build Docker image for application:
1. Create an Dockerfile based on this image
1. Copy PHP code inside the image
1. Run composer inside to install dependencies
1. Compile theme file if needed
1. Install other services in compose.yml
1. Mount upload 'files' as Docker NFS volume and mount via compose.yml (i.e. WordPress 'uploads', Drupal 'files' etc.)

This avoids having any dependency files on the host, just the Docker Compose stack, more like Kubernetes.

Scenario 2 - Mount source code inside container (like in the example below):
1. Use base image
1. Mount static website and othe volumes inside container, i.e. `/var/www/html/web/` etc.

This more simple and easy to setup case adds dependency on the host storage.

---
## **Links**
- [Docker Hub](https://hub.docker.com/r/drupaleaudeweb/nginx-php-fpm)

## **Supported PHP Versions**
- 8.2

## **Specifications**

This Docker image comes pre-installed with the following:

- **PHP Extensions**:
  - apcu
  - imagick
  - imap
  - ldap
  - bcmath
  - bz2
  - exif
  - gettext
  - gd
  - iconv
  - imap
  - intl
  - ldap
  - mbstring
  - memcached
  - mysqli
  - opcache
  - openssl
  - pdo
  - pdo_mysql
  - pdo_pgsql
  - pcntl
  - readline
  - redis
  - sodium
  - sqlite3
  - xml
  - zip

- **Additional Features**:  
  - composer
  - crond
  - nginx
  - supervisord

## **Basic Usage with Docker Compose**

### **Example `compose.yml`**
```yaml
services:
  app:
    image: drupaleaudeweb/nginx-php-fpm:8.2
    container_name: app
    restart: unless-stopped
    volumes:
      # Website root
      - ./web:/var/www/html/web
    ports:
      - "80:80"
```
---

### **Default Web Root**
```
/var/www/html/web
```

---

## **Star the Project**  

If you find this project useful, please give it a ⭐️ on GitHub to show your support! 😊  
