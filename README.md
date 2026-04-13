# ETL Pipeline - Sao Paulo Weather Data

ETL pipeline that collects weather data from Sao Paulo via OpenWeatherMap API, transforms and stores it in PostgreSQL, orchestrated by Apache Airflow.

## Architecture

<img src='arquitetura_de_dados_draw.png' alt='ETL Pipeline Architecture'>

## Tech Stack

- **Python 3.13+**
- **Apache Airflow 3** - Orchestration
- **PostgreSQL 16** - Database
- **Docker & Docker Compose** - Containerization
- **Pandas** - Data transformation
- **SQLAlchemy** - Database connection
- **UV** - Package manager

## Project Structure

```
.
├── dags/
│   └── weather_dag.py        # Airflow DAG
├── src/
│   ├── extract_data.py       # API extraction
│   ├── transform_data.py     # Data transformation
│   └── load_data.py          # PostgreSQL loading
├── config/
│   └── airflow.cfg
├── data/                     # Temporary data
├── main.py                   # Local execution (without Airflow)
├── docker-compose.yaml
└── pyproject.toml
```

## Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/your-username/tutorial_pipeline_weather.git
cd tutorial_pipeline_weather
```

### 2. Set up environment variables

Create the file `config/.env`:

```env
API_KEY=your_openweathermap_key
user=airflow
password=airflow
database=airflow
```

> Get your API Key at [openweathermap.org](https://openweathermap.org/api)

### 3. Start the containers

```bash
echo "AIRFLOW_UID=$(id -u)" > .env
docker-compose up -d
```

### 4. Access Airflow

Open **http://localhost:8080** (user: `airflow`, password: `airflow`) and enable the `weather_pipeline` DAG.

The DAG runs every hour, collecting updated weather data.

### Local execution (without Airflow)

```bash
uv sync
uv run main.py
```

## Pipeline Steps

| Step | Description |
|------|-------------|
| **Extract** | GET request to OpenWeatherMap API, saves raw JSON |
| **Transform** | Flattens nested JSON, renames columns, converts timestamps to Sao Paulo timezone |
| **Load** | Inserts data into PostgreSQL via SQLAlchemy |
