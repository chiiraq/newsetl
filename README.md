# newsetl

A small ETL pipeline that pulls news articles from NewsAPI.org, cleans them up, and loads them into a database hosted on Aiven.

I built it as a hands-on project to practice extracting data from a REST API, transforming it in Python, and loading it into a managed database.

## How it works

The pipeline runs in three steps. First it requests articles from the NewsAPI endpoint. Then `transform.py` cleans the raw JSON: it drops incomplete records, normalizes the timestamps and keeps only the fields I care about. Finally the cleaned articles are written to the database on Aiven.

## Setup

You'll need Python 3, a free API key from https://newsapi.org, and an Aiven database to load into.

Clone the repo and install the dependencies:

```
git clone https://github.com/chiiraq/newsetl.git
cd newsetl
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Create a `.env` file in the project folder with your credentials:

```
NEWSAPI_KEY=your_key_here
DB_HOST=your_host
DB_PORT=your_port
DB_NAME=your_database
DB_USER=your_user
DB_PASSWORD=your_password
```

The `.env` file is listed in `.gitignore` and should never be committed.

## Running it

```
python main.py
```

This fetches the latest articles, transforms them and loads them into the database.

## Notes

The free NewsAPI plan has a daily request limit and delays articles by about 24 hours, so you won't get breaking news from it. The article content field is also truncated by the API, so you get the opening of each article, not the full text.

## Things I'd like to add

Scheduling so it runs automatically, better error handling when the API is down, and tests for the transform step.
