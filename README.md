# Docker Build Scripts

This repository will provide various Dockerfiles for building images as used in hosting environments.

### Postgresql 9.1 Server
```bash
    
# start the database
PGSQL=$(docker run -p 5432 -d synthomat/pgsql /usr/bin/start_pgsql.sh YOURPASSWORD)

# verify it is running
docker ps $PGSQL

# grab the IP
PGSQL_IP=$(docker inspect $PGSQL | grep IPAddress | awk '{ print $2 }' | tr -d ',"')

# connect through the client
psql -h $PGSQL_IP -Uroot -d postgres
```


### Connecting through a linked container
```bash

# start the database
docker run --name pgsql -d synthomat/pgsql /usr/bin/start_pgsql.sh YOURPASSWORD

# connect through a new container
docker run --link pgsql:db --rm -t -i postgres /bin/sh -c 'psql -h $DB_PORT_5432_TCP_ADDR -U root postgres'
