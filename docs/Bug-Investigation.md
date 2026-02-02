# Bug Investigation

Systematisk felsökning med agent-assistans.

## Workflow

```
1. Reproduce  →  2. Locate  →  3. Understand  →  4. Fix  →  5. Verify
   (Du)           (Agent)      (Agent+Du)        (Agent)    (Agent+Du)
```

## Steg för steg

### 1. Reproduce

**Du beskriver** problemet med så mycket kontext som möjligt:
- Vad händer? (faktiskt beteende)
- Vad borde hända? (förväntat beteende)
- Steg för att reproducera
- Felmeddelanden, stack traces, loggar

```
User: Vi får 500-error på /api/orders när customer har fler än 100 orders.
      Stack trace: [paste]
      Händer bara i produktion, inte lokalt.
```

### 2. Locate

**Agenten söker** igenom kodbasen:

```
Agent: [Analyserar stack trace och kodbas...]

Problemet verkar vara i OrderService.GetCustomerOrders():
- Rad 45: Hämtar alla orders utan paginering
- Rad 52: Mappar till DTOs i minnet
- Vid 100+ orders timeout/OOM

Relaterade filer:
- OrderService.cs:45-60
- OrderRepository.cs:23
- OrderController.cs:18
```

### 3. Understand

**Tillsammans gräver ni djupare**:

```
User: Varför funkar det lokalt?

Agent: Lokalt har testkunder max 10 orders.
       I prod har vissa kunder 500+ orders.
       Problemet är O(n) minneskomplexitet.
```

**Du validerar**:
- Stämmer analysen?
- Finns det fler ställen med samma problem?
- Vad är root cause vs symptom?

### 4. Fix

**Agenten implementerar** med TDD:

```
User: Fixa med paginering. Max 50 per request.

Agent: /tdd Lägg till paginering i GetCustomerOrders
       [Skriver test för paginering]
       [Implementerar]
       [Verifierar med hooks]
```

### 5. Verify

**Ni verifierar tillsammans**:
- Kör nya testerna
- Testa manuellt med samma scenario
- Kolla efter regression

```
User: Testa med en kund som har 150 orders

Agent: [Kör test...]
       Request 1: orders 1-50, 45ms
       Request 2: orders 51-100, 43ms
       Request 3: orders 101-150, 41ms
       Alla returnerar korrekt data.
```

## Vanliga bugtyper

### Performance
- **Symtom**: Långsamma requests, timeouts
- **Agent bra på**: Hitta O(n²) loopar, missing indexes, N+1 queries
- **Du kollar**: Är optimering värd komplexiteten?

### Data issues
- **Symtom**: Felaktig data, inkonsistens
- **Agent bra på**: Spåra dataflöde genom systemet
- **Du kollar**: Affärsregler, edge cases

### Integration
- **Symtom**: Fungerar isolerat men inte tillsammans
- **Agent bra på**: Analysera kontrakt, timing, dependencies
- **Du kollar**: Är det vårt system eller externt?

### Security
- **Symtom**: Obehörig åtkomst, data leakage
- **Agent bra på**: Hitta sårbarheter i kod
- **Du kollar**: ALLTID manuell granskning av security fixes

## Anti-patterns

### "Fixa felet"
Dåligt: Ber agenten fixa utan att förstå.
Bra: Analysera först, förstå sedan, fixa sist.

### "Det fungerar nu"
Dåligt: Acceptera fix utan att förstå varför.
Bra: Förstå både problemet och lösningen.

### "Lägg till try-catch"
Dåligt: Dölja symptom istället för att fixa root cause.
Bra: Hantera fel på rätt nivå efter att root cause är fixad.

## Komplexa buggar

För buggar som kräver djupare analys:

```
User: Jag behöver förstå hela flödet från request till database
      för denna endpoint. Stega igenom det.
```

Agenten kan:
- Visa call stack
- Förklara varje steg
- Identifiera var data transformeras
- Peka ut potentiella problem

---

Nästa: [Code-Review-With-Agent](Code-Review-With-Agent.md)
