---
title: "Jak aktualizovat tento web"
date: 2026-08-27
tags: ["sprava-webu"]
---

Tenhle web je postavený jako statické stránky generované nástrojem **Hugo** z obyčejných textových (Markdown) souborů uložených v GitHub repozitáři. Žádná databáze, žádné heslované administrační rozhraní — úprava webu = úprava souboru + uložení do Gitu.

## Nejjednodušší cesta: přes Claude

1. Otevři konverzaci s Claude (Claude Code / Cowork), řekni mu, co chceš přidat nebo upravit (nový nástroj do HOW-TO, novou inspiraci, nový projekt...).
2. Claude vytvoří nebo upraví příslušný `.md` soubor podle šablony (viz `docs/navod-pro-claude.md` v kořeni repozitáře) a commitne změnu.
3. Po pushnutí do větve `main` se web automaticky znovu sestaví a nasadí (GitHub Actions, viz `.github/workflows/hugo.yml`) — během pár minut je změna vidět na živém webu.

## Ruční cesta (bez Claude)

1. V GitHubu otevři repozitář, najdi příslušnou složku v `content/` a buď uprav existující `.md` soubor přímo ve webovém editoru GitHubu, nebo vytvoř nový přes "Add file → Create new file".
2. Dodrž strukturu záhlaví (frontmatter mezi `---`) podle šablony v `archetypes/`.
3. Commitni změnu rovnou do `main` (na prototypu bez schvalování) — build se spustí automaticky.

## Lokální testování před publikováním

```
hugo server
```

otevře web na `http://localhost:1313/` a živě ukazuje změny při ukládání souborů — hodí se, když chceš vidět výsledek dřív, než ho pushneš na GitHub.
