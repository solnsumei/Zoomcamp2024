## To run docker postgres container

```docker run -it -e POSTGRES_USER="root" -e POSTGRES_PASSWORD="root" -e POSTGRES_DB="ny_taxi" -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data -p 5432:5432 postgres:15```

## Running pgcli to log into database

`pgcli -h localhost -p 5432 -u root`

## Adding pgAdmin for postgres
- pulling the docker image use -- `docker pull dpage/pgadmin4`
- running the container 

```
docker run -it \
    -e 'PGADMIN_DEFAULT_EMAIL=admin@admin.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=root' \
    -p 8080:80 \
    dpage/pgadmin4
```

## Network

`docker network create pg-network`

- Running the docker postgres container using the newly created network
```
docker run -it \
    -e POSTGRES_USER="root" \
    -e POSTGRES_PASSWORD="root" \
    -e POSTGRES_DB="ny_taxi" \
    -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data \
    -p 5432:5432 \
    --network=pg-network \
    --name pg-database \
    postgres:15
```

```
docker run -it \
    -e 'PGADMIN_DEFAULT_EMAIL=admin@admin.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=root' \
    -p 8080:80 \
    --network=pg-network \
    --name pgadmin \
    dpage/pgadmin4
```

## To test the ingestion script

```
python ingest_data.py \
    --user=root \
    --password=root \
    --host=localhost \
    --port=5432\
    --db=ny_taxi\
    --table_name=yellow_taxi_trips \
    --url=https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz
```

## Build docker image
`docker build -t taxi_ingest:v001 .`

- To run the new image container with our parameters
```
docker run -it \
    --network=pg-network \
    taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432\
    --db=ny_taxi\
    --table_name=yellow_taxi_trips \
    --url=https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz

```

## Using Docker Compose
- Run `docker-compose up` to start the services defined in the docker-compose.yaml file
- Run `docker-compose down` to shut running services down
- Run `docker-compose up -d` to run in detached mode.