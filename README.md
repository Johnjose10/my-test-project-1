
# FastAPI Application

A simple REST API built using FastAPI and Python.

## Technologies

- Python
- FastAPI
- Uvicorn
- Docker
- Jenkins
- SonarQube

## Run Locally

Install dependencies:

pip install -r requirements.txt

Start application:

uvicorn main:app --host 0.0.0.0 --port 8000

## API Documentation

http://localhost:8000/docs

## Docker Build

docker build -t my-fastapi-app .

## Docker Run

docker run -d -p 8000:8000 my-fastapi-app
