# Distribuované systémy
> Základní pojmy, principy. Rozdíl mezi centralizovanou a distribuovanou architekturou systému, nevýhody obojího a jejich překonávání. Replikace, sdílení dat. Architektura orientovaná na služby (SOA), webové služby. Příklady existujících technologií a jejich využití. Příklady z praxe pro vše výše uvedené. ([PA053](https://is.muni.cz/auth/el/fi/jaro2023/PA053/um/))

## Základní pojmy, principy

**Distribuovaný systém** se skládá z komponentů (počítačů) propojených komunikační sítí. Distribuované systémy řeší problémy spoluprací jednotlivých komponentů. Díky tomu se systém snadněji škáluje (posilujeme subsystém, který má problémy).

Architektury popsány v [otázce 1](./1_programovani_a_softwarovy_vyvoj.md#základní-koncepty-softwarových-architektur-z-pohledu-implementace-vícevrstvá-architektura-moderních-informačních-systémů-architektura-model-view-controller), takže jen shrnutí

**Monolitická architektura** - obsahuje vše, co systém potřebuje, je možné pouze vertikální škálování, špatná spolehlivost (pád znamená pád celého systému)

**Úrovňová (tiered) architektura** - jednotlivé úrovně lze distribuovat, paralelizovat, nahradit (komunikace skrz API)
- Klient může být tenký (e.g. teplating řešení typu php, django, askama)/tlustý (samostatná aplikace, separátně nasaditelná, e.g. js frameworky) dle poskytnuté funkcionality, klasické úrovně bývají klient, server, databáze.

**Hexagonal/Microkernel/component-based** - základní aplikace poskytuje minimální funkcionalitu, zbytek se dodává skrz plug-in komponenty komunikující přes předdefinované api. Komponenty je možné případně zapojovat za běhu systému. Další možnost využití komponentů - pokud potřebujeme používat legacy systém, který si nemůžeme dovolit přepsat, je možné ho zabalit jako komponent a přistupovat k němu přes naše kompatibilní rozhraní. Pokud komponenty zapojujeme sekvenčně, říká se tomu **pipeline architecture** (e.g. unix utilities spojené pipes). Vývoj komponentových systémů je náročnější (zvlášť problematické je správně určit rozhraní), ale umožňuje větší přizpůsobitelnost/znovupoužitelnost komponentů v budoucích projektech. E.g. extensions ve VSCode, component-based architekturu používá Jakarta Enterprise Edition, kde jednotlivé Java Beans jsou komponenty

**Service-oriented architecture** - hybrid mezi monolitem a microservices, service je samostatně nasaditelná služba zajišťující logickou jednotku funkcionality (e.g. objednávání), služby sdílí databázi. Je náročnější na vývoj, ale celkově je systém díky modularizaci lépe udržitelný a škálovatelný. Je potřeba řešit service discovery.

**Microservice architecture** - vysoká koheze, nízká provázanost služeb, systém je tvořen velkým množstvím malých služeb. Důležitá je rychlá komunikace mezi službami (gRPC). Služby nesdílí DB.

**Remote Procedure Call** umožňuje spuštění předem vystavené procedury mezi procesy (i na vzdáleném stroji) tak, jako bychom proceduru volali přímo v kódu. Implementace procedury může být v odlišném programovacím jazyce. Součástí je definice rozhraní, ze kterého je možné vygenerovat odpovídající volatelné funkce/struktury použité pro argumenty pro náš jazyk. De-facto standardem je dnes gRPC. Pro fullstack typescript aplikace je dnes populární používat tRPC. V PA053 se probírala CORBA (primární focus na Javu, ale podporovala i jiné jazyky, nejde jen o RPC, ale o architekturu pro komunikaci mezi objekty v distribuovaném prostředí), ale ta se v nových systémech prakticky nepoužívá kvůli složitosti/lepším alternativám, nahrazena jednodušším SOAP a REST, nebo RPC řešeními (gRPC, funguje na HTTP/2, zprávy binárně serializuje pomocí protocol bufferu).

Pro komunikaci se v distribuovaných systémech kromě RPC používají **message queues** a **event brokers**, kteří umožňují komunikaci typu publisher-subscriber, nebo zpracování jedním z množiny příjemců, a jsou schopny zprávy persistentně uchovávat (hodí se pro transakční zpracování, spolehlivost v případě výpadku). Obecně lze rozlišovat mezi zprávamy určenými do **queue** (klasická fronta, očekáváme zpracování jedním konzumentem) a **topic** (zpráva jde všem poslouchajícím). E.g. Apache Kafka, RabbitMQ.

Alternativně se může pro komunikaci v distribuovaném systému používat e.g. REST, nebo (pokud chceme low level kontrolu a výkon) přímá komunikace mezi sockety.

## Rozdíl mezi centralizovanou a distribuovanou architekturou systému, nevýhody obojího a jejich překonávání

Hlavním rozdílem je, že centralizovaná architektura shromažďuje data a logiku na jednom místě, distribuovaná architektura rozptyluje logiku do více samostatných komponentů (běžících třeba i na samostatných strojích), které spolu komunikují.

Nevýhody centralizované architektury
- neumožňuje horizontální škálování => škálujeme vertikálně
- selhání části znamená selhání celku => redundance, záložní servery
- nízká flexibilita, vysoká provázanost => důraz na kvalitu kódu

Nevýhody distribuované architektury
- komplexita celkového systému, náročnější správa
- vyžadují více/složitější komunikaci, složitější synchronizace, náchylnost na latenci => použití message queues, gRPC, eventual consistency


Oproti centralizované architektuře distribuované systémy
- nebývají požadavky/transakce ACID, ale BASE
    - **BAsically available** - nefunkčnost části nezpůsobí nefunkčnost celku, zbytek funguje i v případě nefunkční části systému. e.g. na netflixu nemusí fungovat služba hledání, ale vše ostatní běží v cajku. Na každý dotaz dostaneme nějakou odpověď.
    - **Soft state** - změny v systému mohou nastávat i když nepřichází žádné dotazy - systém takto propaguje data, aby dosáhl konzistence
    - **Eventually consistent** - data nemusí být konzistentní okamžitě po získání odpovědi na dotaz, ale až po nějaké chvíli
- selhání (pád) části systému neznamená pád celku
- jsou flexibilnější na modifikace díky nízké provázanosti

## Replikace, sdílení dat
TODO

## Architektura orientovaná na služby (SOA)
TODO

## Webové služby

Komponenty umožňující komunikaci a interakci prostřednictvím standardizovaných protokolů a formátů. Jsou založeny na Service Oriented Architecture. Web services poskytují abstrakci funkcionalitě služby skrz webové API. Skrz definiční jazyk je formálně popsáno schéma/rozhraní dané služby a je možné generování klientského kódu pro různé programovací jazyky s cílem usnadnit použití webové služby. Schéma může být zároveň generováno přímo ze zdrojového kódu prostřednictvím anotací. 

Dříve se používaly web services založené na SOAP (simple object access protocol) a XML, definované pomocí Web Service Definition Language (WSDL).

Aktuálně se pro tyto účely spíše používá REST (representational state transfer) a JSON, definované pomocí OpenAPI Specification, případně GraphQL (a JSON) se svým GraphQL Schema a dotazovacím jazykem. GraphQL používá jeden entrypoint (+1 playground) a umožňuje přesně specifikovat kýžená data (až na úroveň polí) a řešit tak problém s overfetching (1 dotaz obsahuje zbytečná data) a underfetching (v dotazu nemáme dostatek dat, takže děláme vícero různých dotazů).



## Příklady existujících technologií a jejich využití.
TODO

## Notes

**Middleware** - vrstva softwaru poskytující rozhraní pro interakci s různými službami/systémy, abstrakce k často používané funkcionalitě, případně vrstva propojující existující systémy.
- e.g. CORBA, Web Services, REST, message queue systémy, event brokeři.