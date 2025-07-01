# Flask-Web-App
CI/CD pipeline for a simple Python Flask web app

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

1. Set Up a Basic Flask Web App

Create a folder named Web app or any, then open it in VSC, and inside open terminal then install flask:

pip install flask

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

Then inside create a app.py

Create app.py:
-------------------------------------------------
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, DevOps!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
-------------------------------------------------

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

Test locally:
python app.py

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

2. Create a GitHub Repository & Push the Code

git init

echo "__pycache__/
*.pyc" > .gitignore

git add .

git commit -m "Initial commit"

git branch -M main

git remote add origin <repo-url>

git push -u origin main

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

3. Set Up GitHub Actions for CI/CD


Create .github/workflows/ci-cd.yml
-----------------------------------
name: CI/CD Pipeline

on: [push, pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: "3.8"

      - name: Install dependencies
        run: pip install flask pytest

      - name: Run tests
        run: pytest tests/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy
        run: echo "Deploying to server..."

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

4. Deploy Using Docker

Create Dockerfile:
---------------------
FROM python:3.8
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
---------------------

/////////////////////////////////////////////////////////////////////////////////////////////////////////////

Build & run:
docker build -t flaskwebapp .
docker run -p 5000:5000 flaskwebapp

/////////////////////////////////////////////////////////////////////////////////////////////////////////////
