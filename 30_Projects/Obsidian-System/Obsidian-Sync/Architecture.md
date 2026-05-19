---
type: decision
status: superseded
project: obsidian-sync
scope: shared
domain: general
sensitivity: internal
subject_user: null
confidence: high
evidence_count: 1
last_confirmed: 2026-05-19
review_after: 2026-08-19
retention: durable
supersedes: []
superseded_by: ["[[Recommendation]]"]
date: 2026-05-11
tags: [obsidian, sync, architecture, superseded]
---

> Diese Entscheidung wurde abgelöst durch [[Recommendation]].

# Obsidian Sync Architektur

## Entscheidung

Der Vault wird als Markdown-Git-Repository geführt.

Zielgeräte:
- Hetzner/Hermes: `/root/obsidian-vault`
- MacBook: lokaler Clone, geöffnet in Obsidian
- Handy: Obsidian Mobile + Git-fähiger Sync-Weg
- Optional: Google Drive nur als zusätzlicher Backup-Spiegel, nicht als primärer Sync-Motor

## Warum Git als Kern

- Server/Hermes kann direkt lesen und schreiben.
- Mac kann direkt lesen und schreiben.
- Änderungen sind versioniert und wiederherstellbar.
- Konflikte sind sichtbar statt still überschrieben.
- Der Vault bleibt normale Markdown-Dateien.

## Warum Google Drive nicht als primärer Sync

Google Drive ist gut für Datei-Backups, aber weniger gut als bidirektionaler Obsidian-Sync über Server, Mac und Handy:
- Konflikte sind schwerer nachvollziehbar.
- iOS/Obsidian/Drive ist nicht so sauber wie Desktop.
- Der Hetzner-Server bräuchte zusätzliche Drive-Automation.

Besser: Git als primär, Google Drive als Backup-Ziel.

## Nächste Einrichtungsschritte

1. Privates Git-Remote anlegen, bevorzugt GitHub oder GitLab.
2. Remote auf dem Server verbinden.
3. MacBook clonen und in Obsidian öffnen.
4. Handy anbinden:
   - iOS: Working Copy oder Obsidian Git Plugin, je nach gewünschter Einfachheit.
   - Android: Obsidian Git Plugin ist meist direkter.
5. Optional: regelmäßiger Server-Backup-Job nach Google Drive.

## Links

- [[Recommendation]]
