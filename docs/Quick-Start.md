# Quick Start

**TL;DR** — Så här jobbar Wizardworks med AI-agenter.

---

## Filosofi

**AI-first med mänskligt ansvar.** Agenter exekverar, människor beslutar.

---

## Övergripande flöde

```
Krav → Design → Implementation (TDD) → Review → Deploy → Produktion
```

| Fas | Vem gör vad |
|-----|------------|
| **Krav** | Människa leder kundsamtal, agent strukturerar |
| **Design** | Arkitekt beslutar, agent analyserar och föreslår |
| **Implementation** | Agent kodar (TDD), hooks validerar automatiskt |
| **Review** | Agent gör första pass, människa gör final review |
| **Deploy** | Människa triggar pipeline, aldrig direkt från terminal |
| **Produktion** | Agent övervakar och föreslår fix, människa godkänner |

---

## Rollfördelning

- **Arkitekt** — Styr riktning, fattar beslut, äger kvalitet
- **AI-agent** — Exekverar: kodar, testar, reviewar, dokumenterar
- **Människa** — Godkänner: varje steg kräver mänsklig validering

---

## Feature-storlek

Ju mindre task, desto bättre resultat. Bryt ner arbetet:
- En vy i taget
- En endpoint i taget
- Frontend först (mockdata) → Backend

---

## Kom igång

1. **Installera Claude Code**: `npm install -g @anthropic-ai/claude-code`
2. **Kopiera template**: Hämta `CLAUDE.md` + `.claude/` från [claude-code](https://github.com/Wizardworks-AB/claude-code) till ditt projekt
3. **Anpassa CLAUDE.md**: Lägg till projektnamn och kundspecifik kontext
4. **Kör `claude`** i projektroten
5. **Börja med `/plan`**: Planera din första feature

---

## Fördjupning

| Ämne | Dokument |
|------|----------|
| Komplett livscykel | [Development Lifecycle](Development-Lifecycle.md) |
| Feature-workflow | [New Feature](New-Feature.md) |
| Filosofi | [AI-First Human Responsibility](AI-First-Human-Responsibility.md) |
| Bugghantering | [Bug Investigation](Bug-Investigation.md) |
| Kodgranskning | [Code Review With Agent](Code-Review-With-Agent.md) |
| Verktyg per fas | [Tooling By Phase](Tooling-By-Phase.md) |
| Dokumentationsstandard | [Documentation Standard](Documentation-Standard.md) |
| Kundspecifik Claude | [Client Agent Setup](Client-Agent-Setup.md) |
| Superpowers-plugin | [Superpowers Integration](Superpowers-Integration.md) |

---

## Relaterade repos

- **[claude-code](https://github.com/Wizardworks-AB/claude-code)** — Template med agents, hooks, commands, skills
- **[patterns-and-practices](https://github.com/Wizardworks-AB/wizardworks-patterns-and-practices)** — Kodstandarder och referensimplementationer
