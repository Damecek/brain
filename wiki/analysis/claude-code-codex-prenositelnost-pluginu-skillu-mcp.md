---
type: analysis
title: "Claude Code ↔ Codex: přenositelnost pluginů, skillů a MCP"
question: "Jak navrhnout přenositelnou vrstvu mezi Claude Code a Codexem, aby pluginy, skilly a MCP integrace šly sdílet, generovat a lintovat?"
tags: [claude-code, codex, mcp, skills, plugins, portability, developer-tools]
created: 2026-05-15
updated: 2026-05-15
---

# Claude Code ↔ Codex: přenositelnost pluginů, skillů a MCP

## Question / Framing

Aktuální problém je vendor lock-in na úrovni AI coding agent tooling vrstvy. V praxi už mohou existovat:
- MCP servery,
- repozitářové instrukce,
- skilly / slash commands,
- hooky / automatizace,
- lokální „marketplace-like" katalog interních rozšíření.

Cíl není jen jednorázový export z [[Claude Code]] do [[Codex]], ale **systematická přenositelná vrstva**, ve které:
- capability vznikne jednou,
- podle potřeby se vygeneruje konfigurace pro více agentů,
- setup jde lintovat a validovat,
- marketplace/discovery vrstva není navázaná jen na jednoho vendora.

## Analysis

### 1) Co je dnes opravdu přenositelné

Nejsilnější reálně existující standardizační vrstva je dnes **[[Model Context Protocol|MCP]]**.

Pokud je schopnost zabalena jako MCP server, lze ji principiálně používat jak v [[Claude Code]], tak v [[Codex]]. To je dnes nejbližší stavu „write once, run in both".

Praktický důsledek:
- **business capability** má být ideálně vyjádřena jako MCP tool/server,
- agent-specific věci mají být jen tenká integrační vrstva,
- marketplace/discovery se má primárně opírat o katalog MCP serverů, ne o proprietární plugin API jednoho nástroje.

### 2) Co přenositelné není, nebo jen omezeně

Plná přenositelnost dnes **není vyřešená** pro tyto vrstvy:

1. **Repo instrukce**
   - [[Claude Code]] používá `CLAUDE.md`.
   - [[Codex]] používá `AGENTS.md`.
   - Obě věci řeší podobný problém (repo guidance), ale nejsou totožné ve struktuře ani ve významových detailech.

2. **Skill / command vrstva**
   - Claude Code má skills / slash-command ekosystém a další workflow primitives.
   - Codex má také skills a slash commands.
   - Neexistuje ale dnes univerzální vendor-neutral formát, který by se dal přímo nainstalovat do obou prostředí bez transformace.

3. **Hooky a lokální automatizace**
   - Hook lifecycle, config surface a bezpečnostní model se mezi nástroji liší.
   - To znamená, že i když je záměr hooku stejný (lint po editaci, guardrails před během commandu), výsledná konfigurace nebývá 1:1 přenositelná.

4. **Subagenti / orchestration primitives**
   - Claude Code i Codex mají vlastní koncepty práce s agenty, ale ne jako sdílený runtime standard.

Shrnutí: **tools ano přes MCP, workflows ne bez adaptační vrstvy**.

### 3) Jaký je rozdíl mezi „pluginem", „skillem" a „MCP"

Pro návrh řešení je důležité tyto věci oddělit:

- **MCP** = standardizovaný protokol pro tool/server capability.
  - Nejlepší kandidát na přenositelnou vrstvu.

- **Skill** = reusable workflow/prompt/instruction balíček pro konkrétního agenta.
  - Částečně přenositelný jen konceptuálně.
  - Většinou potřebuje render do vendor-specific formátu.

- **Plugin / extension** = širší integrační balík, který může kombinovat tool capability, config, commands, hooks a UX discovery.
  - Tohle je právě vrstva, kde dnes chybí univerzální standard mezi Claude Code a Codexem.

Jinými slovy: **MCP řeší nástroje, ne celé chování agenta**.

### 4) Co z toho plyne pro produktový návrh

Pokud má vzniknout interní nebo veřejný „marketplace" mezi Claude Code a Codexem, nemá být postavený jako přímý export proprietárních extension formátů, ale jako **dvouvrstvý systém**:

#### Vrstva A — canonical manifest
Vendor-neutral manifest popisující:
- identitu capability,
- popis, tagy, verze,
- zda jde o MCP server / skill template / hook template,
- vstupní secrets a environment requirements,
- instalační instrukce,
- test/lint pravidla,
- mapování do konkrétních agent targetů.

#### Vrstva B — target adapters / generators
Generátory, které z canonical manifestu vytvoří:
- Claude Code variantu (`CLAUDE.md`, skills, slash commands, MCP config, hooks),
- Codex variantu (`AGENTS.md`, skills, slash commands, `config.toml`, MCP config),
- případně další targety do budoucna.

To znamená, že „plugin marketplace" je ve skutečnosti spíš:
- **registry manifestů**,
- **build/generation toolchain**,
- **validation/lint vrstva**,
- **publish/install adapters** pro jednotlivé agenty.

### 5) Doporučená architektura řešení

Nejrozumnější směr dnes:

1. **Capability-first design**
   - Každou sdílenou funkci vyjadřovat primárně jako MCP server/tool.

2. **Repo guidance as templates**
   - Držet canonical instrukce ve vlastním formátu a generovat z nich `CLAUDE.md` a `AGENTS.md`.

3. **Skill abstraction**
   - Nepokoušet se přenášet vendor-specific skill 1:1.
   - Místo toho mít interní deklarativní model typu:
     - trigger,
     - intent,
     - required tools,
     - examples,
     - safety notes,
     - output expectations.
   - Teprve z toho renderovat target-specific skill docs.

4. **Hook abstraction**
   - Hooky modelovat jako intenty (`post_edit_lint`, `pre_exec_guard`, `session_bootstrap`) a pak je překládat do konkrétních hook systémů.

5. **Installer / doctor / sync CLI**
   - Nástroj typu `agent-market sync`:
     - načte manifesty,
     - detekuje lokálně dostupné klienty,
     - vygeneruje configy,
     - zvaliduje dependencies,
     - zkontroluje drift,
     - provede sync do Claude Code / Codex setupu.

### 6) Dá se to lintovat?

Ano, ale pouze po vrstvách:

#### A. Lint canonical manifestu
- validace schema,
- required fields,
- versioning,
- dependency graph,
- conflict detection.

#### B. Lint MCP vrstvy
- validace MCP contractu,
- smoke test serveru,
- compatibility check,
- inspekce tool metadata.

#### C. Lint target renderů
- je vygenerovaný `CLAUDE.md` konzistentní s manifestem?
- je vygenerovaný `AGENTS.md` konzistentní s manifestem?
- sedí názvy commandů, env vars, secrets, paths, hook scripts?

#### D. Runtime doctor
- je MCP server dostupný?
- je tool opravdu registrovaný v Claude Code / Codex?
- nechybí binárka, token, config nebo env?

Takže ano: **linting je realistický, ale ne jako jeden univerzální linter pro všechno; spíš jako build pipeline s více validátory**.

### 7) Vyřešil už to někdo?

V současné době to vypadá takto:

- **Ano, částečně**: ekosystém už řeší přenositelnost tools přes MCP.
- **Ne, plně ne**: nenašel se silný standard nebo widely adopted projekt, který by automaticky migroval Claude Code extension/skill/config model do Codex modelu a opačně.
- Existují registry a discovery vrstvy pro MCP servery, ale ne univerzální marketplace pro kompletní agent behavior.

To znamená, že tržní mezera je skutečná.

### 8) Nejrealističtější produktová teze

Nejrealističtější produkt není „migrovat pluginy mezi Claude Code a Codexem" v doslovném smyslu, ale:

**vytvořit vendor-neutral agent capability registry + generator stack, kde MCP je portable core a agent-specific config je generated edge.**

To řeší:
- lock-in,
- opakovanou konfiguraci,
- auditovatelnost,
- linting,
- budoucí rozšíření i na další klienty mimo Claude Code / Codex.

### 9) Hlavní rizika

1. **Falešná 1:1 kompatibilita**
   - některé koncepty se mezi nástroji jen tváří podobně, ale nejsou ekvivalentní.

2. **Rychlý vývoj vendor feature setu**
   - capability surface se bude rychle měnit.

3. **Distribuce secrets a auth**
   - největší praktické bolesti nejsou v markdownu, ale v credentials, env vars a lokálních dependencích.

4. **Marketplace chicken-and-egg**
   - katalog bez installeru a bez validace má malou hodnotu; installer bez katalogu také.

### 10) Doporučený MVP

MVP by měl být záměrně úzký:

1. Canonical manifest v YAML/TOML/JSON.
2. Render jen do dvou targetů:
   - Claude Code
   - Codex
3. Scope pouze pro:
   - MCP server registration,
   - instruction file generation,
   - simple skill generation,
   - basic hooks.
4. `doctor` command a `lint` command.
5. Jednoduchý lokální/privátní registry index.

To stačí k ověření, zda je reálná hodnota ve:
- synchronizaci setupu,
- snížení driftu,
- vyšší přenositelnosti mezi agenty.

## Evidence

### Primární zjištění z aktuálního research
- Claude Code dokumentuje MCP, hooks, custom instructions a skill/slash-command rozšiřitelnost, ale nenašel se ekvivalent tradičního extension marketplace modelu.
- Codex dokumentuje `AGENTS.md`, MCP, skills, slash commands a konfigurační vrstvu přes `config.toml`, ale také nevyznívá jako hotový cross-agent plugin marketplace.
- MCP dnes funguje jako nejsilnější společný standard pro tool portability.

### Aktuální veřejné zdroje
- Claude Code MCP: https://docs.anthropic.com/en/docs/claude-code/mcp
- Claude Code hooks: https://docs.anthropic.com/en/docs/claude-code/hooks
- Claude Code skills / slash commands: https://docs.anthropic.com/en/docs/claude-code/slash-commands
- Codex CLI overview: https://developers.openai.com/codex/cli
- Codex AGENTS.md: https://developers.openai.com/codex/guides/agents-md
- Codex skills: https://developers.openai.com/codex/skills
- Codex slash commands: https://developers.openai.com/codex/cli/slash-commands
- Codex config reference: https://developers.openai.com/codex/config-reference
- Codex config docs in repo: https://github.com/openai/codex/blob/main/docs/config.md
- MCP intro: https://modelcontextprotocol.io/introduction
- MCP registry: https://github.com/modelcontextprotocol/registry
- MCP inspector: https://modelcontextprotocol.io/docs/tools/inspector
- MCP directories / marketplace-like discovery:
  - https://smithery.ai/
  - https://glama.ai/mcp/servers
  - https://www.pulsemcp.com/

## Limitations

- Research zachycuje veřejně dokumentovaný stav k 2026-05-15, ne neveřejné interní roadmapy vendorů.
- Některé capability surfaces se mohou rychle měnit, takže dlouhodobě stabilní nemusí být konkrétní naming, CLI flags ani config detail.
- Z veřejného webu není možné potvrdit skutečnou adoption a usage quality jednotlivých marketplace-like katalogů bez hlubšího market interview.

## Connections

- [[carvago-osobni-ai-agenti-mcp-strategie]]
- [[carvago-cli-mcp-filtr-url-chatgpt-aplikace]]
- [[projektova-pravidla-spoluprace]]
