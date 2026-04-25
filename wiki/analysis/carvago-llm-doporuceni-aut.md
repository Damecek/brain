---
type: analysis
title: "Carvago: LLM doporučení auta na míru"
created: 2026-04-24
updated: 2026-04-24
tags: [carvago, llm, personalization, recommendation]
---

# Carvago: LLM doporučení auta na míru

## Zadání (shrnutí nápadu)

Pro každého zákazníka na základě jeho profilu předpovědět auto, které si s nejvyšší pravděpodobností koupí.

Vstup:
- profil klienta (demografie, preference, potřeby, chování na webu, případně další dostupné atributy)

Výstup:
- parametrizovaný „datový model auta“ (značka, model, typ karoserie, pohon, cenové pásmo, stáří, nájezd, výbava apod.)
- následně buď:
  1. dohledání konkrétních vozů v aktuální nabídce,
  2. nebo automatické předvyplnění filtrů na frontendu.

## Navržený přístup

### 1) Trénovací data

Z interní databáze připravit dvojice:
- **X = profil klienta**
- **Y = auto, které skutečně koupil**

Každý historický nákup tvoří jeden trénovací příklad.

### 2) Modelování

Primární cíl:
- model, který pro vstupní profil vrátí doporučený parametrický profil auta.

Možné implementační varianty:
- fine-tuning LLM na strukturované mapování profil → auto,
- nebo hybrid (ranking/recommender model + LLM vrstva pro vysvětlení a práci s textovým vstupem).

### 3) Inferenční flow v produktu

1. Uživatel se přihlásí / vyplní vstupní údaje.
2. Model vrátí doporučený datový profil auta.
3. Systém:
   - spáruje profil s konkrétními skladovými vozy,
   - nebo otevře stránku s přednastavenými filtry.
4. Uživatel dostane personalizovaný shortlist.

## Produktová hodnota

- rychlejší orientace zákazníka v nabídce,
- vyšší relevance doporučení,
- potenciálně vyšší konverze na nákup,
- jednodušší cesta od vstupních preferencí ke konkrétnímu vozu.

## Poznámky k realizaci

- Klíčová aktivita: vytrénovat model, který z profilu zákazníka predikuje auto/parametry auta, které si chce koupit.
- Doporučení lze implementovat dvoukrokově: **predikce parametrů → matching na inventory**.
- Pro provoz je vhodné měřit kvalitu (např. CTR na doporučení, lead-to-sale, conversion uplift proti baseline).

## Otevřené otázky

- Jaké minimum atributů profilu stačí pro solidní predikci?
- Jak řešit cold-start u nových uživatelů?
- Jak vybalancovat business cíle (marže, stáří skladových vozů) vs. čistou relevanci pro zákazníka?
- Jak navrhnout transparentní vysvětlení doporučení pro uživatele?
