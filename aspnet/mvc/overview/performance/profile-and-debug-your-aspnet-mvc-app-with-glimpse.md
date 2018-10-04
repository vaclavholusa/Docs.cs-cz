---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilování a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse | Dokumentace Microsoftu
author: Rick-Anderson
description: Balíčku glimpse je neúspěchu a rostoucí řadu opensourcových balíčků NuGet, která poskytuje podrobné výkonu, ladění a diagnostických informací pro ASP.NET...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577285"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profilování a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse
====================
Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Balíčku glimpse je neúspěchu a rostoucí řadu opensourcových balíčků NuGet, která poskytuje podrobné výkonu, ladění a diagnostických informací pro aplikace ASP.NET. Je triviální k instalaci, zjednodušené, ultrarychlých a klíčové metriky výkonu se zobrazí v dolní části každé stránky. Umožňuje vám a přejít k podrobnostem do vaší aplikace, když budete chtít zjistit, co se děje na serveru. Balíčku glimpse poskytuje mnohem cenné informace, které doporučujeme že použít v celém cyklu vývoje, včetně Azure testovacího prostředí. Zatímco [Fiddler](http://www.telerik.com/fiddler) a [F-12 vývojových nástrojů](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) poskytují na straně klienta zobrazení, balíčku Glimpse poskytuje podrobné zobrazení ze serveru. Tento kurz se zaměřuje na pomocí balíčku Glimpse ASP.NET MVC a balíčky EF, ale jsou k dispozici řada dalších balíčků. Kde je to možné nemohu propojit na příslušné [Nakoukněte dokumentace](http://getglimpse.com/Docs/) které můžu pomáhají udržovat. Balíčku glimpse je opensourcový projekt, příliš se může přispívat do zdrojového kódu a dokumenty.


- [Instalace balíčku Glimpse](#ig)
- [Povolit balíčku Glimpse pro místního hostitele](#eg)
- [Na kartě časové osy](#Time)
- [Vazby modelu](#mb)
- [Trasy](#route)
- [Pomocí balíčku Glimpse v Azure](#da)
- [Další prostředky](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalace balíčku Glimpse

Balíčku Glimpse můžete nainstalovat z konzoly Správce balíčků NuGet nebo **spravovat balíčky NuGet** konzoly. Pro tuto ukázku nainstaluji Mvc5 a EF6 balíčky:

![instalace balíčku Glimpse z NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Vyhledejte *Glimpse.EF*

![Glimpse.EF z dlg instalace NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Výběrem **nainstalované balíčky**, uvidíte balíčku Glimpse závislé moduly, které jsou nainstalovány:

![Nainstalované balíčky balíčku Glimpse z DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Následující příkazy instalace balíčku Glimpse MVC5 a EF6 modulů z konzoly Správce balíčků:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Povolit balíčku Glimpse pro místního hostitele

Přejděte do http://localhost:&lt; port #&gt;/glimpse.axd a kliknutím <strong>balíčku Glimpse zapnout</strong> tlačítko.

![Stránka axd balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Pokud máte panel Oblíbené zobrazí, lze přetáhnout a rozevírací tlačítka balíčku Glimpse a přidejte je jako bookmarklets:

![Aplikace Internet Explorer s boookmarklets balíčku Glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Nyní se můžete dostat vaše aplikace a **vedoucí nahoru zobrazení** (HUD) se zobrazí v dolní části stránky.

![Správce kontaktů stránka s HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Stránky balíčku Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) podrobnosti výše uvedené informace o časování. Zobrazí data HUD nerušivý výkonu může upozornit na problém okamžitě – než se ponoříte do testovacího cyklu. Kliknutím na &quot;g&quot; zobrazí v pravém dolním rohu panelu balíčku Glimpse:

![Panel balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na obrázku výše [karta spuštění](http://getglimpse.com/Docs/Execution-Tab) zaškrtnuto, zobrazí podrobnosti časování akce a filtry v kanálu. Můžete zobrazit Moje [zastavit sledování filtru časovače](http://www.nuget.org/packages/StopWatch/) začínají na fázi 6 kanálu. Moje lehký časovače můžete nabízejí užitečné profilu a data časování, ho výpadky celou dobu trvání autorizace a vykreslení zobrazení. Informace o mé časovač v [profilu a času vaše aplikace ASP.NET MVC úplně přenést do Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Karty](http://getglimpse.com/Docs/Tabs) stránka obsahuje odkazy na podrobné informace na každé kartě.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na kartě časové osy

Upravili Petr Dykstra nezpracovaných [kurz EF 6 a MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) následujícím kódem změnit Instruktoři kontroleru:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Výše uvedený kód umožňuje předat řetězec dotazu (`eager`) do správy dočkat nebo explicitní načtení dat. Na následujícím obrázku se používá explicitní načtení a časování stránka zobrazuje každou registrace načten v `Index` metody akce:

![explicitní načtení](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

V následujícím kódu je zadán eager a načte se po každé registraci `Index` se nazývá zobrazení:

![je zadán eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Při najetí myší nad segment času na získání podrobných informací o časování:

![najetím myši zobrazíte podrobné časování](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Vazby modelu

[Kartu vazby modelu](http://getglimpse.com/Docs/Model-Binding-Tab) nabízí celou řadu informace, které vám pomohou pochopit, jak jsou svázány proměnných formuláře a proč některé nejsou vázány podle očekávání. Obrázek níže ukazuje **?** ikonu, která můžete kliknout na zobrazíte na stránce nápovědy balíčku glimpse této funkce.

![Nakoukněte zobrazení vazby modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Trasy

 Na kartě balíčku Glimpse trasy můžete vám pomůže ladit a lepší pochopení směrování. Na následujícím obrázku produktu trasa se vybere (a zobrazí zeleně, vytváření balíčku Glimpse). ![Vybraný název produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) trasy tokeny omezení, oblastí a data se zobrazují. Zobrazit [trasy balíčku Glimpse](http://getglimpse.com/Docs/Routes-Tab) a [směrováním atributů v ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Další informace. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Pomocí balíčku Glimpse v Azure

Výchozí zásady zabezpečení balíčku Glimpse povoluje jenom data balíčku Glimpse zobrazený z místního hostitele. Tyto zásady zabezpečení můžete změnit, abyste mohli zobrazit tato data na vzdáleném serveru (například webové aplikace v Azure). Pro testovací prostředí v Azure, přidejte značku zvýrazněné až do dolní části *web.confg* soubor balíčku Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Díky této změně samostatně, každý uživatel svá data můžete zobrazit balíčku Glimpse na vzdálený web. Zvažte přidání značky nad profil publikování, obsahuje pouze nasazené použité při použití tohoto profilu publikování (například váš test na platformě Azure proifle.) Pokud chcete omezit data balíčku Glimpse, přidáme `canViewGlimpseData` role a povolit pouze uživatelé v této roli k zobrazení dat balíčku Glimpse.

Odebrat komentáře z *GlimpseSecurityPolicy.cs* soubor a změňte [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) volání z `Administrator` k `canViewGlimpseData` role:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Zabezpečení – bohaté údaje poskytnuté balíčku Glimpse může zpřístupnit zabezpečení vaší aplikace. Microsoft neprovedl auditu zabezpečení balíčku glimpse pro použití v produkční aplikace.


Informace o přidání rolí, najdete v části Moje [nasazení webové aplikace zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) kurzu.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Nasaďte zabezpečené aplikace ASP.NET MVC 5 s Membership, OAuth a SQL Database do Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Nakoukněte konfigurace](http://getglimpse.com/Docs/Configuration) -Doc stránky na karty, zásady modulu runtime, protokolování a další konfigurace.
