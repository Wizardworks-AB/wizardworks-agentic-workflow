# Effective Instructions

Hur du skriver instruktioner som ger bra resultat.

## Grundprinciper

### 1. Var specifik
```
❌ "Lägg till login"
✅ "Lägg till login endpoint med JWT. Använd befintlig JwtService.
    Token ska ha 24h expiry. Returnera 401 vid fel credentials."
```

### 2. Ge kontext
```
❌ "Fixa bugg i OrderService"
✅ "GetCustomerOrders timeoutar för kunder med >100 orders.
    Stack trace: [paste]. Problemet är troligen paginering."
```

### 3. Specificera output
```
❌ "Skriv tester"
✅ "Skriv unit tests för UserService.Register som täcker:
    - Lyckad registrering
    - Duplicate email
    - Invalid email format"
```

### 4. Begränsa scope
```
❌ "Refaktorera systemet"
✅ "Extrahera email-validering från UserService till EmailValidator.
    Uppdatera alla anrop. Behåll samma beteende."
```

## Mönster som fungerar

### Problem → Kontext → Krav

```
Problem: Användare kan registrera sig med ogiltig email.
Kontext: Vi använder FluentValidation för andra endpoints.
Krav: Lägg till email-validering i RegisterUserDto med samma mönster.
```

### Gör X som Y

```
Lägg till en GetOrderById endpoint som följer samma mönster
som GetCustomerById i CustomerController.
```

### Steg-för-steg

```
1. Skapa DTO för request/response
2. Lägg till endpoint i controller
3. Implementera service-metod
4. Skriv tester

Kör /tdd för varje steg.
```

## Vad som hindrar bra resultat

### Vaga instruktioner
```
❌ "Gör det bättre"
❌ "Fixa problemen"
❌ "Optimera"
```

### Motsägande krav
```
❌ "Gör det enkelt men hantera alla edge cases"
❌ "Snabbt men med full testtäckning"
```

### För stort scope
```
❌ "Bygg hela autentiseringssystemet"
Bättre: Bryt ner i login, registration, password reset, etc.
```

### Saknad kontext
```
❌ "Använd rätt pattern"
✅ "Använd Repository pattern som i UserRepository"
```

## Justera under vägen

### Korrigera
```
User: Det ska vara soft delete, inte hard delete.
Agent: Förstår, uppdaterar implementation...
```

### Förtydliga
```
User: När jag sa "validera" menade jag FluentValidation, inte DataAnnotations.
Agent: Tack för förtydligandet. Refaktorerar...
```

### Begränsa
```
User: Vi behöver inte hantera concurrent updates just nu.
      Fokusera på basic CRUD först.
Agent: Ok, förenklar implementation...
```

### Expandera
```
User: Bra! Lägg också till logging för audit trail.
Agent: Lägger till structured logging...
```

## Templates

### Ny feature
```
Implementera [feature] som [beskrivning].

Kontext:
- [Relevanta befintliga system/services]
- [Affärsregler]

Krav:
- [Specifikt krav 1]
- [Specifikt krav 2]

Följ mönster från [referens i kodbasen].
```

### Bug fix
```
Bug: [Beskrivning av problemet]

Reproducera:
1. [Steg]
2. [Steg]

Förväntat: [Vad som borde hända]
Faktiskt: [Vad som händer]

Stack trace / logs:
[Paste]
```

### Refaktorering
```
Refaktorera [kod] för att [mål].

Constraints:
- Behåll samma externa API
- Inga breaking changes
- [Andra begränsningar]

Approach: [Om du har preferens]
```

---

Nästa: [Context-Management](Context-Management.md)
