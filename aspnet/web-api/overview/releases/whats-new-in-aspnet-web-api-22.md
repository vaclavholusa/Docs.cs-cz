---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Co je nového v rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/25/2014
ms.topic: article
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 51b01c4b9ee8d12e4e3925193e308d3ca6c5f2b6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377438"
---
<a name="whats-new-in-aspnet-web-api-22"></a>Co je nového v rozhraní ASP.NET Web API 2.2
====================
podle [Microsoft](https://github.com/microsoft)

Toto téma popisuje, co je nového v ASP.NET Web API 2.2.

- [Stáhnout](#download)
- [Dokumentace](#documentation)
- [Nové funkce v rozhraní ASP.NET Web API 2.2](#newf)

    - [OData v4](#OData)
    - [Atribut směrování vylepšení](#ARI)
    - [Podpora webového rozhraní API klienta pro Windows Phone 8.1](#phone)
- [Známé problémy a změny způsobující chyby](#known-issues)
- [Opravy chyb](#bug-fixes)
- [Microsoft.AspNet.OData 5.2.1](#odata521)
- [Microsoft.AspNet.WebAPI 5.2.2](#522RC)
- [Microsoft.AspNet.WebAPI 5.2.3 Beta](#523)

<a id="download"></a>
## <a name="download"></a>Stáhnout

Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet. Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace. Nejnovější balíček ASP.NET Web API 2.2 má následující verzi: "5.2.0". Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/). Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.

Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a>Dokumentace

Kurzy a další informace o ASP.NET Web API 2.2 jsou k dispozici z webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a>Nové funkce v rozhraní ASP.NET Web API 2.2

<a id="OData"></a>
### <a name="odata-v4"></a>OData v4

Tato verze přidává podporu protokolu OData v4. Další informace najdete v tématu [dokumentace k Web API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)

Tady jsou některé klíčové funkce a změny provedené u OData v4:

- [Podpora pro aliasy vlastnosti v modelu OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [Podpora ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute a ConcurrencyCheckAttribute v ODataConventionModelBuilder](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [Zadejte možnost zadat popisný název akce](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- Integrace s ODL UriParser
- Podpora pro [výčtu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [členství ve skupině](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) a [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)
- Podpora pro primitivní typy přetypování
- [Přidání podpory funkce OData](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Podpora aliasů parametr pro volání funkce](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [Podpora stylem camel case zásady vytváření názvů v modelu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- Podpora pro cast() v $filter
- Podpora pro open komplexní typ.
- Odebrané EntitySetController a AsyncEntitySetController
- [Změněné $link pro $ref](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [Přidání podpory směrování atributů](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- Používá OData základní knihovny 6.4.0

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a>Atribut směrování vylepšení

Směrování atributů poskytuje bod rozšiřitelnosti volá IDirectRouteProvider, což umožňuje úplnou kontrolu nad způsob zjišťování a nakonfigurovat trasy atributů. IDirectRouteProvider zodpovídá za poskytování seznam akce a kontrolery spolu s informací o směrování přidružené k určení přesně je žádoucí jaké konfigurace směrování pro tyto akce. Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes/MapHttpAttributeRoutes.

Přizpůsobení IDirectRouteProvider bude nejjednodušší rozšířením naší výchozí implementace DefaultDirectRouteProvider. Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytváření položky trasy a zjišťování předponu trasy a předponu oblasti.

Toto jsou některé příklady, co můžete udělat pomocí tohoto nového bodu rozšiřitelnosti:

1. Podpora dědičnosti atributů trasy

    Příklad:

    Tady požadavek like "/ api/hodnoty/10" úspěšně vrátí "Úspěch: 10"

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. Zadejte název výchozí trasy pro atribut trasy podle některých vytváření názvů, který rádi používáte. Ve výchozím nastavení směrování atributů nevytváří automaticky názvů pro atribut trasy.
3. Před ukládaly do směrovací tabulky, upravte šablonu trasy atributů trasy v jednom centrálním místě.

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a>Podpora webového rozhraní API klienta pro Windows Phone 8.1

Nyní můžete použít balíček NuGet klientské webové rozhraní API implementovat logiku klienta vaše webového rozhraní API, při cílení na Windows Phone 8.1 nebo z v rámci univerzální aplikace.

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

Tato část popisuje známé problémy a novinkách v ASP.NET Web API 2.2.

### <a name="odata-v4"></a>OData v4

#### <a name="model-builder"></a>Tvůrce modelu

Problém: Přetížených funkcí nelze vystavit jako element FunctionImport

Pokud existují 2 přetížených funkcí a jsou také element FunctionImport, jak je znázorněno níže pak si vyžádat výsledky ~/GetAllConventionCustomers(CustomerName={customerName}) v System.InvalidOperationException.

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

Alternativní řešení: Řešení tohoto problému je přidat jako FunctionImports přetížení funkce.

#### <a name="odata-routing"></a>Směrování protokolu OData

Řetězcové literály, které obsahují adresu URL kódovaný lomítko (% 2F) a backslash(%5C) způsobit chybu 404 při použití v prostředku cesty OData.

Například řetězcové literály lze použít v prostředku cesty OData jako parametry funkce nebo hodnoty klíče ze sady entit.

/Employees/Total.GetCount(Name='Name%2F')

/Employees('Name%5C')

Pokud služby zobrazí požadavky, hostitelé se zruší řídicí ty řídicí sekvence před jejich odesláním do webového rozhraní API modulu runtime. Tím se zajistí ochrana před útoky, jako je následující:  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

To způsobí, že Web API OData zásobníku vrátit chybu 404 (Nenalezeno). K této chybě zabránit, klient by měl používat dvojitou řídicí sekvence pro lomítko (% 252F) a zpětné lomítko (% 255C). To nestalo pro řetězce dotazu, jako je například /Employees? $filter = název eq 'název % 2F:

**Všimněte si uvozeny řídicími znaky lomítka (/) a zpětná lomítka (") nejsou povoleny u řetězcových literálů. cestu OData prostředků. Lomítka by se měla zobrazit pouze jako oddělovače cest a zpětná lomítka by se neměl zobrazit v cestě k prostředku OData vůbec. (Obojí jsou použitelné v některé části řetězce dotazu OData.)**

Alternativní řešení: Může přepsat metodu Parse DefaultODataPathHandler řídicí lomítko a zpětné lomítko u řetězcových literálů. před dokončením analýzy je ve skutečnosti. Můžete najít ukázku tohoto postupu najdete tady.

### <a name="odata-v3"></a>OData v3

#### <a name="queryable"></a>[Dotazovatelný]

Atribut [Queryable] je zastaralá. Nové prostředí OData v3 aplikace by měly používat **System.Web.Http.OData.EnableQueryAttribute**.

**ODataHttpConfigurationExtensions.EnableQuerySupport** – metoda rozšíření se teď přidá **EnableQueryAttribute** do kolekce globálních filtrů. Pokud mají všechny řadiče **[Queryable]** atribut, volání `config.EnableQuerySupport()` způsobí, že **[Queryable]** atribut selhání

Doporučený postup k vyřešení tohoto problému je k nahrazení všech výskytů řetězce **položce QueryableAttribute** s **System.Web.Http.OData.EnableQueryAttribute**.

Alternativní řešení se v konfiguraci webového rozhraní API použít následující kód:

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a>Směrování atributů

Problém: Vazby modelu komplexní typ, který je upravena pomocí atributů FromUri chová odlišně při použití směrování atributů.

Následující odkaz je pro sledování problému a také obsahuje podrobnosti o alternativní řešení.  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

Problém: Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků následek 5.1.2 balíčky, které už neexistují v projektu

Aktualizují se balíčky NuGet pro ASP.NET MVC 5.2 nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET. Používají předchozí verze balíčky runtime ASP.NET (např. 5.1.2 v aktualizaci Update 2). Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verze (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech. ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech. Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků projektů a Web API 2.2 technologie ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a architektura ASP.NET MVC jsou konzistentní vzhledem k aplikacím.

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a>Opravy chyb a aktualizace podverze funkcí

Tato verze zahrnuje také několik oprav chyb a dílčí funkce aktualizace. Úplný seznam najdete:

- [5.2 balíčku](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a>Microsoft.AspNet.OData 5.2.1

Microsoft.AspNet.OData 5.2.1 balíček obsahuje aktualizace závislostí NuGet, ale žádné opravy chyb. Od této aktualizace už není striktní závislost na Microsoft.OData.Core 6.4.0, ale jeden můžete upgradovat na kteroukoli verzi mezi 6.4.0 a 7.0.0.

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a>Microsoft.AspNet.WebAPI 5.2.2

V této verzi jsme změnili závislost u `Json.Net 6.0.4`. Další informace o novinkách v této verzi `Json.NET`, naleznete v tématu [injektáž závislostí Json.NET 6.0 Release 4 – sloučení JSON a](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection). Tato verze neobsahuje žádné nové funkce nebo opravy chyb v rozhraní Web API. Následně jsme aktualizovali všechny ostatní závislé balíčky, které vlastníme, aby závisely na této nové verzi webového rozhraní API.

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a>Microsoft.AspNet.WebAPI 5.2.3 Beta

Může číst informace o verzi [tady](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx). Tato verze obsahuje pouze oprav chyb. Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam problémů, které jsou opravené v této verzi.
