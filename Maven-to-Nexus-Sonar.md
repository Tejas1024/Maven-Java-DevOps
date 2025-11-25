# Maven-to-Nexus-Sonar.md

# Maven Integration – SonarQube & Nexus

## Overview

Typical modern pipeline:  
**Developer writes code → Maven builds (.jar/.war) → SonarQube analyzes code → Nexus stores artifact**

---

## 1. Maven Project Setup (on EC2 Instance)
- VM: t2.medium, 10 GB
- Commands:
    ```
    sudo apt update
    sudo apt install openjdk-17-jdk maven -y
    git clone <repo_url>
    cd <project_folder>
    mvn clean install
    ```
    - `mvn clean install`: Cleans, builds, and packages project as .jar in `target/`

---

## 2. SonarQube Setup
- Instance: t2.medium, 10 GB
- Commands:
    ```
    sudo apt update
    sudo apt install openjdk-17-jdk unzip -y
    wget -O sonarqube.zip <url>
    unzip sonarqube.zip
    cd sonarqube-*/bin/linux-x86-64/
    ./sonar.sh start
    ```
- UI: http://<public-ip>:9000  
- Login: admin/admin

1. Change default password
2. Create Project (e.g. chrome) – manual setup
3. From UI: get Maven command & token, e.g.
    ```
    mvn sonar:sonar -Dsonar.projectKey=chrome -Dsonar.host.url=http://<sonar_ip>:9000 -Dsonar.login=<token>
    ```
4. Run above in project root.

---

## 3. Nexus Repository Manager
- Instance: t2.medium, 10 GB
- Commands:
    ```
    sudo apt update
    sudo apt install openjdk-17-jdk -y
    wget -O nexus.tar.gz <url>
    tar -xvzf nexus.tar.gz
    cd nexus-*/bin
    ./nexus start
    ```
- UI: http://<public-ip>:8081  
- Login: admin/(password in `/nexus-data/admin.password`)

Create repository (hosted, maven2, “sample”).
Set policy: “mixed”, allow redeploy.

---

## Summary Table

| Tool      | Purpose         | Port   | Notes          |
|-----------|-----------------|--------|----------------|
| Maven     | Build/packaging | —      | Needs Java     |
| SonarQube | Code analysis   | 9000   | Checks quality |
| Nexus     | Artifact repo   | 8081   | Stores builds  |

**Flow:**  
Push code → Maven build → Sonar analysis (optional) → Maven deploy to Nexus repo
