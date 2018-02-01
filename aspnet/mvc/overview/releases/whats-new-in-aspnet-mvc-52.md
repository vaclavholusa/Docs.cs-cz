---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: "Co je nového v architektuře ASP.NET MVC 5.2 | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 94f6131fdb86d1694c1f563c5f6806f119c71266
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-52"></a>Co je nového v architektuře ASP.NET MVC 5.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového pro ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) a [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Požadavky na software](#softRequire)
- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v architektuře ASP.NET MVC 5.2](#new-features)

    - [Atribut směrování vylepšení](#attributerouting)
- [Známé problémy a nejnovější změny](#knownbreakingchanges)
- [Opravy chyb](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Požadavky na software

- Sadu Visual Studio 2012: Stáhněte si [ASP.NET a webové nástroje pro sadu Visual Studio 2012 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) nebo vyšší. Tato aktualizace je potřeba pro úpravu zobrazení syntaxe Razor 5.2 ASP.NET MVC.

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace. Nejnovější balíček ASP.NET MVC 5.2 má následující verze: "5.2.0". Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.

Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

Instalovat balíček Microsoft.AspNet.Mvc-verze 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET MVC 5.2 jsou dostupné na webu technologie ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nové funkce v architektuře ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrováním atributů poskytuje bod rozšiřitelnosti názvem IDirectRouteProvider, což umožňuje úplnou kontrolu nad jak zjistit a nakonfigurované trasy atributů. IDirectRouteProvider zodpovídá za poskytování seznam akcí a řadičích a informace o postupu přidružené k určení přesně směrování konfigurace, které se požaduje pro tyto akce. Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes nebo MapHttpAttributeRoutes.

Přizpůsobení IDirectRouteProvider bude nejjednodušší tím, že rozšíří naše výchozí implementace DefaultDirectRouteProvider. Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytvořit položky trasy a zjišťování předponu trasy a předponu oblasti.

S novou atribut směrování rozšiřitelnosti služby IDirectRouteProvider uživatel může takto:

1. Podpora dědičnosti atribut trasy. Například v následujícím scénáři Blog a úložiště řadiče používají konvence trasy atribut, který je definován BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Automaticky generovat názvy tras pro trasy atributů. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Úprava předpon trasy v jednom centrálním místě, před trasy se přidají do tabulky směrování.
4. Filtrovat řadiče, na kterých chcete směrováním atributů, která se má hledat. Brzy Doufáme blogu na 3 a 4.

### <a name="facebook-fixes-for-changed-api-surface"></a>Facebook opravy pro prostor změněné rozhraní API

Balíček MVC Facebook [bylo přerušeno](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) z důvodu několik rozhraní API změny provedené ve službě Facebook. Také vydáváme nový balíček Facebook (Microsoft.AspNet.Facebook 1.0.0) Chcete-li vyřešit tyto problémy.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků balíčky má za následek 5.1.2 pro šablony, které již nejsou v projektu

Aktualizace balíčků NuGet pro rozhraní ASP.NET MVC 5.2.0 nelze aktualizovat, jako je ASP.NET generování uživatelského rozhraní nástroje sady Visual Studio nebo šablona projektu webové aplikace ASP.NET. Používají předchozí verzi modulu runtime ASP.NET balíčky (například 5.1.2 v aktualizaci Update 2). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici v projektech. ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů. Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků projekty webového rozhraní API 2.2 nebo ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a rozhraní ASP.NET MVC jsou konzistentní.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Instalace balíčku Microsoft.jQuery.Unobtrusive.Validation NuGet nezdaří, protože se nepodařilo najít verzi Microsoft.jQuery.Unobtrusive.Validation kompatibilní s jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation vyžaduje jQuery &gt;= 1.8 a jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) musí jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Z toho důvodu při NuGet nainstaluje JQuery 1.8 a jQuery.Validation 1.8 ve stejnou dobu, dojde k chybě. Když se tento problém, můžete jednoduše aktualizovat verzi jQuery.Validation k &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) jehož krytky jQuery pevné nejprve nyní byste měli mít k instalaci Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Verze balíčku nuget ověření 1.13.0 nemůže rozpoznat některé mezinárodní e-mailové adresy

verze balíčku nuget jQuery.Validation 1.11.1 je poslední známá verze, která rozpoznává následující platné e-mailové adresy. Všechny novější verze nemusí být možné rozpoznat. Příklad:

Standardní e-mailovou adresu internacionalizace (EAI) (například [&#29992; &#25143;@domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + identifikátory prostředků Internationalized (IRIs) (např., [&#29992; &#25143; @&#1076; &#1086; &#1084; &#1077; &#1085;. &#1088; &#1092; ](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Problém je hlášené v [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Zvýraznění syntaxe Razor zobrazení v sadě Visual Studio 2013

Pokud aktualizujete k ASP.NET MVC 5.2 bez aktualizace Visual Studio 2013, nezískáte editor Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor. Musíte se k aktualizaci Visual Studio 2013 získat tuto podporu.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a aktualizací menší funkce

Tato verze rovněž obsahuje několik oprav chyb a menší funkce aktualizace. Úplný seznam najdete:

- [5.2 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Tato verze nemá žádné nové funkce nebo opravy chyb v MVC. Jsme provedli [změnit ve webových stránkách](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) pro zlepšení výkonu a následně aktualizují všechny ostatní závislé balíčky jsme vlastní závislý na tuto novou verzi webových stránek.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Další informace o vydání [zde](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Tato verze obsahuje pouze opravy chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam chyby v této verzi.
