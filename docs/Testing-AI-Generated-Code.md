# Testing AI-Generated Code

Teststrategier och verktyg för att säkerställa kvalitet på agent-producerad kod.

## Varför extra viktigt?

AI-agenter kan:
- Skriva kod som "ser rätt ut" men har subtila buggar
- Generera tester som passerar men inte testar rätt saker
- Missa edge cases som en erfaren utvecklare skulle tänka på
- Introducera säkerhetsproblem utan att flagga dem

**Din uppgift**: Verifiera att både kod OCH tester håller måttet.

## Testpyramiden för agentisk utveckling

```
        ╱╲
       ╱  ╲        Manual/Exploratory
      ╱────╲       (Du testar det agenten inte kan)
     ╱      ╲
    ╱  E2E   ╲     Playwright, Cypress
   ╱──────────╲    (Agenten skriver, du validerar)
  ╱            ╲
 ╱ Integration  ╲  API tests, Contract tests
╱────────────────╲ (Agenten skriver, hooks validerar)
╲                ╱
 ╲    Unit      ╱  xUnit, Vitest
  ╲────────────╱   (Agenten skriver, mutation testing validerar)
```

## Nivå 1: Unit Tests

### Vad agenten gör bra
- Generera tester för happy path
- Följa existerande testmönster
- Hög coverage-siffra

### Vad du måste validera
- Testar testerna rätt saker? (inte bara "runs without error")
- Finns meningsfulla assertions?
- Täcks edge cases?

### Verktyg

**Mutation Testing** – Testar dina tester

| Stack | Verktyg | Vad det gör |
|-------|---------|-------------|
| .NET | [Stryker.NET](https://stryker-mutator.io/docs/stryker-net/introduction/) | Muterar kod, verifierar att tester fångar det |
| JS/TS | [Stryker](https://stryker-mutator.io/) | Samma för JavaScript |
| Java | [PIT](https://pitest.org/) | Mutation testing för JVM |

```bash
# Exempel: Stryker.NET
dotnet stryker

# Output visar vilka mutationer som överlevde
# = tester som inte testar vad de borde
```

**När mutation score är låg:**
```
User: Mutation testing visar att 40% av mutationerna överlever
      i OrderService. Förbättra testerna så de fångar faktiska buggar.
```

### Property-Based Testing

Genererar tusentals testfall automatiskt – hittar edge cases.

| Stack | Verktyg |
|-------|---------|
| .NET | [FsCheck](https://fscheck.github.io/FsCheck/) |
| JS/TS | [fast-check](https://github.com/dubzzz/fast-check) |
| Python | [Hypothesis](https://hypothesis.readthedocs.io/) |

```csharp
// Exempel: FsCheck
[Property]
public Property OrderTotal_ShouldNeverBeNegative(PositiveInt[] quantities, PositiveInt[] prices)
{
    var items = quantities.Zip(prices, (q, p) => new OrderItem(q.Get, p.Get));
    var order = new Order(items);

    return (order.Total >= 0).ToProperty();
}
```

```
User: Lägg till property-based tests för prisberäkningen.
      Använd FsCheck. Testa invarianter som:
      - Total är aldrig negativ
      - Total = summan av alla items
      - Tom order = 0
```

## Nivå 2: Integration Tests

### Contract Testing

Säkerställer att frontend och backend är överens om API-kontrakt.

| Verktyg | Användning |
|---------|------------|
| [Pact](https://pact.io/) | Consumer-driven contracts |
| [OpenAPI](https://www.openapis.org/) | Schema-validering |
| [Dredd](https://dredd.org/) | Testar API mot spec |

**Viktigt för agentiskt arbete:**
- Agent genererar frontend → förväntar visst API-format
- Agent genererar backend → implementerar (kanske) annat format
- Contract tests fångar mismatch

```
User: Skapa Pact consumer tests för frontend som dokumenterar
      vilka API-anrop som görs och förväntat response-format.
```

### Database Integration

```
User: Skriv integration tests som verifierar:
      - Transaktioner rullas tillbaka vid fel
      - Concurrent updates hanteras korrekt
      - Foreign keys respekteras
```

## Nivå 3: E2E Tests

### Playwright/Cypress

Agenten kan skriva E2E-tester, men:

**Du validerar:**
- Testar de verkligt användarbeteende?
- Är de stabila (inte flaky)?
- Täcker de kritiska flöden?

```
User: Skriv Playwright E2E-test för checkout-flödet:
      1. Lägg till produkt i varukorg
      2. Gå till checkout
      3. Fyll i leveransinfo
      4. Genomför betalning (mock)
      5. Verifiera orderbekräftelse
```

### Visual Regression

Fångar oavsiktliga UI-ändringar.

| Verktyg | Beskrivning |
|---------|-------------|
| [Percy](https://percy.io/) | Cloud-baserad visual testing |
| [Chromatic](https://www.chromatic.com/) | För Storybook |
| [Playwright screenshots](https://playwright.dev/docs/screenshots) | Inbyggt i Playwright |

## Nivå 4: Security Testing

### SAST (Static Application Security Testing)

Körs automatiskt på AI-genererad kod.

| Verktyg | Stack |
|---------|-------|
| [Semgrep](https://semgrep.dev/) | Multi-language |
| [SonarQube](https://www.sonarqube.org/) | Multi-language |
| [Snyk Code](https://snyk.io/product/snyk-code/) | Multi-language |
| [Bandit](https://bandit.readthedocs.io/) | Python |

```yaml
# GitHub Actions exempel
- name: Semgrep Security Scan
  uses: returntocorp/semgrep-action@v1
  with:
    config: p/owasp-top-ten
```

### Dependency Scanning

AI kan föreslå sårbara dependencies.

| Verktyg | Vad det gör |
|---------|-------------|
| [Dependabot](https://github.com/dependabot) | GitHub-integrerad |
| [Snyk](https://snyk.io/) | Dependency vulnerabilities |
| `npm audit` / `dotnet list package --vulnerable` | Inbyggt |

## Process: Test Review Checklist

### När agenten skrivit tester, kolla:

**Struktur**
- [ ] Arrange-Act-Assert mönster följs
- [ ] Ett koncept per test
- [ ] Beskrivande testnamn

**Coverage**
- [ ] Happy path täckt
- [ ] Error paths täckta
- [ ] Edge cases (null, empty, max values)
- [ ] Boundary conditions

**Kvalitet**
- [ ] Assertions är specifika (inte bara "not null")
- [ ] Testet failar om koden är fel (mutation test)
- [ ] Ingen test-logik (if/else i tester)

**Underhållbarhet**
- [ ] Inte beroende av körordning
- [ ] Inga hårdkodade värden som kan ändras
- [ ] Cleanup sker korrekt

## Automatisering i CI/CD

### Rekommenderad pipeline

```yaml
name: Quality Gates

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      # Unit tests med coverage
      - run: dotnet test --collect:"XPlat Code Coverage"

      # Coverage threshold
      - name: Check coverage
        run: |
          coverage=$(cat coverage.xml | grep -oP 'line-rate="\K[^"]+')
          if (( $(echo "$coverage < 0.80" | bc -l) )); then
            echo "Coverage below 80%"
            exit 1
          fi

      # Mutation testing (kan köras nightly istället)
      - run: dotnet stryker --threshold-high 80 --threshold-low 60

      # Security scan
      - uses: returntocorp/semgrep-action@v1

      # Contract tests
      - run: npm run test:pact

      # E2E
      - run: npx playwright test
```

## När ska du köra vad?

| Test typ | Vid commit | Vid PR | Nightly |
|----------|------------|--------|---------|
| Unit tests | ✅ | ✅ | ✅ |
| Integration | ✅ | ✅ | ✅ |
| E2E | ❌ | ✅ | ✅ |
| Mutation | ❌ | ❌ | ✅ |
| Security scan | ✅ | ✅ | ✅ |
| Visual regression | ❌ | ✅ | ❌ |

## Prompts för testning

### Be om bättre tester
```
User: Testerna du skrev testar bara happy path.
      Lägg till tester för:
      - Null input
      - Tom lista
      - Negativa värden
      - Concurrent access
```

### Validera testkvalietet
```
User: Kör mutation testing på OrderService-testerna
      och förbättra de tester som inte fångar mutationer.
```

### Security-fokus
```
User: Granska koden för OWASP Top 10 sårbarheter.
      Skriv tester som verifierar att vi är skyddade mot:
      - SQL injection
      - XSS
      - Broken authentication
```

---

Se även: [Code-Review-Checklist](Code-Review-Checklist.md), [New-Feature](New-Feature.md)
