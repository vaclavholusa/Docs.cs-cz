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
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="f7dae-102">Co je nového v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f7dae-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="f7dae-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f7dae-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="f7dae-104">Toto téma popisuje, co je nového pro ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f7dae-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="f7dae-105">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="f7dae-105">Download</span></span>](#download)
- [<span data-ttu-id="f7dae-106">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="f7dae-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="f7dae-107">Nové funkce v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f7dae-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="f7dae-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="f7dae-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="f7dae-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="f7dae-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="f7dae-110">Podpora webového rozhraní API klienta pro Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f7dae-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="f7dae-111">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="f7dae-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="f7dae-112">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="f7dae-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="f7dae-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="f7dae-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="f7dae-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="f7dae-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="f7dae-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="f7dae-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="f7dae-116">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="f7dae-116">Download</span></span>

<span data-ttu-id="f7dae-117">Funkce modulu runtime vydávají jako balíčků NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7dae-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="f7dae-118">Všechny balíčky modulu runtime použijte [sémantické verze](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="f7dae-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="f7dae-119">Nejnovější balíček ASP.NET Web API 2.2 má následující verze: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="f7dae-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="f7dae-120">Můžete instalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="f7dae-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="f7dae-121">Verze také zahrnuje odpovídající lokalizované balíčky na NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7dae-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="f7dae-122">Můžete instalovat nebo aktualizovat balíčky NuGet vydaná pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="f7dae-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="f7dae-123">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="f7dae-123">Documentation</span></span>

<span data-ttu-id="f7dae-124">Kurzy a další informace o ASP.NET Web API 2.2 jsou dostupné na webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="f7dae-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="f7dae-125">Nové funkce v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="f7dae-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="f7dae-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="f7dae-126">OData v4</span></span>

<span data-ttu-id="f7dae-127">Tato verze přidává podporu pro protokol OData v4.</span><span class="sxs-lookup"><span data-stu-id="f7dae-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="f7dae-128">Další informace najdete v tématu [Web API OData v4 dokumentaci.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="f7dae-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="f7dae-129">Tady jsou některé klíčové funkce a změny pro OData v4:</span><span class="sxs-lookup"><span data-stu-id="f7dae-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="f7dae-130">Podpora pro aliasy vlastnosti v modelu OData</span><span class="sxs-lookup"><span data-stu-id="f7dae-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="f7dae-131">Podpora ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute a ConcurrencyCheckAttribute v ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="f7dae-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="f7dae-132">Zadejte možnost zadat popisný název akce</span><span class="sxs-lookup"><span data-stu-id="f7dae-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="f7dae-133">Integrovat ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="f7dae-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="f7dae-134">Podpora pro [výčtu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [omezení](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) a [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="f7dae-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="f7dae-135">Podpora pro primitivní typy přetypování</span><span class="sxs-lookup"><span data-stu-id="f7dae-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="f7dae-136">Přidaná podpora funkce OData</span><span class="sxs-lookup"><span data-stu-id="f7dae-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="f7dae-137">Podpora parametr aliasy pro volání funkcí</span><span class="sxs-lookup"><span data-stu-id="f7dae-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="f7dae-138">Podpora formátu camelCase rozlišování názvů v modelu</span><span class="sxs-lookup"><span data-stu-id="f7dae-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="f7dae-139">Podpora pro cast() v $filter</span><span class="sxs-lookup"><span data-stu-id="f7dae-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="f7dae-140">Podpora pro otevřete komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="f7dae-140">Support for open complex type</span></span>
- <span data-ttu-id="f7dae-141">Odebrané EntitySetController a AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="f7dae-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="f7dae-142">Změněné $link pro $ref</span><span class="sxs-lookup"><span data-stu-id="f7dae-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="f7dae-143">Přidaná podpora směrování atribut</span><span class="sxs-lookup"><span data-stu-id="f7dae-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="f7dae-144">Používá OData základní knihovny 6.4.0</span><span class="sxs-lookup"><span data-stu-id="f7dae-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="f7dae-145">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="f7dae-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="f7dae-146">Směrováním atributů poskytuje bod rozšiřitelnosti názvem IDirectRouteProvider, což umožňuje úplnou kontrolu nad jak zjistit a nakonfigurované trasy atributů.</span><span class="sxs-lookup"><span data-stu-id="f7dae-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="f7dae-147">IDirectRouteProvider zodpovídá za poskytování seznam akcí a řadičích a informace o postupu přidružené k určení přesně směrování konfigurace, které se požaduje pro tyto akce.</span><span class="sxs-lookup"><span data-stu-id="f7dae-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="f7dae-148">Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes nebo MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="f7dae-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="f7dae-149">Přizpůsobení IDirectRouteProvider bude nejjednodušší tím, že rozšíří naše výchozí implementace DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="f7dae-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="f7dae-150">Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytvořit položky trasy a zjišťování předponu trasy a předponu oblasti.</span><span class="sxs-lookup"><span data-stu-id="f7dae-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="f7dae-151">Následuje několik příkladů na co můžete udělat pomocí tohoto nového bodu rozšíření:</span><span class="sxs-lookup"><span data-stu-id="f7dae-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="f7dae-152">Podpora dědičnosti trasy atributů</span><span class="sxs-lookup"><span data-stu-id="f7dae-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="f7dae-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f7dae-153">Example:</span></span>

    <span data-ttu-id="f7dae-154">Tady by žádost, například "/ api/hodnoty/10" úspěšně vrátit "Úspěchu: 10"</span><span class="sxs-lookup"><span data-stu-id="f7dae-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="f7dae-155">Zadejte výchozí název trasy pro atribut trasy podle některé konvence, které se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="f7dae-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="f7dae-156">Ve výchozím nastavení směrováním atributů nevytváří automaticky názvů pro atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="f7dae-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="f7dae-157">Upravte šablonu trasy atributů trasy na jednom centrálním místě, před skončili ve směrovací tabulce.</span><span class="sxs-lookup"><span data-stu-id="f7dae-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="f7dae-158">Podpora webového rozhraní API klienta pro Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="f7dae-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="f7dae-159">Balíček webové rozhraní API klienta NuGet teď můžete použít k implementaci logika webového rozhraní API klienta, pokud je cílem Windows Phone 8.1 nebo z v rámci univerzální aplikace.</span><span class="sxs-lookup"><span data-stu-id="f7dae-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="f7dae-160">Známé problémy a nejnovější změny</span><span class="sxs-lookup"><span data-stu-id="f7dae-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="f7dae-161">Tato část popisuje známé problémy a nejnovějších změn v ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="f7dae-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="f7dae-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="f7dae-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="f7dae-163">Tvůrce modelu</span><span class="sxs-lookup"><span data-stu-id="f7dae-163">Model builder</span></span>

<span data-ttu-id="f7dae-164">Problém: Přetížených funkcí nelze vystavit jako element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="f7dae-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="f7dae-165">Pokud existují 2 přetížených funkcí a jsou i element FunctionImport, jak je uvedeno dále pak požaduje ~/GetAllConventionCustomers(CustomerName={customerName}) má za následek System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="f7dae-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="f7dae-166">Alternativní řešení: Alternativní řešení tohoto problému je přidat jako FunctionImports přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="f7dae-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="f7dae-167">Směrování protokolu OData</span><span class="sxs-lookup"><span data-stu-id="f7dae-167">OData Routing</span></span>

<span data-ttu-id="f7dae-168">Textové literály, které obsahují adresu URL kódovaný lomítko (% 2F) a backslash(%5C) způsobit chybu 404, když jsou použity v prostředku cesty OData.</span><span class="sxs-lookup"><span data-stu-id="f7dae-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="f7dae-169">Textové literály můžete například použít v prostředku cesty OData jako parametry funkce nebo hodnoty klíče sad entit.</span><span class="sxs-lookup"><span data-stu-id="f7dae-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="f7dae-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="f7dae-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="f7dae-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="f7dae-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="f7dae-172">Když takovou žádost hostitele se zruší řídicí obdrží služby ty řídicí sekvence před jejich předáním runtime webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f7dae-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="f7dae-173">Je to ochrana proti útokům takto:</span><span class="sxs-lookup"><span data-stu-id="f7dae-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="f7dae-174">To způsobí, že Web API OData zásobníku vrátit chybu 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="f7dae-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="f7dae-175">Aby se tato chyba, by měl klient používat posloupnosti zdvojených uvozovacích pro lomítko (% 252F) a zpětné lomítko (% 255C).</span><span class="sxs-lookup"><span data-stu-id="f7dae-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="f7dae-176">Tato situace pro řetězce dotazů, jako je například /Employees? $filter = název eq 'Name % 2F:</span><span class="sxs-lookup"><span data-stu-id="f7dae-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="f7dae-177">**Poznámka: zrušení uvozovacími znaky lomítka (/) a nejsou právní v OData prostředků cesta textové literály zpětná lomítka ("). Lomítka by měla objevit pouze jako oddělovače cest a zpětná lomítka neměla v cesta prostředku OData vůbec. (Jak se používají v některé části řetězce dotazu OData.)**</span><span class="sxs-lookup"><span data-stu-id="f7dae-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="f7dae-178">Alternativní řešení: Metodě analýzy DefaultODataPathHandler, abyste se vyhnuli lomítko a zpětné lomítko, v textové literály před analýzou je ve skutečnosti může přepsat.</span><span class="sxs-lookup"><span data-stu-id="f7dae-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="f7dae-179">Ukázka tohoto přístupu Zde můžete najít.</span><span class="sxs-lookup"><span data-stu-id="f7dae-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="f7dae-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="f7dae-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="f7dae-181">[Dotazovatelné]</span><span class="sxs-lookup"><span data-stu-id="f7dae-181">[Queryable]</span></span>

<span data-ttu-id="f7dae-182">Atribut [Queryable] je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="f7dae-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="f7dae-183">Nové OData v3 aplikace by měly používat **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="f7dae-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="f7dae-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** metoda rozšíření teď přidá **EnableQueryAttribute** do kolekce globálních filtrů.</span><span class="sxs-lookup"><span data-stu-id="f7dae-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="f7dae-185">Pokud mají všechny řadiče **[Queryable]** atribut, volání `config.EnableQuerySupport()` způsobí, že **[Queryable]** atribut selhání</span><span class="sxs-lookup"><span data-stu-id="f7dae-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="f7dae-186">Doporučený způsob k vyřešení tohoto problému je nahraďte všechny výskyty **položce QueryableAttribute** s **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="f7dae-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="f7dae-187">Alternativní řešení je v konfiguraci webového rozhraní API použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="f7dae-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="f7dae-188">Atribut směrování</span><span class="sxs-lookup"><span data-stu-id="f7dae-188">Attribute Routing</span></span>

<span data-ttu-id="f7dae-189">Problém: Vazby modelu komplexního typu, který opatřen atributem FromUri pracuje odlišně při použití směrováním atributů.</span><span class="sxs-lookup"><span data-stu-id="f7dae-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="f7dae-190">Následující odkaz ke sledování problém a obsahuje také podrobnosti o alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="f7dae-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="f7dae-191">Problém: Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků balíčky má za následek 5.1.2 pro šablony, které již nejsou v projektu</span><span class="sxs-lookup"><span data-stu-id="f7dae-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="f7dae-192">Aktualizace balíčků NuGet pro ASP.NET MVC 5.2 nelze aktualizovat, jako je ASP.NET generování uživatelského rozhraní nástroje sady Visual Studio nebo šablona projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7dae-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="f7dae-193">Používají předchozí verzi modulu runtime ASP.NET balíčky (například 5.1.2 v aktualizaci Update 2).</span><span class="sxs-lookup"><span data-stu-id="f7dae-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="f7dae-194">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verzi (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici v projektech.</span><span class="sxs-lookup"><span data-stu-id="f7dae-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="f7dae-195">ASP.NET generování uživatelského rozhraní v Visual Studio 2013 RTM nebo Update 1, ale nepřepíše nejnovější balíčků ve vašich projektů.</span><span class="sxs-lookup"><span data-stu-id="f7dae-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="f7dae-196">Pokud používáte generování uživatelského rozhraní ASP.NET po aktualizaci balíčků projekty webového rozhraní API 2.2 nebo ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a rozhraní ASP.NET MVC jsou konzistentní.</span><span class="sxs-lookup"><span data-stu-id="f7dae-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="f7dae-197">Opravy chyb a aktualizací menší funkce</span><span class="sxs-lookup"><span data-stu-id="f7dae-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="f7dae-198">Tato verze rovněž obsahuje několik oprav chyb a menší funkce aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f7dae-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="f7dae-199">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="f7dae-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="f7dae-200">5.2 balíčku</span><span class="sxs-lookup"><span data-stu-id="f7dae-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="f7dae-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="f7dae-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="f7dae-202">Balíček Microsoft.AspNet.OData 5.2.1 obsahuje aktualizace závislostí NuGet, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="f7dae-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="f7dae-203">S touto aktualizací již není striktní závislost na Microsoft.OData.Core 6.4.0, ale jeden můžete upgradovat na kteroukoli verzi mezi 6.4.0 a 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="f7dae-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="f7dae-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="f7dae-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="f7dae-205">V této verzi jsme provedli závislost pro změnu `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="f7dae-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="f7dae-206">Další informace o co je nového v této verzi `Json.NET`, najdete v části [vkládání závislostí Json.NET 6.0 verze 4 - JSON sloučení,](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="f7dae-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="f7dae-207">Tato verze nemá žádné nové funkce nebo opravy chyb v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="f7dae-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="f7dae-208">Následně jsme jste aktualizovali všechny ostatní závislé balíčky, které jsme vlastní závislý na tuto novou verzi webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="f7dae-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="f7dae-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="f7dae-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="f7dae-210">Další informace o vydání [zde](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7dae-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="f7dae-211">Tato verze obsahuje pouze opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="f7dae-211">This release contains only bug fixes.</span></span> <span data-ttu-id="f7dae-212">Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam chyby v této verzi.</span><span class="sxs-lookup"><span data-stu-id="f7dae-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
