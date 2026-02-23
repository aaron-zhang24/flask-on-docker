![CI](https://github.com/aaron-zhang24/flask-on-docker/actions/workflows/ci.yml/badge.svg)

## Overview

This repository is a containerized Flask web application with PostgreSQL, Gunicorn, and Nginx using Docker. The app supports static files and image uploads hosted on production with Gunicorn and Nginx. The repository contains the yml files for both production and development configurations.

## Demo 
![Demo](media/demo.gif) 

## Build Instructions

### Prerequisites
- Docker
- Docker Compose

### Development
1. Clone this repo via:
```
git clone https://github.com/aaron-zhang24/flask-on-docker.git
cd flask-on-docker
```

2. Create a `.env.dev` file in project root
```
   FLASK_APP=project/__init__.py
   FLASK_DEBUG=1
   DATABASE_URL=postgresql://hello_flask:hello_flask@db:5432/hello_flask_dev
   SQL_HOST=db
   SQL_PORT=5432
   DATABASE=postgres
   APP_FOLDER=/usr/src/app
```

3. Build images and start the container
```
docker compose up -d --build
```

4. Initialize the database
```
docker compose exec web python manage.py create_db
```

5. Default port is 30000, but app will be available on whatever port you set in `docker-compose.yml`

6. Close the container via
```
docker compose down -v
```

### Production
1. Create `.env.prod` and `.env.prod.db` files with your production credentials.

2. Build and start:
```
docker compose -f docker-compose.prod.yml up -d --build
```

3. Initialize the database:
```
docker compose -f docker-compose.prod.yml exec web python manage.py create_db
```

4. The app will be available at `http://localhost:1444` or whatever port you set in `docker-compose.yml`

5. Close the container via
```
docker compose -f docker-compose.prod.yml down -v
```

### Endpoints
| Endpoint | Method | Description |
|----------|--------|-------------|
| `/` | GET | Returns `{"hello": "world"}` |
| `/static/<filename>` | GET | Serves static files |
| `/upload` | POST | Upload an image file |
| `/media/<filename>` | GET | View an uploaded image |
