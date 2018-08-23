---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profily, motivy a webové části | Dokumentace Microsoftu
author: microsoft
description: Byly zjištěny hlavní změny v konfiguraci a instrumentace v technologii ASP.NET 2.0. Nové rozhraní API technologie ASP.NET konfigurace umožňuje provést změny konfigurace má být provedeno žádosti o přijetí změn...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 98559c2a378c72bc5664faafe5436753050b574f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752233"
---
<a name="profiles-themes-and-web-parts"></a>Profily, motivy a webové části
====================
podle [Microsoft](https://github.com/microsoft)

> Byly zjištěny hlavní změny v konfiguraci a instrumentace v technologii ASP.NET 2.0. Nové rozhraní API technologie ASP.NET konfigurace umožňuje změny konfigurace provedli programově. Kromě toho existují spoustu nových nastavení konfigurace pro nové konfigurace a instrumentace.


ASP.NET 2.0 představuje významné zlepšení v oblasti přizpůsobené weby. Kromě funkcí členství weve popsány ASP.NET profily, motivy a webové části výrazně zlepšuje přizpůsobení webových stránek.

## <a name="aspnet-profiles"></a>Profilů technologie ASP.NET

Profilů technologie ASP.NET jsou podobné relacím. Rozdíl je, že profil je trvalé vzhledem k tomu dojde ke ztrátě relace, při zavření prohlížeče. Další rozdíl mezi relacemi a profily je, že jsou profily silného typu, a to proto vám poskytnou IntelliSense během procesu vývoje.

Profil je definována v konfiguračním souboru počítače nebo soubor web.config pro aplikaci. (Nelze definovat profil v souboru web.config podsložek.) Následující kód definuje profil, který nejprve uložit návštěvníků webové stránky a příjmení.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Výchozí datový typ pro vlastnost profilu je System.String. V předchozím příkladu byl zadán typ žádná data. Proto vlastnosti FirstName a LastName jsou typu řetězec. Jak už jsme zmínili, profilu, které vlastnosti jsou silného typu. Následující kód přidává novou vlastnost pro stáří, která je typu Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profily se obecně používají s ověřování pomocí formulářů ASP.NET. Když použijete v kombinaci s ověřování pomocí formulářů, každý uživatel má samostatný profil přidružený k jejich ID uživatele. Ale je také možné povolit použití profilů v anonymní aplikace s využitím &lt;anonymousIdentification&gt; element v konfiguračním souboru spolu s **allowAnonymous** atribut jako vidíte níže:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Pokud anonymní uživatel prohlíží web, ASP.NET vytvoří instanci **ProfileCommon** pro daného uživatele. Tento profil používá k identifikaci uživatele jako datový typ jedinečných návštěvníků jedinečné ID v souborech cookie v prohlížeči. Tímto způsobem můžete ukládat informace o profilu pro uživatele, kteří se anonymně procházení.

## <a name="profile-groups"></a>Profil skupiny

Je možné vlastnosti skupiny profilů. Seskupení vlastnostmi je možné simulovat více profilů pro danou aplikaci.

Následující konfigurace konfiguruje vlastnost FirstName a LastName pro dvě skupiny. Odběrateli a potenciální zákazníky.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Potom je možné nastavit vlastnosti na konkrétní skupinu následujícím způsobem:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Ukládání komplexních objektů

Příklady, které jsme si popsali zatím jste uložili jednoduché datové typy v profilu. Je také možné ukládat komplexních datových typů v profilu tak, že určíte metodu pomocí serializace **serializeAs** atribut následujícím způsobem:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

V tomto případě je typ PurchaseInvoice. Třída PurchaseInvoice musí být označen jako serializovatelný a může obsahovat libovolný počet vlastností. Například, pokud má vlastnost s názvem PurchaseInvoice **NumItemsPurchased**, můžete odkazovat na tuto vlastnost v kódu následujícím způsobem:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil dědičnosti

Je možné k vytvoření profilu pro použití ve více aplikacích. Vytvoření třídy profil, který je odvozen z ProfileBase, můžete využít profil v několika aplikací s použitím **dědí** atributu, jak je znázorněno níže:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

V tomto případě, třída **PurchasingProfile** by vypadalo takto:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Poskytovatelé profilů

Pomocí profilů ASP.NET podle modelu poskytovatele. Výchozí zprostředkovatel je ukládá informace v databázi serveru SQL Server Express v aplikaci\_složky dat webové aplikace pomocí zprostředkovatele SqlProfileProvider. Pokud databáze neexistuje, ASP.NET jej automaticky vytvoří při k ukládání informací o profilu.

V některých případech však můžete chtít vytvořit vlastní profil zprostředkovatele. Funkce profilu ASP.NET umožňuje snadno používat různí poskytovatelé.

Vytvoření vlastního poskytovatele profilu při:

- Potřebujete k ukládání informací o profilu ve zdroji dat, jako je databáze aplikace FoxPro nebo v databázi Oracle, který není podporován zprostředkovateli profilu rozhraní .NET Framework.
- Potřebujete spravovat informace o profilu pomocí schématu databáze, které se liší od schématu databáze používané zprostředkovatelů obsažených v rozhraní .NET Framework. Běžným příkladem je, že chcete integrovat informace o profilu se uživatelská data ve stávající databázi systému SQL Server.

### <a name="required-classes"></a>Požadované třídy

Pro implementaci zprostředkovatele profil, vytvořte třídu, která dědí z abstraktní třídy System.Web.Profile.ProfileProvider. **ProfileProvider** abstraktní třída dědí zase System.Configuration.SettingsProvider abstraktní třída, která dědí z abstraktní třídy System.Configuration.Provider.ProviderBase. Z důvodu tohoto řetězce dědičnosti, kromě požadovaných členů **ProfileProvider** třídy, musí implementovat požadované členy **SettingsProvider** a  **ProviderBase** třídy.

Následující tabulka popisuje vlastnosti a metody, které je nutné implementovat z **ProviderBase**, **SettingsProvider**, a **ProfileProvider** abstraktní třídy.

### <a name="providerbase-members"></a>Členové ProviderBase

| **Člen** | **Popis** |
| --- | --- |
| Initialize – metoda | Přijímá jako vstup jméno instance zprostředkovatele a NameValueCollection nastavení konfigurace. Umožňuje nastavit možnosti a hodnoty vlastností pro instanci zprostředkovatele, včetně hodnot specifický pro implementaci a možnosti zadané v konfiguraci počítače nebo v souboru Web.config. |

### <a name="settingsprovider-members"></a>Členové SettingsProvider

| **Člen** | **Popis** |
| --- | --- |
| Vlastnost ApplicationName | Název aplikace, která je uložena každý profil. Zprostředkovatel profilu název aplikace používá k ukládání informací o profilu pro každou aplikaci zvlášť. To umožňuje více aplikací prostředí ASP.NET má použít stejný zdroj dat bez konflikt, pokud stejné uživatelské jméno se vytvoří v různých aplikacích. Alternativně můžete sdílet více aplikací prostředí ASP.NET zdroji dat profilu tak, že zadáte stejný název aplikace. |
| GetPropertyValues – metoda | Přijímá jako vstup SettingsContext a SettingsPropertyCollection objektu. **SettingsContext** poskytuje informace o uživateli. Informace můžete použít jako primární klíč pro načtení vlastnosti profilu uživatele. Použití **SettingsContext** můžete získat uživatelské jméno a zda je uživatel ověřený nebo anonymní. **SettingsPropertyCollection** obsahuje kolekci objektů SettingsProperty. Každý **SettingsProperty** objekt, který poskytuje název a typ vlastnosti, jakož i další informace, jako je výchozí hodnota pro vlastnost a určuje, zda je vlastnost jen pro čtení. **GetPropertyValues** metoda naplní SettingsPropertyValue objekty na základě SettingsPropertyValueCollection **SettingsProperty** objekty doplníte jako vstup. Hodnoty ze zdroje dat pro určeného uživatele jsou přiřazeny novým vlastnostem PropertyValue pro každou **SettingsPropertyValue** je vrácen objekt a celou kolekci. Volání metody také aktualizuje LastActivityDate hodnotu pro zadaný uživatelský profil k aktuálnímu datu a času. |
| SetPropertyValues – metoda | Přijímá jako vstup **SettingsContext** a **SettingsPropertyValueCollection** objektu. **SettingsContext** poskytuje informace o uživateli. Informace můžete použít jako primární klíč pro načtení vlastnosti profilu uživatele. Použití **SettingsContext** můžete získat uživatelské jméno a zda je uživatel ověřený nebo anonymní. **SettingsPropertyValueCollection** obsahuje kolekci **SettingsPropertyValue** objekty. Každý **SettingsPropertyValue** objekt, který poskytuje název, typ a hodnotu vlastnosti, jakož i další informace, jako je výchozí hodnota pro vlastnost a určuje, zda je vlastnost jen pro čtení. **SetPropertyValues** metoda aktualizuje hodnoty vlastností profilu ve zdroji dat pro určeného uživatele. Volání metody také aktualizuje **LastActivityDate** a LastUpdatedDate hodnot pro zadaný uživatelský profil k aktuálnímu datu a času. |

### <a name="profileprovider-members"></a>Objekt ProfileProvider s členy

| **Člen** | **Popis** |
| --- | --- |
| DeleteProfiles – metoda | Přijímá jako vstup uživatele pole řetězců názvů a odstraní ze zdroje dat všechny profil informace a hodnoty vlastnosti pro zadané názvy, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. Pokud váš zdroj dat podporuje transakce, doporučujeme zahrnout všechny operace odstranění transakce a transakce vrátit zpět a vyvolání výjimky, pokud všechny operace odstranění se nezdaří. |
| DeleteProfiles – metoda | Vezme jako vstupní údaje kolekce ProfileInfo objekty a odstraní ze zdroje dat všechny profil informace a hodnoty vlastnosti pro každý profil, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. Pokud zdroj dat podporuje transakce, doporučujeme zahrnout všechny operace odstranění transakce a transakce vrátit zpět a vyvolat výjimku, pokud se některé operace odstranění nezdaří. |
| DeleteInactiveProfiles – metoda | Přijímá jako vstupní hodnota ProfileAuthenticationOption a všechny informace o profilu zdrojového objektu data a času a odstraní z dat a hodnot vlastností, kde menší nebo rovna zadané datum a čas je datum poslední aktivity a název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo všechny profily mají být odstraněny. Pokud zdroj dat podporuje transakce, doporučujeme zahrnout všechny operace odstranění transakce a transakce vrátit zpět a vyvolat výjimku, pokud se některé operace odstranění nezdaří. |
| GetAllProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnota, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profilů. Vrátí ProfileInfoCollection, který obsahuje **ProfileInfo** objekty pro všechny profily ve zdroji dat, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo všechny profily mají být vráceny. Výsledky vrácené **GetAllProfiles** metody jsou omezeny na index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty vrátit v **ProfileInfoCollection**. Hodnota indexu stránky určuje kterou stránku výsledků k vrácení, kde 1 označuje první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), který je nastaven na celkový počet profilů. Například, pokud úložiště dat obsahuje 13 profily pro aplikace a hodnota indexu stránky je 2 a 5, velikost stránky **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Hodnota pro celkový počet záznamů je nastavena na 13 po návratu metody. |
| GetAllInactiveProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnotu, **data a času** objekt, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které se nastaví Celkový počet profilů. Vrátí **ProfileInfoCollection** obsahující **ProfileInfo** objekty pro všechny profily ve zdroji dat, kde datum poslední aktivity je menší než nebo rovno zadanému **data a času**  a kde odpovídá názvu aplikace **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo všechny profily mají být vráceny. Výsledky vrácené **GetAllInactiveProfiles** metody jsou omezeny na index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty vrátit v **ProfileInfoCollection**. Hodnota indexu stránky určuje kterou stránku výsledků k vrácení, kde 1 označuje první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), který je nastaven na celkový počet profilů. Například, pokud úložiště dat obsahuje 13 profily pro aplikace a hodnota indexu stránky je 2 a 5, velikost stránky **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Hodnota pro celkový počet záznamů je nastavena na 13 po návratu metody. |
| FindProfilesByUserName – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnota, řetězec obsahující uživatelské jméno, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profily. Vrátí **ProfileInfoCollection** obsahující **ProfileInfo** objekty pro všechny profily v datech zdroj, kde uživatelské jméno odpovídá zadanému uživatelskému jménu a kde odpovídá názvu aplikace **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo všechny profily mají být vráceny. Pokud váš zdroj dat podporuje další vyhledávací funkce, jako je například zástupných znaků, zajistíte rozsáhlejší možnosti vyhledávání uživatelských jmen. Výsledky vrácené **FindProfilesByUserName** metody jsou omezeny na index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty vrátit v **ProfileInfoCollection**. Hodnota indexu stránky určuje kterou stránku výsledků k vrácení, kde 1 označuje první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), který je nastaven na celkový počet profilů. Například, pokud úložiště dat obsahuje 13 profily pro aplikace a hodnota indexu stránky je 2 a 5, velikost stránky **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Hodnota pro celkový počet záznamů je nastavena na 13 po návratu metody. |
| FindInactiveProfilesByUserName – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnota, řetězec obsahující uživatelské jméno **data a času** objekt, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profilů. Vrátí **ProfileInfoCollection** , která obsahuje **ProfileInfo** objekty pro všechny profily ve zdroji dat. Pokud uživatelské jméno odpovídá zadanému uživatelskému jménu, kde datum poslední aktivity je menší než nebo rovná zadanému **data a času**, a kde odpovídá názvu aplikace **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo všechny profily mají být vráceny. Pokud váš zdroj dat podporuje další vyhledávací funkce, jako je například zástupných znaků, zajistíte rozsáhlejší možnosti vyhledávání uživatelských jmen. Výsledky vrácené **FindInactiveProfilesByUserName** metody jsou omezeny na index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty vrátit v **ProfileInfoCollection**. Hodnota indexu stránky určuje kterou stránku výsledků k vrácení, kde 1 označuje první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), který je nastaven na celkový počet profilů. Například, pokud úložiště dat obsahuje 13 profily pro aplikace a hodnota indexu stránky je 2 a 5, velikost stránky **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Hodnota pro celkový počet záznamů je nastavena na 13 po návratu metody. |
| GetNumberOfInActiveProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnotu a **data a času** objekt a vrátí počet všech profilů ve zdroji dat, kde menší nebo rovna zadané jedatumposledníaktivity **Datum a čas** a kde odpovídá názvu aplikace **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda jenom anonymní profily jenom ověřené profily, nebo které se mají spočítat všechny profily. |

### <a name="applicationname"></a>ApplicationName

Protože poskytovatelích profilů ukládání informací o profilu odděleně pro každou aplikaci, musíte zajistit, že vaše datové schéma obsahuje název aplikace a že dotazů a aktualizace také obsahovat název aplikace. Například následující příkaz se používá k načtení hodnoty vlastnosti z databáze na základě uživatelského jména a určuje, zda je k anonymní profil a zajišťuje, že **ApplicationName** hodnota je obsažena v dotazu.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Motivů ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Co jsou motivů ASP.NET 2.0?

Jedním z nejdůležitějších aspektů webovou aplikaci v lokalitě je konzistentní vzhled a chování. ASP.NET 1.x vývojáři obvykle pomocí stylů CSS (Cascading Style) k implementaci konzistentní vzhled a chování. ASP.NET 2.0 motivů CSS výrazně vylepšují, protože technologie ASP.NET pro vývojáře poskytují možnost definovat vzhled serverové ovládací prvky ASP.NET, jakož i prvky jazyka HTML. Motivů ASP.NET můžete použít u jednotlivých ovládacích prvků, konkrétní webovou stránku nebo celé webové aplikace. Motivy pomocí kombinace souborů šablon stylů CSS, soubor volitelné vzhledu a volitelné adresář bitové kopie, potřeby bitové kopie. Soubor skinu řídí vzhled serverové ovládací prvky ASP.NET.

## <a name="where-are-themes-stored"></a>Kde jsou uloženy motivy?

Umístění, kde jsou uložené motivů se liší podle jejich oboru. Motivy, které lze použít u všech aplikací jsou uloženy v následující složce:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Motiv, který je specifický pro konkrétní aplikace je uložena v `App\_Themes\<Theme\_Name>` adresáře v kořenové složce webu.

> [!NOTE]
> Soubor skinu by měl změnit pouze vlastnosti ovládacího prvku serveru, které ovlivňují vzhled.

Jako globální motiv se motiv, který lze použít na všechny aplikace nebo web běžící na webovém serveru. Tato témata jsou uložené ve výchozím nastavení v adresáři ASP.NETClientfiles\Themes, který je v adresáři v2.x.xxxxx. Alternativně můžete přesunout soubory motivu do aspnet\_klienta nebo systému\_web / /Themes/ [version] [motiv\_název] složky v kořenové složce webu.

Motivy specifické pro aplikaci může používat jedině pro aplikace, ve kterém jsou umístěny soubory. Tyto soubory se ukládají v `App\_Themes/<theme\_name>` adresáře v kořenové složce webu.

## <a name="the-components-of-a-theme"></a>Součásti motiv

Motiv tvoří jeden nebo více souborů CSS, soubor volitelné vzhledu a volitelnou složku Obrázky. Soubory šablon stylů CSS, může být jakýkoli název, aby (tj. default.css nebo theme.css atd.) a musí být v kořenovém adresáři motivů. Soubory šablon stylů CSS se používají k definování běžné třídy šablony stylů CSS a atributy pro konkrétní selektory. Použití jedné ze tříd šablon stylů CSS prvku stránky **CSSClass** vlastnost se používá.

Soubor skinu je soubor XML, který obsahuje definice vlastností pro serverové ovládací prvky ASP.NET. Kód uvedený níže je příklad souboru vzhledu.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Obrázek 1** níže ukazuje malé stránky ASP.NET procházet bez použije motiv. **Obrázek 2** ukazuje stejný soubor se použije motiv. Barva pozadí a barvu textu se konfigurují prostřednictvím souboru šablon stylů CSS. Vzhled na tlačítko a textové pole jsou nakonfigurované pomocí souboru skinu uvedené výše.


![Žádné motiv](profiles-themes-and-web-parts/_static/image1.gif)

**Obrázek 1**: žádné motiv


![Motiv](profiles-themes-and-web-parts/_static/image2.gif)

**Obrázek 2**: motiv


Soubor skinu výše uvedených definuje výchozí vzhled všech ovládacích prvků textového pole a ovládací prvky tlačítek. To znamená, že bude trvat každý ovládací prvek textového pole a ovládací prvek tlačítko Vložit na stránku na tento vzhled. Můžete také definovat vzhled, který lze použít ke konkrétním instancím těchto ovládacích prvků pomocí **SkinID** vlastnost ovládacího prvku.

Následující kód definuje skinu pro ovládací prvek tlačítko. Pouze tlačítko – ovládací prvky s **SkinID** vlastnost **goButton** bude trvat na vzhled vzhledu.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Můžete mít jenom jeden výchozí vzhled za typ ovládacího prvku serveru. Pokud budete potřebovat další vzhledy, by měl použít vlastnost identifikátor SkinID.

## <a name="applying-themes-to-pages"></a>Aplikování motivů na stránky

Motiv lze aplikovat pomocí některého z následujících metod:

- V &lt;stránky&gt; element v souboru web.config
- V @Page direktivy stránky
- Prostřednictvím kódu programu

## <a name="applying-a-theme-in-the-configuration-file"></a>Použití motivu v konfiguračním souboru

Použít motiv v konfiguračním souboru aplikace, použijte následující syntaxi:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Název motivu, který je tady zadané, musí odpovídat názvu složku motivů. Tato složka může existovat v některém z umístění uvedeného dříve v tomto kurzu. Pokud se pokusíte použít motiv, který neexistuje, dojde k chybě konfigurace.

## <a name="applying-a-theme-in-the-page-directive"></a>Použití motivu v direktivě stránky

Můžete také použít v Direktiva @ Page motiv. Tato metoda umožňuje použít motiv konkrétní stránky.

Použít motiv v @Page směrnice, použijte následující syntaxi:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Znovu zde zadaný motiv musí odpovídat složce s motivy, jak již bylo zmíněno dříve. Pokud se pokusíte použít motiv, který neexistuje, dojde k selhání sestavení. Visual Studio také zvýrazněte atribut a oznámit, že neexistuje žádný takový motiv.

## <a name="applying-a-theme-programmatically"></a>Použití motivu prostřednictvím kódu programu

Použít motiv prostřednictvím kódu programu, je nutné zadat **motiv** vlastnost na stránce **stránky\_PreInit** metoda.

Chcete-li použít motiv prostřednictvím kódu programu, použijte následující syntaxi:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Je nutné použít motiv v metodě PreInit kvůli životní cyklus stránky. Když je použijete po tomto bodu, motivu stránky se již byly implementovány modulem runtime a změny v tomto okamžiku je příliš pozdě v životní cyklus. Pokud použijete motiv, který neexistuje, **HttpException** vyvolá. Při použití motivu prostřednictvím kódu programu, pokud žádné serverové ovládací prvky mají vlastnost SkinID zadaný dojde upozornění sestavení. Toto upozornění účelem je informovat je, že žádné deklarativně motivu a může být ignorována.

## <a name="exercise-1--applying-a-theme"></a>Cvičení 1: Použití motivu

V tomto cvičení použijete motivu ASP.NET na web.

> [!IMPORTANT]
> Pokud používáte Microsoft Word pro zadání informací do souboru skinu, ujistěte se, že nejsou běžné uvozovky nahradíte inteligentní uvozovky. Inteligentní uvozovky budou způsobit problémy s souborech skinů.

1. Vytvoření nového webu technologie ASP.NET.
2. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte Přidat novou položku.
3. Ze seznamu souborů vyberte soubor webové konfigurace a klikněte na tlačítko Přidat.
4. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte Přidat novou položku.
5. Zvolte soubor vzhledu a klikněte na tlačítko Přidat.
6. Klikněte na tlačítko Ano, když se zobrazí výzva, pokud si umístit daný soubor uvnitř aplikace, jako jsou youd\_složku motivů.
7. Klikněte pravým tlačítkem na složku SkinFile uvnitř aplikace\_motivy složku v Průzkumníku řešení a zvolte Přidat novou položku.
8. Zvolte ze seznamu souborů šablony stylů a klikněte na tlačítko Přidat. Nyní máte všechny soubory nezbytné k implementaci nový motiv. Visual Studio se s názvem složky Motivy SkinFile. Klikněte pravým tlačítkem na tuto složku a změňte název na CoolTheme.
9. Otevřete soubor SkinFile.skin a přidejte následující kód konci souboru: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Uložte soubor SkinFile.skin.
11. Otevřete StyleSheet.css.
12. Veškerý text v něm nahraďte následujícím kódem: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Uložte soubor StyleSheet.css.
14. Otevřete stránku Default.aspx.
15. Přidejte ovládací prvek textového pole a ovládací prvek tlačítko.
16. Uložte na stránku. Teď přejděte na stránku Default.aspx. By měla zobrazit jako normální webového formuláře.
17. Otevřete soubor web.config.
18. Přidejte následující přímo pod otevírání `<system.web>` značky: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Uložte soubor web.config. Teď přejděte na stránku Default.aspx. Měl by se zobrazit motivu použít.
20. Pokud ho ještě není otevřený, otevřete stránku Default.aspx v sadě Visual Studio.
21. Klikněte na tlačítko.
22. Změnit **SkinID** vlastnost goButton. Všimněte si, že Visual Studio poskytuje pro ovládací prvek tlačítko rozevíracího seznamu platnou hodnotou SkinID.
23. Uložte na stránku. Teď na stránce v prohlížeči zobrazte opět náhled. Tlačítko by měl nyní vyslovením příkazu "go" a musí být širší vzhledu.

Použití **SkinID** vlastností, můžete snadno nakonfigurovat různé vzhledy pro různé instance určitého typu serverového ovládacího prvku.

## <a name="the-stylesheettheme-property"></a>Vlastnost StyleSheetTheme

Zatím mluvili pouze o aplikování motivů pomocí vlastnosti motivu. Při použití vlastnosti motivu, soubor vzhledu přepíše nastavení jazyka IDL deklarativní pro ovládací prvky serveru. Například v cvičení 1 zadaná SkinID "goButton" pro ovládací prvek tlačítka a, změnit text na tlačítko "Přejít". Mohli jste si všimnout, že vlastnost Text tlačítka v Návrháři byla nastavena na "Button", ale overrode motiv, který. Motiv se vždy přepíší všechna nastavení vlastnosti v návrháři.

Pokud chcete mít možnost přepsat vlastnosti definované v souboru skinu motivu se vlastnosti zadaný v návrháři, můžete použít **StyleSheetTheme** vlastnosti namísto vlastnosti motivu. Vlastnost StyleSheetTheme je stejná jako vlastnost motiv, s tím rozdílem, že nepřepisuje všechna nastavení explicitní vlastnost jako vlastnost motiv.

Tento údaj zobrazíte v akci, otevřete soubor web.config z projektu v cvičení 1 a změňte `<pages>` – element pro následující:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Teď přejděte na stránku Default.aspx a uvidíte, že ovládací prvek tlačítko má vlastnost Text "Button" znovu. Důvodem je, explicitní vlastnost nastavení v Návrháři přepisuje vlastnost Text nastavil goButton SkinID.

## <a name="overriding-themes"></a>Přepsání motivy

Jako globální motiv lze přepsat pomocí motivu se stejným názvem v aplikaci\_složku motivů aplikace. Motiv se ale nepoužije ve scénáři true přepsání. Pokud modul runtime, zaznamená soubory motivu v aplikaci\_složku motivů, použije motiv pomocí těchto souborů a bude ignorovat globální motiv.

Vlastnost StyleSheetTheme je přepisovatelná a může být přepsána nastaveními v kódu následujícím způsobem:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Webové části

Webové části technologie ASP.NET je integrovaná sada ovládacích prvků pro vytváření webů, které umožňují koncovým uživatelům upravit obsah, vzhled a chování webových stránek přímo z prohlížeče. Změny lze použít pro všechny uživatele v lokalitě nebo k jednotlivým uživatelům. Když uživatelé změní stránek a ovládacích prvků, nastavení se můžou ukládat zachovat osobní preference uživatele napříč relacemi prohlížeče budoucí funkce se nazývá přizpůsobení. Tyto funkce webových částí znamená, že vývojáři mohou umožněte koncovým uživatelům přizpůsobit webovou aplikaci dynamicky, bez zásahu správce nebo vývojáře.

Pomocí sada ovládacích prvků webové části, jako vývojář můžete povolit koncovým uživatelům:

- Přizpůsobení obsahu stránky. Uživatelé můžou přidat nové ovládací prvky webové části na stránku, odebrat je, skrýt nebo minimalizovat jako běžný windows.
- Přizpůsobení rozložení stránky. Uživatelé můžou přetáhněte ovládací prvek webové části na stránce jiné zóny nebo změnit její vzhled, vlastnosti a chování.
- Export a import ovládací prvky. Uživatelům můžete importovat nebo exportovat nastavení webových částí pro použití v jiných stránek nebo webů, vlastnosti, vzhled a dokonce i data v ovládacích prvcích. To snižuje data vstupu a konfigurace požadavky na koncové uživatele.
- Můžete vytvořte připojení. Uživatelé mohou vytvořit připojení mezi ovládacími prvky, takže například může ovládací prvek grafu zobrazí graf pro data v ovládacím prvku akciích. Uživatelé mohou přizpůsobit nejen samotné připojení k Internetu, ale vzhled a podrobnosti o tom, jak ovládací prvek graf zobrazí data.
- Správa a přizpůsobení nastavení na úrovni webu. Autorizovaní uživatelé můžou konfigurovat nastavení na úrovni webu, zjistit, kdo může přistupovat k webu nebo stránky, nastavení přístupu na základě rolí k ovládacím prvkům a tak dále. Uživatel v roli správce může například nastavit ovládací prvek webové části pro všemi uživateli sdílet a zabránit uživatelům, kteří nejsou správci, přizpůsobovat sdílený ovládací prvek.

Obvykle budete pracovat s webovými částmi v jednom ze tří způsobů: vytváření stránek, které využívají ovládací prvky webových částí, vytvořit jednotlivých ovládacích prvků webové části nebo vytvořit úplnou, přizpůsobení webových aplikací, jako je například portál.

## <a name="page-development"></a>Vývoj pro stránku

Stránky mohou vývojáři nástrojů vizuální návrh, jako je Microsoft Visual Studio 2005 k vytvoření stránky, které používají webové části. Jednou z výhod v pomocí nástroje, jako je, že ovládací prvek webové části sady Visual Studio poskytuje funkce pro vytváření přetahování myší a konfigurace ovládacích prvků webové části ve vizuálním návrháři. Například můžete přetáhnout zóny webové části nebo ovládací prvek webové části editoru na návrhovou plochu pomocí návrháře a nakonfigurujte ovládací prvek vpravo v Návrháři přes uživatelské rozhraní poskytované webové části sada ovládacích prvků. To můžete urychlit vývoj aplikací webových částí a snížit množství kódu, který musíte napsat.

## <a name="control-development"></a>Vývoj ovládacích prvků

Všechny existující ovládací prvky ASP.NET můžete použít jako ovládací prvek webové části, včetně standardní ovládací prvky webového serveru, vlastních serverových ovládacích prvků a uživatelských ovládacích prvků. Pro maximální programové řízení prostředí můžete také vytvořit vlastní ovládací prvky webové části, které jsou odvozeny z třídy webové části. Pro jednotlivé vývoj ovládacích prvků webové části obvykle buď vytvoříte uživatelský ovládací prvek a použijte ho jako ovládací prvek webové části nebo vyvíjet vlastní ovládací prvek webové části.

Jako příklad vyvíjet vlastní ovládací prvek webové části, můžete vytvořit ovládací prvek pro žádnou z funkcí poskytovaných jiných serverových ovládacích prvků technologie ASP.NET, které můžou být užitečné k balíčku jako přizpůsobitelný ovládací prvek webové části zadejte: kalendáře, seznamy, finančních informací Novinky, kalkulačky, ovládací prvky s formátovaným textem pro aktualizaci obsahu, upravovat mřížky, která připojení k databázím, grafy, které se dynamicky aktualizovat zobrazení, nebo informace o počasí a projít informace. Pokud zadáte vizuálního návrháře s ovládacím prvkem, pak každý vývojář stránky pomocí sady Visual Studio můžete jednoduše přetáhněte ovládací prvek do zóny webové části a nakonfigurujte jej v době návrhu bez nutnosti psát další kód.

Přizpůsobení je základem pro funkce webových částí. Umožňuje uživatelům upravovat - nebo přizpůsobit--rozložení, vzhled a chování ovládacích prvků webové části na stránce. Individuální nastavení jsou dlouhodobé: jsou trvalé, ne jenom během aktuální relace prohlížeče (jako stav zobrazení), ale také do dlouhodobého úložiště tak, aby nastavení uživatele se uloží pro budoucí relace prohlížeče také. Ve výchozím nastavení pro stránky webových částí je povoleno individuální nastavení.

Strukturálních součásti uživatelského rozhraní závisí na individuální nastavení a poskytují základní struktury a služby potřebné pro všechny ovládací prvky webové části. Jedna komponenta strukturální uživatelského rozhraní, které vyžaduje na každou stránku webových částí je ovládací prvek WebPartManager. I když se nikdy, má tento ovládací prvek kritickou úlohu koordinovat všechny ovládací prvky webové části na stránce. Například sleduje všechny jednotlivé prvky webové části. Spravuje zóny webové části (oblasti, které obsahují ovládací prvky webové části na stránce), a které ovládací prvky jsou ve které zóny. Také sleduje a ovládací prvky různých režimech zobrazení stránky může být, jak procházet, připojit, upravit nebo režim katalogu a zda změny přizpůsobení platí pro všechny uživatele nebo pro jednotlivé uživatele. Nakonec iniciuje a sleduje připojení a komunikace mezi ovládacími prvky webové části.

Druhý typ strukturálních součásti uživatelského rozhraní je zóna. Zóny slouží jako správci rozložení na stránku webových částí. Jejich obsahovat a uspořádat ovládací prvky, které jsou odvozeny z část třídy (částí) a umožňují provést rozložení stránky modulární orientovaný vodorovně nebo svisle. Zóny také nabízí běžné a jednotné prvky uživatelského rozhraní (například styl záhlaví a zápatí, title, styl ohraničení, tlačítka a tak dále) pro každý ovládací prvek, který obsahují; Tyto společné prvky jsou označovány jako okolí ovládacího prvku. Několik specializovaných typů zón se používají v různých režimech zobrazení a různé ovládací prvky. Různé druhy zóny jsou popsány v níže uvedené části Základní ovládací prvky webových částí.

Ovládací prvky webových částí uživatelského rozhraní, které jsou odvozeny z **část** třídy, zahrnují primární uživatelské rozhraní na stránku webových částí. Sada ovládacích prvků webových částí je flexibilní a včetně možností poskytuje pro vytvoření ovládacích prvků části. Kromě vytváření vlastní vlastních ovládacích prvků webové části, je můžete využít taky existující serverové ovládací prvky ASP.NET, uživatelských ovládacích prvků nebo vlastních serverových ovládacích prvků jako ovládací prvky webové části. Základní ovládací prvky, které se běžně používají k vytvoření stránky webové části jsou popsané v další části.

## <a name="web-parts-essential-controls"></a>Základní ovládací prvky webových částí

Sada ovládacích prvků webových částí je rozsáhlý, ale některé ovládací prvky jsou nezbytné, protože jsou potřebné pro webové části pro práci nebo protože jsou ovládací prvky nejčastěji používané na stránky webových částí. Jak můžete začít používat webové části a vytvoření základní webové části stránky, je vhodné se seznámit s základní ovládací prvky webové části jsou popsané v následující tabulce.

| **Ovládací prvek webové části** | **Popis** |
| --- | --- |
| WebPartManager | Spravuje všechny ovládací prvky webové části na stránce. Jeden (a pouze jeden) **WebPartManager** ovládací prvek je vyžadován pro každou stránku webových částí. |
| CatalogZone | Obsahuje ovládací prvky CatalogPart. Můžete vytvořit katalog webových částí, ze kterých mohou uživatelé vybrat ovládací prvky pro přidání na stránku této zóny. |
| Třídy EditorZone | Obsahuje ovládací prvky EditorPart. Povolit uživatelům upravit a přizpůsobit ovládací prvky webové části na stránce pomocí této zóny. |
| WebPartZone | Obsahuje a poskytuje celkové rozložení pro ovládací prvky webové části, které tvoří rozhraní hlavní stránky. Použijte tuto zónu při každém vytvoření stránky s webovými částmi. Stránky mohou obsahovat jeden nebo více zónách. |
| Třídy ConnectionsZone | Obsahuje ovládací prvky WebPartConnection a poskytuje uživatelské rozhraní pro správu připojení. |
| Webová část (GenericWebPart) | Vykreslí primární uživatelské rozhraní; do této kategorie patří většina ovládacích prvků webových částí uživatelského rozhraní. Pro maximální kontroly prostřednictvím kódu programu, můžete vytvořit vlastní ovládací prvky webové části, které jsou odvozeny od základní třídy **webové části** ovládacího prvku. Existující ovládací prvky serveru, uživatelské ovládací prvky nebo vlastní ovládací prvky můžete použít také jako ovládací prvky webové části. Pokaždé, když některý z těchto ovládacích prvků jsou umístěny v zóně, **WebPartManager** ovládací prvek automaticky zabalí s **GenericWebPart** ovládací prvky v době běhu tak, aby je mohli používat funkce webových částí. |
| Ovládací prvek CatalogPart | Obsahuje seznam dostupných ovládacích prvků webové části, které uživatelé můžou přidávat na stránku. |
| WebPartConnection | Vytvoří propojení dvou ovládacích prvků webové části na stránce. Připojení definuje jeden z ovládacích prvků webové části jako poskytovatel (data) a druhý jako spotřebitel. |
| EditorPart | Slouží jako základní třída pro ovládací prvky začínáte se speciálním editorem. |
| Ovládací prvky EditorPart (AppearanceEditorPart LayoutEditorPart, BehaviorEditorPart a PropertyGridEditorPart) | Povolit uživatelům přizpůsobení různé aspekty ovládacích prvků webových částí uživatelského rozhraní na stránce. |

## <a name="lab-create-a-web-part-page"></a>Praktické cvičení: Vytvoření stránky webové části

V tomto testovacím prostředí vytvoříte stránku webových částí, která bude uchovávat informace prostřednictvím profilů technologie ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Vytvoření jednoduché stránky s webovými částmi

V této části tohoto průvodce vytvořit stránku, která používá ovládací prvky webové části zobrazíte statický obsah. Prvním krokem při práci s webovými částmi je vytvořit stránku se dvěma požadovanými strukturální prvky. Stránky webových částí nejprve, musí ovládací prvek WebPartManager ke sledování a koordinovat všechny ovládací prvky webové části. Za druhé stránky webové části musí jeden nebo více oblastí, které jsou složené ovládací prvky, které obsahují webové části nebo jiných serverových ovládacích prvků a zabírají zadanou oblast stránky.

> [!NOTE]
> Není potřeba dělat nic. přizpůsobení webových částí; je povoleno standardně pro sada ovládacích prvků webových částí. Při prvním spuštění stránky webové části na web, ASP.NET nastaví výchozího zprostředkovatele individuálního nastavení pro ukládání individuální uživatelská nastavení. Další informace o přizpůsobení najdete v tématu Přehled přizpůsobení webových částí.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Vytvoření stránky obsahující ovládací prvky webové části

1. Zavřete stránku výchozí a přidejte novou stránku k lokalitě s názvem WebPartsDemo.aspx.
2. Přepnout na **návrhu** zobrazení.
3. Z **zobrazení** nabídky, ujistěte se, že **nevizuální ovládací prvky** a **podrobnosti** jsou vybrané možnosti, abyste si mohli zobrazit rozložení značky a ovládací prvky, které nemají uživatelské rozhraní.
4. Umístěte kurzor před `<div>` značek na návrhové ploše a stisknutím klávesy ENTER přidat nový řádek. Umístěte kurzor před znak nového řádku, klikněte na tlačítko **Blokový formát** rozevíracího seznamu v nabídce ovládací prvek a vyberte **Nadpis 1** možnost. V záhlaví, přidejte text **Ukázková stránka webových částí**.
5. Z **WebParts** kartu na panelu nástrojů přetáhněte **WebPartManager** ovládacího prvku na stránku, umístěte ho hned za znak nového řádku a před `<div>`značky.   
  
   **WebPartManager** ovládacího prvku nezobrazuje žádný výstup, takže se zobrazí jako šedou pole na návrhové ploše.
6. Umístěte kurzor do `<div>` značky.
7. V **rozložení** nabídky, klikněte na tlačítko **Vložit tabulku**a vytvořit novou tabulku, která obsahuje jeden řádek a tři sloupce. Klikněte na tlačítko **vlastnosti buňky** tlačítka, vyberte **horní** z **svislé zarovnání** rozevíracího seznamu, klikněte na tlačítko **OK**a klikněte na tlačítko **OK** znovu k vytvoření této tabulky.
8. Přetáhněte ovládací prvek WebPartZone do levého sloupce tabulky. Klikněte pravým tlačítkem myši **WebPartZone** ovládací prvek, zvolte **vlastnosti**a nastavte následující vlastnosti:   
  
   ID: SidebarZone   
  
   HeaderText: bočního panelu
9. Přetáhněte druhý **WebPartZone** do sloupce tabulky střední a nastavte následující vlastnosti:   
  
   ID: MainZone   
  
   HeaderText: hlavní
10. Uložte soubor.

Vaše stránka nyní obsahuje dvě odlišné zóny, které můžete řídit samostatně. Ani jedna zóna ale nemá žádný obsah, takže vytváření obsahu je dalším krokem. V tomto návodu pracujete s webovými částmi, které zobrazují jenom statický obsah.

Je určená rozložení zóny webové části &lt;zonetemplate&gt; elementu. Uvnitř šablony zóny můžete přidat libovolný ovládací prvek ASP.NET, ať už jde o vlastní ovládací prvek webové části, uživatelský ovládací prvek nebo existujícího ovládacího prvku serveru. Všimněte si, že tady použijete na ovládací prvek popisku a, který chcete jednoduše přidat statický text. Když umístíte regulární serverový ovládací prvek v **WebPartZone** zóny, ASP.NET zpracovává ovládacího prvku jako ovládací prvek webové části v době běhu, který povoluje funkce webové části na ovládacím prvku.

**K vytvoření obsahu hlavní zóny**

1. V **návrhu** zobrazení, přetáhněte **popisek** ovládacího prvku **standardní** kartu na panelu nástrojů do oblasti obsahu zóny jehož **ID** vlastnost je nastaven na MainZone.
2. Přepnout na **zdroj** zobrazení. Všimněte si, že &lt;zonetemplate&gt; element byl přidán k zabalení **popisek** ovládacího prvku MainZone.
3. Přidat atribut s názvem **title** k &lt;asp: label&gt; prvek a nastavte jej na hodnotu obsah. Odeberte Text = "atributem Label, který z &lt;asp: label&gt; elementu. Mezi počátečními a ukončovacími značkami z &lt;asp: label&gt; prvku, přidejte nějaký text, například **Vítá vás domovskou stránku** dvojicí &lt;h2&gt; značky elementů. Váš kód by měl vypadat takto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Uložte soubor.

Dále vytvořte uživatelský ovládací prvek, který je také přidat na stránku jako ovládací prvek webové části.

### <a name="to-create-a-user-control"></a>Chcete-li vytvořit uživatelský ovládací prvek

1. Přidáte nový uživatelský ovládací prvek webu do vašeho webu, který bude sloužit jako ovládací prvek pro hledání. Zrušte výběr možnosti **umístěte zdrojový kód v samostatném souboru**. Přidat ve stejném adresáři jako stránka WebPartsDemo.aspx a pojmenujte ho SearchUserControl.ascx.   
  
    > [!NOTE]
    > Uživatelský ovládací prvek pro Tento názorný postup neimplementuje skutečné funkce vyhledávání; používá se pouze k předvedení funkcí webových částí.
2. Přepnout na **návrhu** zobrazení. Z **standardní** kartu na panelu nástrojů přetáhněte ovládací prvek textového pole na stránce.
3. Umístěte kurzor po textové pole, které jste právě přidali a stisknutím klávesy ENTER přidejte nový řádek.
4. Přetáhněte ovládací prvek tlačítko na nový řádek pod textové pole, které jste právě přidali na stránku.
5. Přepnout na **zdroj** zobrazení. Ujistěte se, že zdrojový kód pro uživatelský ovládací prvek vypadat jako v následujícím příkladu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Soubor uložte a zavřete.

Nyní můžete přidat ovládací prvky webové části do zóny bočním panelu. Při přidávání dvou ovládacích prvků k zóně postranního panelu, jedna obsahuje seznam odkazů a druhý, který je uživatelský ovládací prvek jste vytvořili v předchozím postupu. Odkazy jsou přidány jako standardní **popisek** ovládacího prvku serveru, podobným způsobem, jakým jste vytvořili statický text hlavní zóny. I když součástí jednotlivých serverových ovládacích prvků uživatelského ovládacího prvku může být obsažena přímo v zóně (jako je ovládací prvek popisku), v tomto případě nejsou. Místo toho jsou součástí uživatelského ovládacího prvku, který jste vytvořili v předchozím postupu. Tento příklad ukazuje běžným způsobem balení libovolné ovládací prvky a další funkce, které chcete v uživatelském ovládacím prvku, a pak na něj odkazovat v zóně jako ovládací prvek webové části.

V době běhu sada ovládacích prvků webové části zabalí oba ovládací prvky pomocí ovládacích prvků třídy GenericWebPart. Když **GenericWebPart** ovládací prvek obaluje ovládací prvek webového serveru, na nadřazený ovládací prvek je ovládací prvek obecné části a serverový ovládací prvek máte přístup prostřednictvím vlastnosti ChildControl nadřazeného ovládacího prvku. Standardní ovládací prvky webového serveru, mají stejné základní chování a atributy jako ovládací prvky webové části, které jsou odvozeny z umožňuje toto použití obecné části ovládacích prvků **webové části** třídy.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Přidání ovládacích prvků webové části do zóny bočního panelu

1. Otevřete stránku WebPartsDemo.aspx.
2. Přepnout na **návrhu** zobrazení.
3. Přetáhněte ovládací prvek stránku uživatele jste vytvořili, SearchUserControl.ascx, z **Průzkumníka řešení** do zóny jehož **ID** vlastnost je nastavena na SidebarZone a umístěte ho.
4. Uložení WebPartsDemo.aspx stránky.
5. Přepnout na **zdroj** zobrazení.
6. Uvnitř &lt;asp: webpartzone&gt; – element pro SidebarZone, těsně nad odkaz na uživatelský ovládací prvek, přidejte &lt;asp: label&gt; element s obsaženy odkazy, jak je znázorněno v následujícím příkladu. Přidejte také **Title** atribut ke značce uživatelského ovládacího prvku, s hodnotou **hledání**, jak je znázorněno. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Soubor uložte a zavřete.

Nyní můžete otestovat stránky tak, že k němu přejdete v prohlížeči. Na stránce se zobrazí dvě zóny. Na následujícím snímku obrazovky se zobrazí na stránce.

**Ukázka části webové stránky na dvě zóny**


![Snímek obrazovky webové části VS návod 1](profiles-themes-and-web-parts/_static/image3.gif)

**Obrázek 3**: webové části VS návod 1 – snímek obrazovky


V názvu každého ovládacího prvku je šipku dolů, který poskytuje přístup k nabídce příkazů dostupných akcí, které můžete provádět v ovládacím prvku. Klikněte na nabídku akcí pro jeden z ovládacích prvků a pak klikněte na tlačítko **minimalizovat** operací a Všimněte si, že je minimalizován ovládacího prvku. V nabídce Akce klikněte na **obnovení**, a vrátí jeho normální velikost ovládacího prvku.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Umožňuje uživatelům upravovat stránky a změnit rozložení

Webové části umožňuje uživatelům změnit rozložení ovládacích prvků webové části jejich přetažením z: jednu zónu na jiný. Kromě povolení uživatelům přesunovat **webové části** ovládacích prvků z jednu zónu na jiný, můžete povolit uživatelům upravit různé vlastnosti ovládacích prvků, včetně jejich vzhled, rozložení a chování. Sada ovládacích prvků webové části poskytuje základní funkce pro úpravy **webové části** ovládacích prvků. I když to není v tomto podrobném návodu, můžete také vytvořit vlastní editor ovládací prvky, které uživatelům umožňují upravovat funkce **webové části** ovládacích prvků. Stejně jako u mění se umístění **webové části** ovládacího prvku, úprava vlastností ovládacího prvku spoléhá na individuální nastavení technologie ASP.NET a uložte změny, která uživatelé použijí.

V této části průvodce přidat možnost uživatelů můžete upravit základní vlastnosti libovolného **webové části** ovládací prvek na stránce. K povolení těchto funkcí, přidáte další vlastní uživatelský ovládací prvek na stránku, spolu s &lt;asp: třídy editorzone&gt; elementu a dva ovládací prvky pro úpravy.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Chcete-li vytvořit uživatelský ovládací prvek, který umožňuje měnit rozložení stránky

1. V sadě Visual Studio na **souboru** nabídku, vyberte **nový** podnabídku a klikněte na tlačítko **souboru** možnost.
2. V **přidat novou položku** dialogového okna, vyberte **webový uživatelský ovládací prvek**. Pojmenujte nový soubor DisplayModeMenu.ascx. Zrušte výběr možnosti **umístěte zdrojový kód v samostatném souboru**.
3. Klikněte na tlačítko Přidat nový ovládací prvek.
4. Přepnout na **zdroj** zobrazení.
5. Odeberte všechny existující kód v novém souboru a vložte následující kód. Tento kód uživatelského ovládacího prvku využívá funkce Sada ovládacích prvků webové části, které umožňují změnit jeho zobrazení nebo režim zobrazení na stránce a také umožňuje změnit fyzický vzhled a rozložení stránky, i když jsou v režimech zobrazení. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Soubor uložte kliknutím uložit na panelu nástrojů nebo tak, že vyberete ikonu **Uložit** na **souboru** nabídky.

### <a name="to-enable-users-to-change-the-layout"></a>Povolit uživatelům změnit rozložení

1. Otevřete stránku WebPartsDemo.aspx a přepněte se na **návrhu** zobrazení.
2. Umístěte kurzor **návrhu** zobrazení hned za **WebPartManager** ovládací prvek, který jste přidali dříve. Přidejte pevné vrátit za text tak, aby se za prázdný řádek **WebPartManager** ovládacího prvku. Umístěte kurzor na prázdný řádek.
3. Přetáhněte ovládací prvek jste právě vytvořili (Tento soubor se nazývá DisplayModeMenu.ascx) do WebPartsDemo.aspx stránce a umístěte ho na prázdný řádek.
4. Přetáhněte ovládací prvek třídy EditorZone z **WebParts** části na panelu nástrojů do zbývajících buňky tabulky na stránce WebPartsDemo.aspx.
5. Z **WebParts** části na panelu nástrojů přetáhněte ovládací prvek AppearanceEditorPart a ovládací prvek LayoutEditorPart do **EditorZone** ovládacího prvku.
6. Přepnout na **zdroj** zobrazení. Výsledný kód v buňce tabulky by měl vypadat podobně jako v následujícím kódu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Uložte soubor WebPartsDemo.aspx. Vytvoření uživatelského ovládacího prvku, který umožňuje měnit režimy zobrazení a změna rozložení stránky a odkazujete ovládacího prvku na webové stránce primární.

Teď můžete otestovat schopnost upravovat stránky a změna rozložení.

### <a name="to-test-layout-changes"></a>Chcete-li otestovat změny rozložení

1. Načtení stránky v prohlížeči.
2. Klikněte na tlačítko **režim zobrazení** rozevírací nabídky a vybereme **upravit**. Zobrazí se názvy zón.
3. Přetáhněte **mé odkazy** ovládací prvek pomocí jeho panelu nadpisů ze zóny bočního panelu na konec hlavní zóny. Vaše stránka by měla vypadat podobně jako na následujícím snímku obrazovky.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Části Ukázky webovou stránku pomocí ovládacího prvku mé odkazy přesunout


![Snímek obrazovky webové části VS návod 2](profiles-themes-and-web-parts/_static/image4.gif)

**Obrázek 4**: webové části VS návod 2 – snímek obrazovky


1. Klikněte na tlačítko **režim zobrazení** rozevírací nabídky a vybereme **Procházet**. Na stránce se aktualizují, zmizí názvy zón a **mé odkazy** řídit zůstanou uložena tam, kam jste umístili.
2. Abychom si předvedli, přizpůsobení funguje, ukončete prohlížeč a pak znovu načíst stránku. Provedené změny se uloží pro budoucí relace prohlížeče.
3. Z **režim zobrazení** nabídce vyberte možnost **upravit**.   
  
   Každý ovládací prvek na stránce se nyní zobrazí s šipkou dolů v záhlaví okna, který obsahuje příkazy rozevírací nabídky.
4. Klikněte na šipku a zobrazit příkazy nabídky na **mé odkazy** ovládacího prvku. Klikněte na tlačítko **upravit** příkaz.   
  
   **EditorZone** ovládací prvek se zobrazí, zobrazení EditorPart řídí jste přidali.
5. V **vzhled** části ovládacích prvků pro úpravy, změnit **Title** do oblíbených, použijte **typ stylu okolí** rozevíracího seznamu vyberte **pouze název**a potom klikněte na tlačítko **použít**. Na následujícím snímku obrazovky se zobrazí na stránce v režimu úprav.

### <a name="web-parts-demo-page-in-edit-mode"></a>Ukázka části webové stránky v režimu úprav


![Snímek obrazovky webové části VS návod 3](profiles-themes-and-web-parts/_static/image5.gif)

**Obrázek 5**: webové části VS návod 3 – snímek obrazovky


1. Klikněte na tlačítko **režim zobrazení** nabídky a vybereme **Procházet** vrátit do režimu procházení.
2. Ovládací prvek má teď aktualizovaný titulek a ohraničení, jak je znázorněno na následujícím snímku obrazovky.

### <a name="edited-web-parts-demo-page"></a>Upravované stránky webové části Ukázky


![Snímek obrazovky webové části VS návod 4](profiles-themes-and-web-parts/_static/image6.gif)

**Obrázek 4**: webové části VS návod 4 – snímek obrazovky


### <a name="adding-web-parts-at-run-time"></a>Přidání webových částí v době běhu

Můžete také povolit uživatelům přidávat ovládací prvky webové části na stránku v době běhu. Uděláte to tak, nakonfigurujte na stránce Katalog webových částí, která obsahuje seznam ovládacích prvků webové části, které chcete zpřístupnit uživatelům.

**Umožňuje uživatelům přidání webové části v době běhu**

1. Otevřete stránku WebPartsDemo.aspx a přepněte se na **návrhu** zobrazení.
2. Z **WebParts** kartu na panelu nástrojů přetáhněte ovládací prvek CatalogZone v pravém sloupci v tabulce rat **EditorZone** ovládacího prvku.   
  
   Oba ovládací prvky může být ve stejné tabulce buňce, protože se nezobrazí ve stejnou dobu.
3. V podokně vlastností přiřadit řetězec **přidat webové části** na vlastnost HeaderText **CatalogZone** ovládacího prvku.
4. Z **WebParts** části na panelu nástrojů přetáhněte ovládací prvek DeclarativeCatalogPart do oblasti obsahu **CatalogZone** ovládacího prvku.
5. Klikněte na šipku v pravém horním rohu **DeclarativeCatalogPart** ovládacího prvku zpřístupnit nabídku úlohy a pak vyberte **upravit šablony**.
6. Z **standardní** části panelu nástrojů přetáhněte **FileUpload** ovládacího prvku a **kalendáře** ovládací prvek do **WebPartsTemplate** část **DeclarativeCatalogPart** ovládacího prvku.
7. Přepnout na **zdroj** zobrazení. Zkontrolujte zdrojový kód &lt;asp: catalogzone&gt; elementu. Všimněte si, že **DeclarativeCatalogPart** obsahuje ovládací prvek &lt;webpartstemplate&gt; element s dvěma uzavřené serverové ovládací prvky, které bude možné přidat na stránku z katalogu.
8. Přidat **název** vlastnost na jednotlivé ovládací prvky, které jste přidali do katalogu pomocí zobrazenou hodnotu řetězce pro každý název v následujícím příkladu kódu. Přestože názvem se nenachází ve vlastnosti můžete obvykle nastavit na těchto dvou serverových ovládacích prvků v době návrhu, když uživatel přidá tyto ovládací prvky **WebPartZone** zóny z katalogu v době běhu, jsou každý obaleny  **Třídy GenericWebPart** ovládacího prvku. Díky tomu tak, aby fungoval jako ovládací prvky webové části, takže budou moct zobrazit názvy.   
  
   Kód pro dvou ovládacích prvcích obsažených **DeclarativeCatalogPart** ovládací prvek by měl vypadat takto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Uložte na stránku.

Teď můžete otestovat v katalogu.

### <a name="to-test-the-web-parts-catalog"></a>K otestování webové části katalogu

1. Načtení stránky v prohlížeči.
2. Klikněte na tlačítko **režim zobrazení** rozevírací nabídky a vybereme **katalogu**.   
  
   V katalogu s názvem **přidat webové části** se zobrazí.
3. Přetáhněte **oblíbených** řídit ze zóny hlavní zpět do horní části bočního panelu zóny a umístěte ho.
4. V **přidat webové části** katalogu, vyberte obě políčka a pak vyberte **hlavní** z rozevíracího seznamu, který obsahuje dostupné zóny.
5. Klikněte na tlačítko **přidat** v katalogu. Ovládací prvky jsou přidány do zóny hlavní. Pokud chcete, můžete přidat více instancí prvků z katalogu na stránku.   
  
   Na následujícím snímku obrazovky ukazuje stránku ovládací prvek pro uložení souboru a kalendáře v zóně hlavní. 

![Ovládací prvky přidané do zóny Main z katalogu](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Klikněte na tlačítko **režim zobrazení** rozevírací nabídky a vybereme **Procházet**. Katalog zmizí a obnovení stránky.
7. Zavřete prohlížeč. Znovu načtěte stránku. Provedené změny zachovat.
