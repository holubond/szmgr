# Projektové řízení. 
> Plánování, řízení rizik, role modelů v projektovém řízení. Ganttovy diagramy, síťová analýza, metoda kritické cesty (CPM), Program Evaluation and Review Technique (PERT). Mezinárodní standardy a metodiky projektového řízení (PMI Project Management Body of Knowledge, PRINCE 2). Příklady z praxe pro vše výše uvedené. [PA179](https://is.muni.cz/auth/el/fi/jaro2022/PA179/um/)

## Plánování, řízení rizik

Je třeba rozlišovat mezi
- **Projekty**
    - dočasné - mají start i konec, typické fáze (příprava, provedení, uzavření, případně fáze sw životního cyklu)
    - přinášející změnu, dodávají hodnotu stakeholderům (všem zúčastněným; uživatelům, zákazníkům i dodavatelům produktu...), hodnota popsána v business case
    - unikátní - každý má svá specifika (požadavky, zákazníky, tým...), nejedná se o každodenní rutinnou práci firmy
    - plánované
    - opakovatelné prvky projektu (či celé projekty) jsou **procesy** - řízené událostmi, bývají dobře definované (jak postupovat), vizualizované flow chartem
        > proces je opakovatelná série aktivit s definovanými vstupy, výstupy, nástroji, technikami...
    - bývají spojené s rizikem - spojené s unikátností (nikdy jsme to nedělali), deadliny, děláme nějakou změnu
    - **řízení**
        - balanc mezi časem, cenou a rozsahem/kvalitou
        - začneme užíváním standardů (PRINCE2, PMBOK, IPMA), abychom efektivněji komunikovali, koordinovali, zvýšili důvěru stakeholderů, transparentnost, abychom znovu neobjevovali vymyšlené
- **Programy** - skupina dočasných, vzájemně provázaných projektů řízená jako skupina abychom dosáhli cílů, které projekty sdílí
    - **řízení**
        - správa rizik
        - odstraňování omezení a konfliktů z projektů
        - v projektech se přemýšlí a plánuje i s ohledem na jiné projekty programu
- **Portfolio** - skupina projektů a programů, řízená k dosažení dlouhodobých strategických cílů
    - a.k.a. co dlouhodobě nabízíme
    - **řízení** 
        - monitoringem výkonu firmy
        - výběrem a prioritizací programů a projektů

Specifika IT projektů v porovnání s většinou průmyslových odvětví
- nepřesné/neznámé, časté a měnící se požadavky
- větší nutnost přizpůsobení produktu
- velká složitost
- náročné testování
- neustálý a rapidní vývoj technologií
- možnost globální spolupráce
- projekty mohou v rámci portfolia ovlivnit ostatní projekty (zvlášť při selhání)
- nutnost řízení rizik
- dokončené projekty je často třeba servisovat/poskytovat podporu 

Pro konkrétní projekt je potřeba si zvolit vhodný přístup **prediktivní** nebo **agilní** [viz otázka 3](./3_softwarove_inzenyrstvi.md).

### IT Infrastructure Library (ITIL)
- best practices pro **řízení IT služeb**
- fáze
    - **Service strategy** - požadavky, strategie pro zajištění kýženého, finance, co vlastně budeme dělat
    - **Service design** - Service Level Agreement, řešení rizik, security & business compliance
    - **Service transition** - jak měníme stávající služby, řešení deploymentu, uložení získaných znalostí pro budoucí projekty 
    - **Service operation** - dokumentace pro uživatele/helpdesk, řešení incidentů/změnových požadavků/problémů, řešení identit a přístupu k systému
    - **Continual service improvement** - monitoring, logování, aktualizace běžící služby







## Role modelů v projektovém řízení

## Ganttovy diagramy

## Síťová analýza

## Metoda kritické cesty (CPM)

## Program Evaluation and Review Technique (PERT)

## Mezinárodní standardy a metodiky projektového řízení 
- standardy projektového řízení PRINCE2, PMBOK, IPMA ICB popisují obecnější způsob řízení
- metodiky sw vývoje (RUP, SCRUM) řeší řízení v rámci vývojového týmu, jsou specifické pro vývoj SW

![](img/20230525184623.png)

### PMI Project Management Body of Knowledge (PMBOK)
- **procesně orientovaný** standard, podrobně popsaná sada good practices
- snadno se používá jako handbook pro vhodné znalostní oblasti a nástroje/techniky při životním cyklu projektu
- vhodný, když
    - manažer potřebuje tipy na nástroje a techniky, jaké by měl použít, ale aspoň trochu tuší co a jak

- 49 procesů (série aktivit s definovanými vstupy, výstupy, nástroji a technikami) dělených do
    - 5 procesních skupin, logické dělení procesů podle fází (inicializace, plánování, provedení, monitoring a řízení, uzavírání)
    - 10 vědomostních oblastí/disciplín projektového managementu, každá má vlastní procesy
        - Integrace - tvorba Project Charteru (základní info o projektu), vývoj plánu řízení projektu, nastavení řízení změn (review, schválení, dodání, komunikace), obsahuje i procesy pro uzavírání fáze/projektu
        - Rozsah (scope) - sesbírání požadavků, definice, validace a řízení rozsahu funkcionalit systému, tvorba Work Breakdown Structure
        - Plán - definice a určení pořadí aktivit, odhady časů aktivit, tvorba a řízení plánu
        - Cena - odhad cen a rozpočtu aktivit nebo jednotek práce pomocí Work Breakdown Structure, řízení ceny a rozpočtu
        - Kvalita - plánování, řízení a kontrola kvality
        - Zdroje - odhad nepeněžních a lidských zdrojů, jejich získávání a řízení, tvorba a správa týmů
        - Komunikace - plán, správa a kontrola komunikace a informací o projektu
        - Riziko - identifikace, kvalitativní (míra doparu) a kvantitativní (pravděpodobnost) analýza rizik, jejich monitoring, plán a procesy reagující na rizika
        - Dodavatelé - produkty a služby pocházející mimo náš tým, kontrakty, objednávky, SLAčka, výběr dodavatelů, monitoring výkonu dodavatelů
        - Stakeholdeři - zúčastněné osoby; jejich identifikace, plánování a správa zapojení stakeholderů do projektu

### PRINCE 2 (PRojects IN Controlled Environment)
- standard pro řízení obecného projektu
- předepsaný postup, krok za krokem (spousta formulářů na vyplňování, checklisty)
- součástí není správa požadavků, rozpočtování
- vhodný pro
    - nutnost velkého reportování
    - nutnost kompletní projektové dokumentace
    - tým vyžaduje řád a kontrolu
    - manažery s málo zkušenostmi, hodí se mu podrobný popis postupu

- 7 principů (vše máme nějak zdokumentované)
    - **Kontinuální odůvodnění projektu** - proč to děláme? dokumentujeme v Business Case 
    - **Učení se ze zkušeností** -  co (ne)fungovalo
    - **Role a odpovědnosti** - přesně specifikovaná struktura týmu, vymezené práva a odpovědnosti
    - **Řízení po fázích** - po každé fázi děláme review Business Case, provádíme reporting vyššímu managementu
    - **Manage by exception** - řízení soustřeďujeme na části, které se nějak (negativně) vymykají. Nezasahujeme do toho, co funguje. Vytyčíme cíle a tolerovatelné odchylky v kvalitě, času, ceně a rozsahu, určíme zodpovědnosti za nepřekračování
    - **Důraz na produkt** - primární cíl je produkt, ne práce
    - **Přizpůsobení metodiky projektu** - není nutné PRINCE používat úplně doslovně, řádek po řádku. Ne všechny formuláře jsou vždy zcela relevantní

- 7 témat
    - **Business case** - obsahuje očekávané přínosy, rizika, časový a cenový rozsah, důvody projektu... Měl by být neustále aktualizován a držen validní po celou dobu projektu
    - **Organizace** - definice rolí a odpovědností, typy stakeholderů (dodavatel, business/zákazník, uživatel), 3 úrovně managementu (project board pro směrování projektu, project manager pro řízení projektu, team manager pro dodávání produktu), manage by exception
    - **Kvalita** - monitoring, akceptační kritéria, určíme si strategii řízení kvality (nástroje, procesy), řešíme kvalitu produktu i manažerských výtvorů (plány, reporty)
    - **Plány** - plánujeme projekt i jednotlivé fáze, Gantt diagram, Work Breakdown Structure je základem plánování
    - **Rizika** - identifikace možných rizik, určujeme způsob reakce na dané riziko na základě ceny prevence, pravděpodobnosti a dopadu, uchováváme registr rizik
    - **Změny** - u požadavků na změnu řešíme prioritu, dopad, kritičnost, zkoumáme možnosti řešení, dle změny upravujeme záznamy a plán
    - **Postup projektu** - porovnáváme realitu s plány (čas, cena, kvalita, rozsah, rizika...), sledujeme zda stále projekt splňuje business case

- 7 procesů
    ![](img/20230525115631.png)
    - **Úplný začátek projektu** - nastínění business case, přiřazení klíčových vedoucích osob, studování "lessons learned" předchozích podobných projektů, získání autorizace product boardu
    - **Inicializace projektu** - příprava strategií řízení (rizik, kvality, komunikace, konfigurce), projektového plánu, konkretizace business case, založení dokumentace
    - **Řízení fáze** - řeší produktový manažer, monitoring, reportování významných událostí, řídíme exceptions, revidujeme a schvalujeme práci/nové časti produktu
    - **Řízení dodání produktu** - to samé co řízení fáze, ale řeší to týmový manažer
    - **Směrování projektu** - vysokoúrovňová rozhodnutí, funguje po celou dobu projektu, plán nadcházející fáze, na konci projektu autorizujeme uzavření
    - **Řízení mezi fázemi (managing a stage boundary)** - plán nadcházející fáze, řeší produktový manažer, aktualizace business case a projektového plánu, report předchozí fáze
    - **Uzavření projektu** - řeší projektový manažer, evaluace, předání produktu, návrh board na ukončení

### IPMA ICB
*V otázce není, ale není na škodu znát*
- obecný standard pro vedení projektu
- narozdíl od většiny ostatních obsahuje podrobnou sekci o soft skills
- vhodný, když
    - projekt vyžaduje dobré soft-skills (komunikace, leadership, řešení konfliktů)
    - manažer je zkušený, zná procesy
    - není nutná spousta reportingu
- vhodné pro použití jako handbook pro různé manažerské kompetence
- kompetenční přístup, pro každou ICB popisuje požadované dovednosti a schopnosti, popis a metriky indikátorů kompetence
    > kompetence je aplikace znalostí (knowledge, informace & zkušenosti), dovedností (skill, schopnost aplikovat znalosti) a schopností (ability, použití dovedností efektivně, ve správný čas a na správném místě) k dosažení kýženého výsledku
    - kompetence perspektivy - metody a techniky pro interakci jedinců s prostředím
    - lidské kompetence - techniky pro jednání s jedinci/skupinami
    - praktické kompetence - metody a techniky pro úspěch projektu

### Metodiky
- popsány v [otázce 3](./3_softwarove_inzenyrstvi.md)