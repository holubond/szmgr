# Analýza a návrh systémů

> Objektové metody návrhu informačních systémů. Specifikace a řízení požadavků. Softwarové architektury, komponentové systémy. Návrhové a architektonické vzory. Rozhraní komponent, kontrakty na úrovni rozhraní, OCL. Modely softwarových systémů, jazyk UML. Příklady z praxe pro vše výše uvedené. ([PA103](https://is.muni.cz/auth/el/fi/podzim2021/PA103/um/), PV167, PV258)

## Objektové metody návrhu informačních systémů.

Přístupy a postupy k návrhu IS založených na objektově orientovaném paradigmatu, kde jsou objekty spojením dat a metod nad těmito daty.

Mezi metody se řadí modelování domény pomocí [UML](./dev_1_analyza_a_navrh.md#modely-softwarových-systémů-jazyk-uml), dekompozice systému do menších, koherentních částí, v různých částích vývoje se zabýváme různými úrovněmi detailů. Dále se aplikují návrhové a architektonické vzory, které popisují řešení na dobře známé a často se opakující problémy v (nejen) objektovém světě.

TODO popiš úrovně a použité diagramy, detaily diagramů tady nerozebírej
Fáze vývoje a používané diagramy:
- Analýza požadavků - [Use Case](./dev_1_analyza_a_navrh.md#use-case-diagram), [Conceptual Class Diagram](./dev_1_analyza_a_navrh.md#conceptual-class-diagram)





TODO.



## Specifikace a řízení požadavků.
TODO

## Softwarové architektury, komponentové systémy.
TODO

## Návrhové a architektonické vzory.

Návrhový vzor je obecné řešení k často se opakujícímu problému řešenému při návrhu sw, není potřeba kompletně vymýšlet vlastní řešení. Slouží nejen jako obecný návod pro implementaci, ale umožňují snadnější komunikaci v rámci týmu (e.g. tady použijeme Strategy pattern). Vzory je třeba používat s rozvahou, občas můžou být zbytečně obecné.

### Creational patterns

Řeší tvobu a inicializaci objektů, poskytují jednoduché rozhraní skrývající složitou inicializaci.

#### Singleton

Zajišťuje, že daný objekt existuje v systému jen jednou (globální stav). V OO jazycích se řeší pomocí třídy s private constructorem a se statickou metodou `instance()` poskytující přístup k objektu drženému ve statickém atributu. *Metoda `instance() se obvykle stará i o inicializaci statického atributu* 

Singleton je mnohdy považován za antivzor, protože vytváří globální stav (namísto předávání stavu parametry) - blbě se to testuje, může být nutné zamykání globálního stavu pro thread safety, narušuje se single responsibility principle (singleton třída ovládá svou tvorbu).

E.g. DB pool

![](img/20230603121311.png)


### Structural patterns

Řeší kompozici objektů do hierarchií, oddělení rozhraní a implementace.

#### Composite

Umožňuje tvorbu stromových struktur a poskytuje jednotné rozhraní k operaci na podstromu definovaném svým kořenem.`Component` je buď list `Leaf`, nebo uzel `Composite` obsahující potenciálně další `Component`y.

E.g. stavební prvky grafických rozhraní

![](img/20230603143753.png)

#### Adapter

A.k.a. Wrapper - zapouzdříme/poskytneme rozhraní nekompatibilní jednotce tak, aby se dala použít v našem systému.

E.g. integrace knihovny, případně můžeme adaptér použít k převodu mezi formáty (XML - JSON)

Adaptér lze implementovat ve všech populárních jazycích jako wrapper, je možná i implementace class adaptéru v jazycích podporujících mnohonásobnou dědičnost.

![](img/20230603161229.png)

### Behavioral patterns

Řeší chování objektů a dynamické interakce mezi objekty.

#### Iterator

Poskytuje jednotné rozhraní k průchodu prvky kolekcí. Je to samostatný objekt (specifický pro danou strukturu), má metody jako `current()` a `next()` umožňující přístup k prvku, nebo posunutí interního ukazatele iterátoru na další prvek.

E.g. implementace `for-in/foreach`.

![](img/20230603144800.png)

#### Strategy

Poskytuje rozhraní k výpočtu/operaci, které může klient použít bez znalosti konkrétní implementace a jejích detailů. Díky tomu je možné konkrétní implementace pro výpočet snadno měnit, nebo jednotně používat funkcionalitu objektů s rozdílnými implementacemi. Klient přistupuje přes `Context`, který se stará o případnou volbu strategie. Konkrétní strategie lze měnit za běhu. *?Iterátor by se dal považovat za formu strategy?*

E.g. libovolné použití přístupu přes rozhraní, třeba výpočet trasy (pro auto, pro cyklistu, pro chodce..)

![](img/20230603153352.png)

#### State

Obdobný jako Strategy, výběr implementace děláme na základě aktuálního stavu, který je možné měnit za běhu. O konkrétní stav (a výběr implementace) a jeho přeměny se stará `Context`, díky čemu izolujeme a můžeme jednoduše kontrolovat přechodu stavu v systému. 

Na rozdíl od `Strategy`
- daná implementace je vybrána na základě vnitřního stavu
- řešíme přechody stavu, stavy se můžou nahradit jiným stavem (=> stavy mohou mít referenci na kontext)
- neřešíme jeden specifický task, ale poskytujeme implementaci pro většinu věcí co `Context` nabízí

E.g. postavě se mění útok na základě nasazeného vybavení

![](img/20230603154910.png)

## Rozhraní komponent, kontrakty na úrovni rozhraní
TODO

## OCL.
TODO

## Modely softwarových systémů, jazyk UML.

Modely sw systémů popisují systém vždy z nějakého zjednodušeného pohledu (model je už z definice abstrakce). Různé modely se zabývají různými aspekty/fázemi vývoje systému. Důležité však je, aby byly modely systému vzájemně konzistentní. Obecně lze rozlišovat na modely popisující strukturu a modely popisující chování.

### Use case diagram
TODO

### Conceptual class diagram

Diagram tříd, ale neřešíme datové typy ani metody. Zajímají nás klíčové entity (struktury/třídy), jejich data plynoucí z požadavků, a vazby mezi entitami (kontext). Pomáhá ujasňovat terminologii.

TODO

## Notes

**Motivace objektových metod/návrhových vzorů**
- Systémy bývají složité, špatně se udržují a je náročné měřit/zajistit kvalitu, často se mění nároky
=> pomůže dekompozice systému do menších koherentních částí, které se lépe udržují/mění, snadněji se měří kvalita

Dekompozice podle [SOLID](./2_kvalita_kodu.md#solid-principy)
- single responsibility - každý modul/třída/funkce by se měly soustředit pouze na jednu část funkcionality (a tu zapouzdřovat)
- open/closed - každý modul/třída/(funkce) by měly být rozšiřitelné i.e. přidání změn způsobí minimální modifikaci kódu, většinou rozšiřujeme pomocí nových tříd/metod
- liskov substitution - každý (dědičně) nadřazený objekt by měl být nahraditelný podřazeným objektem, aniž by byl narušen původní kontrakt. E.g. nemůžeme vyhodit výjimku, když to nadřazený nikdy nedělal. Nemužeme brát u stejné metody konkrétnější argument, než jaký bere nadřazený objekt (je v pohodě brát abstraktnější). Nemůžeme vracet abstraktnější typ, než jaký vrací nadřazený. Je v pohodě přidávat funkcionalitu ve formě dalších metod.
- interface segregation - rozbíjíme velká rozhraní na menší, logicky související jednotky. Jen to, co klient opravdu může potřebovat.
- dependency inversion - závisíme na abstrakcích (rozhraní), ne na konkrétních implementacích

**Problém s cyklickou vazbou objektů** - e.g. v metodě toString() je potřeba vhodně řešit, abychom se necyklili. Proto může být vhodnější definovat si pro takové případy speciální objekty s jasnou hierarchií a bez cyklů

### Analytické vzory
Návrhové vzory nabízí řešení na často řešené problémy v návrzích systému. Tato řešení jsou místy až příliš sofistikovaná, takže se doporučuje složitější návrhové vzory používat z rozvahou, abychom problém *neoverengineeringovali*.

Accountability vzory *(přijdou mi ve slajdech popsány složitější, než jsou, proto popisuju koncepty/zapamatovatelné aspekty, zbytek si člověk dokáže odvodit. Odkazy vedou na příslušnou část s diagramem.)*
- [Party](./dev_1_analyza_a_navrh.md#party) - společný název (abstrakce) pro osobu či firmu, obvykle má kontaktní údaje (adresu, telefon, email...)
- [Organization Hierarchies](./dev_1_analyza_a_navrh.md#organization-hierarchies) - řešíme problém reprezentace organizace skládající se z často měnících se hierarchií organizačních jednotek (e.g. Korporace, Region, Pobočka, Oddělení... typy jednotek mohou být také předmětem změn). Řešením je stavební blok `Organizace`, která má 0..1 rodiče `Organizace` a  0..n potomků `Organizace` (rekurzivní vazba). Jednotlivé typy oddělení pak mohou dědit od `Organizace`.
- [Organization Structure](./dev_1_analyza_a_navrh.md#organization-structure) - To samé co organization hierarchies, ale přidáváme k tomu `TimePeriod` (pro verzování v čase), `Typ Organizační Struktury`, který může mít `Pravidla` zajišťující, že třeba oddělení nebude nadřízené divizi.
- [Accountability](./dev_1_analyza_a_navrh.md#accountability) - Organization Structure, ale Organizaci nahradíme Party (a vztahu říkáme accountability). Je tam opět `TimePeriod`, ale `Typ Organizační Struktury` se jmenuje `Accountability Type`. `Pravidla` pro vazby zahazujeme
- [Accountability Knowledge Level](./dev_1_analyza_a_navrh.md#accountability-knowledge-level) - Accountability, ale `Pravidla` pro vazby mezi jednotlivými `Party`s zase přidáme. `Pravidla` jsou definována pro jednotlivé `Accountability Type`s, každé definuje povolenou kombinaci `Party Type` potomka a rodiče v hierarchii. Úrovni, kde popisujeme pravidla (a kde tím pádem jsou i `Accountability Type`s a `Party Type`s) říkáme knowledge level, existuje jen pro zajištění správné kompozice (ale nemá moc význam pro day-to-day operace).

Příklad pro aplikaci accountability je ve [slajdech (str 35+)](https://is.muni.cz/auth/el/fi/podzim2021/PA103/um/02-03-Analysis-patterns.pdf#page=35).

Observations & measurements
- [Quantity](./dev_1_analyza_a_navrh.md#quantity) - kvantita má hodnotu a jednotku (v Rustu bychom použili Newtype pattern konkrétní jednotky a pomocí traitů implementovali funkcionalitu)
- [Conversion Ratio](./dev_1_analyza_a_navrh.md#conversion-ratio) - převedení jedné jednotky na jinou, samo o sobě funguje jen pro lineární vztahy
- [Compound Units](./dev_1_analyza_a_navrh.md#compound-units) - Jednotka může být buď `Atomic Unit` (e.g. kilometry), nebo `Compound Unit`, která má aspoň jeden `Unit Reference` obsahující mocninu (e.g. kilometry za hodinu). 
- [Measurement](./dev_1_analyza_a_navrh.md#measurement) - Reprezentuje výsledek měření. Každé měření bylo někým vykonáno (`Person`), zkoumalo nějaký měřený fenomén (`Phenomenon Type`) a zjistilo nějakou hodnotu, včetně jednotek (`Quantity`)
- [Observation](./dev_1_analyza_a_navrh.md#observation) - výše popsaný `Measurement` je typ `Observation`, stejně jako `Category Observation` umožňující zaznamenávat nekvantitativní měření s nějakou kategorickou hodnotou (e.g. počet lidí s danou krevní skupinou), kde nás zajímá konkrétní `Phenomenon` (e.g. A+), který je součástí `Phenomenon Type`.
    - je možné přidat i způsob měření `Protocol`, či sledovat přítomnost/nepřítomnost kategorického jevu, který může mít závislosti na (pod)jevech modelovaných pomocí `Observation Concept` (e.g. diabetik typu 2 je obecně diabetik)

### Diagramy analytických vzorů

#### Party
![](img/20230602212700.png)

#### Organization Hierarchies
![](img/20230602212810.png)

#### Organization Structure

![](img/20230602221810.png)

#### Accountability
![](img/20230602223147.png)

#### Accountability Knowledge Level
![](img/20230602224136.png)

#### Quantity
![](img/20230603105247.png)

#### Conversion Ratio
![](img/20230603105928.png)

#### Compound Units
![](img/20230603110857.png)

#### Measurement
![](img/20230603111248.png)

#### Observation
![](img/20230603112121.png)

Včetně způsobu měření a logickými vazbami mezi (pod)jevy
![](img/20230603112705.png)