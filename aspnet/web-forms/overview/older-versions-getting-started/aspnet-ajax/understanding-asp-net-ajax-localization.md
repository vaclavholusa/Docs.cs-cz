---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Principy lokalizace technologie ASP.NET AJAX | Dokumentace Microsoftu
author: scottcate
description: Lokalizace je proces návrhu a integrace podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo součásti aplikace. Povinná kontrola úrovně důvěryhodnosti...
ms.author: aspnetcontent
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: ce6404ce4faa1018a4f8118f6167a4f93956abd3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815002"
---
<a name="understanding-aspnet-ajax-localization"></a>Principy lokalizace technologie ASP.NET AJAX
====================
podle [– Scott Cate](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalizace je proces návrhu a integrace podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo součásti aplikace. Platformě Microsoft ASP.NET poskytuje rozsáhlou podporu pro lokalizaci pro standardní aplikace ASP.NET integrací standardní .NET – model lokalizace; Microsoft AJAX Framework využívat integrované modelu mají podporovat nejrůznější scénáře, ve kterých lze provést lokalizace.


## <a name="introduction"></a>Úvod

Technologie ASP.NET společnosti Microsoft přináší objektově orientované a založený na událostech programovací model a sjednotí s výhodami zkompilovaný kód. Svůj model zpracování na straně serveru má však několik nevýhody vyplývajících z technologie, z nichž mnohá vyřešíte tak, že nové funkce zahrnuté v oboru názvů System.Web.Extensions, který zapouzdřuje službě Microsoft AJAX v rozhraní .NET Framework 3.5. Tato rozšíření umožňují mnoho plně funkčního klienta s funkcí dříve k dispozici jako součást rozšíření AJAX technologie ASP.NET 2.0, ale teď součástí základní knihovny tříd rozhraní Framework. Ovládací prvky a funkce v tomto oboru názvů zahrnout částečného zobrazení stránek, bez nutnosti úplná aktualizace stránky, možnost přístupu k webovým službám prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci), a rozsáhlé API na straně klienta navržené pro mnoho zrcadlení schémata ovládací prvek v sadě serverový ovládací prvek technologie ASP.NET.

Tento dokument White Paper zkoumá součástí Microsoft AJAX Framework a Microsoft AJAX Library skriptu, v rámci obchodní potřebě pro podporu lokalizace a kontrola již integrovaná podpora lokalizace ve webovém lokalizace funkce aplikace poskytované rozhraní .NET Framework. Knihovně Microsoft AJAX skript využívá formátu souboru .resx již používá aplikace .NET, který poskytuje integrovaná podpora integrovaného vývojového prostředí a typ ke sdílení prostředků.

Tento dokument White Paper je založen na verzi beta verzi 2 sady Microsoft Visual Studio 2008. Tento dokument White Paper také předpokládá, že můžete pracovat s Visual Studio 2008, nikoli Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio. Šablony projektů, které mohou být k dispozici v aplikaci Visual Web Developer Express bude využívat několik ukázek kódu.

## <a name="the-need-for-localization"></a>*Třeba pro lokalizaci*

Zejména pro vývojářům aplikací pro podniky a vývojáře komponent přestal stále nezbytné schopnost vytvářet nástroje, které mohou být zohledňovat rozdíly mezi jazykové verze a jazyky. Navrhování součásti s možností pro přizpůsobení na národní prostředí klienta zvyšuje produktivitu vývojářů a snižuje množství práce potřebné pro přizpůsobení součásti fungovat globálně.

Lokalizace je proces návrhu a integrace podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo součásti aplikace. Platformě Microsoft ASP.NET poskytuje rozsáhlou podporu pro lokalizaci pro standardní aplikace ASP.NET integrací standardní .NET – model lokalizace; Microsoft AJAX Framework využívat integrované modelu mají podporovat nejrůznější scénáře, ve kterých lze provést lokalizace. S Microsoft AJAX Framework může být skripty lokalizována nasazované do satelitních sestavení, nebo s využitím strukturu systému statický soubor.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Vkládání skripty s satelitní sestavení*

Konzistentní s standardní strategií lokalizace rozhraní .NET Framework, prostředky mohou být součástí satelitních sestavení. Satelitní sestavení poskytuje několik výhod přes tradiční prostředků zařazení do binárních souborů – je možné aktualizovat jakékoli dané lokalizace bez aktualizace větší obrázek, je možné nasadit další lokalizace jednoduše tak, že instalace do satelitních sestavení složky projektu a satelitní sestavení je možné nasadit bez způsobení opakované načtení projektu hlavní sestavení. Zejména v projektech ASP.NET to je užitečné, protože může výrazně snížit množství systémové prostředky využívané třídou přírůstkové aktualizace a minimálně naruší produkční účely v webu.

Skripty jsou vloženy do sestavení uvedete v spravovaný .resx (nebo zkompilovány .resources) soubory, které jsou zahrnuty do sestavení v době kompilace. Svoje prostředky jsou pak dostupné aplikace skriptu prostřednictvím AJAX runtime generovaný kód, prostřednictvím atributy úrovně sestavení

*Zásady vytváření názvů pro vložené soubory skriptů*

Správa skriptů Microsoft AJAX Framework podporuje širokou škálu možností pro použití při nasazení a testování skriptů a pokyny jsou k dispozici pro usnadnění těchto možností.

*K usnadnění ladění:*

Skripty vydání (produkční) by neměla zahrnovat `.debug` kvalifikátoru v názvu souboru. Skripty navržené pro ladění by měl obsahovat `.debug` v názvu souboru.

*Pro usnadnění lokalizace:*

Neutrální jazykové verze skriptů by neměl obsahovat žádné identifikátor jazykové verze názvu souboru. Pro skripty, které obsahují lokalizované prostředky musí být zadán kód jazyka ISO v názvu souboru. Například `es-CO` zkratka pro španělštinu, Kolumbie.

Následující tabulka shrnuje s příklady konvence pojmenování souborů:

| Název souboru | Význam |
| --- | --- |
| Script.js | Skript nezávislá na jazykové verzi, vydání verze. |
| Script.debug.js | Ladicí verze skriptu nezávislá na jazykové verzi. |
| Script.en-US.js | Vydání verze Angličtina, USA skriptu. |
| Script.debug.es-CO.js | Ladicí verze španělské, Kolumbie skriptu. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Návod: Vytvoření lokalizovaného, vložený skript

*Poznámka: Tento postup vyžaduje použití sady Visual Studio 2008, jak Visual Web Developer Express nezahrnuje šablona projektu pro projekty knihovny tříd.*

1. Vytvořte nový projekt webu pomocí technologie ASP.NET AJAX rozšíření integrována. Vytvořte nový projekt, projekt knihovny tříd v rámci řešení volá LocalizingResources.
2. Přidáte soubor Jscript s názvem VerifyDeletion.js LocalizingResources projektu, stejně jako soubory prostředků RESX názvem DeletionResources.resx a DeletionResources.es.resx. První bude obsahovat neutrální jazykové verze prostředků; Ten bude obsahovat Španělština – jazykové prostředky.
3. Přidejte následující kód do VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Pro ty obeznámeni se syntaxí využívající regulární výrazy jazyka JavaScript, text v rámci jednoho lomítka (v předchozím příkladu je příkladem /FILENAME/) označuje RegExp – objekt. Knihovna MSDN obsahuje rozsáhlou reference na JavaScript a prostředky na nativních objektů jazyka JavaScript najdete online.

1. Přidejte následující řetězce prostředků DeletionResources.resx: 

    **VerifyDelete**: Opravdu chcete odstranit název souboru?

    **Odstranit**: název souboru byl odstraněn.

1. Přidejte následující řetězce prostředků DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro hledně desee quitar FILENAME?

    **Odstranit**: název souboru se ha quitado.
2. Přidejte následující řádky kódu do souboru AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Přidáte odkazy na System.Web a System.Web.Extensions LocalizingResources projektu.
2. Přidáte odkaz na projekt LocalizingResources z projektu webové stránky.
3. V default.aspx v projektu webu aktualizujte ovládacího prvku ScriptManager následující další značky:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. V default.aspx kdekoli na stránce patří tento kód:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Stiskněte klávesu F5. Pokud se zobrazí výzva, povolte ladění. Když je stránka načtená, stiskněte klávesu Delete. Všimněte si, výzva se zobrazí v angličtině (Pokud je váš počítač nastavený preferovat Španělština – jazykové prostředky ve výchozím nastavení) pro potvrzení.
2. Zavřete okno prohlížeče a vraťte se na default.aspx. V @Page záhlaví směrnice, automaticky nahradit Culture a UICulture s es-ES. Stisknutím klávesy F5 spusťte webovou aplikaci v prohlížeči znovu. Tentokrát, mějte na paměti, že budete vyzváni k odstranění souboru ve španělštině:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-localization/_static/image6.png))


Všimněte si, že existuje několik variant v tomto návodu. Například skripty nelze zaregistrovat pomocí ovládacího prvku ScriptManager prostřednictvím kódu programu během načítání stránky.

## <a name="including-a-static-script-file-structure"></a>*Včetně struktura souborů statického skriptu*

Při používání statické soubory skriptů pro nasazení, ztratí se některé z výhod používání vlastní schéma lokalizace .NET. Je primárně zobrazen přijít o automatické typu generované včetně souborů skriptu prostředků; ve výše uvedeného návodu například prostředky byly vystavené automaticky generovaný typ, který volá zprávy z ovládacího prvku ScriptManager.

Existují však některé výhody struktury statického skriptu. Aktualizací je možné provést bez opětovné kompilace a opětovné nasazení satelitní sestavení a použití statické struktury je možné provést pro přepsání vložený skript pro integraci dílčí část funkce, která nemůže dodána s komponentou.

Společnost Microsoft doporučuje, jak se vyhnout problém ovládacího prvku verze pomocí automatického generování skriptu zdroje během kompilace projektu. Při údržbě základní kód skriptu v rozsáhlé, může být stále obtížnější zajistit, že změny kódu se projeví v každé lokalizované skriptu. Jako alternativu můžete jednoduše spravovat jeden skript logiky a několik skriptů lokalizace slučování souborů při sestavování projektu.

Protože nejsou k dispozici prostředky deklarativně zahrnout, statické soubory by měly být skriptu odkazovat buď přidáním `<asp:ScriptElement>` prvky jako podřízený objekt `<Scripts>` značky ovládacího prvku ScriptManager nebo prostřednictvím kódu programu přidáním `ScriptReference` objekty Chcete `Scripts` vlastnost `ScriptManager` ovládacího prvku na stránku za běhu.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Prvek ScriptManager a jejich rolí v lokalizace*

Prvek ScriptManager umožňuje různé automatické chování v případě lokalizovaných aplikací:

- Automaticky vyhledá soubory skriptu na základě nastavení a konvence pojmenování; pro instanci načte povolené ladění skriptů v režimu ladění a načte lokalizované skripty na základě výběru rozhraní prohlížeče uživatele.
- Umožňuje definici jazykové verze, včetně vlastních jazykových verzí.
- Komprese souborů skriptu umožňuje pomocí protokolu HTTP.
- Ukládá do mezipaměti skripty efektivně spravovat mnoho požadavků.
- Přidá úroveň dereference na skripty přesměrováním prostřednictvím šifrované adresy URL.

Odkazy na skript je přidat do ovládacího prvku ScriptManager buď programově, nebo pomocí deklarativní. Deklarativní je zvláště užitečná při práci se skripty vložená v sestavení než webový projekt, jako název souboru, který pravděpodobně nezmění jako revize se nasdílejí.

## <a name="summary"></a>Souhrn

Růstem webové aplikace oslovit větší cílové skupiny, nemusí být schopný připojit širší komunity a kultury stane core obchodního modelu; e-commerce webové aplikace musí být schopny vypořádat se cizích měn, systémy správy obsahu musí být schopen nejen obsah, ale také jejich navigace pomocné parametry a pole formuláře v jiných jazycích a společnosti potřebovat vědět, že se tyto potřeby přístupné.

Rozhraní .NET Framework podporuje vnitřně bohaté lokalizace architektura využívající satelitní sestavení a souborů XML prostředky (RESX) zobrazíte jednotným způsobem k vyhledání prostředků řetězců a obrázků. Rozšíření ASP.NET AJAX, včetně Microsoft AJAX Framework a Microsoft AJAX Library skriptu, podporují tento programovací model do kódu na straně klienta, povolení vyhledávání snadno prostředků řetězce. Satelitní sestavení podporují automatické zahrnutí skript prostředků (soubory skutečné js) prostřednictvím ScriptResource.axd tak dlouho, dokud názvy souborů, postupujte podle dané schéma pojmenování. Díky této podpoře rozšíření ASP.NET AJAX zjednodušit lokalizace skripty a globalizace aplikace.

## <a name="bio"></a>*Životopis*

Scott Cate má práce s Microsoft webových technologiích od roku 1997 a je prezident myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na technologie ASP.NET psaní aplikací, zaměřuje na znalostní báze softwarová řešení založených na. Scott můžete kontaktovat prostřednictvím e-mailové adrese [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo na svém blogu [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [další](understanding-asp-net-ajax-web-services.md)
