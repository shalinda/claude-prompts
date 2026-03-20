# Claude Code — Complete Configuration Guide

## Table of Contents

1. [Installation & Authentication](#1-installation--authentication)
2. [Global Configuration](#2-global-configuration)
3. [Project 1: Node + Angular + MongoDB](#3-project-1-node--angular--mongodb)
4. [Project 2: Java + Tomcat + Struts2 + MySQL](#4-project-2-java--tomcat--struts2--mysql)
5. [Project 3: React + Node + MongoDB](#5-project-3-react--node--mongodb)
6. [Project 4: React + Spring Boot + MongoDB](#6-project-4-react--spring-boot--mongodb)
7. [Slash Commands](#7-slash-commands)
8. [Setup Checklist](#8-setup-checklist)

---

## 1. Installation & Authentication

```bash
# Install globally
npm install -g @anthropic-ai/claude-code

# Authenticate (uses your Anthropic API key or OAuth)
claude auth login
```

---

## 2. Global Configuration

### File: `~/.claude/CLAUDE.md`

```markdown
# Global Claude Code Instructions

## Identity & Role
You are a senior full-stack architect and code reviewer. Always consider security, performance, scalability, and maintainability.

## General Principles
- Follow SOLID principles and clean code practices
- Prefer composition over inheritance
- Write meaningful commit messages (conventional commits: feat/fix/chore/refactor)
- All code must be production-ready — no TODOs or placeholder logic unless explicitly asked
- Always consider error handling, logging, and graceful degradation
- Respect existing code style and patterns in the project

## Stack-Wide Conventions
- **React projects**: Always use Vite as the build tool (no CRA)
- **Node.js backends**: Existing projects are JavaScript — do NOT convert to TypeScript unless explicitly asked. New/greenfield projects should use TypeScript with strict mode from the start.
- **Node.js backends do NOT use a service layer** — controllers interact with models/utilities directly
- **Angular projects**: TypeScript strict mode on the frontend as standard

## Code Review Standards
- Flag security vulnerabilities (injection, XSS, CSRF, secrets in code)
- Check for proper input validation and sanitization
- Verify error handling completeness
- Look for N+1 queries and inefficient database access patterns
- Ensure proper use of environment variables (never hardcode secrets)
- Check Docker/K8s configs for resource limits, health checks, and security contexts

## Planning Standards
- When planning, always produce a structured markdown plan with:
  1. Problem statement / goal
  2. Current state analysis
  3. Proposed approach with alternatives considered
  4. File-by-file change list
  5. Migration / deployment steps
  6. Risks and rollback strategy

## Deployment Awareness
- All projects may run in Docker or Kubernetes (K3s or managed K8s)
- Always consider 12-factor app principles
- Config via environment variables, never hardcoded
- Ensure Dockerfiles use multi-stage builds where appropriate
- K8s manifests should include resource requests/limits, liveness/readiness probes

## Version Control
- Projects use either GitLab or GitHub — check the project's remote origin
- Respect branch naming conventions (feature/, bugfix/, hotfix/)
- PR/MR descriptions should reference ticket numbers when available

## Testing
- Suggest unit tests for new logic
- Integration tests for API endpoints and DB operations
- Prefer test files colocated with source or in a parallel test/ directory
```

### File: `~/.claude/settings.json`

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Grep",
      "Glob",
      "LS",
      "Cat"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force)"
    ]
  },
  "env": {
    "CLAUDE_CODE_MAX_TOKENS": "8192"
  }
}
```

---

## 3. Project 1: Node + Angular + MongoDB

### File: `<project-root>/CLAUDE.md`

```markdown
# Project: Node + Angular + MongoDB

## Tech Stack
- **Backend**: Node.js (Express or NestJS)
- **Frontend**: Angular 15+ (standalone components preferred)
- **Build**: Existing backend is JavaScript (ES6+). New code/modules should use TypeScript where feasible
- **Database**: MongoDB (Mongoose ODM)
- **VCS**: GitLab (check .git/config for remote)
- **Deployment**: Docker / Kubernetes

## Project Structure

├── backend/          # Node.js API (JavaScript, migrating to TS for new modules)
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/        # Mongoose schemas
│   │   ├── middleware/
│   │   ├── routes/
│   │   └── utils/
│   ├── test/
│   ├── Dockerfile
│   └── package.json
├── frontend/         # Angular app
│   ├── src/app/
│   │   ├── components/
│   │   ├── services/
│   │   ├── models/
│   │   └── guards/
│   ├── Dockerfile
│   └── angular.json
├── docker-compose.yml
├── k8s/
└── .gitlab-ci.yml

## Coding Standards
- **Backend language**: Existing codebase is JavaScript (ES6+ with CommonJS or ESM). Do NOT convert existing JS files to TypeScript unless explicitly asked. New standalone modules or utilities may use TypeScript if the project has TS tooling configured.
- Angular: standalone components, lazy-loaded routes, reactive forms, OnPush change detection
- Backend: async/await throughout, no callback-style code
- Mongoose: define schemas with TypeScript interfaces, use lean() for read queries
- API responses follow: `{ success: boolean, data?: T, error?: string }`

## MongoDB Guidelines
- Always create indexes for query patterns (check with .explain())
- Use aggregation pipelines for complex queries, not multiple finds
- Avoid `$where` and arbitrary JS execution in queries
- Connection string via `MONGO_URI` env var

## Review Checklist (in addition to global)
- [ ] Angular services use HttpClient with proper error handling (catchError)
- [ ] No direct DOM manipulation — use Angular abstractions
- [ ] Mongoose models have validation rules
- [ ] API routes have authentication middleware where needed
- [ ] CORS configured properly for Angular dev server (port 4200)

## Common Commands

# Dev
cd backend && npm run dev
cd frontend && ng serve

# Test
cd backend && npm test
cd frontend && ng test

# Build
docker-compose build
docker-compose up -d
```

---

## 4. Project 2: Java + Tomcat + Struts2 + MySQL

### File: `<project-root>/CLAUDE.md`

```markdown
# Project: Java + Tomcat + Struts2 + MySQL

## Tech Stack
- **Backend**: Java 8/11+ with Apache Struts 2 framework
- **App Server**: Apache Tomcat 9+
- **Database**: MySQL 8+ (JDBC / MyBatis / Hibernate)
- **Build**: Maven or Gradle
- **VCS**: GitHub or GitLab (check .git/config)
- **Deployment**: Docker / Kubernetes

## Project Structure

├── src/
│   ├── main/
│   │   ├── java/com/company/project/
│   │   │   ├── action/          # Struts2 Action classes
│   │   │   ├── model/           # POJOs / entities
│   │   │   ├── service/         # Business logic
│   │   │   ├── dao/             # Data access layer
│   │   │   ├── interceptor/     # Struts2 interceptors
│   │   │   └── util/
│   │   ├── resources/
│   │   │   ├── struts.xml       # Struts configuration
│   │   │   ├── struts.properties
│   │   │   └── db/              # SQL migrations
│   │   └── webapp/
│   │       ├── WEB-INF/
│   │       │   └── web.xml
│   │       └── jsp/             # JSP views
│   └── test/
├── pom.xml (or build.gradle)
├── Dockerfile
├── k8s/
└── .github/workflows/ (or .gitlab-ci.yml)

## Coding Standards
- Follow Java naming conventions strictly (camelCase methods, PascalCase classes)
- Action classes should be thin — delegate to service layer
- Use parameterized queries / PreparedStatement ALWAYS (SQL injection prevention is critical)
- Struts2: prefer annotation-based configuration over XML where possible
- Close all JDBC resources in finally blocks (or use try-with-resources)
- JSP: use JSTL/EL expressions, never scriptlets (<% %>)

## Struts2 Specific
- Keep struts.xml organized by namespace/module
- Use interceptor stacks for cross-cutting concerns (auth, logging, exception handling)
- Validate inputs using Struts2 validation framework or annotations
- Be aware of known Struts2 CVEs — flag any use of OGNL in user-facing input

## MySQL Guidelines
- Always use InnoDB engine
- Migrations managed via Flyway or Liquibase (or raw SQL scripts in db/ folder)
- Index foreign keys and common WHERE clause columns
- Use connection pooling (HikariCP preferred, or Tomcat JDBC pool)
- Connection config via JNDI or environment variables, never in source

## Security (Critical for Struts2)
- Keep Struts2 version updated — this framework has a history of critical RCE vulnerabilities
- Disable dynamic method invocation unless explicitly needed
- Set `struts.devMode=false` in production
- Validate and sanitize all Action inputs
- CSRF tokens on all state-changing forms

## Review Checklist
- [ ] No SQL string concatenation — parameterized queries only
- [ ] Action classes don't contain business logic (delegate to services)
- [ ] Proper exception handling — no swallowed exceptions
- [ ] JSP files use JSTL, no scriptlets
- [ ] web.xml security constraints configured for protected paths
- [ ] Struts2 version is current / patched

## Common Commands

# Build
mvn clean package -DskipTests
mvn clean package

# Run locally
mvn tomcat7:run  # or deploy WAR to local Tomcat

# Test
mvn test

# Docker
docker build -t project-name .
docker run -p 8080:8080 project-name
```

---

## 5. Project 3: React + Node + MongoDB

### File: `<project-root>/CLAUDE.md`

```markdown
# Project: React + Node.js + MongoDB

## Tech Stack
- **Frontend**: React 18+ (Vite), functional components with hooks
- **Backend**: Node.js (Express) — existing code is JavaScript; new projects use TypeScript
- **Database**: MongoDB (Mongoose ODM)
- **VCS**: GitHub or GitLab (check .git/config)
- **Deployment**: Docker / Kubernetes

## Project Structure

├── client/              # React frontend (Vite)
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/       # Custom hooks
│   │   ├── services/    # API client layer
│   │   ├── context/     # React context providers
│   │   ├── types/
│   │   └── utils/
│   ├── Dockerfile
│   └── vite.config.ts
├── server/              # Node.js API (JS for existing, TS for new projects)
│   ├── src/
│   │   ├── controllers/
│   │   ├── models/
│   │   ├── middleware/
│   │   ├── routes/
│   │   └── utils/
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml
├── k8s/
└── .github/workflows/ (or .gitlab-ci.yml)

## Coding Standards
- React: functional components only, hooks for state/effects, no class components
- Prefer custom hooks to extract reusable logic
- Frontend always uses TypeScript (strict mode) with Vite
- **Backend language policy**:
  - Existing projects: JavaScript (ES6+, CommonJS or ESM). Do NOT convert existing JS to TS unless asked.
  - New projects: TypeScript with strict mode from day one
- State management: React Context + useReducer for app state, or Zustand if installed
- API layer: centralized axios/fetch wrapper in services/ with interceptors for auth
- No service layer in Node backend — controllers call models/utilities directly

## React Specific
- Use React.memo, useMemo, useCallback intentionally (not everywhere)
- Forms: controlled components with proper validation
- Routing: React Router v6+ with lazy-loaded routes
- Error boundaries for graceful UI error handling
- No inline styles — use CSS Modules or SCSS

## MongoDB Guidelines
- Same as Project 1 — Mongoose with TypeScript interfaces, lean(), proper indexes
- Connection via MONGO_URI env var

## Review Checklist
- [ ] No prop drilling beyond 2 levels — use context or composition
- [ ] useEffect has proper dependency arrays and cleanup functions
- [ ] API calls have loading/error/success states handled
- [ ] No secrets in client-side code
- [ ] Vite proxy configured for dev API calls
- [ ] Docker builds use multi-stage (build → nginx for client)

## Common Commands

# Dev
cd server && npm run dev
cd client && npm run dev

# Test
cd server && npm test
cd client && npm test

# Build & Deploy
docker-compose build
docker-compose up -d
```

---

## 6. Project 4: React + Spring Boot + MongoDB

### File: `<project-root>/CLAUDE.md`

```markdown
# Project: React + Spring Boot + MongoDB

## Tech Stack
- **Frontend**: React 18+ (Vite, TypeScript strict mode), functional components with hooks
- **Backend**: Java 17+ / Spring Boot 3.x
- **Database**: MongoDB (Spring Data MongoDB)
- **Build**: Maven or Gradle
- **VCS**: GitHub or GitLab (check .git/config)
- **Deployment**: Docker / Kubernetes

## Project Structure

├── frontend/                # React app (same patterns as Project 3 client)
│   ├── src/
│   ├── Dockerfile
│   └── vite.config.ts
├── backend/                 # Spring Boot API
│   ├── src/main/java/com/company/project/
│   │   ├── config/          # Spring configuration classes
│   │   ├── controller/      # REST controllers
│   │   ├── service/         # Business logic
│   │   ├── repository/      # Spring Data MongoDB repositories
│   │   ├── model/           # Document POJOs / DTOs
│   │   ├── exception/       # Custom exceptions + global handler
│   │   ├── security/        # Security config, filters
│   │   └── util/
│   ├── src/main/resources/
│   │   ├── application.yml
│   │   └── application-{profile}.yml
│   ├── src/test/
│   ├── Dockerfile
│   └── pom.xml
├── docker-compose.yml
├── k8s/
└── .github/workflows/ (or .gitlab-ci.yml)

## Coding Standards
- Spring Boot: use constructor injection (not @Autowired on fields)
- Controllers are thin — validate input, call service, return response
- Use DTOs for API request/response, never expose Document entities directly
- Response wrapper: `ApiResponse<T>` with status, data, message fields
- Use Spring Profiles for environment-specific config (dev, staging, prod)
- Frontend: same React standards as Project 3

## Spring Boot + MongoDB Specific
- Use Spring Data MongoDB repositories (MongoRepository<T, ID>)
- Define indexes with @Indexed and @CompoundIndex annotations
- Use MongoTemplate for complex aggregations
- Config via application.yml: `spring.data.mongodb.uri` from env var
- Enable auditing (@EnableMongoAuditing) for createdDate/lastModifiedDate

## Security
- Spring Security with JWT or OAuth2 for API authentication
- CORS configured in SecurityConfig (not in controllers)
- Validate all inputs with Jakarta Bean Validation (@Valid, @NotNull, etc.)
- Actuator endpoints secured / restricted to internal network

## Review Checklist
- [ ] Controllers use @Valid on request bodies
- [ ] Service methods are @Transactional where needed
- [ ] No business logic in controllers
- [ ] Repository methods use proper query derivation or @Query annotations
- [ ] application.yml has no hardcoded secrets (use env vars or Spring Cloud Config)
- [ ] Dockerfile uses multi-stage build (Maven build → JRE runtime)
- [ ] Tests use @SpringBootTest or @WebMvcTest as appropriate
- [ ] Frontend API calls go through a centralized service layer

## Common Commands

# Backend
cd backend && mvn spring-boot:run
cd backend && mvn test

# Frontend
cd frontend && npm run dev
cd frontend && npm test

# Build & Deploy
docker-compose build
docker-compose up -d
```

---

## 7. Slash Commands

Place these in `~/.claude/commands/` for global access, or `<project-root>/.claude/commands/` for project-specific.

### File: `plan.md`

**Usage:** `/plan <description of feature or change>`

```markdown
Analyze the current project structure and create a detailed implementation plan for: $ARGUMENTS

Follow this structure:
1. **Goal**: What we're trying to achieve
2. **Current State**: Relevant existing code, schemas, and patterns (read the codebase first)
3. **Proposed Approach**: Step-by-step plan with file paths
4. **Alternatives Considered**: Why this approach over others
5. **File Changes**: List every file to create/modify with a summary of changes
6. **Database Changes**: Schema migrations or new indexes needed
7. **API Changes**: New/modified endpoints with request/response shapes
8. **Deployment Impact**: Docker/K8s config changes, env vars, migrations to run
9. **Testing Plan**: What tests to write
10. **Risks & Rollback**: What could go wrong and how to recover

Read the project's CLAUDE.md and relevant source files before generating the plan.
```

### File: `review.md`

**Usage:** `/review` (reviews staged/changed files)

```markdown
Perform a thorough code review of all staged or recently changed files in this project.

For each file, check:
- **Correctness**: Logic errors, edge cases, off-by-one errors
- **Security**: Injection risks, auth bypasses, secrets exposure, XSS/CSRF
- **Performance**: N+1 queries, missing indexes, unnecessary re-renders, memory leaks
- **Style**: Consistency with project conventions (read CLAUDE.md)
- **Error Handling**: Uncaught exceptions, missing try/catch, silent failures
- **Testing**: Are changes covered by tests? Suggest missing test cases
- **Docker/K8s**: If infra files changed, check resource limits, health probes, image tags

Output format:
### 🔴 Critical (must fix)
### 🟡 Warning (should fix)
### 🟢 Suggestions (nice to have)
### ✅ What looks good

Use `git diff --staged` or `git diff` to identify changed files.
```

### File: `review-mr.md`

**Usage:** `/review-mr <branch-name>`

```markdown
Review all changes in the merge/pull request branch compared to main/master.

Run: `git diff main...$ARGUMENTS -- . ':!package-lock.json' ':!*.lock'`

Provide a comprehensive review following the same checklist as /review,
but also comment on:
- Commit history quality (are commits atomic and well-described?)
- Whether the MR is appropriately scoped (too large? should be split?)
- Migration safety (can it be deployed without downtime?)
```

### File: `plan-docker.md`

**Usage:** `/plan-docker`

```markdown
Analyze this project and generate or improve its Docker setup:

1. Read the project structure, dependencies, and build system
2. Generate/review Dockerfile(s) with:
   - Multi-stage builds
   - Non-root user
   - Proper .dockerignore
   - Layer caching optimization
   - Health check instructions
3. Generate/review docker-compose.yml with:
   - All services (app, db, cache, etc.)
   - Volume mounts for data persistence
   - Network configuration
   - Environment variable management
   - Resource limits
4. If K8s manifests exist in k8s/, review those too:
   - Deployments with resource requests/limits
   - Services and Ingress
   - ConfigMaps and Secrets
   - Liveness and readiness probes
   - HPA if applicable
```

### File: `plan-ci.md`

**Usage:** `/plan-ci`

```markdown
Analyze this project and generate or improve its CI/CD pipeline.

Check .git/config to determine if this is GitHub (.github/workflows/) or GitLab (.gitlab-ci.yml).

Generate a pipeline that includes:
1. **Lint & Format Check**
2. **Unit Tests**
3. **Build** (Docker image)
4. **Security Scan** (dependency audit, image scan)
5. **Deploy to Staging** (manual trigger or auto on develop branch)
6. **Deploy to Production** (manual approval gate)

Use project-appropriate tooling (npm/maven/gradle, jest/junit, etc.)
```

---

## 8. Setup Checklist

Run these commands to set everything up:

```bash
# Step 1: Create global config directories
mkdir -p ~/.claude/commands

# Step 2: Place global files
#   ~/.claude/CLAUDE.md          (from Section 2)
#   ~/.claude/settings.json      (from Section 2)

# Step 3: Place global slash commands
#   ~/.claude/commands/plan.md
#   ~/.claude/commands/review.md
#   ~/.claude/commands/review-mr.md
#   ~/.claude/commands/plan-docker.md
#   ~/.claude/commands/plan-ci.md

# Step 4: Set up each project
for project_dir in project-1 project-2 project-3 project-4; do
  cd /path/to/$project_dir
  mkdir -p .claude/commands
  # Copy the corresponding CLAUDE.md from Sections 3-6 into ./CLAUDE.md
done

# Step 5: Commit configs to each repo
cd /path/to/each-project
git add CLAUDE.md .claude/
git commit -m "chore: add Claude Code configuration"

# Step 6: Start using Claude Code
cd /path/to/any-project
claude

# Available commands:
#   /plan Add user authentication with JWT
#   /review
#   /review-mr feature/new-dashboard
#   /plan-docker
#   /plan-ci
```

---

## How It All Works Together

The **global `~/.claude/CLAUDE.md`** sets universal standards — security, review rigor, planning format, Docker/K8s awareness, the Vite-for-React rule, the JS-existing/TS-new policy for Node backends, and the no-service-layer convention. Each **project-local `CLAUDE.md`** layers on stack-specific rules. Claude Code merges them automatically with local taking precedence on conflicts.

The **slash commands** give repeatable workflows: `/plan` for feature planning, `/review` for pre-commit checks, `/review-mr` for branch reviews, and `/plan-docker` and `/plan-ci` for infrastructure work. Since projects span GitLab and GitHub, the commands auto-detect which VCS to use from the git remote.

For **Python AI scripts**, add a section to whichever project repo houses them, or create a standalone `CLAUDE.md` with Python/pip/venv conventions following the same pattern.
