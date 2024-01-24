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
