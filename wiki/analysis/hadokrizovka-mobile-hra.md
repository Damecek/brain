---
type: analysis
title: "Nápad na mobilní hru: Had + slovní puzzle"
question: "Přidej nápad na hru na mobil a zapiš ho do databáze."
tags: [game-design, mobile-game, word-game, brainstorming]
created: 2026-04-15
updated: 2026-04-15
---

# Nápad na mobilní hru: Had + slovní puzzle

## Question / Framing
Uživatel navrhl jádro hry jako hybrid klasického hada a křížovky/slovního puzzle: had sbírá písmena místo jídla. Jakmile posloupnost písmen v těle hada vytvoří validní slovo ze slovníku (čeština, angličtina atd.), slovo se odečte z těla a hráč získá body.

## Analysis
### Core loop
1. Hráč ovládá hada na mřížce.
2. Na mapě se objevují písmena (single tile pickups).
3. Každé sebrané písmeno se přidá do „paměti těla“ hada podle aktuální trasy segmentů.
4. Systém průběžně vyhodnocuje, zda v těle vzniklo validní slovo.
5. Po uznání slova:
   - hráč získá body,
   - použité znaky se z těla odečtou,
   - had se částečně zkrátí a otevře prostor pro další komba.

### Slovní validace
- Použít více slovníků dle jazyka: `cs`, `en` (rozšiřitelné o další jazyky).
- Režimy:
  - **Single-language** (např. jen čeština).
  - **Mixed-language** (čeština + angličtina současně).
- Validace by měla podporovat minimální délku slova (např. 3+ znaky), aby nevznikaly triviální zásahy.

### Směry čtení slov v hadovi
Klíčová funkcionalita je hledání slov v obou směrech segmentové posloupnosti:
- dopředu i dozadu v lineárním pořadí těla,
- s respektováním trajektorie hada (např. úseky zleva doprava i zprava doleva).

Prakticky to znamená, že engine kontroluje kandidátní řetězce jak v přirozeném pořadí segmentů, tak v obráceném pořadí.

### Skórování (návrh)
- Základ: `body = délka_slova × jazykový_koeficient`.
- Bonusy:
  - delší slova (5+),
  - combo za více slov během krátkého časového okna,
  - „cross-path bonus“ za slovo složené přes více zatáček trasy.

### Iterace funkcionalit (návrh roadmapy)
1. **MVP:** jeden jazyk, lineární detekce slov, základní body.
2. **V2:** více jazyků + směrová validace (forward/backward).
3. **V3:** denní challenge (seed mapa + leaderboard).
4. **V4:** power-upy (freeze, wildcard písmeno, multiplikátor).

## Evidence
- Tento záznam vychází z uživatelského zadání v aktuální konverzaci (bez externího zdroje).

## Limitations
- Nejsou definované konkrétní slovníkové datasety, minimální frekvence slov ani pravidla diakritiky.
- Chybí rozhodnutí, zda se mají uznávat vlastní jména, slang nebo zkratky.

## Connections
- [[game-design]]
- [[word-game-mechanics]]
- [[mobile-game-loop]]
