# NumberGuessGame

A simple Java-based web application where users guess a number between 1 and 100.  
The game provides feedback if the guess is too high, too low, or correct.

---

## Features

- Random number generation between 1 and 100.
- User input validation.
- Interactive feedback on guesses.
- Reset game automatically when guessed correctly.

---

## Technology Stack

- **Java 17** (OpenJDK / Corretto)
- **Servlets** (Jakarta Servlet API)
- **Apache Tomcat 10** for deployment
- **Maven** for build management
- **Jenkins** for CI/CD automation
- **Git** for version control

---

## Project Structure
NumberGuessGame/
├─ src/
│ ├─ main/
│ │ ├─ java/com/studentapp/NumberGuessServlet.java
│ └─ test/
│ ├─ java/com/studentapp/NumberGuessServletTest.java
├─ pom.xml
├─ Jenkinsfile
└─ README.md

## CI/CD Pipeline

Continuous Integration: Jenkins monitors the repository for changes, builds the project, and runs tests automatically.

Continuous Deployment: On successful build, the WAR file is deployed to Tomcat without manual intervention.

Pipeline Configuration: Defined in Jenkinsfile with Maven build steps and deployment commands
## Authors

hougo

Team of three contributors: Esther Monday

