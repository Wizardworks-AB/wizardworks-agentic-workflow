# Common Pitfalls

Vanliga misstag att undvika när du arbetar med agenter.

## Instruktionsfel

### För vagt
```
❌ "Gör det bättre"

Problemet: Agenten gissar vad "bättre" betyder.
Fix: Var specifik - "Förbättra läsbarhet genom att extrahera
     denna 50-raders metod till mindre funktioner"
```

### För brett
```
❌ "Bygg ett komplett auth-system"

Problemet: Scope creep, inkonsistenta beslut.
Fix: Bryt ner - "Börja med login endpoint. Vi tar
     registration, password reset separat."
```

### Antar kunskap
```
❌ "Använd vårt standard-pattern"

Problemet: Agenten vet inte vad som är "standard" för er.
Fix: Var explicit - "Använd Repository pattern som i
     UserRepository.cs"
```

## Granskningsfel

### Blind acceptans
```
❌ Agent: "Klart!"
   Du: "Ser bra ut!" [merge utan att läsa]

Problemet: Du missar buggar, säkerhetshål.
Fix: Läs alltid koden. Förstå vad den gör.
```

### Övertro på tester
```
❌ "100% coverage = korrekt kod"

Problemet: Tester kan vara ytliga eller testa fel saker.
Fix: Granska test quality, inte bara quantity.
```

### Ignorera hooks
```
❌ "Hook-varningen är säkert OK"

Problemet: Hooks finns för att fånga riktiga problem.
Fix: Förstå varningen. Fixa eller medvetet override.
```

## Samarbetsfel

### Micro-management
```
❌ "Skriv exakt: public async Task<User>..."

Problemet: Du gör agentens jobb, långsammare än att koda själv.
Fix: Ge mål och constraints, låt agenten välja implementation.
```

### Ingen feedback
```
❌ Agent gör fel → Du fixar tyst själv

Problemet: Agenten lär sig inte inom sessionen.
Fix: Säg till - "Det där var fel. EmailService, inte
     MailService. Uppdatera."
```

### Glömmer kontext
```
❌ Ny session: "Fortsätt där vi slutade"

Problemet: Agenten har inte kontext från tidigare sessioner.
Fix: Sammanfatta relevant kontext, eller använd /plan
     för att återetablera.
```

## Arkitekturfel

### Acceptera anti-patterns
```
❌ Agent föreslår snabb lösning som bryter arkitekturen

Problemet: Teknisk skuld ackumuleras.
Fix: Insistera på rätt lösning även om det tar längre tid.
```

### Overengineering
```
❌ Agent bygger abstraktion för ett enda use case

Problemet: Onödig komplexitet.
Fix: "Vi behöver inte generisk lösning. Hårdkoda för nu,
     abstrahera när vi har fler use cases."
```

### Inkonsistens
```
❌ Olika patterns för samma sak i olika filer

Problemet: Svårt att underhålla.
Fix: "Kolla hur vi gör detta i OrderService och
     följ samma mönster."
```

## Säkerhetsfel

### Lita på agent för security
```
❌ "Agenten har kollat säkerheten"

Problemet: Agenter kan missa subtila sårbarheter.
Fix: Alltid manuell security review för kritisk kod.
```

### Exponera secrets
```
❌ Paste:a API-nycklar i chatten för "kontext"

Problemet: Secrets i prompts är säkerhetsrisk.
Fix: Använd environment variables, referera till
     "API-nyckeln i .env"
```

## Process-fel

### Hoppa över steg
```
❌ Direkt till implementation utan plan

Problemet: Fel approach fastnar, måste göras om.
Fix: /plan först, även för "enkla" uppgifter.
```

### Ingen commit-hygien
```
❌ En gigantisk commit med alla ändringar

Problemet: Svårt att reviewa, svårt att rollback.
Fix: Små, fokuserade commits efter varje logisk ändring.
```

### Skippa tester
```
❌ "Det fungerar, vi skippar tester"

Problemet: Hooks blockerar (korrekt), eller buggar i prod.
Fix: TDD är inte optional. /tdd från start.
```

## Recovery

### När det gått fel

1. **Pausa** - Sluta generera mer kod
2. **Analysera** - Vad gick fel? Instruktion? Granskning?
3. **Kommunicera** - Berätta för agenten vad som var fel
4. **Återställ** - Git reset om nödvändigt
5. **Försök igen** - Med bättre instruktioner

```
Du: "Stopp. Detta blev fel. Vi tog fel approach.
     Låt oss börja om. [ny, bättre instruktion]"
```

---

Nästa: [Context-Management](Context-Management.md)
