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

