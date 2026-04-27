---
type: log
---

# Wiki Activity Log

Newest entries first.

## 2026-04-27

### Analysis: Carvago — osobní AI agenti a MCP strategie
- Zapsána strategická diskuse o budoucnosti osobních AI agentů a napojení firemních služeb.
- Doplněno doporučení pro Carvago: agent-ready přístup, postupné vystavení MCP (private pilot → controlled write → public discovery).
- Zachycen rámec pro discovery MCP endpointů (registry/store + enterprise allowlisty).
- Vytvořena stránka [[carvago-osobni-ai-agenti-mcp-strategie]].

### Operations: Upřesnění pravidla zapisování
- Aktualizována stránka [[projektova-pravidla-spoluprace]]: explicitně se zapisují i diskuzní otázky a strategické úvahy.

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
