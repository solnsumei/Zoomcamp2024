FROM python:3.10

RUN apt-get install wget
RUN pip install pyarrow pandas sqlalchemy psycopg2

WORKDIR /app

COPY ingest_data.py ingest_data.py

ENTRYPOINT [ "python", "ingest_data.py" ]