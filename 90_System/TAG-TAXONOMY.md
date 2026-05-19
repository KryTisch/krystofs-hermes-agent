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
supersedes: [[Topic Taxonomy]]
superseded_by: []
tags: [taxonomy, tags, obsidian, policy]
---

# Tag Taxonomy

Diese Taxonomie basiert auf `[[_System/Topic Taxonomy]]` und normalisiert sie für Wiki v2.

## Prinzipien

- Tags sind stabil, klein und wiederverwendbar.
- Pro Note möglichst ein dominanter Topic-Tag, ergänzende Tags nur wenn nützlich.
- Neue Tags sind erlaubt, wenn ein Thema wiederkehrt.
- Einmalige oder unklare Themen bleiben in `inbox` oder bekommen nur sekundäre Tags.
- Tags sind keine Ordnerersatz-Hierarchie; Ordner + Wikilinks bleiben primär.

## Core Topic Tags

| Thema | Erlaubter Tag | Beispiele |
|---|---|---|
| Lifestyle | `lifestyle` | Alltag, Gewohnheiten |
| Kochen / Ernährung | `kochen`, `ernaehrung`, `rezepte` | Rezeptsystem, Kochtechniken |
| Reisen | `reisen` | Planung, Orte |
| Familie | `familie` | private Familiennotizen |
| Fitness / Gesundheit | `fitness`, `gesundheit`, `kratos` | Training, Coaching-Methodik |
| Arbeit / Projekte | `arbeit`, `projekte` | Umsetzung, Projektstatus |
| Technik / Tools | `technik`, `tools` | Software, Automatisierung |
| Hermes Agent / Bot Ops | `hermes`, `bot-ops`, `model-routing` | Gateway, Modelle, Cron |
| Obsidian / Wissenssystem | `obsidian`, `wissenssystem`, `pkm` | Vault, Sync, Schema |
| Meetings / Transkription | `meetings`, `transkription`, `voice` | Meeting Notes, Voice Intake |
| Finanzen / Budget | `finanzen`, `budget` | Budget, Ausgaben |
| Medien / Content | `medien`, `content`, `youtube` | Videoanalyse, Quellen |
| Social / Kommunikation | `social`, `kommunikation` | DMs, Plattformen |
| Lernen / Weiterbildung | `lernen`, `weiterbildung` | Kurse, Lernnotizen |
| Sonstiges / Inbox | `inbox`, `sonstiges` | unklare Notizen |

## System Tags

| Zweck | Tags |
|---|---|
| Lifecycle | `working`, `episodic`, `semantic`, `procedural` |
| Status | `draft`, `active`, `current`, `superseded`, `review` |
| Note-Typ | `source`, `observation`, `episode`, `fact`, `decision`, `procedure`, `concept`, `query`, `policy`, `mirror` |
| Datenqualität | `low-confidence`, `needs-evidence`, `canonical` |
| Privacy | `privacy`, `anonymized`, `user-private` |

## Domain Tags

| Domain | Primäre Tags |
|---|---|
| Hermes | `hermes`, `bot-ops`, `gateway`, `cron`, `model-routing`, `skills` |
| Kratos | `kratos`, `fitness`, `coaching`, `readiness`, `workout-methodik` |
| Rezepte | `rezepte`, `kochen`, `ernaehrung`, `workflow`, `taxonomy` |
| Obsidian-System | `obsidian`, `pkm`, `sync`, `schema`, `taxonomy`, `crystallization` |

## Schreibweise

- Tags im Frontmatter ohne `#`, z.B. `tags: [obsidian, schema]`.
- Im Fließtext nur bei Bedarf Hashtags verwenden.
- Deutsch bevorzugt, wenn das Thema deutsch geführt wird; technische Begriffe dürfen englisch bleiben.
- Umlaute in Tags vermeiden: `ernaehrung` statt `ernährung`.

## Erweiterungsregel

Ein neuer Tag darf angelegt werden, wenn mindestens eines gilt:

1. Das Thema ist wiederkehrend.
2. Der Tag hilft bei späteren Queries oder Health Checks.
3. Der Tag entspricht einem stabilen Projekt, Tool oder Workflow.
4. Der Tag markiert eine wichtige Policy-/Lifecycle-Eigenschaft.

Nicht anlegen für einmalige Sessiondetails, zufällige Dateinamen oder zu enge Unterthemen.

## Beziehung zur alten Taxonomie

Die alte Note `[[_System/Topic Taxonomy]]` bleibt als Ursprung erhalten. Diese Note ist für neue Wiki-v2-Notes die aktive System-Taxonomie.
