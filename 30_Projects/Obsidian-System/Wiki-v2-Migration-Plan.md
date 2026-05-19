---
type: project
status: completed
project: obsidian-system
scope: shared
domain: general
sensitivity: internal
subject_user: null
confidence: medium
evidence_count: 1
last_confirmed: 2026-05-19
review_after: 2026-08-19
retention: durable
supersedes: []
superseded_by: []
date: 2026-05-19
tags: [plan, obsidian, pkm, migration, completed]
related: ["[[Obsidian-Sync/Recommendation]]", "[[../Hermes-Assistant/Model-Routing-Recommendation]]"]
---

# Wiki v2 Migration — Zielarchitektur & Umsetzungsplan

Handoff-Note: Nach Hermes-Restart mit `gpt-5.5` (lmstudio-Slot, Default ist gesetzt) diese Note öffnen und Schritt 1 starten.

## Kontext

Basis: Karpathy LLM Wiki + Erweiterungen aus dem agentmemory v2 Paper (Lifecycle, Supersession, Forgetting, Consolidation Tiers, Typed Graph, Hybrid Search, Crystallization, Privacy-Scopes).

Status-Befund Vault `/root/obsidian-vault`:
- 92 .md Dateien, davon 77 Rezepte-Mirror
- 84/92 mit Frontmatter, aber 77 ohne `type`
- nur 1/92 mit Wikilinks
- `03_Knowledge` / `04_Sources` / `05_Outputs` enthalten nur README-Stubs
- Widerspruch zwischen `02_Projects/Obsidian-Sync/Architecture.md` (Git primär) und `Recommendation.md` (Obsidian Sync primär) — perfekter Test-Case für Supersession.

## Zielarchitektur

### 4 Scopes (hart getrennt)

| Scope | Inhalt | Sichtbarkeit |
|---|---|---|
| personal | Krystofs Second Brain | nur Hauptuser |
| shared | System-/Methodenwissen, teamfähig | alle Agenten/Hermes |
| domain | Pro Tool: Kratos/Rezepte/Hermes-Architektur | alle, Domain-Lens |
| user_private | Subuser-Capsules, sensible Daten | nur Subject-User + Admin |

Regel: sensible Personendaten NIE in shared/domain. Health/Garmin/Athletendaten bleiben in Hermes-Profilen, nicht im shared Vault.

### Zielordner

```
/root/obsidian-vault/
├── 00_Inbox/
├── 10_Episodes/         verdichtete Sessions (working memory → episodic)
├── 20_People/
│   ├── krystof/
│   └── anonymized-patterns/
├── 30_Projects/
│   ├── Hermes-Assistant/
│   ├── Obsidian-System/
│   ├── Kratos/
│   └── Rezepte/
├── 40_Knowledge/        semantic facts (Konzepte, Bibliotheken)
│   ├── AI/
│   ├── Fitness/
│   ├── Cooking/
│   └── Systems/
├── 50_Sources/          raw/immutable
│   ├── papers/
│   ├── youtube/
│   ├── web/
│   └── voice/
├── 60_Procedures/       Playbooks, Workflows
│   ├── coaching-playbooks/
│   ├── recipe-workflows/
│   └── hermes-ops/
├── 70_Queries/          gefilterte Antworten, Vergleiche
├── 80_Outputs/          Reports, Slides, Briefs
├── 90_System/
│   ├── SCHEMA.md
│   ├── LIFECYCLE.md
│   ├── PRIVACY-SCOPES.md
│   ├── TAG-TAXONOMY.md
│   └── INDEX-RULES.md
└── Rezepte/             bleibt operativer Mirror (nicht migrieren)
```

### Pflicht-Frontmatter (alle neuen Notes)

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
tags: [aus taxonomy]
```

### Lifecycle / Tiers (aus dem Paper)

1. **working** — neue Observations, Inbox
2. **episodic** — Session-Verdichtungen (10_Episodes)
3. **semantic** — Cross-Session-Fakten (40_Knowledge)
4. **procedural** — wiederholte Patterns → Skills (60_Procedures)

Promotion durch Evidence-Akkumulation (`evidence_count` ≥ 2-3 → Promotion-Kandidat).
Forgetting via `retention` + `review_after` + Lint.

### Crystallization-Flow (Hermes Standard nach jeder wertvollen Session)

1. Episode-Note in `10_Episodes/` (kompakt: Frage, Fund, Lessons, betroffene Files)
2. 1-3 Fakten in `40_Knowledge/` neu oder updaten (`evidence_count++`)
3. Bei Widerspruch: Supersession-Link setzen (`supersedes:` / `superseded_by:` / `status: superseded`)
4. Bei wiederholtem Pattern: Procedure in `60_Procedures/` oder Skill-Kandidat markieren
5. Bei substantieller Recherche: Query-Note in `70_Queries/`

## Pro User

### Krystof (Hauptuser)
Vollausbau personal scope. Episodes, Knowledge, Procedures, Queries, Decisions.

### Kratos-Subuser (Lars 1171427466, Chris, Katja 8733691947)
- Capsule in `~/.hermes/profiles/kratos-coaching/memories/users/<id>/USER.md` (nicht shared Vault)
- Shared Vault sieht nur anonymisierte Patterns unter `20_People/anonymized-patterns/`
- Garmin/Health bleiben in `profiles/kratos-coaching/garmin/<id>/` JSON/DB

### Rezepte-Subuser
- Capsule = bestehende `users.yaml` + `Rezepte/Private/<user>/`
- Shared Vault enthält freigegebene Rezepte + Techniken/Patterns
- Keine personenbezogenen Daten in shared notes

### Neue Tools später
Gleicher Bauplan: domain-Ordner unter `30_Projects/`, `40_Knowledge/`, `60_Procedures/`. User-private bleibt im jeweiligen Hermes-Profil.

## Umsetzung — 6 Schritte

### Schritt 1 — Schema schreiben (Tag 1, ~30 Min)
Status: erledigt am 2026-05-19. Angelegt und frontmatter-validiert: [[../../90_System/SCHEMA]], [[../../90_System/PRIVACY-SCOPES]], [[../../90_System/TAG-TAXONOMY]], [[../../90_System/LIFECYCLE]], [[../../90_System/INDEX-RULES]].

Anlegen:
- `90_System/SCHEMA.md` — Typen, Frontmatter-Standard, Lifecycle, Supersession
- `90_System/PRIVACY-SCOPES.md` — Scope-Regeln, was nie wo landen darf
- `90_System/TAG-TAXONOMY.md` — erlaubte Tags, basierend auf bestehendem `_System/Topic Taxonomy.md`
- `90_System/LIFECYCLE.md` — Tier-Promotion, Retention-Regeln
- `90_System/INDEX-RULES.md` — wann index.md/log.md updaten

### Schritt 2 — Bestehende Notes normalisieren (Tag 2-3)
Status: erledigt am 2026-05-19. Legacy-Notes außerhalb `Rezepte/` normalisiert, Wikilinks ergänzt, Supersession-Pilot umgesetzt.
- Alle `02_Projects/**` + `_System/**` + Vault-`README.md` auf neues Frontmatter
- Wikilinks ergänzen (Ziel: >50% verlinkt)
- **Supersession-Pilotcase**: `Obsidian-Sync/Architecture.md` markieren als `status: superseded`, `superseded_by: [[Recommendation]]`; in `Recommendation.md` `supersedes: [[Architecture]]` und `current: true`

### Schritt 3 — Skopierung pro System (Tag 4-5)
Status: erledigt am 2026-05-19. Zielordner angelegt; Hermes-Assistant nach `30_Projects/Hermes-Assistant/`, Obsidian-Sync nach `30_Projects/Obsidian-System/Obsidian-Sync/`; Kratos/Rezepte-Domain-Hubs initialisiert.
- `30_Projects/Hermes-Assistant/`, `Obsidian-System/`, `Kratos/`, `Rezepte/` anlegen
- Bestehende Projekt-Notes dorthin verschieben (oder neu aufsetzen)
- `40_Knowledge/Cooking/` für Techniken/Zutaten/Patterns (verlinkt zu Rezept-Mirror)
- `40_Knowledge/Fitness/` für Methodik (von Kratos `obsidian_meta/` migrieren)
- `60_Procedures/recipe-workflows/`, `hermes-ops/`, `coaching-playbooks/`

### Schritt 4 — Episode/Crystallization-Pipeline (Woche 2)
Status: erledigt am 2026-05-19. Procedure [[../../60_Procedures/hermes-ops/Wiki-Crystallization]] und Hermes Skill `wiki-crystallization` angelegt; Episode-Beispiel erstellt.
- Hermes-Skill `wiki-crystallization` schreiben:
  - Trigger: am Session-Ende oder per `/crystallize` Slash-Command
  - Output: Episode-Note + Knowledge-Updates + Procedure-Kandidaten
- Manual-Test mit den letzten 3-5 Sessions

### Schritt 5 — Subuser-Capsules (Woche 2)
Status: erledigt am 2026-05-19. Bestehende Kratos-USER-Capsules geprüft; shared Vault enthält nur anonymisierte Patterns unter [[../../20_People/anonymized-patterns/index]].
- Pro Subuser `USER.md` in Profile-Memory normalisieren (Präferenzen, Constraints, Kommunikationsstil)
- Anonymized-Patterns-Ordner im Shared Vault initial befüllen (z.B. „Recovery-Reaktionsmuster nach hartem Workout-Tag")

### Schritt 6 — Lint + Health Check (Woche 3)
Status: erledigt am 2026-05-19. Health-Check-Script erstellt, Report nach `80_Outputs/` geschrieben, Cron vorbereitet/eingerichtet.
Cron-Job (täglich/wöchentlich):
- missing type/scope/tags
- superseded ohne Link
- review_after überschritten
- orphans (keine Backlinks, keine inbound Links)
- confidence:low ohne Review
- **Privacy-Verstoß**: `scope: shared` mit `sensitivity: private|health` → ALERT
- Output: Health-Report-Note in `80_Outputs/`

## Abschlussstatus 2026-05-19

Migration vollständig ausgeführt.

Verifikation:
- Nicht-Rezepte-Notes geprüft: 47
- Vollständiges Pflicht-Frontmatter: 47/47
- Gültige `type`-Werte: 47/47
- Notes mit Wikilinks: 47/47
- Health Check: 0 Issues, 0 Privacy Alerts, 0 Orphan-Kandidaten
- Report: [[../../80_Outputs/wiki-health-2026-05-19]]
- Backup vor Migration: `/root/obsidian-vault-backup-wiki-v2-20260519-204227.tar.gz`
- Cron Job: `obsidian-wiki-health-check` (`1d59497bcde9`), wöchentlich Montag 07:00 UTC

## Nächste sinnvolle Arbeit

1. Obsidian Sync/Headless real anbinden, falls noch nicht erledigt.
2. `wiki-crystallization` bei wertvollen Sessions nutzen.
3. Legacy-Ordner (`02_Projects`, `03_Knowledge`, `04_Sources`, `05_Outputs`, `_System`) erst nach stabiler Sync-/Backup-Phase löschen oder archivieren.
4. Vector-/Hybrid-Index erst ab ca. 200-500 kuratierten Nicht-Mirror-Notes erneut prüfen.

## Offene Fragen für später

- Vector-Index: ab welcher Vault-Größe genau aktivieren? Aktuell zurückgestellt.
- Multi-Vault vs Single-Vault mit Scopes: aktuell Single-Vault mit Scopes; später ggf. Kratos-Vault separieren, wenn Privacy es erfordert.
- Anonymized-Patterns weiter schärfen, sobald echte wiederkehrende Muster entstehen.

## Technische Notizen

- Hermes-Patch heute: `model_switch.py` Zeile 1181 + statische `_PROVIDER_MODELS["lmstudio"]` in `models.py` — überlebt `hermes update` NICHT. Post-Update-Hook fehlt noch.
- Provider-Mapping: `lmstudio`-Slot zeigt auf `api.openai.com/v1` via `LM_API_KEY`. Direkter OpenAI-Zugang zu allen gpt-5.x.
- `openai-codex` ist separater ChatGPT-OAuth-Login und nicht nötig für 5.5.
