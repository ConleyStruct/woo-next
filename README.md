## 🎨 Next.js Headless WordPress

- Headless WordPress, using Decoupled Architecture in Next.js
- Backend in WordPress.
- Front end in React

## [Backend](https://github.com/imranhsayed/nextjs-headless-wordpress/tree/master/backend)
Run this from root
```bash
docker-compose -f backend/docker-compose.yml up -d 
```
- Make sure to set Home Page as Front page from WordPress dashboard > Customize > Homepage Settings.
*WordPress Backend* will be available on [http://localhost:8020](http://localhost:8020)

*[phpMyAdmin](https://github.com/phpmyadmin/phpmyadmin)*: You can access php myadmin on [http://localhost:8183](http://localhost:8183)
```shell script
port: mysql:3306
username: root
password: root
``` 

phpmyadmin docker image already comes with the username `root` and we have set the mysql password in the dockerfile

* If you happen to use your own WordPress setup, be sure to install and activate plugins from composer.json and add your own WordPress site URL
in an .env file, You can check the .env-example file for reference.

## [Frontend](https://github.com/imranhsayed/nextjs-headless-wordpress/tree/master/frontend)
Run this from root for the first time.
```bash
cd frontend; npm i && npm run dev
```

### During development
```bash
cd frontend; npm run dev
```

Frontend will be available on port [http://localhost:3000](http://localhost:3000)

### Evironment vars. 
Create a `.env` file taking reference from `.env-example` inside frontend directory and add your WordPress Site URL ( for local development put `http://localhost:8020` as your WordPress URL)

## Development ( Developers only )

1. When we change the composer.json, run from root
```shell script
docker-compose -f backend/docker-compose.yml down && \
docker-compose -f backend/docker-compose.yml up -d 
```

First line command will stops and removes all the docker containers and second line command will restart all containers.
Notice that `-d` is to run in detach mode and you can always remove that flag, and run the command so you can see the live logs.
Or you can check the logs for 

2. Check the logs
While the above command is running in detached mode ( -d ), you can run this command in a new terminal tab to see the live logs.
```shell script
docker logs -f container-name
```

3. Login to SSH and wp cli.
```
docker exec -it image-name bash // e.g. docker exec -it backend_wordpress_1 bash
wp
```

e.g.
```bash
docker container ls
```

#### result
```shell script
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                  NAMES
d0b4a3b0074f        wordpress:latest    "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:8000->80/tcp   backend_wordpress_1
aad078ebe131        mysql:5.7           "docker-entrypoint.s…"   About a minute ago   Up About a minute   3306/tcp, 33060/tcp    backend_db_1
```
Here container-name is `backend_db_1` or `backend_wordpress_1`

3. If you make changes to docker-compose.yml file, run the following:

If you happend to change the port in `docker-compose.yml` make sure to delete the `db` directory and then run below.

```shell script
docker-compose -f backend/docker-compose.yml down && \
docker-compose -f backend/docker-compose.yml up -d
```

## Debugging

1. If you get 404 on frontend for graphql request, check to see that the .htaccess file in wordpress directory has the rules

```shell script
# BEGIN WordPress
# The directives (lines) between "BEGIN WordPress" and "END WordPress" are
# dynamically generated, and should only be modified via WordPress filters.
# Any changes to the directives between these markers will be overwritten.
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>

# END WordPress
```

## References for Docker Images.

1. [phpMyAdmin](https://github.com/fuadajip/dockercompose-mysql-phpmyadmin/blob/master/docker-compose.yml)
