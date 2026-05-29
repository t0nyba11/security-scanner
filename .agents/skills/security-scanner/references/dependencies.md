# Dependency Review

Use this reference when reviewing package manifests, lockfiles, SBOMs, build systems, or supply chain risk.

## Focus Areas

* Vulnerable direct and transitive dependencies
* Outdated packages with security-relevant fixes
* SBOM generation and dependency inventory
* CVE applicability to the codebase
* Dependency integrity issues, lockfile drift, missing hashes, or unpinned versions
* Unsafe package sources, install scripts, typosquatting, dependency confusion, or broad version ranges

## Review Notes

Inspect lockfiles before making version-specific claims. For each advisory, assess whether the vulnerable package, version range, feature, endpoint, or code path is actually used. Treat unused or applicability-unknown vulnerabilities separately from confirmed exploitable dependency risk.

For each ecosystem, identify the authoritative dependency source first: lockfile, resolved dependency tree, SBOM, or built image. Prefer resolved versions over manifest constraints when assessing vulnerability applicability.

## Language Ecosystems

### C# / .NET

Review:

* `*.csproj`, `*.fsproj`, `*.vbproj`, `Directory.Packages.props`, `packages.config`, `packages.lock.json`, `global.json`, `NuGet.config`
* Direct and transitive NuGet packages
* Central package management, floating versions, private feeds, package source mapping, and restore lock mode
* SDK/runtime versions and container base images for deployed .NET apps

Useful tools when available:

* `dotnet list package --vulnerable --include-transitive`
* `dotnet list package --outdated`
* `dotnet list package --include-transitive`
* `dotnet restore --locked-mode`
* `osv-scanner`

### Docker

Review:

* `Dockerfile`, `docker-compose.yml`, image tags, base image digests
* OS package installs: `apt`, `apk`, `yum`, `dnf`, `pip`, `npm`, `mvn`, `gradle`
* Multi-stage builds and whether vulnerable build-time dependencies ship in the final image

Useful tools when available:

* `trivy image`
* `trivy fs`
* `grype`
* `syft`
* `docker scout`

### Java

Review:

* `pom.xml`, `mvnw`, `gradle.lockfile`, `build.gradle`, `build.gradle.kts`, `settings.gradle`
* Maven/Gradle dependency trees and lockfiles where present
* Parent BOMs, plugin versions, repository sources, and transitive dependencies

Useful tools when available:

* `mvn dependency:tree`
* `mvn org.owasp:dependency-check-maven:check`
* `gradle dependencies`
* `gradle dependencyInsight`
* `osv-scanner`

### JavaScript / TypeScript

Review:

* `package.json`, `package-lock.json`, `npm-shrinkwrap.json`, `yarn.lock`, `pnpm-lock.yaml`
* `dependencies` vs `devDependencies`
* Install scripts, package manager config, registry overrides, resolutions/overrides

Useful tools when available:

* `npm audit`
* `npm ls`
* `yarn npm audit`
* `pnpm audit`
* `pnpm list`
* `osv-scanner`

### Python

Review:

* `requirements.txt`, `requirements-*.txt`, `pyproject.toml`, `poetry.lock`, `Pipfile.lock`, `uv.lock`, `setup.py`, `setup.cfg`
* Runtime vs dev/test dependency groups
* Unpinned or broadly pinned packages

Useful tools when available:

* `pip-audit`
* `safety`
* `poetry show --tree`
* `uv pip tree`
* `osv-scanner`
