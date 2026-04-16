# DevSecOps — Intermediate
## Training Participant Guide

**State Bank of India — Technology & Digital Banking, Training Division**

> *Integrating Security into Every Stage of Software Delivery*

| | |
|---|---|
| **Format** | 2-Day Instructor-Led Training |
| **Level** | Intermediate — Hands-on, code-first |
| **Duration** | 10:30 AM – 5:30 PM each day |
| **Labs** | 5 hands-on labs including 1 capstone exercise |
| **Audience** | Experienced Developers — SBI |
| **Project** | SBI Employee Management System (EMS) — Java Spring Boot REST API |

---

## About This Guide

This document is your starting point for the SBI DevSecOps Intermediate training programme. It sets out the purpose and intent of the two days, introduces the hands-on project you will use throughout, explains every tool that has been installed on your workstation, and walks you through the complete syllabus. Read it before the first session — everything that follows in the courseware builds on the context established here.

---

# 1. Training Intent & Purpose

The Indian banking sector operates under one of the most demanding technology security frameworks in the world. RBI's Master Direction on IT Governance (2023), CERT-In's vulnerability disclosure mandates, and SEBI's cybersecurity circulars collectively require that financial institutions maintain a demonstrably secure software development lifecycle. The pressure is not theoretical — SBI, as India's largest public sector bank, is a constant target of sophisticated adversaries.

Yet the most common source of security vulnerabilities in banking systems is not sophisticated zero-day exploits — it is ordinary developers writing ordinary code without security awareness: a hardcoded password here, an unvalidated input there, a Docker container running as root, a Terraform file that accidentally opens a database to the internet. These are problems that skilled developers can fix in minutes once they know to look for them.

**This training exists to close that gap.**

> **The Core Intent:** To equip SBI's development teams with the knowledge, tools, and habits to build security into software from the first line of code — not as an afterthought, not as someone else's job, and not as a checkbox for compliance — but as a natural, automated part of how software is built and delivered.

## 1.1 What This Training Is — and Is Not

| What this training IS | What this training is NOT |
|---|---|
| A practical, hands-on programme using real tools on real code | A theoretical security overview or compliance lecture |
| Training built around a Java Spring Boot banking application (EMS) | A generic course not specific to SBI's technology context |
| A programme that teaches you to find AND fix vulnerabilities | A penetration testing or red team course |
| Aligned to RBI IT Framework, CERT-In, and PCI-DSS requirements | A replacement for periodic security audits or pen-tests |
| A foundation for building a DevSecOps culture in your team | A one-time event — the tools and habits must be sustained |

## 1.2 Why Experienced Developers?

DevSecOps training is most valuable when delivered to developers who already understand the technology stack, build pipelines, and deployment patterns. Junior developers need to learn secure coding; senior developers need to learn to automate and enforce it at scale. This programme is pitched accordingly — we spend minimal time on fundamentals and maximum time on tool integration, pipeline automation, and real banking-context examples.

---

# 2. What This Training Will Change in Your Work

By the end of this two-day programme, specific and measurable changes should be visible in how your team builds and ships software.

## In Your IDE — Today

You will have SonarLint installed in VS Code, flagging security issues as you type — before you even save the file. A hardcoded password or an injection-vulnerable query will be underlined in red the moment you write it.

You will know the difference between a Blocker vulnerability (do not commit) and a Code Smell (fix when convenient).

## In Your Code Reviews — This Week

You will have a security lens for every pull request: Is sensitive data being returned to unauthorised callers? Are all controller inputs validated with `@Valid`? Is there a hardcoded credential anywhere in the diff?

You will be able to reject a PR on security grounds with a specific, articulate reason — not just a vague concern.

## In Your CI/CD Pipeline — This Sprint

You will integrate SonarQube, Trivy, and OWASP ZAP into your pipeline so that every build is automatically scanned. A Critical vulnerability will break the build. A clean scan is a prerequisite for deployment.

Secrets — database passwords, JWT keys, API tokens — will never reach version control. A pre-commit hook will catch them first.

## In Your Infrastructure — This Quarter

Every Terraform change will pass through Checkov before it is applied. A misconfigured database (publicly accessible) or an open security group will be caught in code review — not after a security audit finds it in production.

Your Docker images will be built with hardened Dockerfiles: non-root users, minimal base images, and no unnecessary capabilities.

## 2.1 Regulatory Alignment

| Framework | Relevant Requirement | How This Training Addresses It |
|---|---|---|
| RBI Master Direction on IT (2023) | Secure SDLC, vulnerability management, credential management | SAST/DAST in CI/CD, detect-secrets, Vault integration |
| CERT-In Directives | Mandatory vulnerability scanning, incident reporting timelines | Automated scanning on every build; MTTR metrics |
| PCI-DSS v4.0 | Requirement 6: Secure Systems and Software | OWASP Top 10 in code, dependency scanning, container hardening |
| ISO 27001:2022 | A.8.25 Secure development lifecycle | End-to-end pipeline gates covering code, container, IaC |
| OWASP ASVS | Level 2 Application Security Verification | SAST rules mapped to ASVS; ZAP verification of controls |

---

# 3. The Training Project — SBI Employee Management System (EMS)

Every concept, every tool, and every lab exercise in this training is grounded in a single, shared Java Spring Boot application: the **Employee Management System (EMS)**. Rather than working on toy examples or abstract code snippets, you will secure a complete, realistic banking-context REST API that resembles the kind of application SBI developers build and maintain every day.

## 3.1 Why EMS?

| Design Decision | Rationale |
|---|---|
| Universal HR domain | Everyone understands employees, departments, and salaries — no domain ramp-up time |
| PII and sensitive data | Salary, email, phone — natural anchors for access control and data protection lessons |
| Multiple entity relationships | One-to-Many, Many-to-One, Many-to-Many — realistic JPA and security complexity |
| Spring Boot 3.x + Java 17 | Matches SBI's current and target technology stack |
| JWT authentication | Real-world auth pattern; enables meaningful DAST and auth security lessons |
| OpenAPI / Swagger | Enables ZAP to scan the full API automatically without manual configuration |
| Progressive complexity | CRUD first, then security layers, then monitoring — mirrors real project evolution |

## 3.2 EMS at a Glance

| Entity | Purpose | Key Security Considerations |
|---|---|---|
| Employee | Central entity — personal and employment data | Salary is PII; only accessible to self + ADMIN; soft-delete only |
| Department | Organisational grouping of employees | Name uniqueness; cannot delete if employees assigned |
| Role | Job designation with seniority level (1–10) | Level controls access decisions; name must be unique |
| Project | Business initiative; employees assigned via join entity | Status machine enforced at service layer; cascade delete assignments |

## 3.3 Technology Stack

| Layer | Technology |
|---|---|
| Language & Runtime | Java 17 (Eclipse Temurin) |
| Application Framework | Spring Boot 3.x |
| Data Access | Spring Data JPA + Hibernate |
| Database | MySQL 8.x (Docker container) |
| Security | Spring Security 6.x + JWT (JJWT) |
| Validation | Jakarta Bean Validation (Hibernate Validator) |
| API Documentation | SpringDoc OpenAPI 3 (Swagger UI) |
| Observability | Spring Actuator |
| Build Tool | Apache Maven 3.9.x |
| Containerisation | Docker + Docker Compose |

## 3.4 API Overview

EMS exposes a RESTful API at the base path `/api/v1`. All endpoints require JWT authentication. The Swagger UI at `http://localhost:8080/swagger-ui.html` provides interactive API documentation.

| Resource | Base Path | Key Endpoints |
|---|---|---|
| Employee | `/api/v1/employees` | GET all, GET by ID, POST (create), PUT (update), DELETE (terminate) |
| Department | `/api/v1/departments` | Full CRUD; cannot delete if employees assigned |
| Role | `/api/v1/roles` | Full CRUD; level 1 (junior) to 10 (principal) |
| Project | `/api/v1/projects` | CRUD + assign/remove employees; filter by status |
| Authentication | `/api/v1/auth` | POST /login → JWT token; POST /register (ADMIN only) |

## 3.5 How Participants Use EMS During Training

EMS serves a different purpose in each module. The table below maps every training session to how you will interact with the project:

| Module | How EMS is Used | What You Will Do |
|---|---|---|
| Secure Coding | Read EMS source code; identify and fix vulnerabilities | Add `@PreAuthorize`, `@Valid`, fix error handling in controllers/services |
| SAST — Lab 1 | Run SonarQube scan against EMS source | Triage findings; fix 2 Critical issues; re-scan to verify Quality Gate |
| DAST — Lab 2 | Run OWASP ZAP against the live running EMS API | Import OpenAPI spec; active scan; fix security headers; re-scan |
| Secrets Management | Inspect EMS config; detect and remove hardcoded secrets | Add detect-secrets hook; move JWT secret to env variable / Vault |
| Container Security — Lab 3 | Build EMS Docker image; scan with Trivy | Fix insecure Dockerfile; rebuild; confirm CVE count drops |
| IaC Security — Lab 4 | Scan EMS Terraform config with Checkov | Fix public DB, open SG, unencrypted S3; write custom SBI policy |
| Capstone Lab | Full pipeline run on EMS with introduced vulnerability | SAST catches it; DAST confirms it; fix it; all gates pass |

---

# 4. Tools Installed on Your Workstation

All tools listed in this section have been pre-installed and validated on your training VM. You do not need to install anything.

> **Before You Start — Verify Your Setup:** Open VS Code > Terminal (`Ctrl + \``) and run each verification command below. If any command fails, inform your trainer immediately before the session begins. Do not attempt to install tools yourself — use the IT lab team contact provided at your workstation.

## 4.1 Development Environment

### Java 17 LTS (Eclipse Temurin)

The Java Development Kit — the runtime that powers the EMS Spring Boot application. Java 17 is the current Long-Term Support version and is widely used in SBI's banking systems.

```bash
java -version
# Expected: openjdk version 17.x.x
```

### Apache Maven 3.9.x

The build automation tool for EMS. Maven manages all Java dependencies, compiles the project, runs tests, and packages the application as a runnable JAR file. SonarQube analysis is also triggered through Maven.

```bash
mvn -version
# Expected: Apache Maven 3.9.x
```

### Git for Windows

Version control for the EMS project. Git is also where secrets most commonly leak — the pre-commit hooks you will configure in the Secrets Management module sit inside the `.git/hooks/` directory.

```bash
git --version
```

### Visual Studio Code (with Security Extensions)

The primary IDE for the training. The following security extensions are pre-installed:

- **SonarLint** — real-time security and quality issue highlighting as you type
- **HashiCorp Terraform** — syntax highlighting and validation for IaC files
- **Docker** — container management and Dockerfile support within the IDE
- **GitLens** — git history and blame annotations (useful for tracking when secrets were introduced)

```bash
code --version
```

### Docker Desktop (WSL2 backend)

The container runtime used to build and run EMS, the target lab applications (WebGoat, Juice Shop, DVWA), and security tools (SonarQube, HashiCorp Vault). Docker Desktop with the WSL2 backend is required for all container-related labs.

```bash
docker run hello-world
# Expected: "Hello from Docker!" message
```

## 4.2 Security Tools

| Tool | Module | Purpose in Training |
|---|---|---|
| **SonarQube** (shared server) | SAST — Module 2 & Lab 1 | Central SAST server. Deep analysis of EMS Java code; taint analysis; Quality Gate enforcement. Access at `http://SONAR_IP:9000` |
| **Semgrep CLI** | SAST — Module 2 | Lightweight command-line scanner. Run locally against EMS for OWASP Top 10 rules. Also used in CI pipeline demos. `semgrep --version` |
| **OWASP ZAP** | DAST — Module 3 & Lab 2 | Dynamic application scanner. Proxies HTTP traffic to running EMS; performs active scanning; detects missing headers, injection, auth issues. |
| **Burp Suite Community** | DAST — Module 3 | Manual HTTP interception proxy. Used alongside ZAP to demonstrate manual request manipulation and targeted fuzzing of EMS endpoints. |
| **detect-secrets** | Secrets — Module 4 | Python tool that scans code and git history for credential patterns. Installed as a pre-commit hook to block secrets before they enter git. `detect-secrets --version` |
| **HashiCorp Vault** (dev mode) | Secrets — Module 4 | Enterprise secrets management platform. Run in Docker dev mode to demonstrate dynamic secrets and Spring Boot Vault integration. |
| **Trivy** | Container — Module 6 & Lab 3 | Comprehensive container scanner. Scans EMS Docker images for OS CVEs, Java dependency CVEs, and Dockerfile misconfigurations. `trivy --version` |
| **Checkov** | IaC — Module 7 & Lab 4 | Open-source IaC policy scanner. Analyses EMS Terraform against 1,000+ CIS/PCI-DSS rules; supports custom SBI policies. `checkov --version` |
| **Terraform CLI** | IaC — Module 7 | Infrastructure as Code tool. Used to review and modify EMS infrastructure definitions that Checkov scans. `terraform version` |
| **OWASP Dependency-Check** | Secure Coding — Module 1 | Software Composition Analysis (SCA) tool. Scans EMS Maven dependencies for known CVEs. Complements SAST by covering third-party libraries. |

## 4.3 Lab Target Applications

The following intentionally vulnerable applications are used as additional DAST scanning targets alongside EMS:

| Application | Start Command | Use in Training |
|---|---|---|
| **OWASP WebGoat** | `docker run -d -p 8080:8080 webgoat/webgoat` | Java-based vulnerable app. SAST and DAST target for injection, broken auth demos. |
| **OWASP Juice Shop** | `docker run -d -p 3000:3000 bkimminich/juice-shop` | Node.js vulnerable app. ZAP API scanning; demonstrates real DAST findings. |
| **DVWA** | `docker run -d -p 8081:80 vulnerables/web-dvwa` | PHP vulnerable web app. Manual ZAP scanning; adjustable difficulty levels. |

> **Important:** These applications are intentionally insecure and must only be run on your isolated training VM. Never run them on a network-connected machine in a production environment.

---

# 5. Programme Syllabus

The following syllabus has been designed specifically for experienced Java developers at SBI. The sequence is deliberate: we begin with code-level security (where developers have the most immediate control), move outward to testing and pipeline automation, then address infrastructure security — mirroring the natural DevSecOps shift-left progression.

---

## DAY 1 — Application Security: From Code to Pipeline
`10:30 AM – 5:30 PM`

---

### Module 1 | Secure Coding Practices
**10:30 – 11:30  ·  1 hour  ·  Lecture + Live Code Review**

Developers are the last and most important line of defence. This module builds the mental model and coding habits that prevent vulnerabilities from being written in the first place. We work directly through the EMS source code, identifying and fixing real issues.

- The OWASP Top 10 (2021) — each risk explained with a specific EMS Java example
- **A01 Broken Access Control** — salary PII, `@PreAuthorize`, method-level security in Spring
- **A02 Cryptographic Failures** — BCrypt, TLS enforcement, `@JsonIgnore` on sensitive fields
- **A03 Injection** — JPQL string concatenation vs parameterized queries; Spring Data safety
- **A04 Insecure Design** — state machine enforcement; business rule validation at service layer
- **A05 Security Misconfiguration** — Spring Actuator exposure; CORS; error message leakage
- **A06 Vulnerable Components** — OWASP Dependency-Check; CVE monitoring for Spring Boot
- **A07 Auth Failures** — JWT best practices; token expiry; signing key management
- **A08 Integrity Failures** — CI/CD pipeline security; dependency pinning
- **A09 Logging Failures** — audit logging with Spring AOP; RBI logging requirements
- **A10 SSRF** — URL whitelisting; cloud metadata endpoint risks (AWS IMDS)
- Bean Validation deep dive — `@Valid`, `@NotBlank`, `@Size`, `@Pattern`, `@Email` on EMS DTOs
- Secure error handling — `GlobalExceptionHandler`; correlation IDs; no stack trace leakage

---

### Module 2 | Static Application Security Testing (SAST)
**11:30 – 12:15  ·  45 minutes  ·  Concept + Tool Walkthrough**

SAST analyses your source code before the application runs. This module explains how taint analysis works, how to configure SonarQube for a Java Spring Boot project, and how to read and act on the findings.

- How SAST works: AST, CFG, taint analysis, data flow analysis — with Java examples
- SAST vs DAST — what each catches, why both are mandatory in a DevSecOps pipeline
- SonarQube architecture — rules, issues, Quality Gates, Security Hotspots, severity levels
- Configuring SonarQube for EMS — `pom.xml` setup, project key, exclusions, token auth
- Reading a SonarQube report — taint flow diagrams; effort estimates; issue lifecycle
- Semgrep — writing custom rules for SBI-specific patterns
- SonarLint in VS Code — shift-left to the IDE

---

### Lab 1 | SAST Scan + Fix
**12:30 – 1:30  ·  1 hour  ·  Hands-On**

- Build EMS with Maven; run SonarQube scan against the shared server
- Triage the Quality Gate failure; read taint flow for the top Critical issue
- Fix Issue 1: Move hardcoded JWT secret to `@Value` environment variable
- Fix Issue 2: Add `@PreAuthorize` to the salary endpoint; mask PII for non-admin callers
- Re-scan; confirm Quality Gate passes; note remaining findings for discussion

---

### Module 3 | Dynamic Application Security Testing (DAST)
**1:30 – 2:15  ·  45 minutes  ·  Concept + Tool Walkthrough**

Where SAST analyses code without running it, DAST tests the live, running application from the outside — exactly as an attacker would.

- How DAST works — HTTP proxy interception; active scan payloads; passive analysis
- OWASP ZAP — Spider, AJAX Spider, Active Scan, Passive Scan, Fuzzer explained
- Scanning a REST API — why HTML spidering fails; importing the OpenAPI spec from EMS
- ZAP alert taxonomy — High / Medium / Low / Informational; CVSS scoring
- Typical Spring Boot REST findings — missing security headers, CORS, JWT in URL, verbose errors
- Adding security headers in Spring Security 6.x — CSP, HSTS, X-Content-Type-Options
- Burp Suite Community — manual request interception; targeted fuzzing on EMS endpoints

---

### Lab 2 | DAST Scan + Vulnerability Analysis
**3:00 – 4:00  ·  1 hour  ·  Hands-On**

- Start EMS with Docker Compose; verify health at `/actuator/health`
- Import EMS OpenAPI spec into ZAP; authenticate using JWT from `/api/v1/auth/login`
- Run Active Scan; analyse and document High and Medium alerts
- Apply security header fixes from Module 3 to `SecurityConfig.java`
- Rebuild EMS; re-run ZAP scan; confirm Medium header alerts are resolved

---

### Module 4 | Secrets Management
**4:00 – 4:45  ·  45 minutes  ·  Concept + Live Demo**

- The secret sprawl problem — hardcoded in source, in config files, in Docker Compose, in CI scripts
- Real incident pattern — how committed secrets are found within minutes by automated scanners
- `detect-secrets` — scanning the EMS repository; installing a blocking pre-commit hook
- Environment variables — `application.properties` with `${VAR}` references; `.env` with Docker Compose
- HashiCorp Vault in dev mode — KV secrets engine; storing and retrieving EMS credentials
- Spring Boot + Vault integration — Spring Cloud Vault Config; `bootstrap.properties`
- Secret rotation, lease expiry, and audit logs — the enterprise secrets management model

---

### Module 5 | CI/CD Security Integration
**4:45 – 5:15  ·  30 minutes  ·  Concept + Pipeline Walkthrough**

- The three pipeline security gates: pre-commit (secrets), CI (SAST Quality Gate), CD (DAST scan)
- Full GitHub Actions pipeline YAML for EMS — all stages: detect-secrets, Semgrep, SonarQube, Trivy, ZAP
- Configuring the SonarQube Quality Gate — thresholds for Blocker, Critical, coverage, hotspots
- The shift-left principle — fast, specific failure messages vs slow, vague security audits
- Day 1 wrap-up — Q&A; open issues from labs; action items for each participant's own codebase

---

## DAY 2 — Infrastructure Security: Containers, IaC, and the Full Pipeline
`10:30 AM – 5:30 PM`

---

### Module 6 | Container Security
**10:30 – 11:30  ·  1 hour  ·  Lecture + Live Demo**

Containers are the deployment unit for modern banking applications. This module covers the entire container security lifecycle — from writing a hardened Dockerfile to scanning images and enforcing runtime constraints.

- The four container attack surface layers: base image, app dependencies, runtime, orchestration
- Common Dockerfile mistakes — running as root, full JDK in runtime, single-layer builds, no health check
- Multi-stage builds for EMS — build stage (JDK + Maven) vs runtime stage (JRE only); image size comparison
- Non-root user creation — `addgroup` / `adduser` in Alpine; `USER` directive; `chown` on application files
- Distroless images — no shell, no package manager; maximum attack surface reduction
- Trivy deep dive — OS CVEs, Java dependency CVEs, Dockerfile misconfiguration scanning
- Docker runtime security constraints — `no-new-privileges`, `read_only`, `cap_drop: ALL`, resource limits
- Kubernetes security basics — `PodSecurityContext`, RBAC, network policies, restricted `ServiceAccounts`

---

### Lab 3 | Container Scan + Dockerfile Hardening
**11:30 – 12:15  ·  45 minutes  ·  Hands-On**

- Build `ems:insecure` from `Dockerfile.insecure`; run Trivy scan; record finding count
- Identify security problems in `Dockerfile.insecure` by inspection
- Review `Dockerfile.secure` — confirm multi-stage build, non-root user, `HEALTHCHECK`, JRE runtime
- Build `ems:secure`; re-scan with Trivy; compare CVE counts and image sizes
- Run Trivy config scan on `Dockerfile.secure`; note any remaining misconfigurations

---

### Module 7 | Infrastructure as Code Security
**12:30 – 1:30  ·  1 hour  ·  Lecture + Live Code Review**

IaC errors are infrastructure misconfigurations created at the speed of automation. This module covers the most dangerous Terraform misconfiguration patterns and how Checkov catches them before `apply`.

- Why IaC security matters in banking — the Capital One breach; immutable infrastructure principles
- The three critical EMS Terraform misconfigurations — publicly accessible RDS, open S3 bucket, permissive security group
- Secure RDS configuration — `publicly_accessible = false`; `storage_encrypted = true`; `deletion_protection`
- S3 public access block — all four `aws_s3_bucket_public_access_block` settings; SSE-KMS encryption
- Security group least privilege — port-specific ingress; source by SG ID not CIDR; no `0.0.0.0/0` on sensitive ports
- Checkov command reference — scan, compact output, JUnit XML, framework filters, skip directives
- Policy-as-code with Checkov custom policies — writing `CKV_SBI_001` for RBI Multi-AZ requirement
- Terraform security best practices — variable-driven secrets, encrypted remote state, resource tagging

---

### Lab 4 | IaC Security Scan
**1:30 – 2:15  ·  45 minutes  ·  Hands-On**

- Inspect `terraform/ems/main.tf` — identify misconfigurations by code review before scanning
- Run Checkov scan; record all FAILED check IDs and descriptions
- Apply four fixes: public DB off, storage encrypted, S3 blocked, SG restricted
- Re-scan; confirm 0 FAILED checks (or documented suppressions with justification)
- Write `CKV_SBI_001.py` (Multi-AZ check); run with `--external-checks-dir`; fix and re-scan

---

### Module 8 | Integrated DevSecOps Pipeline
**3:00 – 4:00  ·  1 hour  ·  Architecture Review + Pipeline Walkthrough**

- The seven-stage EMS security pipeline — pre-commit through production approval
- Full production-grade GitHub Actions YAML — SAST, IaC scan, build, Trivy, DAST in sequence
- Parallel vs sequential stages — when to gate on SAST vs let container build proceed in parallel
- The DevSecOps maturity model — Levels 1 through 5; where most PSU banks sit today; target state
- Security metrics that matter — MTTR, vulnerability escape rate, Quality Gate pass rate, image age
- Shift-left economics — cost of fixing a vulnerability at each stage of the pipeline

---

### Capstone Lab | End-to-End Pipeline Run
**4:00 – 5:00  ·  1 hour  ·  Hands-On**

The full DevSecOps cycle — introduce a vulnerability, catch it, fix it, verify all gates pass.

1. Introduce a SQL injection vulnerability into EMS (new department search endpoint)
2. Run SonarQube SAST scan — confirm Critical issue detected; read taint flow
3. Start EMS; run ZAP DAST scan — confirm SQL injection alert on `/employees/search`
4. Fix the vulnerability using Spring Data derived query; commit the fix
5. Re-run SAST — confirm Quality Gate passes
6. Rebuild Docker image (`Dockerfile.secure`); re-run Trivy — confirm 0 CRITICAL
7. Run Checkov on Terraform — confirm 0 FAILED
8. Final ZAP scan on fixed EMS — confirm injection alert is gone
9. **Full pipeline clean: SAST ✓  Container ✓  IaC ✓  DAST ✓**

---

### Module 9 | Banking Case Studies + Q&A
**5:00 – 5:30  ·  30 minutes  ·  Discussion**

- **Case Study 1:** SWIFT Bangladesh Bank fraud (2016) — hardcoded credentials; no container isolation
- **Case Study 2:** Spring4Shell CVE-2022-22965 — how Trivy would have blocked vulnerable deployments
- **Case Study 3:** Indian NBFC S3 breach (2023) — how Checkov would have prevented the public bucket
- RBI compliance mapping — every tool from this training mapped to Master Direction on IT sections
- Next steps — immediate actions for your team this week, this sprint, this quarter
- Open Q&A — bring your codebase challenges; the trainer will address specific SBI context questions

---

# 6. Getting the Most from This Training

This is a hands-on programme. The concepts are straightforward — experienced developers grasp them quickly. The value is in the muscle memory built during the labs.

1. **Bring a real code problem** — if you have a security concern or vulnerability you have encountered in your own project, write it down and raise it during the relevant module.
2. **Type every command yourself** — do not copy-paste from your neighbour. The lab exercises are designed to create lasting recall; passive copying does not.
3. **Break things intentionally** — the lab VMs are sandboxes. Run the vulnerable Dockerfile deliberately. Commit the fake password to see the pre-commit hook fire. Understanding failures creates understanding.
4. **Note three action items per day** — at the end of each day, write down three specific things you will change or implement in your own team's development process within the next sprint.
5. **Ask early** — if a concept is unclear, raise it immediately. An unresolved confusion in Module 1 compounds through all subsequent modules.

> **Note on Tools and Licences:** All tools used in this training are either open-source (OWASP ZAP, Semgrep, Checkov, Trivy, detect-secrets) or have community editions that are free for use (SonarQube Community Edition, Burp Suite Community, HashiCorp Vault). There are no licensing barriers to adopting these tools in your own development environment after the training.

> **Note on RBI Alignment:** Every module in this training programme maps directly to requirements in RBI's Master Direction on Information Technology Governance, Risk, Controls and Assurance Practices (2023). The tools you learn here are not optional add-ons — they are mechanisms for demonstrating compliance with specific sections of the Master Direction. The full RBI compliance mapping is in the Day 2 courseware (Module 9) for reference during internal audits and CISO reviews.

---

> *Security is everyone's job.*
> *This training gives you the tools, the techniques, and the confidence to own it.*
>
> **Welcome to DevSecOps.**

---

*Confidential — For Participant Use Only*
*DevSecOps Intermediate · State Bank of India · Technology Training Programme*
