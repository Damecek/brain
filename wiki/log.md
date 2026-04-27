---
type: log
---

# Wiki Activity Log

Newest entries first.

## 2026-04-27

### Update: Spatial audio běžecký kouč — změna na additive update (ponechání originálu)
- Upraven přístup na základě review: původní obsah stránky [[spatial-audio-bezecky-kouc]] byl zachován.
- Nové produktové body z Claude research byly přidány jako samostatná sekce **ADDITION (2026-04-27)** místo přepisu původní analýzy.
- Zachován původní framing, metadata i struktura; rozšíření je čistě aditivní.

### Update: Spatial audio běžecký kouč — zařazení do pace app produktu
- Přepracována stránka [[spatial-audio-bezecky-kouc]] do produktového tvaru pro pace app roadmapu.
- Doplněna explicitní rozhodnutí pro iOS-first rollout, validaci v terénu a následnou productizaci.
- Přidány implementační zásady: DRR/reverb jako primární distance cue, deadband + hysterese + setrvačnost.
- Zapsány guardrails pro bezpečnost, fallbacky bez head trackingu a hlavní go/no-go KPI pro beta fázi.

### Update: Spatial audio běžecký kouč — validace konceptu a MVP architektura
- Rozšířena analýza o validaci proveditelnosti na iOS/AirPods (spatial audio, head tracking, background usage).
- Doplněn positioning vůči existujícím kategoriím (voice coach, metronom, ghost pacer, motion-reactive audio).
- Přidána doporučená architektura s oddělením pacing engine, behavior mappingu a platformního rendereru.
- Zapsán matematický model řízení (filtrace rychlosti, log-speed error, saturace, setrvačnost).
- Přidána rizika a zúžený MVP scope pro praktické ověření v terénu.
- Aktualizována stránka [[spatial-audio-bezecky-kouc]].

## 2026-04-26

### Update: Yarn Unifikátor — user use case od reálné pletařky
- Doplněn use case s prioritizovanými kritérii výběru příze (materiál, tloušťka, hmotnost, barva, návin, certifikace, jehlice/háček, logistika).
- Přidán požadavek na synonym dictionary pro materiály (mohér/mohair, viscose/viskóza) a normalizaci diakritiky.
- Definovány implikace pro datový model: enumerace s aliasy, sticky merchant preference, geo-filtr CZ/SK.
- Aktualizována stránka [[yarn-unifikator-marketplace]].

## 2026-04-25

### Analysis: Carvago CLI + MCP výstupem URL filtru
- Zapsán návrh sjednocení Carvago CLI a Carvago MCP nad společným `filter_builder` jádrem.
- Definován společný výstup: canonical URL frontendového filtru + debug metadata.
- Popsána proveditelnost napojení přes MCP do ChatGPT aplikace.
- Vytvořena stránka [[carvago-cli-mcp-filtr-url-chatgpt-aplikace]].
- Aktualizován [[index]].

## 2026-04-24

### Analysis: Carvago LLM doporučení auta
- Zapsán nápad na personalizované doporučování aut na základě profilu zákazníka.
- Popsán návrh trénovacích dat (profil klienta → skutečně koupené auto).
- Přidán návrh inferenčního flow: predikce parametrů auta → matching na inventory / předvyplnění filtrů.
- Vytvořena stránka [[carvago-llm-doporuceni-aut]].

### Operations: Zavedeno pravidlo průběžného zapisování požadavků
- Vytvořena stránka [[projektova-pravidla-spoluprace]] s provozním pravidlem,
  že každý nový požadavek má být zapsán do projektu jako Markdown záznam.
- Doplněna praktická interpretace umístění záznamů v rámci wiki struktury.

### Analysis: Nápad na aplikaci Spatial audio běžecký kouč
- Zapsán produktový koncept běžecké aplikace se „zvukovým pronásledovatelem“.
- Popsán mechanismus motivace přes změnu vzdálenosti 3D zvuku podle tempa.
- Přidány doporučené funkce: režimy tréninku, citlivost, setrvačnost,
  post-run analytika, bezpečnostní režim a integrace dat.
- Vytvořena stránka [[spatial-audio-bezecky-kouc]].

## 2026-04-15

### Analysis: Yarn Unifikátor (marketplace přízí)
- Zapsán nový projektový nápad na datově unifikovaný e-shop přízí.
- Popsán návrh datového modelu (produkt, varianta, specifikace, nabídky, snapshoty).
- Přidána roadmapa MVP → F3, monetizace a klíčové UX filtry.
- Vytvořena stránka [[yarn-unifikator-marketplace]].

### Analysis: Nápad na mobilní hru (Had + slovní puzzle)
- Zapsán návrh herní mechaniky, kde had sbírá písmena a skládá validní slova.
- Popsána detekce slov v těle hada dopředu i dozadu.
- Přidán návrh bodování a roadmapa iterací (MVP → V4).
- Vytvořena stránka [[hadokrizovka-mobile-hra]].

### Maintenance: Wiki initialized
- Created wiki structure: `wiki/entities/`, `wiki/concepts/`, `wiki/topics/`,
  `wiki/sources/`, `wiki/analysis/`
- Created [[index]] and [[log]]
- Created [[overview]]
- Wiki is empty and ready for sources
