---
title: Architektur - Rezeptsystem
bereich: Rezepte
erstellt: 2026-05-16
---

# Architektur - Rezeptsystem

## Rollen
- **Admin**: Freigaben, Re-Klassifikation, Vollzugriff
- **Contributor**: kann gemeinsame Rezepte einreichen, die auf Freigabe warten
- **Reader**: liest veröffentlichte Rezepte und pflegt nur eigene private Rezepte

## Wahrheitssystem
Jedes Rezept liegt jetzt in **3 Formen** vor:
1. **Markdown** unter `~/.hermes/rezepte/sammlung/` als Quelle der Wahrheit
2. **ChromaDB** für semantische Suche
3. **Obsidian-Spiegel** unter `Rezepte/`

## Ordnerlogik
- `Shared/` = veröffentlicht
- `Pending/` = wartet auf Freigabe
- `Private/` = nur Owner/Admin
- `Archive/` = deprecated

## Intake-Regeln
- **Foto**: OCR / Vision
- **Video**: Transkription, dann Rezept extrahieren
- **Website**: erst direkt, dann archive.ph-Fallback
- **Text**: direkt normalisieren

## Moderation
- Gemeinsame Rezepte aus Contributor-Intake landen als `pending_approval`
- Veröffentlichung erst nach Admin-Freigabe
- Owner- und Quellen-Tags bleiben erhalten

## Aktueller Stand
- Script: `/root/.hermes/scripts/rezepte_db.py`
- Rollen: `/root/.hermes/rezepte/config/users.yaml`
- Shared-Rezepte werden nach Obsidian gespiegelt
