---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Profily, motivů a webové části | Microsoft Docs
author: microsoft
description: Existují hlavní změny v konfiguraci a instrumentaci v technologii ASP.NET 2.0. Nové rozhraní API ASP.NET konfigurace umožňuje provedení pr změn konfigurace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: b749ed093fbaacf45b60f2826a2c20bac219a5c7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892111"
---
<a name="profiles-themes-and-web-parts"></a>Profily, motivů a webové části
====================
podle [Microsoft](https://github.com/microsoft)

> Existují hlavní změny v konfiguraci a instrumentaci v technologii ASP.NET 2.0. Nové rozhraní API ASP.NET konfigurace umožňuje změny konfigurace, které má být provedeno prostřednictvím kódu programu. Kromě toho existuje mnoho nové nastavení konfigurace povolit pro nové konfigurace a instrumentace.


ASP.NET 2.0 představuje významné zlepšení v oblasti přizpůsobené weby. Kromě funkcí členství weve již zahrnut profilů technologie ASP.NET, motivů a webové části výrazně zvýšit přizpůsobení webových stránek.

## <a name="aspnet-profiles"></a>Profilů technologie ASP.NET

ASP.NET profily jsou podobná relací. Rozdílem je, že profil je trvalé, zatímco při zavření prohlížeče dojde ke ztrátě relace. Další velký rozdíl mezi relací a profilů je, že jsou profily silného typu, a to proto vám poskytnou IntelliSense během procesu vývoje.

Profil je definována v konfiguračním souboru počítače nebo soubor web.config pro aplikaci. (V souboru web.config podsložky nelze definovat profil.) Následující kód definuje profil, který se nejprve ukládat návštěvníkům webu a příjmení.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

Výchozí datový typ pro vlastnost profilu je System.String. V tomto příkladu byl zadán typ žádná data. Vlastnosti FirstName a LastName proto jsou obě typu řetězec. Jak už jsme zmínili, profil, který vlastnosti jsou silného typu. Následující kód přidá novou vlastnost pro stáří, který je typu Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Profily se obecně používají s ověřování pomocí formulářů ASP.NET. Pokud se použije v kombinaci s ověřování pomocí formulářů, každý uživatel má samostatný profil spojený s jejich ID uživatele. Ale je také možné povolit použití profilů v anonymní aplikace pomocí &lt;anonymousIdentification&gt; element v konfiguračním souboru spolu s **allowAnonymous** atribut jako vidíte níže:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Pokud anonymní uživatel prohlíží web, ASP.NET vytvoří instanci **ProfileCommon** pro uživatele. Tento profil používá k identifikaci uživatele jako unikátní návštěvník jedinečné ID v souborech cookie v prohlížeči. Tímto způsobem můžete uložit informace o profilu pro uživatele, kteří se anonymně procházení.

## <a name="profile-groups"></a>Profil skupiny

Je možné vlastnosti skupiny profilů. Seskupování vlastnosti se dá k simulaci víc profilů pro danou aplikaci.

Následující konfigurace konfiguruje vlastnost FirstName a LastName pro dvě skupiny; Odběrateli a potenciální zákazníky.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

Pak je možné nastavit vlastnosti v dané skupině následujícím způsobem:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Ukládání komplexních objektů

Příklady, které jsme si zahrnutých dosavadní práce, uloženého jednoduché datové typy v profilu. Je také možné ukládat komplexními datovými typy v profilu tak, že zadáte metoda serializace použití **serializeAs** atribut následujícím způsobem:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

V takovém případě typ je PurchaseInvoice. Třída PurchaseInvoice musí být označen jako serializovatelný a může obsahovat libovolný počet vlastnosti. Například, pokud má vlastnost s názvem PurchaseInvoice **NumItemsPurchased**, můžete odkazovat na tuto vlastnost v kódu následujícím způsobem:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Profil dědičnosti

Je možné vytvořit profil pro používání v několika aplikací. Vytvořením třídy profil, který je odvozen od ProfileBase můžete opakovaně použít profil v několika aplikací pomocí **dědí** atributu, jak je uvedeno níže:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

V tomto případě třída **PurchasingProfile** vypadat například takto:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Poskytovatelé profilů

ASP.NET profilů pomocí zprostředkovatele modelu. Výchozí zprostředkovatel ukládá informace v databázi systému SQL Server Express v aplikaci\_složky Data webové aplikace pomocí zprostředkovatele SqlProfileProvider. Pokud databáze neexistuje, ASP.NET jej automaticky vytvoří při pokusí uložit informace o profilu.

V některých případech ale můžete vytvořit vlastního zprostředkovatele profilu. Funkce profilu ASP.NET umožňuje snadno používat různé zprostředkovatele.

Vytvoření vlastního profilu poskytovatele při:

- Budete muset uložit informace o profilu ve zdroji dat, jako je databáze FoxPro nebo v databázi Oracle, který není podporován poskytovateli profilu součástí rozhraní .NET Framework.
- Potřebujete spravovat informace o profilu pomocí schématu databáze, které se liší od schématu databáze používá zprostředkovatele součástí rozhraní .NET Framework. Častým příkladem je, že chcete integrovat informace o profilu uživatele data v existující databázi systému SQL Server.

### <a name="required-classes"></a>Požadované třídy

Chcete-li implementovat poskytovatele profilu, vytvořte třídu, která dědí z abstraktní třídy System.Web.Profile.ProfileProvider. **Objekt ProfileProvider** abstraktní třída zase dědí abstraktní třídu System.Configuration.SettingsProvider, která dědí z abstraktní třídy System.Configuration.Provider.ProviderBase. Z důvodu tohoto řetězce dědičnosti kromě požadovaných členů **objekt ProfileProvider** třídy, je nutné implementovat požadované členy **SettingsProvider** a  **ProviderBase** třídy.

Následující tabulka popisuje vlastnosti a metody, které je nutné implementovat z **ProviderBase**, **SettingsProvider**, a **objekt ProfileProvider** abstraktní třídy.

### <a name="providerbase-members"></a>Členové ProviderBase

| **Člen** | **Popis** |
| --- | --- |
| Initialize – metoda | Přijímá jako vstup název instance zprostředkovatele a NameValueCollection nastavení konfigurace. Slouží k nastavení možnosti a hodnoty vlastností pro instance zprostředkovatele, včetně hodnot závisí na implementaci a možností zadaný v souboru Web.config na konfigurace počítače. |

### <a name="settingsprovider-members"></a>Členové SettingsProvider

| **Člen** | **Popis** |
| --- | --- |
| Vlastnost ApplicationName | Název aplikace, která je uložená ke každému profilu. Zprostředkovatel profilů používá název aplikace k ukládání informací o profilu samostatně pro každou aplikaci. To umožňuje více aplikací prostředí ASP.NET používat stejný zdroj dat bez konflikt, pokud stejné uživatelské jméno se vytvoří v různých aplikacích. Alternativně můžete sdílet více aplikací prostředí ASP.NET zdroj dat tak, že zadáte stejný název aplikace. |
| GetPropertyValues – metoda | Vezme jako vstupní a SettingsContext a SettingsPropertyCollection objektu. **SettingsContext** poskytuje informace o uživateli. Informace jako primární klíč slouží k načtení informací o vlastnost profilu pro uživatele. Použití **SettingsContext** objekt, který chcete získat uživatelské jméno a zda je uživatel ověřený nebo anonymní. **SettingsPropertyCollection** obsahuje kolekci SettingsProperty objektů. Každý **SettingsProperty** objekt poskytuje název a typ vlastnosti a také další informace, jako je výchozí hodnota pro vlastnost a jestli je vlastnost jen pro čtení. **GetPropertyValues** metoda naplní SettingsPropertyValueCollection s SettingsPropertyValue objekty na základě **SettingsProperty** objekty zadaný jako vstup. Hodnoty ze zdroje dat pro určeného uživatele přiřazené k PropertyValue vlastnosti pro každou **SettingsPropertyValue** je vrácen objekt a celou kolekci. Volání metody také aktualizuje LastActivityDate hodnotu pro zadaný uživatelský profil na aktuální datum a čas. |
| SetPropertyValues – metoda | Přijímá jako vstup **SettingsContext** a **SettingsPropertyValueCollection** objektu. **SettingsContext** poskytuje informace o uživateli. Informace jako primární klíč slouží k načtení informací o vlastnost profilu pro uživatele. Použití **SettingsContext** objekt, který chcete získat uživatelské jméno a zda je uživatel ověřený nebo anonymní. **SettingsPropertyValueCollection** obsahuje kolekci **SettingsPropertyValue** objekty. Každý **SettingsPropertyValue** objekt poskytuje název, typ a hodnotu vlastnosti a také další informace, jako je výchozí hodnota pro vlastnost a jestli je vlastnost jen pro čtení. **SetPropertyValues** metoda aktualizace hodnoty vlastností profilu ve zdroji dat pro určeného uživatele. Volání metody také aktualizace **LastActivityDate** a LastUpdatedDate hodnot pro zadaný uživatelský profil na aktuální datum a čas. |

### <a name="profileprovider-members"></a>Objekt ProfileProvider členy

| **Člen** | **Popis** |
| --- | --- |
| DeleteProfiles – metoda | Vezme jako vstupní pole řetězců uživatele názvy a odstraní ze zdroje dat všechny profil informace a hodnoty vlastnosti pro zadané názvy, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. Pokud zdroj dat podporuje transakce, doporučujeme umístit všechny operace odstranění v transakci a vraťte transakci zpět a způsobí výjimku, pokud se některé operace odstranění se nezdaří. |
| DeleteProfiles – metoda | Vezme jako vstupní kolekce ProfileInfo objekty a odstraní ze zdroje dat všechny profil informace a hodnoty vlastnosti pro každý profil, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. Pokud zdroj dat podporuje transakce, doporučujeme umístit všechny operace odstranění v transakci a vraťte transakci zpět a způsobí výjimku, pokud se některé operace odstranění se nezdaří. |
| DeleteInactiveProfiles – metoda | Přijímá jako vstup hodnotu ProfileAuthenticationOption a objekt data a času a odstranění z dat zdroje informace o profilech všech a hodnoty vlastností, kde datum poslední aktivity je menší než nebo rovna zadané datum a čas a název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, nebo všechny profily mají být odstraněny. Pokud zdroj dat podporuje transakce, doporučujeme umístit všechny operace odstranění v transakci a vraťte transakci zpět a způsobí výjimku, pokud se některé operace odstranění se nezdaří. |
| GetAllProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnotu, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profilů. Vrátí ProfileInfoCollection, který obsahuje **ProfileInfo** objekty pro všechny profily ve zdroji dat, kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, nebo všechny profily mají být vráceny. Výsledky vrácené **GetAllProfiles** metoda jsou omezeny index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty, které se vrátí v **ProfileInfoCollection**. Hodnota indexu stránky určuje, které stránky s výsledky vrátit, přičemž 1 označuje na první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), je nastaven na celkový počet profilů. Pokud úložiště dat obsahuje 13 profilů pro aplikace a hodnota indexu stránky je 2 s velikostí stránky 5, například **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Celkový počet záznamů hodnota je nastavena na 13 po návratu metody. |
| GetAllInactiveProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnotu, **data a času** objekt, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastavena Celkový počet profilů. Vrátí **ProfileInfoCollection** obsahující **ProfileInfo** objekty pro všechny profily ve zdroji dat, kde datum poslední aktivity je menší než nebo rovna zadané **data a času**  a kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, nebo všechny profily mají být vráceny. Výsledky vrácené **GetAllInactiveProfiles** metoda jsou omezeny index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty, které se vrátí v **ProfileInfoCollection**. Hodnota indexu stránky určuje, které stránky s výsledky vrátit, přičemž 1 označuje na první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), je nastaven na celkový počet profilů. Pokud úložiště dat obsahuje 13 profilů pro aplikace a hodnota indexu stránky je 2 s velikostí stránky 5, například **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Celkový počet záznamů hodnota je nastavena na 13 po návratu metody. |
| FindProfilesByUserName – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnota řetězec obsahující uživatelské jméno, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profily. Vrátí **ProfileInfoCollection** obsahující **ProfileInfo** objekty pro všechny profily v datech zdroj, kde uživatelské jméno odpovídá zadané uživatelské jméno, které shodný název aplikace **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, nebo všechny profily mají být vráceny. Pokud váš zdroj dat podporuje další možnosti vyhledávání, například zástupné znaky, můžete zadat rozsáhlejší možnosti vyhledávání pro uživatelská jména. Výsledky vrácené **FindProfilesByUserName** metoda jsou omezeny index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty, které se vrátí v **ProfileInfoCollection**. Hodnota indexu stránky určuje, které stránky s výsledky vrátit, přičemž 1 označuje na první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), je nastaven na celkový počet profilů. Pokud úložiště dat obsahuje 13 profilů pro aplikace a hodnota indexu stránky je 2 s velikostí stránky 5, například **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Celkový počet záznamů hodnota je nastavena na 13 po návratu metody. |
| FindInactiveProfilesByUserName – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnota, řetězec obsahující uživatelské jméno, **data a času** objekt, celé číslo, které určuje index stránky, celé číslo, které určuje velikost stránky a odkaz na celé číslo, které bude nastaven na celkový počet profilů. Vrátí **ProfileInfoCollection** obsahující **ProfileInfo** objekty pro všechny profily ve zdroji dat kde uživatelské jméno odpovídá zadanému uživatelskému jménu, kde datum poslední aktivity je menší než nebo rovná zadané **data a času**, a kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, nebo všechny profily mají být vráceny. Pokud váš zdroj dat podporuje další možnosti vyhledávání, například zástupné znaky, můžete zadat rozsáhlejší možnosti vyhledávání pro uživatelská jména. Výsledky vrácené **FindInactiveProfilesByUserName** metoda jsou omezeny index stránky a hodnoty velikosti stránky. Hodnota velikosti stránky určuje maximální počet **ProfileInfo** objekty, které se vrátí v **ProfileInfoCollection**. Hodnota indexu stránky určuje, které stránky s výsledky vrátit, přičemž 1 označuje na první stránku. Parametr pro celkový počet záznamů je výstupní parametr (můžete použít **ByRef** v jazyce Visual Basic), je nastaven na celkový počet profilů. Pokud úložiště dat obsahuje 13 profilů pro aplikace a hodnota indexu stránky je 2 s velikostí stránky 5, například **ProfileInfoCollection** vrátil obsahuje šestinu až desetinu profily. Celkový počet záznamů hodnota je nastavena na 13 po návratu metody. |
| GetNumberOfInActiveProfiles – metoda | Přijímá jako vstup **ProfileAuthenticationOption** hodnotu a **data a času** objektu a vrací počet všechny profily ve zdroji dat, kde datum poslední aktivity je menší než nebo rovna zadané  **Data a času** a kde název aplikace odpovídá **ApplicationName** hodnotu vlastnosti. **ProfileAuthenticationOption** parametr určuje, zda pouze anonymní profily pouze ověřené profily, které se mají spočítat nebo všechny profily. |

### <a name="applicationname"></a>ApplicationName

Protože poskytovatelé profilů ukládání informací o profilu samostatně pro každou aplikaci, musíte zkontrolovat, že vaše datové schéma obsahuje název aplikace a že dotazů a aktualizace také obsahovat název aplikace. Například následující příkaz slouží k získání hodnoty vlastnosti z databáze na základě uživatelské jméno a jestli je anonymní profil a zajišťuje, že **ApplicationName** hodnota je obsažena v dotazu.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Motivy ASP.NET

## <a name="what-are-aspnet-20-themes"></a>Jaké jsou ASP.NET 2.0 motivy?

Jedním z nejdůležitějších aspektů webové aplikace je konzistentní vzhled a chování napříč webu. ASP.NET 1.x vývojáři obvykle pomocí stylů CSS (Cascading Style) k implementaci konzistentní vzhled a chování. ASP.NET 2.0 motivů CSS výrazně zdokonalit, protože poskytují vývojáři ASP.NET schopnost definovat vzhled serverových ovládacích prvků ASP.NET, jakož i elementů HTML. Motivy ASP.NET můžete u jednotlivých ovládacích prvků, konkrétní webové stránky nebo celou webovou aplikaci. Motivy použijte kombinaci souborů CSS, soubor volitelné vzhledu a adresář volitelné bitové kopie potřeby bitové kopie. Soubor vzhledu řídí vzhled serverových ovládacích prvků ASP.NET.

## <a name="where-are-themes-stored"></a>Kde jsou uložené motivy?

Umístění, kde jsou uložené motivy se liší v závislosti na rozsahu jejich oboru. Motivy, které mohou být použity na všechny aplikace jsou uloženy v následující složce:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Motiv, který je specifická pro konkrétní aplikace jsou uloženy v `App\_Themes\<Theme\_Name>` adresář v kořenu webové stránky.

> [!NOTE]
> Soubor vzhledu měli změnit, jenom vlastnosti ovládacích prvků serveru, které ovlivňují vzhled.

Jako globální motiv je motiv, který lze použít na všechny aplikace nebo web běžící na webovém serveru. Tyto motivy jsou uloženy ve výchozím nastavení v adresáři ASP.NETClientfiles\Themes, který je uvnitř v2.x.xxxxx adresáře. Alternativně můžete přesunout soubory motiv do aspnet\_klienta nebo systému\_webové / /Themes/ [verze] [motiv\_název] složku v kořenovém adresáři vašeho webu.

Motivy specifické pro aplikaci lze použít pouze k aplikaci, ve kterém jsou umístěny soubory. Tyto soubory jsou uloženy v `App\_Themes/<theme\_name>` adresář v kořenu webové stránky.

## <a name="the-components-of-a-theme"></a>Součástí motiv

Motiv se skládá z jednoho nebo více souborů CSS, soubor volitelné vzhledu a volitelnou složku bitové kopie. Soubory šablon stylů CSS může být jakýkoli název chcete (tj. default.css nebo theme.css atd.) a musí být v kořenové složce motivů. Soubory šablon stylů CSS se používá k definování obyčejnou tříd CSS a atributy pro konkrétní selektorů. Chcete-li použít jednu z tříd CSS na prvek stránky **CSSClass** vlastnost se používá.

Soubor vzhledu je soubor XML, která obsahuje vlastnost definice serverových ovládacích prvků ASP.NET. Kód uvedený níže je příklad souboru vzhledu.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Obrázek 1** níže znázorňuje malé stránku ASP.NET procházet bez použit motiv. **Obrázek 2** ukazuje stejný soubor s použit motiv. Barva pozadí a barvy se konfigurují prostřednictvím soubor CSS. Vzhled tlačítka a textové pole jsou konfigurováni pomocí soubor vzhledu uvedené výše.


![Žádné motiv](profiles-themes-and-web-parts/_static/image1.gif)

**Obrázek 1**: žádné motiv


![Použije motiv](profiles-themes-and-web-parts/_static/image2.gif)

**Obrázek 2**: motiv použít


Soubor vzhledu uvedené výše definuje vzhled výchozí pro všechny ovládací prvky textové pole a tlačítko – ovládací prvky. To znamená, že každý TextBox – ovládací prvek a vložit na stránce tlačítko – ovládací prvek bude trvat na tento vzhled. Můžete také definovat vzhledu, který lze použít k určité instance těchto ovládacích prvků pomocí **SkinID** vlastností ovládacího prvku.

Následující kód definuje vzhledu ovládacího prvku tlačítko. Tlačítko pouze ovládací prvky s **SkinID** vlastnost **goButton** neprovede vzhled vzhledu.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Může mít pouze jeden výchozí vzhledu za – typ ovládacího prvku serveru. Pokud budete potřebovat další vzhledy, používejte SkinID vlastnost.

## <a name="applying-themes-to-pages"></a>Použití motivů na stránky

Motiv může být použita pomocí některého z následujících metod:

- V &lt;stránky&gt; element v souboru web.config
- V @Page direktivy stránky
- Prostřednictvím kódu programu

## <a name="applying-a-theme-in-the-configuration-file"></a>Použití motivu v konfiguračním souboru

Chcete-li motiv v konfiguračním souboru aplikace, použijte následující syntaxi:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

Název motivu, který je zde stanovený musí odpovídat názvu složky motivů. Tato složka může existovat buď v některém z umístění uvedené v předchozí části tohoto kurzu. Pokud se pokusíte použít motiv, který neexistuje, dojde k chybě konfigurace.

## <a name="applying-a-theme-in-the-page-directive"></a>Použití motivu v Page – direktiva

Můžete také použít motiv v direktivě @ stránky. Tato metoda umožňuje použít motiv konkrétní stránky.

Použít motiv v @Page – direktiva, použijte následující syntaxi:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Znovu zde zadaný motiv musí odpovídat složky motivu, jak je uvedeno nahoře. Pokud se pokusíte použít motiv, který neexistuje, dojde k selhání sestavení. Visual Studio bude také zvýrazněte atribut a oznámit, že žádný takový motiv existuje.

## <a name="applying-a-theme-programmatically"></a>Použití motivu prostřednictvím kódu programu

Použití motivu programově, je nutné zadat **motiv** vlastnosti pro stránky na **stránky\_PreInit** metoda.

Použití motiv programově, použijte následující syntaxi:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Je nezbytné k uplatnění motivu v metodě PreInit z důvodu životního cyklu stránky. Pokud ji použijete po provedení této, motivu stránky se již byly použity modulem runtime a změnu v tomto okamžiku je příliš pozdě v životním cyklu. Pokud použijete motiv, který neexistuje, **HttpException** dojde. Při použití motivu prostřednictvím kódu programu, upozornění sestavení dojde-li zadána vlastnost SkinID všech ovládacích prvků serveru. Toto upozornění se má o tom, že žádné deklarativně motivu a můžete ignorovat.

## <a name="exercise-1--applying-a-theme"></a>Cvičení 1: Použití motivu

V tomto cvičení použijete motiv technologie ASP.NET pro webovou stránku.

> [!IMPORTANT]
> Pokud používáte Microsoft Word k zadání informací do souboru vzhledu, ujistěte se, že nejsou nahrazení běžné uvozovky typografickými. Uvozovky způsobí problémy s vzhledu soubory.

1. Vytvoření nového webu ASP.NET.
2. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte Přidat novou položku.
3. Zvolte konfigurační soubor webu ze seznamu souborů a klikněte na tlačítko Přidat.
4. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a zvolte Přidat novou položku.
5. Vyberte soubor vzhledu a klikněte na tlačítko Přidat.
6. Klikněte na tlačítko Ano, když se zobrazí dotaz, pokud youd třeba umístit soubor v aplikaci\_složky motivů.
7. Klikněte pravým tlačítkem na složku SkinFile uvnitř aplikace\_motivy složky v Průzkumníku řešení a zvolte Přidat novou položku.
8. Zvolte ze seznamu souborů šablony stylů a klikněte na tlačítko Přidat. Nyní máte všechny soubory, které jsou nezbytné k implementaci nový motiv. Visual Studio má s názvem své složky Motivy SkinFile. Klikněte pravým tlačítkem na tuto složku a změňte název CoolTheme.
9. Otevřete soubor SkinFile.skin a přidejte následující kód konec souboru: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Uložte soubor SkinFile.skin.
11. Otevřete StyleSheet.css.
12. Veškerý text v ní nahraďte následujícím textem: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Uložte soubor StyleSheet.css.
14. Otevřete stránku Default.aspx.
15. Přidání ovládacího prvku textového pole a ovládacího prvku tlačítko.
16. Uložte stránky. Nyní přejděte na stránku Default.aspx. Mělo by se zobrazit jako normální webového formuláře.
17. Otevřete soubor web.config.
18. Přidejte následující přímo pod otevření `<system.web>` značky: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Uložte soubor web.config. Nyní přejděte na stránku Default.aspx. Mělo by se zobrazit s motiv použít.
20. Pokud již není otevřený, otevřete stránku Default.aspx v sadě Visual Studio.
21. Kliknutím na tlačítko.
22. Změna **SkinID** vlastnost goButton. Všimněte si, že Visual Studio poskytuje rozevírací seznam s platnými hodnotami SkinID pro ovládací prvek tlačítko.
23. Uložte stránky. Nyní znovu náhled stránku v prohlížeči. Tlačítko by měl nyní vyslovte "jít" a mělo by být širší v vzhled.

Pomocí **SkinID** vlastnost, můžete snadno konfigurovat různé vzhledy pro různé instance určitého typu ovládacího prvku serveru.

## <a name="the-stylesheettheme-property"></a>Vlastnost StyleSheetTheme

Pokud už jsme jsme mluvili pouze o použití motivů pomocí vlastnosti motivu. Pokud použijete vlastnost motiv, přepíše soubor vzhledu deklarativní nastavení pro ovládací prvky serveru. Například v cvičení 1, jste zadali SkinID "goButton" pro ovládací prvek tlačítko a který změnit text na tlačítko "Vložit". Jste si všimli, že vlastnost Text tlačítka v Návrháři byla nastavena na "Button", ale overrode motiv, který. Motiv vždy přepíše nastavení vlastnosti v návrháři.

Pokud byste chtěli potlačit vlastnosti definované v souboru vzhledu v motivu se v zadané vlastnosti návrháře, můžete použít **StyleSheetTheme** vlastnost místo vlastnost motivu. Vlastnost StyleSheetTheme je stejná jako vlastnost motiv s tím rozdílem, že nepřepisuje všechna nastavení explicitní vlastnost jako vlastnost motivu.

Chcete-li zobrazit to v praxi, otevřete soubor web.config z projektu v cvičení 1 a změňte `<pages>` element pro následující:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Nyní přejděte na stránku Default.aspx a uvidíte, že ovládací prvek tlačítko má vlastnost Text "Button" znovu. Je to způsobeno vlastnosti textu, která nastavuje goButton SkinID přepisují nastavení explicitní vlastnosti v návrháři.

## <a name="overriding-themes"></a>Přepsání motivů

Jako globální motiv lze přepsat pomocí motivu se stejným názvem v aplikaci\_motivy složky aplikace. Motiv, ale není použita ve scénáři true přepsání. Pokud modul runtime zaznamená motiv souborů v aplikaci\_složky Motivy, se použije motiv pomocí těchto souborů a bude ignorovat globální motiv.

Vlastnost StyleSheetTheme je přepisovatelné a může být přepsána nastaveními v kódu následujícím způsobem:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Webové části

ASP.NET – webové části je integrovaná sada ovládacích prvků pro vytváření webů, které umožňují koncovým uživatelům upravit obsah, vzhled a chování webových stránek přímo z prohlížeče. Změny se použijí pro všechny uživatele na webu nebo pro jednotlivé uživatele. Když uživatelé změnit stránek a ovládacích prvků, lze nastavení uložit Pokud chcete zachovat osobní preference uživatele mezi budoucí relace prohlížeče, funkce se nazývá přizpůsobení. Tyto možnosti webových částí znamenají, že vývojáři mohou umožnit koncovým uživatelům přizpůsobení webových aplikací dynamicky, bez zásahu správce nebo vývojáře.

Pomocí sady webových částí, jako vývojář můžete povolit koncovým uživatelům:

- Přizpůsobit obsah stránky. Uživatele můžete přidat nové ovládací prvky webové části na stránku, je odebrat, skryjete nebo je minimalizovat stejně jako obyčejnou windows.
- Přizpůsobení rozložení stránky. Uživatele můžete přetáhněte ovládací prvek webové části na stránce do jiné zóny nebo změnit jeho vzhled, vlastnosti a chování.
- Export a import ovládací prvky. Uživatelé mohou importovat nebo exportovat nastavení webových částí pro použití na jiných stránkách nebo weby, ponechá vlastnosti, vzhled a dokonce i data v ovládacích prvcích. Tím se snižuje data položku a konfigurace požadavky na koncové uživatele.
- Vytvoření připojení. Uživatelé mohou vytvořit připojení mezi ovládacími prvky, takže například může zobrazit graf pro data v ovládacím prvku běžícími ovládacího prvku grafu. Uživatelé mohou přizpůsobit nejen připojení samotné, ale vzhled a podrobnosti o tom, jak ovládací prvek grafu zobrazí data.
- Spravovat a přizpůsobení nastavení na úrovni webu. Autorizovaní uživatelé můžete nakonfigurovat nastavení na úrovni webu, můžete určit, kdo přístup k webu nebo stránky, nastavení přístupu na základě rolí k ovládacím prvkům a tak dále. Uživatel v roli správce může například nastavení ovládacího prvku webové části ke sdílení všichni uživatelé a zabránit uživatelům, kteří nejsou správci, přizpůsobovat sdílené ovládacího prvku.

Obvykle bude fungovat s webovými částmi v jednom ze tří způsobů: vytváření stránek, které používají ovládací prvky webových částí, vytváření jednotlivé webové části nebo vytváření kompletních, přizpůsobitelných webových aplikací, jako je například portál.

## <a name="page-development"></a>Vývoj pro stránku

Vývojáři stránky můžete použít k vytvoření stránky, které používají webové části nástroje visual návrhu, jako je Microsoft Visual Studio 2005. Jednou z výhod při používání nástroje, jako je Visual Studio je, že sada webových částí na sadu poskytuje funkce pro přetažení myší vytváření a konfiguraci webové části ovládacích prvků v vizuálního návrháře. Například můžete pomocí návrháře přetáhněte zónu webové části nebo ovládacího prvku editoru webové části na návrhovou plochu a pak nakonfigurovat práva ovládacího prvku v Návrháři pomocí uživatelského rozhraní, poskytuje webové části řízení sady. To můžete urychlit vývoj aplikací webových částí a snížit objem kód, který máte k zápisu.

## <a name="control-development"></a>Vývoj pro ovládací prvek

Všechny existující ovládací prvky ASP.NET můžete použít jako ovládací prvek webové části, včetně standardní ovládací prvky webového serveru, vlastní serverové ovládací prvky a uživatelské ovládací prvky. Pro maximální programové řízení vašeho prostředí můžete také vytvořit vlastní ovládací prvky webových částí, které jsou odvozeny od třídy webové části. Pro jednotlivé webové části řízení vývoj můžete se obvykle vytvoření uživatelského ovládacího prvku a použít jako webovou částí nebo vyvíjet vlastní ovládací prvek webové části.

Jako příklad vývoj vlastní ovládací prvek webové části, můžete vytvořit řízení k poskytnutí libovolné funkce poskytované službou jiných serverových ovládacích prvků technologie ASP.NET, které může být užitečná pro balíček jako přizpůsobitelný webovou částí: kalendáře, seznamy, finanční informace, zprávy, kalkulačky, ovládací prvky s formátovaným textem pro aktualizaci obsahu, upravitelné mřížky, připojení k databázím, grafy, které dynamicky aktualizují své zobrazení, nebo informace o počasí a cestují informace. Pokud zadáte vizuálního návrháře s ovládacím prvkem, pak všechny stránky vývojáře pomocí sady Visual Studio můžete jednoduše přetáhněte vlastního ovládacího prvku do zóny webové části a nakonfigurovat ho v době návrhu bez nutnosti napsat další kód.

Přizpůsobení je základem funkce webové části. Umožňuje uživatelům upravovat - nebo přizpůsobit--rozložení, vzhled a chování ovládacích prvků webové části na stránce. Přizpůsobené nastavení je dlouhodobé: jsou trvalé, nikoli pouze během aktuální relace prohlížeče (jako zobrazení stavu), ale také do dlouhodobého úložiště tak, aby nastavení uživatele se uloží pro budoucí relace prohlížeče také. Přizpůsobení je povoleno ve výchozím nastavení pro webové části stránky.

Strukturální součásti uživatelského rozhraní spoléhají na přizpůsobení a poskytují základní struktura a služby potřebné pro všechny webové části. Jediná strukturální komponenta uživatelského rozhraní, která je požadována na každé stránce webové části je ovládací prvek WebPartManager. Přestože není vidět, obsahuje tento ovládací prvek kritické úlohy koordinace všechny webové části na stránce. Například sleduje všechny jednotlivé prvky webové části. Spravuje oblastí webových částí (oblasti, které obsahují webové části na stránce), a které ovládací prvky jsou ve které zóny. Také sleduje a řídí různých režimech zobrazení stránky může být v tak, jak procházet, připojit, upravit nebo katalogu režimu a zda změny přizpůsobení platí pro všechny uživatele nebo pro jednotlivé uživatele. Nakonec iniciuje a sleduje připojení a komunikace mezi webovými částmi.

Druhý druh strukturální součást uživatelského rozhraní je zóny. Zóny působí jako správci rozložení na stránce webové části. Budou obsahovat a uspořádání ovládacích prvků, které jsou odvozeny od třídy část (ovládací prvky součástí) a umožňují rozložením modulární stránky ve vodorovně nebo svisle. Zóny také nabízí jednotnou a společnou prvky uživatelského rozhraní (například záhlaví a zápatí styl, název, styl ohraničení, akce tlačítka a tak dále) pro každý ovládací prvek, která obsahují; Tyto společné prvky se označují jako chrome ovládacího prvku. Několik speciálních typů zón se používají v různých režimech zobrazení a pomocí různých ovládacích prvků. Základní ovládací prvky webových částí níže v části jsou popsané různé typy zón.

Ovládací prvky webových částí uživatelského rozhraní, které dědí z **část** třídy, zahrnují primární uživatelské rozhraní na stránce webové části. Sada webových částí je flexibilní a včetně v možnostech nabízí pro vytváření ovládacích prvků část. Kromě vytvoření vlastní vlastní ovládací prvky webových částí, můžete taky existující serverové ovládací prvky ASP.NET, uživatelských ovládacích prvků nebo vlastní serverové ovládací prvky jako webové části. Základní ovládací prvky, které se běžně používají k vytvoření webové části stránky jsou popsané v další části.

## <a name="web-parts-essential-controls"></a>Základní ovládací prvky webových částí

Sada webových částí je rozsáhlé, ale některé ovládací prvky jsou nezbytné, buď protože jsou požadována pro webové části pro práci, nebo protože jsou ovládacích prvků nejčastěji používaná na stránkách webové části. Začnete používat webové části a vytvoření základní webové části stránky, je vhodné se seznámit s základní ovládací prvky webových částí popsané v následující tabulce.

| **Webová část** | **Popis** |
| --- | --- |
| WebPartManager | Spravuje všechny webové části na stránce. Jeden (a pouze jeden) **WebPartManager** je potřeba na každé stránce webové části řízení. |
| CatalogZone | Obsahuje prvky CatalogPart. Vytvoření katalogu webových částí, ze kterých mohou uživatelé vybrat ovládací prvky přidat na stránku pomocí této zóny. |
| EditorZone | Obsahuje EditorPart ovládací prvky. Povolit uživatelům upravit a upravovat webové části na stránce pomocí této zóny. |
| WebPartZone | Obsahuje a poskytuje celkové rozložení pro webové části ovládacích prvků, které tvoří hlavní uživatelské rozhraní na stránce. Tuto zónu použijte vždy, když vytvoříte stránky s webovými částmi. Stránky může obsahovat jednu nebo více zón. |
| ConnectionsZone | Obsahuje ovládací prvky WebPartConnection a poskytuje uživatelské rozhraní pro správu připojení. |
| Webová část (GenericWebPart) | Vykreslí rozhraní primární; do této kategorie patří většina ovládacích prvků uživatelského rozhraní webových částí. Pro maximální programové řízení, můžete vytvořit vlastní ovládací prvky webových částí, které jsou odvozeny od základní **webové části** ovládacího prvku. Také můžete použít stávající ovládací prvky serveru, uživatelské ovládací prvky nebo vlastní ovládací prvky jako webové části. Kdykoli je některý z těchto prvků umístěn v zóně, **WebPartManager** řízení automaticky zabalí s **GenericWebPart** prvky v době běhu tak, aby je mohli používat s funkcemi webové části. |
| CatalogPart | Obsahuje seznam dostupných webových částí ovládacích prvků, které uživatele můžete přidat na stránku. |
| WebPartConnection | Vytvoří připojení mezi dvěma ovládacími prvky webové části na stránce. Připojení mezi ovládací prvky webových částí definuje poskytovatele (data) a druhý jako příjemce. |
| EditorPart | Slouží jako základní třída pro specializované prvky editoru. |
| Ovládací prvky EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart a PropertyGridEditorPart) | Povolit uživatelům přizpůsobit různé aspekty ovládací prvky webových částí uživatelského rozhraní na stránce. |

## <a name="lab-create-a-web-part-page"></a>Testovacího prostředí: Vytvoření stránku webové části

V tomto testovacím prostředí vytvoříte webovou stránku část, která bude uchovávat informace prostřednictvím profilů technologie ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Vytvoření jednoduché stránky s webovými částmi

V této části průvodce vytvoříte stránky, který používá webové části zobrazíte statický obsah. Prvním krokem při práci s webovými částmi je vytvoření stránky s dvěma požadované strukturální prvky. Webové části stránky musí nejprve ovládacího prvku WebPartManager sledovat a koordinovat všechny webové části. Za druhé stránky s webovými částmi musí jeden nebo více oblastí, které jsou složené ovládací prvky, které obsahují webové části nebo jiných serverových ovládacích prvků a zabírají stanovenou oblast stránky.

> [!NOTE]
> Nemusíte dělat nic přizpůsobení webových částí; ve výchozím nastavení sada webových částí je povolená. Při prvním spuštění stránky s webovými částmi v lokalitě, ASP.NET nastaví výchozího poskytovatele přizpůsobení k uložení individuální uživatelská nastavení. Další informace o přizpůsobení najdete v tématu Přehled přizpůsobení webových částí.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Vytvoření stránky pro jednotlivé webové části ovládací prvky

1. Zavřete stránku výchozího a přidat nové stránky na web s názvem WebPartsDemo.aspx.
2. Přepnout na **návrhu** zobrazení.
3. Z **zobrazení** nabídky, ujistěte se, že **nevizuální ovládací prvky** a **podrobnosti** , abyste viděli rozložení značky a ovládací prvky, které nemají uživatelského rozhraní jsou vybrané možnosti.
4. Umístěte kurzor před `<div>` značky na návrhovou plochu a potom stiskněte klávesu ENTER, chcete-li přidat nový řádek. Umístěte kurzor před znak nového řádku, klikněte na tlačítko **Formát bloku** rozevíracího seznamu řízení v nabídce a vyberte **Nadpis 1** možnost. V záhlaví, přidejte text **Ukázková stránka webových částí**.
5. Z **webové části** karty z panelu nástrojů, přetáhněte **WebPartManager** ovládacího prvku na stránku, umístěte ho bezprostředně za znak nového řádku a před `<div>`značky.   
  
   **WebPartManager** prvek nezobrazuje žádný výstup, takže se jeví jako šedé pole na plochu návrháře.
6. Umístěte kurzor do `<div>` značky.
7. V **rozložení** nabídky, klikněte na tlačítko **Vložit tabulku**a vytvořit novou tabulku, která obsahuje jeden řádek a tři sloupce. Klikněte na tlačítko **vlastnosti buněk** tlačítko, vyberte **horní** z **svislé zarovnání** rozevíracího seznamu, klikněte na tlačítko **OK**a klikněte na tlačítko **OK** znovu a vytvořit v tabulce.
8. Přetáhněte ovládací prvek WebPartZone do levého sloupce tabulky. Klikněte pravým tlačítkem myši **WebPartZone** řídit, zvolte **vlastnosti**a nastavte následující vlastnosti:   
  
   ID: SidebarZone   
  
   HeaderText: Sidebar
9. Přetáhněte druhý **WebPartZone** řízení do sloupce střední tabulky a nastavte následující vlastnosti:   
  
   ID: MainZone   
  
   HeaderText: Main
10. Uložte soubor.

Stránka nyní obsahuje dvě odlišné zóny, které můžete řídit samostatně. Ani zóny však nemá žádný obsah, takže vytváření obsahu je dalším krokem. V tomto návodu pracujete s webovými částmi, které zobrazují jenom statický obsah.

Je zadána rozložení zóny webové části &lt;zonetemplate&gt; elementu. Do šablony zóny můžete přidat libovolný ovládací prvek ASP.NET toho, jestli je vlastní ovládací prvek webové části, uživatelský ovládací prvek nebo existujícího ovládacího prvku serveru. Všimněte si, že zde používáte ovládací prvek popisek a do které jednoduše přidáte statický text. Při umístění ovládacího prvku v běžném serveru **WebPartZone** zóny, ASP.NET zpracovává ovládacího prvku jako webovou částí za běhu, která umožňuje webové části funkce na ovládací prvek.

**Při vytváření obsahu pro hlavní zóny**

1. V **návrhu** zobrazení, přetáhněte ji **popisek** řídit z **standardní** karty z panelu nástrojů do oblasti obsahu zóny jejichž **ID** vlastnost je nastavena na MainZone.
2. Přepnout na **zdroj** zobrazení. Všimněte si, že &lt;zonetemplate&gt; element byla přidána do zabalení **popisek** ovládacího prvku MainZone.
3. Přidání atributu s názvem **název** k &lt;asp: label&gt; elementu a jeho hodnotu nastavte na obsah. Odeberte Text = "Popisek" atribut z &lt;asp: label&gt; elementu. Mezi počáteční a koncové značky &lt;asp: label&gt; elementu, přidejte text, například **Vítá vás domovskou stránku** mezi dva &lt;h2&gt; značky elementu. Váš kód by měl vypadat takto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Uložte soubor.

Dále vytvořte uživatelský ovládací prvek, který lze také přidat na stránku jako webovou částí.

### <a name="to-create-a-user-control"></a>Vytvoření uživatelského ovládacího prvku

1. Přidáte nový uživatelský ovládací prvek webu na váš web, která bude sloužit jako ovládacího prvku vyhledávání. Zrušte výběr možnost **umístit zdrojového kódu v samostatném souboru**. Přidat ve stejném adresáři jako stránky WebPartsDemo.aspx a pojmenujte jej SearchUserControl.ascx.   
  
    > [!NOTE]
    > Uživatelský ovládací prvek pro tento návod neimplementuje skutečné funkce vyhledávání; použije se pouze k předvedení funkcí webové části.
2. Přepnout na **návrhu** zobrazení. Z **standardní** karty na panelu nástrojů, přetáhněte ovládací prvek textové pole na stránce.
3. Umístěte kurzor po textového pole, které jste právě přidali a stiskněte klávesu ENTER a přidejte nový řádek.
4. Přetáhněte ovládací prvek tlačítko na stránce na novém řádku pod textové pole, které jste právě přidali.
5. Přepnout na **zdroj** zobrazení. Ujistěte se, že zdrojový kód uživatelského ovládacího prvku vypadá jako v následujícím příkladu. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Soubor uložte a zavřete.

Teď můžete přidat ovládací prvky webových částí do zóny bočním panelu. Přidáváte dvou ovládacích prvků na bočním panelu zónu, jedna obsahuje seznam odkazů a druhý, který je uživatelského ovládacího prvku jste vytvořili v předchozím postupu. Odkazy jsou přidány jako standardní **popisek** prvku serveru, podobně jako jste vytvořili statický text hlavní zóny. I když součástí jednotlivých serverových ovládacích prvků uživatelského ovládacího prvku může být obsažena přímo v zóně (např. ovládací prvek popisek), v tomto případě nejsou. Místo toho jsou součástí uživatelského ovládacího prvku, kterou jste vytvořili v předchozím postupu. Tento příklad ukazuje běžný způsob balíček ať ovládací prvky a další funkce, které chcete v uživatelský ovládací prvek a potom na něj odkazovat v zóně jako webovou částí.

V době běhu sada webových částí zabalí oba ovládací prvky s ovládacími prvky GenericWebPart. Když **GenericWebPart** řízení zabalí ovládacího prvku webového serveru, Obecná webová část je nadřazeného ovládacího prvku a můžete přístup k ovládacímu prvku serveru prostřednictvím vlastnosti ChildControl nadřazeného ovládacího prvku. Toto použití ovládacích prvků obecné části umožňuje standardní ovládací prvky webového serveru do mají stejné základní chování a atributy jako ovládací prvky webových částí, které jsou odvozeny od **webové části** třídy.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Přidání ovládacích prvků webových částí do zóny bočním panelu

1. Otevřete stránku WebPartsDemo.aspx.
2. Přepnout na **návrhu** zobrazení.
3. Přetáhněte ovládací prvek stránce uživatele jste vytvořili, SearchUserControl.ascx, z **Průzkumníku řešení** do zóny jejichž **ID** vlastnost je nastavena na SidebarZone a umístěte jej.
4. Uložte stránky WebPartsDemo.aspx.
5. Přepnout na **zdroj** zobrazení.
6. Uvnitř &lt;asp: webpartzone&gt; element pro SidebarZone, nad odkaz na váš uživatelský ovládací prvek, přidejte &lt;asp: label&gt; element s obsahovala odkazy, jak je znázorněno v následujícím příkladu. Také přidat **název** atribut ke značce ovládacího prvku uživatele s hodnotou **vyhledávání**, jak je vidět. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Soubor uložte a zavřete.

Nyní můžete otestovat stránku procházením v prohlížeči. Na stránce se zobrazí dvě zóny. Následující snímek obrazovky ukazuje stránku.

**Webové části ukázkové stránky na dvě zóny**


![Webové části VS – návod 1 – snímek obrazovky](profiles-themes-and-web-parts/_static/image3.gif)

**Obrázek 3**: webové části VS – návod 1 – snímek obrazovky


V názvu každého ovládacího prvku je šipku dolů, který poskytuje přístup k nabídce operací dostupných akcí, které můžete provádět v ovládacím prvku. Klikněte na nabídku operací pro jednu z kontrol, a pak klikněte na **minimalizaci** sloveso a Všimněte si, že je minimalizován ovládacího prvku. V nabídce operací klikněte na tlačítko **obnovení**, a vrátí ovládací prvek jeho normální velikost.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Povolení uživatelům upravovat stránky a změnit rozložení

Webové části poskytuje možnost uživatelům změnit rozložení ovládacích prvků webových částí tak, že je přetáhnete z jedné zóny na jiný. Kromě toho, že uživatelům přesunovat **webové části** ovládacích prvků z jedné zóny na jiný, můžete povolit uživatelům upravit různé vlastnosti ovládacích prvků, včetně jejich vzhled, rozložení a chování. Sada webových částí poskytuje základní funkce pro úpravy **webové části** ovládací prvky. I když není obsaženo v tomto návodu, můžete také vytvořit vlastní editor ovládací prvky, které uživatelům umožňují upravovat funkce **webové části** ovládací prvky. Stejně jako u Změna umístění **webové části** ovládací prvek, úprava vlastností ovládacího prvku závisí na technologii ASP.NET přizpůsobení uložit změny, které uživatelé provádět v.

V této části průvodce přidat schopnost uživatelům upravit základní vlastnosti všech **webové části** ovládací prvek na stránce. Pokud chcete povolit tyto funkce, přidáte další vlastní uživatelský ovládací prvek na stránku, spolu s &lt;asp: editorzone&gt; elementu a dvou ovládacích prvků pro úpravy.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Vytvoření uživatelského ovládacího prvku, který umožňuje měnit rozložení stránky

1. V sadě Visual Studio na **soubor** nabídce vyberte možnost **nový** podnabídky a klikněte na tlačítko **souboru** možnost.
2. V **přidat novou položku** dialogovém okně, vyberte **uživatelský ovládací prvek webu**. Název nový soubor DisplayModeMenu.ascx. Zrušte výběr možnost **umístit zdrojový kód do samostatného souboru**.
3. Klikněte na tlačítko Přidat vytvořit nový ovládací prvek.
4. Přepnout na **zdroj** zobrazení.
5. Odeberte všechny existující kód v nový soubor a vložte následující kód. Tento kód uživatelského ovládacího prvku používá funkce sada webových částí, které umožňují na stránce změnit jeho zobrazení nebo režim zobrazení a také umožňuje změnit fyzický vzhled a rozložení stránky při jsou v režimech zobrazení. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Soubor uložte kliknutím uložit ikonu na panelu nástrojů nebo výběrem **Uložit** na **souboru** nabídky.

### <a name="to-enable-users-to-change-the-layout"></a>Chcete-li povolit uživatelům změnit rozložení

1. Otevřete stránku WebPartsDemo.aspx a přejděte do **návrhu** zobrazení.
2. Umístěte kurzor v **návrhu** zobrazit pouze po **WebPartManager** ovládací prvek, který jste přidali dříve. Přidejte pevný vraťte za text tak, aby bylo prázdný řádek po **WebPartManager** ovládacího prvku. Umístěte kurzor na prázdný řádek.
3. Přetáhněte uživatelského ovládacího prvku, kterou jste právě vytvořili (soubor jmenuje DisplayModeMenu.ascx) do WebPartsDemo.aspx stránky a na prázdný řádek.
4. Přetáhněte ovládací prvek EditorZone z **webové části** oddílu na panelu na zbývající buňky tabulky na stránce WebPartsDemo.aspx.
5. Z **webové části** části na panelu nástrojů, přetáhněte ovládací prvek AppearanceEditorPart a do ovládacího prvku LayoutEditorPart **EditorZone** ovládacího prvku.
6. Přepnout na **zdroj** zobrazení. Výsledný kód v buňce tabulky by měla vypadat podobně jako následující kód. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Uložte soubor WebPartsDemo.aspx. Vytvoření uživatelského ovládacího prvku, který vám umožní změnit režim zobrazení a změna rozložení stránky a mít odkazují ovládacího prvku na webové stránce primární.

Nyní můžete otestovat možnost upravovat stránky a změna rozložení.

### <a name="to-test-layout-changes"></a>K otestování změny rozložení

1. Načtěte stránku v prohlížeči.
2. Klikněte **režim zobrazení** rozevírací nabídky a vyberte **upravit**. Zobrazí se názvy zón.
3. Přetáhněte **odkazy** ovládacího prvku záhlaví panelu ze zóny bočním panelu v dolní části hlavní zóny. Stránka by měla vypadat podobně jako na následujícím snímku obrazovky.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Webové stránky částí ukázku pomocí řízení odkazy přesunout


![Webové části VS – návod 2 – snímek obrazovky](profiles-themes-and-web-parts/_static/image4.gif)

**Obrázek 4**: webové části VS – návod 2 – snímek obrazovky


1. Klikněte **režim zobrazení** rozevírací nabídky a vyberte **Procházet**. Stránka je aktualizována, názvy zón zmizely a **odkazy** řízení zůstanou, kde je umístěn.
2. Chcete-li ukazují, že přizpůsobení funguje, zavřete prohlížeč a pak načtení stránky. Provedené změny se uloží pro budoucí relace prohlížeče.
3. Z **režim zobrazení** nabídce vyberte možnost **upravit**.   
  
   Každý ovládací prvek na stránce se nyní zobrazí s klesající šipku v záhlaví, který obsahuje příkazy rozevírací nabídky.
4. Klikněte na šipku zobrazení nabídky operací na **odkazy** ovládacího prvku. Klikněte **upravit** operaci.   
  
   **EditorZone** ovládací prvek se zobrazuje, zobrazení EditorPart ovládací prvky jste přidali.
5. V **vzhled** části ovládacích prvků pro úpravy, změny **název** do oblíbených, použijte **Chrome typ** rozevíracího seznamu vyberte **pouze název**a potom klikněte na **použít**. Na následujícím snímku obrazovky zobrazuje stránku v režimu úprav.

### <a name="web-parts-demo-page-in-edit-mode"></a>Webové části ukázkové stránky v režimu úprav


![Webové části VS – návod 3 – snímek obrazovky](profiles-themes-and-web-parts/_static/image5.gif)

**Obrázek 5**: webové části VS – návod 3 – snímek obrazovky


1. Klikněte **režim zobrazení** nabídce a vyberte **Procházet** se vraťte do režimu procházení.
2. Ovládací prvek teď má aktualizovaný titulek a žádné ohraničení, jak je znázorněno na následujícím snímku obrazovky.

### <a name="edited-web-parts-demo-page"></a>Upravená částí ukázkové webové stránky


![Webové části VS – návod 4 – snímek obrazovky](profiles-themes-and-web-parts/_static/image6.gif)

**Obrázek 4**: webové části VS – návod 4 – snímek obrazovky


### <a name="adding-web-parts-at-run-time"></a>Přidání webové části za běhu

Můžete také umožňují uživatelům přidávat webové části na jejich stránku za běhu. Uděláte to tak, nakonfigurujte stránky webových částí katalog, který obsahuje seznam webových částí, které chcete zpřístupnit uživatelům.

**Aby uživatelé mohli přidat webové části za běhu**

1. Otevřete stránku WebPartsDemo.aspx a přejděte do **návrhu** zobrazení.
2. Z **webové části** karty na panelu nástrojů, přetáhněte ovládací prvek CatalogZone do pravém sloupci tabulky, ybrat **EditorZone** ovládacího prvku.   
  
   Obě ovládacích prvků může být ve stejné tabulce buňku, protože se nezobrazí ve stejnou dobu.
3. V podokně vlastností přiřadit řetězec **přidat webové části** na vlastnost HeaderText **CatalogZone** ovládacího prvku.
4. Z **webové části** části na panelu nástrojů, přetáhněte ovládací prvek DeclarativeCatalogPart do oblasti obsahu **CatalogZone** ovládacího prvku.
5. Klikněte na šipku v pravém horním rohu **DeclarativeCatalogPart** řídit k zobrazení jeho nabídky úkolů a potom vyberte **upravit šablony**.
6. Z **standardní** oddílu na panelu, přetáhněte **odesílání souborů při odpovědích** řízení a **kalendáře** řízení do **WebPartsTemplate** oddíl **DeclarativeCatalogPart** ovládacího prvku.
7. Přepnout na **zdroj** zobrazení. Zkontrolujte zdrojový kód &lt;asp: catalogzone&gt; elementu. Všimněte si, že **DeclarativeCatalogPart** obsahuje ovládací prvek &lt;webpartstemplate&gt; elementu pomocí dvou ovládacích prvků závorkách serveru, které budou moct přidat na stránku z katalogu.
8. Přidat **název** vlastnost všechny ovládací prvky, které jste přidali do katalogu pomocí zobrazí hodnotu řetězce pro každý název v následujícím příkladu kódu. Přestože název se nenachází ve vlastnosti můžete obvykle nastavit tyto dvě serverové ovládací prvky v době návrhu, pokud uživatel přidá tyto ovládací prvky **WebPartZone** zóny z katalogu v době běhu, že se každý je uzavřen do  **GenericWebPart** ovládacího prvku. To umožňuje, aby fungoval jako webové části, tak budou moci zobrazit názvy.   
  
   Kód pro dvou ovládacích prvků, které jsou součástí **DeclarativeCatalogPart** ovládacího prvku by měla vypadat takto. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Uložte stránky.

Nyní můžete otestovat katalogu.

### <a name="to-test-the-web-parts-catalog"></a>K otestování webové části katalogu

1. Načtěte stránku v prohlížeči.
2. Klikněte **režim zobrazení** rozevírací nabídky a vyberte **katalogu**.   
  
   Katalog s názvem **přidat webové části** se zobrazí.
3. Přetáhněte **oblíbených** řízení ze zóny hlavní zpět na začátek bočním panelu zóny a umístěte jej.
4. V **přidat webové části** katalogu, vyberte obě políčka a pak vyberte **hlavní** ze seznamu rozevírací seznam obsahující dostupná zóny.
5. Klikněte na tlačítko **přidat** v katalogu. Ovládací prvky jsou přidány do hlavní zóny. Pokud chcete, můžete přidat více instancí ovládacích prvků z katalogu na stránku.   
  
   Na následujícím snímku obrazovky zobrazuje stránku s ovládací prvek pro uložení souboru a kalendář v hlavní zóny. 

![Ovládací prvky přidané do zóny hlavní z katalogu](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Klikněte **režim zobrazení** rozevírací nabídky a vyberte **Procházet**. Katalog zmizí a aktualizaci stránky.
7. Zavřete prohlížeč. Znovu načtěte stránku. Provedené změny zachovat.
