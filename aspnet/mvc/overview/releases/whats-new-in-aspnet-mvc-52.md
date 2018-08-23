---
uid: mvc/overview/releases/whats-new-in-aspnet-mvc-52
title: Co je nového v architektuře ASP.NET MVC 5.2 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 97972587-2720-48b4-b158-f35f2e855fbf
msc.legacyurl: /mvc/overview/releases/whats-new-in-aspnet-mvc-52
msc.type: authoredcontent
ms.openlocfilehash: 8c3c5de55396635d2e7f2b7726f54be1c06bb691
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752634"
---
<a name="whats-new-in-aspnet-mvc-52"></a>Co je nového v architektuře ASP.NET MVC 5.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET MVC 5.2, [Microsoft.AspNet.MVC 5.2.2](#52) a [ASP.NET MVC 5.2.3 Beta](#mvc523Beta)

- [Požadavky na software](#softRequire)
- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v architektuře ASP.NET MVC 5.2](#new-features)

    - [Atribut směrování vylepšení](#attributerouting)
- [Známé problémy a změny způsobující chyby](#knownbreakingchanges)
- [Opravy chyb](#bug-fixes)
- [Microsoft.AspNet.MVC 5.2.2](#52)

<a id="softRequire"></a>
## <a name="software-requirements"></a>Požadavky na software

- Visual Studio 2012: Stáhněte si [technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).
- Visual Studio 2013: Stáhněte si [Visual Studio 2013 Update](https://go.microsoft.com/fwlink/?LinkId=390064) nebo vyšší. Tato aktualizace je potřeba pro úpravu zobrazení ASP.NET MVC 5.2 syntaxe Razor.

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace. Nejnovější balíček ASP.NET MVC 5.2 má následující verzi: "5.2.0". Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/). Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.

Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

Microsoft.AspNet.Mvc Install-Package-verze 5.2.0

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET MVC 5.2 jsou k dispozici z webu technologie ASP.NET ([https://www.asp.net/mvc](../../index.md)).

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-52"></a>Nové funkce v architektuře ASP.NET MVC 5.2

<a id="attributerouting"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrování atributů poskytuje bod rozšiřitelnosti volá IDirectRouteProvider, což umožňuje úplnou kontrolu nad způsob zjišťování a nakonfigurovat trasy atributů. IDirectRouteProvider zodpovídá za poskytování seznam akce a kontrolery spolu s informací o směrování přidružené k určení přesně je žádoucí jaké konfigurace směrování pro tyto akce. Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes/MapHttpAttributeRoutes.

Přizpůsobení IDirectRouteProvider bude nejjednodušší rozšířením naší výchozí implementace DefaultDirectRouteProvider. Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytváření položky trasy a zjišťování předponu trasy a předponu oblasti.

Nový atribut směrování rozšiřitelnost IDirectRouteProvider uživatel udělat následující:

1. Podpora dědičnosti atribut trasy. Například v následujícím scénáři blogu a Store řadiče používají konvence směrování atributů, který je definován BaseController. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample1.cs)]
2. Automaticky generovat názvy tras pro trasy atributů. 

    [!code-csharp[Main](whats-new-in-aspnet-mvc-52/samples/sample2.cs)]
3. Upravte předpony trasy na jednom centrálním místě, než nechejte se přidat trasy do směrovací tabulky.
4. Vyfiltrování řadiče, na kterých chcete směrování atributů, která se má hledat. Věříme, že na blogu o 3 a 4 brzy.

### <a name="facebook-fixes-for-changed-api-surface"></a>Opravy změněné rovinu rozhraní API Facebooku

Balíček služby Facebook MVC [bylo přerušeno](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=Facebook&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=All) kvůli několik rozhraní API změny provedené ve službě Facebook. Vydáváme také nový balíček pro Facebook (Microsoft.AspNet.Facebook 1.0.0) Chcete-li vyřešit tyto problémy.

<a id="knownbreakingchanges"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

### <a name="scaffolding-mvcweb-api-into-a-project-with-520-packages-results-in-512-packages-for-ones-that-dont-already-exist-in-the-project"></a>Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků následek 5.1.2 balíčky, které už neexistují v projektu

Aktualizují se balíčky NuGet pro rozhraní Asp.net.mvc 5.2.0 nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET. Používají předchozí verze balíčky runtime ASP.NET (např. 5.1.2 v aktualizaci Update 2). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verze (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech. ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech. Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků projektů a Web API 2.2 technologie ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a architektura ASP.NET MVC jsou konzistentní vzhledem k aplikacím.

### <a name="microsoftjqueryunobtrusivevalidation-nuget-package-installation-fails-because-it-is-unable-to-find-a-version-of-microsoftjqueryunobtrusivevalidation-compatible-to-jquery-141"></a>Instalace balíčku Microsoft.jQuery.Unobtrusive.Validation NuGet selže, protože se nepovedlo se najít verzi Microsoft.jQuery.Unobtrusive.Validation kompatibilní jQuery 1.4.1

Microsoft.jQuery.Unobtrusive.Validation vyžaduje jQuery &gt;= 1.8 a jQuery.Validation &gt;= 1.8. But,jQuery.Validation (1.8) vyžaduje jQuery (&#8805; 1.3.2 &amp; &amp; &#8804; 1.6). Proto pokud NuGet nainstaluje JQuery 1.8 a jQuery.Validation 1.8 ve stejnou dobu, dojde k chybě. Až se tento problém, můžete jednoduše aktualizovat verzi jQuery.Validation k &gt; =  [1.8.0.1](https://www.nuget.org/packages/jQuery.Validation/1.8.0.1) obsahující jQuery limit pevné nejprve byste měli nainstalovat Microsoft.jQuery.Unobtrusive.Validation.

### <a name="the-jqueryvalidation-nuget-package-version-1130-does-not-recognize-some-international-email-addresses"></a>Jquery. Verze balíčku nuget ověření 1.13.0 nerozpozná některé mezinárodní e-mailové adresy

verze balíčku nuget jQuery.Validation 1.11.1 je poslední známou verzí, který rozpoznává následující platné e-mailové adresy. Všechny novější verze nemusí být schopen rozpoznat. Příklad:

E-mailovou adresu internacionalizace (EAI) standard (například [ &#29992; &#25143; @domain.com ](mailto:&#29992;&#25143;@domain.com))   
 EAI + identifikátory prostředků Internationalized (IRIS –) (např., [ &#29992; &#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088; &#1092;](mailto:&#29992;&#25143;@&#1076;&#1086;&#1084;&#1077;&#1085;.&#1088;&#1092;))

Problém se použije v hlášení v [https://github.com/jzaefferer/jquery-validation/issues/1222](https://github.com/jzaefferer/jquery-validation/issues/1222)

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a>Zvýraznění syntaxe pro zobrazení syntaxe Razor v sadě Visual Studio 2013

Pokud aktualizujete na ASP.NET MVC 5.2 bez aktualizace sady Visual Studio 2013, nezískáte editoru Visual Studio – podpora pro zvýraznění syntaxe při úpravách zobrazení syntaxe Razor. Je potřeba aktualizovat Visual Studio 2013 k získání této podpory.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a menší aktualizace funkcí

Tato verze zahrnuje také několik oprav chyb a dílčí funkce aktualizace. Úplný seznam najdete:

- [5.2 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2%20RC&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<a id="52"></a>
## <a name="microsoftaspnetmvc-522"></a>Microsoft.AspNet.MVC 5.2.2

Tato verze neobsahuje žádné nové funkce nebo opravy chyb v MVC. Provedli jsme [změnit na webových stránkách](https://blogs.msdn.com/b/webdev/archive/2014/07/28/announcing-the-beta-release-of-web-pages-3-2-1.aspx) pro zlepšení výkonu a pak jsme aktualizovali všechny ostatní závislé balíčky vlastníme, aby závisely na této nové verzi webových stránek.

<a id="mvc523Beta"></a>
## <a name="aspnet-mvc-523-beta"></a>ASP.NET MVC 5.2.3 Beta

Může číst informace o verzi [tady](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Tato verze obsahuje pouze oprav chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=MVC&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam problémů, které jsou opravené v této verzi.
