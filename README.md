# Task 1: Automate Code Deployment Using CI/CD Pipeline (GitHub Actions)

## 📌 Objective
Set up a complete CI/CD pipeline to **build**, **test**, and **deploy** a sample Node.js web application using **GitHub Actions** and **DockerHub**.

## 🛠️ Tech Stack
- **Node.js**
- **Docker**
- **GitHub Actions**
- **DockerHub**
- **Git**

## 🚀 Features
- Continuous Integration: Automatically runs tests and builds the Docker image on every push to `main`.
- Continuous Deployment: Pushes Docker image to DockerHub.

## 📁 Project Structure
. ├── .github │ └── workflows │ └── main.yml # CI/CD pipeline definition ├── app │ └── index.js # Sample Node.js application ├── Dockerfile # Docker image build instructions ├── .dockerignore ├── package.json └── README.md

yaml
Copy
Edit

## ⚙️ GitHub Actions Workflow

### Workflow File: `.github/workflows/main.yml`

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Node.js
      uses: actions/setup-node@v4
      with:
        node-version: '18'

    - name: Install dependencies
      run: npm install

    - name: Run tests
      run: npm test

    - name: Log in to DockerHub
      uses: docker/login-action@v3
      with:
        username: ${{ secrets.DOCKERHUB_USERNAME }}
        password: ${{ secrets.DOCKERHUB_TOKEN }}

    - name: Build and push Docker image
      run: |
        docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/node-app:latest .
        docker push ${{ secrets.DOCKERHUB_USERNAME }}/node-app:latest

## 🐳 Docker
Dockerfile
Dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install



EXPOSE 3000
CMD ["node", "app/index.js"]
🧪 Testing
Ensure your Node.js app has a basic test script in package.json:

json
Copy
Edit
"scripts": {
  "test": "echo \"Running tests...\" && exit 0"
}
🔑 Secrets Configuration
Add the following secrets in your GitHub repo under Settings > Secrets and variables > Actions:

DOCKERHUB_USERNAME – Your DockerHub username

DOCKERHUB_TOKEN – Your DockerHub access token/password

✅ How it works
Push to main branch triggers the workflow.

Code is checked out and dependencies installed.

Tests are run.

Docker image is built and pushed to DockerHub.

🔗 DockerHub Repo
DockerHub: pradnyathite91/node-app

📦 Sample Node.js App
Basic app is served from app/index.js:

js
const http = require('http');
const port = 3000;

const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.end('Hello, CI/CD World!');
});

server.listen(port, () => {
  console.log(`Server running on http://localhost:${port}`);
});
