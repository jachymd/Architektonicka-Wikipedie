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
