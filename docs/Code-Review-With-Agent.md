# Code Review With Agent

Effektiv kodgranskning med agent-assistans.

## Två-stegs review

```
1. Agent Review (automatisk)  →  2. Human Review (fokuserad)
```

Agenten hanterar mekanisk granskning, du fokuserar på det som kräver omdöme.

## Agent Review

```
/code-review
```

Agenten kollar:
- Följer koden våra mönster?
- Finns uppenbara buggar?
- Är testtäckningen tillräcklig?
- Saknas dokumentation?
- Finns duplicerad kod?
- Är namngivning konsekvent?

### Output

```
## Code Review: feature/user-registration

### Issues Found

**HIGH: Missing input validation**
- File: UserController.cs:34
- Email parameter not validated before use
- Recommendation: Add [EmailAddress] attribute

**MEDIUM: Inconsistent error handling**
- File: UserService.cs:45-52
- Some paths throw, others return null
- Recommendation: Standardize on Result<T> pattern

**LOW: Magic number**
- File: UserService.cs:67
- Hardcoded "24" for token expiry hours
- Recommendation: Extract to config

### Coverage
- New code: 85% covered
- Missing: Error paths in RegisterUser

### Passed Checks
✓ Follows Controller-Service-Repository pattern
✓ Uses DTOs for API contracts
✓ No hardcoded secrets detected
```

## Human Review

Med agentens rapport som grund, fokusera på:

### 1. Affärslogik
- Är reglerna korrekt implementerade?
- Fungerar det för alla användartyper?
- Hanteras edge cases?

### 2. Arkitektur
- Passar koden i övergripande design?
- Introduceras teknisk skuld?
- Är abstraktionsnivån rätt?

### 3. Säkerhet
- Input validation på rätt ställen?
- Authorization checks?
- Känslig data hanterad korrekt?

### 4. Läsbarhet
- Förstår du koden vid första genomläsning?
- Skulle en ny teammedlem förstå den?
- Är komplexiteten motiverad?

## Checklista

### Alltid kolla manuellt:
- [ ] Förstår jag vad koden gör?
- [ ] Löser den rätt problem?
- [ ] Är affärslogiken korrekt?
- [ ] Finns säkerhetsrisker?
- [ ] Kommer den vara underhållbar?

### Lita på agenten för:
- [ ] Kodstil och formatering
- [ ] Mönsterefterlevnad
- [ ] Uppenbara buggar
- [ ] Testtäckning

## Ge feedback

Om agenten missade något:

```
User: Du missade att RegisterUser inte hanterar
      fallet där email redan finns.
```

Agenten lär sig av feedback inom sessionen.

## PR Review Workflow

1. Författare kör `/code-review` innan PR skapas
2. Fixar issues som hittas
3. Skapar PR med agentens rapport inkluderad
4. Reviewer fokuserar på mänsklig granskning
5. Agent kan hjälpa reviewer med frågor

---

Nästa: [Code-Review-Checklist](Code-Review-Checklist.md)
