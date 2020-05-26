<iframe width="560" height="315" src="https://www.youtube.com/embed/BfrVWwJv8ls" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>



```shell
mkdire wp-docker && cd wp-docker
```

```shell
touch docker-compose.yml
```



**configure mysql service:**

```yaml
# docker-compose.yml

version: "3"

services:
  db:
    image: mysql:8
    container_name: mysql
    restart: always
    command: "--default-authentication-plugin=mysql_native_password"
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wpdb
      MYSQL_USER: user
      MYSQL_PASSWORD: password

```

[source-docker-mysql](https://hub.docker.com/_/mysql) -- provides setup instructions including all available environment variables

> **NOTES**
>
> `command` uses a specific authentication method related to the latest version of mysql
>
> `MYSQL_DATABSE`, `MYSQL_USER`, `MYSQL_PASSWORD` all are given dummy values for this local install, should use a more secure value for each in production.



**configure wodpress service:**

```yaml
# docker-compose.yml

  wordpress:
    image: wordpress:latest
    container_name: wordpress
    restart: always
    volumes:
      - ./wp-content:/var/www/html/wp-content
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_NAME: wpdb
      WORDPRESS_DB_PASSWORD: password
    ports:
      - 8080:80
      - 443:443

```

[source-docker-wordpress](https://hub.docker.com/_/wordpress) -- provides setup instructions including all available environment variables

> **NOTES**:
>
> `volumes` maps the local folder defined on the left-side to the container's folder defined on the right side of the `:`
>
> **NOTE** the wp_content directory will get created automatically when we first configure wordpress.
>
> `WORDPRESS_DB_HOST` must match the mysql db service name
>
> `WORDPRESS_DB_NAME` must match the mysql db name
>
> `WORDPRESS_DB_PASSWORD` must match the password used for the db service
>
> `ports` maps the wordpress port and the mysql port required.





**configure phpmyadmin**

```yaml
# docker-compose.yml

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    restart: always
    ports:
      - 3333:80
    environment:
      PMA_HOST: db
      MYSQL_ROOT_PASSWORD: password

```

[source-docker-phpmyadmin](https://hub.docker.com/r/phpmyadmin/phpmyadmin) -- provides setup instructions including all available environment variables

>  **NOTES**:
>
> `PMA_HOST` must match the db service name
>
> `MYSQL_ROOT_PASSWORD` must match the db root password





**Start wp container with services**

```shell
docker-compose up
```

- execute the command from the root of the project

> in order to access the wordpress installation we need to navigate to:
>
> ```shell
> http://192.168.99.100:8080
> ```
>
> - for docker toolbox
>
> -or-
>
> ```
> http://localhost:8080
> ```
>
> - for newer docker installations
>


**login to phpmyadmin**

> in order to access the phpmyadmin console goto:
>
> ```shell
> http://192.168.99.100:3333
> ```
>
> -or-
>
> ```
> http://localhost:3333
> ```
>
> ![image-20200525152246549](https://tva1.sinaimg.cn/large/007S8ZIlgy1gf5awo44ppj30gg0crwfb.jpg)

