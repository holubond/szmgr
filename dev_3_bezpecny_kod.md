# Bezpečný kód.

> Bezpečný kód. Metody autentizace a řízení přístupu. Biometrické metody autentizace, jejich dopady a problémy. Elektronický podpis a jeho použití. Autentizace strojů a aplikací. Zásady a principy bezpečného kódu. Typické bezpečnostní chyby na úrovni kódu, souběžnost, ošetření vstupů. Detekce bezpečnostních zranitelností, penetrační testování. Příklady z praxe pro vše výše uvedené. ([PV157](https://is.muni.cz/auth/el/fi/podzim2021/PV157/um/), PA193, PV276)

## Metody autentizace a řízení přístupu.

**Klíčové pojmy**
- **Autentizace/verifikace identity** - verifikace/ověření identity - kombinace identity (e.g. jména) a důkazu (e.g. hesla)
- **Identifikace** - rozpoznání - rozpoznání entity v dané množině entit, subjekt nepředkládá identitu, ale systém se mu ji snaží přiřadit z databáze známých identit, e.g. otisk prstu na vstupních dveřích
- **Autorizace** - udělení práv k vykonání určitých akcí (e.g. autorizace terminálu k platbě vložením karty a pinu, zároveň se uživatel autentizoval pinem vůči kartě)

**Metody autentizace**
- na základě znalosti (pin/heslo)
- na základě vlastnictví fyzického tokenu (klíč/čipová karta, řeší se i cena padělání, obvykle velká za kus, ale funguje economies of scale, fungují legislativní postihy)
- na základě biometrie (otisk prstu, který nikdo jiný nemá)
- na základě fyzické/virtuální lokace (rychlá změna fyzické lokace může být varovný signál, přihlášení z určitého počítače s certifikátem může stačit k autentizaci)

Autentizaci můžeme vyžadovat jednostranně (e.g. webový server, potřebujeme vědět, že se se nejedná o někoho jiného. Serveru je jedno, komu odpovídá, pokud jen vrací veřejnou webovou stránku), nebo oboustraně (pro důležité akce potřebuje server ujištění, že akce provádí opravdu uživatel).

Autentizace může proběhnout na základě výzvy, nebo klidně z iniciativy subjektu.

**Zero-knowledge protokoly** - umožňují demonstraci znalosti tajemství, aniž bychom odhalili jakoukoliv informaci vedoucí k získání tajemství
- vlastnosti 
    - **úplnost/completeness** - poctivci vždy dosáhnou úspěšného výsledku
    - **korektnost/soundness** - mizivá šance na úspěch nepoctivce

### Autentizace tajnými informacemi

**Tajné informace** 
- cílem je autentizace uživatele
- v ideálním případě autorizovaným uživatelům nekomplikují fungování, ale co nejvíce znesnadňují zneužití utočníky
- řeší se
    - ukládání
    - bezpečnost (kvalita/složitost) vs zapamatovatelnost
    - jak bude probíhat kontrola
    - postup v případě kompromitace (změna hesla)
- je nutné dodržet
    - informace by měla být opravdu tajná, neměla by být odvoditelná (e.g. ne rodné jméno matky)
    - informace by měla být vybrána z velkého prostoru hodnot (e.g. vynucení čísel, speciálních symbolů, mixed case...)
    - informaci při autentizaci nepředáváme v čisté podobě (lze odposlechnout). Máme možnosti:
        - šifrujeme, v ideálním případě je šifrovaná celá komunikace
        - posíláme jen haš. Ten lze sice použít pro podvodnou autentizaci, ale nelze z něj odvodit heslo
        - používáme challenge-response, pomocí kterého se tajné informace nešíří
- může jít o pin/heslo/passphrase/identifikaci obrazové informace/spojení bodů v určitém pořadí...

**Hesla**
- možnosti
    - skupinová (více uživatelů sdílí heslo) - mizivá bezpečnost
    - unikátní pro osobu, pomocí hesla se zároveň uživatel identifikuje
    - kombinace s uživatelským jménem
    - jednorázová - jdou odděleným kanálem, obvykle součástí vícefaktorové autentizace, prokazujeme vlastnictví dalšího tokenu
- ukládají se [hašovaná](./5_databaze.md#hašování) (pokud nepotřebujeme získat původní heslo), ideálně včetně soli a pepře, nebo šifrovaná (problém je, že teď musíme chránit místo hesla šifrovací klíč), nikdy ne v plaintextu
    - **salt** - náhodně vygenerovaná data, která se ukládají zároveň s hašem a při hašování se přidávají k heslu, efektivně prodlužuje délku hesla a znemožní detekci stejných hesel dle shody hašů
    - **pepper** - data, která se při hešování přidávájí ke vstupu, jsou však utajená (neukládáme je vedle haše)
- nucená expirace hesla může mít za následek používání obecně slabších hesel
- dobrý kompromis mezi bezpečností a zapamatovatelností jsou hesla založená na frázích (se zakomponováním speciálních znaků/číslic), nebo lokálním důvěryhodným správcem hesel 

**Útoky na hesla**
- cílený (snažíme se zjistit heslo konkrétního uživatele) vs plošný (snažíme se zjistit heslo kohokoliv ze skupiny uživatelů)
- online (lze omezit počet pokusů) vs offline (kdy jsme se zmocnili souboru s haši/šifrovanými daty)
- odpozorování (vizuálně, phishing, nebo zachycení komunikace, sniffing)
- slovníkový (máme slovník často používaných hesel)
- hrubou silou
- zneužití mechanismu na obnovu hesla (mnohdy snadnější, než prolomení hesla)
- analýza zdrojového kódu nebo souborů verzovacího systému (často tam hesla nechávají vývojáři)
- použití iniciálního hesla z manuálu (e.g. často u routerů)
- krádež databáze/databáze, získání obsahu paměti/cache
- použití rainbow tables (předpočítané hodnoty a jejich haše), nástroje na prolomení heslovaných souboru (john the ripper)

**Jednorázová hesla**
- buď náhodně generovaná a zaslaná separátním (multifactor authentication)
- nebo **(Lamportův) řetězec haší**
    - na začátku si uživatel lokálně uloží heslo, který 1000x prožene hash funkcí, výsledek dá serveru
    - při každé autentizaci uživatel pošle předchozí hash (číslo 999, 998...). Server prožene tento předaný hash 1x hash funkcí a porovná s posledním uloženým hashem. Pokud výsledek sedí, uloží si předaný hash jako poslední uložený a považuje uživatele za autentizovaného. Útočník nemůže použít odposlechnutý hash, protože potřebuje předchozí hodnotu (a originál nezná).
- nebo přístroj (autentizační kalkulátor) generující náhodná hesla na základě času, princip generování je znám službě, která autentizuje. Je nutná synchronizace hodin/povolení několika hesel kolem předpokládaného okamžiku. (to už je spíš autentizace tokenem). Případně má autentizační kalkulátor klávesnici a používáme ho pro challenge-response

**PINy**
- obvykle velmi krátké -> omezujeme počet pokusů

#### Autentizace pomocí symetrické kryptografie

Jak ověřit, že komunikuju s tím, kdo se mnou sdílí klíč?
- pošlu náhodné číslo (a třeba sekvenční číslo/timestamp pro prevenci útoku), které mi má druhý vrátit zašifrované
- lze i oboustraně, druhý komunikující ve své odpovědi zahrne zašifrovaný challenge pro mě

Alternativně lze provést výpočetně jednodušší variantu - nepředáváme si šifrovaná data, ale hashe (pro jejichž vytvoření byl zahrnut klíč), jinak je to principiálně stejné. 
- pokud chceme oboustrannou autentizaci, musí mi druhý poslat challenge i v nehašované podobě, ať ho můžu použít taky

Do zpráv je možné přidat identitu challengera, abychom předešli man in the middle útokům.

`r` je náhodné číslo, `E` znamená encrypted, `h` znamená hashed, `K` je sdílený klíč

| s šifrováním | s hešováním |
|---|---|
|![](img/20230613181843.png)|![](img/20230613181909.png)|

#### Autentizace pomocí asymetrické kryptografie

Pomocí šifrování: Zašifruju zprávu veřejným klíčem. Jestli jsi majitel soukromého klíče, dešifruj a pošli výsledek zpět.

Alternativně lze pomocí podpisu: Tady máš náhodné číslo, podepiš mi ho (hash a šifrování soukromým klíčem). Server do dat přihodí svoje náhodné číslo (aby předešel zneužití, kdy útočník chce od serveru získat svoje podepsaná data), celé to podepíše a vrátí.

Lze samozřejmě provádět oboustranně.

Do zpráv je možné přidat identitu challengera, abychom předešli man in the middle útokům.

### Protokoly pro správu klíčů

**Aktualizace klíče**
- Nový klíč lze předat zašifrovaný. Lze opatřit časovým razítkem, nebo spojit s náhodným číslem od parťáka, abychom předešli útoku přehráním.
- Tady máš náhodné číslo. Náš nový klíč je toto číslo zašifrované starým klíčem (je fajn si poslat kontrolní zprávu s informací šifrovanou novým klíčem)

**Ustanovení klíče bez předchozího sdíleného tajemství**
- **Shamirův protokol**
    - Vyžaduje komutativní šifru, kde platí `E_a( E_b( X ) ) = E_b( E_a( X ) )`
        1. vygeneruju nový klíč X. Ten zašifruju svým klíčem a. `E_a(X)` pošlu kámošovi
        2. kámoš výsledek zašifruje svým klíčem b a pošle mi `E_b( E_a( X ) )`
        3. odstraním svůj klíč a, pošlu kámošovi výsledek `E_b( X )`. Kámoš odstraní klíč b a zjistí nový klíč X, kterým můžeme šifrovat
- **Diffie Hellman protokol**
    - spolu s kámošem se dohodneme na společném základu (malé prvočíslo `g` a velké číslo `n`) a každý přidáme svou tajnou ingredienci, já `a`, kámoš `b`, `1 <= a, b <= n` (`x_a = g^a mod n` a `x_b = g^b mod n`)
    - výsledek si vyměníme
    - přidáme opět svou tajnou ingredienci a využijeme ekvivalence a kongruence modulo n (`(g^a mod n)^b mod n = g^ab mod n = (g^b mod n)^a mod n`)
    - výsledek `g^ab mod n` používáme pro šifrování

### Řízení přístupu

Bezpečnostní mechanismus pro umožňení uživatelům vykonávat v systému nějaké akce.

Pojmy
- vlastník dat - zodpovědný za určitá data
- správce dat - zodpovědný za bezpečnost určitých dat
- uživatel - může vykonávat operace s daty dle svých přístupových práv

Politiky řízení přístupu
- volitelný přístup/decentralizovaná správa řízení - vlastník dat/objektu rozhoduje (malá režie, o správu se starají vlastníci/správci, špatné vynucování/koordinace celosystémových pravidel, možnost problému, kdy někdo v souborovém systému tajný soubor zkopíruje a zveřejní ho omylem všem)
- povinný přístup/centralizovaná správa řízení - o přístupu rozhoduje systémová politika
- povinný a volitelný přístup lze kombinovat, systém je pak flexibilnější, ale stále zaručuje bezpečnost u kritických objektů

Základní typy práv (obvykle specifikovatelná pro každou jednotku dat, e.g. soubory, nebo třeba buňky v tabulkách)
- read (může i kopírovat a zrádný člen skupiny tak udělat veřejnou kopii)
- write (modifikace)
- execute (spouštění programu)
- pokročilejší příznaky mohou být třeba append only soubor

Práva mohou být specifikována v matici (jedna osa soubory, druhá uživatelé, uprostřed práva), ale může být nepřehledná -> práva lze ukládat v metadatech daného objektu - **Access Control List (ACL)** (ale těžko se zas hledá, k čemu všemu má daný uživatel přístup).

Přístup k objektu
- může být omezený časem, místem, intervalem hodnot, typem služby... 
- na základě identity uživatele, jeho role/skupiny, nebo třeba jen hesla

V unix systémech bývá obvykle nějaký superadmin/root, který má přístup ke všemu. Dobrou politikou je snaha o omezení práv tohoto uživatele a vytvoření skupin pro příslušné skupiny dat/objektů - pokud se hacker dostane k rootu, je vše v pytli. Moderní unixové systémy (obvykle komerční verze) nabízí jemnější granularitu nad skupinami, právy uživatelů...

Good practices řízení přístupu
- separace oprávnění - potvrzení důležité operace vícero aktéry
- omezení práv jednotlivce - každý má přístup jen k tomu, co nutně potřebuje = _princip nejnižších privilegií_
- defaultní akce je zamítnutí - práva přidělujeme explicitním povolením, abychom nepodolili něco jen proto, že jsme zapomněli vzít v potaz určitý scénář, defaultně zamítneme vša a používáme whitelisty (e.g. firewall)

**Multi-level systems (MLS)**
- do systému mají přístup všichni uživatelé, data jim zobrazujeme/umožňujeme používat objekty dle jejich úrovně (nižším úrovním skrýváme to, co mohou vidět vyšší úrovně)
- role jsou hierarchické
- problémem může být **skrytý kanál**
    - mechanismus, který není určen ke komunikaci je využit pro získání informací
    - e.g. zátěž procesoru, zaplnění disku, čas posledního přístupu k souboru
    - e.g. nemožnost vytvořit soubor (indikuje, že soubor se stejným jménem už existuje, jen je nám skrytý)/vložit hodnotu do databáze 
        => máme automatizované schéma pro pojmenovávání

**Role-based access control (RBAC)**
- uživatelům přiřazujeme role (i vícero)
- na role navazujeme oprávnění
- e.g. role v databázích

## Biometrické metody autentizace, jejich dopady a problémy.

> Automatizované metody identifikace nebo ověření identity na základě měřitelných fyziologických nebo behaviorálních (založených na chování) vlastností člověka

Biometrická data je nejdříve třeba nasnímat (včetně kontroly kvality, extrakce charakteristik) a uložit, potom je možné je používat k autentizaci/identifikaci (pomocí srovnání charakteristik).

Na rozdíl od ostatních metod autentizace **musíme řešit variabilitu** uložených dat a nasnímaného vzorku, data nejsou nikdy zcela identická.
- velmi často závisí na měřících podmínkách, na samotném zařízení, stavu měřeného, schopnosti/motivace měřeného provést si měření správně...
- 100% shoda může znamenat problém - útočník se dostal k uloženým datům
- řeší se balanc mezi **false acceptance** (bezpečnostní problém) a **false rejection** (nepohodlí uživatelů)
![](img/20230613154105.png)

Biometriky jsou vhodné jako doplňkové metody, pro přístup k tajnému klíči, autentizaci uživatele (ne dat/počítače)... ne pro použití jako samotný klíč

**Problémy biometrické autentizace**
- nikdy nejsou zcela bezchybné
- Failure to enroll - není možné získat biometrickou charakteristiku při registraci (e.g. někdo nemá prst, který chceme snímat)
- Failure to acquire/capture - není možné získat charakteristiku při autentizaci
- False positive identification - přijali jsme chybně (bezpečnostní riziko)
- False negative identification - zamítli jsme chybně (naštvanější uživatelé)
- fenotypické charakteristiky (e.g. geometrie ruky) se mohou v čase měnit
- genotypické charakteristiky (e.g. dna) je zase obtížné rychle vyhodnotit
- měření není dokonalé (prostředí, jiný měřák, nezkušenost/nespolupráce měřeného)
- měření může být nepříjemné
- metody mají různé charakteristiky rychlosti, spolehlivosti, příjemnosti, výpočetní náročnosti, míru vlivu prostředí...
- musíme řešit živost - jde skutečně o člověka, nebo třeba o umělý prst? je daný člověk na živu?
- jedna charakteristika může být použita ve více systémech, zveřejnění nesmí ohrozit soukromí
- biometriky nejsou tajné
- ochrana soukromí, legislativní omezení
- kvůli nepřesnostem/možným změnám/netajnosti není vhodné z biometrik generovat kvalitní kryptografické klíče

**Kontinuální autentizace** - subjekt kontinuálně sledujeme a snažíme se detekovat možné odchylky v chování, které by naznačovaly, že se jedná o útočníka, e.g. dynamika psaní na klávesnici

Forenzní systémy pro biometrickou autentizaci jsou přesnější, spolehlivější, dražší, mohou být pomalejší, vyžadují odborníky.

e.g. otisk prstu (tam sledujeme markanty), geometrie ruky, sken duhovky, sken sítnice, rozpoznání obličeje, rozpoznání hlasu, rozpoznání stylu interakce se zařízením (e.g. tempo psaní na klávesnici, dynamika podpisu), dna

## Elektronický podpis a jeho použití.

- zajišťuje **autentizaci** (že zpráva pochází od daného autora) a **integritu** (že zprávu nikdo nepozměnil) **podepisovaných dat**
- musí být ověřitelný třetí stranou
- nezajišťuje důvěrnost dat
- měl by obsahovat i datum/čas
- algoritmy **RSA, DSA**

Některé algoritmy umožňují obnovu dat na základě podpisu (v podpisu jsou nějakým způsobem data obsažena).

**Průběh podepisování**
1. vytvořím asymetrické klíče (veřejný, soukromý), veřejný klíč zaregistruju/vystavím, aby mohl být později použit k ověření
2. (pro každý podepisovaný dokument) - vytvořím hash (e.g. pomocí SHA-2) podepisovaného dokumentu, který šifruju soukromým klíčem (asymetrické algoritmy bývají pomalé, takže nešifruju celý dokument)
3. podepsané dokumenty je možné ověřit pomocí mého vystaveného veřejného klíče 

**Certifikát**
- spojuje veřejný klíč a informace o subjektu, který veřejný klíč poskytuje => potvrzení identity
- společně je s informacemi o certifikační autoritě podepsán pomocí soukromého klíče **certifikační autority**
    - vytváří se řetězec důvěry - pokud věříš autoritě, můžeš věřit i mně
- certifikační autorita 
    - zajišťuje, že daný subjekt opravdu vlastní soukromý klíč
    - autentizuje subjekt vystavující veřejný klíč (zajišťuje, že daný subjekt je tím, co tvrdí) 
    - potvrzuje platnost veřejného klíče daného subjektu svým podpisem
- bývá časově omezen

**Použití podpisu**
- autentizace dat (podpis zprávy, který si mohou ostatní ověřit)
- autentizace počítačů/tokenů díky mechanismu **výzva-odpověď (challenge-response)**
    - jestli jsi opravdu vlastníkem soukromého klíče, tak podepiš tato vzorová data
- autentizace osob pomocí schopnosti spustit aplikaci na počítači/tokenu
- digitální podpis neprovádí člověk, ale počítač
- pokud potřebujeme šifrovat, nejprve podepisujeme, potom šifrujeme

**Soukromý klíč je potřeba chránit**, v případě vyzrazení se za nás může někdo vydávat.
- soukromý klíč bývá ideálně šifrován/blokován (e.g. vyžaduje zadání přístupového hesla/pinu)

**Veřejný klíč musí mít zajištěnou integritu** - pokud bychom používali nesprávný veřejný klíč, mohli bychom dojít k nesprávným výsledkům
    => vystavuje se certifikát, který spojuje klíč s naší identitou pomocí podpisu certifikační autoritou

**Infrastruktura pro správu veřejných klíčů (PKI)**
- **Certifikační autorita** - poskytuje certifikační služby, vydává certifikáty a případně je zneplatňuje
- **Registrační autorita** - registruje žadatele o vydání certifikátu, prověřuje jejich identitu (může být zároveň CA)
- **Adresářová služba** - uchovává a distribuuje platné klíče (a seznam zneplatněných certifikátů)
- certifikační autoritu obvykle certifikuje nadřazená certifikační autorita, čímž se tvoří řetězec důvěry

**Vystavení certifikátu**
- generování klíčových dat (e.g. key-pair pro asymetrickou kryptografii)
- doložení a ověření identifikačních informací (e.g. pro web předáme údaje o identitě a instalujeme *Certbot*, nebo nahrajeme určitý soubor, abychom dokázali, že máme nad serverem kontrolu)
- vydání certifikátu žadateli (včetně zveřejnění v adresářové službě) 

## Autentizace strojů a aplikací.

**Autentizace počítačů**
- na základě adresy (IP, MAC fyzická adresa)
    - MAC fyzická adresa - svázáním portu switch přepínače s určitou MAC adresou, nebo svázání IP adresy s MAC adresou
    - IP - řízení přístupu k webovým službám na základě IP adresy
    - problém může být, že MAC i IP adresy lze změnit, nejsou tajné, je možné uvést cizí IP adresu
- na základě tajné informace (symetrická/asymetrická kryptografie)
    - heslo/tajný symetrický klíč/soukromý asymetrický klíč
    - vhodné ukládat zašifrované a při startu zadat heslo (pak to budeme držet v paměti), nebo použít e.g. HashiCorp Vault (secret manager)

**TLS/SSL** - protokol vyšší úrovně
- SSL je předchůdce TLS
- autentizuje strany pomocí certifikátu a challenge-response (defaultně povinná pro server, volitelná pro klienta)
- zajišťuje integritu a autenticitu dat (pomocí Message Authentication Code, MAC, k datům přidáme tajný klíč a celé to hašujeme (na rozdíl od podpisu neprovádíme šifrování heše dat tajným klíčem))
- zajišťuje důvěrnost
- Nejprve proběhne iniciální handshake (autentizace pomocí asymetrické kryptografie). Následně se stanoví symetrický kryptografický klíč, kterým je šifrována celá komunikace.
- je mezi TCP a aplikací, TLS nevidí do přenášených dat

**IPSec** - na síťové vrstvě, přidán do IPv4, v IPv6 už je defaultně
- pro každý IP datagram
    - zajišťuje autentizaci odesilatele (IP hlavičky (vyjma měněných dat, e.g. TTL) a data, přidá tajný klíč, hash uloží do autentizační hlavičky)
    - zajišťuje integritu dat (nezměněná data, ^^^)
    - zajišťuje důvěrnost dat (symetrický šifrovací klíč známý objema stranám, data jsou šifrována)
    - zajišťuje ochranu před útokem přehráním (MAC v kombinaci se sekvenčním číslem)
- umožňuje transportní (end to end, nepodporuje NAT), nebo tunelovací režim (celý datagram beru jako data, přilepím tomu novou IP hlavičku)

**Secure Shell Host (SSH)**
- slouží k vzdálenému přihlášení k serveru
- oproti telnetu je komunikace šifrovaná (symetrickou šifrou, klíč se stanoví po handshake)
- probíhá autentizace serveru i klienta (určitě jste si někdy generovali key pair pomocí `ssh-keygen`, tak to bylo ono)
- používá se pro to třeba RSA, DSA (asymetrická kryptografie)

**Security Assertion Markup Language (SAML)**
- standard pro popis a výměnu autentizačních dat
- založený na XML, používaný pro webové aplikace
- umožňuje oddělení poskytovatele identity a poskytovatele služeb, jinými slovy umožňuje Single Sign-On (SSO)
- SAML token obsahuje
    - subject (kdo je držitel tokenu, e.g. user id)
    - auth statement (způsob & čas provedené autentizace)
    - příslušnost ke skupinám, rolím, povolené operace
    ...

## Zásady a principy bezpečného kódu.

**Defensive programming** 
- kód by měl být psán tak, aby byl systém připraven pracovat v prostředí, kde mohou nastávat (nechtěné) chyby
    - důraz na ověřování vstupních dat
    - ošetření i těch situací, které *přece nemůžou nastat*
    - příprava systému na jednoduché testování (dekompozice, závislosti na abstrakcích) a diagnostiku chyb (logování, explicitní ošetření chyb)
    - logování událostí (abychom dokázali detekovat, co se v systému dělo)

Pro zajištění bezpečnosti kódy lze postupovat různými způsoby, přístupy se nevylučují (víceúrovňová ochrana rozhodně není na škodu)
- použití bezpečnějšího jazyka, který některé chyby neumožňuje, nebo je aspoň dělá těžší na provedení e.g. Rust
    - případně použití striktnějšího modu překladače
- spouštění aplikace v sandbox prostředí, třeba kontejneru, aby případný útočník nezískal kontrolu nad celým strojem
- důkladné testování statická a dynamická analýza, code reviews...
- pro kritické věci (kryptografie) je dobré použít osvědčené knihovny
- závislosti (knihovny) je dobré pravidelně sledovat ohledně výskytu bezpečnostních slabin (e.g. automatizovaně pomocí dependabot)
- použití bezpečných verzí funkcí (u C/C++ třeba strncpy místo strcpy), nepoužívání funkcí označených *obsolete*
- kontinuální integrace - automatizované spouštění testů, statické (případně i dynamické) analýzy
- je klíčové dobře znát použitý jazyk a jeho typické slabiny
- pokud si můžeme vybrat, je lepší používat whitelisting než blacklisting (deny by default)

Při vývoji kódu je dobré zajistit, aby byly chybové stavy nereprezentovatelné (třeba pomocí builder patternu a různých builder tříd).

## Typické bezpečnostní chyby na úrovni kódu, souběžnost, ošetření vstupů.

Seznamy běžných chyb
- Common Weakness Enumeration (CWE) - obecně časté bezpečnostní chyby v programovacích jazycích
- OWASP top 10 - nejčastější bezpečnostní díry ve webových aplikacích (ale OWASP mají i jiné zajímavé projekty zaměřené na bezpečnost)

Typické chyby
- **Injection** - vložení vlastních instrukcí do dat, která jsou bez dostatečné kontroly vyhodnocována interpretem
    - e.g. SQL injection
- absence logování/monitirování
- **Race condition** - simultánní zápisy (nebo zápis a čtení) do sdílené paměti (ze stejného paměťového místa, ale i třeba ze dvou různých logicky závislých míst) - řešením je sekvenční zpracování nebo zamykání
- **Buffer overflow** - v paměti máme pole a za ním data. Pokud provedeme zápis do pole a zapisovaná data jsou delší než pole (a neohlídáme si délku), mohou nám zapisovaná data přepsat i data za polem. Ovlivňuje hlavně C/C++.
- **Buffer overread** - jako buffer overflow, ale se čtením - jsme schopní číst i data za polem
- použití neinicializované paměti (po malloc), nebo uvolněného ukazatele (po free)
- **Stack exhaustion** - vyplýtvání místa na zásobníku, typicky kvůli velké rekurzi
- **Heap exhaustion** - vyplýtvání místa na haldě, není možné alokovat další paměť (může být způsobeno memory leaky, nebo velkou paměťovou náročností programu)
- **Type overflow** - přetečení hodnoty. E.g. int overflow; způsobeno `i64::MAX + 1`, výsledek je 0
    => kontrola hodnot, nebo speciální operace (e.g. hodí výjimku při přetečení)
- **Floating point reprezentace** - `0.1 + 0.2 == 0.3000000001` => použít decimal/bigdecimal, což jsou inty s fixed-point desetinnou čárkou
- **Off-by-one error**

**Souběžnost** a.k.a. race condition
- špatné načasování operací (nebo jejich pořadí) způsobí nečekané stavy systému
- e.g. máme současně běžící programy, každý chce přečíst hodnotu ze sdílené paměti a zvýšit ji o 1. 
    ```
    Procesy A a B chtějí inkremetovat sdílený čítač, každý o 1
    A čte hodnotu 5
    B čte hodnotu 5
    A inkrementuje načtenou hodnotu 5+1=6
    B inkrementuje načtenou hodnotu 5+1=6
    A zapíše 6
    B zapíše 6
    Oba procesy inkrementovaly čítač, který se reálně zvedl pouze o 1
    ```
- řešením je vyznačení problematické části jako kritické sekce. Pro kritickou sekci se musí vynutit přístupová pravidla písařů a čtenářů:
    - pokud existuje písař, musí být jediný a nesmí existovat žádní čtenáři
    - pokud neexistuje písař, může existovat libovolný počet čtenářů
- striktnějším řešením je uzamčení celé kritické sekce, aby k ním měl přístup vždy jen 1 proces
- pokud máme vícero kritických sekcí a používáme zámky, je třeba dávat pozor na uváznutí (deadlock). Ten nastane, když každý proces z množiny procesů vlastní nějaký zdroj a pro dokončení své práce (a uvolnění vlastněného zdroje) vyžaduje zdroj vlastněný jiným procesem. Všichni tak čekají

Některé chyby bývají specifické pro určité programovací jazyky (buffer overflow pro C/C++)

*Zero-day exploit* - využití bezpečnostní chyby, která ještě není obecně známá/neexistuje proti ní obrana

Zdrojový kód se může od výsledné binárky značně lišit (optimalizace, debug-only sekce kódu). Při vývoji se hodí mít dodatečné informace pro debugging (umožňující třeba detailní stack trace). V release verzi však tyto informace mohou pomoct útočníkovi.

Pro explicitní řízení přechodů mezi stavy programu lze použít **automata-based modelling**.
- Stavy a přechody programu modelujeme jako stavový automat; pomocí dat reprezentujeme stav a na základě něj můžeme explicitně definovat validní přechody (e.g. switch/match statement, případně transformace objektů (pokročilejší builder pattern)). Minimalizujeme tak místa, ve kterých se mění stav, díky čemuž je kód přehlednější a bezpečnější.

Pro **ošetřování vstupů** je vhodný použít fail-fast přístup. Jakmile zjistíme, že pracujeme s chybnými daty, měli bychom přerušit standardní průchod funkcí a zpracovat chybu. Koncovému uživateli sdělujeme jen nutné minimum nutné pro identifikaci důvodu chyby (nadbytečné informace, jako třeba názvy tříd, by mu mohly odhalit interní strukturu aplikace, což by mohlo být bezpečnostní riziko).
- pro jednodušší ověřování je vhodné omezit počet validních vstupů (e.g. jen čísla)
- vstupní data mapujeme na interní data (e.g. používáme enumy)
- maximální délku vstupu je důležité brát v potaz zvlášť u zpracování souborů (navíc je dobré použít bufferované čtení a zpracovávat soubor po řádcích)
- problém může dělat třeba UTF-8 řetězce, kde jeden znak může mít různou délku
- pro ošetření vstupů je navíc fajn používat validační knihovny (e.g. zod pro js, clap pro rust) 
- Pro průzkum co všechno v našem systému závisí na uživatelském vstupu je možné použít **taint analýzu**
- Jednotky systému mohou používat [kontrakty](./dev_1_analyza_a_navrh.md#rozhraní-komponent-kontrakty-na-úrovni-rozhraní-ocl) (preconditions, postconditions, invariants) jako pojistku v případě nedostatečného ošetření vstupů
- Pro kontrolu dostatečného ošetření vstupů je možné použít **fuzzing** (viz další sekce)
- Pro jednoduché zpracování sekvence vstupů (příkazů) je vhodné použít **automata-based modelling**
    - v ideálním případě se chceme nutnosti udržovat stav mezi příkazy vyhnout, bezstavová komunikace je méně náchylná na chyby a je možné systém jednoduššeji škálovat
- důležité je samozřejmě nikdy nevěřit uživatelským vstupům

## Detekce bezpečnostních zranitelností, penetrační testování.

Pro detekci (nejen) bezpečnostních zranitelností je možné použít vícero přístupů (nejsou exkluzivní)
- statická analýza
- dynamická analýza
- fuzzing
- penetrační testování
- security review

Pro detekci buffer overflow lze pomocí překladače použít tzv. canary - data za každým polem. Pokud dojde k přetečení pole, bude canary přepsán, což je signál problému. Problém přetrvá, pokud útočník zná hodnotu canary. Alternativně (režijně náročnější) lze kontrolovat délku pole a zapisovaných dat.

Prevencí změny návratové hodnoty funkce útočníkem (důsledek buffer overflow) je randomizace adres funkcí v kódu.

Prevencí code injection může být data execution prevention - paměť dělíme na datovou a spustitelnou, není možné spouštět kód z datové části. SQL injection (zprávy jsou obvykle interpretované) se řeší pomocí prepared statements, čímž efektivně dělíme části příkazu na datovou a příkazovou.

**Statická analýza**
- analýza kódu programu, aniž by byl program spuštěn
- lze analyzovat zdrojový kód (jednodušší, je k tomu víc kontextu), ale i binárku
- lze aplikovat i na nedokončený kód
- lze využít i pro vynucení jednotného stylu kódu
- důležitou (ale špatně automatizovatelnou a škálovatelnou) variantou je code review
- statickou analýzu provádí i samotný překladač (type checking) - ten si ale nemůže dovolit považovat za chybu něco, co chybou ve skutečnosti není
- lze využít automatizované nástroje, kterým stačí zdrojový kód (e.g. cargo check, cargo clippy, pro vícero jazyků třeba sonarqube)
- běžně bývá součástí CI - důraz na rychlost
- snadno odhalí i časté chyby jako ponechání hardcoded api klíče

**Dynamická analýza**
- program je spuštěn, poskytujeme různé vstupy
- je možné použít virtualizovaný procesor/interpret, můžeme sledovat paměť
- lze vynutit omezené prostředí (málo paměti, omezená práva, omezení v souborovém systému)
- lze sledovat data a jejich změny v programu
- lze vložit logovací instrukce
- e.g. Valgrind (nahrazuje standardní alokátor a poskytuje vlastní, což mu umožňuje sledovat dění v paměti), miri (interpret pro rust, používá se pro detekci undefined behavior při práci s unsafe kódem)
- **Fuzzing** - program spouštíme s *náhodnými* generovanými vstupy a sledujeme výstupy
    - vhodné pro blackbox
    - vstupy mohou být zcela náhodné, v praxi chceme poskytnout několik vhodných vstupů (jako základ), které fuzzer modifikuje různými způsoby (zcela náhodně, nebo pomocí nějaké inteligentní strategie)
    - klíčové je, aby vstupů bylo velké množství, proces je možné snadno automatizovat a opakovat
    - během fuzzingu sledujeme chování aplikace (zamrzla? běží v cajku?)
    - po skončení fuzzingu máme množinu problematických vstupů, jejichž ošetřování se můžeme věnovat
    - obvykle se takto najdou jen poměrně jednoduché chyby, nebo chyby validace (ale záleží na programu)
    - některé fuzzery generují vstupy ze zdrojového kódu, cílem je vysoká code coverage (e.g. American Fuzzy Lop)
    - e.g. MiniFuzz (input file fuzzer)
- **Taint analýza** - data, které nějakým způsobem závisí na nedůvěrném vstupu, jsou označena. Pokud se označení dostane i ke kritickým částem kódu, vyskočí nám upozornění

**Security review**
- provádí se top-down, bottom up (vhodnější při nejasné architektuře, ale náročnější na provedení) či hybridně
- začíná u architektury a dokumentace, snaha o detekci návrhových chyb, stanovují se možná rizika a zranitelnosti
- u kódu se sleduje, jak dobře implementuje architekturu (často existují rozdíly), hledají se možné zranitelnosti v high-level logice, pak i v samotném kódu
- hodnotí se dodržování bezpečnostních standardů
- prakticky se testuje zabezpečení (penetrační testování, DDoS útoky, statická analýza)
- sleduje se vliv nedůvěryhodných dat (taint analýza)
- lze analyzovat přímo kód (řádek po řádku, nebo podle pořadí volaných funkcí), případně si můžeme udělat seznam potenciálních slabin a na ty se zaměřit
- analyzuje se kontrola přístupu a správa oprávnění
- hodnotí se bezpečnostní opatření, monitoring
- hodnotí se ochrana dat, správa klíčů, šifrování
- výsledkem je zpráva popisující současný stav a doporučení do budoucna

**Penetrační testování**
- obecné penetrační testování je náročné, obvykle je dobré si vytipovat/doporučit slabá místa, zaměřit útoky jen na některé části systému
- interně - týmem v rámci organizace
    - lze využít znalosti zdrojového kódu
    - vyžaduje udržování specializovaného odborného týmu
- externě - jinou, specializovanou společností 
    - nabízí pohled z venku, který může brát v potaz problémy, které sami nevidíme
    - může být součástí compliance
    - poskytovat kód externě může být problém
    - může být fajn použít víc společností, aby se vzájemně vychytala slabá místa
- lze provádět whitebox/blackbox (graybox je, když dáme k dispozici dokumentaci, ale ne zdroják)
- dobrou praxí je mít systém odměn za hlášení bezpečnostních chyb (bug bounty)

## Notes

**Brainstorm**
- používání neaktuálních/nepodporovaných verzí systémů
- odesílání uživateli zbytečně detailní informace, které mohou být zneužity
    - e.g. verze používaného dobře známého systému (třeba nginx), útočník může cílit na slabosti této konkrétní verze
    - e.g. *Cannot insert new record into table Users, unique constraint on column Email has been violated*
- xss

**Základní pojmy**
- [hašování](./5_databaze.md#hašování)
- **klíče** - rozsáhlé řetězce bitů, náhodná čísla, prvočísla...
- **šifrování** - zajišťuje důvěrnost (transformace zprávy za účelem skrytí jejího obsahu před nepovolanými aktéry)
- kódování není šifrování (e.g. zakódované heslo v base64 lze snadno převést do původního tvaru bez jakéhokoliv klíče)
- základním principem kryptografie je **veřejný algoritmus** (dobře otestovaný, vytvořený bezpečnostními experty) a zajištění bezpečnosti pomocí **tajného klíče**. Spoléhat na bezpečnost algoritmu jen jeho (algoritmu) utajením není dobrý nápad.
- **symetrická kryptografie** - komunikující strany sdílí identický klíč, kterým se šifruje i dešifruje. Je to rychlejší, než asymetrická kryptografie, ale hůře se využívají, když vyžadujeme autentizaci (e.g. server by si musel bezpečně uchovávat klíč u každého klienta, zároveň je potřeba se na klíči nějak dohodnout, což může být odposloucháváno)
    - e.g. **AES** (advanced encryption standard), **DES** (data encryption standard)
- **asymetrická kryptografie** - existují 2 druhy klíčů, veřejný (pro šifrování/ověření podpisu) a soukromý (pro dešifrování/tvorbu podpisu). Pokud chtějí 2 strany plně komunikovat (full duplex), pak každá potřebuje znát svůj soukromý klíč a veřejný klíč druhé strany. Pokud někomu prozradím svůj soukromý klíč, může se vydávat za mě.
    - e.g. **RSA, DSA**
    - pro šifrování a podepisování používáme rozdílné páry klíčů, abychom nemuseli čelit problémům, kdy zaměstnanec odejde z firmy (a stále zná soukromý klíč)
- **šifrování v praxi** - kombinace symetrické a asymetrické kryptografie
    - pro komunikaci proběhne ustanovení symetrického klíče náhodným vygenerováním, klíč se bezpečně předá pomocí asymetrické kryptografie. Následně probíhá komunikace šifrovaná symetricky.

**SHA**
- secure hashing algorithm
- SHA-0 a SHA-1 jsou zastaralé a nepovažují se za bezpečné
- rodina hašovacích funkcí SHA-2, zahrnuje SHA-224, SHA-256, SHA-384 a SHA-512 (jména podle jejich délky v bitech)

**RSA** - asymetricá kryptografie, funguje na principu faktorizace velkých čísel (a modulo) - faktorizace je lehká na výpočet, těžká na reverzní výpočet

**Bloková šifra** - symetrická, vstupní data jsou rozdělena na bloky fixní délky, které jsou šifrovány stejným způsobem. Pro větší bezpečnost se nešifrují všechny bloky stejně, ale může se použít e.g. Cipher Block Chaining (CBC), kdy je mezi bloky vytvořena závislost (každý další blok je xorován zašifrovaným předchozím blokem). Změna bitu zašifrovaných dat zmenožní dešifrování zprávy. E.g. AES, DES

**Proudová šifra** - symetrická, z klíče vygenerujeme posloupnost a na jejím základě šifrujeme jednotlivé bity dat (=> každý jinak). Díky tomu není nutné dešifrovat všechno (jako u blokových šifer v reřimu Cipher Block Chaining), ale můžeme jít *od prostředka*. Fungují rychle (třeba pomocí XOR dat s klíčem). Změna bitu zašifrované zprávy pozmění původní zprávu => je vhodné přidat nějakou formu hašování.

[https://www.youtube.com/watch?v=wlSG3pEiQdc&t=1s](https://www.youtube.com/watch?v=wlSG3pEiQdc&t=1s)

**Cyclic Redundancy Check (CRC)**
- kontrolní součet, umožňuje detekci neúmyslných chyb při přenosu/uložení
- je snadné vytvořit vstup odpovídající součtu, takže neposkytuje ochranu před úmyslnou změnou
- e.g. xor, modulo

Pro zajištění důvěrnosti dat bez šifrování lze použít **Chaffing and winnowing** - data rozdělíme na bity (lze i větší části). Pro každý bit budeme v náhodném pořadí posílat dvě zprávy, jednu s validním MAC a jednu (obsahující inverzi bitu) s nevalidním MAC.

**Čipové karty**
- součástí je paměť (RAM, ROM, EEPROM), procesor
- data se na kartě ukládají ve formě souborů, každému lze nastavit přístupová práva (volný přístup/přístup jen s pinem/zakázaný přístup)
- karta je schopná pracovat s kryptografickými algoritmy/protokoly
- lze provádět
    - fyzické útoky (preparace čipu, čtení paměti, využití záření/elektromagnetických polí), obvykle zanechávají viditelné známky útoku
    - logické útoky, vyžadují detailní znalosti o struktuře karty
        - časová analýza - sledujeme odezvu dle vstupu, 
            - jako prevenci se snažíme odstranit závislost délky zpracování na vstupu (e.g. vložíme fejkové instrukce)
        - výkonová analýza - odběrová, memory operace jsou levnější, než výpočty
        - indukce chyb - pomocí náhlých změn podmínek (napětí, teplota...) se snažíme změnit operační podmínky
        - útoky přes api - snažíme se využít možné chyby programátora
            - e.g. počítadlo pokusů by mělo nejdřív snížit počet pokusů, pak ověřit pin a v případě úspěchu resetovat počítadlo pokusů... jinak lze po zadání pinu a detekce neúspěchu rychle odpojit zdroj

**Honeypotting** - pro útočníka připravíme izolovaný subsystém, do kterého ho pustíme a sledujeme jeho chování, čímž se učíme o jeho způsobu práce
