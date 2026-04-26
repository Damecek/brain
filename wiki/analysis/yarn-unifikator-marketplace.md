---
type: analysis
title: "Yarn Unifikátor: one-stop marketplace přízí"
question: "Zapiš mi novej nápad na projekt: unifikovaný e-shop přízí se scrapingem nabídek a dat od výrobců"
tags: [e-commerce, yarn, scraping, marketplace, data-platform]
created: 2026-04-15
updated: 2026-04-26
---

# Yarn Unifikátor: one-stop marketplace přízí

## Question / Framing

Cíl je postavit datově řízený marketplace pro příze (pletení i háčkování), který na jednom místě sjednotí produkty, nabídky od prodejců, fotky a technické parametry od výrobců. Výsledkem má být e-shop s výrazně lepší filtrací a orientací než běžné obchody.

## User use case: zkušená pletařka (priorita kritérií)

Vstup od reálné cílové uživatelky (matka zadavatele, aktivní pletařka). Kritéria seřazená podle váhy, jak je sama používá při výběru:

1. **Materiál** — dnes existuje extrémně mnoho variant (i exotika typu „mléčné vlákno"), na běžných e-shopech se v tom nedá orientovat. Filtr musí být robustní a normalizovaný.
2. **Tloušťka příze (yarn weight)** — stejná, ne-li větší váha než materiál. Standardní kategorie:
   - lace
   - fingering
   - sport
   - DK
   - worsted (aran)
   - bulky (chunky)
   - super bulky
   - jumbo
3. **Hmotnost klubka** — typicky 5 g, 50 g, 100 g, 100 g+.
4. **Barva.**
5. **Návin** (metráž).
6. **Certifikace** — např. vhodné pro děti do 3 let, Öko-Tex Standard, GOTS, mulesing-free atd.
7. **Doporučená velikost jehlic a háčku.**
8. **Dostupnost a logistika nákupu:**
   - preference jednoho e-shopu (když už uživatel jednu přízi vybere, dál by se mělo přednostně dohledávat tam — kvůli ušetření poštovného),
   - cena poštovného jako filtr/řadítko,
   - geografické omezení: jen CZ, jen SK, nebo CZ + SK e-shopy.

### Textové vyhledávání s normalizací synonym

Klíčový požadavek: fulltext musí robustně řešit varianty pojmenování stejné věci napříč e-shopy. Příklady:
- `mohér` ↔ `mohair`
- `viscose` ↔ `viskóza` ↔ `viskoza`
- diakritika obecně (CZ trh) musí být ignorována.

Z toho plyne potřeba **synonym dictionary / aliasů na úrovni materiálu** a normalizace dotazů (lowercase, stripped diakritika, jazykové aliasy CZ/EN).

### Implikace pro datový model a UX

- Materiál a tloušťka musí být **enumerace s aliasy**, nikoli volný text. Bez toho filtrace nefunguje.
- Hmotnost klubka jako diskrétní bucket (5 / 50 / 100 / 100+ g) i jako přesná hodnota.
- Certifikace jako multi-select tagy s ověřitelným zdrojem (od výrobce).
- „Sticky merchant" preference: po výběru první příze upřednostnit nabídky stejného e-shopu (optimalizace celkového nákupního košíku včetně poštovného).
- Geo-filtr na e-shopy: CZ / SK / CZ+SK.
- Doporučené jehlice a háček patří mezi povinná zobrazovaná pole na kartě produktu.

## Analysis

### 1) Product intelligence vrstva (core výhoda)

Základ projektu je **normalizovaný katalog přízí**:
- 1 „kanonická“ karta příze (značka + řada + varianta/barva + klubko)
- k ní napárované nabídky z více e-shopů
- enrichment metadat z výrobních specifikací

To řeší hlavní problém trhu: stejné příze jsou napříč internetem popsané nejednotně a špatně se porovnávají.

### 2) Jaká data sbírat

**A) Data od výrobců (pravda o produktu):**
- složení (např. merino 80 %, nylon 20 %)
- hmotnost/návin (g / m)
- doporučená síla jehlic/háčku
- gauge / vzorek
- údržba
- barevnice, lot/šarže, certifikace

**B) Data od prodejců (tržní nabídka):**
- cena, dostupnost, doprava, balení
- EAN/SKU, varianty, obrázky
- akce/slevy, minimální odběr
- hodnocení a popularita

**C) Odvozené metriky (vaše konkurenční výhoda):**
- cena za 100 g
- cena za 100 m
- odhad počtu klubek pro typické projekty
- náhrady/alternativy mezi přízemi podle parametrické podobnosti

### 3) Datový model (MVP)

Doporučené entity:
- **YarnProduct** (kanonický produkt)
- **YarnVariant** (barva/lot)
- **ManufacturerSpec** (ověřená technická data)
- **MerchantOffer** (konkrétní nabídka e-shopu)
- **OfferSnapshot** (historie cen a dostupnosti)
- **ImageAsset** (foto + zdroj + deduplikace)

Klíčová capability: **matching pipeline** (fuzzy názvy + atributy + EAN/SKU + manuální override).

### 4) UX, které prodává

Filtrace by měla být „projektově orientovaná“:
- „chci svetr na zimu“ → filtr podle složení, metráže, měkkosti, ceny
- „hledám náhradu za konkrétní přízi“ → podobné příze podle tolerance parametrů
- „nejlepší nabídka“ → porovnání ceny + dostupnosti + dopravy

### 5) Monetizace

- affiliate / provize za proklik nebo objednávku
- premium listing pro prodejce
- B2B data API (cenový monitoring, benchmark)

### 6) Roadmapa

**Fáze 1 (MVP, 6–8 týdnů):**
- 5–10 největších značek + 10–20 e-shopů
- základní matching + jednotná karta produktu
- filtry: složení, metráž, cena/100 g, cena/100 m

**Fáze 2:**
- historie cen, alerty na pokles
- doporučené alternativy
- kvalita dat dashboard + ruční kurátorství

**Fáze 3:**
- komunitní vrstva (projekty, spotřeba příze, recenze)
- personalizace podle preferencí uživatele

## Evidence

- Zadání vychází přímo z uživatelského cíle: unifikovat data o přízích, nabídkách a výrobních definicích do jednoho nákupního místa s lepším filtrováním.

## Limitations

- Zatím nejsou zpracované konkrétní zdroje v `sources/`, takže návrh je konceptuální a není ověřen proti reálné struktuře dat jednotlivých výrobců a e-shopů.
- Právní rámec scrapingu a použití obrázků je nutné validovat pro cílové trhy.

## Connections

- [[index]]
- [[overview]]
