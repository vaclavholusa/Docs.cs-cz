---
uid: web-api/samples-list
title: Webové rozhraní API ukázky seznamu | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28038985"
---
<a name="web-api-samples-list"></a>Seznam ukázek rozhraní Web API
====================
## <a name="httpclient-samples"></a>Ukázky HttpClient

**Ukázka převede Bing** | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Ukazuje způsob volání [služby Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) pomocí **HttpClient** třídy. Rozhraní API služby Microsoft Translator vyžaduje tokenu OAuth, které aplikace obdrží odesláním požadavku na server Azure tokenu pro každý požadavek pro službu překladač. Výsledek z tokenu serveru předány do požadavek odeslaný do služby překladu. Před spuštěním této ukázce, je nutné získat [klíč aplikace z Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) a vyplňte informace v ukázkové třídy AccessTokenMessageHandler.

**Ukázka mapy Google** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Používá **HttpClient** ke stažení mapu Brno z [rozhraní API map Google](https://developers.google.com/maps/), uloží ho do místního souboru a otevře výchozí prohlížeč obrázků.

**Ukázka klienta služby Twitter** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Ukazuje, jak napsat jednoduchý klienta sítě Twitter **HttpClient**. Ukázce se používá **HttpMessageHandler** vložení informace o ověřování OAuth do odchozích **HttpRequestMessage**. Výsledek z Twitter je pro čtení pomocí technologie JSON.NET. Před spuštěním této ukázce, je nutné získat [klíč aplikace z Twitteru](https://dev.twitter.com/)a zadejte informace ve třídě OAuthMessageHandler ukázka.

**Ukázka World Bank** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 zdroj](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Ukazuje, jak načíst data z webu data World Bank pomocí technologie JSON.NET analyzovat výsledek.

## <a name="web-api-samples"></a>Ukázky webového rozhraní API

**Začínáme s rozhraním ASP.NET Web API** | [VS 2012 zdroje](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Ukazuje, jak vytvořit základní webové rozhraní API, který podporuje požadavky HTTP GET. Obsahuje zdrojový kód pro tento kurz [vaše první rozhraní ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Rozhraní ASP.NET Web API JavaScript scénáře – komentáře** | [VS 2012 zdroje](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Ukazuje, jak používat rozhraní ASP.NET Web API pro vytváření webových rozhraní API, které podporují klienty prohlížeče a lze snadno volat pomocí jQuery.

**Obraťte se na správce** | [zdroj VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Tato ukázka používá k vytvoření jednoduché kontaktujte správce aplikace rozhraní ASP.NET Web API. Aplikace se skládá z kontaktujte správce webového rozhraní API používané aplikace ASP.NET MVC a aplikace Windows Phone k zobrazení a správě seznamu kontaktů.

**Dávkování ukázka** | [podrobný popis](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Ukazuje, jak implementovat HTTP dávkování v rámci technologie ASP.NET. Dávkování tvořený uvedení více požadavků HTTP v rámci jedné obsahu entity vícedílné zprávy standardu MIME, která se pak posílají na server jako HTTP POST. Požadavky jsou zpracovávány jednotlivě a odpovědi jsou vloženy do jiného obsahu MIME s více částmi entity, který je vrácen do klienta.

**Obsah ukázkové řadič** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 zdroj](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Ukazuje, jak číst a zapisovat entity požadavku a odpovědi asynchronně pomocí datových proudů. Ukázka řadiče má dvě akce: PUT akce, který asynchronně čte obsah entity žádosti a ukládá ho do místního souboru a akce GET, který vrátí obsah místního souboru.

**Vlastní vzorek překladač sestavení** | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Ukazuje, jak upravit webového rozhraní API ASP.NET pro podporu zjišťování řadičů ze sestavení dynamicky načíst knihovny. Ukázka implementuje vlastní **IAssembliesResolver** který volá výchozí implementace a pak přidá do výchozí výsledků sestavení knihovny.

**Vlastní vzorek formátovací modul typu média** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Ukazuje, jak vytvořit formátovací modul typu média vlastní pomocí **BufferedMediaTypeFormatter** základní třídy. Tato základní třída je určený pro formátovací moduly, které především použít synchronní pro čtení a zápisu operace. Kromě zobrazení formátovací modul typu média, ukázky ukazuje, jak spojit tak, že zaregistrujete v rámci **HttpConfiguration** pro vaši aplikaci. Všimněte si, že je také možné použít **objekt MediaTypeFormatter** přímo, základní třída pro formátovací moduly, které používají primárně asynchronní operacích čtení a zápisu.

**Ukázka vlastní vazby parametru** | [podrobný popis](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Ukazuje, jak přizpůsobit proces vytváření vazby parametru, což je proces, který určuje, jak informace z požadavku je vázána na parametry akce. V této ukázce má řadičem domovské čtyř akcí:

1. BindPrincipal ukazuje, jak vytvořit vazbu parametru IPrincipal z vlastní obecné zabezpečení, není ze zprávy HTTP GET;
2. BindCustomComplexTypeFromUriOrBody ukazuje, jak vytvořit vazbu parametru komplexní typ, který by mohl pocházet z textu zprávy nebo z identifikátoru URI zprávy HTTP POST; požadavku
3. BindCustomComplexTypeFromUriWithRenamedProperty ukazuje, jak vytvořit vazbu parametr typu komplexní přejmenovat vlastnosti, která pochází z identifikátoru URI zprávy HTTP POST; požadavku
4. PostMultipleParametersFromBody ukazuje, jak vytvořit vazbu několik parametrů z textu zprávy POST;

**Soubor nahrát ukázková** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Ukazuje, jak odesílat soubory **objektu ApiController** pomocí MIME Multipart soubor odeslán a jak nastavit průběh oznámení s **HttpClient** pomocí **ProgressNotificationHandler**. Řadičem asynchronně čte obsah nahrávaný soubor HTML a zapisuje jeden nebo více částí textu do místního souboru. Odpověď obsahuje informace o nahraný soubor (nebo soubory).

**Soubor nahrávání ukázce úložiště objektů Blob Azure** | [podrobný popis](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Tato ukázka je podobný ukázce nahrát soubor, ale místo uložení na místní disk, odeslané soubory, soubory asynchronně odešle [úložišti objektů Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) pomocí [Windows Azure SDK pro .NET](https://www.windowsazure.com/develop/net/). Také poskytuje mechanismus pro výpis aktuálně nachází v objektů blob [kontejner úložiště objektů Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Můžete vyzkoušet ukázkový spuštěným pro **emulátoru úložiště Azure** dodávaný společně s nástrojem Azure SDK. Pokud máte [účet úložiště Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), můžete spustit službě skutečné úložiště.

**Ukázka kanálu obslužné rutiny zpráv HTTP** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Ukazuje, jak propojit až **HttpMessageHandler** instancí na obou klienta (**HttpClient**) a server (rozhraní ASP.NET Web API). V ukázce stejné obslužná rutina se používá na klientovi a serveru. I když je taková situace vzácná, že by přesně stejnou obslužná rutina spustí na obou místech, objektový model je stejná na straně klienta a serveru.

**Vzorek nahrání** | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Ukazuje, jak nahrávání a stahování JSON do a z **objektu ApiController**. Ukázce se používá minimálním **objektu ApiController** a přistupuje pomocí **HttpClient**.

**Ukázka hybridní webové aplikace** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Ukazuje, jak pro přístup k více vzdálených lokalit asynchronně uvnitř **objektu ApiController** akce. Pokaždé, když je dosaženo akci, požadavky se asynchronně, tak, aby žádné vláken nejsou blokovány.

**Ukázka trasování paměti** | [podrobný popis](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Tento ukázkový projekt vytvoří balíček Nuget, který bude instalovat zapisovač vlastního trasování v paměti v aplikacích ASP.NET Web API.

**Ukázka MongoDB** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Ukazuje, jak používat MongoDB jako trvalé úložiště pro **objektu ApiController**, pomocí vzoru úložiště.

**Ukázka procesoru textu odpovědi** | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Ukazuje, jak zkopírovat odpověď entity (tedy textu odpovědi HTTP) do místního souboru před odesláním do klienta a provádět další zpracování u tohoto souboru, asynchronně. Ukázka implementuje **HttpMessageHandler** který zabalí odpověď entity s jedním, obě samotné zapíše na výstup jako normální a do místního souboru.

**Odeslání vzorku XDocument** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [VS 2012 zdroje](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Ukazuje, jak nahrát XDocument k **objektu ApiController** pomocí **PushStreamContent** a **HttpClient**.

**Ukázka ověřování** | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Ukazuje, jak můžete pomocí atributů ověření na vašem modely v ASP.NET WebAPI k ověření obsahu žádosti HTTP. Ukazuje, jak označit vlastnosti, jak je potřeba, jak používat obě definované framework a atributy vlastního ověřování pro přidání poznámek k modelu a jak vracet chybové odpovědi neplatný model stavů.

**Webové formuláře ukázka** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Zobrazuje objektu ApiController přidat do projektu webové formuláře.

**[Ukázka RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs je jednoduchý chyby sledování aplikace, která ukazuje, jak vytvořit řízené hypermédií systém pomocí rozhraní ASP.NET Web API a nové knihovny klienta HTTP. Ukázka zahrnuje implementace klient i server, pomocí rozhraní ASP.NET Web API. Server používá ke generování reprezentace prostředku vlastní formátovací modul Razor. Ukázka také poskytuje server node.js pro ilustraci výhody, které pocházejí z pomocí návrhu hypermédií oddělit klienty a servery.

## <a name="web-api-extensions-preview-samples"></a>Ukázky Preview rozšíření webové rozhraní API

**Ukázka dotazovatelný OData** | [podrobný popis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Ukazuje, jak zavést dotazů OData v buď pomocí rozhraní ASP.NET Web API `[Queryable]` atribut, nebo pomocí **ODataQueryOptions** parametr akce, který umožňuje akci ručně zkontrolovat dotaz předtím, než je spouštěna.

CustomerController ukazuje pomocí atributu [Queryable] a OrderController ukazuje, jak pomocí parametru ODataQueryOptions. ResponseController je podobný CustomerController, ale místo GET akce vrácení `IEnumerable<Customer>` vrátí **objekt HttpResponseMessage**. To umožňuje přidat pole hlavičky navíc, při stále pomocí dotazu funkce manipulaci s stavový kód atd. Ukázka znázorňuje dotazy pomocí $orderby, $skip, $top, any(), all() a $filter.

**Ukázka služby OData** | [podrobný popis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [zdroj VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Tato ukázka ukazuje postup vytvoření služby OData skládající se z tři entity a tři řadiče webového rozhraní API. V řadičích poskytují různé úrovně funkce z hlediska funkci prostředí OData, které vystavují:

SupplierController vystavuje podmnožinu funkcí, včetně dotazů, získat tak, že klíč a vytvořit, podle zpracování tyto požadavky:

- ZÍSKAT /Suppliers
- ZÍSKAT /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

Zpřístupňuje ProductsController GET, PUT, POST, odstraňování a oprava implementací úkonem pro každou z těchto operací přímo.

ProductFamilesController využívá EntitySetController základní třídu, která zpřístupňuje užitečné vzor pro implementaci bohaté služby OData.

Kromě toho služba OData zpřístupní dokument $metadata, který umožňuje data, která mají spotřebované klienti služby WCF Data Service a ostatní klienty, které přijímají formát $metadata.
