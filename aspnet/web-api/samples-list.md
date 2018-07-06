---
uid: web-api/samples-list
title: Ukázkové webové rozhraní API seznamu | Dokumentace Microsoftu
author: rick-anderson
description: ''
ms.author: aspnetcontent
ms.date: 09/18/2012
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 2b8376c89565ac258247425edce06e48be9846dc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824241"
---
<a name="web-api-samples-list"></a>Seznam ukázek webového rozhraní API
====================
## <a name="httpclient-samples"></a>Ukázky HttpClient

**Ukázka přeložit Bingu** | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Ukazuje, jak volat [služby Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) pomocí **HttpClient** třídy. Rozhraní API služby Microsoft Translator se vyžaduje token OAuth, které aplikace obdrží odesláním požadavku na server Azure tokenu pro každý požadavek na službu translator. Výsledek z tokenu serveru předány do požadavku odeslaného do překladatelské služby. Před spuštěním této ukázky musíte získat [klíč aplikace z Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) a vyplňte informace ve třídě AccessTokenMessageHandler vzorku.

**Vzorek mapy Google** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Používá **HttpClient** stáhnout mapu Redmond, WA z [rozhraní API služby mapy Google](https://developers.google.com/maps/), uloží ho do místního souboru a otevře výchozí prohlížeč obrázků.

**Twitter Client Sample** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Ukazuje, jak napsat jednoduchý klienta sítě Twitter **HttpClient**. Ukázka používá **objekt HttpMessageHandler** pro vložení informací o ověřování OAuth do odchozích dat **HttpRequestMessage**. Výsledek z Twitteru je pro čtení pomocí JSON.NET. Před spuštěním této ukázky musíte získat [klíč aplikace z Twitteru](https://dev.twitter.com/)a vyplňte informace ve třídě OAuthMessageHandler vzorku.

**Ukázka Světové banky** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Ukazuje, jak načíst data z webu data World Bank pomocí JSON.NET umožní výsledek analyzovat.

## <a name="web-api-samples"></a>Ukázky webového rozhraní API

**Začínáme s rozhraním ASP.NET Web API** | [zdroje VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Ukazuje, jak vytvořit základní webové rozhraní API, které podporuje požadavky HTTP GET. Obsahuje zdrojový kód pro tento kurz [vaše první rozhraní ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**ASP.NET Web API JavaScript scénáře – komentáře** | [zdroje VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Ukazuje, jak pomocí rozhraní ASP.NET Web API můžete vytvářet webová rozhraní API, které podporují klienty prohlížeče a můžete snadno volat pomocí jQuery.

**Správce kontaktů** | [zdroje VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Tento příklad používá rozhraní ASP.NET Web API k vytvoření jednoduché kontaktujte správce aplikace. Aplikace se skládá z požádejte správce webového rozhraní API, která se používá aplikace ASP.NET MVC a aplikace Windows Phone, zobrazit a spravovat seznam kontaktů.

**Dávkování ukázka** | [podrobný popis](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Ukazuje, jak implementovat dávkové zpracování protokolu HTTP v rámci technologie ASP.NET. Dávkování se skládá z uvedení více požadavků protokolu HTTP v rámci jednoho obsah entity vícedílné zprávy standardu MIME, která se pak odešlou na server jako metody POST protokolu HTTP. Požadavky se zpracovávají jednotlivě a odpovědi jsou vloženy do jiného obsahu entity vícedílné zprávy standardu MIME, který je vrácen do klienta.

**Obsah ukázkové řadič** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Ukazuje, jak číst a zapisovat entity požadavku a odpovědi asynchronně pomocí datových proudů. Ukázka kontroleru má dvě akce: PUT akci, která asynchronně načítá obsah entity žádosti a ukládá ho do místního souboru a akce GET, který vrátí obsah místního souboru.

**Ukázková překladač sestavení** | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Ukazuje, jak upravit rozhraní ASP.NET Web API pro podporu zjišťování řadičů ze sestavení dynamicky načtené knihovny. Ukázka implementuje vlastní **IAssembliesResolver** který volá výchozí implementace a přidá do výchozí výsledky sestavení knihovny.

**Vlastní vzorek formátovací modul typu média** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Ukazuje, jak vytvořit formátovací modul typu média vlastní pomocí **BufferedMediaTypeFormatter** základní třídy. Tato základní třída slouží k formátování, které primárně využívají synchronního čtení a operace zápisu. Kromě zobrazení formátovací modul typu média, ukázky, ukazuje, jak zapojit ji tak, že zaregistrujete jako součást **HttpConfiguration** pro vaši aplikaci. Všimněte si, že je také možné použít **objekt MediaTypeFormatter** přímo, základní třída pro formátovací moduly, které používají primárně asynchronní operace čtení a zápisu.

**Parametr vazby ukázková** | [podrobný popis](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Ukazuje, jak přizpůsobit proces vytváření vazby parametrů, což je proces, který určuje, jak informace z požadavku je vázána na parametry akce. V této ukázce má kontroler Home čtyř akcí:

1. BindPrincipal ukazuje, jak vytvořit vazbu pomocí parametru IPrincipal z vlastní obecný instančnímu objektu, nikoli z zprávy HTTP GET.
2. BindCustomComplexTypeFromUriOrBody ukazuje, jak vytvořit vazbu parametr komplexního typu, který může pocházet z textu zprávy nebo z identifikátoru URI zpráva HTTP POST; požadavku
3. BindCustomComplexTypeFromUriWithRenamedProperty ukazuje, jak vytvořit vazbu parametr typu složité přejmenováno vlastnost, která pochází z identifikátoru URI zpráva HTTP POST; požadavku
4. PostMultipleParametersFromBody ukazuje, jak vytvořit vazbu více parametrů z textu příspěvku zprávy;

**Ukázka nahrávání do souboru** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Ukazuje, jak nahrávat soubory do **objektu ApiController** pomocí MIME Multipart soubor nahrát a jak nastavit oznámení o průběhu s **HttpClient** pomocí **ProgressNotificationHandler**. Kontroler asynchronně přečte obsah nahrání souboru HTML a zapíše jeden nebo více částí textu do místního souboru. Odpověď obsahuje informace o nahraného souboru (nebo souborů).

**Soubor nahrát do Azure Blob Store Sample** | [podrobný popis](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Tento příklad je podobný ukázka nahrávání do souboru, ale místo uložení nahraných souborů na místním disku, soubory, které chcete asynchronně odešle [Store objektů Blob v Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) pomocí [Windows Azure SDK for .NET](https://www.windowsazure.com/develop/net/). Také poskytuje mechanismus pro výpis objektů BLOB v tuto chvíli dostupné [kontejneru úložiště objektů Blob Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Budete moct vyzkoušet ukázkové spouštění **emulátoru úložiště Azure** , který je součástí sady Azure SDK. Pokud máte [účet služby Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), můžete spustit na skutečné úložiště službu.

**Ukázka kanálu obslužné rutiny zpráv HTTP** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Ukazuje, jak nastavit **objekt HttpMessageHandler** instance obou klientovi (**HttpClient**) a serveru (rozhraní ASP.NET Web API). V ukázce se používá stejnou obslužnou rutinu na klienta a serveru. I když je vzácné, že by přesně stejnou obslužnou rutinu spusťte na obou místech, objektový model je stejná na straně klienta a serveru.

**Ukázka nahrávání do formátu JSON** | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Ukazuje, jak nahrávat a stahovat JSON do a z **objektu ApiController**. Ukázka používá minimální **objektu ApiController** a přistupuje k nim pomocí **HttpClient**.

**Hybridní webové aplikace ukázkový** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Ukazuje, jak získat přístup k více vzdálených lokalit asynchronně v rámci **objektu ApiController** akce. Pokaždé, když je volání akce, požadavky se provádějí asynchronně, tak, aby žádná vlákna jsou zablokované.

**Ukázková trasování paměti** | [podrobný popis](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Tento ukázkový projekt vytvoří balíček Nuget, který bude instalovat zapisovač vlastního trasování v paměti v aplikacích ASP.NET Web API.

**Ukázka MongoDB** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Ukazuje, jak používat MongoDB jako trvalé úložiště pro **objektu ApiController**, použití modelu úložiště.

**Ukázku procesoru tělo odpovědi** | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Ukazuje, jak zkopírovat odpověď entity (to znamená, textu odpovědi HTTP) do místního souboru před odesláním do klienta a provádět další u tohoto souboru asynchronní zpracování. Implementuje vzorek **objekt HttpMessageHandler** , který zabalí odpověď entity s jedním, že i samotný zapíše do výstupu jako za normálních okolností a do místního souboru.

**Nahrát ukázku pro XDocument** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [zdroje VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Ukazuje, jak nahrát XDocument do **objektu ApiController** pomocí **PushStreamContent** a **HttpClient**.

**Ukázka ověřování** | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Ukazuje, jak pomocí atributů ověření na vašich modelů v ASP.NET WebAPI ověřit obsah požadavku HTTP. Popisuje, jak lze označit vlastnosti podle potřeby, jak chcete-li použít definované v rámci rozhraní a vlastní ověřovací atributy opatřit váš model a vrátí chybové odpovědi pro stavy modelu je neplatný.

**Webové formuláře ukázka** | [podrobný popis](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Ukazuje objektu ApiController přidány do projektu webových formulářů.

**[Ukázka RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs je jednoduchý ladicí aplikace, která ukazuje, jak vytvořit systém řízené hypermédia pomocí rozhraní ASP.NET Web API a nové knihovny klienta HTTP. Ukázka zahrnuje implementace klienta i serveru, pomocí rozhraní ASP.NET Web API. Server používá vlastní formátování syntaxe Razor pro generování reprezentace prostředků. Ukázka také poskytuje server node.js pro ilustraci výhody, které pocházejí z technologie návrh hypermédia oddělit klienty a servery.

## <a name="web-api-extensions-preview-samples"></a>Webové rozhraní API rozšíření Náhled ukázky

**Ukázka zadávat dotazy OData** | [podrobný popis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Ukazuje, jak zavést dotazů OData v buď pomocí rozhraní ASP.NET Web API `[Queryable]` atribut nebo pomocí **ODataQueryOptions** parametr akce, která umožňuje akci, která se ručně si tento dotaz prohlédnout před jeho provedením.

Ukazuje CustomerController pomocí atributu [Queryable] a OrderController ukazuje, jak použít parametr ODataQueryOptions. Se podobá ResponseController CustomerController, ale místo vrácení akce GET `IEnumerable<Customer>` vrátí **objekt HttpResponseMessage**. To umožňuje přidat pole dodatečné hlavičky, stále přitom funkce dotazů zpracování stavový kód atd. Ukázka znázorňuje dotazy, které používají $orderby, $skip, $top, metoda any(), all() a $filter.

**Ukázka služby OData** | [podrobný popis](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [zdroje VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Tento příklad ukazuje, jak vytvořit služby OData, který se skládá z tři entit a tři kontrolerů rozhraní Web API. Kontrolery poskytují různé úrovně funkčnosti z hlediska funkci prostředí OData, které vystavují:

SupplierController zpřístupňuje podmnožinu funkcí, včetně dotazů, získejte klíč a vytvoření, řeší tyto požadavky:

- ZÍSKAT /Suppliers
- ZÍSKAT /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- /Suppliers příspěvku

Zpřístupňuje ProductsController GET, PUT, POST, odstranit a oprava implementací akci pro každou z těchto operací přímo.

ProductFamilesController využívá EntitySetController základní třídu, která poskytuje užitečné vzor pro implementování bohaté služby OData.

Kromě toho služba OData zveřejňuje dokument $metadata, který umožňuje dat spotřebované klienti WCF Data Service a jiných klientů, které přijímají formátu $metadata.
