# When Agents, When Manual

Guide för att välja rätt approach för olika uppgifter.

## Använd agent för:

### Bra fit
- **CRUD-endpoints** – Standardmönster som agenten kan perfekt
- **Testskrivning** – Speciellt efter implementation (TDD green phase)
- **Refaktorering** – Mekaniska ändringar som renames, extrahering
- **Dokumentation** – Uppdatera docs baserat på kodändringar
- **Boilerplate** – Setup, config, repetitiv struktur
- **Research** – "Hur fungerar X i denna kodbas?"
- **Debugging** – Systematisk felsökning med logganalys
- **Code review** – Första pass för att hitta uppenbara issues

### Perfekt fit
- Uppgifter med tydliga mönster att följa
- Repetitivt arbete
- Väldefinierade krav
- Kod som liknar existerande kod i projektet

## Gör manuellt:

### Kräver människa
- **Arkitekturbeslut** – Systemdesign, val av tekniker
- **Affärslogik** – Komplex domänlogik som kräver djup förståelse
- **Säkerhetskritisk kod** – Auth, kryptering, access control
- **UX-beslut** – Interaktionsdesign, användarbeteende
- **Kommunikation** – Stakeholder-dialog, kravfångst

### Bättre manuellt
- Mycket små ändringar (snabbare att bara göra)
- Kod du behöver förstå djupt
- Explorativt arbete utan tydligt mål
- Känsliga operationer (databas-migrations i prod)

## Gråzon

Dessa beror på situation:

| Uppgift | Agent om... | Manuellt om... |
|---------|-------------|----------------|
| Ny feature | Tydlig spec finns | Kräver discovery |
| Bug fix | Reproducerbar, isolerad | Komplex, systemövergripande |
| Performance | Mätbar, specifik | Kräver profiling och analys |
| Migration | Mekanisk transformation | Kräver dataintegritet-beslut |

## Beslutsträd

```
Har uppgiften ett tydligt mönster att följa?
├─ Ja → Finns det i vår kodbas redan?
│       ├─ Ja → AGENT (hög confidence)
│       └─ Nej → AGENT med extra granskning
└─ Nej → Kräver det affärsbeslut?
         ├─ Ja → MANUELLT (eller agent för research)
         └─ Nej → AGENT för exploration, sedan beslut
```

## Kombinera

Många uppgifter är bäst som kombination:

1. **Agent researchar** → Du fattar beslut → **Agent implementerar**
2. **Du skissar arkitektur** → **Agent fyller i detaljer**
3. **Agent skriver första version** → Du refaktorerar kritiska delar
4. **Agent gör code review** → Du granskar agentens findings

---

Nästa: [New-Feature](New-Feature.md)
