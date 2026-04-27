---
type: analysis
title: "Nápad na aplikaci: Spatial audio běžecký kouč"
question: "Jak navrhnout aplikaci, která pomocí spatial audio motivuje běžce k udržení tempa bez sledování telefonu?"
tags: [running, audio, spatial-audio, motivation, product-idea, ios, validation]
created: 2026-04-24
updated: 2026-04-27
---

# Nápad na aplikaci: Spatial audio běžecký kouč

## Question / Framing
Cílem je navrhnout nerušivou běžeckou aplikaci, která dává zpětnou vazbu na tempo čistě zvukem ve sluchátkách se spatial audio. Uživatel nemá během běhu sledovat displej a feedback má fungovat i při současném poslechu hudby nebo podcastu.

## Analysis

### Validace konceptu (high-level)
Koncept je **technicky realizovatelný** a dává smysl i z pohledu trhu, ale hlavní nejistota není v API, nýbrž v psychoakustice a UX čitelnosti během reálného běhu. Prakticky: problém není „jestli to jde postavit“, ale „jestli to bude při únavě, ruchu a současném poslechu cizího audia konzistentně srozumitelné“. 

### Product model: „virtuální pronásledovatel“
Aplikace pracuje s neverbální metaforou zvukového objektu (běžec, kroky, mechanický zvuk), jehož pozice a charakter se mění podle odchylky od cílového tempa.

- Držím/zrychluji nad target → zvuk ustupuje.
- Zpomaluji pod target → zvuk se přibližuje.
- Reakce má řízenou setrvačnost (ne okamžité skoky), aby nevznikaly trhavé korekce.

Tím se tempo převádí na průběžně čitelný auditivní signál bez hlasových pokynů.

### Proveditelnost na iOS / AirPods
MVP jde postavit čistě přes veřejné Apple API:

- `AVAudioEnvironmentNode` pro 3D audio scénu.
- Head-tracking cesta přes iOS APIs (včetně podpory pro kompatibilní AirPods).
- Audio session režim pro míchání s jiným audiem (`mixWithOthers`) + volitelný ducking.
- Background běh přes location/workout stack tak, aby app fungovala i se zamčeným displejem.

Důsledek pro produkt: **Apple-first MVP** je realistický bez reverse engineeringu.

### Tržní prostor a positioning
Kategorie není prázdná, ale je otevřená konkrétní kombinace:

- existují voice pace kouči,
- metronomy/cadence appky,
- ghost pacers,
- motion-reactive hudební appky,
- spatial audio navigace.

Co je odlišující: **průběžný neverbální pace feedback ve spatial audio vrstvě**, který funguje jako samostatný „regulační kanál“ nad hudbou/podcastem.

### Architektura (oddělené vrstvy)
Doporučené členění:

1. **Pacing engine** (platform-agnostic): odhad rychlosti, filtrace, výpočet odchylky.
2. **Behavior mapping**: převod odchylky tempa na cílový stav zvuku (vzdálenost, laterální offset, intenzita).
3. **Audio renderer** (platform-specific): iOS spatial stack nyní, později Android spatializer.
4. **Session/data layer**: tréninkové režimy, historie běhů, analytika.

Toto rozdělení umožní i budoucí Android variantu bez přepisování jádra chování.

### Matematika řízení (MVP-friendly)
Pro stabilitu je vhodné počítat interně ve speed domain (m/s), ne přímo v min/km:

\[
v^* = 1000 / p^*
\]

\[
\tilde v_t = \alpha \hat v_t + (1-\alpha)\tilde v_{t-1}
\]

\[
e_t = \log\left(\frac{\tilde v_t}{v^*_t}\right)
\]

\[
r_t^* = r_0 + \Delta r\,\tanh(\beta e_t)
\]

\[
r_{t+1} = r_t + \frac{\Delta t}{\tau_r}(r_t^* - r_t)
\]

Interpretace:
- low-pass filtr rychlosti tlumí GPS jitter,
- log-speed chyba dává symetričtější reakci na relativní odchylky,
- `tanh` saturace drží zvuk v uvěřitelném prostoru,
- časová konstanta `\tau_r` dělá plynulé „dohánění“ místo skoků.

### Psychoakustické implikace pro design
Kritický poznatek: samotná změna hlasitosti nestačí.

Základní cue set má kombinovat alespoň:
- azimut/laterální offset,
- vzdálenost + gain,
- poměr early/late energie,
- jemnou změnu barvy zvuku (timbre/transient).

Pro čitelnost je vhodné začínat mírně mimo zadní osu (např. za ramenem), ne tvrdě přímo za hlavou. UX cíl nemá být „uživatel přesně slyší metry“, ale „spolehlivě cítí trend tlaku: roste / drží / klesá“.

### Tréninkové režimy
Stejný model obslouží:
- **steady pace** (konstanta),
- **intervaly** (kusově konstantní target),
- **negative split** (postupná rampa),
- **race simulation** (ghost trajectory nad historickým během).

### Největší rizika
1. **Nestabilní pace estimate v terénu** (GPS podmínky, model telefonu, prostředí).
2. **Psychoakustická křehkost** (komprimované vnímání vzdálenosti, slabší rear-space cue).
3. **Ecosystem variabilita** mimo Apple-first konfiguraci.
4. **IP/positioning**: broad prior art existuje, edge je v konkrétním interaction modelu a kvalitě UX.

### Doporučený MVP scope
- iPhone-first,
- steady pace + intervaly,
- 1–2 zvukové profily,
- silná filtrace + confidence/hold režim při nízké kvalitě odhadu,
- kompatibilita s hudbou/podcastem,
- ovládání přes lock screen/remote commands/hlas.

## Evidence
- Vstupní research shrnuje technickou proveditelnost (iOS spatial audio + head tracking + background běh), existenci sousedních kategorií na trhu a relevantní výzkumné směry (auditory biofeedback, pacing behavior, psychoakustika distance cue).
- Závěr: validace je pozitivní, diferenciace je realistická, ale úspěch bude záviset na kvalitě behavior mappingu a psychoakustického ladění.

## Limitations
- Bez interního prototypu a user testů zatím nelze potvrdit optimální mapování odchylky tempa na zvukový tlak.
- Citlivost řešení bude závislá na hardware kombinaci (sluchátka/OS), prostředí běhu a kvalitě pace estimace.

## Connections
- [[overview]]
- [[index]]
- [[projektova-pravidla-spoluprace]]
