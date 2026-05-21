---
type: concept
scope: shared
domain: hermes
sensitivity: internal
subject_user: null
confidence: high
evidence_count: 1
last_confirmed: 2026-05-21
review_after: 2026-08-21
retention: canonical
supersedes: []
superseded_by: []
tags: [hermes, knowledge-system, user-memory, multi-user, architecture, pkm]
---

# Knowledge-System: Architektur und User-Ebene

Dieses Dokument beschreibt vollständig wie Wissen im Hermes-System gespeichert wird —
auf Systemebene, auf App-Ebene (Bot-Profil) und auf User-Ebene (einzelne Person).
Es gilt als Referenz für alle zukünftigen Erweiterungen und neue Apps.

---

## 1. Grundprinzip: Drei Schichten

```
┌─────────────────────────────────────────────────────────────┐
│  SCHICHT 1 — KRYSTOFS PERSÖNLICHES WISSEN                   │
│  /root/obsidian-vault/  (Git-versioniert)                   │
│  Lesegerät: Obsidian auf Mac / iPhone                       │
│  Schreiber: Hermes (default-Profil), manuell                │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│  SCHICHT 2 — APP-WISSEN (pro Bot-Profil)                    │
│  ~/.hermes/profiles/<app>/memories/MEMORY.md                │
│  Geteilt über alle User dieser App                          │
│  z.B. Coaching-Methodiken, Rezept-Workflows, Bot-Rules      │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│  SCHICHT 3 — USER-WISSEN (pro Person pro App)               │
│  ~/.hermes/profiles/<app>/memories/users/<telegram_id>/     │
│  Privat, isoliert, nur für diesen User in dieser App        │
│  USER.md + domain-spezifische Unterordner                   │
└─────────────────────────────────────────────────────────────┘
```

Jede Schicht ist unabhängig. Ein User-Profil in Kratos beeinflusst
nicht sein User-Profil im Rezepte-Bot oder Hermes-Trader.

---

## 2. Schicht 1: Krystofs persönliches Vault

### Zweck

Das Obsidian-Vault ist Krystofs **persönliches Second Brain** —
Wissen das über alle Apps und Sessions hinweg Bestand hat.
Es akkumuliert sich über Zeit und wächst durch Agent-Arbeit.

### Pfad & Zugriff

```
Server:  /root/obsidian-vault/
Sync:    Git → GitHub: KryTisch/krystofs-hermes-agent
Lesen:   Obsidian auf Mac (git pull), iPhone (Working Copy App)
Schreiben: Hermes default-Profil, manuell via SSH
```

### Struktur (vereinfacht)

```
/root/obsidian-vault/
├── 00_Inbox/          schnelle Eingänge, unverarbeitet
├── 01_Personal/       persönliche Langzeitnotizen
├── 10_Episodes/       verdichtete Session-Zusammenfassungen
├── 20_People/         anonymisierte User-Muster (KEIN PII)
├── 30_Projects/       Projekt-Hubs (Hermes, Kratos, Rezepte...)
├── 40_Knowledge/      langlebige Fakten, Konzepte, Entscheidungen
├── 50_Sources/        Quellen (Papers, Web, YouTube, Voice)
├── 60_Procedures/     Playbooks, wiederverwendbare Abläufe
├── 70_Queries/        Research-Ergebnisse, Vergleiche
├── 80_Outputs/        Reports, Health Checks
├── 90_System/         Schema, Lifecycle, Privacy, Taxonomy
└── Rezepte/           operativer Bot-Mirror (bewusst getrennt)
```

### Was hier landet (und was nicht)

| Landet hier | Landet NICHT hier |
|---|---|
| Krystofs persönliche Erkenntnisse | Rohe User-Chatverläufe |
| Coaching-Methodiken (anonymisiert) | Health-Daten einzelner Personen |
| Bot-Architektur-Entscheidungen | API-Keys, Tokens, Passwörter |
| Research-Ergebnisse, Papers | Nicht-anonymisierte Subuser-Profile |
| Projekt-Status, Procedures | Operative Echtzeit-Daten |

### Frontmatter-Pflicht

Jede Note trägt vollständiges Typed Frontmatter:
```yaml
type: fact|concept|episode|procedure|decision|query|policy|source|person|project
scope: personal|shared|domain|user_private
domain: hermes|kratos|rezepte|general
sensitivity: public|internal|private|health
confidence: low|medium|high
evidence_count: N
last_confirmed: YYYY-MM-DD
review_after: YYYY-MM-DD
retention: transient|working|durable|canonical
```

---

## 3. Schicht 2: App-Wissen (pro Bot-Profil)

### Zweck

Jede App (Bot-Profil) hat ein **geteiltes Gedächtnis** über alle ihre User.
Hier liegt Wissen das für alle Nutzer der App relevant ist:
Methodiken, Regeln, Vorlagen, Routing-Entscheidungen.

### Pfad

```
~/.hermes/profiles/<app>/memories/MEMORY.md
```

### Aktuelle Apps und ihr App-Wissen

```
kratos-coaching/memories/MEMORY.md
  → Coaching-Methodiken, Workout-Systematik, Reporting-Regeln
  → "Alle User profitieren von layout-stabilen Morgenberichten"
  → Modell-Routing-Strategie für Coaching-Kontext

rezepte-bot/memories/MEMORY.md
  → Rezept-Kategorisierung, Workflow-Regeln, Bot-Verhalten

hermes-trader/memories/MEMORY.md
  → Trading-Regeln (nur Krystof als User)

default/memories/MEMORY.md
  → Krystofs persönlicher Haupt-Agent
```

### Was hier landet

- Erkenntnisse die für ALLE User der App gelten
- Anonymisierte Muster (z.B. "manche User starten besser mit wenig Onboarding")
- App-spezifische Konfiguration und Verhaltensregeln
- Routing- und Eskalations-Logik

---

## 4. Schicht 3: User-Wissen (pro Person pro App)

### Zweck

Der **persönliche, private Wissensraum** eines Users innerhalb einer App.
Vollständig isoliert — Lars' Daten in Kratos sind für Katja unsichtbar,
und Lars' Kratos-Profil ist unabhängig von Lars' Rezepte-Bot-Profil.

### Pfad

```
~/.hermes/profiles/<app>/memories/users/<telegram_id>/USER.md
~/.hermes/profiles/<app>/memories/users/<telegram_id>/<domain>/
```

### Reale Struktur heute

```
kratos-coaching/memories/users/
├── 7746126677/          ← Krystof Tischer
│   ├── USER.md          ← Profil, Ziele, Präferenzen
│   └── nutrition/
│       └── meal_library.md
├── 1171427466/          ← Lars/Chris
│   ├── USER.md
│   ├── training_plan_current.md
│   ├── training_plan_bodyweight_backup.md
│   ├── workout_day3_completed_2026-05-14.md
│   └── reporting/
│       ├── tagesbericht_2026-05-16.md
│       ├── tagesbericht_2026-05-17.md
│       ├── tagesbericht_2026-05-18.md
│       ├── tagesbericht_2026-05-19.md
│       ├── morgencheck_layout_fixed.md
│       └── wochenbericht_layout_fixed.md
└── 8733691947/          ← Katja Tischer
    └── USER.md          (noch nicht vollständig onboarded)

rezepte-bot/memories/users/
├── 7746126677/          ← Krystof
│   └── USER.md
└── 1171427466/          ← Lars/Chris
    └── USER.md

hermes-trader/memories/users/
└── 7746126677/          ← nur Krystof
    └── USER.md
```

### USER.md Inhalt (Aufbau)

Jedes USER.md enthält:
- Identität (Telegram-ID, Name, Sprache)
- Ziele und Kontext (sport, ernährung, etc.)
- Präferenzen (Kommunikationsstil, Berichtsformat)
- Constraints (Verletzungen, Unverträglichkeiten, Zeitzonen)
- Aktueller Stand (aktive Phase, letzte Interaktion)
- Bekannte Vorlieben und Abneigungen

---

## 5. Was passiert wenn ein User eine App benutzt

### Ablauf (Session-Start)

```
User schickt Telegram-Nachricht
          │
          ▼
Gateway empfängt (identifiziert user via telegram_id)
          │
          ▼
Hermes lädt in dieser Reihenfolge:
  1. App-MEMORY.md    → Was gilt für alle User dieser App?
  2. User-USER.md     → Wer ist dieser User konkret?
  3. User-Subfiles    → z.B. training_plan_current.md
  4. SOUL.md          → Persönlichkeit und Grundregeln des Bots
          │
          ▼
Konversation mit vollem Kontext über diesen User
          │
          ▼
Session-Ende: Agent schreibt Updates zurück
  → USER.md (neue Erkenntnisse, Fortschritt)
  → Domain-Files (z.B. neuer Tagesbericht, Update Trainingsplan)
  → App-MEMORY.md (wenn App-weite Erkenntnis)
```

### Konkretbeispiel: Lars nutzt Kratos-Coaching

```
Lars öffnet Telegram, schreibt: "Ich hab heute Rückenprobleme"
          │
          ▼
Kratos-Gateway lädt:
  MEMORY.md:    "Morgenberichte sind layout-stabil zu halten"
  USER.md:      "Lars, 1171427466, Bodyweight-Training Phase 2,
                 Morgenbriefing 05:00, will knappes Deutsch"
  plan_current: "Woche 3, Tag 4 — Beine und Core"
          │
          ▼
Kratos antwortet mit Kontext: modifiziert Plan wegen Rücken,
schlägt alternative Übungen vor, dokumentiert in USER.md
          │
          ▼
Tagesbericht wird um 05:00 automatisch generiert und als
Voice-Nachricht + Text gesendet (Cron-Job)
```

---

## 6. Ein User, mehrere Apps — wie funktioniert das?

### Das Kernprinzip: Vollständige Isolation zwischen Apps

Lars (Telegram-ID `1171427466`) hat:
- `kratos-coaching/memories/users/1171427466/` — sein Coaching-Profil
- `rezepte-bot/memories/users/1171427466/` — sein Rezepte-Profil

**Diese beiden Profile teilen sich NICHTS automatisch.**

Der Rezepte-Bot weiß nicht, dass Lars Knieschmerzen hat.
Der Kratos-Bot weiß nicht, welche Rezepte Lars favorisiert.

### Warum diese Trennung?

- **Privacy by Design:** User hat klare Erwartungen pro App
- **Fokus:** Jede App ist Experte in ihrer Domain
- **Datensparsamkeit:** Keine ungewollte Cross-Contamination

### Wenn Cross-App-Wissen gewünscht ist

Wenn Lars dem Rezepte-Bot sagt "ich mache Kraftsport",
schreibt der Rezepte-Bot das in sein eigenes USER.md.
Eine automatische Übernahme aus Kratos gibt es **nicht** —
das ist bewusst.

Ausnahme: Krystof kann als Admin manuell Wissen transferieren
oder eine App kann via SSH-Befehl/Script koordiniert werden.

### Szenario: Lars bekommt drei Apps

```
Lars (1171427466) nutzt:
  kratos-coaching  → Fitness & Coaching
  rezepte-bot      → Ernährung & Rezepte
  [neue-app]       → z.B. Lern-Coach oder Produktivitäts-Bot
```

Beim Onboarding in jeder neuen App:
1. Neue USER.md wird bei erster Interaktion angelegt
2. Bot begrüßt User, stellt Onboarding-Fragen
3. Antworten werden in USER.md gespeichert
4. Spezifische Domain-Files entstehen im Laufe der Nutzung

Keine Synchronisation zwischen den drei Profilen
— außer Krystof entscheidet das explizit.

---

## 7. Krystof als Sonderfall: Admin + User

Krystof (Telegram-ID `7746126677`) ist in jeder App gleichzeitig:
- **User:** hat eigene USER.md pro App
- **Admin:** voller SSH-Zugriff, kann alle User-Profile lesen/ändern
- **Wissens-Eigentümer:** sein persönliches Vault akkumuliert
  Meta-Wissen über alle Apps

```
Krystof in kratos:  Trainingsplan, Nutrition-Library
Krystof in rezepte: Lieblings-Rezepte, Präferenzen
Krystof in trader:  Trading-Strategien (nur er hat Zugriff)
Krystof im Vault:   Übergreifende Erkenntnisse, Architektur,
                    anonymisierte Muster aus allen Apps
```

---

## 8. Wissens-Lifecycle: Von Interaktion zu Vault

```
USER fragt/agiert
     │
     ▼
SESSION-WISSEN (flüchtig, im Kontext-Fenster)
     │
     ▼ (Session-Ende, Agent schreibt zurück)
USER-FILES (persistiert, pro User pro App)
     │
     ▼ (wenn Erkenntnis app-weit relevant)
APP-MEMORY.md (anonymisiert, für alle User der App)
     │
     ▼ (wenn Erkenntnis system-weit relevant)
OBSIDIAN-VAULT (kuratiert, Krystofs Second Brain)
     │
     ▼ (wenn Workflow wiederholbar)
HERMES SKILL (prozedurales Wissen, geladen per skill_view)
```

### Wer schreibt wohin?

| Schreiber | Ziel | Inhalt |
|---|---|---|
| Jede App | User-Files | Interaktions-Ergebnisse, Fortschritt |
| Jede App | App-MEMORY | Anonymisierte Muster |
| Krystofs Agent | Obsidian-Vault | Kuratiertes Wissen |
| Krystof manuell | Überall | Admin-Korrekturen |
| Cron-Jobs | User-Files | Berichte, Tagesupdates |

---

## 9. Automatisierung: Was passiert ohne Krystofs Eingriff

### Täglich
- Garmin-Daten werden für alle User abgerufen (06:15 UTC)
- Lars erhält Morgenbriefing um 05:00 Europe/Berlin
- Hermes Quick Backup (02:20 UTC)
- Wiki-Crystallize prüft Sessions auf vault-würdige Erkenntnisse (22:00 lokal)

### Wöchentlich
- Obsidian Vault Health-Check (Mo 07:00 UTC)
- Hermes Full Backup (So 02:35 UTC)
- Kratos-Profil-Export (So 02:45 UTC)

### On-demand (durch User-Interaktion)
- USER.md wird am Sessionende aktualisiert
- Domain-Files entstehen bei relevanten Ereignissen
- Vault-Commit nach manueller Curation

---

## 10. Erweiterung: neue App hinzufügen

Wenn eine neue App (z.B. `lern-coach`) entsteht:

```bash
# Auf Server:
hermes profile create lern-coach
# SOUL.md schreiben (Persönlichkeit, Regeln)
# MEMORY.md anlegen (App-weites Wissen)
# Telegram-Token in .env
# Gateway installieren
# TELEGRAM_ALLOWED_USERS setzen
```

Für jeden User der diese App nutzen soll:
- USER.md wird automatisch beim ersten Kontakt angelegt
- Oder: Krystof legt USER.md manuell vorab an (Pre-Onboarding)

---

## 11. Privacy-Zusammenfassung

| Was | Wo | Sichtbar für |
|---|---|---|
| Krystofs persönl. Wissen | Obsidian-Vault, `scope: personal` | nur Krystof |
| App-Methodiken | App-MEMORY.md, `scope: domain` | alle Agents der App |
| User-Daten | `memories/users/<id>/` | nur dieser User + Admin |
| Anonymisierte Muster | Vault `20_People/anonymized-patterns/` | alle Agents |
| Health-Daten | User-Files, niemals im Vault | nur dieser User + Admin |

Harte Regel: `scope: shared` + `sensitivity: health` = **verboten**.

---

## 12. Offene Punkte (Stand 2026-05-21)

- [ ] Git-Auto-Commit nach Vault-Sessions einrichten
- [ ] Vault auf Mac via `git clone` verfügbar machen
- [ ] Katja (8733691947) vollständig onboarden in Kratos
- [ ] Lars/Chris (1171427466) Klärung: eine Person oder zwei?
- [ ] Rezepte-Bot lsp-Binary bereinigen
- [ ] Überlegung: Lars bekommt Rezepte-Bot-Erweiterung (Ernährungs-Sync mit Kratos)?
- [ ] goAVA-AI Firmen-Server: Architektur analog aufbauen

## Links

- [[../../90_System/SCHEMA|SCHEMA]]
- [[../../90_System/PRIVACY-SCOPES|PRIVACY-SCOPES]]
- [[../../90_System/LIFECYCLE|LIFECYCLE]]
- [[../../30_Projects/Kratos/index|Kratos Projekt]]
- [[../../60_Procedures/hermes-ops/Wiki-Crystallization|Crystallization Procedure]]
