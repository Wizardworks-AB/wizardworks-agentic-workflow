# Superpowers Integration

Hur Wizardworks använder Superpowers-plugin som komplement till vår setup.

---

## Vad är Superpowers?

Superpowers är ett officiellt Claude Code-plugin med inbyggda workflows för TDD, sub-agents, code reviews och git worktrees. Det ger kraftfulla workflows "out of the box" som kompletterar vår Wizardworks-specifika setup.

---

## Installation

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

Lägg till i projektets CLAUDE.md under setup-instruktioner så alla i teamet installerar samma version.

---

## Vad Superpowers ger oss

| Superpowers-funktion | Ersätter/kompletterar | Status |
|----------------------|----------------------|--------|
| `/brainstorming` | Kompletterar vår `/plan` | Bra för idéfas |
| `/execute-plan` | Sub-agent-driven utveckling med review-checkpoints | Kompletterar TDD-workflow |
| TDD-enforcement | Kompletterar vår `/tdd` | Extra validering |
| Code review | Kompletterar vår `/code-review` | Bredare perspektiv |
| Git worktrees | Parallell utveckling | Nytt — vi har inget motsvarande |

---

## Vad vi behåller som Wizardworks-specifikt

Superpowers ersätter INTE vår domänspecifika konfiguration:

### Skills (pattern libraries)
- `backend-patterns-dotnet/` — .NET/C# patterns
- `frontend-patterns-react/` — React/TypeScript patterns
- `infrastructure-as-code/` — Bicep/Docker/Azure patterns

### Hooks (automatisk validering)
- `check-secrets.js` — Blockerar hårdkodade hemligheter
- `check-dto-usage.js` — Validerar DTO-mönster
- `check-layer-separation.js` — CSR-arkitektur
- `check-public-ids.js` — Aldrig exponera databas-ID
- Övriga Wizardworks-hooks

### Rules (standards)
- `coding-style.md` — Wizardworks namngivning och formatering
- `security.md` — Säkerhetskrav
- `testing.md` — TDD och coverage-krav
- `git-workflow.md` — Commits, branches, PRs

### Commands (Wizardworks-specifika)
- `/commit` — Commit med P&P-validering och dokumentation
- `/security-review` — Wizardworks säkerhetsgranskning

---

## Rekommenderad setup

**Superpowers som bas, Wizardworks som overlay.**

1. Installera Superpowers för generella workflows
2. Behåll alla Wizardworks skills, hooks, rules
3. Behåll Wizardworks-specifika commands
4. Låt Superpowers hantera generell TDD, brainstorming, och worktrees
5. Våra hooks fångar Wizardworks-specifika mönster som Superpowers inte vet om

### Fördelar

- **Mindre underhåll** — Vi slipper underhålla generella commands som Superpowers redan har
- **Bättre generella workflows** — Superpowers utvecklas av community
- **Wizardworks-kvalitet** — Våra hooks och rules säkerställer vår standard oavsett
- **Parallell utveckling** — Git worktrees möjliggör att agenten jobbar på en branch medan du jobbar på en annan

---

## Begränsningar

- Superpowers vet inget om våra patterns (CSR, Public IDs, DTOs)
- Superpowers code review täcker inte Wizardworks-specifika checks
- Behöver kombineras med våra hooks för full validering
- Plugin-ekosystemet är ungt — kan förändras snabbt

---

## Utvärdering

Philip har i uppdrag att testa Superpowers och rapportera. Uppdatera denna sida med resultat.

**Frågor att besvara:**
- Krockar Superpowers med några av våra commands?
- Finns det redundans som kan elimineras?
- Hur fungerar worktrees med vår workflow?
- Påverkar det token-användningen?

---

Se även: [Quick Start](Quick-Start.md), [Development Lifecycle](Development-Lifecycle.md)
