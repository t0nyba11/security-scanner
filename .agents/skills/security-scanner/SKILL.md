---
name: security-scanner
description: Use when performing evidence-based security analysis of source code, dependencies, containers, infrastructure-as-code, secrets, or pull requests, and when producing developer-focused remediation guidance with severity, confidence, affected files, evidence, risk explanation, and recommended fixes.
---

## Operating Principle

Treat automated scanner output, dependency advisories, and pattern matches as leads until they are connected to repository-specific evidence.

## Workflow

1. Scope the target: identify whether the request is for source code, dependencies, containers, infrastructure-as-code, secrets, a pull request, or a report.
2. Inventory the project: identify languages, frameworks, package managers, lockfiles, build files, deployment files, authentication boundaries, data stores, and exposed entry points.
3. Gather evidence from the repository first. Use fast search (`rg`) for discovery, then inspect the relevant code/configuration paths directly.
4. Run relevant local tools when available and appropriate, such as package audit tools, SAST linters, secret scanners, SBOM generators, container scanners, or IaC scanners. Do not require a tool when manual review is more appropriate.
5. Validate findings manually. Confirm the vulnerable source, sink, missing control, misconfiguration, dependency applicability, or reachable code path before reporting a firm issue.
6. Report confirmed findings separately from potential issues and coverage gaps.

## Review Areas

Load only the reference files needed for the requested scope:

* Source code and secrets review: see `references/source-review.md`
* Dependency and supply chain review: see `references/dependencies.md`
* Container, Kubernetes, and infrastructure-as-code review: see `references/containers-iac.md`

## Pull Request Reviews

* Review changed files
* Identify newly introduced vulnerabilities
* Generate security review comments
* Focus on new or worsened risk introduced by the pull request
* Separate pre-existing issues from pull request regressions
* Prefer inline comments only for localized, actionable issues
* Avoid blocking comments for speculative or low-confidence concerns

## Security Reporting

* Generate Markdown and HTML reports
* Create Azure DevOps work items
* Map findings to:

  * OWASP Top 10
  * CWE
  * NIST controls

Default report order:

1. Executive summary
2. Confirmed findings, highest severity first
3. Potential issues or items needing confirmation
4. Coverage: reviewed paths, tools/searches run, and important areas not reviewed
5. Recommended next steps

If no confirmed findings are found, say so directly and still include the Coverage section.

## Evidence Standard

For each confirmed finding, establish:

* Source: attacker-controlled input, untrusted dependency, exposed configuration, credential, or privileged identity
* Sink: sensitive operation, trust boundary, parser, database query, filesystem access, network call, auth decision, deployment permission, or secret exposure
* Missing or weak control: validation, authorization, isolation, sanitization, escaping, encryption, pinning, permission boundary, or runtime hardening
* Exploitability: realistic conditions required to exploit the issue
* Impact: concrete security consequence for confidentiality, integrity, availability, tenancy, compliance, or supply chain trust

Only create a confirmed finding when there is enough evidence for an owner to fix it without first redoing the investigation. Do not report a firm finding when evidence only shows a theoretical pattern with no reachable path. Put uncertain but useful observations under "Potential issues" or "Coverage gaps" with the uncertainty stated plainly.

## Severity

Use severity based on realistic impact and exploitability in this codebase:

* Critical: likely remote compromise, credential theft, tenant breakout, destructive write access, or full supply chain compromise with little user interaction
* High: practical unauthorized access, privilege escalation, sensitive data exposure, command execution, SSRF to sensitive networks, or exploitable injection
* Medium: meaningful security weakness requiring specific conditions, partial exposure, defense bypass, or misconfiguration with limited blast radius
* Low: hardening gap, limited information exposure, low-impact misconfiguration, or issue requiring unlikely preconditions
* Info: non-vulnerable observation, inventory item, or best-practice recommendation

## Confidence

Use confidence to describe evidence quality:

* High: confirmed vulnerable code path, concrete affected version and applicability, reproducible scanner evidence, or direct configuration exposure
* Medium: likely issue with strong indicators but incomplete runtime, deployment, or usage context
* Low: plausible concern that needs owner confirmation or additional evidence

Prefer not to list low-confidence items as findings unless the user requested broad triage. When included, label them as potential issues.

## False-Positive Discipline

Do not report:

* Vulnerable dependencies that are not present in a lockfile, installed manifest, image, or SBOM
* Dependency CVEs without considering whether the vulnerable feature or version range applies
* Test fixtures, examples, mocks, or documentation snippets as production vulnerabilities unless they are shipped or deployed
* Secrets-looking strings without checking surrounding context
* Missing web controls on non-web services
* Theoretical injection, traversal, XSS, SSRF, or auth issues without a credible source-to-sink path
* Generic best practices as vulnerabilities unless they create a concrete risk in context

## Finding Format

Use this structure for Markdown reports:

```markdown
## Finding: <short, specific title>

Severity: High
Confidence: Medium
Category: CWE-89 / OWASP A03 Injection

Affected files:
- path/to/file.ext:42

Evidence:
<specific code, configuration, dependency, or scanner evidence>

Why this matters:
<realistic exploit scenario and impact>

Recommended remediation:
<concrete fix, preferably tailored to the affected code or configuration>

Validation:
<how to verify the fix or prove the issue is not exploitable>
```

## Remediation Guidance

Prefer concrete, developer-ready remediation over generic advice:

* Keep remediation scoped to the vulnerable code, dependency, or configuration. Do not propose broad rewrites, new security platforms, or unrelated hardening unless the finding cannot be fixed safely without them.
* Show parameterized query, escaping, validation, authorization, or policy examples where useful
* Recommend least-privilege permissions with specific scope reductions
* Recommend dependency upgrades with version ranges and compatibility notes when known
* Recommend secret rotation and history cleanup only when exposure is credible
* Recommend image, Dockerfile, Kubernetes, and IaC changes that fit the existing deployment model
* Include tests, checks, or commands that would catch regressions
