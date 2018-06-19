---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profil a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse | Microsoft Docs
author: Rick-Anderson
description: Balíčku glimpse je neúspěchu a rozšiřujících se řadu balíčky NuGet s otevřeným zdrojem, který poskytuje podrobné výkon, ladění a diagnostické informace pro technologii ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872972"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Profil a ladění aplikace ASP.NET MVC pomocí balíčku Glimpse
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> Balíčku glimpse je neúspěchu a rozšiřujících se řadu balíčky NuGet s otevřeným zdrojem, který poskytuje podrobné výkon, ladění a diagnostické informace pro aplikace ASP.NET. Je trivial k instalaci, lightweight, ultrarychlého a zobrazí klíčové metriky výkonu v dolní části každé stránky. Umožňuje rozbalit vaší aplikace, když potřebujete zjistit, co se děje na serveru. Balíčku glimpse poskytuje mnohem cenné informace, doporučujeme, že můžete ji použít v celé vaší cyklu vývoje, včetně Azure testovacího prostředí. Při [Fiddler](http://www.telerik.com/fiddler) a [nástroje pro vývoj F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) poskytují straně klienta zobrazení balíčku Glimpse poskytuje podrobný pohled ze serveru. V tomto kurzu se soustředí na pomocí balíčku Glimpse ASP.NET MVC a EF balíčků, ale jsou k dispozici mnoho dalších balíčků. Kde to bude možné I odkaz na příslušné [balíčku Glimpse dokumentace](http://getglimpse.com/Docs/) který I pomáhají udržovat. Balíčku glimpse je opensourcový projekt, příliš se může přispívat k zdrojový kód a dokumentaci.


- [Instalace balíčku Glimpse](#ig)
- [Povolit balíčku Glimpse pro místního hostitele](#eg)
- [Na kartě časové osy](#Time)
- [Vazby modelu](#mb)
- [Trasy](#route)
- [Pomocí balíčku Glimpse na Azure](#da)
- [Další zdroje informací](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalace balíčku Glimpse

Balíčku Glimpse můžete nainstalovat z konzoly Správce balíčků NuGet nebo **spravovat balíčky NuGet** konzoly. V této ukázce nainstaluji Mvc5 a EF6 balíčky:

![instalace balíčku Glimpse z NuGet dialogové okno](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Vyhledejte *Glimpse.EF*

![Glimpse.EF z dialogové okno instalace NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Výběrem **nainstalované balíky**, můžete zobrazit závislé moduly balíčku Glimpse nainstalován:

![Instalovat balíčky balíčku Glimpse z dialogové okno](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Následující příkazy instalace balíčku Glimpse MVC5 a EF6 modulů z konzoly Správce balíčků:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Povolit balíčku Glimpse pro místního hostitele

Přejděte na http://localhost: &lt;portu #&gt;/glimpse.axd a klikněte na <strong>balíčku Glimpse zapnout</strong> tlačítko.

![Stránka axd balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Pokud máte panelu Oblíbené položky zobrazí, vám může přetahování tlačítka balíčku Glimpse a přidejte je jako bookmarklets:

![Aplikace Internet Explorer s balíčku Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Teď můžete Navigovat vaší aplikace a **hlav si zobrazení** (HUD) se zobrazí v dolní části stránky.

![Obraťte se na správce stránka s HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Stránky balíčku Glimpse HUD](http://getglimpse.com/Docs/Heads-up-Display) časování podrobnosti uvedené výše. Zobrazí data HUD nerušivý výkonu vás může upozornit na problém okamžitě – předtím, než získáte cyklus testu. Kliknutím na &quot;g&quot; zobrazí v pravém dolním panelu balíčku Glimpse:

![Panel balíčku glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

Na obrázku výše [kartě provádění](http://getglimpse.com/Docs/Execution-Tab) je vybraná, který obsahuje podrobnosti časování akcí a filtrů v kanálu. Můžete zobrazit Moje [zastavit sledování filtru časovače](http://www.nuget.org/packages/StopWatch/) spustit ve fázi 6 kanálu. Při Moje lehká časovače může poskytnout užitečné profil/časování data, jeho neúspěšných přístupů do všech čas strávený v autorizace a vykreslování zobrazení. Další informace o Moje časovače v [profilu a času Azure vaše aplikace ASP.NET MVC úplně](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). [Karty](http://getglimpse.com/Docs/Tabs) stránka obsahuje odkazy na podrobné informace na každé kartě.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>Na kartě časové osy

Došlo tní Dykstra nezpracovaných [kurz EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) změnit řadiče vyučující následujícím kódem:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

Výše uvedený kód umožňuje mi předat v řetězci dotazu (`eager`) na ovládací prvek přes nebo explicitní načítání dat. Na následujícím obrázku se používá explicitní načítání a časování stránka zobrazuje každou registrace načten do `Index` metodu akce:

![explicitní načítání](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

V následujícím kódu, je zadána eager a načte jsou po každém zápisu `Index` se nazývá zobrazení:

![je zadán eager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Můžete podržet přes segment čas časování podrobné informace:

![Počkejte, až se najdete v podrobné časování](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Vazby modelu

[Kartě vazby modelu](http://getglimpse.com/Docs/Model-Binding-Tab) poskytuje širokou řadu informace, které vám pomohou pochopit, jak jsou svázané s proměnných formuláře a proč některé nejsou vázán, jako byste očekávali. Obrázek níže znázorňuje **?** ikonu, která můžete kliknutím na se zprovoznit stránku nápovědy balíčku glimpse této funkce.

![balíčku glimpse zobrazení vazby modelu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Trasy

 Na kartě balíčku Glimpse trasy se vám může pomoct ladění a pochopit směrování. Na obrázku níže produktu trasa se vybere (a zobrazuje zeleně balíčku Glimpse convention). ![Vybraný název produktu](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) tokeny omezení, oblasti a data trasy se také zobrazí. V tématu [trasy balíčku Glimpse](http://getglimpse.com/Docs/Routes-Tab) a [atribut směrování v ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) Další informace. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Pomocí balíčku Glimpse na Azure

Výchozí zásady zabezpečení balíčku Glimpse umožňuje pouze data balíčku Glimpse zobrazit z místního hostitele. Tuto zásadu zabezpečení můžete změnit, aby se dala zobrazit tato data na vzdáleném serveru (například webové aplikace v Azure). Pro testovací prostředí v Azure, přidejte zvýrazněná značka až do dolní části *Web.config* souboru balíčku Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

V této změně samostatně může každý uživatel zobrazit vaše data balíčku Glimpse na vzdálený web. Zvažte přidání značka výše k profil publikování, je nasazena pouze použité při použití tohoto profilu publikování (například vaše Azure testovací proifle.) Chcete-li omezit data balíčku Glimpse, přidáme `canViewGlimpseData` role a povolit pouze uživatelé v této roli k zobrazení dat balíčku Glimpse.

Odebere komentáře z *GlimpseSecurityPolicy.cs* soubor a změňte [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) volat z `Administrator` k `canViewGlimpseData` role:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Zabezpečení – bohaté údaje poskytnuté balíčku Glimpse mohla vystavit zabezpečení vaší aplikace. Microsoft neprovedl auditu zabezpečení balíčku glimpse pro použití v aplikacích výroby.


Informace o přidávání rolí, najdete v části Moje [nasazení webové aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) kurzu.

<a id="addRes"></a>
## <a name="additional-resources"></a>Další prostředky

- [Nasazení aplikace zabezpečené ASP.NET MVC 5 s členství, OAuth a databáze SQL Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Konfigurace balíčku glimpse](http://getglimpse.com/Docs/Configuration) -stránka dokumentace týkající se konfigurace karty, zásady modulu runtime, protokolování a další.
