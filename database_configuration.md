# Database Configuration

Configure the following flags to connect ReN3 services to your database.  
**These flags should be set in both the server and ingestor configurations.**

| Flag Name            | Type   | Description                                                                                  |
|----------------------|--------|----------------------------------------------------------------------------------------------|
| `db_hostname`        | string | The hostname or IP address of your database server.                                           |
| `db_user`            | string | The username used to authenticate with the database.                                          |
| `db_password`        | string | The password for the database user.                                                           |
| `db_dbname`          | string | The name of the database to connect to.                                                       |
| `db_port`            | string | The port number on which the database server is listening.                                    |
| `db_source`          | string | The type of database source. Use `skysql` for MariaDB, `mysql` for MySQL, or `azure` for MSSQL.|
| `db_connectionlimit` | int    | The maximum number of concurrent connections allowed to the database.                         |

**Example Configuration:**
```
db_hostname=your-db-host
db_user=your-db-user
db_password=your-db-password
db_dbname=your-db-name
db_port=3306
db_source=skysql
db_connectionlimit=10
```

> Select the appropriate `db_source` option based on the database you are connecting to:
> - Use `skysql` for MariaDB instances on SkySQL.
> - Use `mysql` for standard MySQL databases.
> - Use `azure` for Microsoft SQL Server databases hosted on Azure.

