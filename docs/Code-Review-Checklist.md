# Code Review Checklist

Vad du ALLTID ska kontrollera vid granskning av agent-genererad kod.

## Kritiskt (blockerar merge)

### Säkerhet
- [ ] **Ingen SQL injection** – Parametriserade queries överallt
- [ ] **Ingen XSS** – Output encoding i frontend
- [ ] **Auth/authz på plats** – Rätt användare får rätt data
- [ ] **Secrets hanterade** – Inget hårdkodat, Key Vault används
- [ ] **Input validerat** – Aldrig lita på user input

### Korrekthet
- [ ] **Löser rätt problem** – Matchar faktiskt kravet
- [ ] **Edge cases** – Null, empty, max values hanterade
- [ ] **Error handling** – Fel fångas och hanteras vettigt
- [ ] **Transaktioner** – Data-integritet vid fel

### Arkitektur
- [ ] **Rätt lager** – Affärslogik i service, inte controller
- [ ] **Inga läckor** – DTOs ut, entities stannar inne
- [ ] **Dependency direction** – Inga cirkulära beroenden

## Viktigt (bör fixas)

### Underhållbarhet
- [ ] **Läsbar** – Förstår du koden direkt?
- [ ] **Namngivning** – Beskrivande, konsekvent
- [ ] **Komplexitet** – Motiverad eller överengineered?
- [ ] **DRY** – Duplicering som borde extraheras?

### Testning
- [ ] **Rätt saker testas** – Beteende, inte implementation
- [ ] **Meningsfulla assertions** – Inte bara "runs without error"
- [ ] **Edge cases täckta** – Inte bara happy path

### Performance
- [ ] **O(n) eller bättre** – Inga dolda O(n²) loopar
- [ ] **Databasfrågor** – Inga N+1, onödiga queries
- [ ] **Minnesanvändning** – Streamar stora datamängder

## Nice-to-have (notera för framtiden)

- [ ] Dokumentation uppdaterad
- [ ] Logging för debugging
- [ ] Metrics/monitoring
- [ ] Feature flag för gradvis utrullning

## Snabbguide

### Green flags (troligen OK)
- Följer existerande mönster i kodbasen
- Har tester som verifierar beteende
- Agenten förklarade resonemanget
- Hooks passerade utan varningar

### Red flags (kräver extra granskning)
- "Jag är inte säker på..." i agentens svar
- Ny pattern som inte finns i kodbasen
- Komplex logik utan tester
- Security-relaterad kod
- Databasmigration eller schemaändring

### Frågor att ställa
1. "Vad händer om X är null?"
2. "Vad händer vid concurrent access?"
3. "Vad händer om external service är nere?"
4. "Kan en illvillig användare missbruka detta?"

## Reject-kriterier

Neka merge om:
- Du inte förstår vad koden gör
- Säkerhetsproblem finns
- Tester saknas för kritisk logik
- Arkitekturen bryts
- Koden "luktar" fel även om du inte kan peka exakt

**Ditt ansvar**: Om du godkänner, ansvarar du för koden.

---

Nästa: [Effective-Instructions](Effective-Instructions.md)
