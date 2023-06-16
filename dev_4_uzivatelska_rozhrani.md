# Uživatelská rozhraní.

> Uživatelská rozhraní. Principy návrhu a vývoje uživatelského rozhraní v moderních softwarových systémech, vč. webových, mobilních. Proces vývoje uživatelského rozhraní a zásady kvality. User experience (UX), interaction design, prototypování, wireframování, uživatelský výzkum, testování použitelnosti. Technologie a nástroje. Příklady z praxe pro vše výše uvedené. (PV182 || PV247 || PV278)

*Fun fact: v PV247 se toho o této otázce (vyjma základů Reactu) moc nedozvíte. Ostatní předměty jsem neměl, tak jen z vlastních zkušeností*

## Principy návrhu a vývoje uživatelského rozhraní v moderních softwarových systémech, vč. webových, mobilních.

Klíčem k úspěchu UI je uživatelská přívětivost a snadné použití rozhraní. Našlapaný systém s tunou pokročilých funkcionalit bude k ničemu, pokud se nedá jednoduše používat koncovými uživateli. Běžní uživatelé obvykle dokáží systémum s dobrým UI prominout i některé funkcionální nedostatky.

Návrh rozhraní může ovlivňovat
- jedná se i interní, nebo veřejný systém (musí s ním uživatelé pracovat, nebo s ním pracují jen když chtějí?)
- úroveň odbornosti uživatelů
- nutnost možnosti přizpůsobení
- nutnost vícejazyčnosti aplikace (problém mohou dělat třeba odlišné druhy písma)
- nutnost přístupnosti (accessibility) pro uživatele s nějakým omezením (zrakové či sluchové specifické potřeby, nutnost používat pro ovládání pouze myš/pouze klávesnici)
- platforma

UI se obvykle vyvíjí jako monolit, ale je možné dělat i microfrontendy.

Je fajn brát v potaz
- uživatelskou přívětivost (složité formuláře je fajn dělit na vícero částí, mít pole top-down (ne tabulka polí))
- bezpečnost - pro aplikace, na které mohou cílit útočníci je fajn stránku uživateli přizpůsobit, třeba jeho fotkou, aby bylo těžké stránku napodobit
    - skrývání hesel
- používání vhodných input polí
- rozhraní by mělo být jednotné
- responsivity - fungování aplikace na různě velkých obrazovkách

V současnosti je nejuniverzálnějším způsobem tvorby UI HTML+CSS+JS. Aktuálně jsou populární frontend js frameworky (React, Vue, Solid, Svelte, Angular..). Lze je použít v prohlížeči, jako desktopovou aplikaci (Electron - přibalí se k aplikaci chromium, nebo s využitím native webview třeba Tauri), pro mobilní aplikace (Progressive Web App, není nutné instalovat z appstore), případně mají vlastní verze nativní mobilním zařízení (React Native, Svelte Native...). Tyto technologie mají obvykle dobře řešené věci jako accessibility či lokalizace, existují pro ně solidní komponentové knihovny.

Aktuálním trendem se zajímavým potenciálem je WebAssembly, které umožňuje použití kompilovaného jazyka. Výsledná aplikace je spustitelná v prohlížeči a z pravidla rychlejší, než JS. WASM podporuje 2 mody - práci s DOM, nebo přímé vykreslování na canvas (používá třeba Figma).

Alternativou může být použití multiplafrormního frameworku Flutter (používá jazyk Dart, podporuje android, ios, web), který umožňuje vývoj pro vícero platform z jednoho zařízení.

Nativními jazyky pro mobilní aplikace jsou swift (ios) a java/kotlin (android).

Pro desktopové aplikace je možné použít i technologie jako GTK (existují bindingy na různé jazyky) nebo Qt (C++), JavaFx...

**Specifika pro web**
- řešíme, kdy je stránka renderovaná
- Server side rendering - plně renderovaná na serveru, vhodné pro statické aplikace
- Client side rendering - renderovaná na straně klienta, data jsou do aplikace přidaná (pomocí dotazů na server)

- Tradičně aplikace fungovaly multi-page (přechod na stránku znamenal kompletně nový page load (e.g. PHP))
- Později se přešlo na single-page přístup pomocí client side rendering
- aktuálně je možné přístupy míchat (initial page load je SSR, následně CSR), což je fajn pro iniciální page load, friendly pro SEO roboty... (e.g. nextjs)

## Proces vývoje uživatelského rozhraní a zásady kvality.

Začneme uživatelským výzkumem, abychom pochopili skutečné požadavky na UI. Následuje návrh pomocí wireframů a prototypů, které můžeme použít pro zpětnou vazbu a jako předlohu pro vývojovou práci. Na konec je důležité provést uživatelské testování s různými typy uživatelů, abychom předešli problémům při nasazení aplikace.

### Uživatelský výzkum, analýza
- Je důležité určit, kdo jsou naši uživatelé, jak dosud pracují se stávajícími systémy, co jim vyhovuje a co ne...
- Je důležité určit, co jsou požadavky na systém, jak se používá (můžeme se snažit o minimalizaci kliknutí pro dosažení operace...)
- s uživateli lze řešit i náš produkt ve **focus groups**
- lze použít A/B testování - každé skupině uživatelů prezentujeme určitou variantu produktu a sledujeme vliv

### Interaction design

Snažíme se pochopit, jakým způsobem uživatelé se systémem pracují či interagují a uzpůsobit tomu uživatelské rozhraní. 

### Wireframování

Vhodné pro návrh rozložení jednotlivých komponentů rozhraní, neřešíme barvy/styly, ale jen layout a hrubé rozložení obsahu.

### Prototypování 

Pomocí návrhových nástrojů jako AdobeXD nebo Figma, lze vytvořit (z části i interaktnvní) prototyp, který lze použít pro prvotní uživatelské testování.

## User experience (UX)

Je kombinací aspektů uživatelského rozhraní
- vizuální krása
- použitelnost (umožňuje fungovat i s omezeními)
- užitnost (umožňuje provádět chtěné akce)
- efektivita (usnadňuje provádění akcí), e.g.
    - uživatel by neměl být nucen do systému zadávat totožnou informaci vícekrát
    - pro časté akce je fajn umožnit použití dobře známých klávesových zkratek

Celkové user experience určuje, zda uživatelé budou chtít náš produkt používat.

## Testování použitelnosti

Může zahrnovat
- pozorování uživatelů při provádění úkonů v rámci systému
    - sledujeme jejich akce, reakce, potíže
    - na základě sledování lze navrhnout řešení
- evaluace experty z oblastí přístupnosti (accessability), lze také použít automatizované nástroje jako Lighthouse
- sledování očí uživatele při používání produktu, zjistíme tak, jaké části rozhraní nejvíce upoutají pozornost uživatelů, je k tomu potřeba souhlas uživatele
