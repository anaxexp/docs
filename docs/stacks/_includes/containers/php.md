* PHP can be configured with the following [environment variables](https://github.com/anaxexp/drupal-php#environment-variables)
* Available [php extensions](https://github.com/anaxexp/php#php-extensions)
* Composer pre-installed with a default global package `hirak/prestissimo:^0.3` to download dependencies in parallel 

### Files directory permissions

Public files directory (symlink to `/mnt/files/public`) that used for uploads owned by `www-data` user (PHP-FPM user) by default and the default container user (`anaxexp`) has no writing permissions. So if you run a command that creates files in a public directory you will get insufficient permissions error. You can fix this problem by giving writing permissions for files directory to the owner's group (user `anaxexp` is a member of `www-data` group) by using one of the [helper scripts](https://github.com/anaxexp/php#helper-scripts):

```bash
sudo files_chmod /mnt/files/public
```

For mode details about users and permissions in PHP container see https://github.com/anaxexp/php#users-and-permissions

### Environment variables

!!! info "Variables availability" 
    Environment variables provided by AnaxExp are always available in PHP even if `PHP_FPM_CLEAR_ENV` set to `no`. 

In addition to [global environment variables](/infrastructure/env-vars.md), we provide the following variables in PHP container that you can use in your [post-deployment scripts](/apps/post-deployment-scripts.md) or settings files:

| Variable              | Description                                   |
| --------------------- | --------------------------------------------  |
| `$APP_ROOT`           | `/var/www/html` by default                    |
| `$HTTP_ROOT`          | e.g. `/var/www/html/web`                      |
| `$CONF_DIR`           | `/var/www/conf` by default                    |
| `$ANAXEXP_APP_NAME`     | My app                                        |
| `$ANAXEXP_HOST_PRIMARY` | example.com                                   |
| `$ANAXEXP_URL_PRIMARY`  | http://example.com                            |
| `$ANAXEXP_HOSTS`        | `[ "example.com", "dev.example.org.wod.by" ]` |

Deprecated variables:

| Variable             | Instead use    |
| -------------------- | -------------- |
| `$ANAXEXP_APP_ROOT`    | `$APP_ROOT`    |
| `$ANAXEXP_APP_DOCROOT` | `$HTTP_ROOT`   |
| `$ANAXEXP_CONF`        | `$CONF_DIR`    |
| `$ANAXEXP_DIR_CONF`    | `$CONF_DIR`    |
