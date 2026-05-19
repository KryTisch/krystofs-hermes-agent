---
type: decision
status: proposed
project: hermes-assistant
scope: domain
domain: hermes
sensitivity: internal
subject_user: null
confidence: high
evidence_count: 1
last_confirmed: 2026-05-19
review_after: 2026-08-19
retention: durable
supersedes: []
superseded_by: []
date: 2026-05-11
tags: [hermes, model-routing, decision]
---

# Hermes Model-Routing Empfehlung

## Ausgangslage

Krystof bemerkt zurecht: Der Main-Chat läuft aktuell mit `gpt-5.5`, was für normale Konversation und kleine Organisationsaufgaben zu stark/teuer ist.

Lokaler Stand am 2026-05-11:

```yaml
model:
  provider: openai-codex
  default: gpt-5.5
  base_url: https://chatgpt.com/backend-api/codex
```

Verfügbare Codex-Modelle in diesem Account:

```text
gpt-5.5
gpt-5.4
gpt-5.4-mini
gpt-5.3-codex
gpt-5.2
```

Wichtig: `gpt-5.1` wurde in der aktuellen Codex-Modellliste nicht gefunden. Für günstige Standardarbeit ist daher aktuell `gpt-5.4-mini` der naheliegende kleinste verfügbare Codex-Kandidat.

## Recherchepunkte

OpenAI-Dokumentation zu Model Selection empfiehlt das klassische Muster:

1. Erst mit einem starken Modell eine verlässliche Qualitätsbaseline herstellen.
2. Dann kleinere/günstigere Modelle testen.
3. Kosten/Latenz reduzieren über:
   - weniger Requests,
   - weniger Tokens,
   - kleinere Modelle,
   - Aufgaben-spezifische Modellwahl.

Hermes-Dokumentation unterscheidet:

- Main model: denkt im Hauptchat, Tool-Loops, Antworten.
- Auxiliary models: Side-Jobs wie Compression, Session Search, Web Extract, Title Generation, Approval, Skill Search usw.
- Auxiliary-Modelle sollten fast immer günstiger sein als das Main-Modell.

OpenAI Codex-Dokumentation sagt für Codex sinngemäß:

- `gpt-5.5`: aktuell stärkstes Modell, für hochkomplexe Aufgaben.
- `gpt-5.4`: weiterhin stark, wenn 5.5 nicht nötig/verfügbar ist.
- `gpt-5.4-mini`: schneller/günstiger für leichtere Coding-Aufgaben und Subagents.
- `gpt-5.3-codex`: Codex-orientiertes Modell für Coding/agentische Aufgaben.
- `gpt-5.2`: älteres general-purpose/coding Modell.

## Empfohlene Modellrollen

### 1. Default Main Chat: `gpt-5.4-mini`

Für:
- normale Unterhaltung,
- Rückfragen,
- kurze Zusammenfassungen,
- einfache Rechercheplanung,
- einfache Dateisuchen,
- Rezept-/Workout-Alltagsfragen,
- kleine Obsidian-Notizen,
- Triage und Selektion.

Warum:
- spart Kosten und Latenz,
- reicht sehr wahrscheinlich für 70-85% der täglichen Chats,
- verhindert, dass jede Kleinigkeit mit 5.5 läuft.

### 2. Escalated Main Chat: `gpt-5.4`

Für:
- komplexere Planung,
- Architekturentscheidungen,
- mehrstufige Recherche,
- nicht-triviale Debugging-Aufgaben,
- längere Schreib-/Syntheseaufgaben,
- Aufgaben mit mehreren Tools und Abhängigkeiten.

Warum:
- deutlich stärker als Mini,
- aber nicht so teuer/oversized wie 5.5.

### 3. Max Reasoning / High Stakes: `gpt-5.5`

Für:
- schwierige Architektur,
- wichtige irreversible Entscheidungen,
- komplexes Coding,
- große Refactors,
- Security-/Datenverlust-Risiken,
- finale Review von wichtigen Plänen,
- wenn ein günstigeres Modell sichtbar scheitert.

Warum:
- als Spitzenmodell reservieren, nicht als Dauer-Default.

### 4. Coding-Agent/Subagent: `gpt-5.3-codex` oder `gpt-5.4-mini`

Für:
- autonome Coding-Subtasks,
- Implementierung nach Plan,
- Tests/Refactors,
- Pull-Request-artige Aufgaben.

Empfehlung:
- leichte Coding-Subagents: `gpt-5.4-mini`
- anspruchsvollere Coding-Agenten: `gpt-5.3-codex` testen
- finale Review/harte Fehler: `gpt-5.5`

### 5. Auxiliary Tasks: günstiges Modell fest konfigurieren

Für:
- `title_generation`
- `compression`
- `session_search`
- `skills_hub`
- `approval`
- ggf. `web_extract`

Empfehlung:
- im Codex-only Setup: `gpt-5.4-mini`
- wenn OpenRouter/Gemini verfügbar und günstiger: Gemini Flash o.ä. testen

Wichtig: Aktuell stehen Auxiliary-Slots auf `auto`, d.h. sie können das Main-Modell nutzen. Wenn Main = 5.5 bleibt, werden Nebenjobs unnötig teuer.

## Praktische Hermes-Konfiguration

### Sofortiger pragmatischer Schritt

Setze Default Main auf `gpt-5.4-mini`:

```bash
hermes config set model.default gpt-5.4-mini
hermes config set model.provider openai-codex
```

Dann pro Session eskalieren:

```text
/model gpt-5.4 --provider openai-codex
/model gpt-5.5 --provider openai-codex
```

### Auxiliary explizit günstig setzen

Beispiel:

```bash
hermes config set auxiliary.compression.provider openai-codex
hermes config set auxiliary.compression.model gpt-5.4-mini
hermes config set auxiliary.session_search.provider openai-codex
hermes config set auxiliary.session_search.model gpt-5.4-mini
hermes config set auxiliary.title_generation.provider openai-codex
hermes config set auxiliary.title_generation.model gpt-5.4-mini
hermes config set auxiliary.approval.provider openai-codex
hermes config set auxiliary.approval.model gpt-5.4-mini
hermes config set auxiliary.skills_hub.provider openai-codex
hermes config set auxiliary.skills_hub.model gpt-5.4-mini
```

### Delegation prüfen

Aktuell:

```yaml
delegation:
  provider: anthropic
  model: claude-haiku-4-5
```

Das ist inkonsistent mit Codex-Routing und kann zusätzliche Kosten/Auth-Abhängigkeiten erzeugen. Empfehlung:

```bash
hermes config set delegation.provider openai-codex
hermes config set delegation.model gpt-5.4-mini
```

Für anspruchsvolle Coding-Subagents kann man später gezielt `gpt-5.3-codex` testen.

## Routing-Regeln für Krystofs Setup

### Mini verwenden, solange alle Bedingungen erfüllt sind

- einfache Unterhaltung,
- kein großes Risiko,
- keine umfangreiche Codeänderung,
- keine komplexe Recherche-Synthese,
- keine hohe Unsicherheit,
- Ergebnis kann leicht korrigiert werden.

### Auf 5.4 eskalieren, wenn

- mehrere Systeme betroffen sind,
- Aufgabe > 3-5 Schritte hat,
- Recherche + Synthese nötig ist,
- Debugging/Planung statt reiner Ausführung,
- User explizit „recherchiere gründlich“ oder „vollumfänglich“ sagt.

### Auf 5.5 eskalieren, wenn

- falsche Entscheidung teuer wäre,
- Codebase-Architektur / komplexe Implementierung,
- Datenverlust-/Security-Risiko,
- längerer autonomer Task,
- 5.4/Mini bereits Fehler gemacht hat,
- finale Review vor Umsetzung.

## Empfehlung

Nicht 5.5 als Dauer-Main behalten.

Beste aktuelle Aufstellung:

```text
Default Main Chat:       gpt-5.4-mini
Complex Main Chat:       gpt-5.4
Max/Final/Coding Review: gpt-5.5
Coding Subagents:        gpt-5.4-mini -> gpt-5.3-codex Test -> gpt-5.5 Review
Auxiliary Jobs:          gpt-5.4-mini oder noch günstigerer externer Flash-Anbieter
```

Nächster Schritt: Default + Auxiliary + Delegation konfigurieren, danach ein paar Tage beobachten und bei Qualitätsproblemen gezielt nachjustieren.

## Links

- [[index]]
