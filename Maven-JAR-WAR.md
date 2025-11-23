# Maven-JAR-WAR.md

# Maven – Creating .jar and .war Files

## What are .jar and .war?

| File | Full Form           | Used For        | Description                                                  |
|------|---------------------|-----------------|--------------------------------------------------------------|
| .jar | Java ARchive        | Java apps       | Bundles compiled .class files, dependencies – runs with Java |
| .war | Web Application ARC | Web apps        | Web/servlet projects for web servers (Tomcat, etc.)          |

- `.java` → compiled into `.class` → packaged into `.jar` (Java apps)
- `.jsp`/servlet projects → packaged into `.war` (Web apps)

---

## Steps to Create a .jar File Using Maven

### Windows Steps

**Prereqs:**  
- Java 17 JDK installed  
- Maven installed (in PATH)  

**Check:**
java -version
mvn -version

 

**Step 1: Create a Maven Project**
mvn archetype:generate

 
Inputs example:
- sample project: 2249
- version: 9:9
- groupId: google
- artifactId: chrome
- version: 1.0-SNAPSHOT
- package: in

**Step 2: Enter Project Folder**
cd chrome

 

**Step 3: Package the Project**
mvn package

 
- JAR located at: `chrome/target/chrome-1.0-SNAPSHOT.jar`

---

### Linux Steps (e.g. EC2, Ubuntu)

**Step 1: Install Java 17 & Maven**
sudo apt update && sudo apt install openjdk-17-jdk maven -y

 
Check:
java -version
mvn -version

 
**Step 2–4:**  
Repeat steps as Windows.

---

### Recap Table

| Step | Windows        | Linux                         |
|------|---------------|-------------------------------|
| 1    | Install Java/Maven | sudo apt install openjdk-17-jdk maven -y |
| 2    | archetype:generate | archetype:generate         |
| 3    | cd chrome     | cd chrome                     |
| 4    | mvn package   | mvn package                   |

---

### Result

- The JAR is found in the `target/` folder.
- Run with: `java -jar target/chrome-1.0-SNAPSHOT.jar`

---

### Beginner Notes

- `mvn archetype:generate`: Creates project skeleton
- `pom.xml`: Maven's config file for dependencies/etc.
- `target/`: Output folder for builds
- `SNAPSHOT`: Means dev version, not final release