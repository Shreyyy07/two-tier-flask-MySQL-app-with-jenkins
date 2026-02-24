# 🚀 Two-Tier Flask-MySQL Application with Jenkins CI/CD

A containerized two-tier web application built with Flask and MySQL, featuring automated CI/CD deployment using Jenkins.

![Python](https://img.shields.io/badge/Python-3.10-blue)
![Flask](https://img.shields.io/badge/Flask-3.1.2-green)
![MySQL](https://img.shields.io/badge/MySQL-8.0-orange)
![Docker](https://img.shields.io/badge/Docker-Enabled-2496ED)
![Jenkins](https://img.shields.io/badge/Jenkins-CI%2FCD-red)

## 📑 Table of Contents
- [Architecture](#architecture)
- [Features](#features)
- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Prerequisites](#prerequisites)
- [Installation & Setup](#installation--setup)
- [Usage](#usage)
- [CI/CD Pipeline](#cicd-pipeline)
- [Environment Variables](#environment-variables)
- [API Endpoints](#api-endpoints)

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         CLIENT BROWSER                          │
│                      (HTTP Requests/Responses)                  │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            │ Port 5000
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      JENKINS CI/CD SERVER                       │
│  ┌────────────────────────────────────────────────────────┐    │
│  │  Pipeline Stages:                                       │    │
│  │  1. Build Docker Image                                  │    │
│  │  2. Stop Old Container                                  │    │
│  │  3. Run New Container                                   │    │
│  └────────────────────────────────────────────────────────┘    │
└───────────────────────────┬────────────���────────────────────────┘
                            │
                            │ Docker Build & Deploy
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    DOCKER CONTAINER LAYER                       │
│                                                                 │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │           Flask Application Container                    │  │
│  │           (two-tier-app)                                 │  │
│  │                                                          │  │
│  │  ┌────────────────────────────────────────────────┐    │  │
│  │  │        PRESENTATION TIER                       │    │  │
│  │  │  ┌──────────────────────────────────────┐     │    │  │
│  │  │  │  Flask Web Server (Port 5000)        │     │    │  │
│  │  │  │  - app.py (Main Application)         │     │    │  │
│  │  │  │  - Routes: /, /submit, /edit,        │     │    │  │
│  │  │  │    /update, /delete                  │     │    │  │
│  │  │  └──────────────────────────────────────┘     │    │  │
│  │  │                                                │    │  │
│  │  │  ┌──────────────────────────────────────┐     │    │  │
│  │  │  │  Templates (Jinja2)                  │     │    │  │
│  │  │  │  - index.html (List Users)           │     │    │  │
│  │  │  │  - edit.html (Edit User)             │     │    │  │
│  │  │  └──────────────────────────────────────┘     │    │  │
│  │  │                                                │    │  │
│  │  │  ┌──────────────────────────────────────┐     │    │  │
│  │  │  │  Static Assets                       │     │    │  │
│  │  │  │  - CSS/JS Files                      │     │    │  │
│  │  │  └──────────────────────────────────────┘     │    │  │
│  │  └────────────────────────────────────────────────┘    │  │
│  └──────────────────────────┬───────────────────────────────┘  │
│                             │                                   │
│                             │ MySQL Connector                   │
│                             │ (mysql.connector.connect)         │
│                             ▼                                   │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │           MySQL Database Container                       │  │
│  │           (mysql-container)                              │  │
│  │                                                          │  │
│  │  ┌────────────────────────────────────────────────┐    │  │
│  │  │          DATA TIER                             │    │  │
│  │  │  ┌──────────────────────────────────────┐     │    │  │
│  │  │  │  MySQL 8.0 Server (Port 3306)        │     │    │  │
│  │  │  │                                       │     │    │  │
│  │  │  │  Database: twotier                   │     │    │  │
│  │  │  │  ┌─────────────────────────────┐    │     │    │  │
│  │  │  │  │  Table: users               │    │     │    │  │
│  │  │  │  │  - id (INT, PRIMARY KEY)    │    │     │    │  │
│  │  │  │  │  - name (VARCHAR(100))      │    │     │    │  │
│  │  │  │  └─────────────────────────────┘    │     │    │  │
│  │  │  └──────────────────────────────────────┘     │    │  │
│  │  └────────────────────────────────────────────────┘    │  │
│  └──────────────────────────────────────────────────────────┘  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘

                    Docker Network Bridge
```

### Architecture Components:

1. **Presentation Tier (Flask Application)**
   - Handles HTTP requests and responses
   - Renders HTML templates using Jinja2
   - Processes form submissions and user interactions
   - Exposes REST API endpoints for CRUD operations

2. **Data Tier (MySQL Database)**
   - Persistent data storage
   - Manages user records in the `users` table
   - Provides ACID compliance for data integrity

3. **CI/CD Layer (Jenkins)**
   - Automated build process
   - Docker image creation
   - Container lifecycle management
   - Continuous deployment

## ✨ Features

- ✅ **User Management**: Create, Read, Update, Delete (CRUD) operations
- ✅ **Containerized Deployment**: Docker & Docker Compose support
- ✅ **CI/CD Pipeline**: Automated Jenkins deployment
- ✅ **Database Persistence**: MySQL 8.0 for data storage
- ✅ **Environment-based Configuration**: Secure credential management
- ✅ **RESTful API**: Clean endpoint structure
- ✅ **Responsive Web Interface**: HTML templates with Jinja2

## 🛠️ Technologies Used

| Technology | Version | Purpose |
|------------|---------|---------|
| Python | 3.10 | Backend language |
| Flask | 3.1.2 | Web framework |
| MySQL | 8.0 | Relational database |
| mysql-connector-python | 9.6.0 | Database driver |
| Docker | Latest | Containerization |
| Docker Compose | v3 | Multi-container orchestration |
| Jenkins | Latest | CI/CD automation |

## 📁 Project Structure

```
two-tier-flask-MySQL-app-with-jenkins/
├── app.py                  # Main Flask application
├── requirements.txt        # Python dependencies
├── Dockerfile              # Docker image definition
├── docker-compose.yml      # Multi-container setup
├── Jenkinsfile             # CI/CD pipeline configuration
├── .gitignore              # Git ignore rules
├── templates/              # HTML templates (Jinja2)
│   ├── index.html          # User list view
│   └── edit.html           # User edit view
└── static/                 # Static assets (CSS, JS, images)
```

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- **Docker**: >= 20.10
- **Docker Compose**: >= 1.29
- **Jenkins**: >= 2.x (for CI/CD)
- **Git**: >= 2.x

## 🚀 Installation & Setup

### Option 1: Docker Compose (Recommended)

1. **Clone the repository**
   ```bash
   git clone https://github.com/Shreyyy07/two-tier-flask-MySQL-app-with-jenkins.git
   cd two-tier-flask-MySQL-app-with-jenkins
   ```

2. **Start the application**
   ```bash
   docker-compose up -d
   ```

3. **Access the application**
   - Open browser: `http://localhost:5000`
   - MySQL: `localhost:3306`

### Option 2: Manual Docker Setup

1. **Build the Docker image**
   ```bash
   docker build -t two-tier-app .
   ```

2. **Run MySQL container**
   ```bash
   docker run -d \
     --name mysql-container \
     -e MYSQL_ROOT_PASSWORD=rootpassword \
     -e MYSQL_DATABASE=twotier \
     -p 3306:3306 \
     mysql:8
   ```

3. **Run Flask application**
   ```bash
   docker run -d \
     --name two-tier-app \
     --link mysql-container:mysql \
     -e DB_HOST=mysql-container \
     -e DB_USER=root \
     -e DB_PASSWORD=rootpassword \
     -e DB_NAME=twotier \
     -p 5000:5000 \
     two-tier-app
   ```

## 🎯 Usage

### Web Interface

1. **View Users**: Navigate to `http://localhost:5000`
2. **Add User**: Enter a username and click "Submit"
3. **Edit User**: Click the "Edit" button next to a user
4. **Delete User**: Click the "Delete" button next to a user

### API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Display all users |
| POST | `/submit` | Add a new user |
| GET | `/edit/<id>` | Edit user form |
| POST | `/update/<id>` | Update user |
| GET | `/delete/<id>` | Delete user |

## 🔄 CI/CD Pipeline

The Jenkins pipeline automates the deployment process with three stages:

### Jenkinsfile Stages

```groovy
1. Build Docker Image
   └─ Builds the Flask app image from Dockerfile

2. Stop Old Container
   └─ Removes existing container (if any)

3. Run New Container
   └─ Deploys the new container on port 5000
```

### Setup Jenkins Pipeline

1. **Install Jenkins plugins**:
   - Docker Pipeline
   - Git Plugin

2. **Create a new Pipeline job**:
   - Source: Git
   - Repository URL: `https://github.com/Shreyyy07/two-tier-flask-MySQL-app-with-jenkins.git`
   - Script Path: `Jenkinsfile`

3. **Configure Docker permissions**:
   ```bash
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```

4. **Run the pipeline**: Jenkins will automatically build and deploy on every commit

### Setting Environment Variables

**Docker Compose:**
```yaml
environment:
  - DB_HOST=mysql
  - DB_USER=root
  - DB_PASSWORD=rootpassword
  - DB_NAME=twotier
```

**Docker Run:**
```bash
docker run -e DB_HOST=mysql -e DB_USER=root ...
```

### 🗄️ Database Schema

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100)
);
```

## 🐛 Troubleshooting

### Common Issues

1. **Cannot connect to MySQL**
   ```bash
   # Check if MySQL container is running
   docker ps | grep mysql
   
   # Check logs
   docker logs mysql-container
   ```

2. **Port already in use**
   ```bash
   # Find process using port 5000
   lsof -i :5000
   
   # Kill the process
   kill -9 <PID>
   ```

3. **Jenkins permission denied**
   ```bash
   sudo usermod -aG docker jenkins
   sudo systemctl restart jenkins
   ```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is open-source and available under the MIT License.

## 👤 Author

**Shreyyy07**
- GitHub: [@Shreyyy07](https://github.com/Shreyyy07)

---

**⭐ If you found this project helpful, please give it a star!**
