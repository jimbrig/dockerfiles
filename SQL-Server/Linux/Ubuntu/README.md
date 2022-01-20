# SQL Server on Linux - Ubuntu

- This Ubuntu-based image is built and maintened by Microsoft, and published on [Docker Hub](https://hub.docker.com/r/microsoft/mssql-server-linux/).
- Full documentation can be found at the [SQL Server on Linux Docker image page](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-docker).
- Learn more about the latest release of SQL Server on Linux here: [SQL Server on Linux Documentation](https://docs.microsoft.com/en-us/sql/linux/).

## Usage

- Run:

```bash
docker run -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=yourStrong(!)Password' -p 1433:1433 -d microsoft/mssql-server-linux
```

- Connect:

```bash
docker exec -it <container_id|container_name> /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <your_password>
```

You can also use the tools in an entrypoint.sh script to do things like create databases or logins, attach databases, import data, or other setup tasks.  See this example of [using an entrypoint.sh script to create a database and schema and bcp in some data](https://github.com/twright-msft/mssql-node-docker-demo-app).

You can connect to the SQL Server instance in the container from outside the container by using various command line and GUI tools on the host or remote computers.  See the [Connect and Query](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-connect-and-query-sqlcmd) topic in the SQL Server on Linux documentation.

## Environment variables

- ACCEPT_EULA confirms acceptance of the [End-User Licensing Agreement](http://go.microsoft.com/fwlink/?LinkId=746388).
- SA_PASSWORD is the database system administrator (userid = 'sa') password used to connect to SQL Server once the container is running.

Additional, optional environment variables are documented in the [product documentation](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-configure-environment-variables).


# Further reading
---
+ [SQL Server on Linux for Docker documentation](https://docs.microsoft.com/en-us/sql/linux/sql-server-linux-setup-docker)
+ [SQL Server - Developer Getting Started Tutorials](https://www.microsoft.com/en-us/sql-server/developer-get-started/?utm_source=DockerHub)

# Additional Microsoft SQL Server Docker images
+ Latest *Pre-Release* Version of SQL Server *Evaluation* Edition for Windows Containers: [microsoft/mssql-server-windows](https://hub.docker.com/r/microsoft/mssql-server-windows/)
+ Latest *Released* Version of SQL Server *Developer* Edition for Windows Containers: [microsoft/mssql-server-windows-developer](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/)
+ Latest *Released Version* of SQL Server *Express* Edition for Windows Containers: [microsoft/mssql-server-windows-express](https://hub.docker.com/r/microsoft/mssql-server-windows-express/)

**Note:** Developer Edition is a full-featured version of SQL Server without resource limits, but can only be used in dev/test.  Express Edition is a resource-limited edition of SQL Server that can be used in production.
Get more information on [SQL Server Editions](https://www.microsoft.com/en-us/sql-server/sql-server-editions).