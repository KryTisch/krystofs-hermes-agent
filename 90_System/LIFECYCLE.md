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
tags: [lifecycle, retention, crystallization, obsidian, policy]
---

# Lifecycle

Diese Note beschreibt, wie Informationen von Rohmaterial zu kanonischem Wissen reifen.

## Tiers

| Tier | Zweck | Typische Orte | Typische Typen |
|---|---|---|---|
| working | neue, unverdichtete Informationen | `00_Inbox/`, Projektordner | `observation`, `source` |
| episodic | verdichtete Sessions und Arbeitsverläufe | `10_Episodes/` | `episode` |
| semantic | cross-session Fakten und Konzepte | `40_Knowledge/` | `fact`, `concept`, `decision` |
| procedural | wiederholbare Abläufe | `60_Procedures/` oder Hermes Skills | `procedure` |

## Standard-Crystallization nach wertvollen Sessions

1. Episode-Note in `10_Episodes/` schreiben: Frage, Ergebnis, Lessons, betroffene Dateien.
2. 1-3 langlebige Fakten in `40_Knowledge/` neu anlegen oder aktualisieren.
3. Bei Widerspruch Supersession setzen: `supersedes`, `superseded_by`, `status: superseded`.
4. Wiederholte Muster als Procedure in `60_Procedures/` oder als Skill-Kandidat markieren.
5. Substantielle Recherche als Query-Note in `70_Queries/` ablegen.

## Promotion-Regeln

Eine Information darf befördert werden, wenn:

- sie in mindestens zwei unabhängigen Sessions/Quellen auftaucht (`evidence_count >= 2`), oder
- sie explizit als Entscheidung/Policy festgelegt wurde, oder
- sie für zukünftige Agentenarbeit operational wichtig ist, oder
- sie ein wiederholbares Workflow-Muster beschreibt.

Promotion-Pfade:

- `observation` → `episode`, wenn sie Teil eines Sessionverlaufs ist.
- `episode` → `fact`, wenn ein langlebiger Fakt extrahiert wird.
- `fact`/`decision` → `procedure`, wenn daraus ein wiederholbarer Ablauf wird.
- `procedure` → Hermes Skill, wenn der Ablauf regelmäßig agentisch ausgeführt werden soll.

## Retention-Regeln

| retention | Review | Lösch-/Archivregel |
|---|---|---|
| transient | 7-30 Tage | löschen/verdichten, wenn nicht mehr relevant |
| working | 30-60 Tage | in Episode/Fakt überführen oder schließen |
| durable | 3-12 Monate | prüfen, ob noch korrekt |
| canonical | 3-12 Monate | nur durch Supersession ablösen |

`review_after` ist Pflicht, außer bei bewusst stabilen Mirror- oder Legacy-Notes während der Migration.

## Forgetting

Vergessen heißt nicht zwangsläufig löschen:

- Doppelte Notizen zusammenführen.
- Überholte Entscheidungen superseden statt löschen.
- Transiente Arbeitsnotizen löschen, wenn kein Wert mehr besteht.
- Quellen behalten, wenn sie Belegfunktion haben.
- Personenbezogene Daten minimieren oder in passende private Capsules verschieben.

## Supersession-Lifecycle

Wenn eine Note veraltet:

1. `status: superseded` setzen.
2. `superseded_by: [[Neue Note]]` eintragen.
3. In der neuen Note `supersedes: [[Alte Note]]` setzen.
4. Optional im Body oben eine kurze Warnung einfügen: „Diese Note wurde abgelöst durch ...“
5. Bei aktueller Entscheidung `current: true` in der neuen Note setzen.

## Confidence-Updates

- Mehr Evidenz: `evidence_count` erhöhen und ggf. `confidence` anheben.
- Widerspruch: `confidence` senken oder Supersession-Review starten.
- Explizite Nutzerkorrektur: Note sofort aktualisieren, `last_confirmed` setzen.

## Privacy-Gate vor Promotion

Vor jeder Promotion in shared/domain prüfen:

- Enthält die Note personenbezogene Details?
- Enthält sie Health/Garmin/Coachingdaten einer konkreten Person?
- Ist sie ausreichend anonymisiert?
- Passt `scope` zu `sensitivity`?

Details: [[PRIVACY-SCOPES]].
