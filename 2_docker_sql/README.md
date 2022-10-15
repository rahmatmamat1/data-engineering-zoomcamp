## Docker and SQL
### Objective:
* Simple data ingestion from CSV to the PostgresSQL
* Containerizing data ingestion process

## NY Trips Dataset
Dataset:
* https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page
* https://www1.nyc.gov/assets/tlc/downloads/pdf/data_dictionary_trip_records_yellow.pdf

According to the TLC data website, from 05/13/2022, the data will be in .parquet format instead of .csv The website has provided a useful link with sample steps to read .parquet file and convert it to Pandas data frame.

You can use the csv backup located here, https://github.com/DataTalksClub/nyc-tlc-data/releases/download/yellow/yellow_tripdata_2021-01.csv.gz

## Command
### Docker-Compose
`docker-compose.yaml` contain 2 Images, PostgresSQL and pgadmin.

Run it:

```
docker-compose up
```

Run in detached mode:

```
docker-compose up -d
```

### Dockerize ingestion script
Dockerize ingest_data.py with Dockerfile

Build image:

```
docker build -t taxi_ingest:v001 .
```

Specify download data URL:

```
URL="http://192.168.1.3:8000/output.csv.gz"
```

Run Docker data ingestion:

```
docker run -it \
  --network=pg-network \
  taxi_ingest:v001 \
    --user=root \
    --password=root \
    --host=pg-database \
    --port=5432 \
    --db=ny_taxi \
    --table_name=yellow_taxi_trips \
    --url=${URL}
```

Shutting it down:

```
docker-compose down
```