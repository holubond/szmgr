# Bezpečný kód.

> Bezpečný kód. Metody autentizace a řízení přístupu. Biometrické metody autentizace, jejich dopady a problémy. Elektronický podpis a jeho použití. Autentizace strojů a aplikací. Zásady a principy bezpečného kódu. Typické bezpečnostní chyby na úrovni kódu, souběžnost, ošetření vstupů. Detekce bezpečnostních zranitelností, penetrační testování. Příklady z praxe pro vše výše uvedené. ([PV157](https://is.muni.cz/auth/el/fi/podzim2021/PV157/um/), PA193, PV276)

## Metody autentizace a řízení přístupu.

**Klíčové pojmy**
- **Autentizace** - verifikace/ověření identity - kombinace identity (e.g. jména) a důkazu (e.g. hesla)
- **Identifikace** - rozpoznání - rozpoznání entity v dané množině entit, subjekt nepředkládá identitu, ale systém se mu ji snaží přiřadit z databáze známých identit, e.g. otisk prstu na vstupních dveřích
- **Autorizace** - udělení práv k vykonání určitých akcí (e.g. autorizace terminálu k platbě vložením karty a pinu, zároveň se uživatel autentizoval pinem vůči kartě)

**Metody autentizace**
- na základě znalosti (pin/heslo)
- na základě vlastnictví fyzického tokenu (klíč/čipová karta)
- na základě biometrie (otisk prstu, který nikdo jiný nemá)
- na základě fyzické/virtuální lokace (rychlá změna fyzické lokace může být varovný signál, přihlášení z určitého počítače s certifikátem může stačit k autentizaci)

## Biometrické metody autentizace, jejich dopady a problémy.
TODO

## Elektronický podpis a jeho použití.

- zajišťuje **autentizaci** (že zpráva pochází od daného autora) a **integritu** (že zprávu nikdo nepozměnil) **podepisovaných dat**
- musí být ověřitelný třetí stranou
- nezajišťuje důvěrnost dat
- měl by obsahovat i datum/čas
- algoritmy **RSA, DSA**

Některé algoritmy umožňují obnovu dat na základě podpisu (v podpisu jsou nějakým způsobem data obsažena).

**Průběh podepisování**
1. vytvořím asymetrické klíče (veřejný, soukromý), veřejný klíč zaregistruju/vystavím, aby mohl být později použit k ověření
2. (pro každý podepisovaný dokument) - vytvořím hash (e.g. pomocí SHA-2) podepisovaného dokumentu, který podepíšu soukromým klíčem (asymetrické algoritmy bývají pomalé, takže nepodepisuju celý dokument)
3. podepsané dokumenty je možné ověřit pomocí mého vystaveného veřejného klíče 

**Certifikát**
- spojuje veřejný klíč a informace o subjektu, který veřejný klíč poskytuje
- společně je s informacemi o certifikační autoritě podepsán pomocí soukromého klíče **certifikační agentury**
    - vytváří se řetězec důvěry - pokud věříš autoritě, můžeš věřit i mně
- certifikační autorita 
    - zajišťuje, že daný subjekt opravdu vlastní soukromý klíč
    - autentizuje subjekt vystavující veřejný klíč (zajišťuje, že daný subjekt je tím, co tvrdí) 
    - potvrzuje platnost veřejného klíče daného subjektu svým podpisem (lze časově omezit)

**Použití podpisu**
- autentizace dat (podpis zprávy, který si mohou ostatní ověřit)
- autentizace počítačů/tokenů díky mechanismu **výzva-odpověď (challenge-response)**
    - jestli jsi opravdu vlastníkem soukromého klíče, tak podepiš tato vzorová data
- autentizace osob pomocí schopnosti spustit aplikaci na počítači/tokenu

## Autentizace strojů a aplikací.
TODO

## Zásady a principy bezpečného kódu.
TODO

## Typické bezpečnostní chyby na úrovni kódu, souběžnost, ošetření vstupů.
TODO

## Detekce bezpečnostních zranitelností, penetrační testování.
TODO

## Notes

**Základní pojmy**
- [hašování](./5_databaze.md#hašování)
- **klíče** - rozsáhlé řetězce bitů, náhodná čísla, prvočísla...
- **šifrování** - zajišťuje důvěrnost (transformace zprávy za účelem skrytí jejího obsahu před nepovolanými aktéry)
- základním principem kryptografie je **veřejný algoritmus** (dobře otestovaný, vytvořený bezpečnostními experty) a zajištění bezpečnosti pomocí **tajného klíče**. Spoléhat na bezpečnost algoritmu jen jeho (algoritmu) utajením není dobrý nápad.
- **symetrická kryptografie** - komunikující strany sdílí identický klíč, kterým se šifruje i dešifruje. Je to rychlejší, než asymetrická kryptografie, ale hůře se využívají, když vyžadujeme autentizaci (e.g. server by si musel bezpečně uchovávat klíč u každého klienta, zároveň je potřeba se na klíči nějak dohodnout, což může být odposloucháváno)
- **asymetrická kryptografie** - existují 2 druhy klíčů, veřejný (pro šifrování/ověření podpisu) a soukromý (pro dešifrování/tvorbu podpisu). Pokud chtějí 2 strany plně komunikovat (full duplex), pak každá potřebuje znát svůj soukromý klíč a veřejný klíč druhé strany
- **šifrování v praxi** - kombinace symetrické a asymetrické kryptografie
    - pro komunikaci proběhne ustanovení symetrického klíče náhodným vygenerováním, klíč se bezpečně předá pomocí asymetrické kryptografie. Následně probíhá komunikace šifrovaná symetricky.


**SHA**
- secure hashing algorithm
- SHA-0 a SHA-1 jsou zastaralé a nepovažují se za bezpečné
- rodina hašovacích funkcí SHA-2, zahrnuje SHA-224, SHA-256, SHA-384 a SHA-512 (jména podle jejich délky v bitech)

TODO možná rozvést oauth, oidc, kerberos, saml, tls certifikáty, symetrické/asymetrické klíče, kryptografie bez klíčů, zpracování dat po blocích/v souvislém proudu