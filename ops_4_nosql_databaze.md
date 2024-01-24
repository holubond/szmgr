# NoSQL databáze
> Principy a taxonomie NoSQL databází. Distribuované databáze, zajištění konzistence. Architektury a základní charakteristika hlavních zástupců NoSQL databází (key-value uložiště, dokumentové databáze, column-family uložiště, grafové databáze). Příklady z praxe pro vše výše uvedené. (PA195)

## Principy a taxonomie NoSQL databází
- jiný přístup ukládání dat, který se liší od relačních DB
- hlavně pro big data
- SQL databáze jsou dost často bottleneck v aplikacích -> zajišťují dobrou konzistenci a ACID vlastnosti, ale nehodí se pro dynamická data a mají omezený throughput
### Vlastnosti NoSQL
- nepoužívají relační model
- jsou navrhnuty pro distribuování v clusteru -> víc nodes a horizontální škálování -> u SQL omezené jen na čtení
- nemají pevnou schému
- největší výzvy jsou v konzistenci dat, pravidly jejich replikace a zatím nedostatečnými standardy -> NoSQL databázové systémy se od sebe o dost víc liší než SQL
- slabé garance konzistence, je to hlavně na aplikaci
- dost velká redundance dat, není tam běžné spojování pomocí joinů atd.

### Základní typy
- Key-Value - záznamy jsou uloženy jednoduše jako klíč a hodnota. Příkladem můžu být Redis, DynamoDB. Jedna taková hodnota v DB může být: Klíč: "uživatel123" , Hodnota: {"barvaPozadí": "modrá", "jazyk": "český"}. Nejjednodušší typ databází, dokáže ale ukládat největší objemy dat. Ve světě SQL tabulka o dvou řádcích - klíč a hodnota.
- Column based - tyto databáze mají data uložená po sloupcích. Oproti SQL databázím však řádek nemusí obsahovat vždy všechny sloupce. Některé databáze mají optimalizovaný přístup na celé sloupce, což je účinné, když máme podobný typ dotazů. Apache Cassandra však indexuje pomocí primárních id řádků. Ve světě SQL tabulka, kde každý řádek může mít rozdílný počet a typ sloupců.
- Document based - jednotlivé záznamy jsou většinou ukládané jako JSON/XML objekty. Objekty by měly být podobné, ale můžou se lišit. Většinou aspoň obsahují nějaký společní primární klíč na indexaci. Příklad je MongoDB
- Graph based - Umožňují reprezentaci a dotazování komplexních vztahů mezi daty. Neo4j a Amazon Neptune jsou příklady grafových databází. Dokážou dost dobře implementovat vztahy mezi entitami. Jan má rád Aničku -> Jan a Anička jsou uzly, hrana je potom "má rád"- Jsou nejkomplexnější z celých DB a od ostatních typů se také o dost více liší

### Agregáty
- je to objekt/datová jednotka souvisejících informací
- v SQL agregáty nejsou, většina dat je implementována pomocí tabulek a následně spojována joiny, či je jinak s němi manipulováno -> při správném návrhu nevznikají redundance
- u agregátů nám nevadí duplikace, chceme hlavně, aby data byla na jednom místě -> duplikace dat, pokud chceme změnit nějaké data, musíme procházet všemi objekty -> není ideální jako nějaká source of truth
- dobrý příklad je Objednávka, která by měla uvnitř objekt položky -> v SQL bychom měli položky v jedné tabulce, objednávku v druhé a potom nějakou tabulku PoložkaObjednávky -> v NoSQL prostě v každé Objednávce vytvoříme list položek a zduplikujeme se všemi údaji (jméno, cena, atd.) -> kdybychom chtěli měnit cenu položky pro všechny objednávky, tak docela složité

## Distribuované databáze, zajištění konzistence
- NoSQL databáze vytvořeny hlavně pro horizontální škálovatelnost -> levnější než vertikální a také nemá takové limity
- u vertikálního škálování a rozdělení dat je těžké zajistit konzistenci
- musí být nastavená nějaká pravidla, která zajistí konzistenci, efektivně rozdělí data a příchozí operace atd.
- existují i NoSQL databáze, které nevyužívají víc uzlů, jedná se hlavně o grafové, nebo ty, které nepotřebujeme škálovat
- **Sharding** - data rozdělujeme podle nějakých pravidel, můžeme mít například primární klíč, podle kterého data rozdělujeme na uzly a rozkládáme zátěž
- **Replikace** - jednotlivá data jsou uložena na více uzlech, může to být master-slave, nebo rovnocenný peer-peer
- většinou se používá kombinace obojího, u shardingu je třeba dobře zvolit pravidla, podle kterých jsou data rozložena -> optimalizace našich dotazů na DB
### MVCC
- MVCC je metoda pro řízení současného přístupu k datům v databázích, která umožňuje vytvářet různé verze dat pro různé transakce. Pokud jedna transakce zapisuje a jiná čte, MVCC umožňuje druhé transakci vidět snímku dat v okamžiku před zápisem, čímž se vyhne blokování.
### 2PC
2PC je distribuovaný algoritmus používaný pro koordinaci potvrzení nebo zrušení (rollback) transakcí ve víceuzlových systémech. Je to forma konsenzuálního protokolu, který zajistí, že všechny uzly (účastníci) distribuované transakce se shodnou na jejím potvrzení nebo zrušení.
#### Master-slave replikace
Master je source of truth, zápisy se dělají do něj a dále se potom propagují do dalších uzlů. Čtení je i ze slavů. Pokud master spadne, tak ho může nahradit nějaký slave. Jednodušší architektura, protože je jasně dáno, který uzel je master. Jednotlivé uzly můžou část dat držet jako master, pro další část dat jsou zase slaves -> rozložení zátěže při operaci psaní.
#### Peer to peer
Všechny uzly jsou rovnoceny. Rozložení je efektivnější, ale je těžší zajistit konzistenci. Uživatel totiž může zapisovat do dvou různých uzlů. Existují různé techniky, jak dosáhnout nějakého stavu konzistence (quorum, timestamps atd.). Hlavně u column based databází. Je nějaký replikační level - kolik uzlů má data.
### Transakce
- v NoSQL většinou nejsou implementované transakce, ale je to taky možnost, buď jsou provedeny všechny operace, nebo žádná
- jsou různé úrovně izolace:

1) **Read Uncommitted** - můžu číst aktualizované informace od ostatních transakcí, velká nekonzistence dat a může načítat hodnoty, které další transakce můžou vrátit zpátky
2)  **Read committed** - čte jen ostatní transakce, které jsou potvrzené, ale pro jednotlivé záznamy může během transakce načítat jiné hodnoty
3)  **Repeatable reads** - pokud se dvakrát zeptám na záznam během transakce, dostanu vždy stejnou hodnotu, akorát může načítat nové položky z ostatních transakcí
4)  **Serializable** - plně izolovaná
### Konzistence
- **Write konzistence** - při zápisu se může stát, že dva uživatelé aktualizují stejná data -> musíme buď jednoho odmítnout/udělat zámek, nebo případně přepsat data nejnovější verzí, zaznamenávat si časová razítka atd.
- **Read konzistence** - pokud jeden uživatel zapisuje, tak druhý mu "může číst pod rukami", pokud máme centrální DB, tak můžeme udělat ACID transakce, u distribuovaných databází je to složitější -> existují různé typy protokolů, které tyto věci zajišťují s odlišnou účinností
- **Replication konzistence** - jestli jsou data ve všech uzlech stejná, u master-slave to není takový problém, protože se zapisuje do masteru -> celkově se řeší tak, že se data postupně zpropagují, jak říká definice BASE

### CAP teorém
Jsou hlavní 3 vlastnosti databází: **Consistency, Availability, Partition tolerance** Teorém říká, že je možné dosáhnout jen dvou těchto vlastností a je nutno obětovat třetí. Klasické SQL databáze nabízí konzistenci a dostupnost. Není však možné tolerovat rozdělení jednotlivých uzlů. Většina databází, které používají více uzlů mají P, jelikož musí fungovat i pokud jsou problémy na síti a nedokáži spolu komunikovat
#### PC
Pomalé zápisy, protože se musí uzly shodnout na datech k zajištění konzistence. Pokud spolu ztratí kontakt, tak nelze zapisovat, aby se neztratila konzistence. Není moc často používané.
#### PA
Tento model je vhodný pro systémy, kde je důležitější udržet systém funkční a dostupný i při problémech se sítí než udržovat striktní konzistenci dat. Představte si cloudové úložiště, které je distribuováno mezi více datových center. Když dojde k výpadku sítě mezi těmito centry (dělení sítě), systém stále umožňuje uživatelům ukládat a získávat data (dostupnost), ale nemusí okamžitě zaručit, že všechny kopie dat jsou v každém centru úplně stejné (konzistence). Používají se kvóra -> kolik uzlů musí potvrdit operace zápisu a čtení, abychom je mohli brát jako splněná. Většinou je to aspoň polovina uzlů. 
#### BASE
viz  [otázka 7](./7_distribuovane_systemy.md#rozd%C3%ADl-mezi-centralizovanou-a-distribuovanou-architekturou-syst%C3%A9mu-nev%C3%BDhody-oboj%C3%ADho-a-jejich-p%C5%99ekon%C3%A1v%C3%A1n%C3%AD)

## Architektury a základní charakteristika hlavních zástupců NoSQL databází (key-value uložiště, dokumentové databáze, column-family uložiště, grafové databáze)
### Key-value
- Data jsou uložena jednoduše jako klíč-hodnota. Většina databází umožňuje přístup k hodnotám jen pomocí klíče, nelze hledat většinou podle hodnoty. Klíč může dodat sám uživatel, nebo je vytvořen systémem. Poskytuje základní operace, jako get pomocí klíče, změnu hodnoty, nebo její vymazání. Data můžou být rozmístěna do různých namespaces (něco jako tabulky). 
- Řešíme otázku, jestli data udržovat oddělená, nebo pomocí agregátů. Pokud chceme často přistupovat k většině dat, tak lepší agregát.
- Sharding většinou pomocí nějaké hashovací funkce a klíče. Každý uzel je zodpovědný za nějaký rozsah údajů (circle).
- u P2P musíme nějak řešit konflikt write-write při replikaci dat -> většinou razítka u údajů, každý uzel si udržuje svoj, nebo se propagují -> pokud dva různí uživatelé zapisují do stejného záznamu na různých uzlech: Poslední vyhrává, novější zápis vyhrává, data můžou být nekonzistentní, než uživatel vyřeší, quorum based, uživatel je vyzván k opravě
- nedoporučuje se, když chceme přistupovat podle dat, pokud jsou vztahy mezi daty
- vhodné pro ukládání uživatelské relace, košíku a dat, kde chceme přistupovat podle klíče a nechceme dělat složité dotazy -> také dobré u cache
- příklad je Redis, který je používaný v cache -> zvolíme si klíč a objekt, který chceme zacachovat -> následně se DB zeptáme, jestli záznam existuje v cache a má validní časový údaj. Dalším příkladem je Riak
- Riak je zajímavý v tom, že nabízí sekundární indexování, které si může navolit uživatel -> lze se tak ptát optimalizovaně i podle hodnoty
- Riak také nabízí text search
