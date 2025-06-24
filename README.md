# Dockerized ETL Pipeline: MySQL to PostgreSQL

## 📌 Project Overview

This project involves implementing an ETL (Extract, Transform, Load) pipeline using Docker containers to transfer data from a MySQL database into a PostgreSQL-based Data Warehouse. The source data is a CSV file (`global_yotube_stat.csv`) which will be loaded into MySQL and then transferred into PostgreSQL using a Python ETL script.

---

## 🛠️ Tools & Requirements

1. [Docker Desktop](https://docs.docker.com/desktop/install/windows-install/)
2. Python 3.x

---

## ⚙️ How to Run

### 1. Start Docker

* Ensure Docker Desktop is running.

### 2. Create Containers

* Write a `docker-compose.yml` file to define two services: MySQL (`db-mysql`) and PostgreSQL (`db-psql`).

### 3. Run Containers

```bash
docker-compose up -d    # detached mode (run in background)
docker-compose up       # non-detached (run in foreground)
```

### 4. Access MySQL Container

```bash
docker exec -it <mysql_container_id> bash
```

### 5. Load Data into MySQL

```bash
mysql --local-infile=1 -uroot -pmysql operational < /docker-entrypoint-initdb.d/init.sql
```

> 🛑 If error: `Loading local data is disabled...`, run inside MySQL:

```sql
SET GLOBAL local_infile=1;
```

Then re-run the `LOAD DATA LOCAL INFILE` query.

### 6. Set Up Python Environment

```bash
python3 -m venv env
source env/bin/activate     # on Linux/macOS
env\Scripts\activate        # on Windows
pip install -r requirements.txt
```

### 7. Create PostgreSQL Database

```bash
docker exec -it <postgres_container_id> bash
psql -Upostgres

-- inside psql:
\l                              # list databases
CREATE DATABASE data_warehouse; # create new database
\c data_warehouse               # switch to it
\d                              # list tables
```

### 8. Run ETL Script

```bash
python etl.py
```

The ETL script will read data from MySQL and write it to PostgreSQL (`data_warehouse.youtube_etl`).

---

## 🖼️ Visual Workflow

![image](https://github.com/user-attachments/assets/183c9440-fb78-43bc-a40f-b5a7e167452a)

---

## 📚 References

* [https://towardsdatascience.com/upload-your-pandas-dataframe-to-your-database-10x-faster-eb6dc6609ddf](https://towardsdatascience.com/upload-your-pandas-dataframe-to-your-database-10x-faster-eb6dc6609ddf)
* [https://medium.com/analytics-vidhya/importing-data-from-a-mysql-database-into-pandas-data-frame-a06e392d27d7](https://medium.com/analytics-vidhya/importing-data-from-a-mysql-database-into-pandas-data-frame-a06e392d27d7)
* [https://medium.com/geekculture/run-docker-in-windows-10-11-wsl-without-docker-desktop-a2a7eb90556d](https://medium.com/geekculture/run-docker-in-windows-10-11-wsl-without-docker-desktop-a2a7eb90556d)
* [https://community.sap.com/t5/technology-blogs-by-sap/how-to-find-out-ports-used-by-applications-on-windows/ba-p/13564904](https://community.sap.com/t5/technology-blogs-by-sap/how-to-find-out-ports-used-by-applications-on-windows/ba-p/13564904)
