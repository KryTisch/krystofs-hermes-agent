---
type: decision
status: active
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

# Model-Routing Policy

## Ziel

Krystof möchte nicht jedes Mal selbst entscheiden, welches Modell für eine Anfrage angemessen ist. Hermes soll mit einem günstigen Standardmodell starten und je nach Komplexität selbst eskalieren.

## Wichtige technische Grenze

Hermes wählt das Main-Modell für eine eingehende Nachricht, bevor das Modell die Nachricht verarbeitet. Ein Modell kann also nicht vor seinem eigenen ersten Token entscheiden: „Diese Anfrage sollte nicht ich, sondern ein stärkeres Modell beantworten.“

Praktisches Muster:

1. Das Default-Modell ist günstig genug für alle normalen Nachrichten.
2. Dieses Default-Modell erkennt Komplexität/Risiko.
3. Bei Bedarf delegiert/escalated es die eigentliche Arbeit an einen stärkeren Agenten bzw. eine neue Hermes-Instanz mit explizitem Modell.
4. Für Folgesessions kann die globale Default-Konfiguration angepasst werden.

## Empfohlenes Verhalten

Default Entry Model:
- `gpt-5.4-mini`

Escalation Targets:
- `gpt-5.4` für komplexe Planung, Research, Debugging, mehrstufige Tasks.
- `gpt-5.5` für High-Stakes, finale Reviews, schwere Coding-/Architekturaufgaben.
- `gpt-5.3-codex` als Testkandidat für Coding-Agenten.

## Automatische Eskalationsregeln

Mini soll selbst eskalieren, wenn mindestens eines zutrifft:

- User sagt: „gründlich“, „vollumfänglich“, „recherchiere“, „beste Option“, „Architektur“, „Strategie“.
- Aufgabe betrifft Hermes-Konfiguration, Memory-System, Sync, Datenhaltung, Security oder Kostenkontrolle.
- Aufgabe hat mehr als 3-5 operative Schritte.
- Aufgabe nutzt viele Tools oder externe Quellen.
- Es gibt Datenverlust-, Security-, Privacy- oder Geldrisiko.
- Es geht um Programmierung, Debugging, Refactoring oder Code Review.
- Das Modell ist unsicher oder erkennt widersprüchliche Informationen.

## Gewünschte UX

Krystof muss nicht manuell `/model ...` eingeben.

Stattdessen:

- Normale Antwort direkt mit `gpt-5.4-mini`.
- Bei komplexer Aufgabe: „Ich eskaliere diese Aufgabe auf 5.4/5.5, weil …“ und dann Ausführung über stärkeren Agenten / passende Hermes-Instanz.
- Bei extremen Aufgaben: 5.5 nur für Kernanalyse oder Final Review verwenden, nicht für triviale Tool-Arbeit.

## Implementierungsoptionen

### Sofort möglich

- Global Default auf `gpt-5.4-mini` setzen.
- Auxiliary Tasks auf `gpt-5.4-mini` setzen.
- Delegation/Subagents auf `gpt-5.4-mini` oder `gpt-5.3-codex` setzen.
- Für Eskalationen gezielt `hermes chat -q ... --provider openai-codex -m gpt-5.4/5.5` oder separate Profile verwenden.

### Besser als nächster Entwicklungsschritt

Ein kleiner Hermes-Router:

- lightweight classifier entscheidet vor Session-Start anhand der User-Nachricht:
  - mini / standard / expert / coding
- startet dann Hermes mit passendem Modell.
- optional berücksichtigt Chat/Platform/Profile, Task-Typ und historische Kosten.

Das wäre echte automatische Pre-Routing-Logik, nicht nur Selbsteskalation nach Eingang beim Mini-Modell.

## Links

- [[index]]
