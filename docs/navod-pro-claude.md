# Návod pro Claude — jak zpracovávat nové záznamy

Tenhle soubor slouží jako instrukce pro Claude (nebo jiného spolupracovníka), když někdo přinese surový text, poznámky z návštěvy, nebo fotky a chce z toho hotový záznam do INSPIRACE nebo PROJEKTY.

## Postup

1. Zjisti, jestli jde o INSPIRACI (cizí, posbíraný podnět) nebo PROJEKT (Huť architektury / spolupracovníci). Podle toho zvol správnou složku (`content/inspirace/` nebo `content/projekty/`) a archetyp (`archetypes/inspirace.md` nebo `archetypes/projekt.md`).
2. Založ soubor příkazem `hugo new content <slozka>/<nazev-souboru>.md` — název souboru piš bez diakritiky, s pomlčkami místo mezer.
3. Vyplň frontmatter (pole mezi `---`):
   - **title** — krátký výstižný název.
   - **zdroj** — `inspirace` nebo `hut-architektury`.
   - **autor** — architekt / studio / autor, pokud je znám.
   - **rok** — rok realizace nebo vzniku.
   - **lokace** — místo.
   - **typ** — `realizace`, `nerealizace`, nebo `studie`.
   - **zdroj_informace** — `osobni-navsteva`, `casopis`, `online`, nebo `portfolio-studia`.
   - **tagy** — nejdřív zkontroluj `data/tags.yaml`. Pokud existující tag sedí, použij ho. Pokud opravdu chybí, přidej ho do `data/tags.yaml` (s krátkým popisem) a teprve pak ho použij v záznamu — nevytvářej tagy jen v jednotlivém záznamu bez zápisu do řízeného seznamu.
   - **obrazky** — seznam URL adres (obrázky se v tomto prototypu neukládají do repozitáře, ale odkazují se externě, kvůli omezené kapacitě budoucího hostingu).
   - jen u PROJEKTY navíc: **faze** (`studie`/`dsp`/`dps`/`realizace`/`dokonceno`) a **role** (`autor`/`spoluprace`/`konzultace`).
4. Do těla záznamu (pod frontmatter) napiš stručný, jasně strukturovaný popis — z případného chaotického vstupu (hlasová poznámka, útržkovité poznámky) udělej 3–6 souvislých vět: co je na tom zajímavé/důležité, jaký detail nebo princip stojí za zapamatování, proč by to mohlo být užitečné pro budoucí projekty kanceláře.
5. Nastav `draft: false`, až je záznam hotový (dokud `draft: true`, Hugo ho v produkčním buildu nezobrazí).
6. Commitni a pushni do `main` — GitHub Actions web automaticky znovu sestaví a nasadí.

## Zásady

- Nikdy nevymýšlej fakta (rok, autor, lokace) — pokud si uživatel není jistý, nech pole prázdné nebo napiš `"doplnit"` a uprav to v textu záznamu jako otevřenou otázku.
- Drž se řízeného seznamu tagů (`data/tags.yaml`), ať propojení mezi HOW-TO / INSPIRACE / PROJEKTY přes `/tagy/` zůstane spolehlivé.
- U sekce HOW-TO postupuj obdobně, ale bez pevného schématu polí — stačí `title`, `date`, `tagy` (viz `archetypes/default.md`) a věcný, stručně strukturovaný text/postup.

## Šablona pro návody na pluginy (HOW-TO → Workflow pro software)

Instruktáže konkrétních pluginů (typicky pro SketchUp, ale klidně i jiný software) drž ve stejné struktuře, ať se v nich tým dokáže zorientovat i po desáté a ať se dají budoucí díly propojovat přes tagy.

1. **Soubor a archetyp** — nový plugin = nový soubor `content/how-to/software-workflow/plugin-<nazev-pluginu>.md` (bez diakritiky, pomlčky místo mezer). Šablona je v `archetypes/plugin.md` (`hugo new content how-to/software-workflow/plugin-nazev.md --kind plugin`), ale při zpracování přes Claude stačí strukturu dodržet ručně.
2. **Tagy** — vždy dvě vrstvy: obecný tag pro danou kategorii pluginů (`sketchup-pluginy` pro SketchUp pluginy) + specifický tag pro konkrétní plugin (např. `profile-builder`). Díky tomu jde přes `/tagy/sketchup-pluginy/` najít přehled všech pluginů a přes `/tagy/profile-builder/` všechny díly o jednom konkrétním pluginu, i když jich časem přibude víc. Přidej i tematicky sedící tagy ze zavedeného seznamu (např. `sketchup`), pokud to dává smysl.
3. **Struktura těla článku** (nadpisy `##`, Hugo z nich samo vygeneruje obsah nahoře):
   - Krátký úvodní odstavec — co plugin je a k čemu v praxi slouží.
   - **Kde plugin najít** — oficiální odkaz, poznámka o zkušební verzi/ceně, pokud to zdroj zmiňuje.
   - **Co plugin řeší** — klíčové funkce jako odrážky.
   - **Jak na to** — krokové postupy; klidně víc podsekcí (`###`), pokud zdroj popisuje víc scénářů/příkladů.
   - **Na co si dát pozor** — caveaty, časté chyby, věci, které zdroj sám označuje za složitější nebo za téma "příště".
   - **Navazující díly** — pokud zdrojové video/článek je součástí série, napiš sem, co v seriálu předchází nebo bude následovat; jakmile ten díl vznikne jako vlastní záznam, prolinkuj ho napřímo (a spoléhej i na sdílený tag pluginu, který je propojí automaticky).
   - **Zdroj** — autor/kanál a odkaz, pokud ho uživatel dodal. Nikdy nevymýšlej URL, která nebyla dodaná — napiš, že chybí a ať se doplní.
4. Stejně jako jinde: nic nevymýšlej nad rámec zdroje (video, přepis, dokumentace pluginu) — pokud zdroj nepokrývá základy (třeba instalaci nebo úplný začátek), řekni to v článku otevřeně, ať to tým může doplnit v navazujícím díle.
