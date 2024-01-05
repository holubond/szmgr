# Bezpečná infrastruktura.
> Aspekty ovlivňující bezpečnost systému na úrovni jeho infrastruktury, síťová bezpečnost. Kyberbezpečnostní hrozby a příklady útoků. Návrh bezpečnostních mechanismů, detekce zranitelností, penetrační testování, analýza bezpečnostních útoků. Příklady z praxe pro vše výše uvedené. (PA018 || PV276 || PA211)

## Aspekty ovlivňující bezpečnost systému na úrovni jeho infrastruktury
Aspekty ovlivňující bezpečnost systému na úrovni jeho infrastruktury jsou rozmanité a zahrnují řadu klíčových oblastí:
###  Fyzická bezpečnost
Ochrana hardwarových komponent, datových center a dalších fyzických prostředků před neoprávněným přístupem, poškozením nebo krádeží. Opatření pro vyšší bezpečnost mohou zahrnovat správné nastavení přístupu do datového centra, opatření proti požáru apod.
### Zabezpečení dat
Opatření k ochraně dat uložených v systému. Zvyšování můžeme dělat například šifrováním, které ma za úkol znemožnit útočníkům získání hodnotných dat. Efektivní správa šifrovacích klíčů je zásadní pro zabezpečení dat; šifrovací klíče by měly být uchovávány odděleně od šifrovaných dat. Dnes je to běžné v cloudech, kde na to existují speciální resources. 
Jako příklad je také solení hesel. V historii se mnohokrát stalo, že unikly databáze uživatelů a solení pomohlo před odhalením hesel uživatelů v raw formě.
Další příklad zabezpečení dat je pravidelné zálohování, která ma za úkol obnovu např. v případě výpadku systému. U zálohování je také třeba dbát na více úrovní zálohování, když by jednotlivé úrovně selhaly, nebo byly zasaženy taky např. požárem datacentra.
### Aktualizace a správa záplat
Pravidelné aktualizace softwaru a hardwaru, aby se zajistilo, že jsou všechny systémové komponenty chráněny proti nejnovějším bezpečnostním hrozbám. Většina aplikací používá nějaké dependence, nebo third party software. Jedná se o knihovny v kódu, SW pro DB atd. Staré verze těchto aplikací obsahují dost často již objevené vulnerability.
Je tak nutné sledovat a aktualizovat. Dnes již existuje SW, který dokáže tyto věci automatizovat jako např. Snyk. Je dobré dělat pravidelné bezpečnostní audity softwaru pro odhalení možných slabých míst.
### Identity a přístupové managementy (IAM)
Správa uživatelských identit a jejich přístupových práv k zajištění, že jen oprávnění uživatelé mají přístup k citlivým systémovým zdrojům. Každý uživatel/admin by měl dostat přístup jen k částem, ke kterým ho přistupuje. Nemělo by se jít opačnou metodou, tedy povolit přístup ke všemu a následně omezovat, to je mnohem náchylnější na chyby.
Mnoho časté byly i útoky, kde nebyla víceúrovňová autentizace. Zaměstnanec se např. mohl přihlásit pouze SSH klíčem. Dnes je to ošetřeno již nutným přístupem do vnitřní sítě, 2FA atd. Politiky minimálních oprávnění a pravidelné revize oprávnění jsou klíčové pro zamezení nadměrného přístupu a potenciálního zneužití oprávnění. Víceúrovňová autentizace, včetně biometrických metod, může výrazně zvýšit zabezpečení přístupu k citlivým systémům.
### Aplikační bezpečnost
Bezpečné programovací techniky a používání ověřených vývojových frameworků jsou klíčové pro prevenci zranitelností, jako jsou SQL injection nebo Cross-Site Scripting (XSS). Pravidelné bezpečnostní revize kódu a automatizované nástroje pro statickou analýzu mohou pomoci odhalit a opravit bezpečnostní slabiny v aplikacích. Častým problémem je i vypisování hesel a citlivých informací do logů.
### Monitoring a logování
Monitoring a logování je důležité v případech, kdy máme podezření na průnik systému (dokážeme zpětně trasovat útočníka), nebo i k běžným kontrolám, co se v našem systému aktuálně děje. Bez správného logování a monitoringu ztrácíme přehled o vnitřním ději v systému. Automatizované nástroje pro analýzu logů a detekci anomálií mohou výrazně zlepšit schopnost identifikovat a reagovat na bezpečnostní hrozby v reálném čase.
### Lidský faktor
Asi jen zmínit, že lidský faktor je jedním z nejčastějších příčin průniku do systému. Slabá hesla, špatná obezřetnost, nedodržování standardů nastavených firmou atd. Existuje mnoho bezpečnostních politik, které se snaží s lidským faktorem počítat a dělat doporučení i v závislosti na něj. Problémy: Je dobré, aby si uživatel měnil pravidelně heslo? Jaká je vhodná doba expirace přihlášení zaměstnance?

## Síťová bezpečnost
Síťová bezpečnost je klíčovým aspektem ochrany IT infrastruktury organizací před neoprávněným přístupem, útoky a poškozením. Většina útoků aspoň nějakým způsobem využívá síť. Kyberútoky většinou začínají útokem přes síť a také je nejjednoduší a nejlepší je eliminovat již přes síť. Zde jsou aspekty bezpečnosti

### Firewally
Jedná se o zařízení nebo softwarové aplikace, které monitorují a kontrolují vstupní a výstupní síťový provoz na základě předem stanovených bezpečnostních pravidel. Firewally jsou první linií obrany v síťové bezpečnosti, protože filtrují nežádoucí síťový provoz.

### Virtual Private Networks (VPN)
VPN umožňuje bezpečné připojení k síti přes veřejný internet tím, že šifruje data, která jsou přenášena mezi uživatelem a síťovými zdroji. To je zvláště důležité pro vzdálené pracovníky, kteří potřebují bezpečný přístup k firemním zdrojům. VPN je velmi často předpoklad, aby zaměstnanec mohl využívat firemní zdroje. Jde tak o snahu eliminovat komunikaci s vnější sítí na minimum.

### Anti-malware a antivirové programy
Tato softwarová řešení chrání síť proti malware, virům, červům, trojským koním a dalším hrozbám. Velmi často obsahuje protekci jednotlivých endpointů, ale zároveň i monitorování sítě a detekci možných hrozeb a podezřelé aktivity.

### Šifrování dat
Šifrování je klíčové pro ochranu citlivých dat během jejich přenosu po síti. Protokoly jako SSL/TLS jsou používány pro šifrování dat přenášených mezi webovými servery a prohlížeči. Šifrování lze vidět i v transportní vrstvě (TCP) a IPv6

### Síťová segmentace
Rozdělení sítě na menší části (segmenty) může pomoci omezit šíření škodlivého softwaru a zvýšit výkon tím, že se oddělí citlivé oblasti sítě od ostatních. Využití lze například najít při testování škodlivého software. Lze omezit síť tak, že systémy, které dané hrozby spouštějí jsou na omezené síti (black network) a zamezí se tak možné lateral movement hrozbě po síti.

### Správa přístupu a autentizace
Síťová bezpečnost zahrnuje také správu přístupu k síťovým zdrojům. To znamená ověřování uživatelů a zařízení, které se pokoušejí získat přístup do sítě, a zajištění, že mají příslušná oprávnění. Jsou tam jednotlivé části jako Identifikace, Autentizace a Autorizace


## Kyberbezpečnostní hrozby a příklady útoků 
![](img/threat_vuln.png)

Analýza rizik identifikuje možná rizika a identifikuje možná vylepšení. Útok je konkrétní akt prolomení bezpečnostních kontrol a vniku do systému, kde může udělat aktivní změny, nebo jen např. pasivně vytáhnout nějaká data. Útok nemusí být vždy úspěšný a může zůstat u stádia pokusu. Daný útok nemusí využívat jen jedné vulnerability/threatu, může se jednat o kombinaci, která dovolí útočníkovi proniknout do systému.

![](img/tactics.png)
Na obrázku můžete vidět části konkrétního útoku. Příklad může být následovný:
Taktika: Eskalace privilegií a získání přístupu nad systémem
Technika: Využití vulnerability k prolomení SSH hesla uživatele + následné využití zastaralého balíčku v systému k získání sudo právům
Procedury: SW pro slovníkový útok pomocí hesla + CLI daného balíčku k získání přístupu

Existují různé katalogy technik, taktik atd. Nejdůležitější z nich je MITRE ATT&CK. Ten obsahuje matice taktik a technik. Daná společnost také hodnotí jednotlivé antiviry a protection software, jak velké množství daných hrozeb dokáže detekovat a reagovat na ně. Také hodnotí přesnost daného SW v popisu jednotlivých hrozeb

Cyber threat hunting je proaktivní hledání nových hrozeb a možných útoků. Je velmi důležitý při hledání 0-day vulnerabilit. To jsou vulnerability, které nejsou veřejně známé, tudíž proti nim nejsou vydané security patche a jsou skrz to nebezpečné. Může se stát, že útočník má k dispozici danou vulnerabilitu několik měsíců/let, než ji odhalí tvůrce systému. Kombinaci 0-day vulnerabilit využíval například spyware Pegasus k ovládnutí celého systému chytrých telefonů. Společnosti vypisují bug bounty program, který má motivovat "hodné hackery" k hledání těchto chyb a jejich reportování společnostem. Ty jsou následně peněžně ohodnoceny firmami. Firmy dost často mívají i svůj red team, který má za úkol prolomit systém, chovat se jako útočník a reportovat zjištěné slabiny.

### Příklady útoků
- 0-day - Útoky využívající zero-day zranitelností, tedy slabých míst v softwaru, o kterých výrobce ještě neví a neexistuje pro ně záplata. Příkladem je Stuxnet, kybernetická zbraň, která využívala několik zero-day exploitů k napadení íránského jaderného programu. Jedná se o kategorizaci závažnosti hrozby spíše než o její typ.
- Malware - zahrnuje SW, který má za úkol škodit a nějak napadnout systém. Může zahrnovat mnoho různých typů škodlivého SW, který je dále kategorizován podle toho, jak má útočit. Na internetu existují databáze known malware souborů, které využívají většinou hash souboru. Příkladem databáze je VirusTotal.
- Ransomware - má za úkol zašifrovat data na daném systému a poté většinou vyžadovat výkupné. Aktuálně typ malware, který je nejvíc na vzestupu. Mohli jsme vidět útoky i na nemocnici v Brně. Speciálním typem jsou wipery, které mají za úkol data smazat.
- 0-click - hlavně u smartphonů. Jedná se o útok, který nepotřebuje jakoukoliv interakci uživatele a většinou po sobě zamaže i stopy. Příkladem je Pegasus, který dokázal infikovat zařízení uživatelů jen pomocí pouhého zavolání, které ani daná osoba nemusela přijmout.
- Phising - tyto útoky zahrnují posílání podvodných e-mailů, které se zdají pocházet z důvěryhodných zdrojů, s cílem získat citlivé informace, jako jsou přihlašovací údaje a čísla kreditních karet. Slavným příkladem je útok na Hillary Clintonovou během prezidentské kampaně v USA v roce 2016.
- DDOS - cílem těchto útoků je přetížit webové servery nebo sítě tak, aby se staly nedostupnými pro legitimní uživatele. Například v roce 2016 byl proveden masivní DDoS útok na službu Dyn, což vedlo k výpadku mnoha velkých webových stránek, včetně Twitteru a Netflixe.
- Insider threats - tyto hrozby pochází od jedinců uvnitř organizace, kteří zneužívají svůj přístup k citlivým informacím. Jeden z nejznámějších případů je Edward Snowden, který v roce 2013 ukradl a zveřejnil tajné dokumenty NSA.
- Lateral movement - útočníci používají k rozšíření svého dosahu v rámci síťové infrastruktury oběti po počátečním průniku. Tento pohyb umožňuje útočníkům získat přístup k dalším systémům a zdrojům v síti, často s cílem získat citlivé informace, způsobit škody nebo připravit terén pro další útoky.

Existuje software, který dokáže jednotlivé útoky monitorovat a detekovat (detect) a v případě zájmu uživatele i udělat protiakci. Může se jednat o zablokování malware souborů s negativním hashem, zablokování škodlivého procesu, revert souborů u ransomware atd.
