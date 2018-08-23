---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Ochrana připojovacích řetězců a dalších konfiguračních údajů (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Aplikace ASP.NET obvykle ukládá informace o konfiguraci v souboru Web.config. Některé z těchto informací je citlivý a zaručuje ochranu. Ve výchozí...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 9713bbd983c4e922273a23356cbbb3848a8b7c50
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751845"
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Ochrana připojovacích řetězců a dalších konfiguračních údajů (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) nebo [stahovat PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Aplikace ASP.NET obvykle ukládá informace o konfiguraci v souboru Web.config. Některé z těchto informací je citlivý a zaručuje ochranu. Ve výchozím nastavení nebude možné tento soubor dodávat do návštěvníky webu, ale správce nebo se hacker může získat přístup k systému souborů webového serveru a zobrazit obsah souboru. V tomto kurzu jsme dozvíte, že technologie ASP.NET 2.0, umožníte nám kvůli ochraně citlivých informací tím, že šifruje části souboru Web.config.


## <a name="introduction"></a>Úvod

Informace o konfiguraci pro aplikace ASP.NET je obvykle uložen v souboru XML s názvem `Web.config`. V průběhu těchto kurzů jsme aktualizovali `Web.config` několika časy. Při vytváření `Northwind` typované datové sady v [první kurz](../introduction/creating-a-data-access-layer-vb.md), například informace o připojovacím řetězci byl automaticky přidán do `Web.config` v `<connectionStrings>` oddílu. V pozdější [stránky předlohy a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu ručně aktualizovali jsme `Web.config`, přidáním `<pages>` element označující, že všechny stránky technologie ASP.NET v našem projektu by měly používat `DataWebControls` motiv.

Protože `Web.config` mohou obsahovat citlivá data, jako je například připojovací řetězce, je důležité, který obsah `Web.config` zachovaná, bezpečné a skryté z neoprávněné uživatele. Ve výchozím nastavení, všechny HTTP žádosti do souboru s `.config` rozšíření se postará modul ASP.NET, který vrátí *typ stránky není poskytováni,* zprávy je znázorněno na obrázku 1. To znamená, že nelze zobrazit návštěvníci vašeho `Web.config` s obsah souboru tak, že jednoduše zadáte http://www.YourServer.com/Web.config do adresního řádku svého prohlížeče s.


[![Navštívit Web.config prostřednictvím prohlížeč vrátí tento typ stránky není poskytováni zprávy](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Obrázek 1**: navštívit `Web.config` prostřednictvím prohlížeč vrátí tento typ stránky není poskytováni zprávy ([kliknutím ji zobrazíte obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Ale co když je útočník schopen nalézt některé zneužití, umožňující jí chcete-li zobrazit vaše `Web.config` s obsah souboru? Co může útočník udělat pomocí těchto informací a popíše, co děláte dalším stupněm ochrany citlivých informací v rámci `Web.config`? Naštěstí většině částí v `Web.config` neobsahují citlivé informace. Jaké poškození může útočník perpetrate Pokud název výchozí motivu, který používá vaše stránky technologie ASP.NET, které znají?

Některé `Web.config` části, ale obsahují citlivé informace, které mohou zahrnovat připojovací řetězce, uživatelská jména, hesla, názvy serverů, šifrovacích klíčů a tak dále. Tyto informace se většinou nacházejí ve následující `Web.config` oddíly:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

V tomto kurzu se podíváme na techniky k ochraně těchto citlivých informací konfigurace. Jak vidíte, rozhraní .NET Framework verze 2.0 obsahuje chráněné konfigurace systému, který umožňuje programově šifrování a dešifrování vybranou konfigurační oddíly podrobným.

> [!NOTE]
> V tomto kurzu končí podívat s doporučením společnosti Microsoft pro připojení k databázi z aplikace technologie ASP.NET. Kromě šifrování připojovací řetězce, pomáhá posílit ochranu systému díky zajištění, které se připojujete k databázi zabezpečeným způsobem.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Krok 1: Zkoumání ASP.NET 2.0 s chráněné možnosti konfigurace

Technologie ASP.NET 2.0 obsahuje chráněné konfigurace systému pro šifrování a dešifrování informace o konfiguraci. To zahrnuje metody v rozhraní .NET Framework, je možné programově zašifrovat nebo dešifrovat informace o konfiguraci. Používá systém chráněné konfigurace [modelu poskytovatele](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), která vývojářům umožňuje zvolit, jaké implementaci se používá.

Rozhraní .NET Framework se dodává s dva poskytovatelé chráněné konfigurace:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -používá asymetrický [algoritmus RSA](http://en.wikipedia.org/wiki/Rsa) pro šifrování a dešifrování.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -používá Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) pro šifrování a dešifrování.

Od systému chráněné konfigurace implementuje vzor návrhu poskytovatele, je možné vytvářet vlastní chráněné konfigurace zprostředkovatele a zařadit ho do své aplikace. Zobrazit [implementace poskytovatele konfigurace chráněné](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Další informace o tomto procesu.

Poskytovatelé RSA a DPAPI pomocí kláves pro jejich šifrování a dešifrování rutiny a tyto klíče mohou být uloženy v počítači - nebo -úrovni uživatele. Úroveň počítače klíče jsou ideální pro scénáře, ve kterém je webová aplikace spuštěna na svůj vlastní vyhrazený server nebo pokud existuje více aplikací na serveru, potřebujete sdílet šifrované informace. Individuální klíče jsou bezpečnější možnost v sdílená hostitelská prostředí, kde jiné aplikace na stejném serveru správně neměl moct dešifrovat konfigurační oddíly funkce s chránit vaše aplikace.

V tomto kurzu použije našich příkladů DPAPI zprostředkovatele a klíče úrovni počítače. Konkrétně se podíváme na šifrování `<connectionStrings>` tématu `Web.config`, i když chráněný konfigurační systém lze použít k většině žádné šifrování `Web.config` oddílu. Informace o používání klíčů individuální nebo pomocí zprostředkovatele RSA vyhledejte prostředky v části Další čtení na konci tohoto kurzu.

> [!NOTE]
> `RSAProtectedConfigurationProvider` a `DPAPIProtectedConfigurationProvider` poskytovatelé jsou registrované ve `machine.config` soubor s názvy zprostředkovatelů `RsaProtectedConfigurationProvider` a `DataProtectionConfigurationProvider`v uvedeném pořadí. Při šifrování nebo dešifrování informace o konfiguraci bude potřeba zadat název odpovídající zprostředkovatele (`RsaProtectedConfigurationProvider` nebo `DataProtectionConfigurationProvider`) namísto skutečný typ název (`RSAProtectedConfigurationProvider` a `DPAPIProtectedConfigurationProvider`). Můžete najít `machine.config` soubor `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` složky.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Krok 2: Programově šifrování a dešifrování konfigurační oddíly funkce

Po zadání několika řádků kódu jsme šifrování nebo dešifrování konkrétní konfigurační oddíl se zadaným zprostředkovatelem. Kód, protože jsme se zobrazí po chvíli, jednoduše prostřednictvím kódu programu odkazovat na části odpovídající konfiguraci volat jeho `ProtectSection` nebo `UnprotectSection` metodu a poté zavolejte `Save` metoda a zachová tak změny. Kromě toho rozhraní .NET Framework obsahuje užitečné příkazového řádku nástroj, který můžete šifrovat a dešifrovat informace o konfiguraci. Podíváme se na tento nástroj příkazového řádku v kroku 3.

Pro ilustraci programově chrání informace o konfiguraci, umožňují s vytvoření stránky technologie ASP.NET, který obsahuje tlačítka pro šifrování a dešifrování `<connectionStrings>` tématu `Web.config`.

Začněte otevřením `EncryptingConfigSections.aspx` stránku `AdvancedDAL` složky. Přetáhněte ovládací prvek textového pole z panelu nástrojů do Návrháře nastavení jeho `ID` vlastnost `WebConfigContents`, jeho `TextMode` vlastnost `MultiLine`a jeho `Width` a `Rows` vlastnosti 95 %, 15, v uvedeném pořadí. Tento ovládací prvek textové pole se zobrazí obsah `Web.config` umožňuje nám chcete rychle zobrazit, pokud se obsah nebo není zašifrovaná. Samozřejmě v reálné aplikaci byste nikdy chcete zobrazit obsah `Web.config`.

Pod textové pole, přidejte dva ovládací prvky tlačítka s názvem `EncryptConnStrings` a `DecryptConnStrings`. Nastavte jejich vlastnosti Text připojovací řetězce šifrování a dešifrování řetězce připojení.

V tomto okamžiku vaše obrazovka by měla vypadat podobně jako na obrázku 2.


[![Přidat textové pole a dva ovládací prvky tlačítka Web na stránku](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Obrázek 2**: Přidání textového pole a dva ovládací prvky tlačítka Web na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Dále musíme napsat kód, který načte a zobrazí obsah `Web.config` v `WebConfigContents` textového pole na stránce při prvním načtení. Přidejte následující kód do třídy stránky s kódem na pozadí. Tento kód přidá metodu s názvem `DisplayWebConfig` a nazve je z `Page_Load` obslužné rutiny události při `Page.IsPostBack` je `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig` Metoda používá [ `File` třídy](https://msdn.microsoft.com/library/system.io.file.aspx) otevřete aplikaci s `Web.config` souboru [ `StreamReader` třídy](https://msdn.microsoft.com/library/system.io.streamreader.aspx) přečtení jeho obsahu do řetězce a [ `Path` třídy](https://msdn.microsoft.com/library/system.io.path.aspx) ke generování fyzickou cestu k `Web.config` souboru. Tyto tři třídy se nacházejí v [ `System.IO` obor názvů](https://msdn.microsoft.com/library/system.io.aspx). V důsledku toho budete muset přidat `Imports``System.IO` příkaz do horní části třídy modelu code-behind nebo, případně tyto třídy názvy s předponou `System.IO.`

V dalším kroku je potřeba přidat obslužné rutiny události pro dva ovládací prvky tlačítka `Click` události a přidání nezbytného kódu k šifrování a dešifrování `<connectionStrings>` části pomocí klíče počítače úrovni s poskytovateli rozhraní DPAPI. Z návrháře, klikněte dvakrát na každé z tlačítka pro přidání `Click` obslužné rutiny události v modelu code-behind třídu a přidejte následující kód:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Kód použitý v obslužné rutině dvě události je téměř shodná. Oba začít tím, že získáme informace o aktuální aplikaci s `Web.config` souboru prostřednictvím [ `WebConfigurationManager` třídy](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metoda](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Tato metoda vrátí konfiguračním souboru webu pro zadanou virtuální cestu. Další, `Web.config` soubor s `<connectionStrings>` oddílu se přistupuje přes [ `Configuration` třídy](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), který vrátí hodnotu [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) objektu.

`ConfigurationSection` Objekt zahrnuje [ `SectionInformation` vlastnost](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) , která poskytuje další informace a funkce týkající se konfigurační oddíl. Jako kód výše znázorňuje, můžeme určit, jestli se šifruje konfiguračního oddílu tak, že zkontrolujete `SectionInformation` vlastnost s `IsProtected` vlastnost. Navíc můžete šifrovat nebo dešifrovat prostřednictvím části `SectionInformation` vlastnost s `ProtectSection(provider)` a `UnprotectSection` metody.

`ProtectSection(provider)` Metoda přijímá jako vstupní řetězec určující název poskytovatele chráněné konfigurace pro použití při šifrování. V `EncryptConnString` předáme DataProtectionConfigurationProvider do obslužné rutiny události tlačítka s `ProtectSection(provider)` metodu tak, aby používalo poskytovateli rozhraní DPAPI. `UnprotectSection` Metody můžete určit zprostředkovatele, který se použil k zašifrování konfiguračního oddílu a proto není nutné žádné vstupní parametry.

Po volání `ProtectSection(provider)` nebo `UnprotectSection` metoda, je třeba zavolat `Configuration` objektu s [ `Save` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) a zachová tak změny. Jakmile informace o konfiguraci byla zašifrovaná nebo dešifrovat, a tyto změny uložit, říkáme `DisplayWebConfig` k načtení aktualizované `Web.config` obsah do ovládacího prvku textového pole.

Po zadání výše uvedený kód ji otestovat přechodem `EncryptingConfigSections.aspx` stránky prostřednictvím prohlížeče. Zpočátku zobrazí stránka, která uvádí obsah `Web.config` s `<connectionStrings>` části zobrazí ve formátu prostého textu (viz obrázek 3).


[![Přidat textové pole a dva ovládací prvky tlačítka Web na stránku](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Obrázek 3**: Přidání textového pole a dva ovládací prvky tlačítka Web na stránku ([kliknutím ji zobrazíte obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Nyní klikněte na tlačítko šifrovat připojovací řetězce. Pokud je povoleno ověření požadavku, značky vrácena zpět `WebConfigContents` TextBox způsobí `HttpRequestValidationException`, který zobrazí zprávu, která je potenciálně nebezpečná `Request.Form` z klienta byla zjištěna hodnota. Žádost o ověření, která je povolena ve výchozím nastavení v technologii ASP.NET 2.0, zakazují zpětná volání, které zahrnují bez kódování HTML a pomoct zabránit útokům prostřednictvím injektáže skriptu. Tato kontrola je možné zakázat na stránce - nebo -úrovni aplikace. Chcete-li vypnout pro tuto stránku, nastavte `ValidateRequest` nastavení `False` v `@Page` směrnice. `@Page` – Direktiva se nachází v horní části stránky s deklarativní.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Další informace o ověření žádosti, jeho účel, zakažte ho na stránce - a -úrovni aplikace, jak jak kódovat do formátu HTML značky, naleznete v tématu [ověření požadavku – prevence útoků skript](../../../../whitepapers/request-validation.md).

Po zakázání ověření žádosti pro stránku, zkuste to znovu kliknutím na tlačítko šifrovat připojovací řetězce. Na zpětné volání, budou mít přístup konfiguračního souboru a jeho `<connectionStrings>` části šifrované pomocí poskytovatele rozhraní DPAPI. Textové pole se pak aktualizuje a zobrazí nový `Web.config` obsah. Obrázek 4 ukazuje, `<connectionStrings>` informace je zašifrovaný.


[![Kliknutí zašifrovat připojovací řetězce tlačítko šifruje &lt;connectionString&gt; oddílu](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Obrázek 4**: kliknutí zašifrovat připojovací řetězce tlačítko šifruje `<connectionString>` oddílu ([kliknutím ji zobrazíte obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Šifrovaný `<connectionStrings>` část vygenerované v mém počítači se řídí, i když některé z obsahu `<CipherData>` byl prvek odebrán pro zkrácení:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Prvek určuje zprostředkovatele, který slouží k provádění šifrování (`DataProtectionConfigurationProvider`). Tyto informace používá `UnprotectSection` metodu po kliknutí na tlačítko dešifrování řetězce připojení.


Když se informace o připojovacím řetězci přistupuje z `Web.config` – buď pomocí ovládacího prvku SqlDataSource napíšeme kód nebo automaticky generovaný kód z objektů TableAdapter v naší datové sady typu – jsou automaticky dešifrována. Stručně řečeno, jsme nemusíte přidávat žádné další kódu nebo logice k dešifrování šifrovaný `<connectionString>` oddílu. Abychom to navštíví některý z předchozích kurzů v tuto chvíli, jako je například tento kurz jednoduchého zobrazení z části základní tvorbou sestav (`~/BasicReporting/SimpleDisplay.aspx`). Jak je vidět na obrázku 5, tento kurz pracuje přesně tak, jak jsme byste očekávali, označující, že šifrované připojovacího řetězce se automaticky dešifruje stránka technologie ASP.NET.


[![Vrstva přístupu k datům automaticky dešifruje informace o připojovacím řetězci](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Obrázek 5**: vrstva přístupu k datům automaticky dešifruje informace o připojovacím řetězci ([kliknutím ji zobrazíte obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Vrátit zpět `<connectionStrings>` části zpět do její znázornění prostého textu, klikněte na tlačítko dešifrování řetězce připojení. Při zpětném odeslání byste měli vidět připojovací řetězce v `Web.config` ve formátu prostého textu. V tomto okamžiku vaše obrazovka by měla vypadat stejně, jako kdyby při první návštěvě této stránky (viz obrázek 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Krok 3: Šifrování konfigurační oddíly funkce pomocí`aspnet_regiis.exe`

Zahrnuje celou řadu nástrojů příkazového řádku v rozhraní .NET Framework `$WINDOWS$\Microsoft.NET\Framework\version\` složky. V [použití závislostí mezipaměti SQL](../caching-data/using-sql-cache-dependencies-vb.md) kurz, například jsme se podívali na použití `aspnet_regsql.exe` nástroj příkazového řádku pro přidání infrastrukturu nutnou pro závislosti mezipaměti SQL. Dalším nástrojem užitečné příkazový řádek v této složce je [nástroje ASP.NET IIS Registration (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Jak již název napovídá, nástroje ASP.NET IIS Registration slouží především pro registraci aplikace technologie ASP.NET 2.0 s Microsoft s profesionální webový server IIS. Kromě jeho funkce související s IIS nástroje ASP.NET IIS Registration také slouží k šifrování nebo dešifrování zadaný konfigurační oddíly funkce v `Web.config`.

Následující příkaz ukazuje obecnou syntaxi použitý k šifrování konfiguračního oddílu s `aspnet_regiis.exe` nástroj příkazového řádku:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*část* oddíl konfigurace pro šifrování (například connectionStrings), je *fyzické\_directory* je úplnou fyzickou cestu k kořenový adresář webové aplikace s a *zprostředkovatele*  je název zprostředkovatele chráněné konfigurace (například DataProtectionConfigurationProvider). Můžete také webové aplikace bude zaregistrovaná ve službě IIS můžete zadat virtuální cesta, která místo fyzickou cestu, použijte tuto syntaxi:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Následující `aspnet_regiis.exe` šifruje příklad `<connectionStrings>` části pomocí rozhraní DPAPI zprostředkovatele s klíčem úrovni počítače:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Podobně platí `aspnet_regiis.exe` nástroj příkazového řádku je možné dešifrovat konfigurační oddíly funkce. Namísto použití `-pef` přepnout, použijte `-pdf` (nebo namísto něj `-pe`, použijte `-pd`). Všimněte si také, že název zprostředkovatele není nutné k dešifrování.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Protože používáme rozhraní DPAPI poskytovatele, který používá klíče, které jsou specifické pro počítač, je nutné spustit `aspnet_regiis.exe` ze stejného počítače, ve kterém se obsluhuje webové stránky. Například je-li spustit tento program příkazového řádku z místního vývojového počítače a pak nahrajte zašifrovaný soubor Web.config na provozní server, provozní server nebude možné dešifrovat informace o připojovacím řetězci, vzhledem k tomu, že byla přenášena šifrovaná pomocí klíčů konkrétní do svého vývojového počítače. Zprostředkovatel RSA nemá toto omezení, jak je možné exportovat klíčů RSA do jiného počítače.


## <a name="understanding-database-authentication-options"></a>Principy možnosti ověřování databáze

Předtím, než všechny aplikace může vydat `SELECT`, `INSERT`, `UPDATE`, nebo `DELETE` dotazy do databáze Microsoft SQL Server, databázi musíte nejprve identifikovat žadatel. Tento proces se označuje jako *ověřování* a systému SQL Server nabízí dvě metody ověřování:

- **Ověřování Windows** – proces, pod kterým běží aplikace se používá ke komunikaci s databází. Při spuštění aplikace ASP.NET pomocí sady Visual Studio 2005 s serveru ASP.NET Development Server, předpokládá aplikace ASP.NET identity aktuálně přihlášeného uživatele. Pro aplikace ASP.NET na Internet Information Server (IIS), ASP.NET, aplikace obvykle převzít identitu `domainName``\MachineName` nebo `domainName``\NETWORK SERVICE`, i když je možné přizpůsobit.
- **Ověřování SQL** – uživatelské jméno a heslo hodnoty se zadávají jako přihlašovací údaje pro ověřování. Pomocí ověřování SQL ID uživatele a heslo zadané v připojovacím řetězci.

Ověřování Windows je upřednostňována před ověřování SQL, protože je bezpečnější. Pokud webový server a serverem databáze nachází na dvou různých počítačích, přihlašovací údaje se neodesílají přes síť jako prostý text s ověřováním Windows je připojovací řetězec bez uživatelského jména a hesla. Pomocí ověřování SQL přihlašovací údaje pro ověřování jsou však pevně zakódované v připojovacím řetězci a se přenáší z webového serveru databázový server ve formátu prostého textu.

Tyto kurzy využívají ověřování Windows. Můžete zjistit, který režim ověřování je používán kontrola připojovacího řetězce. Připojovací řetězec v `Web.config` byl pro naše kurzy:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True a nedostatečné uživatelské jméno a heslo znamenat, že se používá ověřování Windows. V některých připojovací řetězce termín důvěryhodné připojení = Ano nebo Integrated Security = SSPI se používá namísto Integrated Security = True, ale všechny tři označuje použití ověřování Windows.

Následující příklad ukazuje připojovací řetězec, který používá ověřování SQL. Poznámka: přihlašovací údaje vkládán připojovací řetězec:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Představte si, že útočník je moct zobrazit vaše aplikace s `Web.config` souboru. Pokud používáte ověřování SQL pro připojení k databázi, která je přístupná přes Internet, útočník může použít tento připojovací řetězec pro připojení k databázi SQL Management Studio nebo ze stránky technologie ASP.NET na vlastní web. K zmírnění této hrozby, šifrovat informace o připojovacím řetězci v `Web.config` pomocí systému chráněné konfigurace.

> [!NOTE]
> Další informace o různých typech ověřování, které jsou k dispozici v systému SQL Server najdete v tématu [vytváření bezpečných aplikací technologie ASP.NET: ověřování, autorizaci a zabezpečená komunikace](https://msdn.microsoft.com/library/aa302392.aspx). Další připojovací řetězec příklady ilustrující rozdíly mezi syntaxe ověřování Windows a SQL, najdete v tématu [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Souhrn

Ve výchozím nastavení, soubory s `.config` rozšíření v aplikaci technologie ASP.NET není možné přistupovat prostřednictvím prohlížeče. Tyto typy souborů, nebudou zobrazeny, protože mohou obsahovat citlivé informace, jako jsou databázové připojovací řetězce, uživatelská jména a hesla a tak dále. Chráněné konfigurační systém v rozhraní .NET 2.0 pomáhá rozšířit ochranu citlivých informací tím, že zadaný konfigurační oddíly funkce šifrování. Existují dva poskytovatelé integrované chráněné konfigurace: ten, který používá algoritmus RSA a ten, který používá Windows Data Protection API (DPAPI).

V tomto kurzu jsme se podívali na tom, jak šifrování a dešifrování pomocí zprostředkovatele rozhraní DPAPI nastavení konfigurace. To lze provést prostřednictvím kódu programu, jako jsme viděli v kroku 2, stejně jako prostřednictvím `aspnet_regiis.exe` nástroj příkazového řádku, které bylo popsané v kroku 3. Další informace o používání klíčů individuální nebo místo toho použít zprostředkovatel RSA najdete v materiálech v části Další čtení.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Vytváření aplikace zabezpečené ASP.NET: Ověřování, autorizaci a zabezpečenou komunikaci](https://msdn.microsoft.com/library/aa302392.aspx)
- [Informace o konfiguraci v technologii ASP.NET 2.0 šifrování aplikací](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Šifrování `Web.config` hodnoty v technologii ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí rozhraní DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí technologie RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Konfigurace rozhraní API v rozhraní .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Ochrana dat pro Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Teresy Murphy a Randy Schmidt. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [další](debugging-stored-procedures-vb.md)
