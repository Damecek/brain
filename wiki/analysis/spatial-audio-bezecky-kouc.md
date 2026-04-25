---
type: analysis
title: "Nápad na aplikaci: Spatial audio běžecký kouč"
question: "Jak navrhnout aplikaci, která pomocí spatial audio motivuje běžce k udržení tempa bez sledování telefonu?"
tags: [running, audio, spatial-audio, motivation, product-idea]
created: 2026-04-24
updated: 2026-04-24
---

# Nápad na aplikaci: Spatial audio běžecký kouč

## Question / Framing
Cílem je navrhnout jednoduchou, nerušivou běžeckou aplikaci, která dává zpětnou vazbu na tempo čistě zvukem ve sluchátkách se spatial audio. Uživatel nemá během běhu sledovat displej a zpětná vazba má být intuitivní i při současném poslechu podcastu nebo hudby.

## Analysis
Aplikace pracuje s metaforou „virtuálního pronásledovatele“ (např. traktor, běžec, kolo, auto, kroky), jehož pozice ve 3D zvukovém prostoru se mění podle aktuální rychlosti běžce.

- Když běžec drží nebo zrychlí nad cílové tempo, zvuk se vzdaluje.
- Když běžec zpomalí pod cílové tempo, zvuk se přibližuje.
- Zvukový objekt má setrvačnost (záměrné zpoždění reakce), aby působil realisticky a podporoval plynulé tempo místo trhavých korekcí.

Tento model převádí abstraktní metriky tempa na přirozeně čitelný sluchový vjem. Běžec tak okamžitě „slyší výkon“, aniž by ho rušily hlasové pokyny nebo notifikace.

### Core value proposition
- **Eyes-free trénink:** žádné sledování telefonu/hodinek.
- **Jemná motivace:** místo direktivních povelů vzniká přirozený tlak na udržení tempa.
- **Nízká kognitivní zátěž:** vjem vzdálenosti je intuitivní i v únavě.
- **Kompatibilita s obsahem:** funguje jako vrstva nad podcastem/hudbou.

### Doporučené funkce
1. **Volba zvukových scén**
   - mechanické (traktor, motor),
   - sportovní (soupeř, kroky pelotonu),
   - neutrální (metronomická textura).
2. **Nastavení „chování pronásledovatele“**
   - citlivost na odchylku tempa,
   - setrvačnost a maximální přiblížení,
   - intenzita mixu vůči hudbě/podcastu.
3. **Tréninkové režimy**
   - steady pace,
   - intervaly,
   - negative split,
   - závodní simulace.
4. **Post-run analytika**
   - úseky, kde se pronásledovatel výrazně přibližoval,
   - úseky stabilního tempa,
   - trend zlepšení mezi běhy.
5. **Integrace dat**
   - GPS tempo,
   - tep (pro bezpečné limity),
   - export do běžeckých platforem.

### Product notes (relevant additions)
- Vhodné je přidat „pozitivní“ i „tlakové“ profily motivace (ne každému sedí stresová stimulace).
- Bezpečnostní režim by měl zachovat slyšitelnost okolí (doprava, varovné zvuky).
- Ovládání má být minimalistické: hlasem nebo gestem na sluchátku.

## Evidence
- Vychází z uživatelského zadání a popisu požadovaného zážitku (běh + spatial audio + motivace přes změnu vzdálenosti zvuku).

## Limitations
- Bez prototypu nelze potvrdit optimální psychoakustické parametry (míra přiblížení, rychlost reakce, únavová tolerance).
- Výsledek bude záviset na kvalitě implementace spatial audio u konkrétních sluchátek/OS.

## Connections
- [[overview]]
- [[index]]
