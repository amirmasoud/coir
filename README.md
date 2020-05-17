# Coir

A docker based project for WordPress utilizing MariaDB and NGINX.

## Version

`WordPress`:5.4.1

`PHP`: php7.4-fpm-alpine

`MariaDB`: 10.5.2

`NGINX`: 1.17.7-alpine

## How to use

clone the repository:

```console
git@github.com:amirmasoud/coir.git
```

Fire `docker-compose.yml` file:

```console
docker-compose up -d
```

Open [localhost](http://localhost) on default port, 80.

## Stop containers

```console
docker-compose stop
```

## Remove containers

⚠️ Your database and WordPress will remain intact.

```console
docker-compose down -v
```

## Modify .env

⚠️ After finish modifying `.env` file you have to restart containers for changes to take effect.

## Where is WordPress

After running `docker-compose up -d` your WordPress files to be copied to `wordpress` directory. navigate to that directory. the rest is history.

## Disable WordPress Debug Mode

1. Set `WORDPRESS_DEBUG` to an empty values
2. Restart containers

## Change Versions

You can upgrade our downgrade to your desired version by stoping containers first, editing `.env` file and start your containers again.

For example, if you want to use `php`: 7.2 instead of 7.4 you should:

1. change `WORDPRESS_VERSION` in `.env` to `5.4.1-php7.2-fpm`
2. run `docker-compose up -d` to pull the new WordPress image.

⚠️ Only `fpm` versions are supported in this project.

## Custom Domain

1. Open `deploy/conf.d/coir.conf`
2. Change `server_name` to your domain.
3. Restart your containers for changes to take effect.

⚠️ for `.test` domain you should also append `127.0.0.1 yourcustomdomain.test` to `/etc/hosts` file on Mac or `c:\Windows\System32\Drivers\etc\hosts` file on Windows. (You may need root access on Mac or administrator privileges on Windows to perform this step)

## Database Credintials

in `.env`, following credentials are corresponding to database credentials:

`DATABASE_USER`: database username that WordPress is using.

`DATABASE_PASSWORD`: database username that WordPress is using.

`DATABASE_NAME`: name of the database that WordPress is using.

`DATABASE_ROOT`: database root password which WordPress is NOT using it.

## SSH into containers

**SSH into WordPress container:**

```console
docker-compose exec wordpress bash
```

**SSH into database (MariaDB) container:**

_as root_: following command will ask for your root database password (`DATABASE_ROOT` variable in `.env`)

```console
docker-compose exec database mysql --password
```

_as non-root_: It will ask for you non-root database password (`DATABASE_PASSWORD` variable in `.env`). Change `--user` value if you have changed the user in `.env`

```console
docker-compose exec database mysql --user=coir --password
```

**SSH into proxy (NGINX) container:**

```console
docker-compose exec proxy sh
```

## uploads.ini

Change `deploy/wordpress/uploads.ini` parameters value to modify upload limits. (Default `upload_max_filesize` is 64M)

Also change `client_max_body_size` (in server and location) in `deploy/proxy/conf.d/coir.conf`.

## Cannot Download Thmese/Plugins

Try followig solutions:

1. Run `docker-compose exec wordpress chown -R www-data:www-data /var/www/html/wp-content` to fix directory permissions.
2. add `define( 'FS_METHOD', 'direct' );` to `wp-config.php`

## NGINX Logs

Navigate to `deploy/proxy/logs` and you can check `access.log` and `error.log` files.

## File Structure

```console
  |.env                 # Coir config
  |docker-compose.yml   # docker compose
  |-wordpress           # WordPress files
  |-deploy
  |  |-database         # MariaDB
  |  |-proxy            # NGINX
  |  |  |-logs          # error.log & access.log
  |  |  |-conf.d        # .conf
```

## Support

If you found any issue or have any ideas to enhance the project please do not hesitate to open an issue.

If you found this repository useful please consider starring the project.
