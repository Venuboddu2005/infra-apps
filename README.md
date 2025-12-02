# 🚀 CI/CD Stack with Docker Compose

This project provides production-ready Docker Compose files to deploy a complete CI/CD stack on your local machine.

## 🛠️ Stack Components

- **Jenkins** - CI/CD automation server (Port: 8080)
- **Nexus** - Artifact repository manager (Port: 8081)
- **SonarQube** - Code quality analysis (Port: 9000)
- **Nginx** - Reverse proxy with domain routing (Port: 80)
- **PostgreSQL** - Database for SonarQube

## ✅ Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- 8GB+ RAM recommended

## 🚀 Quick Start

### 1. Clone Repository
```bash
git clone <repository-url>
cd infra-apps
```

### 2. Setup Network
```bash
# Create shared network
docker network create ci-cd-network

# Or use the provided script
./dev.sh
```

### 3. Start Services

**Option A: Start All Services**
```bash
docker-compose -f docker-compose-jenkins.yml -f docker-compose-nexus.yml -f docker-compose-sonarqube.yml -f docker-compose-nginx.yml up -d
```

**Option B: Start Individual Services**
```bash
# Jenkins
docker-compose -f docker-compose-jenkins.yml up -d

# Nexus
docker-compose -f docker-compose-nexus.yml up -d

# SonarQube (with PostgreSQL)
docker-compose -f docker-compose-sonarqube.yml up -d

# Nginx Reverse Proxy
docker-compose -f docker-compose-nginx.yml up -d
```

### 4. Configure Local DNS

Add to your `/etc/hosts` (Linux/Mac) or `C:\Windows\System32\drivers\etc\hosts` (Windows):
```
127.0.0.1 jenkins.local
127.0.0.1 nexus.local
127.0.0.1 sonarqube.local
```

## 🌐 Access URLs

| Service | Direct Access | Via Nginx Proxy |
|---------|---------------|------------------|
| Jenkins | http://localhost:8080 | http://jenkins.local |
| Nexus | http://localhost:8081 | http://nexus.local |
| SonarQube | http://localhost:9000 | http://sonarqube.local |

## 🔐 Default Credentials

**Jenkins:**
- Initial admin password: `docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword`

**Nexus:**
- Username: `admin`
- Password: `docker exec nexus cat /nexus-data/admin.password`

**SonarQube:**
- Username: `admin`
- Password: `admin` (change on first login)

## 📁 Data Persistence

All services use Docker volumes for data persistence:
- `jenkins_home` - Jenkins configuration and jobs
- `nexus_data` - Nexus repositories and artifacts
- `sonarqube_data` - SonarQube configuration
- `postgresql_data` - Database data

## 🛠️ Management Commands

**Stop all services:**
```bash
docker-compose -f docker-compose-jenkins.yml -f docker-compose-nexus.yml -f docker-compose-sonarqube.yml -f docker-compose-nginx.yml down
```

**View logs:**
```bash
docker-compose -f docker-compose-jenkins.yml logs -f jenkins
```

**Restart service:**
```bash
docker-compose -f docker-compose-jenkins.yml restart jenkins
```

## 🔧 Customization

- **Jenkins plugins:** Edit `plugins.txt` and rebuild custom image
- **Nginx routing:** Modify `nginx.conf` for custom domains
- **Resource limits:** Adjust memory/CPU limits in compose files

## 📋 Troubleshooting

**Services not starting:**
```bash
# Check network exists
docker network ls | grep ci-cd-network

# Check container status
docker ps -a

# View service logs
docker logs <container-name>
```

**Port conflicts:**
- Ensure ports 8080, 8081, 9000, 80 are available
- Modify port mappings in compose files if needed

**Memory issues:**
- Increase Docker memory allocation (8GB+ recommended)
- Reduce Nexus memory limit in compose file

## 📂 Repository Structure

```
infra-apps/
├── docker-compose-jenkins.yml    # Jenkins service
├── docker-compose-nexus.yml      # Nexus service
├── docker-compose-sonarqube.yml  # SonarQube + PostgreSQL
├── docker-compose-nginx.yml      # Nginx reverse proxy
├── nginx.conf                    # Nginx configuration
├── Dockerfile.jenkins            # Custom Jenkins image
├── plugins.txt                   # Jenkins plugins list
├── dev.sh                        # Setup script
└── README.md                     # This file
```
