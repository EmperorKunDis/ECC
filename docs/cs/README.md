# ECC česky

Tato složka je česká lokalizační vrstva pro fork projektu **Everything Claude Code (ECC)**. Záměrně žije odděleně od anglického upstream obsahu, aby šel fork bezpečně synchronizovat s původním repozitářem bez zbytečných konfliktů.

## Co je ECC

ECC je sada agentů, dovedností, pravidel, hooků, MCP konfigurací a instalačních nástrojů pro práci s AI coding harnessy, jako jsou Codex, Claude Code, Cursor, OpenCode, Gemini, Zed a další. Repo obsahuje hlavně:

- `agents/` — specializované subagenty pro plánování, review, bezpečnost, build chyby a doménové úkoly.
- `skills/` — znovupoužitelné workflow dovednosti a doménové postupy.
- `rules/` — obecná i jazykově specifická pravidla pro kvalitu, testy, bezpečnost a styl.
- `commands/` — legacy slash-command kompatibilní vrstva.
- `hooks/` — automatizace spouštěné během práce agenta.
- `scripts/` — instalační, validační, auditní a release utility.
- `docs/` — dlouhodobější dokumentace, release poznámky, lokalizace a architektura.

## Doporučený způsob zobrazení češtiny na GitHubu

Nejbezpečnější varianta pro fork je **nepřekládat celý kořenový `README.md`**; praktický kompromis je ponechat ho převážně upstreamový a přidat jen krátký český odkaz/přepínač nahoře. GitHub sice na hlavní stránce repozitáře vždy zobrazuje právě kořenový `README.md`, ale jeho úprava by při každém upstream updatu vytvářela konflikty. Místo toho:

1. Kořenový `README.md` držte převážně upstreamový.
2. Nahoře v `README.md` mějte jen krátký odkaz/přepínač na češtinu.
3. Český obsah udržujte v `docs/cs/`.
4. V popisu repozitáře nebo v poli **Website** na GitHubu můžete navíc odkazovat na `docs/cs/README.md` nebo na GitHub Pages stránku vytvořenou z této složky.

Detailní návrh synchronizace a režimů lokalizace je v dokumentu [FORK-STRATEGY.md](FORK-STRATEGY.md).

## Jak fork aktualizovat z upstreamu

Repo obsahuje workflow `.github/workflows/sync-upstream.yml`, které lze spustit ručně nebo plánovaně. Workflow:

1. přidá upstream remote `affaan-m/ECC`,
2. stáhne jeho výchozí větev,
3. pokusí se ji sloučit do aktuální větve forku,
4. zachová českou dokumentaci v `docs/cs/`,
5. pushne výsledek zpět do forku, pokud merge proběhne bez konfliktů.

Pokud merge selže, workflow skončí chybou a konflikt je nutné vyřešit ručně.
