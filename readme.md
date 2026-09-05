# Internship Opportunity Tracker and Search API

A Python prototype that crawls internship and company career pages, stores
normalized opportunities in PostgreSQL, and exposes paginated search and
authentication APIs. It was built to reduce repetitive manual searches and to
experiment with scheduled Scrapy jobs.

> **Historical project:** this repository is archived and is not maintained.
> Its spiders target third-party page structures from 2021 and are likely to
> require repairs before use.

## What is included

- Scrapy project with multiple site-specific spiders and item pipelines
- PostgreSQL persistence and database-creation helpers
- Daily crawl scheduling
- Flask endpoints for paginated listings and filters
- Separate registration/login API with password hashing
- Proxy and Splash-related crawler configuration

## Setup

The project uses an older Python dependency set. Python 3.8 is the most likely
compatible runtime.

```bash
git clone https://github.com/xzaviourr/internship-opportunity-tracker.git
cd internship-opportunity-tracker
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Configure PostgreSQL in
`InternTracker/Database/connection.config` and review the connection modules
before starting. Never commit real database credentials.

Run every discovered spider from the Scrapy project directory:

```bash
cd InternTracker
python new_main.py
```

To run one spider, first list the names available in this checkout:

```bash
scrapy list
scrapy crawl <spider-name>
```

`scheduler.py` starts the internship and authentication APIs in threads and
schedules a daily crawl. It is a development launcher, not a production process
manager.

## API outline

- `GET /internships/<page>` — 25 opportunities per page
- `POST /internships/<page>` — filter by category and/or minimum stipend
- `GET /internship/about/<id>` — opportunity details
- authentication endpoints are implemented in `Routes/auth.py`

## Operational and data limitations

- Respect every target site's terms, robots policy, rate limits, and applicable
  law before crawling.
- Websites change frequently; stale selectors can silently produce incomplete
  or incorrect listings.
- The API uses development defaults and has not been security hardened.
- Review database queries and validation before exposing it to untrusted users.
- Opportunity data belongs to the originating sites; this repository does not
  grant redistribution rights.

No license is asserted because this was a multi-contributor project.
