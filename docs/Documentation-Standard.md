# Documentation Standard

Wizardworks standard för hur AI-agenter dokumenterar i projekt.

---

## Principer

1. **Brief** — Dokumentation ska vara kort och kärnfull. Den äter kontext.
2. **Per feature** — Dokumentera varje feature, inte hela systemet på en gång.
3. **Funktionellt + tekniskt** — Beskriv både vad och hur.
4. **Automatiserat** — Genereras som del av commit-flödet (via `/commit`).
5. **Strukturkapital** — Dokumentationen är en tillgång som skiljer oss från konkurrenter.

---

## Format

Alltid Markdown. Kort, scanbart, utan fluff.

---

## Feature-dokumentation (mall)

```markdown
# [Feature-namn]

## Vad
[1-2 meningar: vad gör denna feature för användaren?]

## Hur
[Kort teknisk beskrivning: vilka komponenter, endpoints, datamodell?]

## Filer
- `src/Services/FeatureService.cs` — Affärslogik
- `src/Controllers/FeatureController.cs` — API endpoint
- `tests/FeatureServiceTests.cs` — Tester

## Beslut
[Eventuella arkitekturbeslut och varför]
```

---

## Var dokumenteras det?

| Typ | Plats |
|-----|-------|
| Feature-dokumentation | `docs/features/` i projektets repo |
| API-dokumentation | Swagger/OpenAPI (autogenererad) |
| Arkitekturbeslut (ADR) | `docs/decisions/` i projektets repo |
| Runbooks / deploy-guider | `docs/operations/` i projektets repo |
| Mötesanteckningar | Projektets wiki eller `docs/meetings/` |

---

## När dokumenteras det?

- **Vid commit** — `/commit`-kommandot genererar kort feature-dokumentation automatiskt
- **Vid ny feature** — Steg 7 (DOCUMENT) i Wizardworks-workflow
- **Vid arkitekturbeslut** — ADR skrivs som del av designfasen
- **Vid deploy** — Release notes genereras

---

## Riktlinjer

### Gör
- Skriv för den som kommer efter dig (ny arkitekt, ny utvecklare)
- Fokusera på "varför" snarare än "vad" (koden visar vad)
- Håll det uppdaterat — föråldrad dokumentation är värre än ingen

### Gör inte
- Skriv inte romaner — 5-10 rader per feature räcker
- Dokumentera inte självklarheter (getters, setters, boilerplate)
- Duplicera inte information som finns i koden (JSDoc, XML comments)
- Generera inte dokumentation som ingen kommer läsa

---

## Integration med `/commit`

`/commit`-kommandot i claude-code inkluderar automatisk dokumentationsgenerering:

1. Analyserar ändringarna i committen
2. Genererar kort feature-dokumentation (om relevant)
3. Sparar i `docs/features/` eller uppdaterar befintlig

Se [claude-code](https://github.com/Wizardworks-AB/claude-code) för `/commit`-kommandots implementation.

---

Se även: [Development Lifecycle](Development-Lifecycle.md), [New Feature](New-Feature.md)
