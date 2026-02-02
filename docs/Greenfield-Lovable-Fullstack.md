# Greenfield Fullstack med Lovable + Claude Code

Beprövat workflow för nya frontend+backend projekt.

## Översikt

```
1. Lovable.dev    →  2. Synka        →  3. Setup       →  4. Claude Code
   (Frontend)         (GitHub)          (Lokalt)          (Backend)
```

**Filosofi**: Frontend först, feature för feature, sedan backend.

## Steg 1: Frontend i Lovable.dev

Bygg hela frontend-MVP:en i Lovable innan du rör backend.

### Gör:
- **En vy i taget** – Gör klart varje sida innan du går vidare
- **Validering** – Se till att formulär etc fungerar med mock-data
- **Responsivitet** – Testa olika skärmstorlekar
- **Alla vyer** – Bygg HELA MVP-scopet

### Undvik:
- **One-shot hela appen** – Fungerar sällan
- **"Fix everything for X"** – För brett scope
- **Hoppa mellan features** – Slutför en sak först

### Praktiskt i Lovable

```
Prompt 1: "Skapa en login-sida med email och lösenord.
          Visa felmeddelanden vid validering."

[Testa, iterera tills det fungerar]

Prompt 2: "Skapa dashboard-sidan som visar efter login.
          Visa användarens namn och en lista med items."

[Testa, iterera tills det fungerar]

... fortsätt feature för feature
```

## Steg 2: Synka till GitHub

När frontend är klar:

1. Koppla Lovable till ditt GitHub-konto
2. Synka projektet till ett repo
3. Klona ner lokalt

```bash
git clone https://github.com/your-org/project-name.git
cd project-name
```

## Steg 3: Lokal setup

### Skapa projektstruktur

```
project-name/
├── CLAUDE.md                    # Från Wizardworks template
├── .claude/                     # Från Wizardworks template
│   ├── agents/
│   ├── commands/
│   ├── hooks/
│   └── ...
├── docs/
│   └── lovable-frontend/        # Lovable-koden för referens
│       ├── src/
│       ├── components/
│       └── ...
├── src/                         # Backend (byggs med Claude)
│   ├── api/
│   ├── services/
│   └── ...
└── frontend/                    # Lovable-frontend (eller i src/)
```

### Kommando

```bash
# Skapa docs-mapp och kopiera Lovable-kod
mkdir -p docs/lovable-frontend
cp -r src/* docs/lovable-frontend/

# Hämta Wizardworks Claude-template
# (kopiera innehållet från claude-code/template/)
```

## Steg 4: Claude Code tar över

### Första sessionen

```
User: Gå igenom docs/lovable-frontend för att förstå frontend-strukturen.
      Identifiera:
      - Vilka API-endpoints behövs
      - Vilka datamodeller krävs
      - Autentiseringsflöde

      Skapa sedan en implementation plan.
```

Claude kommer:
- Analysera alla komponenter
- Hitta API-anrop (även mockade)
- Förstå dataflöden
- Föreslå backend-arkitektur

### Iterera feature för feature

```
User: Börja med auth. Implementera:
      1. POST /api/auth/login
      2. POST /api/auth/register

      Matcha det frontend förväntar sig i docs/lovable-frontend.
```

Följ sedan [New-Feature](New-Feature.md) workflow för varje del.

## Fördelar med detta approach

### Frontend-first
- Användaren ser värde direkt (UI finns)
- API-kontrakt definieras av faktiskt behov
- Lättare att demo:a och få feedback

### Feature-isolation
- Varje del funkar innan nästa börjar
- Lättare att felsöka
- Tydlig progress

### Dokumentation inbyggd
- Lovable-koden i docs/ fungerar som spec
- Claude kan alltid referera tillbaka
- Inga missförstånd om vad frontend behöver

## Vanliga fallgropar

### "Jag fixar det sen"
```
❌ Halvfärdig login → Halvfärdig dashboard → Kaos

✅ Login 100% klar → Dashboard 100% klar → Fungerar
```

### "Lovable kan göra backend också"
```
❌ Blandar Lovable-genererad backend med egen

✅ Lovable = frontend only, Claude Code = backend
```

### "Jag behöver inte docs/"
```
❌ Tar bort Lovable-koden efter setup

✅ Behåll som referens – Claude behöver den
```

## Checklista

### Innan du lämnar Lovable:
- [ ] Alla MVP-vyer byggda
- [ ] Formulär validerar korrekt
- [ ] Navigation fungerar
- [ ] Responsivt på mobil/tablet/desktop
- [ ] Inga kvarvarande errors

### Efter lokal setup:
- [ ] Wizardworks template på plats
- [ ] Lovable-kod i docs/
- [ ] Git-repo initierat
- [ ] Claude kan läsa och förstå strukturen

### Under Claude Code-fas:
- [ ] En feature i taget
- [ ] Matcha exakt vad frontend förväntar
- [ ] Testa integration löpande

---

Se även: [New-Feature](New-Feature.md), [When-Agents-When-Manual](When-Agents-When-Manual.md)
