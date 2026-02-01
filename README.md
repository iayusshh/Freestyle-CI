# CILabProject

Submission-ready CI lab project scaffold using Maven + Jenkins Multibranch Pipeline.

## Project structure
- `src/main/java/com/muj/ci/Calculator.java`
- `src/test/java/com/muj/ci/CalculatorTest.java`
- `pom.xml`
- `Jenkinsfile`
- `docker/Dockerfile`
- `scripts/build.sh`
- `scripts/deploy.sh`

## Run locally
Prereqs: Java (JDK) + Maven.

- Build: `./scripts/build.sh`
- Test: `mvn test`
- Run jar after build: `java -jar target/CILabProject-1.0-SNAPSHOT.jar`

## Jenkins setup (Multibranch Pipeline)
1. Jenkins → **New Item** → **Multibranch Pipeline**
2. **Branch Sources** → Add Source → **GitHub** → add credentials
3. Select this repository
4. Enable **Discover branches**
5. Save (Jenkins auto-detects branches and runs the `Jenkinsfile` per branch)

## Branch strategy
Pipeline behavior by branch name:
- `main`: Build + Test + Deploy
- `feature/*`: Build + Test + Feature Validation
- `release/*`: Build + Test + Release Build

This satisfies “different build strategies for main, feature, and release branches”.

## Docker
Build after producing the jar:
- `docker build -t cilabproject -f docker/Dockerfile .`
