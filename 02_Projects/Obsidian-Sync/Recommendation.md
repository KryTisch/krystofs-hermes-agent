---
type: decision
status: proposed
project: obsidian-sync
date: 2026-05-11
tags: [obsidian, sync, git, backup]
---

# Empfehlung: Obsidian Sync als Primärsync, Git als Backup/Versionierung

## Kontext

Ziel: Krystofs Wissen soll auf MacBook, Handy und Hetzner/Hermes verfügbar sein. Änderungen von Mac/Handy sollen zum Server kommen; Änderungen von Hermes sollen zurück auf Mac/Handy.

## Recherche-Ergebnis

### Beste Primärlösung: Obsidian Sync + Obsidian Headless

Obsidian Sync bietet:
- offiziellen Sync zwischen Mac, iOS, Android, Linux
- Offline-Arbeit mit späterem Merge
- Ende-zu-Ende-Verschlüsselung / AES-256 laut Obsidian Sync-Seite
- Version History
- offiziellen Headless-Client für Server: `obsidian-headless`

Der Headless-Client kann auf dem Server per CLI arbeiten:
- `npm install -g obsidian-headless`
- `ob login`
- `ob sync-list-remote`
- `ob sync-setup --vault "..."`
- `ob sync --continuous`

Der Server hat passende Runtime:
- Node v22.22.2
- npm 10.9.7

### Warum nicht Git als Primärsync

Git ist für Server/Mac perfekt, aber auf iPhone/iPad deutlich weniger bequem. Es braucht Working Copy oder Obsidian Git Plugin, Tokens/SSH, Pull/Commit/Push-Disziplin und Konfliktlösung. Für täglichen mobilen Capture ist das fehleranfälliger.

### Warum nicht Google Drive als Primärsync

Google Drive ist gut als Backup/Spiegel, aber nicht ideal als sauberer bidirektionaler Obsidian-Sync über Mac, iPhone und Headless-Server. Konflikte und iOS-Verhalten sind weniger transparent.

### Gute Nebenlösung: Git-Backup vom Server

Der Server-Vault kann zusätzlich regelmäßig in ein privates Git-Repo gepusht werden. Damit gibt es:
- unabhängige Historie
- einfaches Rollback
- Backup außerhalb von Obsidian Sync
- maschinenlesbare Markdown-Dateien für Hermes

Google Drive kann optional als weiteres Backup-Ziel dienen, aber nicht als primäres Sync-Protokoll.

## Empfohlene Architektur

Primär:

Obsidian Sync Remote Vault
↔ MacBook Obsidian
↔ iPhone Obsidian
↔ Hetzner `/root/obsidian-vault` via `obsidian-headless`

Sekundär:

Hetzner `/root/obsidian-vault`
→ privates GitHub/GitLab Repo als Backup/Versionierung
→ optional Google Drive Backup-Snapshot

## Entscheidungsvorschlag

1. Obsidian Sync abonnieren/nutzen.
2. Remote Vault erstellen.
3. Hetzner per `obsidian-headless` anbinden.
4. MacBook und Handy direkt über Obsidian Sync anbinden.
5. Server-Cron einrichten:
   - kontinuierlicher oder regelmäßiger `ob sync`
   - danach Git-Backup Commit/Push, falls Änderungen vorhanden

## Nicht speichern

Keine Tokens, Passwörter oder privaten Credentials in den Vault.
Kratos-/Gesundheitsdaten und Daten anderer Personen getrennt halten.
