# Client Agent Setup

Hur vi förbereder och delar en kundspecifik Claude mellan arkitekter.

---

## Koncept

Varje kundprojekt får en egen förberedd Claude med domänkunskap. Tänk "Boku Claude" eller "Ikano Claude" — en agent som förstår kundens domän, krav och kodbas.

---

## Uppstartsfas (före kod)

### 1. Samla kontext

Innan en rad kod skrivs, bygg domänkunskap:

- **Kundmöten** — Transkribera och sammanfatta
- **Kravdokumentation** — User stories, acceptanskriterier
- **Kundens riktlinjer** — Kodstandarder, arkitektur, designsystem
- **Domänterminologi** — Ordlista med kundens begrepp

### 2. Skapa CLAUDE.md

Projektets `CLAUDE.md` ska innehålla:

```markdown
# [Kundprojekt]

## Domän
[Kort beskrivning av kundens verksamhet och projektets syfte]

## Terminologi
- [Term] — [Definition]

## Projektspecifika patterns
[Avviker vi från Wizardworks standard? Dokumentera här]

## Kontakter
- [Roll] — [Namn]
```

### 3. Populera memory

Spara viktiga insikter från kundmöten i Claude Codes memory:
- Arkitekturbeslut och varför
- Kundens preferenser
- Kända begränsningar
- Vanliga frågor och svar

---

## Vid kodleverans

### 4. Analysera befintlig kodbas

Om kunden har befintlig kod:
- Review mot kundens patterns (inte Wizardworks blint)
- Dokumentera avvikelser i CLAUDE.md
- Anpassa hooks och rules efter kundens standard

### 5. Iterativ uppbyggnad

Varje sprint/iteration:
- Uppdatera CLAUDE.md med nya insikter
- Kör `/insight` för att identifiera mönster och misstag
- Spara insikter till memory

---

## Delning inom teamet

Agent-kontext och kundens kod lever som syskon — vår CLAUDE.md, `.claude/` och memory hamnar **aldrig** i kundens repo. Se **[Team Workflow](Team-Workflow.md)** för komplett katalogstruktur, koordinering mellan arkitekter, och dagligt arbetsflöde.

**Kort sammanfattning:**

```
~/clients/
├── agents/
│   └── bokio-agent/           ← Wizardworks privat Git-repo
│       ├── CLAUDE.md           ← Delad kontext om kunden
│       ├── .claude/            ← Agents, hooks, rules
│       ├── humans/             ← Per-person status och kontext
│       │   ├── daniel/
│       │   └── mia/
│       └── shared/             ← Delade insikter
└── code/
    └── bokio/
        └── project-a/          ← Kundens repo (deras Git)
```

- **Agent-repot** delas mellan arkitekter via Git (Wizardworks privat)
- **Kundens kod** är separat — vår IP rör aldrig deras repo
- **Per-person kontext** i `humans/` — undviker merge-konflikter
- **Delade insikter** i `shared/` — alla ser samma

---

## Säkerhet och Versionhantering

### Lessons Learned

Claude har vid tillfällen raderat filer och tabeller — säkerhetsinstruktioner i CLAUDE.md är **mandatory**.

### Obligatoriska säkerhetsinstruktioner i CLAUDE.md

Varje kundprojekts CLAUDE.md ska innehålla:

```markdown
## Säkerhet

### Filhantering
- Vid flytt av filer: ALLTID kopiera först, verifiera, sedan radera original
- Använd `cp` följt av `rm`, ALDRIG `mv` för viktiga filer
- Radera ALDRIG filer utan explicit godkännande

### Databas
- Kör ALDRIG destructiva queries (DROP, DELETE utan WHERE, TRUNCATE) utan explicit godkännande
- Gör ALLTID backup innan schema-ändringar
- Testa migrations i staging innan produktion

### Git
- Kör ALDRIG `git push --force` utan explicit godkännande
- Committa ALDRIG hemligheter (API-nycklar, lösenord, connection strings)
- Pusha ALDRIG direkt till main/master
```

### Versionhantering av agentdata

All agentdata ska cloud-synkas för att undvika förlust:

| Data | Var | Synk |
|------|-----|------|
| CLAUDE.md | Projektets repo | Git |
| `.claude/` (agents, commands, hooks, rules) | Projektets repo | Git |
| Delade insikter | `.claude/memory/` i projektets repo | Git |
| Personlig memory | `~/.claude/` | Personlig (ej delad) |
| Mötesanteckningar | Projektets wiki/docs | Git |

**Backup-strategi:**
- Git push regelbundet (minst dagligen vid aktivt arbete)
- Viktiga insikter sparas i CLAUDE.md (inte bara i personlig memory)
- OneDrive som extra backup för filer utanför Git

---

## Överlämning

### Ny arkitekt tar vid

När någon tar över ett kundprojekt:

1. **Klona repot** — CLAUDE.md och `.claude/` följer med
2. **Fråga Claude**:
   - "Vad har gjorts hittills?"
   - "Vad är nästa steg?"
   - "Vilka beslut har tagits och varför?"
   - "Finns det kända problem eller risker?"
3. **Läs mötesanteckningar** — i projektets wiki/docs
4. **Kör `/plan`** — för att förstå nuläget

### Scenarion

**Mia går på semester → Daniel tar vid:**
1. Daniel klonar/pullar senaste versionen
2. CLAUDE.md ger full kontext om kundens domän
3. Git-historik visar vad som gjorts
4. Daniel kan direkt börja jobba

**Nytt teammedlem onboardas:**
1. Läs CLAUDE.md (5 min)
2. Läs docs/ för arkitekturbeslut (10 min)
3. Kör `/plan` på första tasken — Claude har redan kontexten

---

## Organisationsmodell

### Arkitektteam per kund

Vi organiserar oss i team per kund — inte en person per kund. Detta möjliggör:
- **Kontinuitet** — Teamet har alltid någon tillgänglig (semester, sjukdom)
- **Kunskapsdelning** — Flera personer förstår kundens domän
- **Skalbarhet** — Kan öka/minska kapacitet per kund efter behov

### Roller i teamet

```
┌─────────────────────────────────────────┐
│              Kundprojekt                │
│                                         │
│  Kundansvarig (Arkitekt + PL)          │
│  ├── Customer facing                    │
│  ├── Äger kundrelation                  │
│  ├── Fattar arkitekturbeslut            │
│  └── Styr agenternas arbete            │
│                                         │
│  Teammedlemmar (Arkitekter/Utvecklare) │
│  ├── Ej customer facing                │
│  ├── Utbytbara vid behov               │
│  ├── Jobbar via samma CLAUDE.md        │
│  └── Övertar vid semester/sjukdom      │
│                                         │
│  AI-agenter                            │
│  ├── Exekverar tasks                   │
│  ├── Delar kontext via Git             │
│  └── Konfigurerade per kund            │
└─────────────────────────────────────────┘
```

### Prissättning

- **Standard** — Arkitektteam, ej låsta personer
- **Premium** — Kund vill låsa specifika personer → premium-prissättning (personen blir sårbar punkt, kunden betalar för den risken)

---

## Checklist: Ny kund

- [ ] CLAUDE.md skapad med domänbeskrivning
- [ ] Kundens kodstandarder dokumenterade (om befintlig kodbas)
- [ ] Terminologi-ordlista
- [ ] Hooks anpassade efter kundens patterns
- [ ] Första kundmöte transkriberat och sammanfattat
- [ ] `.claude/` committat till projektets repo

---

Se även: [Development Lifecycle](Development-Lifecycle.md), [Quick Start](Quick-Start.md)
