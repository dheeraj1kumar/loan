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

### 1️⃣ Clone the repository
```bash
git clone https://github.com/<your-username>/dummy-branch-app.git
cd dummy-branch-app
```

### 2️⃣ Create and activate a virtual environment
```bash
python -m venv venv
source venv/bin/activate       # (Linux/Mac)
venv\Scripts\activate          # (Windows)
```

### 3️⃣ Build and start Docker services
```bash
docker compose up -d --build
```

### 4️⃣ Run database migrations
```bash
docker compose exec api alembic upgrade head
```

### 5️⃣ Seed dummy data (optional)
```bash
docker compose exec api python scripts/seed.py
```

### 6️⃣ Test API endpoints
```bash
curl http://localhost:8000/health
curl http://localhost:8000/api/loans
```

---

## ⚙️ Default Configuration

Example `.env` configuration:
```
DATABASE_URL=postgresql+psycopg2://postgres:postgres@db:5432/microloans
FLASK_ENV=development
PORT=8000
```

By default:
- API runs on `http://localhost:8000`
- Database service: `db`

---

## 🧭 API Endpoints

| Method | Endpoint | Description |
|--------|-----------|-------------|
| GET | `/health` | Check API health |
| GET | `/api/loans` | List all loans |
| GET | `/api/loans/:id` | Get a loan by ID |
| POST | `/api/loans` | Create a new loan |
| GET | `/api/stats` | Fetch loan statistics |

### Example Create Request
```bash
curl -X POST http://localhost:8000/api/loans \
  -H "Content-Type: application/json" \
  -d '{
    "borrower_id": "usr_india_999",
    "amount": 12000.50,
    "currency": "INR",
    "term_months": 6,
    "interest_rate_apr": 24.0
  }'
```

---

## 🐳 Docker Architecture

```text
+-----------------------------+
|         API (Flask)         |
|   app/, models, routes,     |
|   Alembic migrations        |
+-------------+---------------+
              |
              v
+-----------------------------+
|       PostgreSQL DB         |
|  Data persistence, volumes  |
+-----------------------------+
```

📸 **Suggested Image Placement:**  
`/docs/images/docker-architecture.png`

---

## ☁️ AWS EC2 Deployment (Production)

### Steps:
1. Launch **Ubuntu EC2** instance
2. Install dependencies:
   ```bash
   sudo apt update
   sudo apt install docker.io docker-compose git -y
   ```
3. Clone the repo & start services:
   ```bash
   git clone https://github.com/<your-username>/dummy-branch-app.git
   cd dummy-branch-app
   docker compose up -d --build
   ```
4. Verify using:
   ```bash
   curl http://<EC2-Public-IP>:8000/health
   ```

📸 **Suggested Image Placement:**  
`/docs/images/aws-ec2-setup.png`

---

## 🔄 Jenkins CI/CD Pipeline (Production Deployment)

### Pipeline Stages
1. **Clone Repository** – Pull code from GitHub  
2. **Build Docker Images** – Build API & DB containers  
3. **Run Migrations** – Apply Alembic migrations  
4. **Deploy Containers** – Start or update running containers  
5. **Health Check** – Validate API response  

### Example `Jenkinsfile`
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/dummy-branch-app.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Run Migrations') {
            steps {
                sh 'docker compose exec api alembic upgrade head'
            }
        }
        stage('Deploy Containers') {
            steps {
                sh 'docker compose up -d'
            }
        }
        stage('Post-deploy Check') {
            steps {
                sh 'curl http://localhost:8000/health'
            }
        }
    }
}
```

📸 **Suggested Image Placement:**  
`/docs/images/jenkins-pipeline.png`

---

## 🧩 Project Structure

```bash
dummy-branch-app/
├── app/
│   ├── __init__.py
│   ├── models.py
│   ├── routes.py
│   └── schemas.py
├── alembic/
├── scripts/
│   └── seed.py
├── docker-compose.yml
├── Dockerfile
├── Jenkinsfile
├── .env.example
├── README.md
└── wsgi.py
```

---

## 🧠 Notes

- Amount validation: `0 < amount ≤ 50000`  
- Authentication: Not included (prototype)  
- For production: Use `.env` with strong credentials  
- Add **Nginx reverse proxy** for HTTPS and load balancing

---

## 🧾 License

This project is licensed under the **MIT License**.

---

## 👨‍💻 Author

**Dheeraj Kumar**  
Full Stack Developer — Flask | React | Docker | AWS  
📧 Email: dheerajkumar@example.com  
🔗 GitHub: [@dheeraj1kumar](https://github.com/dheeraj1kumar)

---

## 🖼️ Image Placement Summary

| Section | Suggested Path | Description |
|----------|----------------|-------------|
| Docker Architecture | `/docs/images/docker-architecture.png` | Overview of API & DB containers |
| AWS EC2 Setup | `/docs/images/aws-ec2-setup.png` | EC2 instance setup visual |
| Jenkins Pipeline | `/docs/images/jenkins-pipeline.png` | CI/CD pipeline flow diagram |

Add images using:
```markdown
![Docker Architecture](docs/images/docker-architecture.png)
![Jenkins Pipeline](docs/images/jenkins-pipeline.png)
```

---

⭐ **Pro Tip:** Add screenshots under `docs/images/` and link them as shown above for professional project documentation.
