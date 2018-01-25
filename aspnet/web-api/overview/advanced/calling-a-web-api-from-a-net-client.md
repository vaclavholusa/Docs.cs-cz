---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: "Volání webového rozhraní API z klienta .NET (C#) | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/24/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 8156bd1c7cfc111a6a121a89d845ca284ee1b7af
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="call-a-web-api-from-a-net-client-c"></a>Volání webového rozhraní API z klienta .NET (C#)
====================
podle [Karel Wasson](https://github.com/MikeWasson) a [Rick Anderson](https://twitter.com/RickAndMSFT)

[Stáhněte si dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample)

Tento kurz ukazuje způsob volání webového rozhraní API z aplikace .NET, pomocí [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

V tomto kurzu je zapsán klientskou aplikaci, která využívá následující webové rozhraní API:

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání ID produktu | GET | /api/products/*id* |
| Vytvoření nového produktu | POST | / api/produkty |
| Aktualizace produktu | PUT | /api/products/*id* |
| Odstranit produktu | DELETE | /api/products/*id* |

Pokud chcete dozvědět, jak implementovat toto rozhraní API s rozhraním ASP.NET Web API, přečtěte si téma [vytváření webového rozhraní API této operace CRUD podporuje](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Pro jednoduchost klientská aplikace v tomto kurzu je konzolová aplikace z Windows. **HttpClient** je také podporována pro aplikace Windows Phone a Windows Store. Další informace najdete v tématu [psaní webové rozhraní API klienta kódu pro více platforem, pomocí přenosné knihovny](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Vytvoření konzolové aplikace

V sadě Visual Studio vytvořte novou konzolovou aplikaci systému Windows s názvem **HttpClientSample** a vložte následující kód:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Předchozí kód je kompletní klientské aplikace.

`RunAsync`Spustí a bloky až do dokončení. Většina **HttpClient** metody jsou asynchronní, protože fungují v/v sítě. Všechny asynchronních úloh se provádějí v `RunAsync`. Za normálních okolností aplikace neblokuje hlavního vlákna, ale tato aplikace nebude povolovat interakce.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Nainstalujte knihovny webové rozhraní API klienta

Použití Správce balíčků NuGet nainstalujte balíček knihovny klienta webové rozhraní API.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**. V balíček správce konzoly (pomocí PMC), zadejte následující příkaz:

`Install-Package Microsoft.AspNet.WebApi.Client`

Předchozí příkaz přidá následující balíčky NuGet do projektu:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET je oblíbených rozhraní vysoce výkonné JSON pro rozhraní .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Přidejte třídu modelu

Zkontrolujte `Product` třídy:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Tato třída neodpovídá modelu dat používá webové rozhraní API. Aplikace můžete použít **HttpClient** ke čtení `Product` instance z odpovědi HTTP. Aplikace nemá psaní jakéhokoli kódu deserializace.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Vytvoření a inicializace HttpClient

Zkontrolujte statických **HttpClient** vlastnost:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** je určená pro vytvoření instance jednou a znovu po celou dobu životnosti aplikace. Může mít za následek následující podmínky **SocketException** chyby:

* Vytvoření nové **HttpClient** instance pro každý požadavek.
* Server v případě velkého zatížení.

Vytvoření nové **HttpClient** instance pro každý požadavek může vyčerpat dostupných soketů.

Následující kód inicializuje **HttpClient** instance:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Předchozí kód:

* Nastaví základní identifikátor URI pro požadavky HTTP. Změňte číslo portu na port použitý při aplikace serveru. Aplikace nebude fungovat, pokud se používá port pro aplikace serveru.
* Nastaví hlavičku Accept pro "application/json". Nastavení této hlavičky informuje server k odesílání dat ve formátu JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Odeslat požadavek GET pro načtení prostředku

Následující kód odešle požadavek GET pro produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** metoda odešle požadavek HTTP GET. Po metodě dokončení vrátí **objekt HttpResponseMessage** , který obsahuje odpověď HTTP. Pokud stavový kód v odpovědi kód úspěch, text odpovědi obsahuje reprezentace JSON produktu. Volání **ReadAsAsync** k datové části JSON k deserializaci `Product` instance. **ReadAsAsync** metoda je asynchronní, protože text odpovědi lze libovolně velké.

**HttpClient** nevyvolá výjimku, pokud odpověď HTTP, která obsahuje kód chyby. Místo toho **IsSuccessStatusCode** vlastnost je **false** Pokud je stav chybový kód. Pokud dáváte přednost zacházet s kódy chyb HTTP jako výjimky, volání [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) na objekt odpovědi. `EnsureSuccessStatusCode`vyvolá výjimku, pokud kód stavu spadá mimo rozsah 200&ndash;299. Všimněte si, že **HttpClient** může vyvolat výjimky z jiných důvodů &mdash; například, pokud vyprší časový limit žádosti.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formátovací moduly typu média k deserializaci

Když **ReadAsAsync** je volána bez parametrů, používá výchozí sadu *formátovací moduly pro média* ke čtení textu odpovědi. Výchozí formátování podporovat JSON, XML a data formuláře kódovaná adresy url.

Místo použití výchozí formátování, můžete zadat seznam formátovací moduly, které **ReadAsAsync** metoda.  Použití seznam formátovacích modulů je užitečné, pokud máte vlastní typ média formátování:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Další informace najdete v tématu [formátovací moduly pro média ve webovém rozhraní API 2 ASP.NET](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Odesílání požadavku POST k vytvoření prostředku

Odešle požadavek POST, který obsahuje následující kód `Product` instance ve formátu JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** metoda:

* Serializuje objekt do formátu JSON.
* Odešle datové části JSON v požadavku POST.

Pokud neproběhne:

* Měla by vrátit odpověď 201 (vytvořeno).
* Odpověď by měla obsahovat adresu URL vytvořené prostředky hlavička umístění.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Odesílání PUT žádost o aktualizaci prostředku

Následující kód odešle požadavek PUT pro aktualizaci produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** metoda se dá použít jako **PostAsJsonAsync**kromě toho, že odešle požadavek PUT místo POST.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Odesílání požadavku na odstranění odstranění prostředku

Následující kód odešle požadavek DELETE k odstranění produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Žádost o odstranění jako GET, nemá textu požadavku. Nemusíte zadejte ve formátu JSON nebo XML s odstranit.

## <a name="test-the-sample"></a>Testování vzorku

K testování aplikace klienta:

1. [Stáhněte si](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) a spuštění aplikace serveru. [Pokyny ke stahování](https://docs.microsoft.com/aspnet/core/tutorials/#how-to-download-a-sample). Ověřte, zda že je funkční aplikace serveru. Pro exaxmple `http://localhost:64195/api/products` by měla vrátit seznam produktů.
2. Nastaví základní identifikátor URI pro požadavky HTTP. Změňte číslo portu na port použitý při aplikace serveru.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Spusťte klientskou aplikaci. Vytváří následující výstup:

 ```console
 Created at http://localhost:64195/api/products/4
Name: Gizmo     Price: 100.0    Category: Widgets
Updating price...
Name: Gizmo     Price: 80.0     Category: Widgets
Deleted (HTTP Status = 204)
```
