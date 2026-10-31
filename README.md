# CI/CD Lab — Express, TypeScript & GitHub Actions

A complete CI/CD project demonstrating automated testing, continuous integration, branch protection, and continuous deployment using GitHub Actions and Render.

## Project Information

- **GitHub Repository:** https://github.com/firaskhdhir11-lab/cicd-lab
- **Production URL:** https://cicd-lab-ctve.onrender.com
- **Runtime:** Node.js 24
- **Language:** TypeScript
- **Backend:** Express 5
- **Deployment:** Render
- **CI:** GitHub Actions

## Features

- Express application with TypeScript.
- Health check API.
- Version API.
- Automated tests using Node.js Test Runner.
- TypeScript validation.
- Automated build.
- Combined local verification command.
- GitHub Actions CI workflow.
- Protected main branch.
- Pull Request-based workflow.
- Render automatic deployment after successful CI checks.
- Automatic Render Pull Request Previews.

## Application Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Application homepage |
| GET | `/api/health` | Returns application health |
| GET | `/api/version` | Returns the application version |

### Health endpoint

```json
{
  "status": "ok"
}

cd ~/cicd-lab

cat > README.md <<'EOF'
# CI/CD Lab — Express, TypeScript & GitHub Actions

A complete CI/CD project demonstrating automated testing, continuous integration, branch protection, and continuous deployment using GitHub Actions and Render.

## Project Information

| Item | Value |
|---|---|
| Project | CI/CD Lab |
| Author | Firas Khdhir |
| GitHub Repository | https://github.com/firaskhdhir11-lab/cicd-lab |
| Production URL | https://cicd-lab-ctve.onrender.com |
| Runtime | Node.js 24 |
| Language | TypeScript |
| Backend | Express.js |
| CI/CD | GitHub Actions |
| Deployment Platform | Render |

## 1. Project Overview

The objective of this project is to implement a continuous integration and continuous deployment pipeline for a Node.js application.

The project demonstrates:

- Building an Express application using TypeScript.
- Writing and executing automated tests.
- Validating TypeScript code.
- Automating application builds.
- Configuring GitHub Actions.
- Managing feature branches and Pull Requests.
- Protecting the main branch.
- Deploying the application using Render.
- Deploying only after successful CI checks.
- Creating temporary Preview Deployments for Pull Requests.
- Demonstrating CI failures and their correction.

## 2. Technologies Used

- Node.js 24
- TypeScript
- Express.js
- npm
- Git
- GitHub
- GitHub Actions
- Render
- Linux / Ubuntu

## 3. Application Features

The application provides three HTTP endpoints.

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Application homepage |
| GET | `/api/health` | Application health check |
| GET | `/api/version` | Application version information |

### Health Endpoint

URL:

https://cicd-lab-ctve.onrender.com/api/health

Expected response:

```json
{
  "status": "ok"
}
```

### Version Endpoint

URL:

https://cicd-lab-ctve.onrender.com/api/version

The endpoint returns the value of the `APP_VERSION` environment variable.

When this variable is not configured, the default value is `development`.

Expected default response:

```json
{
  "version": "development"
}
```

## 4. Installation and Local Execution

Clone the repository:

```bash
git clone https://github.com/firaskhdhir11-lab/cicd-lab.git
cd cicd-lab
```

Install dependencies:

```bash
npm ci
```

Start the development server:

```bash
npm run dev
```

Open the application:

- http://localhost:3000
- http://localhost:3000/api/health
- http://localhost:3000/api/version

## 5. Available npm Scripts

| Command | Description |
|---|---|
| `npm run dev` | Start the development server |
| `npm run typecheck` | Check TypeScript without generating output |
| `npm test` | Execute automated tests |
| `npm run build` | Compile TypeScript |
| `npm start` | Start the production server |
| `npm run verify` | Run all verification steps |

### Combined Verification

The `verify` script executes:

1. TypeScript validation.
2. Automated tests.
3. Application build.

Command:

```bash
npm run verify
```

Final local verification results:

- TypeScript: Passed.
- Tests: 2 passed.
- Failed tests: 0.
- Build: Passed.

## 6. GitHub Actions — Continuous Integration

The CI workflow is configured in:

`.github/workflows/ci.yml`

The workflow is triggered by:

- Push events on the `main` branch.
- Pull Requests targeting the `main` branch.

### CI Pipeline Steps

1. Checkout the repository.
2. Set up Node.js 24.
3. Install dependencies using `npm ci`.
4. Execute `npm run verify`.

The required GitHub Actions status check is:

`Verify`

The workflow automatically validates changes before they can be merged into the protected main branch.

## 7. Git Branching Strategy

The project uses a feature-branch workflow.

The main development process is:

1. Create a feature branch.
2. Implement the required changes.
3. Run local verification.
4. Push the feature branch to GitHub.
5. Create a Pull Request targeting `main`.
6. Wait for GitHub Actions checks.
7. Merge only after the required checks pass.

This approach keeps the main branch protected from unverified changes.

## 8. GitHub Branch Protection

A GitHub Ruleset named `Protect main` is configured.

Configuration:

- Enforcement status: Active.
- Target branch: `main`.
- Require a Pull Request before merging.
- Required approvals: 0.
- Required status check: `Verify`.
- Bypass list: Empty.

The ruleset ensures that changes targeting `main` must pass the required CI check before merging.

## 9. Continuous Deployment with Render

The application is deployed using Render Web Services.

Production URL:

https://cicd-lab-ctve.onrender.com

### Render Configuration

| Setting | Value |
|---|---|
| Service Name | cicd-lab |
| Repository | firaskhdhir11-lab/cicd-lab |
| Branch | main |
| Runtime | Node |
| Instance | Free |
| Build Command | `npm ci && npm run build` |
| Start Command | `npm start` |
| Auto-Deploy | After CI Checks Pass |
| Pull Request Previews | Automatic |

Render deploys updates from the main branch after the required CI checks pass.

## 10. Pull Request Preview Deployments

Render Pull Request Previews are configured as:

`Automatic`

For each eligible Pull Request, Render creates a temporary deployment with its own URL.

This allows changes to be tested before merging them into `main`.

A Preview Deployment was successfully tested using Pull Request #4.

Preview URL:

https://cicd-lab-pr-4-6xhl.onrender.com/

The preview displayed:

- CI/CD Lab
- The application is running.
- Version development
- Verified by GitHub Actions. Deployed on Render.
- Preview deployment test.

After the Pull Request was merged, Render destroyed the temporary Preview Deployment.

## 11. CI Failure and Recovery Demonstration

A deliberate CI failure was introduced to demonstrate automated quality control.

### Initial Failure

A test was added for the missing `/api/version` endpoint.

The test expected:

- HTTP status: 200.

However, the endpoint initially returned:

- HTTP status: 404.

The test results showed:

- 1 test passed.
- 1 test failed.

GitHub Actions detected the failure and reported an unsuccessful required status check.

The Pull Request could not be merged while the required check was failing.

### Fix and Successful Verification

The missing `/api/version` route was added to `src/app.ts`.

After the correction, the verification process was executed again.

Final test results:

- GET `/api/health`: Passed.
- GET `/api/version`: Passed.
- Total tests: 2.
- Failed tests: 0.

GitHub Actions subsequently passed, allowing the corrected Pull Request to be merged.

This demonstrates the role of automated testing and branch protection in preventing unverified changes from reaching production.

## 12. Project Structure

```text
cicd-lab/
│
├── .github/
│   └── workflows/
│       └── ci.yml
│
├── captures/
│   ├── cp1.png
│   ├── cp2.png
│   ├── render-auto-deploy.png
│   ├── homepage-updated.png
│   ├── pr-failing-verify.png
│   ├── api-version.png
│   └── render-preview.png
│
├── src/
│   ├── app.ts
│   └── server.ts
│
├── test/
│   └── app.test.ts
│
├── .gitignore
├── package.json
├── package-lock.json
├── tsconfig.json
├── README.md
└── REPORT.md
```

## 13. Screenshots and Evidence

All project screenshots are stored in the `captures/` directory.

The screenshots document:

- Initial application deployment.
- Render service configuration.
- Auto-Deploy after CI checks.
- Updated production homepage.
- Failed GitHub Actions verification.
- Version API response.
- Render Preview Deployment.

These screenshots provide visual evidence of the implementation and testing stages.

## 14. Final Results

The main objectives of the CI/CD project were implemented and tested.

| Component | Result |
|---|---|
| Express Application | Completed |
| TypeScript Validation | Passed |
| Automated Tests | 2 Passed |
| Application Build | Passed |
| GitHub Actions CI | Successful |
| Pull Request Workflow | Tested |
| Main Branch Protection | Active |
| Render Production Deployment | Successful |
| Auto-Deploy After CI Checks | Configured |
| CI Failure Demonstration | Successful |
| CI Failure Correction | Successful |
| Version API | Implemented |
| Render Preview Deployment | Successfully tested |

## 15. Author

**Firas Khdhir**

GitHub:

https://github.com/firaskhdhir11-lab

Repository:

https://github.com/firaskhdhir11-lab/cicd-lab

Production Application:

https://cicd-lab-ctve.onrender.com

---

This project demonstrates a complete practical workflow covering automated verification, collaborative Git practices, protected branches, and continuous deployment.
