- make folders and files structure
- copy 


Perfect 👍
I’ll now give you a **BEGINNER-FRIENDLY, STEP-BY-STEP CI/CD GUIDE** **specifically for *this exact Flask URL Shortener code*** using **GitHub Actions**.s

I’ll assume:

* You are **new to CI/CD**
* You want **real deployment**, not theory
* You want something you can **teach or follow yourself**

---

# 🚀 CI/CD FOR FLASK URL SHORTENER

### Using **GitHub Actions** (Beginner → Real-World)

---

## 🎯 WHAT CI/CD WILL DO (FIRST UNDERSTAND THIS)

Whenever you **push code to GitHub**:

1. GitHub Actions will **build your app**
2. It will **create a Docker image**
3. It will **push the image to Docker Hub**
4. (Optional next step) Deploy to EC2 / EKS

For now, we’ll do **CI + basic CD** (industry-correct for freshers).

---

# 🧱 STEP 0: PROJECT STRUCTURE (VERY IMPORTANT)

Your GitHub repository should look like this:

```
url-shortener/
├── app.py
├── requirements.txt
├── Dockerfile
├── templates/
│   └── index.html
└── .github/
    └── workflows/
        └── cicd.yml
```

---

# 🧩 STEP 1: CREATE `requirements.txt`

Since you are using **mysql.connector**, your file must be:

```txt
flask
mysql-connector-python
```

---

# 🧩 STEP 2: CREATE `Dockerfile`

This Dockerfile is **100% compatible with your code**.

```dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 5000

CMD ["python", "app.py"]
```

🧠 **Understand this**

* Docker packages your Flask app
* Same image runs locally, on EC2, on Kubernetes

---

# 🧩 STEP 3: TEST LOCALLY (DON’T SKIP)

```bash
docker build -t url-shortener .
docker run -p 5000:5000 url-shortener
```

Open:

```
http://localhost:5000
```

❗ **Only continue if this works**

---

# 🧩 STEP 4: CREATE GITHUB REPOSITORY

1. Go to **GitHub**
2. Create repo → `url-shortener`
3. Push your code:

```bash
git init
git add .
git commit -m "Initial URL shortener"
git branch -M main
git remote add origin https://github.com/<your-username>/url-shortener.git
git push -u origin main
```

---

# 🧩 STEP 5: CREATE DOCKER HUB ACCOUNT

* Go to [https://hub.docker.com](https://hub.docker.com)
* Create account
* Create repository:

  * Name: `url-shortener`
  * Visibility: Public

Your image name will be:

```
<dockerhub-username>/url-shortener
```

---

# 🧩 STEP 6: ADD GITHUB SECRETS (VERY IMPORTANT)

### GitHub → Repo → Settings → Secrets → Actions → New secret

Add these:

| Secret Name       | Value                   |
| ----------------- | ----------------------- |
| `DOCKER_USERNAME` | your_dockerhub_username |
| `DOCKER_PASSWORD` | your_dockerhub_password |

🧠 **Why secrets?**

> Never store passwords in code.

---

# 🧩 STEP 7: CREATE CI/CD WORKFLOW FILE

Create file:

```
.github/workflows/cicd.yml
```

---

## ✅ FULL CI/CD WORKFLOW (COPY-PASTE)

```yaml
name: CI-CD Flask URL Shortener

on:
  push:
    branches:
      - main

jobs:
  build-and-push:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout code
      uses: actions/checkout@v3

    - name: Set up Docker
      uses: docker/setup-buildx-action@v2

    - name: Login to Docker Hub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_PASSWORD }}

    - name: Build Docker image
      run: |
        docker build -t ${{ secrets.DOCKER_USERNAME }}/url-shortener:latest .

    - name: Push Docker image
      run: |
        docker push ${{ secrets.DOCKER_USERNAME }}/url-shortener:latest
```

---

# 🧠 WHAT JUST HAPPENED (VERY IMPORTANT)

When you **push code**:

```
Git Push
 ↓
GitHub Actions starts
 ↓
Docker image built
 ↓
Image pushed to Docker Hub
```

🎉 **CI/CD SUCCESS**

---

# 🧪 STEP 8: TRIGGER CI/CD (LIVE DEMO)

Make a small change in `app.py`:

```python
print("CI/CD working")
```

Then:

```bash
git add .
git commit -m "Test CI/CD"
git push
```

Go to:
👉 GitHub → Actions tab
Watch pipeline run ✅

---

# 🧩 STEP 9: RUN IMAGE FROM DOCKER HUB (CD DEMO)

On **any server / system**:

```bash
docker pull <dockerhub-username>/url-shortener
docker run -p 5000:5000 <dockerhub-username>/url-shortener
```

🧠 **THIS IS CONTINUOUS DELIVERY**

---

# 🔐 IMPORTANT SECURITY NOTE (TEACH THIS)

❌ Never hardcode DB password
✔ Use environment variables (next step)

For now it’s OK for learning.

---

# 🧠 FINAL CI/CD FLOW (MEMORIZE THIS)

```
Code Push
 ↓
GitHub Actions
 ↓
Docker Build
 ↓
Docker Push
 ↓
Ready to Deploy Anywhere
```

---

# 🎓 INTERVIEW ANSWER (PERFECT)

> “I implemented CI/CD using GitHub Actions where every code push builds a Docker image and pushes it to Docker Hub automatically.”

---

# 🔜 NEXT NATURAL STEPS (WHEN READY)

I can now help you with:
1️⃣ Deploy this Docker image to **EC2 automatically**
2️⃣ CI/CD → **AWS ECR + EKS**
3️⃣ Add **env variables & secrets**
4️⃣ Blue-Green / Rolling deployment
5️⃣ Resume-ready CI/CD explanation

Just tell me 👍
