# Nexus-Deploy-Steps.md

# Uploading .jar to Nexus with Maven

## 1. Prerequisites

Instance: t2.medium, 20GB  
Security: open ports 22 (SSH), 80 (HTTP), 8081 (Nexus)

Setup:
sudo apt update
sudo apt install openjdk-17-jdk maven -y

 

## 2. Nexus Install

wget https://download.sonatype.com/nexus/3/nexus-3.80.0-06-unix.tar.gz
tar -xvzf nexus-3.80.0-06-unix.tar.gz
cd nexus-3.80.0-06/bin
./nexus start

 

Access UI: http://<public-ip>:8081  
Login:  
sudo cat /nexus-data/admin.password

 
(default admin password location)

## 3. Generate Sample Maven Project
mvn archetype:generate

sample: 2249, version: 9:9, groupId: google, artifactId: chrome, version: 1.0-SNAPSHOT, package: in.cdchrome
cd chrome
mvn package

 
(.jar now in target/)

## 4. Create Repository in Nexus (UI)

- Settings > Repositories > Create > Type: maven2 (hosted)
- Name: sample
- Policy: mixed, Allow redeploy

## 5. Configure pom.xml

Add:
<distributionManagement> <repository> <id>nexus-mixed</id> <url>http://<public-ip>:8081/repository/sample/</url> </repository> </distributionManagement> ```
6. Maven Credentials (settings.xml)
 
cd ~/.m2
nano settings.xml
Add:

 
<settings>
  <servers>
    <server>
      <id>nexus-mixed</id>
      <username>admin</username>
      <password>your-nexus-password</password>
    </server>
  </servers>
</settings>
<id> must match in both files

7. Deploy
 
mvn deploy
This builds and uploads your .jar to Nexus!

 

***

```markdown
# DevOps-Integration-Flows.md

# Maven DevOps Integration: Realistic Multi-Server Example

## Multi-Server DevOps Pipeline

**Servers:**

- Maven + Tomcat Server
- SonarQube Server
- Nexus Server

**Maven + Tomcat Server**

- Launch EC2 (t2.medium, ~20GB, open ports 22, 80, 8080, 9000, 8081)
- Install Java 17:
    ```
    sudo apt update
    sudo apt install openjdk-17-jdk -y
    ```
- Install Maven:
    ```
    sudo apt install maven -y
    ```
- Clone Sample project (e.g., Petclinic):
    ```
    git clone https://github.com/spring-projects/spring-petclinic.git
    cd spring-petclinic
    ```
- Configure pom.xml for Nexus:
    ```
    <distributionManagement>
        <repository>
            <id>nexus</id>
            <url>http://<nexus-public-ip>:8081/repository/sample/</url>
        </repository>
    </distributionManagement>
    ```

- Settings.xml for credentials:
    ```
    <settings>
      <servers>
        <server>
          <id>nexus</id>
          <username>admin</username>
          <password>your-nexus-password</password>
        </server>
      </servers>
    </settings>
    ```

- Build and deploy:
    ```
    mvn clean package
    mvn deploy
    ```

- Install Tomcat:
    ```
    sudo apt install tomcat10 -y
    sudo systemctl status tomcat10
    ```

- Deploy .war:
    ```
    sudo cp ./target/petclinic.war /var/lib/tomcat10/webapps
    sudo systemctl restart tomcat10
    ```

- Access web app at:
    `http://<public-ip>:8080/petclinic`

**SonarQube & Nexus Servers**
- Same as in previous files; install, configure, set up tokens, links; ports 9000/8081.

**PORT SUMMARY**

| Service   | Port | Notes                |
|-----------|------|----------------------|
| SSH       | 22   | VM remote access     |
| HTTP      | 80   | Standard web server  |
| Tomcat    | 8080 | Java web server      |
| SonarQube | 9000 | Code analysis        |
| Nexus     | 8081 | Artifact repository  |

---

**Pipeline:**

1. Dev writes code → pushes to Git
2. Maven builds (mvn package)
3. SonarQube checks code (mvn sonar:sonar)
4. Nexus receives artifact (mvn deploy)
5. Tomcat runs the app (.war file in webapps)
