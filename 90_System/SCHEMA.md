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
tags: [schema, frontmatter, lifecycle, supersession, obsidian, policy]
---

# Schema

Diese Note ist die kanonische Strukturvorgabe für neue Notes im Vault.

## Pflicht-Frontmatter für neue Notes

```yaml
type: source|observation|episode|fact|decision|procedure|concept|person|project|query|mirror|policy
scope: personal|shared|domain|user_private
domain: hermes|kratos|rezepte|general
sensitivity: public|internal|private|health
subject_user: <id oder null>
confidence: low|medium|high
evidence_count: <N>
last_confirmed: YYYY-MM-DD
review_after: YYYY-MM-DD
retention: transient|working|durable|canonical
supersedes: [[...]]
superseded_by: [[...]]
tags: [aus [[TAG-TAXONOMY]]]
```

Für bestehende Legacy-Notes gilt: schrittweise normalisieren, nicht blind massenweise überschreiben.

## Typen

| type | Zweck | Typischer Ort |
|---|---|---|
| source | Rohquelle oder importiertes Originalmaterial | `50_Sources/` |
| observation | Einzelbeobachtung, noch nicht verallgemeinert | `00_Inbox/` oder Projektordner |
| episode | verdichtete Session-/Arbeitszusammenfassung | `10_Episodes/` |
| fact | langlebiger semantischer Fakt | `40_Knowledge/` |
| decision | Architektur-/Produkt-/Systementscheidung | Projektordner oder `40_Knowledge/Systems/` |
| procedure | wiederholbarer Ablauf / Playbook | `60_Procedures/` |
| concept | erklärendes Wissenskonzept | `40_Knowledge/` |
| person | Person oder anonymisiertes Muster | `20_People/` |
| project | Projekt-Hub oder Projektstatus | `30_Projects/` |
| query | recherchierte Antwort / Vergleich / Synthese | `70_Queries/` |
| mirror | operativer Spiegel externer Daten | z.B. `Rezepte/` |
| policy | verbindliche Systemregel | `90_System/` |

## Scopes

Kurzfassung; Details in [[PRIVACY-SCOPES]].

| scope | Bedeutung |
|---|---|
| personal | Krystofs persönliches Second Brain |
| shared | teamfähiges System-/Methodenwissen |
| domain | Wissen für ein Tool/eine Domäne, ohne private Personendaten |
| user_private | kapselte Daten eines Subusers, nicht in shared auswerten |

## Domains

| domain | Verwendung |
|---|---|
| hermes | Hermes Agent, Bot Ops, Modellrouting, Infrastruktur |
| kratos | Coaching-System, Workout-Methodik, anonymisierte Muster |
| rezepte | Rezeptsystem, Kochwissen, Workflows |
| general | Obsidian-System, allgemeines Wissen, domänenübergreifende Methoden |

## Sensitivity

| sensitivity | Bedeutung |
|---|---|
| public | unkritisch öffentlich teilbar |
| internal | intern/systembezogen, nicht bewusst veröffentlichen |
| private | personenbezogen oder privat |
| health | Gesundheits-, Fitness-, Garmin- oder Coachingdaten |

Regel: `scope: shared` darf nicht mit `sensitivity: private` oder `sensitivity: health` kombiniert werden.

## Confidence und Evidence

- `confidence: low`: Hypothese, einmalige Beobachtung, ungeprüft.
- `confidence: medium`: plausibel, aber noch nicht kanonisch.
- `confidence: high`: geprüft, mehrfach belegt oder explizit entschieden.
- `evidence_count`: Anzahl unabhängiger Hinweise/Sessions/Quellen, die die Note stützen.

Promotion-Kandidat: `evidence_count >= 2` und keine offenen Widersprüche.

## Retention

| retention | Bedeutung |
|---|---|
| transient | kann bald gelöscht/überschrieben werden |
| working | aktive Arbeitsnotiz |
| durable | längerfristig relevant, reviewbar |
| canonical | Systemregel oder kanonische Entscheidung |

## Supersession

Wenn eine Note durch eine andere abgelöst wird:

1. Alte Note erhält `status: superseded` und `superseded_by: [[Neue Note]]`.
2. Neue Note erhält `supersedes: [[Alte Note]]` und, falls passend, `current: true`.
3. Nie alte Entscheidungen löschen, wenn sie Architekturgeschichte erklären.
4. Bei Widerspruch immer beide Richtungen verlinken.

Pilotfall für Schritt 2: `Obsidian-Sync/Architecture.md` wird durch `Obsidian-Sync/Recommendation.md` abgelöst.

## Minimalbeispiele

### Episode

```yaml
type: episode
scope: personal
domain: general
sensitivity: internal
subject_user: null
confidence: medium
evidence_count: 1
last_confirmed: 2026-05-19
review_after: 2026-06-19
retention: durable
supersedes: []
superseded_by: []
tags: [episode, obsidian]
```

### Fact

```yaml
type: fact
scope: shared
domain: hermes
sensitivity: internal
subject_user: null
confidence: high
evidence_count: 2
last_confirmed: 2026-05-19
review_after: 2026-08-19
retention: durable
supersedes: []
superseded_by: []
tags: [hermes, model-routing]
```
