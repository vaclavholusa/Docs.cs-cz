---
uid: web-api/overview/advanced/calling-a-web-api-from-a-net-client
title: Volání webového rozhraní API z klienta .NET (C#) | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 11/24/2017
msc.legacyurl: /web-api/overview/advanced/calling-a-web-api-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: be237bee43bc5e32939cb0b3e0948fd8b35bd1eb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755199"
---
<a name="call-a-web-api-from-a-net-client-c"></a>Volání webového rozhraní API z klienta .NET (C#)
====================
podle [Mike Wasson](https://github.com/MikeWasson) a [Rick Anderson](https://twitter.com/RickAndMSFT)

[Stáhnout dokončený projekt](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample). [Pokyny ke stažení](/aspnet/core/tutorials/#how-to-download-a-sample). 

Tento kurz ukazuje postupy při zavolání webového rozhraní API z aplikace .NET pomocí [System.Net.Http.HttpClient.](https://msdn.microsoft.com/library/system.net.http.httpclient(v=vs.110).aspx)

V tomto kurzu je zapsán klientskou aplikaci, že následující webové rozhraní API, který využívá:

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání produktu podle ID | ZÍSKAT | / webové rozhraníAPI/produkty/*id* |
| Vytvořit nový produkt | PŘÍSPĚVEK | / api/produkty |
| Aktualizace produktu | PUT | / webové rozhraníAPI/produkty/*id* |
| Odstranit produkt | DELETE | / webové rozhraníAPI/produkty/*id* |

Další postup implementace tohoto rozhraní API s rozhraním ASP.NET Web API najdete v tématu [této operace CRUD podporuje vytvoření webového rozhraní API](xref:web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
).

Pro jednoduchost klientská aplikace v tomto kurzu je konzolová aplikace Windows. **HttpClient** je také podporována pro aplikace pro Windows Phone a Windows Store. Další informace najdete v tématu [psaní webové rozhraní API klienta kódu pro více platforem, pomocí přenosné knihovny](https://blogs.msdn.com/b/webdev/archive/2013/07/19/writing-web-api-client-code-for-multiple-platforms-using-portable-libraries.aspx)

<a id="CreateConsoleApp"></a>
## <a name="create-the-console-application"></a>Vytvoření konzolové aplikace

V sadě Visual Studio vytvořte novou konzolovou aplikaci s Windows s názvem **HttpClientSample** a vložte následující kód:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_all)]

Předchozí kód je kompletní klientské aplikace.

`RunAsync` spuštění a blokuje, dokud se nedokončí. Většina **HttpClient** metody jsou asynchronní, protože jejich provádění v / v sítě. Všechny asynchronní úlohy dokončení uvnitř `RunAsync`. Obvykle aplikace nebrání v hlavním vlákně, ale tuto aplikaci neumožňuje zásahu.

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_run)]

<a id="InstallClientLib"></a>
## <a name="install-the-web-api-client-libraries"></a>Instalace webové rozhraní API klientské knihovny

Použití Správce balíčků NuGet nainstalujte balíček webové rozhraní API klientské knihovny.

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** > **Konzola správce balíčků**. V balíčku správce konzoly (konzolu PMC), zadejte následující příkaz:

`Install-Package Microsoft.AspNet.WebApi.Client`

Ve výstupu předchozího příkazu přidá následující balíčky NuGet do projektu:

* Microsoft.AspNet.WebApi.Client
* Newtonsoft.Json

Json.NET je oblíbená architektura výkonné JSON pro .NET.

<a id="AddModelClass"></a>
## <a name="add-a-model-class"></a>Přidejte třídu modelu

Zkontrolujte `Product` třídy:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_prod)]

Tato třída odpovídá datového modelu používají webové rozhraní API. Můžete použít aplikaci **HttpClient** ke čtení `Product` instanci z odpovědi HTTP. Aplikace nebude muset psát kód deserializace.

<a id="InitClient"></a>
## <a name="create-and-initialize-httpclient"></a>Vytváření a inicializace HttpClient

Prozkoumejte statické **HttpClient** vlastnost:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_HttpClient)]

**HttpClient** by se měly vytvořit jednou a opětovně používat v rámci životnosti aplikace. Následující podmínky mohou způsobit **socketexception –** chyby:

* Vytvoření nového **HttpClient** instance pro každý požadavek.
* Server v případě velkého zatížení.

Vytvoření nového **HttpClient** instance pro každý požadavek lze vyčerpání dostupných soketů.

V následujícím kódu inicializuje **HttpClient** instance:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5)]

Předchozí kód:

* Nastaví základní identifikátor URI pro požadavky HTTP. Změňte číslo portu na port v serveru aplikaci. Aplikace nebude fungovat, pokud se používá port pro serverovou aplikaci.
* Nastaví hlavičku Accept na "application/json". Nastavení této hlavičky určuje server k odesílání dat ve formátu JSON.

<a id="GettingResource"></a>
## <a name="send-a-get-request-to-retrieve-a-resource"></a>Odeslat požadavek GET na načtení prostředku

Následující kód odešle požadavek GET pro produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_GetProductAsync)]

**GetAsync** metoda odešle požadavek HTTP GET. Když dokončení metody, vrátí **objekt HttpResponseMessage** , který obsahuje odpověď HTTP. Pokud stavový kód odpovědi na kód úspěchu, tělo odpovědi obsahuje reprezentaci JSON produktu. Volání **ReadAsAsync** datovou část JSON k deserializaci `Product` instance. **ReadAsAsync** metoda je asynchronní, protože tělo odpovědi může být libovolně velké.

**HttpClient** nevyvolá výjimku, pokud odpověď HTTP, která obsahuje kód chyby. Místo toho **IsSuccessStatusCode** vlastnost je **false** -li stav nastaven chybový kód. Pokud chcete přistupovat ke všem chybových kódech HTTP jako výjimky, volání [HttpResponseMessage.EnsureSuccessStatusCode](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.ensuresuccessstatuscode(v=vs.110).aspx) v objektu odpovědi. `EnsureSuccessStatusCode` vyvolá výjimku, pokud se stavovým kódem spadá mimo rozsah 200&ndash;299. Všimněte si, že **HttpClient** může vyvolat výjimky z jiných důvodů &mdash; například, pokud vyprší časový limit žádosti.

<a id="MediaTypeFormatters"></a>
### <a name="media-type-formatters-to-deserialize"></a>Formátovací moduly typu médií k deserializaci

Když **ReadAsAsync** je volána bez parametrů, použije výchozí sadu *formátovací moduly médií* ke čtení textu odpovědi. Výchozí formátování Podpora JSON, XML a data formuláře kódovaná adresy url.

Místo použití výchozí formátování, můžete zadat seznam formátovacích modulů k **ReadAsAsync** metody.  Použití seznam formátovacích modulů je užitečné, pokud máte vlastní typ média formátování:

```csharp
var formatters = new List<MediaTypeFormatter>() {
    new MyCustomFormatter(),
    new JsonMediaTypeFormatter(),
    new XmlMediaTypeFormatter()
};
resp.Content.ReadAsAsync<IEnumerable<Product>>(formatters);
```

Další informace najdete v tématu [formátovací moduly médií ve ASP.NET Web API 2](../formats-and-model-binding/media-formatters.md)

## <a name="sending-a-post-request-to-create-a-resource"></a>Odeslání požadavku POST vytvořit prostředek

Následující kód odešle požadavek POST, který obsahuje `Product` instance ve formátu JSON:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_CreateProductAsync)]

**PostAsJsonAsync** metody:

* Serializuje objekt do formátu JSON.
* Odešle datové části JSON v požadavku POST.

Pokud je žádost úspěšná:

* Měla by vrátit odpověď 201 (vytvořeno).
* Má odpověď obsahovat adresu URL vytvořené prostředky v hlavičce Location.

<a id="PuttingResource"></a>
## <a name="sending-a-put-request-to-update-a-resource"></a>Odesílání požadavek PUT pro aktualizace prostředku

Následující kód odešle požadavek PUT pro aktualizaci produktu:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_UpdateProductAsync)]

**PutAsJsonAsync** metoda se dá použít jako **PostAsJsonAsync**, s tím rozdílem, že odešle žádost PUT místo příspěvku.

<a id="DeletingResource"></a>
## <a name="sending-a-delete-request-to-delete-a-resource"></a>Odesílání požadavku na odstranění odstranění prostředku

Následující kód odešle žádost o odstranění se odstranit produkt:

[!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet_DeleteProductAsync)]

Třeba GET žádost o odstranění nemá tělo požadavku. Není nutné zadat ve formátu JSON nebo XML DELETE.

## <a name="test-the-sample"></a>Testování ukázky

Testování aplikace klienta:

1. [Stáhněte si](https://github.com/aspnet/Docs/tree/master/aspnet/web-api/overview/advanced/calling-a-web-api-from-a-net-client/sample/server) a spusťte serverovou aplikaci. [Pokyny ke stažení](/aspnet/core/tutorials/#how-to-download-a-sample). Ověřte, že server aplikace funguje. Pro exaxmple `http://localhost:64195/api/products` by měla vrátit seznam produktů.
2. Nastaví základní identifikátor URI pro požadavky HTTP. Změňte číslo portu na port v serveru aplikaci.
    [!code-csharp[Main](calling-a-web-api-from-a-net-client/sample/client/Program.cs?name=snippet5&highlight=2)]

3. Spuštění klientské aplikace. Následující výstup je vytvořen:

   ```console
   Created at http://localhost:64195/api/products/4
   Name: Gizmo     Price: 100.0    Category: Widgets
   Updating price...
   Name: Gizmo     Price: 80.0     Category: Widgets
   Deleted (HTTP Status = 204)
   ```
