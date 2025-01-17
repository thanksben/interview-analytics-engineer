# dbt-dummy
This is a dummy dbt (data build tool) project you can use to populate dbt seeds, models, snapshots and tests for testing purposes or experimentation.

The `docker-compose.yml` file consists of two services:
- `postgres`
- `dbt`

that are used to build the data models defined in the example project into a target Postgres database.

## `postgres` service and the Sakila Database
This is an instance of a Postgres database initialised with Sakila database (and thus we are using the 
`frantiseks/postgres-sakila` image which is available on Docker Hub). 

The database models a DVD rental store and contains several normalised tables that correspond to films, payments, 
customers and other entities.

Sakila Database was developed by Mike Hillyer, who used to be a member of the AB documentation team at MySQL. For more 
information regarding Sakila Database you can refer to the 
[official MySQL documentation](https://dev.mysql.com/doc/sakila/en/sakila-introduction.html). 
![Sakila Database Schema](https://www.jooq.org/img/sakila.png)

## `dbt` service
This service is built out of the `Dockerfile` and is responsible for creating dbt seeds, models and snapshots
snapshots.


## Running the dummy dbt project
First, let's build the services defined in our `docker-compose.yml` file:

```bash
docker-compose build
```

and now let's run the services so that the dbt models are created in our target Postgres database: 

```commandline
docker-compose up
```

This will spin up two containers namely `dbt` (out of the `dbt-dummy` image) and `postgres` (out of the
`frantiseks/postgres-sakila` image).

Notes:
- For development purposes, both containers will remain up and running
- If you would like to end the `dbt` container, feel free to remove the `&& sleep infinity` in `CMD` command of the 
`Dockerfile`


### Building additional or modified data models
Once the containers are up and running, you can still make any modifications in the existing dbt project 
and re-run any command to serve the purpose of the modifications. 

In order to build your data models, you first need to access the container.

To do so, we infer the container id for `dbt` running container:
```commandline
docker ps
```

Then enter the running container:
```commandline
docker exec -it <container-id> /bin/bash
```

### Querying seeds, models and snapshots on Postgres
In order to query and verify the seeds, models and snapshots created in the dummy dbt project, simply follow the 
steps below. 

Find the container id of the postgres service (`postgres`):
```commandline
docker ps 
```

Then run 
```commandline
docker exec -t <container-id> /bin/bash
```

We will then use `psql`, a terminal-based interface for PostgreSQL that allows us to query the database:
```commandline
psql -U postgres
```
