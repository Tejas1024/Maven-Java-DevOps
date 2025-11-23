# Tomcat-Deploy-JaCoCo.md

# Deploy Maven Build to Tomcat & Check Code Coverage with JaCoCo

## Deploying .war File with Tomcat

1. Build project:
mvn clean package

produces .war file in target/
 

2. Install Tomcat:
sudo apt install tomcat10 -y
sudo systemctl status tomcat10

 

3. Copy .war to Tomcat:
sudo cp ./target/petclinic.war /var/lib/tomcat10/webapps
sudo systemctl restart tomcat10

 

4. Access in browser:  
`http://<public-ip>:8080/petclinic`

---

## Check Code Coverage Using JaCoCo

**JaCoCo = Java Code Coverage Tool**

1. Run tests with Jacoco in project root:
mvn test jacoco:report

 

2. Review coverage report:
- Open: `target/site/jacoco/index.html` in your browser

3. Outputs:
- Table/graph showing % of code covered
- `jacoco.exec` binary file for further analysis

**Summary Table**

| Tool    | Purpose            | Command                  | Output                  |
|---------|--------------------|--------------------------|-------------------------|
| Tomcat  | Deploy/run .war    | cp target/file.war ...   | http://ip:8080/app      |
| JaCoCo  | Code coverage      | mvn test jacoco:report   | target/site/jacoco/     |

**Common Ports:**  
8080 (Tomcat), 22 (SSH)

**TL;DR:**  
Maven builds and packages; Tomcat serves the app; JaCoCo checks test coverage.