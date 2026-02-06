# Vision: AI-First Development

Vart Wizardworks är på väg med AI-first utveckling.

---

## Arkitekter som Orchestrators

Arkitektens roll förändras. Istället för att skriva kod orchestrerar arkitekten:

```
Kund ←→ Arkitekt ←→ AI-agenter
              ↓
        Kvalitetssäkring
        Arkitekturbeslut
        Kunddialog
```

**Arkitekten:**
- Leder kunddialoger och samlar krav
- Fattar arkitekturbeslut
- Styr agenternas arbete via CLAUDE.md, plans och tasks
- Granskar och godkänner output
- Äger kvalitet och leverans

**AI-agenter:**
- Exekverar: kodar, testar, dokumenterar, reviewar
- Rapporterar: flaggar risker, oklarheter, avvikelser
- Föreslår: lösningar, förbättringar, optimeringar

**Utvecklare (ej arkitekter):**
- Jobbar med agenter som par — guidar och granskar
- Fokuserar på domänkunskap och affärslogik
- Manuell QA och validering

---

## Gemensam Dashboard

**Vision:** En gemensam skärm på kontoret som visar:
- Alla aktiva kundprojekt
- Tasks per kund (pågående, blockerade, klara)
- Agent-aktivitet (vad jobbar agenterna med just nu?)
- Hälsostatus per system (grönt/gult/rött)

**Syfte:** Hela teamet ser vad som händer utan att fråga. Arkitekter kan snabbt se om ett kundprojekt behöver uppmärksamhet.

---

## Proaktiv Bugghantering

Idag: Kund rapporterar bugg → vi fixar.
Imorgon: Agent upptäcker problem → föreslår fix → staging → människa validerar → produktion.

### Long-Running Production Agent

**Koncept:** En agent som kontinuerligt övervakar kundprojekt i produktion:

```
Produktion
    ↓
Monitoring (Application Insights, Sentry, etc.)
    ↓
Agent analyserar loggar och metrics
    ↓
┌─ Nya fel detekterade?
│   ├─ Allvarliga → Alert till arkitekt + buggrapport
│   └─ Mindre → Samlar, rapporterar veckovis
│
├─ Performance-degradering?
│   └─ Analyserar root cause, föreslår optimering
│
├─ Säkerhetsvarningar?
│   └─ Dependency-uppdateringar, CVE-scanning
│
└─ Allt OK → Statusrapport
```

**Människa behövs fortfarande för:**
- Deploy-beslut (aldrig automatisk deploy till produktion)
- Kundkommunikation vid incidenter
- Prioritering av vad som fixas

**Status:** Vision — inte implementerat ännu. Kräver MCP-server eller liknande integration.

---

## Kontextfönster Växer

Kontextfönster förväntas växa kraftigt de närmaste åren. Implikationer:

- **Dokumentation blir mer användbar** — Claude kan ta in mer kontext per session
- **Längre sessioner** — Mindre risk för att kontext komprimeras bort
- **Hela kodbaser i kontext** — Agenten kan se allt samtidigt, inte navigera fil för fil
- **Rikare CLAUDE.md** — Mer utrymme för domänkunskap, historik, beslut

**Vad vi gör nu för att förbereda:**
- Dokumenterar allt i CLAUDE.md och memory (kommer bli mer värdefullt)
- Strukturerar kunskap så den är lätt att konsumera (korta, scanningsbara dokument)
- Bygger processer som skapar kontext automatiskt (transkribering, commit-dokumentation)

---

## Tidsfönster

**3–6 månaders försprång** innan konkurrenter kopierar AI-first-modellen.

- All ledig tid (utanför kundprojekt) ska läggas på att lära sig arbetsflödena
- Målet: etablera Wizardworks som ledare inom AI-first utveckling
- Repos (claude-code, wizardworks-agentic-workflow, patterns-and-practices) = vår "moat"
- Strukturkapital (dokumentation, processer, templates) är svårt att kopiera snabbt

---

Se även: [Development Lifecycle](Development-Lifecycle.md), [Quick Start](Quick-Start.md)
