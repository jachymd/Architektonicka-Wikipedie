---
title: "Profile Builder pro SketchUp — úvod a tvorba sestav (assemblies)"
date: 2026-08-28
tagy: ["sketchup-pluginy", "profile-builder", "sketchup"]
draft: false
---

**Profile Builder** je plugin do SketchUp na rychlou tvorbu opakujících se stavebních prvků — lišt, madel, plotů, zábradlí, rámovaných stěn a podobných věcí, které by se jinak musely v modelu skládat ručně kus po kuse. Tenhle záznam vychází z jednoho dílu seriálu o Profile Builderu (viz Zdroj níže), který se věnuje konkrétně tvorbě tzv. **smart assemblies** (chytrých sestav) — podle autora jde o jednu z nejsilnějších funkcí pluginu.

## Kde plugin najít

Oficiální stránka pluginu: [profilebuilder4sketchup.com](https://profilebuilder4sketchup.com/). Podle zdrojového videa by k němu měla být dostupná zkušební verze zdarma ("free trial") — než se pro plugin kancelář rozhodne naostro, stojí za to si ho takhle nejdřív vyzkoušet.

## Co plugin řeší

- **Profily** — 2D profil (průřez), který se vytáhne (extruduje) podél cesty do 3D tvaru.
- **Sestavy (assemblies)** — kombinace vytahovaného profilu a objektů, které se podél stejné cesty pravidelně opakují (typicky reálné věci: plot, zábradlí, rámovaná stěna). Přístupné přes druhé tlačítko v Profile Builderu — ikonu lupy, která otevře knihovnu ukázkových sestav.
- **Spany** — způsob, jak v rámci jedné sestavy rozmístit opakující se prvky rovnoměrně *mezi* pevnými podpěrami (sloupky), místo aby se opakovaly nezávisle na nich od začátku cesty.
- **Auto Assemble** — funkce, která se pokusí z vybraných objektů v modelu automaticky poskládat sestavu/profil sama.

## Jak na to

### Sestavy z knihovny a podél křivek

Po kliknutí na ikonu lupy v assembly dialogu se otevře knihovna ukázkových sestav (např. "crash barrier" — svodidlo). Po výběru a najetí myší se sestava kreslí podél přímé cesty. Stejně funguje volba **Build Along Path** pro vytažení podél existující křivky v modelu. Existující sestavu lze v modelu snadno zaměnit za jinou: vybrat objekt a použít **Apply Assembly Attributes with the Selection**.

### Vytvoření vlastní sestavy — jednoduchý příklad (rámovaná stěna)

Při návrhu sestavy je potřeba myslet na dvě věci: co se bude podél cesty *vytahovat* a co se bude *opakovat*. Postup na příkladu jednoduché rámované stěny:

1. Kliknutím na **+** založit novou sestavu.
2. Nejdřív přidat věc, která se má vytahovat — třeba spodní práh (base plate). V modelu je potřeba mít nakreslený profil (např. profil prkna 2×6" — 5,5" × 1,5"), pak ho přidat tlačítkem **+** u **Profile member** a zaregistrovat v profil manageru pod jménem (např. "2x6 board").
3. Zvolit, podle kterého bodu profilu se má umísťovat (střed, nebo třeba pravý dolní roh) — od téhle volby se pak odvíjí všechny další offsety.
4. Přes **Copy profile member** přidat další vytahovaný prvek (např. horní práh o 10 stop výš) — náhled sestavy se v panelu aktualizuje živě.
5. Doladit **up/down offset** a **setback** tak, aby celková výška/délka sestavy odpovídala realitě — protože se objekt umísťuje podle zvoleného bodu (např. spodní hrana), je nutné počítat i s tloušťkou profilu (příklad ze zdroje: aby stěna vyšla přesně na 10 stop, muselo se nastavit posunutí 9'10,5", ne rovných 10').
6. Pro opakující se svislé prvky (sloupky) je potřeba mít v modelu hotový **component** (ne group) — nakreslit geometrii, přes trojklik + pravé tlačítko udělat "Make Component" a pojmenovat.
7. Tenhle component přidat do sestavy přes **+** u **Component** a tlačítko **Pick**.
8. Nastavit **spacing** (rozteč) — např. 16" on center je standardní rozteč sloupků ve stěně — a doladit **up/down offset** (aby sloupek stál na prahu, ne pod ním) a **setback** na začátku/konci cesty (aby poslední sloupek nepřečníval za okraj). Tohle se často ladí metodou pokus/omyl s kladnými a zápornými hodnotami.

Takhle poskládaná sestava umí velmi rychle vygenerovat celou rámovanou stěnu se sloupky ve správné rozteči. Zdrojové video se záměrně omezuje na rovné úseky — rohy jsou složitější a měly by být tématem samostatného dílu (viz Navazující díly).

### Složitější příklad — zábradlí s pravidelně rozmístěnými sloupky (spany)

Druhý příklad ze zdroje je zábradlí: podpěrný sloupek (component) + spodní a horní madlo (profily) + svislé výplňové tyčky mezi sloupky.

Pokud se výplňové tyčky přidají jako obyčejný opakující se component (stejně jako sloupky v předchozím příkladu), vznikne problém: podpěrné sloupky sestavu "zaberou" nerovnoměrně a rozteč tyček mezi jednotlivými sloupky vyjde jinak než rozteč přes sloupek — vizuálně to není souměrné.

Řešení je **span**:

1. Nejdřív se v modelu vytvoří samostatná pomocná sestava, která obsahuje jen opakující se výplňovou tyčku s požadovanou roztečí (v příkladu 4").
2. V hlavní sestavě zábradlí se přes **+** u **Span** přidá nový span, jako typ se zvolí **subassembly** a vybere se tahle pomocná sestava.
3. Span pak výplňové tyčky mezi každými dvěma sousedními podpěrnými sloupky rozmístí rovnoměrně a "zastaví" přesně u sloupku, místo aby počítal rozteč od začátku celé cesty.
4. I tady je nutné doladit **up/down offset** a **end setback** (v příkladu měřením zjištěno, že bylo potřeba doladit o čtvrt palce), aby vzdálenost tyček od sloupku vyšla na obou stranách stejně.

Spany podle zdroje fungují i podél křivky, pokud se zaškrtne volba **Allow curve** — samotné rohové napojení (kde se cesta láme) je ale i v tomto případě složitější a je tématem, který autor nechává na samostatné video.

### Auto Assemble — automatické sestavení z vybraných objektů

Poslední volba se pokusí sestavu/profil poskládat automaticky z toho, co je v modelu vybrané — v ukázce ze zdroje ze dvou profilů umístěných u počátku modelu (jeden s hliníkovým materiálem jako základna, druhý jako sklo). Po výběru obou objektů a spuštění Auto Assemble se z nich zkusí vygenerovat hotový profil. Autor v videu sám poznamenává, že si není jistý, jak dobře tahle funkce funguje se spany — u profile members (a pravděpodobně i u components) ji ale označuje za užitečnou zkratku, jak sestavu poskládat rychle.

## Na co si dát pozor

- **Rohy (corners)** — u žádného z postupů výše zdroj neřeší napojení v rohu/na lomu cesty; autor to výslovně odkládá na samostatné video, které zatím k dispozici nemáme.
- **Bod umístění profilu** (roh/střed/hrana) zvolený při zakládání profilu ovlivňuje úplně všechny další offsety v sestavě — vyplatí se ho promyslet předem, ne ho měnit dodatečně.
- **Auto Assemble u spanů** — funkčnost není podle zdroje jistá, u profile members/components se ale dá zkusit jako rychlejší cesta k sestavení profilu.
- Tenhle záznam nepokrývá úplné základy práce s **profily** (první díly seriálu, na které toto video navazuje) ani instalaci pluginu — pokud to bude potřeba, doplníme jako samostatný navazující díl.

## Navazující díly

Zdrojové video navazuje na dřívější, začátečnický díl seriálu o Profile Builderu (základy práce s profily) — ten zatím v naší wiki zpracovaný není. Autor zároveň slibuje následující díl věnovaný právě rohům (corners) u sestav a spanů. Až se tyhle díly zpracují jako samostatné záznamy, propojíme je napřímo — mezitím je najdete pohromadě přes tag `profile-builder`, případně přes `sketchup-pluginy` všechny plugin návody dohromady.

## Zdroj

Přepis videa z YouTube kanálu **The SketchUp Essentials** (autor Justin), díl ze seriálu o Profile Builderu věnovaný tvorbě smart assemblies — přepis dodal uživatel, přímý odkaz na video zatím k dispozici není (doplnit, pokud bude potřeba zpětně dohledat).
