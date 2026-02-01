# CILabProject

CI Lab project using Maven + Jenkins (Freestyle + Multibranch Pipeline) with branch-based build strategies.

## 1) Project overview
This repository demonstrates:
- A simple Java module (`Calculator`) built with Maven
- Unit tests with JUnit 5
- Jenkins Multibranch Pipeline that changes behavior for `main`, `feature/*`, and `release/*`

## 2) Prerequisites
- Java (JDK 11+)
- Maven
- Jenkins (with Git + GitHub integration)

Recommended:
- Git
- Docker (optional; for building the container image)

## Project structure
- `src/main/java/com/muj/ci/Calculator.java`
- `src/test/java/com/muj/ci/CalculatorTest.java`
- `pom.xml`
- `Jenkinsfile`
- `docker/Dockerfile`
- `scripts/build.sh`
- `scripts/deploy.sh`

## 3) How to run locally
From the repo root:

- Build: `./scripts/build.sh`
- Test: `mvn test`
- Run jar after build: `java -jar target/CILabProject-1.0-SNAPSHOT.jar`

## 4) Jenkins setup steps

### A) Global tool configuration
1. Jenkins → Manage Jenkins → Tools
2. Ensure Maven is configured (either system Maven or an auto-installed Maven)
3. If your Jenkinsfile uses a named Maven tool, create it here (example name: `Maven-3`)

### B) Multibranch Pipeline job
1. Dashboard → New Item
2. Name: `MultiBranch-CI`
3. Select: Multibranch Pipeline → OK
4. Branch Sources → Add Source → GitHub
5. Add GitHub credentials (PAT recommended)
6. Select repository
7. Enable: Discover branches
8. Save

Jenkins will scan the repo and create separate jobs per branch automatically.

## 5) Branch strategy explanation
The pipeline runs different stages based on `BRANCH_NAME`:
- `main`: Build + Test + Deploy
- `feature/*`: Build + Test + Feature Validation
- `release/*`: Build + Test + Release Build

This matches the assignment requirement: different strategies for main/feature/release.

## 6) Installation guide (lab manual)

### Step 1 — Install JDK
- macOS: `brew install temurin` (or install a JDK from Adoptium)
- Verify: `java -version`

### Step 2 — Install Git
- macOS: `brew install git`
- Verify: `git --version`

### Step 3 — Install Maven
- macOS: `brew install maven`
- Verify: `mvn -version`

### Step 4 — Install Jenkins
- Option A (recommended): Docker-based Jenkins
	- Install Docker Desktop
	- Run Jenkins container and expose port 8080
- Option B: Native install via package manager

### Step 5 — Configure Jenkins tools
1. Manage Jenkins → Tools
2. Configure JDK and Maven (name the Maven installation `Maven-3` if your Jenkinsfile expects it)

### Step 6 — Add GitHub credentials
1. GitHub → Developer settings → Personal Access Tokens (classic or fine-grained)
2. Jenkins → Manage Jenkins → Credentials → (Global) → Add Credentials
3. Use the PAT as “Secret text” or “Username with password” (token as password)

### Step 7 — Create jobs

Multibranch Pipeline:
1. New Item → Multibranch Pipeline
2. Branch Sources → GitHub → select repo → Discover branches → Save
3. Trigger: “Scan Multibranch Pipeline Now” or wait for periodic scan

Freestyle job (for the demo requirement):
1. New Item → Freestyle project
2. Source Code Management → Git
3. Build Step → Execute shell → `mvn -B -ntp test`
4. Save → Build Now

## 7) Troubleshooting (real issues)

### `mvn: command not found`
Cause: Maven not installed or not on PATH.
Fix: Install Maven (`brew install maven`) and re-open the terminal, then verify with `mvn -version`.

### `master` vs `main` branch mismatch
Symptom: Jenkins/Docs refer to `master` but repo default is `main` (or vice versa).
Fix: Update Jenkinsfile `when { branch 'main' }` to match your default branch, and ensure Jenkins job points to the correct branch.

### GitHub API rate limiting
Symptom: Multibranch scan fails or GitHub branch discovery errors.
Fix:
- Use GitHub credentials (PAT) in Branch Sources
- Reduce scan frequency if repeatedly scanning

### Wrong build executor (Windows batch on macOS/Linux)
Symptom: Using `bat` steps or Windows commands on a Unix agent.
Fix:
- Use `sh` steps on macOS/Linux agents
- If running on Windows agents, replace `sh` with `bat` and use Windows-compatible commands

## 8) Video demo script (5 minutes)

Minute 1 — Jenkins dashboard + tools
- Show Manage Jenkins → Tools (JDK + Maven configured)

Minute 2 — Freestyle job
- Show Freestyle job config (Git SCM + `mvn test`)
- Click Build Now and show console output

Minute 3 — Multibranch job
- Show `MultiBranch-CI`
- Show branch discovery / Scan Multibranch Pipeline Now

Minute 4 — Feature branch build
- Push a small commit to `feature/login` (or create it)
- Show Jenkins automatically triggering feature build

Minute 5 — Test results + artifacts
- Open build → Test Result
- Show archived jar artifacts

## 9) Final check
- Repo structure matches assignment
- Jenkinsfile present
- pom.xml present
- Dockerfile present
- scripts present
- README complete
- Report PDF includes diagrams + references

## Docker
Build after producing the jar:
- `docker build -t cilabproject -f docker/Dockerfile .`
