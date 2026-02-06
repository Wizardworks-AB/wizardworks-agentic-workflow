# Development Lifecycle

Komplett livscykel för agentisk utveckling - från krav till produktion.

## Översikt

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                                                                             │
│   1. KRAV          2. DESIGN         3. IMPL          4. TEST      5. DEPLOY│
│   ─────────────────────────────────────────────────────────────────────────│
│                                                                             │
│   Kravinfångst  →  Teknisk design  →  TDD/Kod     →  QA        →  Release  │
│   Prioritering     Arkitektur         Review         Staging       Prod     │
│   Scope            Datamodell         Merge          Sign-off      Monitor  │
│                                                                             │
│   ─────────────────────────────────────────────────────────────────────────│
│        ↑                                                          │        │
│        │                    6. PRODUKTION                         │        │
│        │                    ──────────────                        ↓        │
│        └──────────── Buggar & nya krav ←──────────────────────────┘        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

## Fas 1: Kravinfångst

**Mål:** Förstå vad som ska byggas och varför.

### Det konkreta flödet

```
Kundmöte → Transkribering → Strukturering → DevOps Board → Agenter agerar på tasks
```

1. **Kundmöte** — Arkitekt leder samtalet, spelar in (med kundens godkännande)
2. **Transkribering** — AI transkriberar ljudfilen (ElevenLabs STT eller liknande)
3. **Strukturering** — Agent omvandlar transkription till user stories, acceptanskriterier, oklarheter
4. **DevOps Board** — Stories skapas i Azure DevOps / Jira med prioritering
5. **Agenter agerar** — Varje task i backloggen kan tilldelas en agent via `/plan`

### Aktiviteter

| Aktivitet | Agent | Människa |
|-----------|-------|----------|
| Intervjua stakeholders | - | Leder samtalet |
| Transkribera möte | Transkriberar, sammanfattar | Verifierar |
| Dokumentera krav | Strukturerar till user stories | Validerar |
| Identifiera oklarheter | Flaggar luckor | Beslutar |
| Prioritera | Föreslår baserat på beroenden | Beslutar affärsprioritet |
| Skriva user stories | Genererar utkast | Granskar, justerar |

### Agent-användning

```
User: Här är anteckningar från kundmötet: [paste]

      Strukturera detta till:
      1. User stories (som användare vill jag... så att...)
      2. Acceptanskriterier för varje story
      3. Oklarheter vi behöver följa upp
```

```
User: Vi har dessa krav: [lista]

      Analysera beroenden och föreslå implementation-ordning.
      Flagga risker och oklarheter.
```

### Output
- Prioriterad backlog med user stories
- Acceptanskriterier
- Lista på oklarheter att följa upp
- Initial scope-avgränsning

### Kvalitetsgate
- [ ] Alla user stories har acceptanskriterier
- [ ] Oklarheter dokumenterade och tilldelade
- [ ] Stakeholder sign-off på scope

---

## Fas 2: Teknisk Design & Arkitektur

**Mål:** Designa lösningen innan kod skrivs.

**Verktyg:** Mermaid, Draw.io, Excalidraw. AI ger bra utkast men diagram kräver ofta manuell finputsning — speciellt layout, visuell tydlighet och flödeslogik.

### Aktiviteter

| Aktivitet | Agent | Människa |
|-----------|-------|----------|
| Analysera befintlig kodbas | Kartlägger, rapporterar | - |
| Föreslå arkitektur | Genererar alternativ | Beslutar |
| Designa datamodell | Föreslår schema | Validerar |
| Identifiera integrationer | Listar beroenden | Verifierar |
| Riskanalys | Flaggar tekniska risker | Bedömer affärsrisk |
| Skapa ADR (Architecture Decision Record) | Skriver utkast | Godkänner |

### Agent-användning

```
User: /plan Implementera [feature] baserat på dessa user stories: [paste]

      Analysera:
      1. Vilka befintliga komponenter påverkas
      2. Föreslå arkitektur (diagram om möjligt)
      3. Datamodell-ändringar
      4. API-kontrakt
      5. Risker och oklarheter
```

```
User: Vi överväger [Approach A] vs [Approach B].

      Gör en trade-off analys:
      - Komplexitet
      - Performance
      - Underhållbarhet
      - Time to market
```

### Output
- Teknisk design-dokument
- Datamodell/schema
- API-specifikation (OpenAPI)
- Architecture Decision Records
- Implementation plan (ordning, beroenden)

### Kvalitetsgate
- [ ] Design reviewad av senior utvecklare
- [ ] Datamodell validerad
- [ ] API-kontrakt definierat
- [ ] Risker dokumenterade med mitigering
- [ ] Estimate på scope (t-shirt sizing)

---

## Fas 3: Implementation

**Mål:** Bygga funktionaliteten med hög kvalitet.

**Verktyg:** Claude Code (primärt). Codex (OpenAI) som alternativ/backup om Claude inte är tillgänglig.

### Feature-storlek — Kritiskt

**Ju mindre task, desto högre success rate.** Låt inte agenten "refaktorera hela appen".

- **Bryt ner**: En feature i taget, en vy i taget, en endpoint i taget
- **Greenfield**: Frontend först (mockdata) → Backend. Vy för vy, sida för sida
- **Befintlig kodbas**: En modul i taget, gradvis förbättring

### Workflow per feature

```
3.1 Setup        →  3.2 TDD Red    →  3.3 TDD Green  →  3.4 Refactor  →  3.5 PR
Branch              Skriv tester      Implementera      Städa            Review
```

### 3.1 Branch Setup

```bash
git checkout -b feature/user-registration
```

```
User: Läs igenom design-dokumentet i docs/design/user-registration.md
      och bekräfta att du förstår scope och approach.
```

### 3.2 TDD Red Phase

```
User: /tdd Skriv tester för user registration enligt designen.
      Täck:
      - Happy path
      - Validering (email format, password strength)
      - Duplicate email
      - Error handling
```

**Människa validerar:**
- Testar testerna rätt saker?
- Täcks alla acceptanskriterier?

### 3.3 TDD Green Phase

```
User: Implementera så testerna passerar.
      Följ patterns i befintlig kodbas.
```

**Hooks validerar automatiskt:**
- Kodstil
- Arkitekturmönster
- Säkerhetsregler
- Coverage

### 3.4 Refactor

```
User: Refaktorera för läsbarhet och underhållbarhet.
      Inga nya features - bara städning.
```

### 3.5 Code Review & PR

```
User: /code-review

User: /security-review  (för känslig kod)
```

**Människa gör final review** enligt [Code-Review-Checklist](Code-Review-Checklist.md).

### Output
- Feature branch med implementation
- Tester (80%+ coverage)
- Uppdaterad dokumentation
- PR redo för merge

### Kvalitetsgate
- [ ] Alla tester passerar
- [ ] Coverage >= 80%
- [ ] Hooks passerar
- [ ] Code review godkänd
- [ ] Security review (om tillämpligt)

---

## Fas 4: Test & QA

**Mål:** Verifiera kvalitet innan produktion.

> **Policy:** Test är mandatory i alla projekt — inga undantag. AI gör det på minuter, det finns ingen ursäkt att skippa. Detta höjer leveranskvaliteten dramatiskt.

### Testtyper

| Typ | Ansvar | När |
|-----|--------|-----|
| Unit tests | Agent (skriver), CI (kör) | Vid varje commit |
| Integration tests | Agent (skriver), CI (kör) | Vid varje commit |
| E2E tests | Agent (skriver), CI (kör) | Vid PR |
| Manuell QA | Människa | Innan release |
| Security scan | CI (automatisk) | Vid varje commit |

### Agent-användning

```
User: Skriv E2E-test med Playwright för hela registration-flödet:
      1. Gå till /register
      2. Fyll i formulär
      3. Submitta
      4. Verifiera confirmation page
      5. Kolla att welcome email skickats (mock)
```

```
User: Kör /security-review på all ny kod i denna PR.
```

### Staging-validering

```
User: Gör en smoke test av staging-miljön.
      Verifiera:
      - Login fungerar
      - Nya features accessbara
      - Inga console errors
      - Performance rimlig
```

### Output
- Test rapport (coverage, pass/fail)
- Security scan rapport
- QA sign-off
- Release notes draft

### Kvalitetsgate
- [ ] Alla automatiska tester passerar
- [ ] Manuell QA genomförd
- [ ] Security scan utan kritiska issues
- [ ] Performance acceptabel
- [ ] Stakeholder demo/sign-off

---

## Fas 5: Deployment

**Mål:** Säker release till produktion.

> **Policy:** Deploy sker alltid via DevOps-pipelines (Azure DevOps / GitHub Actions). Aldrig direkt från terminal. Människan fattar deploy-beslutet.

### Deployment Checklist

```
User: Generera deployment checklist för denna release:
      - Pre-deployment checks
      - Deployment steps
      - Verification steps
      - Rollback plan
```

### Pre-deployment

| Check | Ansvar |
|-------|--------|
| Alla tester gröna | CI |
| Staging validerad | QA |
| Database migrations testade | Agent + Människa |
| Feature flags konfigurerade | Människa |
| Monitoring/alerts uppsatta | Agent + Människa |

### Deployment

```
User: Verifiera att deployment-scriptet är uppdaterat för:
      - Nya environment variables
      - Database migrations
      - Infrastructure changes (Bicep)
```

**Människa utför:**
- Triggar deployment pipeline
- Övervakar deployment
- Gör smoke test i produktion

### Post-deployment

```
User: Generera post-deployment verification checklist baserat på
      vilka features som releasades.
```

- [ ] Health checks passerar
- [ ] Smoke test OK
- [ ] Inga error spikes i logs
- [ ] Performance metrics normala

### Output
- Deployment genomförd
- Verification checklist avbockad
- Release notes publicerade
- Stakeholders notifierade

---

## Fas 6: Produktion - Buggar & Nya Krav

**Mål:** Hantera verkligheten efter release — och helst upptäcka problem innan kunden gör det.

### Proaktiv Övervakning (Vision)

Målet är att gå från reaktiv till proaktiv bugghantering:

```
Idag:     Kund rapporterar bugg → Vi fixar
Imorgon:  Agent upptäcker problem → Föreslår fix → Staging → Människa validerar → Prod
```

**Long-running production agent** (framtida):
- Övervakar loggar och metrics kontinuerligt
- Rapporterar nya fel, performance-degradering, säkerhetsvarningar
- Föreslår fix → arkitekt granskar → staging → validering → produktion
- **Människan fattar alltid deploy-beslutet**

Se [Vision](Vision.md) för mer detaljer.

### Bug Workflow

```
6.1 Rapport    →  6.2 Triage    →  6.3 Fix       →  6.4 Deploy
Inkomna          Prioritera       Implementera     Hotfix/Release
```

#### 6.1 Bug Rapport

```
User: Strukturera denna buggrapport:
      [paste från användare/support]

      Extrahera:
      - Steg för att reproducera
      - Förväntat vs faktiskt beteende
      - Severity (kritisk/hög/medium/låg)
      - Påverkade användare
```

#### 6.2 Triage

```
User: Analysera denna bugg mot kodbasen.
      - Var är trolig root cause?
      - Hur många användare påverkas?
      - Finns workaround?
      - Är det regression?
```

**Människa beslutar:**
- Hotfix (direkt till prod) vs normal release
- Prioritet i backlog

#### 6.3 Bug Fix

Se [Bug-Investigation](Bug-Investigation.md) för detaljerat workflow.

```
User: Fixa buggen med TDD-approach:
      1. Skriv test som reproducerar buggen
      2. Fixa så testet passerar
      3. Verifiera ingen regression
```

#### 6.4 Hotfix Deploy

```
User: Generera hotfix deployment checklist.
      Extra focus på:
      - Minimal change set
      - Snabb rollback-plan
      - Kommunikation till påverkade
```

### Nya Krav i Produktion

Feedback loop tillbaka till Fas 1:

```
User: Vi har fått denna feedback från produktion: [paste]

      Kategorisera som:
      - Bug (fel i befintlig funktionalitet)
      - Enhancement (förbättring av befintlig)
      - New feature (ny funktionalitet)

      Föreslå prioritering baserat på:
      - Antal påverkade användare
      - Affärsvärde
      - Implementation effort
```

---

## Roller genom livscykeln

| Fas | Agent | Utvecklare | Tech Lead | Product Owner |
|-----|-------|-----------|-----------|---------------|
| Krav | Strukturerar | - | Validerar teknik | Driver |
| Design | Analyserar, föreslår | Implementerar design | Beslutar arkitektur | Validerar scope |
| Implementation | Kodar, testar | Granskar, guidar | Review | - |
| Test | Skriver tester | Manuell QA | Sign-off | Acceptans |
| Deploy | Assisterar | Utför | Godkänner | Sign-off |
| Produktion | Analyserar, fixar | Triagerar | Prioriterar | Prioriterar |

---

## Dokumentation genom livscykeln

| Fas | Dokument | Var |
|-----|----------|-----|
| Krav | User stories, acceptanskriterier | Backlog (Jira/Linear) |
| Design | ADR, API spec, datamodell | `docs/design/` |
| Implementation | Kod, tester | `src/`, `tests/` |
| Test | Testrapporter | CI artifacts |
| Deploy | Release notes, runbooks | `docs/releases/` |
| Produktion | Incident reports | `docs/incidents/` |

---

## Iteration & Kontinuerlig Förbättring

### Retrospektiv med `/insight`

Använd Claude Codes `/insight` regelbundet för att identifiera mönster:

```
User: /insight
```

Claude analyserar sessionen och visar:
- Vanligaste misstagen
- Återkommande frågor
- Mönster som kan förbättras

**Spara insikter till memory** så att Claude undviker samma misstag i framtiden.

### Frekvens

| Nivå | När | Hur |
|------|-----|-----|
| Daglig | Slutet av arbetsdagen | Kör `/insight`, spara till memory |
| Sprint | Varje sprintavslut | Kör `/retrospective`, diskutera i teamet |
| Release | Efter varje release | Analysera hela releasen |

### Efter varje release

```
User: Analysera denna release:
      - Vad gick bra?
      - Vad kunde förbättrats?
      - Vilka oväntade problem uppstod?
      - Hur kan vi förebygga dem nästa gång?
```

Uppdatera:
- Checklists
- Automation
- Dokumentation
- Team practices

---

Se även: [Quick-Start](Quick-Start.md), [Vision](Vision.md), [Tooling-By-Phase](Tooling-By-Phase.md), [New-Feature](New-Feature.md), [Bug-Investigation](Bug-Investigation.md), [Testing-AI-Generated-Code](Testing-AI-Generated-Code.md), [Code-Review-Checklist](Code-Review-Checklist.md), [Documentation-Standard](Documentation-Standard.md), [Client-Agent-Setup](Client-Agent-Setup.md)
