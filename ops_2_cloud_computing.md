# Cloud computing
> IaaS, PaaS, SaaS cloud. Koncepty a principy návrhu a vývoje aplikací určených pro nasazení do cloudového prostředí. Cloudové služby v kontextu PaaS cloud, služby pro ukládání dat. Zajištění kvality aplikací v cloudovém prostředí, výkonnost, škálovatelnost, dostupnost, bezpečnost. Příklady z praxe pro vše výše uvedené.

## Cloud computing

Cloud computing je model poskytování výpočetních služeb prostřednictvím internetu, který umožňuje uživatelům přistupovat k široké škále zdrojů, jako jsou servery, úložiště, databáze, síťové komponenty, software a další služby. Tyto zdroje jsou hostovány a spravovány poskytovateli cloudových služeb a jsou uživatelům k dispozici na vyžádání, obvykle na základě předplatného nebo platby za využití.

Cloud computing nabízí několik klíčových výhod, jako je snížení nákladů na IT infrastrukturu, zvýšená flexibilita a škálovatelnost zdrojů, a také umožňuje uživatelům přistupovat k aplikacím a datům odkudkoliv, kde je připojení k internetu. Mezi výhody patří i dodržování předpisů a např. GDPR pro ochranu dat. Uživatel si většinou může určit, v jakém státě uchovávat data atd. Existují různé modely cloudových služeb, jako je veřejný cloud, soukromý cloud a hybridní cloud, každý s vlastními specifickými vlastnostmi a výhodami.

## IaaS, PaaS, SaaS
![](img/cloud_models.png)
#### IaaS (Infrastructure as a Service)
**Definice**: Poskytuje výpočetní výkon, disková úložiště, síťová řešení atd. Můžeme si například na cloudu rezervovat VM s určitým provisioned HW a operačním systémem.
**Funkce**: Alokuje virtuální stroje (VM) podle požadavků zákazníka, odstínění od manuálního nastavení sítě, firewallu a hardwaru. Samotný software jako instalace dockeru a nastavování prostředí pro aplikaci řešíme už sami.
**Zodpovědnost zákazníka**: Správa operačního systému a vyšších vrstev infrastruktury. Musíme sami instalovat potřebný software, aktualizovat systém a SW. Částečně i nastavování síťových pravidel atd.
**Výhody**: Stabilita (provideři mají většinou implementovanou high availability, kterou bychom u klasické VM nedostali), jednoduchá vertikální škálovatelnost (dokážeme celkem jednoduše přidat HW pro virtualizaci a zaplatit více), nemusíme spravovat vlastní hardware. Oproti on-premise nulové počáteční investice do HW a možnost platby podle aktuálního vytížení systému.
**Nevýhody**: Závislost na externím systému, potřeba vlastní konfigurace a znalost požadavků. Ve výsledku není extra moc automatizované, je to podobné jako kdybychom dostali od nějakého hostingu přístup k virtuálnímu stroji/mašině. Pokud víme, že budeme mít stálé využití HW, jsou on-premise řešení dlouhodoběji levnější.
**Příklady**: Azure Compute, HPC, GCP Compute Engine, Azure Storage managed disk, Google Persistent Disk.
#### PaaS (Platform as a Service)
**Definice**: Kompletní vývojářské prostředí v cloudu. Píšeme aplikaci dle nějakého protokolu (C#, Python aplikace) a cloud nasazení vyřeší již kompletně za nás. Je možné využívat a instalovat dependence pro projekt.
**Funkce**: Odstínění od O/S,middleware úrovně (neřešíme záplaty systému, instalování potřebných dependencí). Cloud poskytuje celkové nastavení a deployment pro aplikaci, stačí jen nahrát kód do repozitáře a mělo by všechno proběhnout automaticky.
**Výhody**: Automatizované škálování a nasazování, větší bezpečnost na O/S, middleware vrstvách, jelikož jsou spravovány cloudem a aktualizovány. Opadá také práce a nutnost, která souvisí s udržováním těchto vrstev.
**Nevýhody**: Ztráta kontroly nad některými vrstvami, složitější debugování, nutnost dodržovat protokol pro daný typ aplikace (cloud povoluje jen drobnější změny)
**Příklady**: Azure Web App, GCP Cloud Run, Azure Storage, Amazon S3, Google Cloud Storage.
#### SaaS (Software as a Service)
**Definice**: Hotový software poskytovaný uživatelům.
**Funkce**: Uživatelé přímo využívají software bez potřeby jeho nasazování nebo správy zdrojů.
**Výhody**: Minimalizace nákladů a starostí o aplikaci. Používáme buď jako uživatel, nebo pomocí API a nestaráme se tak o nic jiného než odpověď a pricing za dotazy.
**Příklady**: Office 365, Google Apps (Gmail, Google Docs).
nějak sem to opravil, lepší?


## Vzory a principy návrhu cloudových aplikací:
Abychom plně využili možnosti cloudu, je třeba kromě klasických principů u aplikací myslet na tyto věci:
- **Škálovatelnost** - v cloudu je velmi jednoduché škálovat aplikaci vertikálně i horizontálně a měla by na to být připravená bez větších změn
- **Drobné chyby cloudu** - v cloudu se můžeme setkat se specifickými chybami jako milisekundové výpadky, nebo pomalý náběh u služby, která nebyla dlouho využívaná -> aplikace by s němi měla počítat
- **Chytré využívání zdrojů** - v cloudu je možné platit jen využívaný výkon aplikace, proto je důležité, aby aplikace využívala výkon, který opravdu potřebuje a zbytečně jim neplýtvala -> je také důležité rozmyslet se jaký typ služby a pricingu si vybereme, protože to dokáže dělat velké rozdíly




#### Výběr úložiště
Při výběru vhodného úložiště pro cloudové aplikace je třeba zvážit několik faktorů. Cloudové prostředí nabízí širokou paletu úložných možností, každá s vlastními specifikami, výhodami a omezeními. Například, Azure SQL Database nabízí bohaté možnosti dotazů a je ideální pro úložiště, které vyžadují silnou podporu transakcí a konzistence dat. Na druhou stranu, Azure Table Storage je vhodnější pro scénáře, které vyžadují vysokou propustnost a nízké provozní náklady, ale s méně náročnými požadavky na dotazy a konzistenci. Ve většině složitějších aplikací je vhodné kombinovat tyto typy databází, protože nejlépe chceme na různé use casy využívat jejich výhody. Zde jsou porovnány jednotlivé typy DB služeb u Azure (PaaS):

![](img/db_differences.png)

#### Materializované zobrazení vs cache-aside
**Materializované zobrazení** je užitečný pro situace, kde je potřeba optimalizovat čtecí operace. Materializované zobrazení předvyplňuje a udržuje data ve formě, která je přímo vhodná pro čtení, což může znamenat výrazné zlepšení výkonu pro často používané dotazy. Tento přístup je obzvláště užitečný, pokud jsou zdrojová data složitá a jejich příprava pro dotazy je náročná na výpočetní výkon.
Je třeba si uvědomit, že relační databáze jsou jedním z nejčastějších bottlenecků v aplikaci. Pokud si dotazy předpočítáváme, ušetříme složité dotazy na databázi. Materializované zobrazení nemusí mít vždy nejnovější data z relační databáze, to nám však nevadí. Můžeme aplikovat různé principy na invalidaci.
Na obrázku jde vidět o kolik dokáže být rychlejší NoSQL databáze s předpočítanými dotazy:

![](img/sql_nosql_comparison.png)

**Cache-Aside** má podobné využití jako materalizované zobrazení, je zde však mnoho rozdílů. Cache-aside vzor nepředpočítává data a jen cachuje ty, které si uživatel vyžádal -> není potřeba dávat stejný dotaz do DB dvakrát. Data jsou oproti materializovanému zobrazení většinou ve stejné podobě jako z originální databáze -> materializované zobrazení předpočítává většinou jiný typ dat a ukládá je někam. U cache-aside patternu se ptáme přímo relační databáze a pokud dotaz již přišel dřív, posíláme cachovaný výsledek -> u materializovaného zobrazení máme většinou odlišnou DB, kam se dotazujeme jinak než na relační. U obou přístupů můžeme nastavit politiku, jakým způsobem, nebo jak často mají být data přepočítána/invalidována -> to ovlivňuje efektivitu těchto přístupů a pravděpodobnost, že jsou k dispozici nejnovější data. Každý přístup je použitelný v jiných situacích. Materializované view jsou vhodné pro scénáře s komplexními dotazy, kde je přijatelná mírná zpoždění v konzistenci (třeba předpočítávání zdi na FB). Cache aside je vhodný pro aplikace, kde se opakují jednotlivé cally do relační databáze a nepotřebujeme tam úplnou nutnost aktuálnosti dat.





Sharding Pattern: Efektivní rozdělení (sharding) dat je klíčové pro škálovatelnost aplikací v cloudu. Tento vzor umožňuje rozložit data do více oddělených oddílů nebo shardů, což umožňuje lepší distribuci zátěže a optimalizaci výkonu. Hlavní výzvou je navrhnout správnou strategii rozdělení tak, aby bylo možné efektivně ukládat a dotazovat data.

Hosting Statického Obsahu:

Při návrhu cloudových aplikací je důležité efektivně spravovat statický obsah, jako jsou CSS, JavaScript a obrázky. Tento obsah by neměl být hostován společně s dynamickým obsahem na aplikačním serveru. Místo toho je vhodnější využít služby jako Azure Blob Storage ve spojení se síťovým distribučním systémem (CDN), což může výrazně zvýšit rychlost načítání statického obsahu a snížit zatížení aplikačních serverů.
Vzor Valet Key:

Vzor Valet Key se používá pro poskytování bezpečného, ale omezeného přístupu k zdrojům. V cloudových aplikacích tento vzor umožňuje klientům přímý přístup k určitým zdrojům, jako jsou soubory uložené v cloudu, aniž by museli procházet aplikačním serverem. Tím se snižuje zatížení serveru a zvyšuje se efektivita přenosu dat.
Oddělení Zodpovednosti za Příkazy a Dotazy (CQRS):

CQRS je vzor, který odděluje čtecí operace (dotazy) od zapisovacích operací (příkazy) v aplikaci. Toto rozdělení umožňuje optimalizovat každou část aplikace pro její specifické potřeby, což vede k vyšší efektivitě, lepší škálovatelnosti a usnadňuje správu.
Messaging a Zpracování Dat:

Ve světě cloudových aplikací je často výhodné využívat asynchronní zpracování zpráv, což umožňuje efektivnější a škálovatelnější architekturu. Asynchronní zpracování je zvláště užitečné pro dlouhotrvající nebo náročné operace, kde by synchronní zpracování mohlo vést k časovým limitům nebo špatné škálovatelnosti. PaaS cloudu nabízí různé služby pro zasílání zpráv, které mohou být využity k oddělení různých částí systému a zlepšení celkového výkonu.
