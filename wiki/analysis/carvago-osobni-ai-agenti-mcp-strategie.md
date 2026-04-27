---
type: analysis
title: "Carvago: osobní AI agenti a MCP strategie"
question: "Jak se budou firmy připojovat k osobním AI agentům, má Carvago vystavit MCP endpoint, a kde bude přehled MCP endpointů?"
tags: [carvago, ai-agents, mcp, strategy, platform]
created: 2026-04-27
updated: 2026-04-27
---

# Carvago: osobní AI agenti a MCP strategie

## Question / Framing

Uvažovaný scénář je budoucnost, kde většina uživatelů pracuje přes osobního AI agenta. Klíčové otázky:
- jak se na tyto agenty budou firmy produktově a technicky napojovat,
- zda je vhodné už nyní vystavit MCP endpoint,
- kde bude probíhat discovery MCP endpointů (registry/store model).

## Analysis

### 1) Jak se firmy pravděpodobně připojí k osobním agentům

Praktický model bude ve třech vrstvách:
1. **Core business API** (stabilní doménové operace firmy),
2. **Agent adapter vrstva** (např. MCP server),
3. **Discovery + trust vrstva** (registry/store, reputace, bezpečnostní politika klienta).

Z toho plyne doporučení nestavět vše na jednom protokolu, ale držet čisté doménové API a nad ním adaptér pro agentní ekosystémy.

### 2) Má Carvago vystavit MCP endpoint už teď?

Ano, ale postupně a řízeně:
- **Fáze 1 (private pilot):** interní nebo partner-only MCP pro read-only/useful operace (vyhledání aut, dostupnost, kalkulace).
- **Fáze 2 (controlled write):** omezené akce se silným authz, auditem a limity.
- **Fáze 3 (public discovery):** publikace do registru po ověření SLA, bezpečnosti a obchodního přínosu.

Tento postup minimalizuje riziko a umožní získat reálná data o chování agentů.

### 3) Kde bude „přehled endpointů"

Pravděpodobný stav trhu je pluralitní:
- oficiální/centrální registry jednoho standardu,
- vertikální katalogy,
- privátní enterprise registry a allowlisty.

Pro Carvago je praktické počítat s kombinací:
- veřejná dohledatelnost pouze pro bezpečné capability,
- citlivé operace ponechat za privátním onboardingem.

### 4) Co připravit hned

- **Tool contracty:** idempotence, timeouty, strukturované chyby, verzování schémat.
- **Identity & access:** OAuth + jemné scope per capability.
- **Policy gateway:** rate limiting, detekce zneužití, kill-switch.
- **Observability:** end-to-end telemetry (intent → tool call → business outcome).
- **Governance:** jasná pravidla, které akce jsou agent-autonomous a které vyžadují potvrzení uživatelem.

## Decision Notes

- Carvago by mělo být **agent-ready** nyní, i kdyby veřejný MCP endpoint přišel později.
- Největší práce nebude protokol, ale **trust, bezpečnost, měření výkonu a governance**.
- Discovery je vhodné chápat jako distribuované prostředí (ne jen jeden store).

## Connections

- [[carvago-cli-mcp-filtr-url-chatgpt-aplikace]]
- [[carvago-llm-doporuceni-aut]]
- [[projektova-pravidla-spoluprace]]
