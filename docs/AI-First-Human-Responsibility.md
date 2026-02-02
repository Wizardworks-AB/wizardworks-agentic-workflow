# AI-First Human Responsibility

Wizardworks grundprincip för agentisk utveckling.

## Vad betyder det?

**AI-first**: Vi använder AI-agenter som förstahandsval för de flesta utvecklingsuppgifter. Inte som komplement eller experiment, utan som primärt verktyg.

**Human responsibility**: Ansvaret för kvalitet, säkerhet, arkitektur och affärsbeslut ligger alltid hos människan. Agenten är ett kraftfullt verktyg, men du är ansvarig för resultatet.

## Praktiskt

### Agenten ansvarar för:
- Kodgenerering enligt etablerade mönster
- Testskrivning och implementation
- Dokumentationsuppdateringar
- Refaktorering inom definierade ramar
- Research och kunskapsinhämtning
- Repetitiva uppgifter

### Du ansvarar för:
- Arkitekturbeslut och systemdesign
- Säkerhetsgranskning av kritisk kod
- Affärslogik och domänförståelse
- Kvalitetskontroll av agentens output
- Beslut om när agenten har fel
- Kommunikation med stakeholders

## 70-20-10 regeln

Som tumregel:
- **70%** – Agenten producerar (kod, tester, docs)
- **20%** – Du styr och guidar (instruktioner, korrektioner)
- **10%** – Du fixar och finputsar (edge cases, integration)

Detta är inte en strikt regel utan en mental modell. Vissa uppgifter är 90-5-5, andra 50-30-20.

## Varför inte 100% agent?

1. **Kontext**: Agenten saknar fullständig affärskontext
2. **Ansvar**: Någon måste stå för besluten
3. **Kvalitet**: Mänsklig granskning fångar subtila fel
4. **Lärande**: Du behöver förstå koden du ansvarar för

## Varför inte 0% agent?

1. **Hastighet**: Agenter är snabbare på rutinarbete
2. **Konsistens**: Följer mönster utan att tröttna
3. **Kunskapsbredd**: Kan fler tekniker än en enskild utvecklare
4. **Fokus**: Frigör tid för det som kräver mänskligt omdöme

## Anti-patterns

### "Agenten sa så"
Aldrig en giltig ursäkt. Du granskade och godkände.

### "Jag förstår inte koden men den fungerar"
Om du inte förstår den, ska den inte mergas.

### "Det går snabbare att låta agenten göra allt"
Kortsiktigt ja. Långsiktigt nej – du tappar förståelse och kontroll.

### "Agenten kan inte göra detta"
Ofta fel. Prova med bättre instruktioner först.

---

Nästa: [When-Agents-When-Manual](When-Agents-When-Manual.md)
