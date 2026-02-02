# Developer-Agent Dynamics

Vem gör vad i samarbetet mellan människa och agent.

## Rollfördelning

| Område | Agent | Utvecklare |
|--------|-------|------------|
| **Kodgenerering** | Producerar | Granskar, godkänner |
| **Arkitektur** | Föreslår | Beslutar |
| **Testning** | Skriver, kör | Validerar täckning |
| **Debugging** | Analyserar, söker | Verifierar fix |
| **Dokumentation** | Genererar | Säkerställer korrekthet |
| **Säkerhet** | Flaggar issues | Ansvarar för beslut |
| **Affärslogik** | Implementerar | Definierar krav |
| **Deployment** | Assisterar | Ansvarar, utför |

## Tre modes

### Pair Programming Mode
**Intensivt samarbete, du är aktivt involverad.**

Använd när:
- Komplex problemlösning
- Du vill förstå koden djupt
- Utforskande arbete
- Onboarding i ny kodbas

Workflow:
```
Du: "Visa mig hur OrderService hanterar concurrent orders"
Agent: [Förklarar kod]
Du: "Vad händer om två orders kommer samtidigt?"
Agent: [Analyserar race condition]
Du: "Lägg till locking, men förklara varje steg"
Agent: [Implementerar med förklaringar]
```

### Delegation Mode
**Agenten jobbar självständigt, du granskar resultat.**

Använd när:
- Tydligt definierad uppgift
- Följer etablerade mönster
- Du litar på hooks för validering

Workflow:
```
Du: "/tdd Implementera CRUD för Products enligt samma
     mönster som Orders"
Agent: [Kör hela flödet]
[Hooks validerar]
Agent: "Klart. 6 tester, 94% coverage. PR redo för review."
Du: [Granskar diff, godkänner]
```

### Review Mode
**Du granskar agentens eller andras arbete.**

Använd när:
- PR review
- Säkerhetsgranskning
- Kvalitetskontroll

Workflow:
```
Du: "/code-review denna PR"
Agent: [Analyserar, rapporterar issues]
Du: [Fokuserar på affärslogik och arkitektur]
Du: "Flagga om det finns N+1 query issues"
Agent: [Djupare analys]
```

## Kommunikationsmönster

### Tydlig handoff
```
Du: "Analysera problemet och rapportera. Implementera INTE ännu."
Agent: [Bara analys]

Du: "Bra analys. Implementera alternativ 2."
Agent: [Implementerar]
```

### Checkpoint
```
Du: "Pausa efter varje fil och visa mig ändringarna."
Agent: [Ändrar fil 1]
Agent: "UserController.cs uppdaterad. Fortsätta?"
Du: "Ja, fortsätt"
```

### Scope-begränsning
```
Du: "Bara ändra i OrderService. Rör inte andra filer."
Agent: [Håller sig till scope]
```

## Trust levels

### High trust (delegation OK)
- Boilerplate och setup
- Tester baserade på spec
- Dokumentationsuppdateringar
- Refaktorering med tydliga ramar
- Bug fixes med isolerat scope

### Medium trust (pair programming)
- Ny affärslogik
- Arkitekturförändringar
- Komplext tillståndshantering
- API-designbeslut

### Low trust (manual + review)
- Säkerhetskritisk kod
- Databas-migrations
- Produktionsändringar
- Irreversibla operationer

## Eskalering

### Agent osäker
```
Agent: "Jag är osäker på hur vi hanterar X.
        Två alternativ:
        1. [Approach A] - fördel/nackdel
        2. [Approach B] - fördel/nackdel

        Vilket föredrar du?"
```

**Du beslutar**, agenten implementerar.

### Du osäker
```
Du: "Jag är inte säker på bästa approach.
     Vad är pros/cons med caching här?"

Agent: [Analyserar, presenterar options]

Du: [Fattar informerat beslut]
```

### Komplex situation
```
Du: "Detta är komplext. Låt oss ta det steg för steg.
     Börja med att förklara nuvarande implementation."

Agent: [Förklarar]

Du: "OK, nu förstår jag. Nästa steg..."
```

---

Nästa: [Pair-Programming-Mode](Pair-Programming-Mode.md)
