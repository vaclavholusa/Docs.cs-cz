---
title: Middleware ASP.NET Core
author: rick-anderson
description: "Další informace o ASP.NET Core middleware a kanál požadavku."
ms.author: riande
manager: wpickett
ms.date: 01/22/2018
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: ef130e736e2f32fa134156d979ce5bfbedcae828
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/22/2018
---
# <a name="aspnet-core-middleware-fundamentals"></a>Základy Middleware ASP.NET Core

<a name="fundamentals-middleware"></a>

Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Steve Smith](https://ardalis.com/)

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-middleware"></a>Co je middleware?

Middleware je software, který je sestavit do kanálu určité aplikace pro zpracování požadavků a odpovědí. Jednotlivé komponenty:

* Zvolí, jestli se k předání požadavku na další komponenta v kanálu.
* Můžete práci před a po vyvolání další komponenta v kanálu. 

Delegáti požadavku se používají k vytvoření kanálu požadavku. Delegáti požadavek zpracovat každý požadavek HTTP.

Žádosti o Delegáti jsou konfigurováni pomocí [spustit](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [mapy](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), a [použití](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) rozšiřující metody. Delegáta individuální žádosti může být zadaný v řádku jako anonymní metody (nazývané v řádku middleware), nebo může být definováno v třídě opakovaně použitelné. Tyto opakovaně použitelné třídy a metody anonymní v řádku jsou *middleware*, nebo *komponenty middlewaru*. Jednotlivé komponenty middleware v kanálu požadavku zodpovídá za vyvolání další komponenta v kanálu nebo krátká smyčka řetězu v případě potřeby.

[Migrace modulů HTTP s Middlewarem](../migration/http-modules.md) najdete vysvětlení rozdílu mezi požadavek kanály v ASP.NET Core a předchozí verze a poskytuje další middleware ukázky.

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a>Vytvoření kanálu middlewaru s IApplicationBuilder

Kanál požadavku ASP.NET Core se skládá z posloupnost delegáti požadavku, říká jeden za druhým, tento diagram zobrazuje (vlákno provádění způsobem černé šipky):

![Žádost o zpracování vzoru zobrazující žádost přicházejících, zpracování prostřednictvím tři middlewares a odpověď je aplikaci. Každý middleware běží svou logikou a předá požadavek na další middleware v next() příkaz. Po třetí middleware zpracuje požadavek, je předané zpět prostřednictvím předchozí dva middlewares pro další zpracování po next() příkazy každý zase před opuštěním aplikace jako odpověď klientovi.](middleware/_static/request-delegate-pipeline.png)

Každý delegát provádět operace před a po další delegáta. Delegát můžete také rozhodnout není předat požadavek na další delegáta, který se nazývá krátká smyčka kanál požadavku. Krátká smyčka je často žádoucí, protože při ní nedochází nepotřebné práci. Middleware se statickými soubory můžete například vrátit požadavek na statický soubor a krátká smyčka zbytek kanálu. Zpracování výjimek delegáti muset volat již v rané fázi v kanálu, takže se můžete zachytit výjimky, ke kterým došlo v novější fázemi kanálu.

Nejjednodušší možné aplikace ASP.NET Core nastaví delegáta jedné žádosti, která zpracovává všechny požadavky. Tento případ neobsahuje kanál skutečné požadavku. Místo toho jedné anonymní funkce je volána v reakci na každý požadavek HTTP.

[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]

První [aplikace. Spustit](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegáta ukončí kanálu.

Můžete řetězu více delegátů žádost společně s [aplikace. Použití](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions). `next` Parametr představuje další delegát v kanálu. (Mějte na paměti, že může krátká smyčka kanálu pomocí *není* volání *Další* parametru.) Můžete obvykle provádět akce před i po další delegáta, jak ukazuje tento příklad:

[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> Nevolejte `next.Invoke` po odeslání odpovědi klientovi. Změny `HttpResponse` po spuštění odpovědi vyvolá výjimku. Například změny, jako je třeba nastavení hlavičky, stavový kód atd., vyvolá výjimku. Zápis do text odpovědi po volání `next`:
> - Může způsobit narušení protokolu. Například zápis více než deklarovaným `content-length`.
> - Může dojít k poškození formátu textu. Například zápatí HTML zápis do souboru CSS.
>
> [HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) je užitečné nápovědu k označení, pokud byly odeslány hlavičky nebo text byl proveden zápis.

## <a name="ordering"></a>Řazení

Middleware součásti jsou přidány v pořadí `Configure` metoda definuje pořadí, ve kterém jsou vyvolány na požadavky a pro odpověď v obráceném pořadí. Toto pořadí je důležité pro zabezpečení, výkonu a funkce.

Metodu konfigurace (viz dole) přidá middleware následující součásti:

1. Zpracování výjimek/chyb
2. Statické souborového serveru
3. Ověřování
4. MVC

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

-----------

Ve výše, kódu `UseExceptionHandler` je první komponenta middlewaru přidané do kanálu – tedy zachytávalo jakékoli výjimky, které se vyskytují v pozdější volání.

Middleware se statickými soubory se nazývá již v rané fázi v kanálu, aby mohl zpracovávat požadavky a krátká smyčka – bez průchodu přes zbývající součásti. Poskytuje middleware se statickými soubory **žádné** kontroly autorizace. Všechny soubory obsluhovat, včetně těch na *wwwroot*, jsou veřejně dostupné. V tématu [práce s statické soubory](xref:fundamentals/static-files) pro přístup k zabezpečení statické soubory.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET základní 2.x](#tab/aspnetcore2x)


Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseAuthentication`), který provádí ověřování. Identita není krátká smyčka neověřené požadavky. I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní stránky Razor nebo kontroleru a akce.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET základní 1.x](#tab/aspnetcore1x)

Pokud požadavek není zpracováván middleware se statickými soubory, je předána Identity middleware (`app.UseIdentity`), který provádí ověřování. Identita není krátká smyčka neověřené požadavky. I když žádosti o ověření Identity autorizace (a odmítání) nastane pouze po MVC vybere konkrétní kontroleru a akce.

-----------

Následující příklad ukazuje, kde zpracovává požadavky pro statické soubory middleware se statickými soubory před middleware komprese odpovědi řazení middleware. Statické soubory nejsou komprimované s toto uspořádání middleware. Z odpovědi MVC [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) lze komprimovat.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a>Použití, spuštění a mapy

Můžete nakonfigurovat pomocí kanálu HTTP `Use`, `Run`, a `Map`. `Use` Metoda může krátká smyčka kanálu (tj. Pokud ho nevyvolá `next` delegáta požadavek). `Run`je konvence, a může vystavit některé komponenty middlewaru `Run[Middleware]` metody, které běží na konci kanálu.

`Map*`rozšíření jsou použity jako konvence pro vytvoření větve kanálu. [Mapa](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) větví kanálu požadavku podle odpovídá zadanou cestu požadavku. Pokud cesta požadavku začíná zadané cestě, je proveden větev.

[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]

V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:

| Požadavek | Odpověď |
| --- | --- |
| localhost:1234 | Hello z jiných mapy delegáta.  |
| localhost:1234/map1 | Mapa Test 1 |
| localhost:1234/map2 | Mapa testu 2 |
| localhost:1234/map3 | Hello z jiných mapy delegáta.  |

Když `Map` se používá, segment(s) odpovídající cesta se odeberou z `HttpRequest.Path` a připojením k `HttpRequest.PathBase` pro každý požadavek.

[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) větví kanál požadavku na základě výsledku daného predikátu. Všechny predikát typu `Func<HttpContext, bool>` lze použít k mapování požadavků na nové větve kanálu. V následujícím příkladu predikát slouží k detekci přítomnosti proměnné řetězce dotazu `branch`:

[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]

V následující tabulce jsou uvedeny požadavky a odpovědi z `http://localhost:1234` pomocí předchozí kód:

| Požadavek | Odpověď |
| --- | --- |
| localhost:1234 | Hello z jiných mapy delegáta.  |
| localhost:1234/?branch=master | Větev použít = hlavní|

`Map`podporuje vnoření, například:

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

`Map`Můžete také shoda s více segmenty najednou, například:

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a>Předdefinované middlewaru

ASP.NET Core se dodává s následující komponenty middlewaru, jakož i popis pořadí, ve kterém se má přidat:

| Middleware | Popis | Pořadí |
| ---------- | ----------- | ----- |
| [Ověřování](xref:security/authentication/identity) | Poskytuje podporu ověřování. | Před `HttpContext.User` je potřeba. Terminálu zpětná volání OAuth. |
| [CORS](xref:security/cors) | Konfiguruje sdílení prostředků různého původu. | Před provedením komponent, které používají CORS. |
| [Diagnostika](xref:fundamentals/error-handling) | Nakonfiguruje diagnostiky. | Před provedením komponent, které generují chyby. |
| [ForwardedHeaders/HttpOverrides](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | Předává směrovány přes proxy server hlavičky do aktuálního požadavku. | Před provedením komponent, které využívají aktualizovaná pole (příklady: schéma, hostitele, když, metoda). |
| [Ukládání odpovědí do mezipaměti](xref:performance/caching/middleware) | Poskytuje podporu pro ukládání do mezipaměti odpovědi. | Před provedením komponent, které vyžadují ukládání do mezipaměti. |
| [Komprese odpovědi](xref:performance/response-compression) | Poskytuje podporu pro kompresi odpovědí. | Před provedením komponent, které vyžadují komprese. |
| [RequestLocalization](xref:fundamentals/localization) | Poskytuje podporu lokalizace. | Před provedením komponent citlivé lokalizace. |
| [Směrování](xref:fundamentals/routing) | Definuje a omezí požadavek trasy. | Terminálu odpovídající trasy. |
| [Relace](xref:fundamentals/app-state) | Poskytuje podporu pro správu uživatelských relací. | Před provedením komponent, které vyžadují relace. |
| [Statické soubory](xref:fundamentals/static-files) | Poskytuje podporu pro obsluhující statické soubory a procházení adresářů. | Terminál, pokud požadavek odpovídá soubory. |
| [Přepisování adres URL](xref:fundamentals/url-rewriting) | Poskytuje podporu pro přepisování adres URL a přesměrování požadavků. | Před provedením komponent, které využívají adresu URL. |
| [Webové sokety](xref:fundamentals/websockets) | Umožňuje protokol Websocket. | Před provedením komponent, které jsou nutné k přijímání požadavků protokolu WebSocket. |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a>Zápis middlewaru

Middleware je obecně zapouzdřené v třídě a vystavené pomocí metody rozšíření. Vezměte v úvahu následující middlewaru, který nastaví jazykovou verzi pro aktuální požadavek z řetězce dotazu:

[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]

Poznámka: Výše uvedeném ukázkovém kódu se používá k předvedení vytváření komponenta middlewaru. V tématu [ globalizace a lokalizace](xref:fundamentals/localization) pro podporu předdefinované lokalizace ASP.NET Core.

Middleware můžete otestovat pomocí předání v jazykovou verzi, například `http://localhost:7997/?culture=no`.

Následující kód přesune delegáta middleware na třídu:

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]

Zpřístupní metodu rozšíření middleware prostřednictvím [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):

[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

Následující kód volá middleware z `Configure`:

[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

Postupujte podle middleware [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/) díky zpřístupnění jeho závislé součásti v jeho konstruktoru. Middleware je vytvořený jednou za *životního cyklu aplikace*. V tématu *požadavků závislosti* níže v případě, je potřeba sdílet s middlewaru v rámci žádost o služby.

Komponenty middlewaru lze vyřešit závislé z vkládání závislostí prostřednictvím parametrů konstruktor. [`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)Můžete také přijímat další parametry, přímo.

### <a name="per-request-dependencies"></a>Závislosti na žádost

Protože middlewaru je vytvořený při spuštění aplikace, není požadavků, *obor* služby životního cyklu, které middleware konstruktory používá nejsou sdílené s jinými typy vložit závislostí při každé žádosti. Pokud je nutné sdílet *obor* služby mezi vlastního middlewaru a jinými typy, přidejte tyto služby `Invoke` podpis metody. `Invoke` Metoda může přijímat další parametry, které jsou naplněny pomocí vkládání závislostí. Příklad:

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a>Prostředky

* [Ukázkový kód používá v této dokumentace](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [Migrace modulů HTTP k middlewaru.](../migration/http-modules.md)
* [Spuštění aplikace](startup.md)
* [Funkce požadavků](request-features.md)
