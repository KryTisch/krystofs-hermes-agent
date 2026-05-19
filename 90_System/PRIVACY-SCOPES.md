---
type: policy
scope: shared
domain: general
sensitivity: internal
subject_user: null
confidence: high
evidence_count: 1
last_confirmed: 2026-05-19
review_after: 2026-08-19
retention: canonical
supersedes: []
superseded_by: []
tags: [privacy, scopes, policy, obsidian]
---

# Privacy Scopes

Diese Note definiert die harten Sichtbarkeitsgrenzen des Vaults.

## Grundregel

Sensible Personendaten gehören nicht in `scope: shared` und nicht in allgemein lesbare Domain-Notes.

Besonders geschützt:

- Gesundheitsdaten
- Garmin-/Readiness-/Trainingstagebücher einzelner Personen
- private Präferenzen, Constraints, IDs und Kommunikationsmuster von Subusern
- Credentials, Tokens, Passwörter, API Keys
- private Rezepte oder nicht freigegebene Nutzerinhalte

## Scope-Matrix

| scope | Inhalt | Sichtbarkeit | Erlaubte sensitivity |
|---|---|---|---|
| personal | Krystofs Second Brain | Hauptuser/Admin | `internal`, `private`, ggf. `health` |
| shared | System-/Methodenwissen, teamfähig | alle Agenten/Hermes | `public`, `internal` |
| domain | Tool-/Domänenwissen ohne private Personendaten | Agenten mit Domain-Lens | `public`, `internal` |
| user_private | Subuser-Capsules und personenbezogene Details | Subject-User + Admin | `private`, `health` |

## Nie in shared/domain speichern

- Health/Garmin/Athletendaten einzelner Subuser.
- Nicht anonymisierte Coaching-Profile.
- Vollständige private Chatverläufe.
- Zugangsdaten oder Tokens.
- Nicht freigegebene Rezeptdaten aus privaten Bereichen.
- Rohdaten, aus denen eine Person leicht rekonstruiert werden kann.

## Kratos-Regeln

- Subuser-Capsules liegen außerhalb des shared Vaults in Hermes-Profilen, z.B. `~/.hermes/profiles/kratos-coaching/memories/users/<id>/USER.md`.
- Garmin- und Health-Daten bleiben in den Kratos-Profilpfaden/DBs, nicht in `shared`.
- Shared Vault darf anonymisierte Trainings- oder Recovery-Muster enthalten, wenn kein Rückschluss auf eine Person naheliegt.
- Methodik darf in `40_Knowledge/Fitness/` oder `60_Procedures/coaching-playbooks/` liegen, solange sie nicht personenbezogen ist.

## Rezepte-Regeln

- Operativer Rezept-Mirror bleibt `Rezepte/` und wird nicht unüberlegt in die neue Wissensstruktur migriert.
- Private Rezepte bleiben in `Rezepte/Private/<user>/` bzw. im bestehenden Rezepte-System.
- Freigegebene Rezepte und allgemeine Kochtechniken dürfen als shared/domain Wissen verlinkt oder extrahiert werden.
- Personenbezogene Freigabe- oder Rollenlogik gehört in System-/Projektwissen nur ohne private Details.

## Hermes-/Ops-Regeln

- Systemarchitektur, Modellrouting, Bot-Ops und Workflows dürfen shared/domain sein.
- Credentials, Tokens, Chat-IDs mit sensiblem Kontext und API Keys dürfen nicht im Vault stehen.
- Wenn eine Note operative Pfade nennt, nur Pfade ohne Secrets dokumentieren.

## Anonymisierung

Eine Note gilt nur dann als anonymisiert, wenn:

1. keine Namen, User-IDs oder eindeutigen Kontaktmerkmale enthalten sind,
2. keine ungewöhnliche Kombination von Merkmalen eine Re-Identifikation nahelegt,
3. Details auf Muster- oder Methodikniveau verdichtet wurden,
4. der Nutzen als geteiltes Wissen höher ist als das Re-Identifikationsrisiko.

## Lint-Regeln

Der spätere Health Check soll mindestens prüfen:

- `scope: shared` + `sensitivity: private|health` = ALERT
- `scope: domain` + `sensitivity: health` = Review erforderlich
- fehlendes `subject_user` bei `scope: user_private` = ALERT
- offensichtliche Secrets im Vault = ALERT
- Health-/Garmin-Schlüsselwörter in shared Notes = Review erforderlich

## Links

- [[README]]
