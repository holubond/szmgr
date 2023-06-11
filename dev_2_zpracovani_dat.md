# Zpracování dat.

> Základní pojmy a principy datových skladů, datové analytiky a business intelligence. Životní cyklus datového skladu. Analytika velkých dat, jazyky pro realizaci analytický úloh, analytika na úrovni databází. Pokročilé techniky zpracování dat, výkonnostní aspekty zpracování velkých dat. Příklady z praxe pro vše výše uvedené. ([PA220](https://is.muni.cz/auth/el/fi/podzim2021/PA220/um/), PA036)

## Základní pojmy a principy datových skladů, datové analytiky a business intelligence.

**Business intelligence** 
- procesy a nástroje pro sběr, analýzu a prezentaci/vizualizaci dat za účelem asistence při tvorbě informovaných rozhodnutí v podnikovém řízení
- umožňuje transformaci dat do informací
- jádrem je datový sklad

**OLTP vs OLAP**
- OLTP (online transaction processing) 
    - způsob ukládání dat v db pro transakční zpracování
    - data v databázi se mění, cílem je zajistit konzistenci a umožnit CRUD
    - nutné zamykání tabulek/řádků pro zajištění konzistence
    - vhodné pro uložení dat v operativním provozu podniku (zajímá nás, co máme na skladě, jaká je aktuální cena produktů...)
    - normalizovaná forma (není datová redundance, používají se public keys pro případné spojování dat)
    - používané queries známe dopředu

- OLAP (online analytical processing)
    - způsob ukládání dat pro analytické zpracování
    - data v databázi se nemění
    - zamykání není třeba, data se nemodifikují 
    - pracujeme s mnohem větším objemem dat
    - vhodné pro dlouhodobé uložení dat, reflektuje historii dat z produkční databáze a jejich vývoj v čase
    - denormalizovaná forma, hodně indexů, snažíme se minimalizovat nutné joiny, nevadí datová redundance
    -  **hvězdicové schéma/dimenzionální modelování** - středobodem je vždy nějaký subjekt (obsahující fakta e.g. prodej) s **tabulku faktů** (obsahující konkrétní záznamy měření), ke které se pomocí referencí (foreign key) vážou **tabulky dimenzí** (často odpovědi na otázky KDO, KDY, KDE, JAK..., obvykle detailní (denormalizované) pro snadnou analytiku, redundance nevadí, e.g. datum dělíme na den, měsíc, rok, den v týdnu, kvartál... můžeme mít separé date i time dimenze, obvykle 4-15). Čím více dimenzí máme, tím více/konkrétněji se můžeme DW dotazovat (dimenze tvoří kontext). Tabulky dimenzí obvykle předvyplníme (pro datum můžeme použít numerický přepis data, e.g. 20230621), rozlišujeme dimenze data a času. Jako stěžejní data (to, co nás zajímá) bereme fakta, dimenze jsou popisná data k faktům, podle kterých je možné seskupovat. Reference jsou jen v tabulce faktů. ID pro tabulky (surrogate keys, int) dimenzí si generujeme sami, abychom nebyli limitování použitými klíči z OLTP. 
    ![](img/20230611150321.png)
    - typy faktů
        - transakční, událost spojená s hodnotou (e.g. nákup)
        - snapshot - zachycující nějakou aktuální hodnotu (e.g. naplněnost skladu)
        - bez hodnoty - fakt nemá žádnou numerickou hodnotu, obvykle jde o nějakou událost (e.g. click na určitý prvek)
        Za fakty se považují i odvozená data (e.g. kumulativní hodnoty za nějaké období), nebo data kombinovaná z vícero procesů (e.g. prodeje a jejich předpovědi pro dané období). Ty obvykle neukládáme do tabulky faktů (ale může to mít své opodstatnění, třeba pro zrychlení dotazů) 
    - nevadí nám drobná neaktuálnost dat
    - query neznáme dopředu, záleží na tom, co chceme zjistit
![](img/20230610171630.png)

**Snowflake schema** - star schema, kde dimenze mají hloubku (obsahují reference na další tabulky, e.g. obsahující month ID a month name). Způsobují performance problémy, jde o antipattern.

![](img/20230611150909.png)

**Granularita** popisuje, z jaké úrovně se na fakta díváme (e.g. zajímá nás, kolik se prodalo daného produktu? Za jeden den? V konkrétním obchodě?). Nejnižší granularita je jeden fakt (ukládáme opravdu fakty, nebo třeba jen agregovaná data?).

**Měření/Measure** - aspekt faktu, který nás zajímá, lze agregovat (e.g. cena prodeje). Některé hodnoty měření lze sčítat (e.g. tržby), některé jen v některých dimenzích (e.g. zůstatek na pokladnách, nelze sčítat v čase), některé vůbec (e.g. cena za jednotku produktu nebo průměrná cena za období...)

**Conformed (přizpůsobivá?) dimension** - dimenze, která má stejné hodnoty a význam pro data pocházející z vícero zdrojů. E.g. čas je obvykle conformed dimenze, pobočka nemusí být (pod jakou pobočku by spadal prodej přes internet?).

**Datový sklad**
- OLAP
- oddělení od OLTP, abychom nezatěžovali produkční db
- obvykle 1 db fungující jako centrální zdroj pravdy pro analýzu a reporting
- data ve skladu se nemění, pouze přidávají, je vidět vývoj dat v čase
- může obsahovat data z vícero zdrojů
- vyžaduje, aby byla zdrojová data očištěna a konzistentně uložena
- lze využít standardní databázi (e.g. postgres), nebo specialozovaná řešení (e.g. Google BigQuery, Teradata)
- jednoduchá reprezentace dat, aby s nimi mohli pracovat analytici a bylo umožňěno používat jednoduché analytické dotazy (s minimem joinů)
- cílem je umožnit a zjednodušit analýzu dat

**Data mart**
- malý data warehouse, soustředí se na jednu zájmovou jednotku (e.g. objednávky)
- cílem je dekompozice za účelem zvýšení efektivity/omezení přístupu do jednotlivých částí datového skladu
- mohou být dvě podoby
    - Nezávislé data marty - nemáme žádný centrální zdroj pravdy (DW), data do data martů jdou přímo ze zdrojů
    - Logické data marty - data marty fungují jako logické pohledy na část datového skladu, jednodušší na údržbu

**Data Cube** 
- obsah DW, umožňuje pohled na data z různých dimenzí (rozměrů kostky, obvykle 4-15)
- skládá se z buněk (cells) - každá je kombinací hodnot dimenzí. No data = prázdná buňka. 
- **Dense/sparse cube** - hodně/málo neprázdných buněk v data cube

## Životní cyklus datového skladu.

Životní cyklus
- **určení cíle a plánování** - co od systému očekáváme, jaký rozsah dat nás zajímá, odhad ceny, rizik, prioritizace subjektů (=> datamartů)
- **návrh infrastruktury** - volba vhodných nástrojů a technologií, architektonických řešení
- **návrh a vývoj data martů** - iterativně tvoříme data marty, každý zapojujeme do DW systému
    - **volba procesu** - včetně modelování procesu (třeba UML diagramem, nebo BPMN), e.g. prodeje
    - **určení granularity** - e.g. prodej jednoho produktu (jedné položky z objednávky) jednomu zákazníkovi, na jedné pobočce v jeden moment
    - **identifikace dimenzí** - vychází z granularity, můžeme dimenze rozšířit o další jevy (e.g. den v týdnu, slevové akce...)
    - **identifikace faktů** - všech sloupců, které budou v tabulce faktů (e.g. cena jednotky produktu, prodané množství)
    - čištění dat a jejich přidání do DW systému

![](img/20230610173720.png)

**Změny dimenzí**
- dimenze se mohou v průběhu života DW měnit (změní se třeba region, pod který spadá pobočka)
- možnosti implementace změny
    - **Přepis** - nahradíme stará data novými, je to jednoduché, ale ztrácíme informaci o historii
    - **Přidání sloupce s předchozí verzí (a valid from)** - vyřeší problém, ale při další změně čelíme stejnému problému
    - **Verzování** - tabulce dimenzí přidáme sloupce `valid from` a `valid to`, při změně pouze upravíme `valid to` a přidáme řádek pro novou hodnotu dimenze
    - **Přidání dimenze** - výběr aktuální verze musíme řešit jen v případě, že nás daná hodnota zajímá (e.g. mění se přiřazení obchodu do regionu, ale ne v každém dotazu nás zajímá region)
    Pokud často pracujeme s aktuální hodnotou, můžeme použít verzování, ale držet i aktuální hodnotu v separé sloupci.

Přístupy tvorby datových skladů
- top-down - analogie vodopádu, nejdříve analyzujeme datové zdroje, pak navrhneme a implementujeme sklad, nakonec naplníme daty a vytvoříme data marty
- bottom-up - iterativně inkrementální přístu, postupně pro každý zájmový objekt analyzujeme zdroje, postavíme data mart a případně rozšíříme (pokud nějaký centrální používáme) datový sklad

## Analytika velkých dat, jazyky pro realizaci analytický úloh, analytika na úrovni databází.

Pro (efektivní) analytiku velkých dat je vhodné vytvořit data warehouse, ve kterém jsou data organizována do hvězdicového schématu. Jazykem pro realizaci analytických úloh bývá většinou SQL (zvlášť pro ROLAP), které podporuje i analytické dotazy. Pokud používáme specializované analytické řešení, může se jazyk samozřejmě lišit, obvykle však bývá na SQL aspoň z části založen.

**Druhy dotazů specifické pro analytiku**
- **Slice** - v rámci jedné dimenze vybíráme konkrétní hodnotu a zobrazujeme pouze data s touto hodnotou dimenze. V sql pomocí WHERE. E.g. kolik se prodalo laptopů?
- **Dice** - jako slice, akorát pracujeme s intervaly/vícero hodnotami jedné dimenze (e.g. prodeje od-do, prodeje laptopů a telefonů), nebo hodnot vícero dimenzí (prodeje laptopů v říjnu) 
- **Roll-up** - provádíme agregaci dat. Dimenzionální - můžeme vynechat nějakou dimenzi (kolik jsme prodali za celý čas? kolik ve všech pobočkách?) nebo hierarchický - můžeme se dívat z pohledu vyšší úrovně nějaké dimenze (kolik jsme prodali v jednotlivých regionech, které se skládají z vícero poboček?). Oba přístupy lze kombinovat. V sql pomocí agregačních funkcí (GROUP BY a třeba SUM)
- **Drill-down** - opak roll-upu, jdeme z abstrakce do většího detailu. Je nutné, aby nějaká detailnější data existovala. Obvykle děláme drill-down z nějakého materializovaného pohledu a jdeme na konkrétní data.

**Pivoting** 
- přeskládání a agregace dat za účelem vizualizace
- nejjednodušší variantou je **kontingenční tabulka** (cross table), ve které se zaměřujeme na dvě dimenze: 
![](img/20230611214059.png)
- v SQL se dříve muselo provádět pomocí sjednocení (union) několika příkazů
![](img/20230611214616.png)
- nyní je v SQL možné použít (uvádím i příklady, je možné uvést více sloupců pro vícedimenzionální kontingenční tabulky) 
    - `GROUP BY ROLLUP(year, band)` - vrací *polovinu* kontingenční tabulky (vrátí data, agregaci pro každý rok a celkovou agregaci) 
        ![](img/20230611215146.png)
    - `GROUP BY CUBE(year, band)` - vrací celou kontingenční tabulku (vrátí data, agregaci pro každý rok, agregaci pro každou skupinu a celkovou agregaci) 
        ![](img/20230611215253.png)
    - `GROUP BY GROUPING SETS(...)` - umožňuje větší kontrolu nad agregací dat (lze mimo jiné realizovat příkazy ROLLUP, CUBE)
        ![](img/20230611215834.png)

**Přístupy k implementaci OLAP**
- **Relational OLAP (ROLAP)** 
    - data ukládáme v relační databázi (e.g. postgres), dimenze simulujeme pomocí star schema, pro dotazování používáme standardní SQL
    - (+) není potřeba specializovaný systém
    - (+) dobrá flexibilita
    - (-) response time
    - (-) zabírá 3-4x více místa, než MOLAP (v případě dense cubes)
- **Multidimensional OLAP (MOLAP)**
    - data ukládáme ve speciálních multidimenzionálních strukturách (e.g. in-memory db, nebo multidimenzionální pole/matice kde používáme přímou adresaci na disku)
    - rychlejší queries než ROLAP, zabírá míň místa (není potřeba ukládat foreign keys)
    - horší flexibilita (při přidání hodnoty do domény dimenze je nutné přidat velké množství buněk, u ROLAP jde o jeden řádek v tabulce dimenze)
    - je potřeba specializovaný systém
    - mnohdy bývá součástí/add-on databázového řešení (MS SQL Server, Oracle...)
- **Hybrid OLAP (HOLAP)**
    - kombinace MOLAP a ROLAP
    - čistá data uložena v ROLAP
    - agregace uloženy v MOLAP
    - => flexibilita, rychlost, ale vyšší složitost systému

## Pokročilé techniky zpracování dat, výkonnostní aspekty zpracování velkých dat.

Pro zajištění rychlosti dotazů v OLAP se používá redundance v podobě
- materializovaných pohledů (vkládáme jednou za čas, takže to není problém)
- indexů
- denormalizovaného schéma

**Indexy** - umožňují rychlejší získání dat, která nas zajímají, pomocí předpočítaných výsledků. Omezují prostor nutný k prohledání při čtení dat.
- obvykle se používají [B+ stromy](./5_databaze.md#indexování), ty jsou však limitovány jen pro 1D data
- **UB stromy** - multidimenzionální data jsou linearizovány pomocí Z-křivky a následně indexovány pomocí B* stromu (jako B+, akorát tam jsou jiná pravidla pro rebalanc). Linearizace Z-křivkou poskytuje dobrý výkon pro intervalové dotazy a zajišťuje, že data, která si byla blízká původně si budou blízká i po linearizaci. Indexovat do linearizovaných dat lze pomocí konverze souřadnic na binární číslo a naslédném prokládání bitů souřadnic.

    |![](img/20230611224121.png)|![](img/20230611224138.png)|
    |---|---|
    |![](img/20230611224805.png)|![](img/20230611224906.png)|
- **R stromy**

**Sloupcové databáze** - na rozdíl od řádkových databází (e.g. Postgres), kde jsou uloženy vedle sebe data náležící jednomu řádku ukládají sloupcové databáze (e.g. BigQuery, S4HANA) vedle sebe data z jednoho sloupce. Díky mohou být sloupcové databáze rychlejší pro čtení dat.

**In-memory databáze** - namísto uložení dat na pevném disku držíme data v RAM -> rychlejší přístup, ale mnohem vyšší cena.

**Distribuované databáze** - umožňují horizontální škálování TODO

### Apache Hadoop

- platforma pro paralelní/distribuované zpracování velkých datasetů
- vysoká dostupnost zajištěna replikací dat
- využívá modelu **MapReduce**
    - umožňuje paralelní zpracování dat
    - **Map** - transformace dat, jedná se o operace, které je možné triviálně paralelizovat (stejná operace na všech datech), vrací key-value data
    - **Reduce** - sumarizace dat, hodnoty z vícero řádků agregujeme do jedné hodnoty, transformuje vícero key-value na jednu/menší množství value(s)
- využívá **Hadoop File System (HDFS)** - distribuovaný souborový systém vhodný pro immutable data

### Apache Hive

- data warehouse postaven nad Hadoop (a HDFS)
- poskytuje SQL-like (HiveQL) rozhraní pro dotazy, které je převedeno do MapReduce dotazů

TODO hadoop, mapReduce
