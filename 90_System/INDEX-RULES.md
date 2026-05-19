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
tags: [index, navigation, policy, obsidian]
---

# Index Rules

Diese Note legt fest, wann `index.md`, `README.md` oder `log.md` gepflegt werden.

## Ziel

Indizes sollen Orientierung geben, aber nicht zu manueller Buchhaltung ausarten.

Grundsatz: Nur stabile Hubs und wichtige Navigationspunkte pflegen; flüchtige Arbeitsnotizen nicht überall eintragen.

## Wann ein Index aktualisiert wird

Aktualisieren, wenn eine Note eines der folgenden Kriterien erfüllt:

1. Sie ist eine kanonische Policy, Entscheidung oder Architektur-Note.
2. Sie ist ein Projekt-Hub oder verändert Projektstatus wesentlich.
3. Sie ist eine Procedure, die wiederverwendet werden soll.
4. Sie ist ein Fact/Concept mit hoher Wiederverwendungswahrscheinlichkeit.
5. Sie superseded eine bestehende zentrale Note.
6. Sie ist ein Query-Result, das später wahrscheinlich erneut gebraucht wird.

Nicht aktualisieren für:

- transient/working Scratch Notes,
- einmalige Zwischenstände,
- reine Mirror-Dateien,
- private oder sensible Subuser-Daten im falschen Scope,
- automatisch generierte Rohquellen ohne kuratierte Zusammenfassung.

## Empfohlene Hubs

| Ort | Zweck |
|---|---|
| `90_System/README.md` oder `90_System/index.md` | Systemregeln, Schema, Lifecycle, Privacy |
| `30_Projects/<Projekt>/index.md` | Projektstatus, zentrale Entscheidungen, nächste Schritte |
| `40_Knowledge/<Domain>/index.md` | wichtige Fakten/Konzepte pro Domain |
| `60_Procedures/<Bereich>/index.md` | wiederverwendbare Playbooks |
| `70_Queries/index.md` | wichtige Recherche-/Vergleichsantworten |
| `80_Outputs/index.md` | Reports, Briefs, Health Checks |

## Log vs Index

- `index.md`: kuratierte Navigation, aktuelle und kanonische Einträge.
- `log.md`: chronologische Ereignisse, wenn Verlauf wichtig ist.
- Episode-Notes ersetzen nicht automatisch ein Projektlog; sie können aber daraus verlinkt werden.

## Link-Regeln

- Jede kanonische Note soll mindestens einen sinnvollen Wikilink nach außen haben.
- Projekt-Notes sollen zum Projekt-Hub linken.
- System-Policies sollen sich gegenseitig verlinken, wenn Abhängigkeiten bestehen.
- Supersession immer bidirektional verlinken.
- Bei Quellen möglichst von kuratierter Note zur Quelle linken, nicht jede Quelle in jeden Index aufnehmen.

## Index-Eintrag Format

Bevorzugtes Format:

```markdown
- [[Note Name]] — kurzer Zweck / Status / warum wichtig
```

Bei Entscheidungen:

```markdown
- [[Recommendation]] — current; supersedes [[Architecture]]; Entscheidung: Obsidian Sync primär, Git Backup
```

## Migration-Regel

Während Schritt 1-3 der Wiki-v2-Migration genügt es, `90_System` vollständig anzulegen. Projekt- und Knowledge-Indizes werden erst gepflegt, wenn die Zielordner in Schritt 3 angelegt oder normalisiert werden.
