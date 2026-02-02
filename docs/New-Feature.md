# New Feature Workflow

Hur du bygger en ny feature med agent-assistans.

## Översikt

```
1. Planering    →  2. TDD Red     →  3. TDD Green   →  4. Review     →  5. Ship
   (Agent+Du)       (Agent)           (Agent)           (Agent+Du)       (Du)
```

## Steg för steg

### 1. Planering

**Mål**: Tydlig spec innan kod skrivs.

```
/plan Implementera [feature] enligt [krav]
```

Agenten kommer:
- Analysera befintlig kod för kontext
- Föreslå implementation approach
- Identifiera filer som behöver ändras
- Flagga potentiella risker

**Du granskar**:
- Stämmer approachen med arkitekturen?
- Saknas något i analysen?
- Är scopet rätt?

### 2. TDD Red Phase

**Mål**: Tester som definierar korrekt beteende.

```
/tdd Skriv tester för [feature] enligt planen
```

Agenten skriver failing tests som specificerar:
- Happy path
- Edge cases
- Error handling

**Du granskar**:
- Täcker testerna rätt scenarios?
- Är assertions meningsfulla?
- Saknas viktiga test cases?

### 3. TDD Green Phase

**Mål**: Minimal implementation som får testerna att passera.

```
Fortsätt - implementera så testerna passerar
```

Agenten:
- Skriver implementation
- Kör tester iterativt
- Justerar tills grönt

**Hooks validerar automatiskt**:
- Kodstil
- Arkitekturmönster
- Säkerhetsregler
- Test coverage (80%+)

### 4. Review

**Mål**: Kvalitetssäkra innan merge.

```
/code-review
```

Agenten gör första pass:
- Kodkvalitet
- Mönsterefterlevnad
- Potentiella buggar

**Du gör final review**:
- Affärslogik korrekt?
- Edge cases hanterade?
- Läsbar och underhållbar?

För säkerhetskänslig kod:
```
/security-review
```

### 5. Ship

**Mål**: Säker deploy till produktion.

1. Skapa PR med beskrivning
2. CI/CD kör automatiska checks
3. Manuell QA om tillämpligt
4. Merge och deploy

## Exempel: User Registration

```
User: /plan Lägg till user registration med email verification

Agent: [Analyserar kodbas...]

## Implementation Plan

### Scope
- POST /api/users/register endpoint
- Email verification flow
- User entity utökning

### Filer att ändra
- UserController.cs (ny endpoint)
- UserService.cs (registreringslogik)
- EmailService.cs (verification email)
- User.cs (nya fält)

### Approach
1. Utöka User med EmailVerified, VerificationToken
2. Register endpoint skapar user med unverified status
3. Skicka verification email via befintlig EmailService
4. Verify endpoint validerar token och aktiverar user

### Risker
- Rate limiting behövs på register endpoint
- Token expiry behöver hanteras

Godkänn för att fortsätta med TDD?
```

## Tips

### Ge kontext
Istället för:
> "Lägg till login"

Skriv:
> "Lägg till login med JWT tokens. Vi använder redan JwtService för andra endpoints. Tokens ska ha 24h expiry."

### Iterera i små steg
Stora features → bryt ner i mindre delar → implementera en i taget.

### Lita på hooks
Om hooks klagar, lyssna. De finns där av en anledning.

### Pausa och granska
Även om agenten kan köra hela flödet – pausa mellan stegen och granska.

---

Nästa: [Bug-Investigation](Bug-Investigation.md)
