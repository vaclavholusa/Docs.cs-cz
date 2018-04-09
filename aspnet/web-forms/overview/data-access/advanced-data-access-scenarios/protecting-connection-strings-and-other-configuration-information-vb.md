---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Ochrana připojovací řetězce a informace o konfiguraci (VB) | Microsoft Docs
author: rick-anderson
description: Informace o konfiguraci aplikace ASP.NET obvykle ukládá v souboru Web.config. Některé z těchto informací velká a malá písmena a vyžaduje ochrany. Podle def....
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 3372416dd9143afbfd442eaffb39cd807fae0de6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Ochrana připojovací řetězce a informace o konfiguraci (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) nebo [stáhnout PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Informace o konfiguraci aplikace ASP.NET obvykle ukládá v souboru Web.config. Některé z těchto informací velká a malá písmena a vyžaduje ochrany. Ve výchozím nastavení nebude tento soubor zpracovat pro návštěvníka webového serveru, ale správce nebo se hacker může získat přístup k serveru webového systému souborů a zobrazit obsah souboru. V tomto kurzu jsme informace, že technologie ASP.NET 2.0 umožňuje při ochraně citlivých informací šifrování části souboru Web.config.


## <a name="introduction"></a>Úvod

Informace o konfiguraci pro aplikace ASP.NET je obvykle uložen v souboru XML s názvem `Web.config`. V průběhu následujících kurzech jsme aktualizovali `Web.config` několik časy. Při vytváření `Northwind` typové datové sady v [první kurz](../introduction/creating-a-data-access-layer-vb.md), například informace o připojovacím řetězci byl automaticky přidán do `Web.config` v `<connectionStrings>` oddílu. Dále v [hlavní stránky a navigace na webu](../introduction/master-pages-and-site-navigation-vb.md) kurzu ručně aktualizovali jsme `Web.config`, přidávání `<pages>` element značí, všechny stránky ASP.NET v našem projektu by měl použít `DataWebControls` motivu.

Vzhledem k tomu `Web.config` mohou obsahovat citlivá data, jako je například připojovací řetězce, je důležité, který obsah `Web.config` zachovány bezpečné a skrytá z neoprávněné uživatele. Ve výchozím nastavení, všechny HTTP žádost o soubor s `.config` rozšíření se zpracovává souborem modulu ASP.NET, která vrátí hodnotu *tento typ stránky není obsluhovat* zpráva na obrázku 1. To znamená, že nelze zobrazit návštěvníci vašeho `Web.config` soubor s obsahem jednoduše zadáním http://www.YourServer.com/Web.config do panelu Adresa svého prohlížeče s.


[![Návštěvou Web.config prostřednictvím prohlížeče vrátí tento typ stránky není zpracování zpráv](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Obrázek 1**: návštěvou `Web.config` prostřednictvím prohlížeče vrátí tento typ stránky není zpracování zprávy ([Kliknutím zobrazit obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))


Ale co když je možné najít některé zneužití, který mu pomůže zobrazení umožňuje útočník vaší `Web.config` soubor s obsahem? Co může útočník dosáhnout pomocí těchto informací a jak můžete přesměrováni na další ochraně citlivých informací v rámci `Web.config`? Naštěstí většina části `Web.config` neobsahují citlivé informace. Jaké škodu může útočník perpetrate případě, že zná název výchozího motivu, který používá vaše stránky ASP.NET?

Některé `Web.config` části, ale obsahují citlivé informace, které mohou zahrnovat připojovací řetězce, uživatelská jména, hesla, názvů serverů, šifrovacích klíčů a tak dále. Tyto informace se většinou nachází v následujícím `Web.config` částech:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

V tomto kurzu se podíváme techniky pro ochranu konfigurace citlivé informace. Jak zjistíme, zahrnuje rozhraní .NET Framework verze 2.0 chráněné konfigurace systému, který umožňuje programovou šifrování a dešifrování objektu vybrané konfigurační oddíly uloženy.

> [!NOTE]
> V tomto kurzu ukončí Podívejte se na s doporučením společnosti Microsoft pro připojení k databázi z aplikace technologie ASP.NET. Kromě šifrování připojovací řetězce, můžete pomoct posílení systému zajištěním, ke kterému se připojujete k databázi zabezpečené způsobem.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Krok 1: Zkoumat ASP.NET 2.0 s chráněné možnosti konfigurace

Technologie ASP.NET 2.0 obsahuje chráněné konfigurace systému pro šifrování a dešifrování informace o konfiguraci. To zahrnuje metody v rozhraní .NET Framework, které je možné programově šifrování nebo dešifrování informace o konfiguraci. Používá systém chráněné konfigurace [modelu poskytovatelů](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), což vývojářům umožňuje zvolit, jaké implementaci se používá.

Rozhraní .NET Framework se dodává s dva poskytovatelé chráněné konfigurace:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -používá asymetrický [algoritmu RSA](http://en.wikipedia.org/wiki/Rsa) pro šifrování a dešifrování.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -používá Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) pro šifrování a dešifrování.

Vzhledem k tomu, že chráněný konfigurační systém implementuje vzor návrhu poskytovatele, je možné vytvořit vlastního poskytovatele chráněné konfigurace a zapojte jej do vaší aplikace. V tématu [implementace poskytovatele konfigurace chráněné](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) Další informace o tomto procesu.

Poskytovatelé RSA a DPAPI používají klíče pro jejich šifrování a dešifrování rutiny a tyto klíče může být uložený v počítači - nebo -úrovni uživatele. Úroveň počítače klíče jsou ideální pro scénáře, kde je webová aplikace spuštěna na svůj vlastní vyhrazený server nebo pokud je na serveru, který potřebují sdílet více aplikací šifrované informace. Individuální klíče jsou bezpečnější možnost ve sdílených hostitelských prostředích, kde by neměl být schopno dešifrovat vaší aplikace s chráněné konfigurační oddíly jiné aplikace na stejném serveru.

V tomto kurzu použije našich ukázkách DPAPI zprostředkovatele a úroveň počítače klíče. Konkrétně se podíváme na šifrování `<connectionStrings>` kapitoly `Web.config`, i když chráněný konfigurační systém lze použít k šifrování většina žádné `Web.config` části. Informace o používání klíčů individuální nebo pomocí zprostředkovatele RSA najdete v části Další odečty prostředky na konci tohoto kurzu.

> [!NOTE]
> `RSAProtectedConfigurationProvider` a `DPAPIProtectedConfigurationProvider` poskytovatelé jsou zaregistrovány v `machine.config` soubor s názvy zprostředkovatelů `RsaProtectedConfigurationProvider` a `DataProtectionConfigurationProvider`, v uvedeném pořadí. Při šifrování nebo dešifrování informace o konfiguraci, které je potřeba zadat název příslušného poskytovatele (`RsaProtectedConfigurationProvider` nebo `DataProtectionConfigurationProvider`) místo skutečného názvu (`RSAProtectedConfigurationProvider` a `DPAPIProtectedConfigurationProvider`). Můžete najít `machine.config` v soubor `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` složky.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Krok 2: Prostřednictvím kódu programu šifrování a dešifrování konfiguračních oddílů

Zadání několika řádků kódu jsme zašifrovat nebo dešifrovat konkrétní konfigurací oddíl se zadaným zprostředkovatelem. Kód, protože jsme se zobrazí chvíli, jednoduše prostřednictvím kódu programu odkazovat na části odpovídající konfiguraci volat jeho `ProtectSection` nebo `UnprotectSection` metoda a pak volání `Save` metoda a zachová tak změny. Kromě toho zahrnuje rozhraní .NET Framework nástroj užitečné příkazového řádku, který může šifrování a dešifrování informace o konfiguraci. Se podíváme na tento nástroj příkazového řádku v kroku 3.

Pro ilustraci prostřednictvím kódu programu ochranu informace o konfiguraci, umožňují s vytvořit stránku ASP.NET, která obsahuje tlačítka pro šifrování a dešifrování `<connectionStrings>` kapitoly `Web.config`.

Začněte otevřením `EncryptingConfigSections.aspx` stránku `AdvancedDAL` složky. Přetáhněte ovládací prvek textové pole z panelu nástrojů na návrháře nastavení jeho `ID` vlastnost `WebConfigContents`, jeho `TextMode` vlastnost `MultiLine`a jeho `Width` a `Rows` vlastnosti 95 %, 15, v uvedeném pořadí. Tato TextBox – ovládací prvek zobrazí obsah `Web.config` abychom mohli rychle zobrazit, pokud obsah jsou šifrovaná, nebo ne. Samozřejmě v reálné aplikaci byste nikdy chcete zobrazit obsah `Web.config`.

Pod textové pole, přidejte dvou ovládacích prvků tlačítko s názvem `EncryptConnStrings` a `DecryptConnStrings`. Nastavte vlastnosti jejich Text připojovací řetězce šifrování a dešifrování řetězce připojení.

Obrazovky v tomto okamžiku by měla vypadat podobně jako na obrázku 2.


[![Přidat textové pole a dvou tlačítko webových ovládacích prvků na stránku](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Obrázek 2**: Přidání textového pole a dvou tlačítko webových ovládacích prvků na stránku ([Kliknutím zobrazit obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))


Dále je potřeba psát kód, který načte a zobrazí obsah `Web.config` v `WebConfigContents` načíst TextBox po první stránky. Přidejte následující kód do třídy stránky s kódem v pozadí. Tento kód přidá metodu s názvem `DisplayWebConfig` a volá z `Page_Load` obslužné rutiny události při `Page.IsPostBack` je `False`:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

`DisplayWebConfig` Metoda používá [ `File` – třída](https://msdn.microsoft.com/library/system.io.file.aspx) otevřete aplikaci s `Web.config` souboru [ `StreamReader` – třída](https://msdn.microsoft.com/library/system.io.streamreader.aspx) načíst jeho obsah do řetězce a [ `Path` třída](https://msdn.microsoft.com/library/system.io.path.aspx) ke generování fyzickou cestu k `Web.config` souboru. Tyto tři třídy se nacházejí v [ `System.IO` obor názvů](https://msdn.microsoft.com/library/system.io.aspx). V důsledku toho budete muset přidat `Imports``System.IO` příkaz do horní části kódu třídu nebo, případně tyto třídy názvy s předponou `System.IO.`

Dále je potřeba přidat obslužných rutin událostí pro dvě tlačítka `Click` události a přidání nezbytného kódu k šifrování a dešifrování `<connectionStrings>` části klíč úrovni počítače pomocí rozhraní DPAPI zprostředkovatele. Z návrháře, dvakrát klikněte na každou z tlačítka pro přidání `Click` obslužné rutiny událostí v modelu code-behind třídu a přidejte následující kód:


[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Kód použitý v obslužné rutině dvě události je téměř shodná. Obě začít nastavením informace o aktuální aplikaci s `Web.config` souboru prostřednictvím [ `WebConfigurationManager` třída](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` metoda](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Tato metoda vrátí konfigurační soubor webu pro zadanou virtuální cestu. Dále `Web.config` soubor s `<connectionStrings>` části přistupuje prostřednictvím [ `Configuration` – třída](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), která vrátí [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) objektu.

`ConfigurationSection` Objekt zahrnuje [ `SectionInformation` vlastnost](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) poskytuje další informace a funkce týkající se konfigurační oddíl. Jako kód výš ukazuje, můžeme určit, zda je konfigurační oddíl zašifrována kontrolou `SectionInformation` vlastnosti s `IsProtected` vlastnost. Kromě toho můžete v části šifrování a dešifrování prostřednictvím `SectionInformation` vlastnosti s `ProtectSection(provider)` a `UnprotectSection` metody.

`ProtectSection(provider)` Metoda akceptuje jako vstupní řetězec určující název zprostředkovatele chráněné konfigurace použít při šifrování. V `EncryptConnString` jsme předat DataProtectionConfigurationProvider do obslužné rutiny události tlačítko s `ProtectSection(provider)` metoda tak, aby se používá rozhraní DPAPI zprostředkovatele. `UnprotectSection` Metoda můžete určit zprostředkovatele, který se používá k šifrování konfiguračního oddílu a proto nevyžaduje žádné vstupní parametry.

Po volání `ProtectSection(provider)` nebo `UnprotectSection` metodu, musí volat `Configuration` objekt s [ `Save` metoda](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) a zachová tak změny. Jakmile informace o konfiguraci byla zašifrovaná nebo dešifrovat a změny uložit, říkáme `DisplayWebConfig` načíst aktualizovaný `Web.config` obsah do ovládacího prvku textového pole.

Po výše uvedeném kódu, které jste zadali, otestovat ji navštivte stránky `EncryptingConfigSections.aspx` stránku prostřednictvím prohlížeče. Nejdřív měli vidět stránky, který uvádí obsah `Web.config` s `<connectionStrings>` části zobrazit v prostém textu (viz obrázek 3).


[![Přidat textové pole a dvou tlačítko webových ovládacích prvků na stránku](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Obrázek 3**: Přidání textového pole a dvou tlačítko webových ovládacích prvků na stránku ([Kliknutím zobrazit obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))


Nyní klikněte na tlačítko šifrování připojovací řetězce. Pokud je povoleno ověření požadavku, kód vrácena zpět `WebConfigContents` vytvoří textové pole `HttpRequestValidationException`, který zobrazí zprávu, potenciálně nebezpečná `Request.Form` byla zjištěna hodnota z klienta. Ověření žádosti, která je povolena ve výchozím nastavení v technologii ASP.NET 2.0, zakáže zpětná volání, které zahrnují bez kódování HTML a je navržená tak, aby se zabránilo útokům vložení skriptu. Tato kontrola se dá vypnout na stránce - nebo -úrovni aplikace. Chcete-li vypnout pro tuto stránku, nastavte `ValidateRequest` nastavení `False` v `@Page` – direktiva. `@Page` – Direktiva se nachází v horní části stránky s deklarativní.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Další informace o ověření žádosti, jeho účel, zakažte ho na stránku - a -úrovni aplikace, jak dobře, jak do formátu HTML kódování značek najdete v tématu [ověření žádosti - prevence útoků skriptu](../../../../whitepapers/request-validation.md).

Po zakázání ověření žádosti pro stránku, zkuste to znovu kliknutím na tlačítko šifrování připojovací řetězce. Na zpětné volání, budou mít přístup konfiguračního souboru a jeho `<connectionStrings>` části šifrovat pomocí rozhraní DPAPI zprostředkovatele. Do textového pole se pak zaktualizuje a zobrazí nový `Web.config` obsah. Jak ukazuje obrázek 4, `<connectionStrings>` nyní jsou informace zašifrované.


[![Kliknutím šifrování připojení řetězce tlačítko šifruje &lt;connectionString&gt; části](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Obrázek 4**: Kliknutím šifrování připojení řetězce tlačítko šifruje `<connectionString>` část ([Kliknutím zobrazit obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))


Šifrované `<connectionStrings>` části vygenerované v mém počítači následuje, i když některé z obsah `<CipherData>` element byla odebrána jako stručný výtah:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> `<connectionStrings>` Element určuje zprostředkovatele, který používá k provádění šifrování (`DataProtectionConfigurationProvider`). Tyto informace používá `UnprotectSection` metoda při kliknutí na tlačítko dešifrování řetězce připojení.


Když se získají informace o připojovacím řetězci z `Web.config` – buď kód jsme zápis z ovládacího prvku SqlDataSource nebo automaticky generovaný kód z TableAdapters v naší datové sady zadali - jsou automaticky dešifrována. Stručně řečeno, jsme nemusíte přidávat žádné další kód nebo logiku dešifrovat šifrované `<connectionString>` části. Ukazuje to navštíví některý z dřívějších kurzy v tuto chvíli, jako je například kurzu jednoduchého zobrazení z části vytváření základních sestav (`~/BasicReporting/SimpleDisplay.aspx`). Jak je vidět na obrázku 5, kurz funguje přesně tak, jak by Očekáváme, která udává, že se informace o řetězce šifrované připojení automaticky dešifruje podle stránky ASP.NET.


[![Data Access Layer automaticky dešifruje informace o připojovacím řetězci](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Obrázek 5**: Data Access Layer automaticky dešifruje informace o připojovacím řetězci ([Kliknutím zobrazit obrázek v plné velikosti](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))


Vrátit zpátky `<connectionStrings>` části zpět na reprezentaci formátu prostého textu, klikněte na tlačítko dešifrování řetězce připojení. Na zpětné volání byste měli vidět připojovací řetězce v `Web.config` ve formátu prostého textu. V tomto okamžiku by měl svou obrazovku vypadaly jako při první návštěvě této stránky (viz obrázek 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Krok 3: Šifrování pomocí konfiguračních oddílů`aspnet_regiis.exe`

Zahrnuje celou řadu nástrojů příkazového řádku v rozhraní .NET Framework `$WINDOWS$\Microsoft.NET\Framework\version\` složky. V [pomocí závislosti mezipaměti SQL](../caching-data/using-sql-cache-dependencies-vb.md) kurzu, například jsme se podívali na pomocí `aspnet_regsql.exe` nástroj příkazového řádku přidat infrastruktury potřebné pro závislosti mezipaměti SQL. Dalším nástrojem užitečné příkazový řádek v této složce je [nástroje ASP.NET IIS Registration (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Jak již název napovídá, nástroj ASP.NET IIS Registration slouží především zaregistrovat technologii ASP.NET 2.0 aplikaci s Microsoft s profesionální webového serveru IIS. Kromě funkcí jeho souvisí se službou IIS, nástroje ASP.NET IIS Registration lze také se zašifrovat nebo dešifrovat zadaný konfigurační oddíly funkce v `Web.config`.

Následující příkaz ukazuje obecnou syntaxi použitý k šifrování konfigurační oddíl s `aspnet_regiis.exe` nástroj pro příkazový řádek:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

*část* je konfigurační oddíl k šifrování (např. connectionStrings), *fyzické\_directory* je úplnou fyzickou cestu k adresáři webové aplikace s kořenové a *zprostředkovatele*  je název poskytovatele chráněné konfigurace, které chcete použít (například DataProtectionConfigurationProvider). Případně pokud webová aplikace je zaregistrován ve službě IIS můžete zadat virtuální cestu místo k fyzické cestě pomocí následující syntaxe:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Následující `aspnet_regiis.exe` příklad šifruje `<connectionStrings>` část pomocí poskytovatele rozhraní DPAPI klíčem úrovni počítače:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Podobně `aspnet_regiis.exe` nástroj příkazového řádku lze použít k dešifrování konfigurační oddíly. Místo použití `-pef` přepínače, použijte `-pdf` (nebo místo `-pe`, použijte `-pd`). Všimněte si také, že název zprostředkovatele není nezbytné k dešifrování.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Vzhledem k tomu, že se používá rozhraní DPAPI poskytovatele, který používá klíče, které jsou specifické pro počítač, je nutné spustit `aspnet_regiis.exe` ze stejného počítače, ze které jsou obsluhovány webové stránky. Například pokud jste spuštění tohoto programu Příkazový řádek z místní vývojovém počítači a pak nahrajte zašifrovaný soubor Web.config na provozním serveru, na provozním serveru nebude moci dešifrovat informace o připojovacím řetězci, protože byla zašifrována použití klíče, které jsou specifické pro vývojovém počítači. Zprostředkovatel RSA nemá toto omezení, jako je možné exportovat klíče RSA k jinému počítači.


## <a name="understanding-database-authentication-options"></a>Principy možnosti ověřování databáze

Předtím, než můžete vydat žádnou aplikaci `SELECT`, `INSERT`, `UPDATE`, nebo `DELETE` dotazy do databáze Microsoft SQL Server, databáze musí nejprve identifikovat žadatel. Tento proces se označuje jako *ověřování* a SQL serveru nabízí dvě metody ověřování:

- **Ověřování systému Windows** -proces, pod kterým běží aplikace se používá ke komunikaci s databází. Při spuštění aplikace ASP.NET pomocí sady Visual Studio 2005 s vývojový Server ASP.NET, předpokládá aplikace ASP.NET identity aktuálně přihlášeného uživatele. Pro aplikace ASP.NET na Internet Information Server (IIS), aplikace ASP.NET obvykle převzít identitu `domainName``\MachineName` nebo `domainName``\NETWORK SERVICE`, i když to lze přizpůsobit.
- **Ověřování SQL** -uživatelské ID a heslo hodnot jako pověření pro ověření. Pomocí ověřování SQL ID uživatele a heslo zadané v připojovacím řetězci.

Ověřování systému Windows je preferovaná přes ověřování SQL, protože je bezpečnější. Pomocí ověřování systému Windows je připojovací řetězec bez zadání uživatelského jména a hesla a pokud webový server a databázový server nacházet na dvou různých počítačů, nejsou přihlašovací údaje odeslány prostřednictvím sítě ve formátu prostého textu. Pomocí ověřování SQL ale ověřovací pověření jsou pevně zakódovaná v připojovacím řetězci a z webového serveru, se přenáší do databázový server ve formátu prostého textu.

Tyto kurzy použili ověřování systému Windows. Můžete zjistit, jaké režimu ověřování používá zkontrolováním připojovací řetězec. Připojovací řetězec v `Web.config` pro naše kurzy byl:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Integrated Security = True a nedostatečná uživatelské jméno a heslo indikovat, že používá ověřování systému Windows. V některých připojovací řetězce termín důvěryhodné připojení = Yes nebo integrované zabezpečení = SSPI se používá namísto integrované zabezpečení = True, ale všechny tři indikovat použití ověřování systému Windows.

Následující příklad ukazuje připojovací řetězec, který používá ověřování SQL. Poznámka: přihlašovací údaje vkládán připojovací řetězec:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Představte si, že útočník je moct zobrazit vaše aplikace s `Web.config` souboru. Pokud používáte ověřování SQL pro připojení k databázi, která je přístupná prostřednictvím Internetu, může útočník použít tento připojovací řetězec pro připojení k databázi pomocí aplikace SQL Management Studio nebo ze stránky ASP.NET na vlastní web. K zmírnění této hrozby, šifrování informace o připojovacím řetězci v `Web.config` pomocí chráněného konfiguračního systému.

> [!NOTE]
> Další informace o různých typech ověřování, které jsou k dispozici v systému SQL Server najdete v tématu [vytváření zabezpečení aplikací ASP.NET: ověřování, autorizace a zabezpečená komunikace](https://msdn.microsoft.com/library/aa302392.aspx). Další připojovací řetězec příklady ilustrující rozdíly mezi syntaxe pro ověřování systému Windows a SQL, najdete v části [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Souhrn

Ve výchozím nastavení, soubory s `.config` rozšíření aplikace technologie ASP.NET není přístupný prostřednictvím prohlížeče. Tyto typy souborů, nebudou zobrazeny, protože mohou obsahovat citlivé informace, například databázové připojovací řetězce, uživatelská jména a hesla a tak dále. Chráněné konfigurační systém v rozhraní .NET 2.0 pomáhá tím, že zadaný konfigurační oddíly pro zašifrování dál chránit citlivé informace. Existují dva poskytovatelé předdefinované chráněné konfigurace: jeden, který používá algoritmus RSA a jeden, který používá rozhraní Windows Data Protection API (DPAPI).

V tomto kurzu jsme se podívali na to, jak k šifrování a dešifrování pomocí poskytovatele rozhraní DPAPI nastavení konfigurace. Můžete to provést prostřednictvím kódu programu, jako jsme viděli v kroku 2, stejně jako prostřednictvím `aspnet_regiis.exe` nástroj příkazového řádku, které bylo popsané v kroku 3. Další informace o používání klíčů individuální nebo místo toho použít zprostředkovatel RSA najdete v materiálech v části Další čtení.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Vytváření aplikace zabezpečené ASP.NET: Ověřování, autorizace a zabezpečenou komunikaci](https://msdn.microsoft.com/library/aa302392.aspx)
- [Šifrování informací o konfiguraci technologie ASP.NET 2.0 aplikace](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Šifrování `Web.config` hodnoty v technologii ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí rozhraní DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Postupy: Šifrování konfigurační oddíly funkce v technologii ASP.NET 2.0 pomocí RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [Rozhraní API konfigurace v rozhraní .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Ochrana dat systému Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Teresy Murphy a Randy Schmidt. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [další](debugging-stored-procedures-vb.md)
