# Tooling By Phase

Rekommenderade verktyg och tekniker för varje fas i utvecklingslivscykeln.

## Översikt

| Fas | Primära verktyg |
|-----|-----------------|
| Krav | Azure DevOps, Figma |
| Design | Mermaid, OpenAPI, draw.io |
| Implementation | Claude Code, VS Code, Rider |
| Test | xUnit, Playwright, Stryker |
| Deploy | Azure DevOps Pipelines, GitHub Actions |
| Produktion | Application Insights, Sentry |

---

## Fas 1: Kravinfångst

### Backlog & Planning

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Azure DevOps Boards](https://azure.microsoft.com/en-us/products/devops/boards)** | Work items, sprints, backlog | Vår standard - integrerat med repos & pipelines |
| [GitHub Issues](https://github.com/features/issues) | Enkel issue tracking | För open source / mindre projekt |
| [Linear](https://linear.app/) | Issues, sprints | Alternativ med bra UX |

### Dokumentation

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Azure DevOps Wiki](https://learn.microsoft.com/en-us/azure/devops/project/wiki/)** | Projektdokumentation | Integrerat med DevOps |
| Markdown i repo | Teknisk dokumentation | Versionshanterad |
| [Notion](https://notion.so/) | Intern wiki, knowledge base | Flexibelt för ej-tekniskt |

### Design & Prototyping

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Figma](https://figma.com/)** | UI/UX design, prototyper | Branschstandard |
| [Excalidraw](https://excalidraw.com/) | Snabba skisser | Gratis, enkelt |
| [Whimsical](https://whimsical.com/) | Flowcharts, wireframes | Snyggt och enkelt |

### AI-assistans

```
User: Strukturera dessa mötesanteckningar till user stories med
      acceptanskriterier i Gherkin-format (Given/When/Then).
```

---

## Fas 2: Teknisk Design & Arkitektur

### Diagramverktyg

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Mermaid](https://mermaid.js.org/)** | Diagram-as-code | Versionshanteras, Claude kan generera |
| [draw.io](https://draw.io/) | Visuella diagram | Gratis, flexibelt |
| [PlantUML](https://plantuml.com/) | UML diagram-as-code | Bra för sekvensdiagram |
| [Eraser.io](https://eraser.io/) | Arkitekturdiagram | AI-assisterad |

### API Design

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[OpenAPI/Swagger](https://swagger.io/)** | API-specifikation | Branschstandard |
| [Stoplight](https://stoplight.io/) | API design GUI | Visuell OpenAPI-editor |
| [Postman](https://postman.com/) | API testing & docs | Bra för manuell testning |

### Architecture Decision Records (ADR)

```markdown
# ADR-001: Val av autentiseringslösning

## Status
Accepted

## Context
Vi behöver autentisering för API:et.

## Decision
JWT med refresh tokens, lagrade i HttpOnly cookies.

## Consequences
- Stateless backend
- Kräver token rotation logic
```

**Verktyg:** Markdown-filer i `docs/adr/`

### AI-assistans

```
User: Generera ett Mermaid sequence diagram för login-flödet
      med dessa steg: [beskrivning]
```

```
User: Skapa OpenAPI spec för user registration endpoint.
      Request: email, password, name
      Response: user object med id, email, name, createdAt
      Errors: 400 (validation), 409 (duplicate email)
```

---

## Fas 3: Implementation

### IDE & Editor

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[VS Code](https://code.visualstudio.com/)** | Primär editor | Claude Code integration |
| [JetBrains Rider](https://www.jetbrains.com/rider/) | .NET IDE | Kraftfull refaktorering |
| [Cursor](https://cursor.sh/) | AI-first editor | Alternativ till VS Code |

### AI Coding

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Claude Code](https://github.com/anthropics/claude-code)** | Agentisk utveckling | Vår standard |
| [GitHub Copilot](https://github.com/features/copilot) | Autocomplete | Komplement |
| [Lovable](https://lovable.dev/)| Frontend prototyping | Snabb UI-utveckling |

### Version Control

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Azure Repos](https://azure.microsoft.com/en-us/products/devops/repos)** | Git hosting | För kundprojekt i Azure DevOps |
| **Git + GitHub** | Git hosting | För interna/open source projekt |
| [Conventional Commits](https://www.conventionalcommits.org/) | Commit-format | `feat:`, `fix:`, `docs:` |
| [Husky](https://typicode.github.io/husky/) | Git hooks | Pre-commit validering |

### Kodkvalitet

| Verktyg | Stack | Användning |
|---------|-------|------------|
| **ESLint** | JS/TS | Linting |
| **Prettier** | JS/TS | Formatering |
| **dotnet format** | .NET | Formatering |
| [SonarQube](https://www.sonarqube.org/) | Multi | Kodanalys |

### AI-assistans

```
User: /tdd Implementera UserService.Register med:
      - Email validering
      - Password hashing med BCrypt
      - Duplicate check

      Använd samma patterns som CustomerService.
```

---

## Fas 4: Test & QA

### Unit Testing

| Verktyg | Stack | Kommentar |
|---------|-------|-----------|
| **[xUnit](https://xunit.net/)** | .NET | Vår standard |
| **[FluentAssertions](https://fluentassertions.com/)** | .NET | Läsbara assertions |
| [NSubstitute](https://nsubstitute.github.io/) | .NET | Mocking |
| **[Vitest](https://vitest.dev/)** | JS/TS | Snabbt, Vite-native |
| [Jest](https://jestjs.io/) | JS/TS | Alternativ |

### Property-Based Testing

| Verktyg | Stack | Kommentar |
|---------|-------|-----------|
| **[FsCheck](https://fscheck.github.io/FsCheck/)** | .NET | Genererar testfall |
| **[fast-check](https://github.com/dubzzz/fast-check)** | JS/TS | Samma koncept |

### Mutation Testing

| Verktyg | Stack | Kommentar |
|---------|-------|-----------|
| **[Stryker.NET](https://stryker-mutator.io/docs/stryker-net/introduction/)** | .NET | Validerar test-kvalitet |
| **[Stryker](https://stryker-mutator.io/)** | JS/TS | Samma koncept |

### E2E Testing

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Playwright](https://playwright.dev/)** | Browser testing | Vår standard |
| [Cypress](https://www.cypress.io/) | Browser testing | Alternativ |

### API Testing

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client)** | VS Code extension | `.http` filer |
| [Postman](https://postman.com/) | GUI API testing | Collections |
| [Pact](https://pact.io/) | Contract testing | Consumer-driven |

### Security Scanning

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Semgrep](https://semgrep.dev/)** | SAST | Regelbaserad scanning |
| [Snyk](https://snyk.io/) | Dependency scanning | Kända sårbarheter |
| [OWASP ZAP](https://www.zaproxy.org/) | DAST | Runtime scanning |

### AI-assistans

```
User: Kör mutation testing på OrderService och förbättra
      testerna som inte fångar mutationer.
```

---

## Fas 5: Deployment

### CI/CD

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Azure DevOps Pipelines](https://azure.microsoft.com/en-us/products/devops/pipelines)** | CI/CD pipelines | Vår standard för kundprojekt |
| **[GitHub Actions](https://github.com/features/actions)** | CI/CD pipelines | För GitHub-hostade repos |

**När använda vad:**
- **Azure DevOps Pipelines** - Kundprojekt, .NET, Azure-deploy, behöver Boards-integration
- **GitHub Actions** - Open source, GitHub-repos, snabba workflows

### Infrastructure as Code

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Bicep](https://learn.microsoft.com/en-us/azure/azure-resource-manager/bicep/)** | Azure IaC | Vår standard |
| [Terraform](https://www.terraform.io/) | Multi-cloud IaC | Alternativ |
| [Pulumi](https://www.pulumi.com/) | IaC med riktiga språk | TypeScript/C# |

### Containers & Orchestration

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Docker](https://www.docker.com/)** | Containerisering | Standard |
| [Azure Container Apps](https://azure.microsoft.com/en-us/products/container-apps) | Managed containers | Enklare än K8s |
| [Kubernetes](https://kubernetes.io/) | Orchestration | För större system |

### Secrets Management

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Azure Key Vault](https://azure.microsoft.com/en-us/products/key-vault)** | Secrets storage | Vår standard |
| [GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets) | CI/CD secrets | För pipelines |

### AI-assistans

```
User: Generera GitHub Actions workflow för:
      - Build & test på PR
      - Deploy till staging vid merge till main
      - Deploy till prod vid release tag

      Använd Azure Container Apps.
```

---

## Fas 6: Produktion

### Monitoring & Observability

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Application Insights](https://learn.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)** | APM, logging | Azure-native |
| [Sentry](https://sentry.io/) | Error tracking | Bra för frontend |
| [Datadog](https://www.datadoghq.com/) | Full observability | Enterprise |
| [Grafana](https://grafana.com/) | Dashboards | Open source |

### Logging

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **[Serilog](https://serilog.net/)** | .NET structured logging | Vår standard |
| [Azure Monitor](https://azure.microsoft.com/en-us/products/monitor) | Log aggregation | Azure-native |

### Alerting

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| **Azure Monitor Alerts** | Infrastructure alerts | Azure-native |
| [PagerDuty](https://www.pagerduty.com/) | Incident management | On-call rotation |
| [Opsgenie](https://www.atlassian.com/software/opsgenie) | Alerting | Atlassian |

### Feature Flags

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| [Azure App Configuration](https://azure.microsoft.com/en-us/products/app-configuration) | Feature flags | Azure-native |
| [LaunchDarkly](https://launchdarkly.com/) | Feature management | Enterprise |
| [Unleash](https://www.getunleash.io/) | Feature toggles | Open source |

### Status Pages

| Verktyg | Användning | Kommentar |
|---------|------------|-----------|
| [Statuspage](https://www.atlassian.com/software/statuspage) | Public status | Atlassian |
| [Instatus](https://instatus.com/) | Status page | Enklare alternativ |

### AI-assistans

```
User: Analysera dessa Application Insights logs och identifiera:
      - Error patterns
      - Performance bottlenecks
      - Anomalier

      [paste logs eller query]
```

---

## Rekommenderad Stack (Wizardworks)

### Backend
- **Runtime:** .NET 8+
- **API:** ASP.NET Core
- **ORM:** Entity Framework Core
- **Testing:** xUnit + FluentAssertions + FsCheck
- **Logging:** Serilog → Application Insights

### Frontend
- **Framework:** React 19+ / Next.js
- **State:** TanStack Query
- **Forms:** TanStack Form
- **Testing:** Vitest + Playwright
- **Styling:** Tailwind CSS

### Infrastructure
- **Cloud:** Azure
- **IaC:** Bicep
- **Containers:** Docker → Azure Container Apps
- **CI/CD:** Azure DevOps Pipelines / GitHub Actions
- **Secrets:** Azure Key Vault

### Development
- **AI:** Claude Code
- **Editor:** VS Code / Rider
- **Version Control:** Azure Repos / GitHub
- **Project Management:** Azure DevOps Boards

---

Se även: [Development-Lifecycle](Development-Lifecycle.md), [Testing-AI-Generated-Code](Testing-AI-Generated-Code.md)
