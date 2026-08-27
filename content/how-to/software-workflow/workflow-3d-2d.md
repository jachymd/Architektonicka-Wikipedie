---
title: "Workflow tvorby 3D modelu a 2D dokumentace (SketchUp / BricsCAD / Twinmotion / Affinity)"
date: 2026-08-27
tagy: ["sketchup", "bricscad", "twinmotion", "affinity", "xref", "texturovani", "pojmenovani-souboru"]
draft: false
---

*Interní pracovní manuál — pracovní verze 0.1, zpracováno srpen 2026. Stav: pracovní verze — k diskuzi a doladění v týmu, než se stane firemním standardem.*

Tento manuál stanovuje jednotný postup, jak v kanceláři stavět, tagovat, exportovat a dokumentovat 3D architektonické studie — od prvního objemového modelu v SketchUp až po rendery a tištěné výkresové sady. Řeší tři opakující se problémy: modely, které se rozpadnou, když je otevře kolega; exporty do BricsCAD nebo Twinmotion, které přenesou špatné věci nebo nic použitelného; a 2D výkresy, které je nutné při každé změně modelu kreslit znovu od nuly.

Jde o pracovní verzi. Sekce označené jako "k rozpracování" jsou záměrně stručné — pokrývají oblasti, které v kanceláři zatím nejsou standardizované, a měly by se doplnit až se shodneme na konvencích. Vše ostatní popisuje doporučený výchozí postup: od něj se lze odchýlit, pokud to projekt skutečně vyžaduje, ale vědomě, ne omylem.

> **Pořadí čtení:** Noví členové týmu by si měli v celém rozsahu přečíst části A a B ještě před rozpracováním projektového modelu. Části C a D jsou referenční — používají se ve chvíli, kdy se dostanete k dokumentaci nebo renderování, nemusí se číst od začátku do konce.

## 1. Role jednotlivých softwarů — přehled

Tuto hierarchii mějte na paměti po celou dobu: SketchUp je jediné místo, kde se 3D model skutečně staví a udržuje. Každá další aplikace čerpá z něčeho, co z něj vychází — nikdy by se nemělo modelovat od nuly v BricsCAD nebo Twinmotion, pokud pro danou studii už existuje model ve SketchUp.

| Software | Hlavní role v procesu | Vstup → výstup |
|---|---|---|
| SketchUp | Jediný zdroj pravdy pro 3D model: geometrie, struktura komponent, Tags, materiály. | Výchozí bod → vše ostatní |
| BricsCAD | Přesná 2D projektová dokumentace (půdorysy, řezy, pohledy, detaily, výkresové listy). | Přebírá 2D linework ze SketchUp |
| Twinmotion | Real-time rendering, průchody modelem, prezentační vizualizace. | Přebírá živý 3D model přes Datasmith |
| Affinity (Photo / Designer / Publisher) | Postprodukce renderů a finální sazba publikačních listů / panelů. | Přebírá rendery (Twinmotion) a vektorové výkresy (BricsCAD) |

## Část A — Struktura modelu ve SketchUp

### A.1 Základní princip

Strom komponent stavějte tak, aby odpovídal skutečné budově, ne procesu modelování, kterým vznikl. Kolega, který váš soubor nikdy neotevřel, by měl umět rozbalit Outliner a rozumět tomu, na co se dívá: Site, Structure, Envelope, Openings, Interior, Furniture — v tomto pořadí a vnořené tak, jak se stavba skutečně skládá. Právě to dělá model znovupoužitelným pro export, pro výkazy a pro kolegu, který na projektu naváže.

### A.2 Hierarchie komponent

Použijte následující členění nejvyšší úrovně jako firemní výchozí standard. Každý projektový model by měl tyto skupiny mít v Outlineru na nejvyšší úrovni, i když jsou u konkrétního projektu některé prázdné.

| Tag | Obsahuje (typické komponenty) | Poznámky |
|---|---|---|
| 00_TEREN | Terén / topografie, hranice pozemku, sadové úpravy a zpevněné plochy, mobiliář, okolní kontextová hmota | Kontextová hmota okolí má být jednoduchý placeholder s nízkým detailem — nikdy nemodelovat na stejném LOD jako řešená budova. |
| 01_KONSTRUKCE_NOSNE | Základy / podkladní deska, nosné sloupy a průvlaky, stropní desky (po podlažích), konstrukce střechy | Rozdělit po podlažích, pokud má budova víc než jedno patro, aby šlo izolovat jedno podlaží. |
| 02_KONSTRUKCE | Nosná vrstva obvodové stěny, tepelná izolace, fasádní obklad / omítka, střešní krytina a hydroizolace | Izolaci modelovat jako vlastní vnořenou komponentu i když je tenká — bude potřeba později pro poznámky k U-hodnotám a férovost renderů. |
| 03_OTVORY | Okna (podle typu), vstupní/venkovní dveře, střešní okna | Modelovat buď jako komponenty vnořené do stěny, nebo jako samostatné komponenty umístěné do otvoru ve stěně — zvolit jednu konvenci a držet se jí (viz A.4). |
| 04_INTERIER | Příčky, vnitřní dveře, podlahové nášlapné vrstvy (po místnostech), podhledy (po místnostech) | Podskupiny podle místností se později vyplatí při předávání výkazů ploch a povrchů. |
| 05_VYBAVENI | Vestavěný nábytek, volný nábytek, zařizovací předměty, kuchyňské spotřebiče | Vždy jako vlastní skupina nejvyšší úrovně — první věc, kterou vypínáte při exportu pro konstrukci / do BricsCAD. |
| 07_DOKUMENTACE | Section Planes, pomocná geometrie pro kótování, úrovně / rastr os, značky kamer | Nikdy se neexportuje do Twinmotion; obvykle se vylučuje i z exportů do BricsCAD. |
| 08_PODKLADY | Importované CAD podklady, hmotové studie, skicovní geometrie, která už není aktivní | Karanténní zóna — vše zde je kandidát na smazání, než je model považován za čistý. |

### A.3 Konvence pojmenování

Pojmenujte každou komponentu a skupinu — výchozí pojmenování SketchUp "Component#123" je jednoznačně nejčastější příčina nepoužitelných Outlinerů a neúspěšných exportů (nepojmenované prvky se do BricsCAD mnohem častěji importují jako anonymní, needitovatelné bloky). Používejte krátký prefix odpovídající kódům skupin výše, následovaný srozumitelným popisem:

| Prefix | Příklad názvu | Použití |
|---|---|---|
| 00_Teren_ | 00_Teren_Okoli | Prvky terénu a sadových úprav |
| 01_Kce_ | 01_Kce_Zaklady | Konstrukční prvky, se suffixem podlaží |
| 02_Obv_ | 02_Obv_Izolace | Skladby obvodového pláště |
| 03_Otvor_ | 03_Otvor_OknoObyvak | Okna / dveře, rozměr uveden přímo v názvu, kde je to užitečné |
| 05_Nabytek_ | 05_Nabytek_SofaObyvak | Nábytek a FF&E |

*Poznámka k tabulce: aktuální verze manuálu neobsahuje řádek pro prefix `04_INTERIER` (na rozdíl od tabulky v A.2, kde tato skupina je) — vypadá to na přehlédnutí při editaci, stojí za doplnění v týmu.*

### A.4 Pravidla pro seskupování a vnořování

- Veškerá geometrie musí být uvnitř skupiny nebo komponenty — nikdy nenechávejte volné hrany/plochy v modelu bez seskupení. Volná geometrie je příčinou nechtěného "slepování" při dotyku prvků.
- Vnořujte cíleně: komponenta stěny může obsahovat vnořené subkomponenty pro izolaci a povrchovou úpravu, ale vyhněte se vnořování hlubšímu než 3–4 úrovně — pak se špatně vybírá i čistě exportuje.
- V rámci kanceláře se jednou rozhodněte, zda se otvory (okna/dveře) prostupují a vnořují přímo do komponenty stěny, nebo se modelují jako samostatné komponenty pouze umístěné do otvoru. Vnoření je přehlednější v Outlineru; samostatné komponenty se snáz zaměňují nebo zapisují do výkazů. Funguje obojí — důležitější než volba samotná je důslednost.
- Před každým důležitým uložením vyčistěte nepoužité komponenty a materiály (Window ▸ Model Info ▸ Statistics ▸ Purge Unused, nebo položka Purge Unused v menu File). Udržuje to velikost souboru i exportu pod kontrolou.

### A.5 Struktura Tags

Tags (dříve "Layers") řídí, co je vidět a co je vypnuté — během modelování, a zásadně také ve chvíli exportu do BricsCAD nebo synchronizace do Twinmotion. Vytvořte tag folders zrcadlící hierarchii komponent z A.2, aby zapnutí/vypnutí celé kategorie bylo otázkou jednoho kliknutí, ne pátrání v plochém seznamu.

| Tag folder | Příklad Tags uvnitř | Doporučená barva |
|---|---|---|
| *(k doplnění)* | *(k doplnění)* | Okrová |
| *(k doplnění)* | *(k doplnění)* | Šedá |
| *(k doplnění)* | *(k doplnění)* | Cihlově červená |
| *(k doplnění)* | *(k doplnění)* | Nebesky modrá |
| *(k doplnění)* | *(k doplnění)* | Písková |
| *(k doplnění)* | *(k doplnění)* | Petrolejová |
| *(k doplnění)* | *(k doplnění)* | Jantarová |
| *(k doplnění)* | *(k doplnění)* | Purpurová |
| *(k doplnění)* | *(k doplnění)* | Světle šedá |

*Poznámka: sloupce "Tag folder" a "Příklad Tags uvnitř" jsou v aktuální verzi manuálu prázdné (zůstaly jen barvy) — pravděpodobně se čeká na sladění s novými kódy z A.2/A.3. Doplnit, až bude jasné.*

> **Zlaté pravidlo:** Tags přiřazujte pouze skupinám a komponentám — nikdy syrové, neseskupené geometrii. "Untagged" nechte jako tag, na kterém začíná každá nová editace; pokud kreslíte novou geometrii, nejdřív ji seskupte a teprve pak otagujte skupinu. Přímé tagování jednotlivých hran je nejčastější příčinou toho, že tag spolehlivě neskryje vše, co má.

### A.6 Scenes jako přednastavení viditelnosti

Kombinace viditelnosti Tags ukládejte jako Scenes (Camera ▸ Scenes), místo abyste tags přepínali ručně pokaždé znovu. Jako minimum udržujte:

- **Design – All Visible** — vše zapnuto, pro modelování.
- **Structure Only** — zapnuto Site, Structure, Envelope, Openings; vypnuto Interior, Furniture, Documentation, Reference. Užitečné pro rané posouzení hmoty a konstrukční koordinaci.
- **Export – BricsCAD 2D** — doladěno pro konkrétní výkres (půdorys / řez / pohled), s Documentation aids a Reference geometrií viditelnou jen tam, kde se má skutečně tisknout, a Furniture zapnutým nebo vypnutým podle toho, jestli jde o zařízený půdorys.
- **Export – Twinmotion** — Reference a Documentation aids vypnuté, vše ostatní zapnuté; toto je stav viditelnosti, který se přenese při synchronizaci Datasmith (viz B.2).

Scenes pojmenovávejte popisně a seznam držte krátký — scéna, které za půl roku nikdo nerozumí, je horší než žádná scéna.

## Část B — Export modelu

### B.1 SketchUp → BricsCAD: co skutečně funguje

Přímý 3D import souboru .skp do BricsCAD je lákavý, ale v současnosti nespolehlivý pro cokoliv, co chcete dál editovat. Podle testů i zkušeností dalších kanceláří přicházejí komponenty ze SketchUp do BricsCAD jako meshe a z velké části jako anonymní, nepojmenované bloky — názvy komponent a barvy tagů se často nepřenesou čistě a BricsCAD navíc s mesh geometrií špatně pracuje při operacích jako je tvorba řezů.

> **Doporučení:** Přímý import .skp do BricsCAD nepoužívejte jako workflow pro dokumentaci. Vyhraďte ho maximálně pro orientační vizuální kontrolu. Pro reálnou tvorbu výkresů použijte cestu přes 2D export popsanou v Části C — je to víc práce dopředu, ale je to jediný postup, který skutečně vydrží přes více revizí.

Pokud projekt opravdu potřebuje plnou 3D výměnu geometrie s BricsCAD (např. konstrukční koordinace), spolehlivější cesty jsou výměna přes IFC, nebo modul BricsCAD Communicator, pokud ho vaše licence obsahuje (BricsCAD Platinum a vyšší) — obě varianty zachovávají strukturu produktu mnohem lépe než prostý import .skp. Berte to jako výjimku, ne jako výchozí workflow pro dokumentaci.

## Část C — Strategie 2D dokumentace

### C.1 LayOut nebo BricsCAD — skutečný kompromis

Oba nástroje mají své reálné místo; volba není o tom, který je "lepší", ale o tom, jakou cenu jste ochotni nést.

| | LayOut (SketchUp) | BricsCAD |
|---|---|---|
| Propojení s 3D modelem | Nativní živé viewporty odkazující na soubor SketchUp; upozorní, když jsou neaktuální, aktualizace na vyžádání. | Žádné nativní živé propojení. Každá změna ve SketchUp vyžaduje nový cyklus exportu/importu. |
| Přesnost kreslení | Základní kótování a popisky; není stavěno na dokumentaci ve stavebním standardu. | Plnohodnotný CAD nástroj: přesné kótování, šrafy, detaily, standardy výkresových listů a razítek. |
| Stabilita | Náchylný ke zpomalení a pádům u složitých modelů nebo výkresových sad s mnoha viewporty. | Stabilní i u rozsáhlých, detailně propracovaných výkresů. |
| Vhodné pro | Rychlé koncepční listy, rané prezentační půdorysy, interní tiskové kontroly. | Dokumentace ve stavebním standardu: detailní půdorysy, řezy, pohledy, detaily, předávání projektantům. |

> **Pracovní pravidlo:** LayOut používejte pro rychlý, méně formální výstup, kde je živé propojení důležitější než kresličská preciznost. BricsCAD, přes propojený workflow níže, používejte pro vše, co skutečně půjde ven jako stavební nebo koordinační výkres.

### C.2 Problém s aktualizací a funkční řešení

Obava, že BricsCAD ztratí odkaz na model při každé jeho změně, je oprávněná u obyčejného importu — lze se jí ale vyhnout použitím External Reference (Xref) místo importu. Exportujte pokaždé na stejnou, pevně danou cestu k souboru DWG/DXF a nechte BricsCAD zacházet s ním jako s živě se znovunačítající referencí, ne jako s jednorázovým vložením.

- Vyexportujte potřebný 2D linework ze SketchUp (viz C.3) na pevnou, předvídatelnou cestu k souboru — například `Exports/DWG/SEC-A-A.dxf` — a vždy přepisujte tentýž soubor, místo abyste u každé revize vytvářeli nový.
- V BricsCAD tento soubor vložte jako Xref (External Reference), ne jako obyčejný Insert, a nikdy ho nerozbalujte (explode).
- Veškeré kótování, šrafy, poché, popisky i práci s razítkem dělejte ve výkresu hostitele, na vlastních vrstvách — nikdy uvnitř samotného Xref.
- Když se model ve SketchUp změní: spusťte export znovu a přepište tentýž soubor. Otevřete (nebo znovu načtěte) výkres v BricsCAD — Xref se aktualizuje automaticky a vše, co jste nad ním přidali, zůstává přesně tam, kde to bylo.
- Po výraznější změně geometrie zkontrolujte okem kóty a hranice šraf — kóty na obyčejném linework nejsou parametricky asociativní, takže u přesunuté stěny je nutné kótu znovu nabrat, i když se výkres samotný aktualizoval.

Tento postup ruční čištění zcela neodstraní, ale zabrání tomu, aby se tato práce při každé revizi zahodila a dělala znovu — a právě to je ta skutečná otrava, které se chcete vyhnout.

### C.3 Export řezů a pohledů ze SketchUp

- B.2 Curic toCAD - velmi přínosný plugin pro export z modelu do CADu

### C.4 Pojmenování souborů a verzí

| Položka | Konvence | Příklad |
|---|---|---|
| Hlavní model SketchUp | `[ROK][MĚSÍC][DEN]_AKCE_typ.skp` | `260827_HUT_SAZAVA_interier.skp` |
| 2D export (zdroj pro Xref) | `dwg/exports/[ROK][MĚSÍC][DEN]_AKCE_typ.dxf` (vždy se přepisuje, neverzuje se) | `dwg/260827_HUT_SAZAVA_REZ-A-A.dxf` |

Cestu k exportovanému DXF/DWG držte po celou dobu projektu neměnnou, aby se odkazy Xref nikdy nerozbily; verzujte model SketchUp a výkresový soubor BricsCAD, ne mezikrok exportu.

## Část D — Materiály a texturování

Na úrovni modelování nižší priorita, protože materiály ovlivňují až fázi renderingu — ale vyplatí se je nastavit správně hned od začátku, protože opravovat problémy s texturami dodatečně je mnohem otravnější, než jim předejít.

### D.1 Častá chyba: osa / orientace textury

Nejčastější problém je textura běžící špatným směrem nebo zkosená, téměř vždy z jedné z těchto příčin:

- Zrcadlení komponenty (Flip Along) zrcadlí i její texturu — zrcadlená zárubeň dveří nebo okna pak často ukazuje kresbu nebo vzor v opačném směru. Tam, kde jde o směrový materiál, použijte místo zrcadlení otočení o 180°, nebo po zrcadlení opravte pozici textury.
- Neproporcionální škálování komponenty texturu natáhne nebo zkosí, pokud materiál není skutečně bezešvý/dlaždicovatelný v použitém měřítku.
- Aplikace materiálu ve špatném reálném měřítku hned na začátku, které se pak už nikdy neopraví poté, co se komponenta zkopíruje po modelu.

### D.2 Doporučený postup

- Nastavte správné reálné měřítko textury jednou, na jediné "master" instanci komponenty, dřív než se tato komponenta zkopíruje po modelu.
- Pro opravu rotace/měřítka/zkosení na konkrétní ploše použijte pravý klik ▸ Texture ▸ Position s pevnými piny.
- Pravidelně model zkontrolujte pomocí šachovnicového nebo mřížkového testovacího materiálu, abyste vizuálně odhalili zkosené nebo špatně zarovnané textury, zejména po jakékoliv dávce zrcadlení nebo škálování.
- Materiály pojmenovávejte konzistentně a podle kategorie (např. `WD_Oak_Floor`, `MTL_Alu_Frame`) — toto pojmenování se přenese i do Twinmotion a výrazně zrychlí tam nastavení světel a povrchů.

## Část E — Affinity Collection pro publikace

*Zatím jen stručně jako placeholder — rozpracovat, až se v kanceláři ustálí pevnější konvence.* Prozatím obecná doporučení:

- Pro sestavení finálních listů/panelů používejte Affinity Publisher, kombinujte rendery z Twinmotion (rastr) s výkresovými exporty z BricsCAD (vektor, pokud možno vkládané jako PDF, aby zůstal linework ostrý).
- Velké render obrázky u listů, které se budou opakovaně revidovat, spíš linkujte než vkládejte (embed) — udrží to velikost souboru rozumnou a aktualizovaný render se pak jednoduše obnoví na místě.
- Udržujte sdílenou firemní šablonu (.aftemplate) pro standardní formáty listů, razítka a typografii, aby výkresy i rendery vždy zapadly do konzistentního layoutu.
- Před finálním exportem nastavte správný barevný profil pro daný výstup — CMYK pro vše určené k tisku, RGB pro obrazovku / digitální PDF.

## Příloha — Kontrolní seznamy

**Nastavení nového projektu**

- Vytvořte strukturu projektových složek (model, exports, sheets, renders).
- Založte soubor SketchUp z firemní šablony, s předpřipravenými tag folders podle A.5.
- Nejdřív nastavte skupinu 00_SITE: terén, hranice pozemku, kontextová hmota okolí.
- Před vážným modelováním ověřte jednotky, měřítko a geo-lokaci.

**Před exportem (BricsCAD 2D / Twinmotion)**

- Vybraná správná Scene pro cílový export.
- Vyčištěné nepoužité komponenty a materiály.
- Namátkově zkontrolované nedávno zrcadlené/škálované komponenty kvůli posunu textur (Část D).
- Export zapsaný na dohodnutou, pevnou cestu k souboru — ne jako nový/přejmenovaný soubor.

**Před dokončením 2D listu**

- Section Planes pojmenované a umístěné shodně s kódem výkresu.
- Ověřená nastavení Style pro hrany/řezné čáry v aktuálním souboru SketchUp.
- Export DXF/DWG přepsaný na pevné cestě a Xref znovu načtený v BricsCAD.
- Po jakékoliv významné změně modelu okem zkontrolované kóty a hranice šraf.
