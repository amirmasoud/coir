# Coir

A docker based project for WordPress utilizing MariaDB and NGINX.

## Version

`WordPress`:5.4.1

`PHP`: php7.4-fpm

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

⚠️ Notice: Your database and WordPress will remain intact.

```console
docker-compose down -v
```

## Where is WordPress

After running `docker-compose up -d` your WordPress files to be copied to `wordpress` directory. navigate to that directory. the rest is history.

## Change Versions

You can upgrade our downgrade to your desired version by stoping containers first, editing `.env` file and start your containers again.

For example, if you want to use `php`: 7.2 instead of 7.4 you should:

1. change `WORDPRESS_VERSION` in `.env` to `5.4.1-php7.2-fpm`
2. run `docker-compose up -d` to pull the new WordPress image.

⚠️ Only `fpm` versions are supported in this project.

## Custom Domain

1. Open `deploy/conf.d/coir.conf`
2. Change `server_name` to your domain.

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

*as root*: following command will ask for your root database password (`DATABASE_ROOT` variable in `.env`)

```console
docker-compose exec database mysql --password
```

*as non-root*: It will ask for you non-root database password (`DATABASE_PASSWORD` variable in `.env`). Change `--user` value if you have changed the user in `.env`

```console
docker-compose exec database mysql --user=coir --password
```

**SSH into proxy (NGINX) container:**

```console
docker-compose exec proxy sh
```

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

> If you found this repository useful please consider starring the project.