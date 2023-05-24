# Softwarové inženýrství

> Životní cyklus SW, proces vývoje a řízení softwarového vývoje. Metodika (Rational) Unified Process (UP, RUP), agilní metodiky a principy agilního vývoje SW. Nasazení a provoz softwarových systémů. Údržba softwarových systémů, znovupoužitelnost. Příklady z praxe pro vše výše uvedené. [PA017](https://is.muni.cz/auth/el/fi/podzim2020/PA017/um/cz/)

## Životní cyklus sw, proces vývoje a řízení softwarového vývoje

Vždy nějakým způsobem obsahuje fáze analýza, návrh, implementace, testování a provoz (včetně nasazení). Rozdíly jsou v tom, zda a jakým způsobem dělíme projekt na uchopitelnější části. Důsledkem toho jsou i různé způsoby, jakým se vývoj řídí.

Existuje několik základních modelů

- **Vodopádový model**
    - Skládá se z:
        - **Analýza**
            - sběr požadavků klienta
            - Je důležité rozlišovat mezi tím, co říká že potřebuje, a co skutečně potřebuje. Pro lepší představu můžeme sledovat, jak koncový uživatel pracuje se současným řešením.
            - zajímá nás **co** a **proč**, často ale klient zmiňuje **jak**. V takových případech je důležité se ptát **proč**. Může jít o legitimní důvod, ale také třeba o nevědomost.
            => studie proveditelnosti, dokument požadavků...
        - **Návrh**
            - návrh architektury, jednotek, výběr technologií, plán testování
            => diagramy (uml), wireframy, prototypy
        - **Implementace**
            - tvorba systému dle návrhu
        - **Testování**
        - **Provoz**
    - i.e. nejprve sesbíráme všechny požadavky, pak sw jako celek postupně navrhneme, implementujeme, otestujeme a nasadíme
    - Výhody:
        - snadný na řízení
        - pokud vše jde hladce, je to nejlevnější způsob
    - Nevýhody:
        - většinou všechno nejde hladce
        - špatně se reaguje na změny (musíme se vracet do předchozích fází modelu)
        - zákazník předem nedokáže přesně a úplně definovat, co potřebuje
        - v praxi nejsou kroky v tomto pořadí dodržovány (testovat chceme ideálně při vývoji, něco chceme ukázat netrpělivému zákazníkovi...)

- **Inkrementální model**
    - Projekt se rozdělí na inkrementy, části, které budou vyvíjeny a dodávány postupně, pro každý si uděláme jednoduchou rámcovou analýzu
    - Inkrementy se vyvíjí v pořadí podle priority
    - Po nasazení do systému máme o inkrementu od zákazníka zpětnou vazbu
    - O nutnosti změny se dozvíme dříve a její zavedení bude levnější (není třeba vše překopávat, přidáme změnový inkrement)

- **Spirála**
![](img/20230523213434.png)
    - kombinace iterací a vodopádu, důraz na analýzu rizik
    - vývoj probíhá v cyklech, každý má několik fází
    - fáze
        - **Analýza**
        - **Návrh**
        - **Implementace**
        - **Testování, zpětná vazba a plán dalšího cyklu** - zpětnou vazbu používáme pro práci v dalším cyklu
    - oproti iteraci nemusíme mít po každé iteraci hotovou část nasazeného systému.
    - cykly aplikujeme i na jednotlivé fáze vodopádu
    - lépe pracujeme s nejistotou, ale trvá to dýl

- **Prototypování**
    - vytvoříme prototyp systému, abychom porozuměli, jakým způsobem chce zákazník systém používat a co od něj očekává
    - po analýze prototypu ho zahodíme a začneme práci na reálném systému, využijeme vhodný model

- **Model výzkumník**
    - navrhni systém a implementuj ho. Vyhovuje? Super. Nevyhovuje? Zpět k návrhu/implementaci
    - nelze pořádně řídit, neexistuje dokumentace, řešitelé jsou obtížně nahraditelní, jde o experimentování 

Nezávisle na modelu je důležité nastavit správnou komunikaci, definovat a používat jednotný jazyk. Pokud chceme cokoliv řídit, je potřeba mít informace o aktuálním stavu, dodržování plánu, očekávaných změnách, problémech...

Hlavní metodiky řízení sw projektů jsou **prediktivní metodiky (e.g. RUP)** a **agilní (e.g. SCRUM)**.

## Metodika (Rational) Unified Process (UP, RUP)
- rigidní, důraz na procesy
- vhodná, pokud máme jasné a pevné požadavky
- vyžaduje podstatné plánování předem
- iterativní a inkrementální
- řízena riziky, use-case požadavky
- argitektura je středobodem
- umožňuje pevnou kontrolu nad procesy a týmem
- vhodná, pokud potřebujeme pořádnou dokumentaci (UML diagramy)
- Výhody:
    - zákazník není při vývoji potřeba, definice produktu je zakotvena v kontraktu (přesně ví, co dostane)
- Nevýhody
    - fixní deadliny a rozpočet
    - změnové pořadavky jsou problém
    - potřeba více času k plánování
    - složitý kontrakt, je třeba myslet na všechno (exhaustive kritéria přijetí, penále...)

![](img/20230523215135.png)
- iterace jsou seskupovány do fází
    - **Inception** (1 iterace)
        - řešíme feasibilitu, zachycujeme klíčové požadavky, rizika
        - na konci známe cíle
    - **Elaboration** (2 iterace)
        - řešíme požadavky, architekturu, hrajeme si s UML diagramy
        - na konci máme architekturu, návrh systému reflektující požadavky 
    - **Construction** (4 iterace)
        - tvoříme systém, testujeme, nasazujeme
    - **Transition** (2 iterace)
        - hledáme a opravujeme chyby, děláme manuály, poskytujeme konzultace

- iterace by neměla překročit 3 měsíce, přínos iterace je **inkrement**, každá iterace obsahuje workflows, které jsou více či méně přítomné. Pro každé workflow se používají určité UML diagramy
    - **Business modelování**
        - activity diagram
    - **Požadavky**
        - use case diagram
    - **Analýza a návrh**
        - sequence, collaboration diagrams
    - **Implementace**
        - class, object, component diagrams
    - **Testování**
        - use case, class, activity diagrams
    - **Deployment**
        - deployment diagram

RUP je konkrétní metodika stavějící na UP (přidává třeba jednotlivé role a odpovědnosti v týmu, konkrétní postupy...), UP je obecný rámec

## Agilní metodiky a principy agilního vývoje SW
- flexibilní, důraz na lidi
- vhodná, pokud se požadavky mění, nebo není jasná kýžená výsledná podoba systému
    => není přesné datum dokončení
- vyžaduje minimální plánování předem

### SCRUM
- nejšastěji využívaná agilní metodika
- iterativní, inkrementální
- jednoduchý, očeává se použití i dalších nástrojů/procesů

- role
    - **product owner** - reprezentuje stakeholdery, má největší přehled o požadavcích na produkt, spravuje product backlog
    - **scrum master** - zodpovědný za dodržování scrumu, řeší procesy
    - **tým vývojářů** - 3-9 lidí, soběstačný (má lidi na všechno) a sebeorganizující se, spravují sprint backlog, zodpovědní za doručení produktu

- artefakty
    - **product backlog** 
        - obsahuje veškerou zbývající požadovanou funkcionalitu ve formě **user stories** (jednotka funkcionality z pohledu uživatele)
        - každé story má **story points** reprezentující časovou náročnost odhadnutou pomocí planning poker
            - planning poker - pro každé story každý z týmu provede odhad, odhady se zveřejní najednou. následuje diskuze, dokud se na bodech za dané story všichni neshodnou
        - tvořen celým scrum týmem, spravuje ho product owner
        - v praxi jde o tabuli (reálnou/virtuální) se sticky notes
    
    - **sprint backlog** 
        - část product backlogu (množina user stories), která se má provést v daném sprintu
        - stories jsou rozděleny na jednotlivé tasky, u každého je určen časový odhad v hodinách
        - task má fáze Todo, In progress a Done
        - tasky si k práci vybírají vývojáři dle vlastního uvážení, ale žádné (ani user stories) nemohou být v rámci sprintu přidány/odebrány
            - bylo by nutné zrušit celý sprint product ownerem
        - spravován týmem vývojářů
    
    - **product increment**
        - všechny předměty product backlogu, které se splní během sprintu (a.k.a. to, co se za sprint stihne/udělá)
        - tvořen týmem vývojářů, testován zákazníkem, může být released product ownerem
        - je nutné, aby byl použitelný a byl splněn (dle definice scrum týmu)

- události
    - **sprint planning**
        - probíhá na začátku sprintu, cca 8 hodin
        - účastní se celý scrum tým
        - vytyčuje se cíl nadcházejícího se sprintu (a.k.a. co chceme udělat), vybíráme věci z product backlogu a přiřazujeme jim tasky
    - **sprint**
        - iterace soustředěná na vývoj funkcionality v spring backlogu, cílem je vytvořit použitelný a potenciálně vydatelný product increment
        - pracuje na něm celý scrum team
        - product owner řešní komunikaci, vývojáři vývojaří, scrum master sleduje dodržování procesů
        - analýza, návrh, implementace, testování
        - max 1 měsíc, všechny sprinty trvají stejnou dobu 
    - **daily scrum**
        - 15 minut každý den, účastní se vývojáři a možná i scrum master
        - co jsem dělal včera, co budu dělat dneska, narazil jsem na nějaké problémy?
    - **sprint review**
        - 4 hodiny, účastní se celý scrum team a klíčoví stakeholdeři (e.g. zákazník)
        - proběhne předvedení inkrementu
        - proberou se případné změny product backlogu
        - případně se přepočítá předpokládané datum dokončení
    - **retrospective**
        - 3 hodiny, účastní se scrum team
        - řeší se procesy, vztahy, nástroje, lidi
        - co nefungovalo, co můžeme zlepšit
        - ideálně se vymyslí jedno zlepšení procesů, které se v příštím sprintu bude používat









## Nasazení a provoz softwarových systémů

## Údržba softwarových systémů, znovupoužitelnost




<!-- 

Po dokončení návrhu přichází na řadu samotná implementace softwarového produktu. Programátoři používají různé programovací jazyky a technologie k vytvoření funkčního softwarového kódu. Během této fáze se také provádí testování, aby se ověřilo, zda software pracuje správně a splňuje stanovené požadavky.

Dalším důležitým krokem je nasazení softwarového řešení, které zahrnuje instalaci a konfiguraci na cílovém prostředí. To může zahrnovat serverovou infrastrukturu, databáze nebo další technické prvky, které jsou nezbytné pro provoz softwarového produktu. Správné nasazení je klíčové pro zajištění správného a stabilního chodu softwaru.

Po nasazení je důležité provádět údržbu a podporu softwarového řešení. To zahrnuje opravu chyb, aktualizace, optimalizaci výkonu a další úpravy, které jsou potřebné v průběhu životnosti softwaru. Údržba je nezbytná pro zajištění správného fungování a spokojenosti uživatelů.

Celý proces vývoje softwarových řešení může být realizov

...ován pomocí různých metodologií a přístupů, jako je například vodopádový model, iterativní vývoj, nebo agilní metodiky, jako je Scrum nebo Kanban. Každá z těchto metodologií má své výhody a vhodnost pro různé typy projektů a týmů.

V průběhu vývoje softwarového řešení je také důležité zajistit správnou komunikaci a spolupráci mezi členy týmu. Projekt může zahrnovat programátory, designéry, analytiky a další odborníky, kteří musí spolupracovat a koordinovat svou práci. Komunikační nástroje, jako jsou pravidelné schůzky, nástěnky, nebo nástroje pro správu úkolů a verzování kódu, mohou pomoci udržet tým synchronizovaný a efektivní.

V dnešní době je vývoj softwarových řešení často prováděn v prostředí, které podporuje automatizaci a kontinuální integraci a nasazení (CI/CD). To umožňuje rychlé a opakované testování, sestavování a nasazování softwaru, což vede k vyšší kvalitě a rychlosti dodávání.

V neposlední řadě je důležité také sledovat trendy a novinky v oblasti vývoje softwarových řešení. Technologie se neustále vyvíjejí a nové přístupy a nástroje se objevují. Důkladné sledování a přizpůsobování se těmto trendům může pomoci vytvářet moderní a konkurenceschopné softwarové produkty.

Celkově lze říci, že vývoj softwarových řešení je komplexní proces, který vyžaduje dobrou analýzu, návrh, implementaci, nasazení a údržbu. Efektivní spolupráce týmu, využívání moderních metodologií a technologií a sledování trendů jsou klíčové pro úspěšné vytváření softwarových produktů, které splňují požadavky a potřeby uživatelů.

-->