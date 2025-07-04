# Flask-Web-App

CI/CD pipeline for a simple Python Flask web app.

## Folder Structure

```
FLASK-WEB-APP/
│
├── .github/
│   └── workflows/
│       └── ci-cd.yml
│
├── tests/
│   └── test_demo.py
│
├── app.py
├── Dockerfile
├── README.md
└── requirements.txt
```

## 1. Set Up a Basic Flask Web App

Create a folder named `Web app` (or any name), open it in **VS Code**, and then open the terminal inside it.

### Install Flask

```bash
pip install flask
```

### Create app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, DevOps!"

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=5000)
```

### Test Locally

```bash
python app.py
```

## 2. Create a GitHub Repository & Push the Code

```bash
git init
echo "__pycache__/
*.pyc" > .gitignore
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin <your-repo-url>
git push -u origin main
```

## 3. Set Up GitHub Actions for CI/CD

Create a workflow file: `.github/workflows/ci-cd.yml`

```yaml
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
          python-version: '3.8'

      - name: Install dependencies
        run: pip install flask pytest

      - name: Run tests
        run: pytest tests/
        
  deploy:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy (Example: Docker)
        run: echo "Deploying to server..."
```

## 4. Deploy Using Docker

### Create Dockerfile

```dockerfile
FROM python:3.8
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

### Build & Run Docker Container

```bash
docker build -t flaskwebapp .
docker run -p 5000:5000 flaskwebapp
```

## Additional Files

### requirements.txt

```
flask
pytest
```

### tests/test_demo.py

```python
import pytest
from app import app

@pytest.fixture
def client():
    app.config['TESTING'] = True
    with app.test_client() as client:
        yield client

def test_home(client):
    response = client.get('/')
    assert response.status_code == 200
    assert b"Hello, DevOps!" in response.data
```
