# 🚀 Jobnex — Intelligent Resume & Job Description Analyzer 

> **Bridge the gap between your resume and your dream job with automated matching, skill gap analysis, and personalized career suggestions.**

[![FastAPI](https://img.shields.io/badge/FastAPI-005571?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

## 📌 1. Overview

**Jobnex** is an automated resume screening and job description analysis platform. It parses candidate resumes and job descriptions (JDs), performs keyword and skill-matching algorithms, calculates similarity scores, highlights missing proficiencies, and provides actionable recommendations to boost interview chances.

All analysis reports are persisted in a relational **MySQL** database for history tracking and candidate auditing, powered by high-performance **FastAPI** backend services and a clean, responsive web frontend.

---

## ✨ 2. Key Features

- ⚡ **Real-Time Skill Matching:** Instantly identifies overlapping technical competencies between resumes and job requirements.
- 🎯 **Accurate Match Scoring:** Computes transparent similarity percentages based on industry-relevant tech stacks.
- 🔍 **Skill Gap Detection:** Surfaces missing skills and requirements candidates need to acquire or showcase.
- 💡 **Actionable Career Suggestions:** Generates customized advice on what skills to learn and add to resumes.
- 🗄️ **Persistent History Tracking:** Stores resumes, JDs, scores, and tagged skills in MySQL as structured JSON records.
- 📖 **Interactive API Documentation:** Native Swagger UI (`/docs`) and ReDoc (`/redoc`) powered by FastAPI.
- 🌐 **Modern Web Interface:** Clean, lightweight frontend built with HTML5, CSS3, and modern JavaScript.

---

## 🛠️ 3. Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Backend Framework** | [FastAPI](https://fastapi.tiangolo.com/) | Asynchronous, high-performance RESTful API |
| **Language** | [Python 3.9+](https://www.python.org/) | Core backend and matching logic |
| **Database** | [MySQL 8.0+](https://www.mysql.com/) | Persistent relational storage & JSON data support |
| **DB Connector** | `mysql-connector-python` | Database communication and connection pooling |
| **Data Validation** | [Pydantic](https://docs.pydantic.dev/) | Request and response schema enforcement |
| **Web Server** | [Uvicorn](https://www.uvicorn.org/) | Lightning-fast ASGI production web server |
| **Frontend** | HTML5 / CSS3 / JavaScript | Responsive, interactive client user interface |

---

## 📂 4. Project Folder Structure

```text
jobnex/
├── backend/
│   ├── Algonex/               # Auxiliary algorithm modules
│   ├── analyzer.py            # Resume & JD skill-matching algorithm
│   ├── database.py            # MySQL database connection handler
│   ├── insert_test.py         # DB insertion unit test script
│   ├── main.py                # FastAPI app, endpoints, and CORS config
│   ├── test_analyzer.py       # Analyzer logic unit test
│   └── test_db.py             # Database connectivity test
├── index.html                 # Main frontend landing page
├── script.js                  # Frontend client logic & fetch API handler
├── style.css                  # UI styling and responsive design
├── .gitignore                 # Git ignore file for secrets and caches
├── README.md                  # Project documentation
```

---

## ⚙️ 5. Installation Guide

Follow these steps to set up and run Jobnex on your local environment:

### Prerequisites
- **Python 3.9+** installed on your system
- **MySQL Server** installed, running, and accessible on port `3306`
- **Git** installed

### Step 1: Clone the Repository
```bash
git clone https://github.com/kolkarnisha/jobnex.git
cd jobnex
```

### Step 2: Set Up Virtual Environment (Recommended)
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS / Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Required Dependencies
```bash
pip install fastapi uvicorn mysql-connector-python pydantic
```

### Step 4: Configure the MySQL Database
Log into your MySQL shell or workbench and execute:

```sql
CREATE DATABASE IF NOT EXISTS jobnex;

USE jobnex;

CREATE TABLE IF NOT EXISTS resume_analyzer (
    analysis_id INT AUTO_INCREMENT PRIMARY KEY,
    resume_text LONGTEXT NOT NULL,
    jd_text LONGTEXT NOT NULL,
    match_score DECIMAL(5,2),
    matched_skills JSON,
    missing_skills JSON,
    sugesstions JSON,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

> **Note:** Update your database credentials (username and password) inside [`backend/database.py`](backend/database.py).

---

## 🚀 6. Usage Instructions

### 1. Start the FastAPI Backend Server
Navigate to the `backend` folder and start Uvicorn:

```bash
cd backend
uvicorn main:app --reload --port 8000
```
- Server URL: `http://127.0.0.1:8000`
- Interactive Swagger UI: `http://127.0.0.1:8000/docs`
- Redoc UI: `http://127.0.0.1:8000/redoc`

### 2. Launch the Web Frontend
Open `index.html` in your browser (or use VS Code Live Server on `http://127.0.0.1:5500`).

---

## 📡 7. API Reference

### Health Check
- **Endpoint:** `GET /`
- **Response:**
  ```json
  {
    "message": "Resume Analyzer Backend is Running"
  }
  ```

### Analyze Resume vs Job Description
- **Endpoint:** `POST /api/analyze`
- **Headers:** `Content-Type: application/json`
- **Sample Request Body:**
  ```json
  {
    "resume_text": "Experienced Python developer proficient in FastAPI, SQL, Docker, and building microservices.",
    "jd_text": "Looking for a Software Engineer with skills in Python, SQL, React, and Docker."
  }
  ```
- **Sample Response:**
  ```json
  {
    "Analysis_Id": 28,
    "matched_skills": [
      "python",
      "sql",
      "docker"
    ],
    "missing_skills": [
      "react",
      "fastapi"
    ],
    "sugesstions": [
      "Try learning react for this profile add react to your resume",
      "Try learning fastapi for this profile add fastapi to your resume"
    ],
    "match_score": 60.0
  }
  ```

### Retrieve Past Analyses History
- **Endpoint:** `GET /api/analyses`
- **Response:** Returns list of the last 20 evaluations with decoded skill tags and match scores.

---

## 🔮 8. Future Scope & Roadmap

- [ ] **PDF / DOCX Resume Parsing:** Support direct file uploads via `pdfplumber` or `PyPDF2`.
- [ ] **AI-Powered Semantic Scoring:** Integrate Sentence Transformers / OpenAI Embeddings for contextual matching beyond simple keywords.
- [ ] **Experience & Education Weighting:** Calculate comprehensive fit based on years of experience, degree, and certifications.
- [ ] **ATS Score Meter:** Visual radial progress bar and downloadable PDF analysis reports.
- [ ] **Authentication & User Profiles:** Support saved accounts for applicants and recruiters.

---

## 🤝 9. Contributing Guidelines

Contributions, issues, and feature requests are welcome!

1. Fork the repository.
2. Create your feature branch (`git checkout -b feature/AmazingFeature`).
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 10. License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

## 📬 11. Contact & Author

**Kolkar Nisha**  
- **GitHub:** [@kolkarnisha](https://github.com/kolkarnisha)  
- **Repository:** [https://github.com/kolkarnisha/jobnex](https://github.com/kolkarnisha/jobnex)  
- **LinkedIn:** [Connect on LinkedIn](https://www.linkedin.com/in/kolkarnisha/)  
- **Email:** neeshakolkar@gmail.com  

⭐ *If you find this project helpful, don't forget to give it a star on GitHub!*
