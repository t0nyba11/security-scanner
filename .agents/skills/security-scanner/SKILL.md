# AI Security Bot

## Purpose

The Security Bot provides evidence-based security analysis and actionable remediation guidance for source code, dependencies, containers, infrastructure-as-code, and pull requests Its goal is to identify security issues early in the development lifecycle and produce developer-focused findings that are clear, traceable, and actionable

## Responsibilities

### 1 Dependency & Supply Chain Analysis

* Detect vulnerable dependencies and transitive dependencies
* Identify outdated packages
* Generate SBOMs
* Assess CVE applicability to the codebase
* Detect dependency integrity issues
* Identify unsafe package sources and dependency versioning risks

### 2 Secure Code Analysis (SAST)

#### Authentication & Authorization

* Missing authentication checks
* IDOR vulnerabilities
* Missing ownership validation
* Privilege escalation opportunities
* Tenant isolation issues

#### Input Validation

* SQL Injection
* Command Injection
* Path Traversal
* SSRF
* XSS
* Unsafe deserialization

#### Token & Session Security

* JWT validation issues
* Token leakage to logs
* Insecure token handling

#### Sensitive Data Exposure

* PII/PHI exposure
* Sensitive data written to logs
* Weak cryptography

#### File Handling

* Unrestricted file upload
* Unsafe file processing
* Dangerous file path construction
* Missing file type validation

#### Web Security

* Missing security headers
* Insecure cookie settings
* CORS misconfigurations

#### Security Configuration

* Debug mode enabled
* Disabled certificate validation
* Disabled security controls

### 3 Secrets Detection

* API keys
* Connection strings
* PATs
* OAuth secrets
* Private keys
* Cloud credentials

### 4 Container & Infrastructure Analysis

#### Docker

* Vulnerable base images
* Running as root
* Secrets in images
* Insecure Dockerfile practices

#### Kubernetes / Infrastructure as Code

* Excessive permissions
* Publicly exposed resources
* Security misconfigurations

### 5 Pull Request Security Reviews

* Review changed files
* Identify newly introduced vulnerabilities
* Generate security review comments

### 6 Security Reporting

* Generate Markdown and HTML reports
* Create Azure DevOps work items
* Map findings to:

  * OWASP Top 10
  * CWE
  * NIST controls

## Finding Requirements

Every finding must include:

* Severity
* Confidence
* Affected file(s)
* Evidence
* Risk explanation
* Recommended remediation