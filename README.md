# 🚀 Flask Microloans API + PostgreSQL (Docker + AWS + Jenkins CI/CD)

A **production-ready REST API** for managing microloans, built with **Flask**, **SQLAlchemy**, **Alembic**, and **PostgreSQL**, containerized using **Docker**, and deployed on **AWS EC2** using **Jenkins CI/CD**.

---

## 🧱 Features

- Flask REST API for microloans management  
- PostgreSQL database via Docker  
- SQLAlchemy ORM + Alembic migrations  
- Seed script for dummy data  
- AWS EC2 + Docker + Jenkins CI/CD pipeline  
- Modular and scalable structure  

---

## ⚙️ Tech Stack

| Component | Technology |
|------------|-------------|
| Backend | Flask (Python) |
| Database | PostgreSQL |
| ORM | SQLAlchemy |
| Migrations | Alembic |
| Containerization | Docker & Docker Compose |
| Deployment | AWS EC2 |
| CI/CD | Jenkins |
| Version Control | Git & GitHub |

---

## 🧑‍💻 Local Development Setup

1. Clone the repository and change directory:
```
git clone https://github.com/dheeraj1kumar/loan.git
cd dummy-branch-app
```

2. Build and start services:
```
docker compose up -d --build
```

3. Run migrations and seed data:
```
docker compose exec api alembic upgrade head
docker compose exec api python scripts/seed.py
```

4. Test API endpoints:
```
curl http://public ip of ec2:8000/health
curl http://public ip of ec2:8000/api/loans
```

---

## 🔍 Screenshots (as requested for PM)

Below are the screenshots you provided. I copied them into `/docs/images/` and referenced them here so you can present to your project manager. Each image includes a short caption about what it shows.

### 1) EC2 Instances list (Stopped/Branch)
![EC2 Instances](./docs/images/Screenshot%20(10).png)
*Caption: EC2 console showing instances (branch, stopped).*

### 2) Jenkins pipeline overview (successful stages)
![Jenkins Pipeline Overview](./docs/images/Screenshot%20(11).png)
*Caption: Jenkins pipeline overview showing Checkout, Build, Push, Test, Deploy succeeded.*

### 3) Docker Hub repositories (branch-app pushed)
![Docker Hub Repos](./docs/images/Screenshot%20(12).png)
*Caption: Docker Hub repository showing `dheeraj1kumar/branch-app` uploaded.*

### 4) Postman - Empty list response initially
![Postman Empty](./docs/images/Screenshot%20(13).png)
*Caption: Initial POST returned empty array (before seed/migrations).*

### 5) Postman - Successful POST (201 Created)
![Postman Created](./docs/images/Screenshot%20(16).png)
*Caption: POST /api/loans returned 201 Created with created record.*

### 6) EC2 Console (docker ps and container status)
![EC2 Docker PS](./docs/images/Screenshot%20(18).png)
*Caption: `docker ps -a` on EC2 showing branch_api_1 and branch_db_1 containers.*

### 7) API response listing loans (console)
![API Loans List](./docs/images/Screenshot%20(19).png)
*Caption: GET /api/loans shows seeded loan records in JSON.*

### 8) Jenkins pipeline run with DB Update stage (all green)
![Jenkins Run Success](./docs/images/Screenshot%20(20).png)
*Caption: Jenkins run #15 overview with DB Update stage successful.*

### 9) Final API GET showing multiple loans
![Final API GET](./docs/images/Screenshot%20(19).png)
*Caption: Production API GET response listing several loan entries.*

---

## ✅ Notes  (Step-by-step summary)

1. **Code & CI**: Jenkins builds the image, pushes to Docker Hub, and deploys to EC2 via `docker-compose`. The Jenkins pipeline stages (Checkout → Build → Push → DB Update → Deploy) all succeeded in the latest run.
2. **Deployment**: On EC2, `docker compose up -d` creates `branch_api_1` and `branch_db_1`. Postgres reports healthy in `docker ps -a` output.
3. **DB Migrations**: Alembic migrations were applied during the `DB Update` stage. If you see empty lists initially, run the seed script: `docker compose exec api python scripts/seed.py`.
4. **API Validation**: Verified with Postman: POST returns `201 Created` and GET returns loan records as shown in screenshots.
5. **Next steps**: Configure health checks, secrets management (AWS Secrets Manager), and optionally migrate DB to RDS for production resilience.

---

## 📁 Files added by this script
Copied 9 images to `/loan/docs/images`.
Missing images (if any): 0

---

## 📥 Where to find the updated README file and images

- README: `/mnt/data/README_updated.md`
- Images folder: `/loan/docs/images`

---

If you want, I can automatically rename `README_updated.md` to `README.md` and prepare a git commit command sequence you can run on the EC2 host to push these changes to your repository. Let me know which commit message you'd like to use.
