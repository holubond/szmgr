# Kvalita kódu

> Kvalita ve vývoji softwarových systémů, atributy kvality a softwarové metriky. Taktiky pro zajištění kvality na úrovni jednotlivých atributů kvality. Principy Clean Code a SOLID, refaktoring kódu. Testování kódu, jednotkové testy, integrační testy, uživatelské a akceptační testy. Ladění a testování výkonu. Proces řízení kvality ve vývoji softwarových systémů. Příklady z praxe pro vše výše uvedené. [PV260](https://is.muni.cz/auth/el/fi/jaro2023/PV260/um/lectures/), PA017, PA103

## Kvalita ve vývoji softwarových systémů

- důležitý aspekt při vývoji sw systémů, 
- kvalita je často definována jako **schopnost produktu dostát požadavkům** => je důležité si určit, co jsou požadavky
    - **zákaznické požadavky** a.k.a. externí kvalita (použitelnost, přesnost/správnost, spolehlivost, bezpečnost, výkon..)
    - abychom dostáli těmto ^, je třeba, aby šel vývoj snadno, aby se produkt dal jednoduše dlouhodobě udržovat (byl snadný modifikovat/rozšířit), a aby nebyl zbytečně drahý (skrz náklady na provoz i cenu úprav). Toho docílíme dodržováním **interní kvality produktu** (modularita, jednoduchost jednotek, testovatelnost, přizpůsobitelnost změnám, čitelnost kódu, znovupoužitelnost, škálovatelnost, přenositelnost, udržitelnost, dodržování standardů...)
- špatná externí kvalita je často symptomem špatné interní kvality produktu (opravy chyb trvají dlouho, systém je pomalý... ale nemusí to být vždy pravda, třeba jen máme slabší UI)

## Atributy kvality

- za nejdůležitější atributy kvality kódu se považují
    - **Udržitelnost (maintainability)** = snadnost úprav bez technického dluhu
    - **výkonnost** = reakční doba systému (a efektivita využití zdrojů)
    - **Spolehlivost** = pravděpodobnost bezchybného fungování po určitou dobu
    - **Testovatelnost** = jak snadno (a co všechno) lze systém testovat
    - **Škálovatelnost** = schopnost systému zpracovat větší množství dat/uživatelů..
    - **Bezpečnost** = jak je systém odolný vůči útokům
    - **Použitelnost** = snadnost používání systému a jednoduchost učení se práce s ním, správná funkcionalita (obvykle samostatný bod)

## Softwarové metriky
- měřitelné aspekty sw systému (počet řádků kódu, pokrytí testy, cyklomatická složitost...), které nám dávají informace o celkovém obrazu, ale může být netriviální je vhodně interpretovat
- e.g. 100% pokrytí testy nemusí znamenat, že v systému nejsou chyby. Velký počet malých tříd zní dobře, ale třídy mohou být naprosto nelogicky strukturované a vzájemně silně závislé...
- mohou být přímé (to co přímo změříme, e.g. počet defektů) nebo odvozené (vypočítané z přímých, e.g. hustota defektů; počet defektů na velikost produktu)

- **Lines of code**
- **(Non)Commented lines of code** - řádky (ne)obsahující komentář
- **Počet tříd**
- **Počet funkcí/metod**
- **Počet packages**
- **Počet souborů**
- **Provázanost tříd** - kolik jiných tříd voláme
- **Hloubka dědičnosti**
- **Počet metod na třídu vážený složitostí**
...
- **Cyklomatická složitost** - počet nezávislých cest ve zkoumané jednotce (obvykle funkce), kterými se může běh vydat
    `= <počet hran/větví> - <počet vrcholů/nevětvených bloků> + 2<počet vzájemně nepropojených grafů(pro funkci 1)>`
    - nejnižší je 1 (bez větvení)

často nás spíš zajímají poměry/odvozené metriky, e.g. procento komentů z celkového počtu řádků, průměrná velikost metody, odchylky v rámci projektu/mezi releases...

- metriky je nebezpečné používat k ohodnocení výkonu vývojáře

## Taktiky pro zajištění kvality (pro jednotlivé atributy)

- **Udržitelnost (maintainability)** - refaktoring na koherentní jednotky, aby bylo místo nutné změny minimální a snadno lokalizovatelné, separace dat od logiky (aby bylo možné jednotku nahradit jinou), decoupling (závislosti na rozhraních, namísto na implementacích)
- **Výkonnost** 
    - kešování
    - paralelismus
    - asynchronní komunikace/zpracování
    - detekce a mitigace bottlenecků
    - odstranění adaptérů, zjednodušení komunikace
    - separace dat od logiky
- **Spolehlivost** 
    - detekce a náprava zdrojů nespolehlivosti
    - kontrolní mechanismy pro zajištění spolehlivosti
    - vhodné ošetření chyb,
    - automatický reporting neočekávaných chyb,
    - timeout po requestu
    - pravidelný ping (iniciuje volající služby)
    - heartbeat (služba pravidelně reportuje stav)
    - pravidelné snapshoty a rollback v případě pádu
    - transakce
    - kontrola vstupů na každé úrovni
    - odstranění single point of failure
- **Testovatelnost** 
    - refaktoring na závislosti na abstrakcích, decoupling
    - separace dat a logiky
    - odstranění globálního stavu
    - u UI systému například dodržování standardů používání id/class názvů atd.
- **Škálovatelnost** 
    - refaktoring na jednodušší, samostetně nasaditelné jednotky
    - extrakce dat pro umožnění paralelizace jednotek
    - extrakce a samostatné nasazení subsystému 
    - distribuce a/nebo replikace dat (db bývá bottleneck, ostatní věci lze snadněji paralelizovat)
- **Bezpečnost** 
    - detekce a oprava chyb
    - použití šifrované komunikace
    - automatická kontrola zranitelností v kódu pomocí CI (např. Snyk)
    - udržování updated dependencí
    - zabezpečení soukromých klíčů a dat
- **Použitelnost** 
    - zlepšení UX
    - použití taktik pro zlepšení výkonnosti/škálovatelnosti (když je to pomalý)

## Clean Code

- čitelný, snadno pochopitelný. Kód bývá mnohem více čten než psán, proto je důležité, aby byl srozumitelný, čas vývojářů je drahý. Klíčové je
    - **jasné pojmenovávání** reflektující doménu problému, dostatečně výstižné (a ne příliš dlouhé či generické, viď, Javo). V ideálním případě by mělo být sebevysvětlující a komentáře by neměly být potřeba, ALE i tak jsou komentáře fajn pro vysvětlení myšlenky, nebo shrnutí, co má daná funkce dělat
        - důležitá je konzistence napříč codebase (ideálně stavěná na standardech daného jazyka) - například můžu definovat, že `check*()` metody vrací true/false a `verify*()` vyhazují vyjímky
        - pokud vrací bool, pojmenuj to `has*()` nebo `is*()`
        - struktury jsou podstatná jména, metody začínají slovesem (nebo se jedná o getter v rustu)
        - veřejné API (public) jednotky by mělo být jasné a jednoduché, interně (private) se mohou používat delší názvy metod, když je díky tomu jasnější, k čemu slouží
    - **rozumná velikost jednotek** - ideálně krátké funkce, jednoduché třídy... single responsibility principle. Obsah jednotky by měl reflektovat její název
    - **užívání standardů** jazyka/technologie

Dále se řídí principy:

### Don't repeat yourself (DRY) princip
- každá informace by měla být v systému jednoznačně definována na jediném místě
- e.g. dokumentaci generujeme ze zdrojáku, abychom neměli více sources of truth
- e.g. definujeme schéma (prisma), ze kterého vygenerujeme jak SQL tabulky, tak struktury pro náš jazyk
- e.g. vytáhneme sdílenou funkcionalitu do vlastní funkce
- ... platí na vše, co může být v systému duplikováno (ale i v procesech, třeba opakované manuální spouštění testů => automatizovat)

### Keep it simple stupid (KISS) princip
- jednoduchost před výkonem
- nejlépe fungují systémy, které jsou co nejjednodušší
- není důvod používat složité techniky na jednoduché problémy

### You Ain't Gonna Need It (YAGNI)
- nezabýváme se tvorbou něčeho, co nebudeme potřebovat (e.g. neděláme přílišné abstrakce pro podporu možné budoucí funkcionality, pokud to není nutné)
- je lepší věc udělat jednoduše a pak ji snadno upravit, než ji udělat univerzálně, abychom pak zjistili, že nás nenapadl nějaký edge case a musíme to stejně celé přepsat. Vývoj postupuje po malých krůčcích.

## SOLID

- **Single responsibility**
    - každá třída by měla mít pouze jednu zodpovědnost, a.k.a. pro každou třídu by měl být pouze jeden důvod, proč by se měla změnit (e.g. FileReader by se měl starat pouze o čtení ze souboru, ne o zpracovávání čtených dat. Pouze změna způsobu čtení ze souboru může zapříčinit, že musíme měnit FileReader)
    => nižší provázanost (závislosti) tříd, vyšší koheze (zaměřenost na jednu věc), flexibilnější kód...

- **Open/closed principle**
    - Otevřeno pro rozšíření, uzavřeno pro modifikaci, preferujeme přidávání nové funkcionality před změnou zdrojového kódu/binárky toho, co už máme
    => menší šance, že něco rozbijeme, na nových třídách nic nezávisí
    - používá se implementace rozhraní/abstraktní třídy
    - dodržování OCP způsobuje vyšší komplexitu (a třeba v rustu trochu ubírá výkon díky nutnosti dynamic dispatch), takže je potřeba ho používat obezřetně a jen tam, kde se často mění/přidává funkcionalita
    - např. v Pythonu (a jiných jazycích) se dost často vyplácí přidávat k existujícím funkcím optional variables -> funkce se pro existující volání chová stejně, ale se změnou nového atributu je možno dosáhnout jiného chování
    - Příklad: Máte třídu ReportGenerator s metodou generateReport. Chcete přidat nový typ reportu. Místo úpravy ReportGenerator, vytvoříte novou třídu FinancialReportGenerator, která rozšiřuje ReportGenerator a přepisuje metodu generateReport pro generování finančních reportů.

- **Liskov substitution principle**
    - instance tříd by měly být nahraditelné jejich podtřídami, aniž by došlo k narušení chování systému - všechny podtřídy by měly dodržovat kontrakty nadtříd a neměly by odstraňovat chování nadtříd
    - problém je, když musíme explicitně ověřovat, o jaký podtyp se jedná - toto by měl řešit polymorfismus
    - pokud dvě třídy sdílí mnoho funkcionality, ale nejsou nahraditelné, je vhodné je upravit na podtřídy nové třídy obsahující sdílené chování
    - Příklad: Máte třídu Bird s metodou fly. Třída Duck je podtřídou Bird a může používat metodu fly. Ale když vytvoříte třídu Ostrich (pštros), který je také ptákem, nemůže používat metodu fly, protože pštrosi nelétají. Zde porušujete Liskov Substitution Principle, protože podtřída Ostrich nemůže správně nahradit svou nadřazenou třídu Bird.

- **Interface segregation principle**
    - klienti kódu by neměli být závislí na metodách, které nepoužívají, a.k.a. dělej malá a jednoduchá rozhraní namísto velkých
    - e.g. chci v rustu převést strukturu na string. Jediné co proto musím udělat je zajistit implementaci Display traitu (a ničeho jiného).
    - Příklad: Místo jednoho velkého rozhraní Vehicle s metodami jako drive(), fly(), a sail(), ISP by doporučil rozdělit toto rozhraní na několik menších, jako LandVehicle, AirVehicle, a WaterVehicle
      
- **Dependency inversion**
    - moduly by měly záviset na abstrakcích (rozhraní), ne na konkrétních implementacích
    - snižuje se tím provázanost modulů, je možné poskytnout vlastní implementaci či mockovat
    - konstruktur by měl přijímat vše, na čem struktura závisí, ne si vytvářet zdroje sám (e.g. repo si nemá tvořit připojení do databáze, ale má být předáno v konstruktoru) = dependency injection konstruktorem
    - Například, pokud máte třídu Car závislou na konkrétní třídě GasEngine, změna na elektrický motor by vyžadovala změnu v třídě Car. Ale pokud Car závisí na abstraktním rozhraní Engine, lze snadno vyměnit GasEngine za ElectricEngine bez nutnosti měnit kód třídy Car.

## Refaktoring

- úprava modulu takovým způsobem, aby se nezměnilo jeho externí chování, ale pouze došlo ke zlepšení jeho interního struktury/modifikovatelnosti...
- před refaktoringem je důležité mít chování solidně pokryto testy, abychom nezpůsobili nechtěnou změnu
- během refaktoringu neděláme nic jiného (žádná nová funkcionalita)
- techniky (některé editory je podporují, což usnadňuje práci a je pravděpodobně spolehlivější)
    - **Extrakce funkce** - kus kódu funguje jako jednotka/potřeboval by komentář => vytáhni ho do funkce, dej tomu přiléhající jméno, bude možné to použít na více místech
    - **Inline funkce** opak -//-, vhodné pro triviální situace jako `isMoreThanFiveEven(x)`
    - **Nahrazení funkce metodou struktury** - fajn, když funkce používá ranec proměnných => stanou se fieldy struktury
    - **Move method/field** - z jedné do jiné struktury, pokud to dává smysl (třeba doménově)
    - **Extrakce/inline třídy** - z třídy obsahující množinu polí, která jsou related, vytáhneme nový objekt, který bude původní třída obsahovat/nebo naopak pro inline
    - **Early return** - obecně chceme, aby funkce popisovala správný/bezchybný tok programu. Pokud při zpracování funkce objevíme chybu ve vstupních datech, hodíme tam return. V takových případech nepoužíváme `if-else`, ale `if return`
    - **Rename** cokoliv
    - **Seskupení mnoha parametrů do struktury**

Kód, který se dobře čte a udržuje nemusí být ten nejrychlejší/nejefektivnější (abstrakce mohou něco stát). Obvykle nám mírné snížení výkonu za vyšší čitelnost nevadí, ale nemusí to být vždy pravda.

## Testování
= proces evaluace, zda systém splňuje specifikované požadavky... což se snadněji řekne, než dělá
- v praxi je testování z pravidla nekompletní. Testováním odhalujeme chyby, ale nedokazujeme bezchybnost.
- každý test by měl testovat pouze jednu věc/vlastnost/feature, ideální je spousta malých testů, díky čemuž můžeme snadno identifikovat zdroj problému.
- ideálně by měl testování provádět někdo jiný, než autor testovaného kódu - tohle je důležité hlavně při black box metodách, u unit testů je dost časté, že je píše sám vývojář

- **whitebox (strukturální)** - vidíme zdrojový kód a můžeme vstupy testů cílit na spouštění kritických míst (off-by-one error, zero division...)
    - e.g. unit, integration, performance tests
- **blackbox (funkcionální)** - nevidíme co se děje uvnitř systému, pouze sledujeme vstupy a výstupy 
    - e.g. acceptance tests, system tests

- pokud narazíme na chybu, pro kterou nebyl test, je důležitá nejen oprava, ale i přidání (ideálně automatizovaného) testu, aby se chyba už nemohla opakovat
- regresní testování - sledujeme, zda změny v systému nepřinesly pády (automatizovaných) testů
- smoke testy - sledujeme, zda vybrané kritické funkce fungují v novém prostředí. Pokud ne, nemá vůbec cenu nasazovat a testovat další věci
- sanity testy - jako smoke, ale spouští se pro ověření nápravy chyb/přidání funkcionality
- A/B testování - používáme dvě varianty a sledujeme, která je úspěšnější (obvykle při testování UI)
- :hahaa: v praxi někteří experti praktikují melounové testování pro zvýšení test coverage, zvenku zelené, uvnitř červené :hahaa:

- kvalita testů lze oveřit mutačním testováním. Do aplikace zavedeme defekty (mutací zdrojového kódu, lze automatizovat třeba negací operátoru, off-by-one, vynechání volání...) a sledujeme, kolik jich bylo odhaleno testy => pokud něco prošlo, může jít o kandidáta na další testy
- některé situace jsou pro náš produkt rizikovější (lze odhadnout při analýze), než ostatní - na ty bychom se měli zaměřit při testování
- vstupy testů vhodně rozdělujeme na kategorie (e.g. <0, 0, >0), z každé vybereme pár reprezentantů (abychom nemuseli testovat úplně každou hodnotu)
- typy pokrytí testy jsme popsali v [pokrytí testy](#pokrytí-testy)

- testování si můžeme usnadnit tím, že v systému modelujeme nevalidní stavy jako nereprezentovatelné (rust enum <3, builder pattern, stavový automat...)

### Pokrytí testy
- Můžeme sledovat různá kritéria, pokrytí znamená, že danou cestou kódu prošel aspoň jeden test, metrika je obvykle v procentech
    - **line/statement coverage** - pokryté řádky/výrazy
    - **function coverage** - pokryté funkce/metody, jde o to, zda byla aspoň jednou zavolána
    - **branch coverage** - pokryté logické větve programu
    - **condition coverage** - každá boolean podmínka byla vyhodnocena jako true i false
- mnohdy není 100% pokrytí možné (pokud třeba někde něco reduntantně testujeme, better be safe than sorry) a zároveň 100% pokrytí neznamená bezchybnost
- některé části kódu je mnohem těžší pokrýt, než jiné
- může pomoct hledat části kódu, které jsou neotestované, ale o kvalitě testů se toho moc nedozvíme

### Jednotkové (unit) testy 
- validace, že se izolovaná jednotka kódu (funkce/třída) chová tak, jak bychom očekávali
- white box
- testy jsou automatizované, rychlé, jednoduché, čitelné, deterministické, každý testuje jednu jedinou věc
- izolujeme jednotku od zbytku systému pomocí *test double*s, nafejkovaných závislostí
    - dummy objekt - nikdy se nepoužije, ale je potřeba třeba jako parametr
    - fake objekt - jen pro účelý testů, jednoduchý, ale v praxi nepoužitelný (e.g. in-memory db)
    - stub - vrací vždy stejnou věc
    - spy - je schopen si zapamatovat, jak a s čím byl volán (e.g. volala se metoda odeslání mailu s tímto obsahem)
    - mock - předprogramovaný objekt (když tě někdo zavolá s parametrem A, uděláš toto, jinak něco jiného)
- *AAA*, arrange (příprava), act (provedené testovaného chování), assert (ověření) - tři fáze každého testu, act by měl být co nejkratší
- e.g. cargo test, jest, junit
- pokročilejší techniky zahrnující analýzu zdrojového kódu a následné vygenerování vstupních hodnot (symbolic execution), případně formální verifikace využívající matematických důkazů, model checking...
- v Pythonu existuje knihovna hypothesis - ta dokáže generovat kombinaci různých zajímavých vstupů pro testy (větší než 0, blízko maximální hodnotě daného typu atd.) a kontrolovat nějaké predikáty, které očekáváme

### Integrační testy
- sledují, zda spolu jednotky interagují tak, jak bychom očekávali
- pomalejší, větší a složitější, než unit testy
- black/white box

### Systémové testy
- ověření, že systém splňuje specifikované požadavky
- testují použitelnost, kapacitu, výkon, splnění funkcionality, bezpečnost...
- benchmarking, penetrační testování, uživatelské testy...
- lze automatizovat pomocí programem ovládaným prohlížečem (selenium, puppeteer)
- je velmi časté vyvíjet další nezávislý systém/software na validaci/testování nějakého většího produktu - automatizovatelnost různých scénářů a i interní nástroj, který dokáže zajistit větší kvalitu kódu již při vývoji
- black box

### Akceptační testy
- ověření, že systém splňuje business požadavky a je připraven k vydání
- může být ve formě odškrtávání políček s požadavky na systém, které zákazník předem určil
- prováděny se zákazníkem
- pokud jsou dev/test týmy odděleny, dost často probíhá psání těchto testů paralelně -> developeři tvoří danou feature a zároveň se na ni připravují i automatizované testy podle popsaného scénáře -> feature je kompletně akceptována, až projdou dané testy
- black box


### Test-driven development (TDD)
- skládá se z tří fází, red, green, blue, které iterativně aplikujeme. V každé části se snažíme docílit pouze jedné věci (a ničeho jiného, holt počkáme do další fáze)
- **red/test** - vytvoříme failující test pro co nejmenší část funkcionality, kterou chceme implementovat.
- **green/write** - implementujeme funkcionalitu co nejjednodušeji tak, aby test prošel (a zároveň nerozbil jiný test)
- **blue/refactor** - upravíme implementaci tak, aby odpovídala standardům, aby byl kód hezký...

### Behaviour-driven development (BDD)
- se zákazníkem sepíšeme chování systému jako jednotlivé scénáře
- scénáře slouží vývojářům i testerům jako jednotky
- e.g. gherkin, cucumber - konstrukty given, when a then (jako AAA) se používají pro definici scénářů v english-like jazyce srozumitelném zákazníkovi, tyto scénáře se pak objevují i v testech

## Ladění a testování výkonu

Cílem je identifikace a řešení případných problémů týkajících se rychlosti, odezvy a propustnosti systému, nalezení hranic

- **Load testing**
    - zátěžové testy, sledujeme jak systém zvládá dlouhodobější zátěž
- **Stress testing**
    - sledujeme, jak se systém vypořádává s krátkodobými výkyvy v zátěži (když najednou přijde spousta požadavků)

Běžící systém je také vhodné dlouhodobě monitorovat, abychom odhalili další slabá místa.

Výkon lze obecně zvýšit za cenu dalších atributů (například maintainability), proto je nutné volit správný kompromis pro náš případ.

e.g. Gatling, Siege, LoadRunner

## Proces řízení kvality ve vývoji softwarových systémů

skládá se z
- definice požadavků na sw kvalitu a plánování - specifikace funkcionálních i nefunkcionálních požadavků, stanovení hodnotících kritérií, rizik, určení metrik, podrobný popis a rozvržení aktivit k zajištění kvality
- zajištění (assurance) sw kvality - definice a kontrola procesů, které povedou k zajištění sw kvality a prevenci defektů (mimo jiné nastavení CI/CD)
- kontrola sw kvality - kontrola, zda produkt/jeho části splňují požadavky (včetně požadavků na kvalitu) a jejich vývoj se řídí definovanými procesy, monitoring zda se držíme procesů a vytyčených cílů
- zlepšení kvality - snaha zlepšit procesy, abychom docílili zlepšení kvality
- u velkých produktů mohou existovat speciální týmy, které mají za úkol validaci kvality produktu. Ty nejen tvoří testy a scénáře, ale mohou vyvíjet i vnitřní infrastrukturu na automatizování celého tohoto procesu. 

## Notes
- **Capability Maturity Model** - definuje úrovně vyspělosti organizace v kontextu zajištění kvality
    - Level 1 Výchozí - Procesy jsou ad hoc a často chaotické. Úspěch je závislý na individuálních úsilích.
    - Level 2 Opakovatelný - Organizace má stabilní procesy pro projekty podobného typu, které umožňují opakování dřívějších úspěchů.
    - Level 3 Definovaný - Procesy jsou standardizované a dokumentované napříč celou organizací.
    - Level 4 Řízený - Organizace monitoruje a měří procesy, a používá tyto data pro řízení kvality a efektivity.
    - Level 5 Optimalizující - Organizace se neustále zlepšuje a optimalizuje své procesy na základě shromažďovaných měření a zjištění.


### Prevence problémů kvality
- následování best practices a konvencí, obecných (SOLID, clean code) i pro danou technologii/jazyk (eslint, cargo fmt)
- využití principů návrhových vzorů
- techniky jako TDD, párové programování, code reviews
- použití procesních standardů (ITIL), agilních technik (scrum, kanban)
- automatizované testování, CI
- komunikace, jednotný jazyk
- fail-fast přístup - snažíme se detekovat problém ve vstupech, namísto abychom klidně akceptovali cokoliv a pak se divili při neočekávaném chování - příklad u typování JS/Pythonu -> mnoho chyb odhalíme už při kontrole a není třeba ani spouštět kód
- design by contract - naše metody (zvlášť při tvorbě) mohou vyžadovat splnění určitého kontraktu (lze vynutit asserty), aby mohly poskytnout garance o výstupech. Je možné použít podmíněnou kompilaci a mít kontrakty třeba jen ve vývojovém prostředí (tím se ale můžeme připravit o přesné určení místa problému na produkci) 

Nonfunkcionální problémy kvality se řeší architekturou. Pro prevenci těchto problémů je možné vytvořit model systému a na něm si simulačně ověřovat požadavky (e.g. schopnost obsloužit určitý počet požadavků za určitý čas) a případně odvodit nároky na jednotlivé komponenty (třeba maximální dobu zpracování požadavku v daném komponentu).

Pro ověření kvality je také  možné použít formální verifikaci (používá se třeba pro dokazování správnosti algoritmů).

### Detekce problémů kvality
- code reviews (vzájemné mezi vývojáři), inspections (formální, je fajn použít formulář; ukazuje to přípravu, na nic se nezapomene a zároveň se odfiltrují zbytečnosti, nelpíme na stylu, řešíme správnost, dodržování standardů...)
- (automatizované) testování (rust\cargo test)
- statická analýza (rust\cargo clippy, borrow checker, sonarqube) - nespouštíme kód

### Špatný kód
- pomíchané úrovně abstrakce
- nízká koheze (megatřídy, dlouhé funkce..)
- kruhové závislosti (je pak závislost na implementaci, blbě se sleduje flow a vztahy, blbě se to testuje, udržuje a škáluje)
- duplikace kódu
- spousta parametrů
- blbé názvy
- dělání věcí příliš "chytře", když to není nutné
- nedodržování standardů/vhodných konstruktů jazyka
- magické konstanty

#### Code smells a řešení

!Jednotlivé taktiky mohou být vzájemně v rozporu, je potřeba si určit, čeho chceme docílit! (e.g. udržitelnost vs výkon)

- **Udržitelnost**
    - příliš brzké optimalizace => nejdřív profiluj, pak případně optimalizuj
    - přílišná flexibilita => snaž se o jednoduchost, pak případně rozšiřuj
    - snaha o moc chytré řešení => stavební základy by měly být co nejjednodušší
    - nepoužívání standardů, principů návrhových vzorů
    - nízká modularizace
- **Výkonnost**
    - redundantní práce => kešování, memoizace, bottom-up approach dynamického programování
    - sekvenční zpracování/hledání => binární hledání, chytřejší algoritmy, práce se seřazenými kolekcemi, paralelizace go brrrrrr
    - dlouhé kritické sekce (ve vícevláknových programech) => minimalizujeme kritickou sekci, může být lepší použít vícero zámků
    - aktivní čekání => async zpracování, necháme se notifikovat až operace skončí..
- **Spolehlivost** 
    - nevalidujeme vstupní data, slepá důvěra
    - špatný error/exception handling
    - nepředpokládáme, že by funkce mohl někdo zavolat v jiném pořadí
    - přílišný hypetrain, používáme technologie, kterým úplně nerozumíme
    - absence logování => loguj, je fajn vědět, co se v systému dělo před pádem
    - snadný pád celého systému kvůli jedné části => nasaď více služeb, implementuj restart/recover, automatické přepnutí se na jinou, funkční službu 
- **Testovatelnost**
    - globální stav, proměnné
    - schovávání závislostí; je lepší provést dependency injection, než sázet na předchozí volání `init()` funkce pracující s globálním stavem
    - komunikace mezi jednotkami, které by neměly komunikovat => SOLID
    - nutnost hacky solutions, abychom vůbec mohli testovat => SOLID
    - nedeterminismus (závislost na čase, náhodnosti, globálním stavu, databázi... e.g. bacha na iteraci přes rust std::collections::HashMap, elementy jsou náhodně seřazené)
    - neoddělujeme inicializační a aplikační logiku
- **Škálovatelnost** 
    - monolitická aplikace, distribuce může zvýšit výkon/kapacitu, ale bacha na nutný režijní overhead, těžší testování, nasazování, bezpečnost...

### Sledování problémů kvality
- issue tracking
- správa technického dluhu (tracking, vyhrazení času na jeho nápravu)
