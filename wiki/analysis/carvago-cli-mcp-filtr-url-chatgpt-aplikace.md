---
type: analysis
title: "Carvago CLI + Carvago MCP: URL filtru pro frontend"
question: "Pro Carvago: navrhnout Carvago CLI a Carvago MCP, kde výstupem obou je URL filtru na frontendu; lze připojit do ChatGPT jako aplikace?"
tags: [carvago, cli, mcp, chatgpt, frontend, filters]
created: 2026-04-25
updated: 2026-04-25
---

# Carvago CLI + Carvago MCP: URL filtru pro frontend

## Question / Framing

Cíl je sjednotit dva vstupní kanály:
- **Carvago CLI** (pro interní uživatele / automatizaci),
- **Carvago MCP** (pro AI agenty, včetně napojení do ChatGPT).

U obou kanálů má být **stejný výstup**: bezpečně sestavená URL frontendového filtru, kterou lze hned otevřít a pokračovat ve výběru vozu.

## Analysis

### 1) Proč je URL filtr dobrý „contract“

URL filtru je praktický meziformát, protože:
- je okamžitě použitelný v produktu (deep-link na frontend),
- je auditovatelný a snadno sdílitelný,
- umožňuje stejný výstup pro člověka, skript i AI agenta,
- odpojuje „porozumění dotazu“ od samotného UI.

### 2) Návrh architektury (jedno jádro, více vstupů)

Doporučené je mít společný backend modul:
- `filter_builder` (validace vstupu → normalizace → mapování na parametry → canonical URL).

Nad ním dvě tenké vrstvy:
- **CLI command**: `carvago search --budget 650000 --body suv --fuel hybrid --year-min 2021`
- **MCP tool**: `build_filter_url({ budget_max, body, fuel, year_min, ... })`

Obě vrstvy volají stejné jádro a vrací:
- `url` (canonical frontend link),
- `applied_filters` (debug/telemetrie),
- `warnings` (např. konflikt filtrů).

### 3) MCP + ChatGPT app: proveditelnost

Ano, koncept je proveditelný:
- ChatGPT agent může přes MCP volat nástroj typu `build_filter_url`.
- Výstupní URL může uživatel otevřít přímo v Carvago frontendu.
- Prakticky to funguje jako „AI vrstva pro sestavení filtru“, zatímco nákupní flow zůstává v Carvago.

### 4) Důležité implementační detaily

- **Canonicalizace URL**: stejné zadání musí vracet stejnou URL (pořadí parametrů, defaulty).
- **Whitelist parametrů**: nepouštět neznámé query parametry.
- **Validace rozsahů**: cena, rok, nájezd, výkon atd.
- **Konflikty**: vracet varování místo tichého selhání (např. EV + manuál).
- **Telemetry**: logovat vstupní intent → finální URL → otevření/konverze.
- **Verzování contractu**: např. `v1` schéma pro MCP i CLI.

### 5) Doporučené MVP

1. Definovat filter schema (`v1`) a mapování na frontend query params.
2. Implementovat `filter_builder` jako jediný zdroj pravdy.
3. Přidat CLI příkaz vracející URL + JSON debug výstup.
4. Vystavit MCP tool se stejným schématem.
5. Měřit metriky: CTR na výsledky, time-to-first-relevant-car, lead rate.

## Evidence

- Navazuje na [[carvago-llm-doporuceni-aut]] (predikce preferencí a předvyplnění filtrů).
- Provozně souvisí s [[projektova-pravidla-spoluprace]] (průběžné ukládání návrhů do wiki).

## Limitations

- Chybí detailní znalost aktuálního frontend query schématu Carvago (nutné potvrdit s FE/BE).
- Chybí rozhodnutí, zda canonical URL bude čistě query-string, nebo i path-segmenty.
- Bez produkčních dat nelze předem vyčíslit uplift konverze.

## Connections

- [[carvago-llm-doporuceni-aut]]
- [[projektova-pravidla-spoluprace]]
