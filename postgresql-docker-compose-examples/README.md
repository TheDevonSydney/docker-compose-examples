# Simple Docker Compose for a Single Instance Postgresql

## Running the container with the Postgresql instance

To run the container and the associated with the associated Postgresql instance using `docker compose`, open a shell on your computer go to the `postgresql-docker-compose-examples/postgresql-single` directory and run the following

```txt
docker compose -f dc-postgresql-single.yml up -d
```

If your are on Windows, you can use WSL or if you have the docker engine installed (via Docker Desktop or Rancher Desktop) you can use Windows Powershell.

![First pull of an image postgres:17.4-alpine from Windows Powershell](./pics/docker-compose-result-for-first-time-pull-image-pg-17.4-alpine.png)

![Running container of an image postgres:17.4-alpine on Rancher Desktop on Windows](./pics/rancher-desktop-after-pull-image-pg-17.4-alpine.png)

![First pull of the default latest image version from Windows Powershell](./pics/docker-compose-result-for-first-time-pull-image-pg-latest.png)

![Running container of the default latest image of postgres on Rancher Desktop on Windows](./pics/rancher-desktop-after-pull-image-pg-latest.png)

## Accessing the PostgreSQL instance with the CLI in the container

To access your database instance with the dedicated CLI from a shell in the container itself : 

* open an interactive shell on the running container of our postgres instance : `docker exec -it postgres /bin/sh`
  * The container name defined in `dc-postgresql-single.yml` is `postgres`
* Running the Postgres CLI : `psql -W -Umydb -dmydb`. 
  * With the option `-W`, you will be prompted for the password
  * The password is defined in the `.env` file, under the variable `POSTGRES_PASSWORD`
* To quit `psql` you can type `exit`
* To quit the shell you can also type `exit`

The `.env` file defines the 3 environment variables used in the docker compose file `dc-postgresql-single.yml` : `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`, which respectively corresponds to the name of a Postgres Database, the name of a user of this database and the password of this user.
The Postgres instance will be created with this database and the associated user.

![Accessing the Postgres instance from the PSQL CLI](./pics/accessing_pg_instance_from_cli_in_container.png)

## Accessing the PostgreSQL instance with DBeaver

[DBeaver Community](https://dbeaver.io/) is an open source and free tool that offers a graphical user interface to access various relational databases including Postgresql.

After starting DBeaver :

* Go in the menu `File` and select the `New` option (`File > New`) or use the shortcut `Ctrl+N`
* Select `DBeaver > Database Connection` and click on the `Next` button

![Selecting the creation of a new Database Connection](./pics/access_pg_with_dbeaver_001.png)

* Select `PostgreSQL` and click on the `Next` button

![Selecting PostgreSQL as the database for which you want to create a connection](./pics/access_pg_with_dbeaver_002.png)

* A window with the connection settings to fill should open.
  * You should change the value for `Database` (should be the value associated with `POSTGRES_DB` in the `.env` file), for `Username` (should be the value associated with `POSTGRES_USER` in the `.env` file) and `Password` (should be the value associated with `POSTGRES_PASSWORD` in the `.env` file).
  * The default value for the other fields should be the values you need.

![Database connection settings](./pics/access_pg_with_dbeaver_003.png)

* If it is the first time you try to connect to a PostgreSQL Database, you will need to download the database driver.
  * To this end, you have to select the tab `Driver properties`
  * DBeaver should propose to download a PostgreSQL driver, you just have to click on the `Download` button.
  * When the driver has been downloaded, you will now see the driver properties and you just have to click on the `Finish` button.

![Downloading the driver](./pics/access_pg_with_dbeaver_004.png)

![Driver properties](./pics/access_pg_with_dbeaver_005.png)

* You should now see the connection to your PostgreSQL Database in the `Database Navigator` view. 

![The new connection in the Database Navigator view](./pics/access_pg_with_dbeaver_006.png)

* You can now access your new (empty) database.
  * You can found the created user under `Roles`

![Accessing the new database from the Database Navigator view](./pics/access_pg_with_dbeaver_007.png)

## Accessing the PostgreSQL instance with the pgAdmin Desktop client

When you [download pgAdmin 4](https://www.pgadmin.org/download/), it comes with a desktop GUI written in Electron.
Once pgAdmin is installed on your computer, it is under the subdirectory `runtime` with the name `pgAdmin4` or `pgAdmin4.exe` on Windows.
Executing this file will start the pgAdmin web application and the Electron application.
The whole application require a lot of ressources of your system and DBeaver is probably a more lightweigth solution.
However, pgAdmin is a very complete and powerful solution to work with a PostgreSQL instance.
As such it is a matter of needs and preferences.

To configure pgAdmin :

* Run the desktop application (`pgAdmin4` or `pgAdmin4.exe` on Windows)
* When the application is started, on your first run, you should have something similar to the next screenshot

![First connection to pgAdmin](./pics/pgAdmin_001.png)

* You first need to register a new server by a right click on `Servers` in the `Object explorer`

![Initiating the registration of a new server](./pics/pgAdmin_002.png)

* A popup window with several tabs should appear.
* In the `General` tab you have to fill the `Name` field (this is the name you want to give to your PostgreSQL Instance, it can be whatever you want). You should also toggle on the `Connect now ?` button.

![Configuring the connection to the new server](./pics/pgAdmin_003.png)

* In the `Connection` tab, you have to fill the `Host name/address` field with the value `localhost`, the `Port` field with the value `5432`, and the fields `Maintenance database`, `Username` and `Password` with respectively the values of the environment variables `POSTGRES_DB`, `POSTGRES_USER`, `POSTGRES_PASSWORD`.

![Configuring the connection to the new server](./pics/pgAdmin_004.png)

* When you click on the `Save` button, pgAdmin should connect to the database server and it should appear in the `Object Explorer` panel.

![Accessing the new server](./pics/pgAdmin_005.png)

## Stopping the container

To stop the container, type the following in your shell, from the directory which contains the docker compose file `dc-postgresql-single.yml` :

```txt
docker compose -f dc-postgresql-single.yml down
```

![Stopping the container with docker compose](./pics/stopping-the-container-with-docker-compose.png)

## Ressources

* [Official PostgreSQL Site](https://www.postgresql.org/)
  * [Official Documentation](https://www.postgresql.org/docs/)
* [Postgresql Logo](https://wiki.postgresql.org/wiki/Logo)
* [Docker Official Image on Docker Hub](https://hub.docker.com/_/postgres)
* [pgadmin4 Image on Docker Hub](https://hub.docker.com/r/dpage/pgadmin4/)
* [PostgreSQL Clients](https://wiki.postgresql.org/wiki/PostgreSQL_Clients)
  * [pgAdmin](https://www.pgadmin.org/)
    * [Documentation](https://www.pgadmin.org/docs/pgadmin4/latest/index.html)
  * [DBeaver Community](https://dbeaver.io/)
  * [pgmanage](https://github.com/commandprompt/pgmanage)
* Documentation in French
  * [PostgreSQL documentation in French](https://docs.postgresql.fr/)
  * [Site of the French PostgreSQL community ](https://www.postgresql.fr/)
* Baeldung tutorial if you just want to use Docker and not Docker Compose : [PostgreSQL with Docker Setup](https://www.baeldung.com/ops/postgresql-docker-setup)