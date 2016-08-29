Flynn External PostgreSQL Provider
==================================

This app can be configured as a Flynn provider that will provision databases on a shared external PostgreSQL cluster.

Setup
-----

First setup the target external database by creating a system user for the provider to connect to.

Executing the following will create a user `flynn` with the appropriate permissions

```
CREATE USER flynn WITH SUPERUSER CREATEDB CREATEROLE PASSWORD '<database admin pass>'
```

Clone the repository locally and execute the following:

```
$ flynn create pg-external
$ flynn env set PGHOST="<external database hostname or IP>" PGUSER="flynn" PGPASSWORD="<database admin pass>" SERVICE_NAME="pg-external"
$ flynn route remove $(flynn route | tail -1 | tr -s ' ' | cut -d" " -f3)
$ git push flynn master
$ flynn provider add pg-external http://pg-external-web.discoverd/databases
```

Usage
-----

Once you have completed the setup you can add databases to your applications with:

```
$ flynn resource add pg-external
```

Normal Flynn pg commands will work with this appliance:

```
$ flynn pg psql
```

Caveats
-------

The CLI commands will use the built in PostgreSQL appliance container to launch commands.
As such the external database provider needs to use a compatible version of PostgreSQL.
