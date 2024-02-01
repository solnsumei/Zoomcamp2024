# Sol Zoomcamp2024
Data Engineering Zoomcamp training by DataTalk.club

## Use Docker to run postgres
```
docker run -it -e POSTGRES_USER="<username>" -e POSTGRES_PASSWORD="<password>" -e POSTGRES_DB="ny_taxi" -v $(pwd)/ny_taxi_postgres_data:/var/lib/postgresql/data -p 5432:5432 postgres:15
```