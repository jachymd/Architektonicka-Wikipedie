# Architektonická wikipedie — Huť architektury

Interní nástroj pro živé a dynamické sdílení informací, nástrojů a archivaci inspirací kanceláře Huť architektury. Prototyp postavený jako statický web (Hugo) nad Markdown soubory v tomto repozitáři — bez databáze, editovatelný odkudkoli včetně přes Claude.

## Struktura

- **HOW-TO** (`content/how-to/`) — tři podsekce: komunikace a organizace práce, workflow pro software (včetně návodu na aktualizaci tohoto webu), dokumentace a ekonomické kalkulace.
- **INSPIRACE** (`content/inspirace/`) — archiv posbíraných inspirací.
- **PROJEKTY** (`content/projekty/`) — projekty Huti architektury, spolupracovníků a komunity.
- **Tagy** (`data/tags.yaml`) — řízený seznam tagů, propojuje záznamy napříč všemi třemi sekcemi (automatický rejstřík na `/tagy/`).

Podrobný návrh a zdůvodnění struktury: viz projektový dokument `navrh-struktury-webu.md` (uložený v Claude projektu „Architektonická wikipedie").

## Jak přidat nový záznam

```
hugo new content inspirace/nazev-inspirace.md
hugo new content projekty/nazev-projektu.md
hugo new content how-to/software-workflow/nazev-navodu.md
```

Vyplň frontmatter podle vzoru (viz `docs/navod-pro-claude.md` pro přesná pravidla, nebo nech vyplnit Claude). Vzorové hotové záznamy: `content/inspirace/ukazkovy-zaznam.md`, `content/projekty/ukazkovy-zaznam.md`.

## Lokální spuštění

```
hugo server
```

→ `http://localhost:1313/`

## Nasazení

- **Testovací provoz:** GitHub Pages, build přes `.github/workflows/hugo.yml` (spustí se automaticky při push do `main`). GitHub Pages nejde spustit z privátního repozitáře na GitHub Free plánu — repozitář je proto (dočasně, pro prototypovou fázi) nastavený jako **public**. V nastavení repozitáře je potřeba v **Settings → Pages** nastavit zdroj na "GitHub Actions" (`baseURL` v `hugo.toml` už je nastavená na `https://jachymd.github.io/Architektonicka-Wikipedie/`).
- **Ostrý provoz:** doména kosnar.eu / hosting Weglobe — nahrát obsah vygenerované složky `public/` (výstup `hugo --minify`) přes FTP/správu hostingu. Řešení limitu úložiště (obrázky mimo repozitář/hosting) je popsáno v projektovém dokumentu.

> Poznámka: dokud je repozitář public, je hotový web i zdrojový obsah viditelný komukoli na internetu (i když na něj nikdo neodkazuje). Až se bude nahrávat reálný interní obsah kanceláře, zvážit buď přepnutí zpět na privátní řešení (Cloudflare Pages + Access), nebo rovnou přesun na kosnar.eu s vlastním omezením přístupu.

## Poznámka k obrázkům

Obrázky se v tomto prototypu neukládají přímo do repozitáře — pole `obrazky` v záznamech odkazuje na externí URL, kvůli omezené kapacitě (5 GB) budoucího hostingu.
