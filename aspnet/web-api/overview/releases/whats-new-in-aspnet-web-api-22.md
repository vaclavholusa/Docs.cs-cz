---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: Co je nového v rozhraní ASP.NET Web API 2.2 | Dokumentace Microsoftu
author: microsoft
description: ''
ms.author: riande
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: ef08a3bb397ff54795ca6c70cdcc35206cf122f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753069"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="c72a3-102">Co je nového v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="c72a3-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="c72a3-103">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c72a3-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="c72a3-104">Toto téma popisuje, co je nového v ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="c72a3-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="c72a3-105">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="c72a3-105">Download</span></span>](#download)
- [<span data-ttu-id="c72a3-106">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="c72a3-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="c72a3-107">Nové funkce v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="c72a3-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="c72a3-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="c72a3-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="c72a3-109">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="c72a3-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="c72a3-110">Podpora webového rozhraní API klienta pro Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="c72a3-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="c72a3-111">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="c72a3-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="c72a3-112">Opravy chyb</span><span class="sxs-lookup"><span data-stu-id="c72a3-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="c72a3-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="c72a3-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="c72a3-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="c72a3-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="c72a3-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="c72a3-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="c72a3-116">Stáhnout</span><span class="sxs-lookup"><span data-stu-id="c72a3-116">Download</span></span>

<span data-ttu-id="c72a3-117">Funkce modulu runtime jsou vydané jako balíčky NuGet v galerii NuGet.</span><span class="sxs-lookup"><span data-stu-id="c72a3-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="c72a3-118">Postupujte podle všech balíčků modulu runtime [Semantic Versioning](http://semver.org/) specifikace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="c72a3-119">Nejnovější balíček ASP.NET Web API 2.2 má následující verzi: "5.2.0".</span><span class="sxs-lookup"><span data-stu-id="c72a3-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="c72a3-120">Můžete nainstalovat nebo aktualizovat tyto balíčky prostřednictvím [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span><span class="sxs-lookup"><span data-stu-id="c72a3-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="c72a3-121">Tato verze rovněž obsahuje odpovídající lokalizované balíčky NuGet.</span><span class="sxs-lookup"><span data-stu-id="c72a3-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="c72a3-122">Můžete nainstalovat nebo aktualizovat vydané balíčky NuGet pomocí konzoly Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="c72a3-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="c72a3-123">Dokumentace</span><span class="sxs-lookup"><span data-stu-id="c72a3-123">Documentation</span></span>

<span data-ttu-id="c72a3-124">Kurzy a další informace o ASP.NET Web API 2.2 jsou k dispozici z webu technologie ASP.NET ([https://www.asp.net/web-api](../../index.md)).</span><span class="sxs-lookup"><span data-stu-id="c72a3-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="c72a3-125">Nové funkce v rozhraní ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="c72a3-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="c72a3-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="c72a3-126">OData v4</span></span>

<span data-ttu-id="c72a3-127">Tato verze přidává podporu protokolu OData v4.</span><span class="sxs-lookup"><span data-stu-id="c72a3-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="c72a3-128">Další informace najdete v tématu [dokumentace k Web API OData v4.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="c72a3-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="c72a3-129">Tady jsou některé klíčové funkce a změny provedené u OData v4:</span><span class="sxs-lookup"><span data-stu-id="c72a3-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="c72a3-130">Podpora pro aliasy vlastnosti v modelu OData</span><span class="sxs-lookup"><span data-stu-id="c72a3-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="c72a3-131">Podpora ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute a ConcurrencyCheckAttribute v ODataConventionModelBuilder</span><span class="sxs-lookup"><span data-stu-id="c72a3-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="c72a3-132">Zadejte možnost zadat popisný název akce</span><span class="sxs-lookup"><span data-stu-id="c72a3-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="c72a3-133">Integrace s ODL UriParser</span><span class="sxs-lookup"><span data-stu-id="c72a3-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="c72a3-134">Podpora pro [výčtu](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [členství ve skupině](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) a [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="c72a3-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="c72a3-135">Podpora pro primitivní typy přetypování</span><span class="sxs-lookup"><span data-stu-id="c72a3-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="c72a3-136">Přidání podpory funkce OData</span><span class="sxs-lookup"><span data-stu-id="c72a3-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="c72a3-137">Podpora aliasů parametr pro volání funkce</span><span class="sxs-lookup"><span data-stu-id="c72a3-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="c72a3-138">Podpora stylem camel case zásady vytváření názvů v modelu</span><span class="sxs-lookup"><span data-stu-id="c72a3-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="c72a3-139">Podpora pro cast() v $filter</span><span class="sxs-lookup"><span data-stu-id="c72a3-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="c72a3-140">Podpora pro open komplexní typ.</span><span class="sxs-lookup"><span data-stu-id="c72a3-140">Support for open complex type</span></span>
- <span data-ttu-id="c72a3-141">Odebrané EntitySetController a AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="c72a3-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="c72a3-142">Změněné $link pro $ref</span><span class="sxs-lookup"><span data-stu-id="c72a3-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="c72a3-143">Přidání podpory směrování atributů</span><span class="sxs-lookup"><span data-stu-id="c72a3-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="c72a3-144">Používá OData základní knihovny 6.4.0</span><span class="sxs-lookup"><span data-stu-id="c72a3-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="c72a3-145">Atribut směrování vylepšení</span><span class="sxs-lookup"><span data-stu-id="c72a3-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="c72a3-146">Směrování atributů poskytuje bod rozšiřitelnosti volá IDirectRouteProvider, což umožňuje úplnou kontrolu nad způsob zjišťování a nakonfigurovat trasy atributů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="c72a3-147">IDirectRouteProvider zodpovídá za poskytování seznam akce a kontrolery spolu s informací o směrování přidružené k určení přesně je žádoucí jaké konfigurace směrování pro tyto akce.</span><span class="sxs-lookup"><span data-stu-id="c72a3-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="c72a3-148">Implementace IDirectRouteProvider je možné zadat při volání metody MapAttributes/MapHttpAttributeRoutes.</span><span class="sxs-lookup"><span data-stu-id="c72a3-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="c72a3-149">Přizpůsobení IDirectRouteProvider bude nejjednodušší rozšířením naší výchozí implementace DefaultDirectRouteProvider.</span><span class="sxs-lookup"><span data-stu-id="c72a3-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="c72a3-150">Tato třída poskytuje samostatné virtuální přepisovatelné metody změnit logiku pro zjišťování atributy, vytváření položky trasy a zjišťování předponu trasy a předponu oblasti.</span><span class="sxs-lookup"><span data-stu-id="c72a3-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="c72a3-151">Toto jsou některé příklady, co můžete udělat pomocí tohoto nového bodu rozšiřitelnosti:</span><span class="sxs-lookup"><span data-stu-id="c72a3-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="c72a3-152">Podpora dědičnosti atributů trasy</span><span class="sxs-lookup"><span data-stu-id="c72a3-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="c72a3-153">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c72a3-153">Example:</span></span>

    <span data-ttu-id="c72a3-154">Tady požadavek like "/ api/hodnoty/10" úspěšně vrátí "Úspěch: 10"</span><span class="sxs-lookup"><span data-stu-id="c72a3-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="c72a3-155">Zadejte název výchozí trasy pro atribut trasy podle některých vytváření názvů, který rádi používáte.</span><span class="sxs-lookup"><span data-stu-id="c72a3-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="c72a3-156">Ve výchozím nastavení směrování atributů nevytváří automaticky názvů pro atribut trasy.</span><span class="sxs-lookup"><span data-stu-id="c72a3-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="c72a3-157">Před ukládaly do směrovací tabulky, upravte šablonu trasy atributů trasy v jednom centrálním místě.</span><span class="sxs-lookup"><span data-stu-id="c72a3-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="c72a3-158">Podpora webového rozhraní API klienta pro Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="c72a3-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="c72a3-159">Nyní můžete použít balíček NuGet klientské webové rozhraní API implementovat logiku klienta vaše webového rozhraní API, při cílení na Windows Phone 8.1 nebo z v rámci univerzální aplikace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="c72a3-160">Známé problémy a změny způsobující chyby</span><span class="sxs-lookup"><span data-stu-id="c72a3-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="c72a3-161">Tato část popisuje známé problémy a novinkách v ASP.NET Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="c72a3-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="c72a3-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="c72a3-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="c72a3-163">Tvůrce modelu</span><span class="sxs-lookup"><span data-stu-id="c72a3-163">Model builder</span></span>

<span data-ttu-id="c72a3-164">Problém: Přetížených funkcí nelze vystavit jako element FunctionImport</span><span class="sxs-lookup"><span data-stu-id="c72a3-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="c72a3-165">Pokud existují 2 přetížených funkcí a jsou také element FunctionImport, jak je znázorněno níže pak si vyžádat výsledky ~/GetAllConventionCustomers(CustomerName={customerName}) v System.InvalidOperationException.</span><span class="sxs-lookup"><span data-stu-id="c72a3-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="c72a3-166">Alternativní řešení: Řešení tohoto problému je přidat jako FunctionImports přetížení funkce.</span><span class="sxs-lookup"><span data-stu-id="c72a3-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="c72a3-167">Směrování protokolu OData</span><span class="sxs-lookup"><span data-stu-id="c72a3-167">OData Routing</span></span>

<span data-ttu-id="c72a3-168">Řetězcové literály, které obsahují adresu URL kódovaný lomítko (% 2F) a backslash(%5C) způsobit chybu 404 při použití v prostředku cesty OData.</span><span class="sxs-lookup"><span data-stu-id="c72a3-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="c72a3-169">Například řetězcové literály lze použít v prostředku cesty OData jako parametry funkce nebo hodnoty klíče ze sady entit.</span><span class="sxs-lookup"><span data-stu-id="c72a3-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="c72a3-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="c72a3-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="c72a3-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="c72a3-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="c72a3-172">Pokud služby zobrazí požadavky, hostitelé se zruší řídicí ty řídicí sekvence před jejich odesláním do webového rozhraní API modulu runtime.</span><span class="sxs-lookup"><span data-stu-id="c72a3-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="c72a3-173">Tím se zajistí ochrana před útoky, jako je následující:</span><span class="sxs-lookup"><span data-stu-id="c72a3-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="c72a3-174">To způsobí, že Web API OData zásobníku vrátit chybu 404 (Nenalezeno).</span><span class="sxs-lookup"><span data-stu-id="c72a3-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="c72a3-175">K této chybě zabránit, klient by měl používat dvojitou řídicí sekvence pro lomítko (% 252F) a zpětné lomítko (% 255C).</span><span class="sxs-lookup"><span data-stu-id="c72a3-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="c72a3-176">To nestalo pro řetězce dotazu, jako je například /Employees? $filter = název eq 'název % 2F:</span><span class="sxs-lookup"><span data-stu-id="c72a3-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="c72a3-177">**Všimněte si uvozeny řídicími znaky lomítka (/) a zpětná lomítka (") nejsou povoleny u řetězcových literálů. cestu OData prostředků. Lomítka by se měla zobrazit pouze jako oddělovače cest a zpětná lomítka by se neměl zobrazit v cestě k prostředku OData vůbec. (Obojí jsou použitelné v některé části řetězce dotazu OData.)**</span><span class="sxs-lookup"><span data-stu-id="c72a3-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="c72a3-178">Alternativní řešení: Může přepsat metodu Parse DefaultODataPathHandler řídicí lomítko a zpětné lomítko u řetězcových literálů. před dokončením analýzy je ve skutečnosti.</span><span class="sxs-lookup"><span data-stu-id="c72a3-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="c72a3-179">Můžete najít ukázku tohoto postupu najdete tady.</span><span class="sxs-lookup"><span data-stu-id="c72a3-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="c72a3-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="c72a3-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="c72a3-181">[Dotazovatelný]</span><span class="sxs-lookup"><span data-stu-id="c72a3-181">[Queryable]</span></span>

<span data-ttu-id="c72a3-182">Atribut [Queryable] je zastaralá.</span><span class="sxs-lookup"><span data-stu-id="c72a3-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="c72a3-183">Nové prostředí OData v3 aplikace by měly používat **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="c72a3-184">**ODataHttpConfigurationExtensions.EnableQuerySupport** – metoda rozšíření se teď přidá **EnableQueryAttribute** do kolekce globálních filtrů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="c72a3-185">Pokud mají všechny řadiče **[Queryable]** atribut, volání `config.EnableQuerySupport()` způsobí, že **[Queryable]** atribut selhání</span><span class="sxs-lookup"><span data-stu-id="c72a3-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="c72a3-186">Doporučený postup k vyřešení tohoto problému je k nahrazení všech výskytů řetězce **položce QueryableAttribute** s **System.Web.Http.OData.EnableQueryAttribute**.</span><span class="sxs-lookup"><span data-stu-id="c72a3-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="c72a3-187">Alternativní řešení se v konfiguraci webového rozhraní API použít následující kód:</span><span class="sxs-lookup"><span data-stu-id="c72a3-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="c72a3-188">Směrování atributů</span><span class="sxs-lookup"><span data-stu-id="c72a3-188">Attribute Routing</span></span>

<span data-ttu-id="c72a3-189">Problém: Vazby modelu komplexní typ, který je upravena pomocí atributů FromUri chová odlišně při použití směrování atributů.</span><span class="sxs-lookup"><span data-stu-id="c72a3-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="c72a3-190">Následující odkaz je pro sledování problému a také obsahuje podrobnosti o alternativní řešení.</span><span class="sxs-lookup"><span data-stu-id="c72a3-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="c72a3-191">Problém: Generování uživatelského rozhraní MVC nebo webového rozhraní API do projektu s 5.2.0 balíčků následek 5.1.2 balíčky, které už neexistují v projektu</span><span class="sxs-lookup"><span data-stu-id="c72a3-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="c72a3-192">Aktualizují se balíčky NuGet pro ASP.NET MVC 5.2 nelze aktualizovat nástroje sady Visual Studio, jako je například technologie ASP.NET generování uživatelského rozhraní nebo šablonu projektu webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c72a3-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="c72a3-193">Používají předchozí verze balíčky runtime ASP.NET (např. 5.1.2 v aktualizaci Update 2).</span><span class="sxs-lookup"><span data-stu-id="c72a3-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="c72a3-194">Generování uživatelského rozhraní ASP.NET v důsledku toho nainstaluje předchozí verze (např. 5.1.2 v aktualizaci Update 2) požadované balíčky, pokud již nejsou k dispozici ve vašich projektech.</span><span class="sxs-lookup"><span data-stu-id="c72a3-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="c72a3-195">ASP.NET generování uživatelského rozhraní v aplikaci Visual Studio 2013 RTM a Update 1, ale nepřepisuje nejnovější balíčky v projektech.</span><span class="sxs-lookup"><span data-stu-id="c72a3-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="c72a3-196">Pokud používáte ASP.NET generování uživatelského rozhraní po aktualizaci balíčků projektů a Web API 2.2 technologie ASP.NET MVC 5.2, ujistěte se, že verze webového rozhraní API a architektura ASP.NET MVC jsou konzistentní vzhledem k aplikacím.</span><span class="sxs-lookup"><span data-stu-id="c72a3-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="c72a3-197">Opravy chyb a aktualizace podverze funkcí</span><span class="sxs-lookup"><span data-stu-id="c72a3-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="c72a3-198">Tato verze zahrnuje také několik oprav chyb a dílčí funkce aktualizace.</span><span class="sxs-lookup"><span data-stu-id="c72a3-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="c72a3-199">Úplný seznam najdete:</span><span class="sxs-lookup"><span data-stu-id="c72a3-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="c72a3-200">5.2 balíčku</span><span class="sxs-lookup"><span data-stu-id="c72a3-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="c72a3-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="c72a3-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="c72a3-202">Microsoft.AspNet.OData 5.2.1 balíček obsahuje aktualizace závislostí NuGet, ale žádné opravy chyb.</span><span class="sxs-lookup"><span data-stu-id="c72a3-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="c72a3-203">Od této aktualizace už není striktní závislost na Microsoft.OData.Core 6.4.0, ale jeden můžete upgradovat na kteroukoli verzi mezi 6.4.0 a 7.0.0.</span><span class="sxs-lookup"><span data-stu-id="c72a3-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="c72a3-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="c72a3-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="c72a3-205">V této verzi jsme změnili závislost u `Json.Net 6.0.4`.</span><span class="sxs-lookup"><span data-stu-id="c72a3-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="c72a3-206">Další informace o novinkách v této verzi `Json.NET`, naleznete v tématu [injektáž závislostí Json.NET 6.0 Release 4 – sloučení JSON a](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="c72a3-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="c72a3-207">Tato verze neobsahuje žádné nové funkce nebo opravy chyb v rozhraní Web API.</span><span class="sxs-lookup"><span data-stu-id="c72a3-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="c72a3-208">Následně jsme aktualizovali všechny ostatní závislé balíčky, které vlastníme, aby závisely na této nové verzi webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c72a3-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="c72a3-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span><span class="sxs-lookup"><span data-stu-id="c72a3-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="c72a3-210">Může číst informace o verzi [tady](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span><span class="sxs-lookup"><span data-stu-id="c72a3-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="c72a3-211">Tato verze obsahuje pouze oprav chyb.</span><span class="sxs-lookup"><span data-stu-id="c72a3-211">This release contains only bug fixes.</span></span> <span data-ttu-id="c72a3-212">Můžete použít [tento dotaz](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) zobrazíte seznam problémů, které jsou opravené v této verzi.</span><span class="sxs-lookup"><span data-stu-id="c72a3-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
