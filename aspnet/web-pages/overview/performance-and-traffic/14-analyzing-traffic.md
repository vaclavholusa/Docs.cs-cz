---
uid: web-pages/overview/performance-and-traffic/14-analyzing-traffic
title: "Sledování návštěvníka informace (Analytics) pro ASP.NET Web Pages lokality (Razor) | Microsoft Docs"
author: tfitzmac
description: "Poté, co jste, že jste podmínky webu přechodem, můžete analyzovat provoz vašeho webu."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 360bc6e1-84c5-4b8e-a84c-ea48ab807aa4
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/14-analyzing-traffic
msc.type: authoredcontent
ms.openlocfilehash: 9a381ebaed30325fdfa5f0f558910d3002c61559
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="tracking-visitor-information-analytics-for-an-aspnet-web-pages-razor-site"></a>Sledování návštěvníka informace (Analytics) pro stránku ASP.NET – webové stránky (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje, jak přidat web analytics na stránky na webu technologie ASP.NET Web Pages (Razor) pomocí pomocné rutiny.
> 
> Získáte informace:
> 
> - Postup odeslání informací o webu provozu do poskytovatele analytics.
> 
> Jsou to ASP.NET programování v článku nové funkce:
> 
> - `Analytics` Pomocné rutiny.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Knihovnu ASP.NET Web Helpers (balíček NuGet)


Analýza je obecný termín pro technologie, která měří provoz na vašem webu můžete pochopit, jak lidé použít webu. Mnoho analytické služby jsou k dispozici, včetně služeb z Google, Yahoo, StatCounter a dalších.

Způsob analýzy, které funguje je, že zaregistrujete k účtu pomocí zprostředkovatele datové analýzy, ve které registrujete lokality, které chcete sledovat. Zprostředkovatel odešle fragment kódu jazyka JavaScript, který obsahuje ID nebo sledování kód pro váš účet. Přidejte fragment kódu JavaScript do webové stránky v lokalitě, která chcete sledovat. (Obvykle přidáte fragmentu analytics zápatí nebo rozložení stránky nebo jiné jazyka HTML, který se zobrazí na každé stránce ve vaší lokalitě.) Pokud uživatelé požadují stránky, který obsahuje jednu z těchto fragmenty kódu jazyka JavaScript, fragmentu odesílá informace o aktuální stránku do zprostředkovatele datové analýzy, který zaznamenává různé podrobnosti o stránce.

Pokud chcete Podíváme se na vaší lokality statistiky, jste přihlášení k webu zprostředkovatele datové analýzy. Potom můžete zobrazit nejrůznějším sestavy o vašem webu, jako jsou:

- Počet zobrazení stránky pro jednotlivé stránky. To odpovídá (přibližně) kolik lidí připojeni k serveru, a jsou nejoblíbenější stránky, které na váš web.
- Jak dlouho uživatelé vynaložit na konkrétní stránky. To vám sdělí takové věci, jako jestli je domovská stránka uchovávání zájmu uživatelů.
- Jaké weby uživatelé byly předtím, než se tak stalo vaší lokality. To vám pomůže pochopit zda provozu pochází z odkazy z hledání a tak dále.
- Když uživatelé, kteří navštíví váš web a zůstanou jak dlouho.
- Jaké země návštěvnících jsou z.
- Jaké prohlížeče a operační systémy návštěvnících používají.

    ![Ch14traffic-1](14-analyzing-traffic/_static/image1.jpg)

## <a name="using-a-helper-to-add-analytics-to-a-page"></a>Přidat Analytics na stránku pomocí pomocné rutiny

Rozhraní ASP.NET Web Pages zahrnuje několik Pomocníci analytics (`Analytics.GetGoogleHtml`, `Analytics.GetYahooHtml`, a `Analytics.GetStatCounterHtml`), můžete snadno spravovat fragmenty kódu JavaScript použít pro analýzu. Místo přijít na to, jak a kde uvést kód JavaScript, stačí je přidat na stránku pomocné rutiny. Veškeré informace, které budete muset zadat je váš název účtu, ID nebo sledování kód. (Pro StatCounter, je také nutné zadat pár dalších hodnot.)

V tomto postupu vytvoříte rozložení stránky, která používá `GetGoogleHtml` pomocné rutiny. Pokud již máte účet s jedním z dalších zprostředkovatelů analýzy, můžete místo toho použijte tento účet a podle potřeby proveďte mírné úpravy.

> [!NOTE]
> Když vytvoříte účet analytics, je zaregistrovat adresu URL webu, který chcete sledovat. Pokud testujete všechno, co v místním počítači, můžete nebude sledovat skutečná provoz (pouze provoz je můžete), takže nebudete moct záznamů a zobrazení statistiky lokality. Ale tento postup ukazuje, jak přidat pomocná analytics na stránku. Při publikování webu živý web odešle informace ke zprostředkovateli analytics.


1. Přidejte knihovnu ASP.NET Web Helpers na váš web, jak je popsáno v [instalaci pomocné rutiny v stránku ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=252372), pokud jste to ještě už přidali.
2. Vytvořte účet s Google Analytics a zaznamenejte název účtu.
3. Vytvoření rozložení stránky s názvem *Analytics.cshtml* a přidejte následující kód:

    [!code-cshtml[Main](14-analyzing-traffic/samples/sample1.cshtml)]

    > [!NOTE]
    > Je nutné umístit volání `Analytics` pomocné rutiny v těle webové stránky (před `</body>` značka). Prohlížeč, jinak nebude spusťte skript.

    Pokud používáte jiný analytics poskytovatele, použijte jednu z následujících pomocné místo:

    - (Yahoo)`@Analytics.GetYahooHtml("myaccount")`
    - (StatCounter)`@Analytics.GetStatCounterHtml("project", "security")`
4. Nahraďte `myaccount` s názvem účtu, ID nebo sledování kód, který jste vytvořili v kroku 1.
5. Spusťte stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.)
6. V prohlížeči zobrazte zdrojový kód stránky. Budete moct najdete ve vykreslené analýzy kódu:

    [!code-html[Main](14-analyzing-traffic/samples/sample2.html)]
7. Přihlášení na webu Google Analytics a prozkoumat statistiku pro váš web. Pokud používáte stránky na živý web, uvidíte položku protokoly najdete na stránku.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

- [Google Analytics lokality](https://www.google.com/analytics/)
- [Yahoo! Analýza webu](http://help.yahoo.com/l/us/yahoo/ywa/)
- [StatCounter lokality](http://statcounter.com/)
