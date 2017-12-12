---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: "Seznámení s ASP.NET AJAX lokalizace | Microsoft Docs"
author: scottcate
description: "Lokalizace je proces návrhu a integraci podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo určité součásti aplikace. Povinná kontrola úrovně důvěryhodnosti..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 5b801586ea77af78284f780fe47fe09cafb984af
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-localization"></a>Principy ASP.NET AJAX lokalizace
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Lokalizace je proces návrhu a integraci podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo určité součásti aplikace. Platformě Microsoft ASP.NET poskytuje rozsáhlou podporu pro lokalizaci pro standardní aplikace ASP.NET integrací standardní model lokalizace .NET; Rozhraní Microsoft AJAX využívat integrované modelu mají podporovat různé scénáře, ve kterých lze provést lokalizace.


## <a name="introduction"></a>Úvod

Technologie ASP.NET společnosti Microsoft poskytuje programovací model objektově orientované a událostmi řízené a jednotek s výhod zkompilovaný kód. Svůj model zpracování na straně serveru má však několik nevýhody vyplývajících z technologie, z nichž mnohé lze řešit novým funkcím zahrnutým v oboru názvů System.Web.Extensions, který zapouzdřuje služby Microsoft AJAX v rozhraní .NET Framework 3.5. Tato rozšíření umožňují mnoha plně funkčního klienta s funkcí, dříve k dispozici jako součást rozšíření ASP.NET 2.0 AJAX, ale teď součástí základní knihovny tříd Framework. Ovládací prvky a funkce v tomto oboru názvů zahrnout částečným vykreslením stránky bez nutnosti celou stránku obnovení, umožňuje přístup k webovým službám prostřednictvím klientského skriptu (včetně ASP.NET, rozhraní API pro profilaci) a rozhraní API rozsáhlé straně klienta navržené tak, aby mnoho zrcadlení ovládací prvek schémata, vidět v sadě serverový ovládací prvek ASP.NET.

Tento dokument White Paper prozkoumá nachází v knihovně skript AJAX společnosti Microsoft a Microsoft AJAX Framework v rámci obchodní potřeba pro podporu lokalizace a kontrola již integrovanou podporu pro lokalizaci ve webové lokalizace funkce aplikace poskytované rozhraní .NET Framework. Knihovna Microsoft AJAX skript využívá resx formát souboru již používá aplikace .NET, která poskytuje integrovanou podporu IDE a typ prostředku ke sdílení.

Tento dokument White Paper je založen na verzi beta verzi 2 sady Microsoft Visual Studio 2008. Tento dokument White Paper také předpokládá, že budou pracovat se Visual Studio 2008, není Visual Web Developer Express a poskytne návody podle uživatelského rozhraní sady Visual Studio. Některé ukázky kódu, bude využívat šablony projektů, které pravděpodobně není k dispozici v aplikaci Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*Třeba pro lokalizaci*

Zejména pro vývojáře aplikací a vývojáři komponent stala stále nutné schopnost vytvářet nástroje, které může být zohledňovat rozdíly mezi jazyky a jazykové verze. Navrhování komponenty s schopnost přizpůsobit se národní prostředí klienta zvyšuje produktivitu vývojářů a snižuje množství práce potřebné pro přizpůsobení součást globálně fungovat.

Lokalizace je proces návrhu a integraci podporu pro konkrétní jazyk a jazykovou verzi do aplikace nebo určité součásti aplikace. Platformě Microsoft ASP.NET poskytuje rozsáhlou podporu pro lokalizaci pro standardní aplikace ASP.NET integrací standardní model lokalizace .NET; Rozhraní Microsoft AJAX využívat integrované modelu mají podporovat různé scénáře, ve kterých lze provést lokalizace. Pomocí rozhraní AJAX Microsoft můžete být skripty lokalizované nasazován na satelitní sestavení nebo s využitím strukturu systému statických souborů.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Vložení skripty s satelitní sestavení*

Konzistentní s standardní strategie lokalizace rozhraní .NET Framework, prostředků můžou být součástí satelitní sestavení. Satelitní sestavení poskytují několik výhod přes tradiční prostředků zahrnutí v binárních souborech - jakékoli dané lokalizace lze aktualizovat bez aktualizace větší bitové kopie, jednoduše tak, že instalace satelitní sestavení do lze nasadit další lokalizace složky projektu a satelitních sestavení se dá nasadit aniž by to způsobilo opětovného načtení sestavení hlavní projekt. Zejména v projekty ASP.NET to je výhodné, protože může významně snížit množství systémové prostředky používané přírůstkové aktualizace a minimálně naruší produkční použití webu.

Skripty jsou vloženy do sestavení zahrnutím je spravovat resx (nebo zkompilovat .resources) soubory, které jsou zahrnuty do sestavení při kompilaci. Jejich prostředky jsou pak dostupné aplikace skriptu prostřednictvím AJAX generované modulem runtime kódu, prostřednictvím atributy úrovně sestavení

*Zásady vytváření názvů pro vložené soubory skriptů*

Správa skriptu Microsoft AJAX Framework podporuje celou řadu možností pro použití v nasazení a testování skriptů a jsou uvedeny pokyny pro usnadnění tyto možnosti.

*Pro usnadnění ladění:*

Skripty verze (produkční) by neměla zahrnovat `.debug` kvalifikátor v názvu souboru. Skripty určená pro ladění by měla obsahovat `.debug` v názvu souboru.

*Pro usnadnění lokalizace:*

Neutrální kultury skripty nesmí obsahovat žádné identifikátor jazykové verze názvu souboru. Pro skripty, které obsahují lokalizované prostředky musí být zadán kód ISO jazyka v názvu souboru. Například `es-CO` znamená Španělština Columbia.

Následující tabulka shrnuje soubor s příklady konvence vytváření názvů:

| Název souboru | Význam |
| --- | --- |
| Script.js | Skript neutrální jazykové verze, prodejní verzi. |
| Script.Debug.js | Ladicí verze skript neutrální jazykové verze. |
| Script.en US.js | Vydání verze Angličtina, USA skript. |
| Script.Debug.ES CO.js | Ladicí verze španělské, Columbia skript. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Návod: Vytvoření lokalizované, vloženého skriptu

*Poznámka: Tento postup vyžaduje použití sady Visual Studio 2008, protože Visual Web Developer Express nezahrnuje šablona projektu u projektů knihovny tříd.*

1. Vytvoření nového projektu webové stránky s integrovaná rozšíření ASP.NET AJAX. Vytvořte jiného projektu, projektu knihovny tříd, v rámci řešení názvem LocalizingResources.
2. Přidejte soubor Jscript s názvem VerifyDeletion.js do projektu LocalizingResources, stejně jako soubory RESX prostředky názvem DeletionResources.resx a DeletionResources.es.resx. První bude obsahovat prostředky neutrální jazykové verze; k tomu bude obsahovat prostředky Španělština jazyk.
3. Přidejte následující kód do VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Pro ty, které jsou obeznámeni s JavaScript Regex syntaxe, textu v rámci jedné lomítka (v předchozím příkladu je příklad /FILENAME/) označuje RegExp – objekt. V knihovně MSDN obsahuje rozsáhlé referenční dokumentace technologie JavaScript a prostředky v nativních objektů JavaScript lze nalézt online.

1. Přidejte následující řetězce prostředků DeletionResources.resx: 

    **VerifyDelete**: Opravdu chcete odstranit FILENAME?

    **Odstranit**: název souboru byl odstraněn.

1. Přidejte následující řetězce prostředků DeletionResources.es.resx: 

    **VerifyDelete**: Est seguro que desee quitar FILENAME?

    **Odstranit**: název souboru se ha quitado.
2. Přidejte následující řádky kódu do souboru AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Přidejte odkazy na System.Web a System.Web.Extensions LocalizingResources projektu.
2. Přidáte odkaz na projekt LocalizingResources z webový projekt.
3. V default.aspx pod webový projekt aktualizujte ovládací prvek ScriptManager následující další kód:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. V default.aspx kdekoli na stránce patří tyto značky:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Stiskněte klávesu F5. Pokud se zobrazí výzva, povolte ladění. Při načítání stránky klikněte na tlačítko Odstranit. Všimněte si, že budete vyzváni v angličtině (Pokud váš počítač je ve výchozím nastavení má přednost Španělština jazyk prostředky) k potvrzení.
2. Zavřete okno prohlížeče a vraťte se na default.aspx. V @Page – direktiva záhlaví, automaticky nahradit pro jazykovou verzi a UICulture s es-ES. Stisknutím klávesy F5 spuštění webové aplikace v prohlížeči znovu. Tentokrát, vezměte na vědomí, budete vyzváni k odstranění tohoto souboru ve španělštině:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-localization/_static/image6.png))


Všimněte si, že existuje několik variant pro účely tohoto postupu. Například skripty nelze zaregistrovat ovládací prvek ScriptManager prostřednictvím kódu programu při načítání stránky.

## <a name="including-a-static-script-file-structure"></a>*Včetně struktury souborů statické skriptu*

Při používání statické soubory skriptů pro nasazení, ztratíte některé z výhod použití schéma vyplývajících lokalizace .NET. Především viditelné je, že přijdete o automatické typ vygenerovat z včetně soubory skriptu prostředků; ve výše uvedené návod například prostředky byly vystavené automaticky generovaný typu s názvem zprávy z ovládacího prvku ScriptManager.

Existují však některé výhody struktury skriptu statických souborů. Aktualizace lze provádět bez nutnosti rekompilace a znovu nasazovat satelitní sestavení a používání statických souborů struktura provést také k přepsání vloženého skriptu k integraci dílčí část funkcí, které nemusí dodávané se součástí.

Společnost Microsoft doporučuje zabraňující problémem verze ovládacího prvku pomocí automatického generování prostředkům skriptu během kompilace projektu. Při údržbě zadání kódu rozsáhlé skriptu základní, může stát stále obtížné zajistit, že kód změny se projeví v každé lokalizované skriptu. Jako alternativu můžete jednoduše udržovat jeden skript logiku a několik skriptů lokalizace sloučení souborů při sestavování projektu.

Protože nejsou k dispozici prostředky deklarativně zahrnout, soubory by měly být statické skriptu odkazovat buď přidáním `<asp:ScriptElement>` elementy jako podřízenou `<Scripts>` značky ovládacího prvku ScriptManager nebo prostřednictvím kódu programu přidáním `ScriptReference` objekty na `Scripts` vlastnost `ScriptManager` ovládacího prvku na stránku za běhu.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*Prvek ScriptManager a její Role v lokalizace*

Prvek ScriptManager umožňuje několika automatické chování pro lokalizované aplikace:

- Automaticky vyhledá soubory skriptu na základě nastavení a zásady vytváření názvů; například načte povolit ladění skriptů v režimu ladění a zatížení lokalizované skripty na základě výběru rozhraní prohlížeče uživatele.
- Umožňuje definici jazykové verze, včetně vlastních jazykových verzí.
- Umožňuje kompresi skriptu souborů přes protokol HTTP.
- Ukládá do mezipaměti skripty, které efektivně spravovat mnoho požadavků.
- Přidá úroveň dereference skriptů tím je přes šifrované adresu URL.

Reference skriptu lze přidat na ovládací prvek ScriptManager buď programově, nebo deklarativní značky. Deklarativní je zvláště užitečná při práci s skripty vložený v sestavení než webový projekt, samostatně, jako název skriptu nezmění pravděpodobně jako revize odesílají.

## <a name="summary"></a>Souhrn

Jelikož webové aplikace k dosažení větší počet uživatelů, potřeba dosáhnout širší jazykové verze a komunit stane základní obchodní modelu; webové aplikace pro elektronické obchodování musejí mít jak nakládat s cizích měn, musí být schopný, nejen přítomen jejich obsah, ale jejich navigační odkazy na servery a pole formuláře v jiných jazyků a společnosti potřebujete vědět, že je tato potřebná systémy správy obsahu přístupné.

Rozhraní .NET Framework vnitřně podporuje bohaté lokalizace framework, využívá satelitní sestavení a soubory XML prostředků (RESX) jednotným způsobem k vyhledání řetězce prostředků a bitové kopie k dispozici. Rozšíření ASP.NET AJAX, včetně Microsoft AJAX Framework a knihovna skript AJAX Microsoft, podporují tento programovací model do kódu na straně klienta, povolení vyhledávání řetězec snadno prostředků. Satelitní sestavení podporují automatické zahrnutí prostředků skriptu (skutečné .js soubory) prostřednictvím ScriptResource.axd tak dlouho, dokud danou schéma pojmenování podle názvy souborů. Tato podpora rozšíření ASP.NET AJAX zjednodušit skripty lokalizace a globalizace aplikací.

## <a name="bio"></a>*Bio*

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Předchozí](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
[další](understanding-asp-net-ajax-web-services.md)
