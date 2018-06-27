---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Co je nového v rozhraní ASP.NET Web API 2.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 89b065fccd0e4864f4a24c37b4caa29a1e127840
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961296"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Co je nového v rozhraní ASP.NET Web API 2.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového pro ASP.NET Web API 2.2.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v rozhraní ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Atribut směrování vylepšení](#ARI)
    - [Podpora webového rozhraní API klienta pro Windows Phone 8.1](#phone)
- [Známé problémy a nejnovější změny](#known-issues)
- [Opravy chyb](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet. Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace. Nejnovější balíček ASP.NET Web API 2.2 má následující verze: "5.2.0". Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.

Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET Web API 2.2 jsou dostupné na webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nové funkce v rozhraní ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Tato verze přidává podporu pro protokol OData v4. Další informace najdete v tématu [Web API OData v4 dokumentaci.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Tady jsou některé klíčové funkce a změny pro OData v4:

- [Podpora pro aliasy vlastnosti v modelu OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Podpora ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute a ConcurrencyCheckAttribute v ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Zadejte možnost zadat popisný název akce](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrovat ODL UriParser
- Podpora pro [výčtu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [omezení](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) a [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Podpora pro primitivní typy přetypování
- [Přidaná podpora funkce OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Podpora parametr aliasy pro volání funkcí](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Podpora formátu camelCase rozlišování názvů v modelu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Podpora pro cast() v $filter
- Podpora pro otevřete komplexní typ.
- Odebrané EntitySetController a AsyncEntitySetController
- [Změněné $link pro $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Přidaná podpora směrování atribut](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Používá OData základní knihovny 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrováním atributů poskytuje bod rozšiřitelnosti názvem IDirectRouteProvider, což umožňuje úplnou kontrolu nad jak zjistit a nakonfigurované trasy atributů. IDirectRouteProvider zodpovídá za poskytování seznam akcí a řadičích a informace o postupu přidružené k určení přesně směrování konfigurace, které se požaduje pro tyto akce. Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes nebo MapHttpAttributeRoutes.

Přizpůsobení IDirectRouteProvider bude nejjednodušší tím, že rozšíří naše výchozí implementace DefaultDirectRouteProvider. Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytvořit položky trasy a zjišťování předponu trasy a předponu oblasti.

Následuje několik příkladů na co můžete udělat pomocí tohoto nového bodu rozšíření:

1. Podpora dědičnosti trasy atributů

    Příklad:

    Tady by žádost, například "/ api/hodnoty/10" úspěšně vrátit "Úspěchu: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Zadejte výchozí název trasy pro atribut trasy podle některé konvence, které se vám líbí. Ve výchozím nastavení směrováním atributů nevytváří automaticky názvů pro atribut trasy.
3. Upravte šablonu trasy atributů trasy na jednom centrálním místě, před skončili ve směrovací tabulce.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Podpora webového rozhraní API klienta pro Windows Phone 8.1

Balíček webové rozhraní API klienta NuGet teď můžete použít k implementaci logika webového rozhraní API klienta, pokud je cílem Windows Phone 8.1 nebo z v rámci univerzální aplikace.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

Tato část popisuje známé problémy a nejnovějších změn v ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Tvůrce modelu

Problém: Přetížených funkcí nelze vystavit jako element FunctionImport

Pokud existují 2 přetížených funkcí a jsou i element FunctionImport, jak je uvedeno dále pak požaduje ~/GetAllConventionCustomers(CustomerName={customerName}) má za následek System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Alternativní řešení: Alternativní řešení tohoto problému je přidat jako FunctionImports přetížení funkce.

#### <a name="odata-routing"></a>Směrování protokolu OData

Textové literály, které obsahují adresu URL kódovaný lomítko (% 2F) a backslash(%5C) způsobit chybu 404, když jsou použity v prostředku cesty OData.

Textové literály můžete například použít v prostředku cesty OData jako parametry funkce nebo hodnoty klíče sad entit.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Když takovou žádost hostitele se zruší řídicí obdrží služby ty řídicí sekvence před jejich předáním runtime webového rozhraní API. Je to ochrana proti útokům takto:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

To způsobí, že Web API OData zásobníku vrátit chybu 404 (není nalezena). Aby se tato chyba, by měl klient používat posloupnosti zdvojených uvozovacích pro lomítko (% 252F) a zpětné lomítko (% 255C). Tato situace pro řetězce dotazů, jako je například /Employees? $filter = název eq 'Name % 2F:

**Poznámka: zrušení uvozovacími znaky lomítka (/) a nejsou právní v OData prostředků cesta textové literály zpětná lomítka ("). Lomítka by měla objevit pouze jako oddělovače cest a zpětná lomítka neměla v cesta prostředku OData vůbec. (Jak se používají v některé části řetězce dotazu OData.)**

Alternativní řešení: Metodě analýzy DefaultODataPathHandler, abyste se vyhnuli lomítko a zpětné lomítko, v textové literály před analýzou je ve skutečnosti může přepsat. Ukázka tohoto přístupu Zde můžete najít.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Dotazovatelné]

Atribut [Queryable] je zastaralá. Nové OData v3 aplikace by měly používat **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** metoda rozšíření teď přidá **EnableQueryAttribute** do kolekce globálních filtrů. Pokud mají všechny řadiče **[Queryable]** atribut, volání `config.EnableQuerySupport()` způsobí, že **[Queryable]** atribut selhání

Doporučený způsob k vyřešení tohoto problému je nahraďte všechny výskyty **položce QueryableAttribute** s **System.Web.Http.OData.EnableQueryAttribute**.

Alternativní řešení je v konfiguraci webového rozhraní API použít následující kód:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Atribut směrování

Problém: Vazby modelu komplexního typu, který opatřen atributem FromUri pracuje odlišně při použití směrováním atributů.

Následující odkaz ke sledování problém a obsahuje také podrobnosti o alternativní řešení.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problém: Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků balíčky má za následek 5.1.2 pro šablony, které již nejsou v projektu

Aktualizace balíčků NuGet pro ASP.NET MVC 5.2 nelze aktualizovat, jako je ASP.NET generování uživatelského rozhraní nástroje sady Visual Studio nebo šablona projektu webové aplikace ASP.NET. Používají předchozí verzi modulu runtime ASP.NET balíčky (například 5.1.2 v aktualizaci Update 2). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici v projektech. ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů. Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků projekty webového rozhraní API 2.2 nebo ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a rozhraní ASP.NET MVC jsou konzistentní.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a aktualizací menší funkce

Tato verze rovněž obsahuje několik oprav chyb a menší funkce aktualizace. Úplný seznam najdete:

- [5.2 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Balíček Microsoft.AspNet.OData 5.2.1 obsahuje aktualizace závislostí NuGet, ale žádné opravy chyb. S touto aktualizací již není striktní závislost na Microsoft.OData.Core 6.4.0, ale jeden můžete upgradovat na kteroukoli verzi mezi 6.4.0 a 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

V této verzi jsme provedli závislost pro změnu `Json.Net 6.0.4`. Další informace o co je nového v této verzi `Json.NET`, najdete v části [vkládání závislostí Json.NET 6.0 verze 4 - JSON sloučení,](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Tato verze nemá žádné nové funkce nebo opravy chyb v rozhraní Web API. Následně jsme jste aktualizovali všechny ostatní závislé balíčky, které jsme vlastní závislý na tuto novou verzi webového rozhraní API.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Další informace o vydání [zde](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Tato verze obsahuje pouze opravy chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam chyby v této verzi.
