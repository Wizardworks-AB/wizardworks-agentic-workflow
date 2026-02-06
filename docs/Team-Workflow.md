# Team Workflow

Hur flera arkitekter och utvecklare samarbetar med AI-agenter i samma kundprojekt.

---

## Tre lager

```
┌─────────────────────────────────────┐
│  1. TASK MANAGEMENT                 │  ← Var lever arbetet?
│     Azure DevOps: backlog, board    │
├─────────────────────────────────────┤
│  2. KOORDINERING                    │  ← Vem gör vad just nu?
│     Agent-repo: humans/, status.md  │
├─────────────────────────────────────┤
│  3. EXEKVERING                      │  ← Hur utförs arbetet?
│     Claude Code + kundens kod-repo  │
└─────────────────────────────────────┘
```

---

## Katalogstruktur

Agent-kontext och kundens kod lever som syskon — aldrig i samma repo:

```
~/
├── agents/
│   ├── bokio-agent/                    ← Git repo (Wizardworks privat)
│   │   ├── CLAUDE.md                   ← Delad kontext om kunden
│   │   ├── .claude/                    ← Agents, hooks, rules, commands
│   │   ├── docs/
│   │   │   ├── meeting-notes/          ← Mötesanteckningar
│   │   │   └── decisions/              ← Arkitekturbeslut (ADR)
│   │   ├── humans/
│   │   │   ├── daniel/
│   │   │   │   └── status.md           ← Daniels pågående arbete
│   │   │   ├── mia/
│   │   │   │   └── status.md           ← Mias pågående arbete
│   │   │   └── andreas/
│   │   │       └── status.md
│   │   └── shared/
│   │       └── insights.md             ← Delade insikter alla bör veta
│   │
│   └── ikano-agent/                    ← Annat kundprojekt
│       ├── CLAUDE.md
│       └── ...
│
└── code/
    ├── bokio/
    │   ├── project-a/                  ← Kundens repo (deras Git)
    │   └── project-b/
    └── ikano-bostad/
        └── ip-2.0/
```

### Varför sibling-struktur?

- **Vår IP skyddas** — CLAUDE.md, agents, hooks, memory hamnar aldrig i kundens repo
- **Ren separation** — Kundens kod är orörd, inga Wizardworks-filer
- **Flera projekt per kund** — Enkelt att lägga till project-b, project-c
- **Teamdelning** — Alla klonar agent-repot, alla har samma kontext

---

## Lager 1: Task Management — Azure DevOps

Azure DevOps är den centrala platsen för tasks. Alla ser samma board.

### Board-struktur

```
Epic: Fastighetsdata-pilot
├── Feature: Larmhantering-integration
│   ├── Task: Designa API-kontrakt för larmdata          [Daniel] [In Progress]
│   ├── Task: Implementera adapter Vitec → IP 2.0        [Mia]    [To Do]
│   └── Task: E2E-test larmflöde                          [—]      [To Do]
├── Feature: Värmestyrning-dashboard
│   └── Task: ...
```

### Board-kolumner

```
To Do  →  In Progress  →  In Review  →  Done
```

### Regler

- Varje task är **agent-stor** — en task = en agent kan lösa den i en session
- Tasks har alltid en **tilldelad människa** (ägare)
- Ingen startar en task utan att tilldela sig själv
- Status uppdateras i DevOps, synligt för alla

### Vad är "agent-stor"?

Ju mindre task, desto högre success rate. En task ska vara typ:
- "Implementera endpoint X med TDD"
- "Skriv adapter för system Y"
- "Fixa bugg Z"

Inte:
- "Bygg hela integrationen"
- "Refaktorera all autentisering"

---

## Lager 2: Koordinering — `humans/`

Varje person har en egen mapp i agent-repot. Varje person skriver bara i sin egen mapp — inga merge-konflikter.

### `humans/mia/status.md`

```markdown
# Mia — Bokio

## Arbetar med just nu
- IB-1234: Larmdata API-kontrakt (branch: feature/IB-1234)
- Blockerad på: väntar på API-spec från kund

## Senast avslutat
- IB-1230: Vitec-adapter (PR #42, mergad)

## Insikter
- Vitec API har rate limit på 100 req/min — dokumenterat i shared/insights.md
```

### `shared/insights.md`

Delade insikter som alla i teamet bör veta:

```markdown
# Delade insikter — Bokio

## API-begränsningar
- Vitec API: rate limit 100 req/min
- HomeQ API: kräver OAuth2, token expires efter 1h

## Kända problem
- Staging-miljön har gammal DB-schema — kör migrations innan test

## Kundens preferenser
- Föredrar kebab-case i API-endpoints
- Vill ha svenska felmeddelanden i kundgränssnitt
```

### Vad koordineringen löser

| Scenario | Utan `humans/` | Med `humans/` |
|----------|---------------|---------------|
| Mia går på semester | Daniel vet inte var hon slutade | Läser `humans/mia/status.md` |
| Två börjar på samma task | Ingen ser det förrän konflikt | DevOps board + status.md visar det direkt |
| Ny person onboardas | "Fråga Mia vad som hänt" | Läs CLAUDE.md + status + insights |
| Insikt om kundens API | Stannar i Mias huvud | Sparas i `shared/insights.md` |

---

## Lager 3: Exekvering

### Flödet: en task från start till slut

```
1. Arkitekt plockar task från DevOps board
   → Tilldelar sig själv, sätter "In Progress"

2. Arkitekt kör `claude` från agent-repot
   → Claude: "Vem jobbar?" → "Mia"
   → Claude läser humans/mia/status.md
   → Claude vet var koden är (CLAUDE.md pekar på code/)

3. Arkitekt ger task till agenten
   → Copy/paste från DevOps (idag)
   → MCP-integration med DevOps (framtid)

4. Agent exekverar i kundens kod-repo
   → /plan → /tdd → /code-review → /commit
   → Kod-commits hamnar i kundens repo

5. Agent uppdaterar status
   → Skriver till humans/mia/status.md
   → Eventuella insikter till shared/insights.md

6. Arkitekt granskar och mergar PR
   → Sätter task till "Done" i DevOps

7. Arkitekt pushar agent-repot
   → Daniel och Andreas kan se vad Mia gjort
```

### Två separata Git-flöden

| Vad | Repo | Push-frekvens |
|-----|------|---------------|
| Kundens kod | `code/bokio/project-a/` | Vid varje PR/feature |
| Vår agent-kontext | `agents/bokio-agent/` | Dagligen (minst) |

### Branch per task i kundens repo

```
feature/IB-1234-larmdata-api-kontrakt
feature/IB-1235-vitec-adapter
```

- En branch = en task = en agent-session
- Inga två personer på samma branch
- PR tillbaka till develop/main

---

## CLAUDE.md — Instruktioner till agenten

Varje kundprojekts CLAUDE.md berättar för agenten om strukturen:

```markdown
# Bokio

## Domän
[Beskrivning av kundens verksamhet]

## Kod
Kundens kod finns i: `~/code/bokio/project-a/`
Git-operationer (branch, commit, push) körs därifrån.
Ändra ALDRIG filer i detta agent-repo förutom humans/ och shared/.

## Vem är jag?
Vid sessionsstart: fråga vilken utvecklare som jobbar.
Läs `humans/[namn]/status.md` för kontext.
Uppdatera status.md löpande under sessionen.

## Tasks
Backlog finns i Azure DevOps: [länk till board]

## Säkerhet
[Obligatoriska säkerhetsinstruktioner — se Client-Agent-Setup.md]
```

---

## Setup: Ny kund

### 1. Skapa agent-repo

```bash
mkdir -p ~/agents/bokio-agent
cd ~/agents/bokio-agent
git init

# Kopiera template från claude-code
cp -r /path/to/claude-code/template/.claude .
cp /path/to/claude-code/template/CLAUDE.md .
# (eller relevant variant: existing-codebase, maintenance)

# Skapa struktur
mkdir -p docs/meeting-notes docs/decisions shared
mkdir -p humans/daniel humans/mia  # Per teammedlem

# Initial commit
git add -A && git commit -m "Initial setup for Bokio"
git remote add origin git@github.com:Wizardworks-AB/bokio-agent.git
git push -u origin master
```

### 2. Klona kundens kod

```bash
mkdir -p ~/code/bokio
cd ~/code/bokio
git clone <kundens-repo-url> project-a
```

### 3. Anpassa CLAUDE.md

- Lägg till domänbeskrivning
- Peka på rätt kodsökväg
- Lägg till DevOps board-länk
- Anpassa hooks/rules efter kundens patterns (om befintlig kodbas)

### 4. Resten av teamet

```bash
# Mia klonar agent-repot
git clone git@github.com:Wizardworks-AB/bokio-agent.git ~/agents/bokio-agent

# Mia klonar kundens kod
git clone <kundens-repo-url> ~/code/bokio/project-a
```

---

## Planerat: MCP-integration med Azure DevOps

Idag: Människa copy/pastar task-beskrivning till agenten.
Nästa steg: Agent läser direkt från DevOps via MCP.

**Status:** Ej installerat ännu — står på att-göra-listan.

```
Agent ←→ MCP Server ←→ Azure DevOps API
              ↓
         Läser board
         Ser tilldelade tasks
         Uppdaterar status
```

**Vad det möjliggör:**
- "Visa mina tasks" → Agent visar board
- "Ta nästa task" → Agent plockar högst prioriterade
- "Markera klar" → Agent uppdaterar DevOps

Detta är den MCP-server som diskuterades i Lunch & Learn — DevOps-integration + Harvest-tidrapportering i samma server.

**TODO:**
- [ ] Sätta upp MCP-server för Azure DevOps
- [ ] Integrera Harvest-tidrapportering i samma server
- [ ] Testa med ett pilotprojekt

---

## Visualisering

| Tidshorisont | Verktyg | Vad |
|-------------|---------|-----|
| **Nu** | Azure DevOps boards | Tasks per kund, status, tilldelning |
| **Snart** | Power BI + DevOps | Velocity, blockerade tasks, trender |
| **Vision** | Gemensam skärm på kontoret | Alla kunder, agent-aktivitet, hälsostatus |

---

## Daglig synk

Behöver inte vara standup. Kan vara async i Teams eller via agent-repot:

- "Jag tar IB-1234 och IB-1235 idag"
- "IB-1230 är klar, PR uppe"
- "Blockerad på IB-1237, behöver input om API"

Alternativt: varje persons `status.md` ÄR den dagliga synken — synlig för alla via Git.

---

Se även: [Client-Agent-Setup](Client-Agent-Setup.md), [Development-Lifecycle](Development-Lifecycle.md), [Vision](Vision.md), [Quick-Start](Quick-Start.md)
