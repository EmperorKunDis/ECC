# Strategie českého forku ECC

## Cíl

Cílem je provozovat českou prezentační a dokumentační vrstvu na vlastním GitHub forku tak, aby:

- původní upstream repozitář zůstal co nejméně upravený,
- český obsah nekolidoval s anglickou dokumentací,
- fork šel pravidelně aktualizovat při každé nové upstream verzi,
- bylo jasné, co je lokální český overlay a co je převzatý upstream.

## Proč nepřekládat celý kořenový `README.md`

GitHub na hlavní stránce repozitáře zobrazuje kořenový `README.md`. To svádí k jeho překladu, ale u forku je to nejčastější zdroj konfliktů, protože upstream README se často mění při releasu, změnách badge, sponsorů, instalačních příkazů a release poznámek.

Doporučený model je proto:

- `README.md` ponechat převážně upstreamový,
- nahoře v `README.md` držet jen minimální český odkaz v jazykovém přepínači,
- českou dokumentaci držet v `docs/cs/`,
- na GitHubu odkazovat na český vstupní bod `docs/cs/README.md`,
- případně publikovat český vstup přes GitHub Pages.

Tento model minimalizuje konflikty, protože upstream zatím používá jazykové složky jako `docs/ja-JP/`, `docs/es/`, `docs/de-DE/`, `docs/tr/`, `docs/zh-CN/` a podobně. Samostatná složka `docs/cs/` je lokální overlay, který upstream merge typicky nepřepisuje.

## Doporučená struktura

```text
docs/cs/
  README.md          # český vstupní bod pro GitHub návštěvníky
  FORK-STRATEGY.md   # tento dokument: provozní plán forku
```

Pokud bude potřeba překládat víc, přidávejte české soubory zrcadlově podle upstream dokumentace:

```text
docs/cs/releases/2.0.0/release-notes.md
docs/cs/rules/common/security.md
docs/cs/rules/common/testing.md
```

Nepřesouvejte upstream soubory a nepřepisujte anglické originály. Česká vrstva má být přídavná, ne náhradní.

## Režimy zobrazení na GitHubu

### Režim A — nejméně konfliktní

Použijte `docs/cs/README.md` jako český landing page a vložte odkaz do horního jazykového přepínače v `README.md`. Volitelně stejný odkaz přidejte i do GitHub polí **Description** nebo **Website**.

Výhody:

- téměř žádné konflikty při merge z upstreamu,
- jasné oddělení originálu a českého forku,
- jednoduché ruční i automatické aktualizace.

Nevýhoda:

- hlavní README na stránce repozitáře zůstane převážně anglické, protože GitHub neumí pro fork nastavit jiný README bez změny souboru; český přepínač je ale viditelný nahoře.

### Režim B — GitHub Pages pro českou prezentaci

Publikujte `docs/cs/README.md` přes GitHub Pages a nastavte odkaz do pole **Website**.

Výhody:

- český veřejný vstup má vlastní URL,
- kořenový README může zůstat upstreamový,
- stránka může časem dostat vlastní navigaci bez zásahu do originální dokumentace.

Nevýhoda:

- vyžaduje zapnout GitHub Pages v nastavení repozitáře nebo doplnit samostatný deploy workflow.

### Režim C — celý český kořenový README

Kořenový `README.md` lze kompletně přeložit do češtiny.

Výhody:

- návštěvník uvidí plnou češtinu ihned na hlavní stránce forku.

Nevýhody:

- vyšší riziko konfliktů,
- upstream změny README bude nutné častěji řešit ručně,
- fork se bude více odchylovat od originálu.

Tento režim nedoporučujeme pro pravidelně synchronizovaný fork. Lepší kompromis je malý český přepínač v kořenovém README a vlastní český obsah v `docs/cs/`.

## Publikace změn na GitHub fork

Lokální commit s českým přepínačem nestačí. GitHub zobrazí změny až po pushi do vzdáleného forku a po merge do větve, kterou právě sledujete, typicky `main`. Pokud `git remote -v` neukazuje žádný `origin`, nelze z lokálního prostředí vytvořit skutečný GitHub pull request ani pushnout změny.

Doporučený postup:

```bash
git remote add origin git@github.com:<vas-ucet>/ECC.git
git push -u origin work
```

Následně otevřete pull request `work` → `main` v GitHub UI. Po merge do `main` bude český přepínač viditelný v root README.

## Automatická synchronizace s upstreamem

Workflow `.github/workflows/sync-upstream.yml` je navrženo jako jednoduchý bezpečný merge bot pro fork. Spouští se ručně přes `workflow_dispatch` a plánovaně jednou denně.

Základní postup workflow:

1. Checkout aktuální větve forku.
2. Přidání upstream remote `https://github.com/affaan-m/ECC.git`.
3. Zjištění výchozí upstream větve.
4. `git fetch upstream`.
5. `git merge --no-edit upstream/<default-branch>`.
6. Push zpět do stejné větve forku.

Pokud se objeví konflikt, workflow se zastaví a nic nepushuje. To je záměr: konflikt je lepší vyřešit vědomě než automaticky zahodit upstream nebo český overlay.

## Doporučený provozní postup

1. V GitHub nastavení forku povolte workflow oprávnění **Read and write permissions**.
2. Zkontrolujte, že default branch forku je větev, kterou chcete aktualizovat.
3. Poprvé spusťte workflow ručně.
4. Pokud projde, nechte zapnutý denní plán.
5. Český obsah přidávejte pouze do `docs/cs/` nebo do jasně označených overlay souborů.
6. Při nové upstream lokalizaci češtiny rozhodněte, zda lokální `docs/cs/` zachovat jako fork overlay, nebo jej sjednotit s upstream verzí.

## Řešení konfliktů

Nejčastější konflikty vzniknou, pokud upravíte stejné soubory jako upstream. Priorita řešení:

1. Upstream produktové soubory (`scripts/`, `skills/`, `agents/`, `rules/`, package metadata) preferujte z upstreamu.
2. Lokální české dokumenty v `docs/cs/` preferujte z forku.
3. Kořenový `README.md` držte ideálně upstreamový kromě krátkého českého přepínače; delší české změny přesouvejte do `docs/cs/`.
4. Po ručním merge spusťte validační testy podle `package.json`.

## Shrnutí doporučení

Nejlepší kompromis je **krátký český přepínač v kořenovém `README.md` + český overlay v `docs/cs/` + automatický upstream merge workflow + volitelný odkaz z GitHub metadat nebo Pages**. Tento přístup splňuje českou prezentaci, zachovává vazbu na originální ECC a minimalizuje konflikty při nových verzích.
