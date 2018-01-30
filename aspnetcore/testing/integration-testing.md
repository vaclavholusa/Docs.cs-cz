---
title: "Integrace testování v ASP.NET Core"
author: ardalis
description: "Jak používat ASP.NET Core integrace testování pro zajištění, že součásti aplikace fungovat správně."
manager: wpickett
ms.author: riande
ms.date: 09/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: testing/integration-testing
ms.openlocfilehash: ebae76da01e1b24466174179a9d4bbe826202cc3
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
# <a name="integration-testing-in-aspnet-core"></a>Integrace testování v ASP.NET Core

Podle [Steve Smith](https://ardalis.com/)

Testování integrace zajistí, že součásti aplikace fungovat správně při sestaví společně. Integrace podporuje ASP.NET Core testování pomocí systémů testování částí a integrované testovací webového hostitele, který lze použít ke zpracování požadavků bez nároky na síť.

[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Úvod do integrace testování

Integrace testy ověřte správné fungování společně různé části aplikace. Na rozdíl od [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integrace testy často zahrnují aspekty infrastruktury aplikace, jako je například databáze, systém souborů, síťové prostředky, nebo webové požadavky a odpovědi. Testování částí používat fakes nebo mock objektů místo tyto otázky, ale účelem testů integrace je potvrďte, že systém funguje podle očekávání s těmito systémy.

Integrace testy, protože využijí větší segmenty kódu a proto spoléhají na prvky infrastruktury jsou obvykle pořadí podle velikosti pomalejší než jednotka testy. Proto je vhodné omezit kolik testů integrace, že napíšete, zejména v případě, že stejné chování můžete otestovat s testování částí.

> [!NOTE]
> Pokud některé chování lze otestovat pomocí testování částí nebo test integrace, raději testování částí vzhledem k tomu, že je téměř vždy být rychlejší. Můžete mít desítek nebo stovky testování částí s mnoha různými vstupy, ale jen někteří z nich integrace testy pokrývajících nejdůležitější scénáře.

Testování logiky v rámci vlastní metody, je obvykle doménu testování částí. Testování, jak vaše aplikace funguje v rámci, například pomocí ASP.NET Core nebo s databází je kde integrace testy začalo play. Neberou v příliš mnoho integrace testů, abyste se ujistili, že dokážete zapsat řádek do databáze a přečtěte si ho zpátky. Nemusíte otestovat všechny možné Permutace přístupový kód dat – stačí pouze k testování dostatek získáte jistotu, zda aplikace pracuje správně.

## <a name="integration-testing-aspnet-core"></a>Integrace testování ASP.NET Core

Pokud chcete získat nastavena tak, aby integrace spuštění testů, budete potřebovat pro vytvoření projektu testů, přidejte odkaz na webový projekt ASP.NET Core a instalaci spuštění testu. Tento proces je popsán v [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentaci podrobnější pokyny ke spuštění testů a doporučení pro pojmenovávání testů a testovací třídy.

> [!NOTE]
> Jednotlivé testy částí a integrace testů pomocí různých projektů. To pomáhá zajistit, že nemáte zavedete omylem riziko z hlediska infrastruktury do testů jednotek a umožňuje snadno vybrat sadu testů ke spuštění.

### <a name="the-test-host"></a>Testovacího hostitele

ASP.NET Core zahrnuje hostitele test, který lze přidat do projektů testování integrace a používané k hostiteli ASP.NET Core aplikace, slouží test požadavky bez nutnosti skutečné webového hostitele. Zadaný vzorek zahrnuje integrace testovacího projektu, na který byl nakonfigurován na použití [xUnit](https://xunit.github.io) a otestovat hostitele. Použije `Microsoft.AspNetCore.TestHost` balíček NuGet.

Jednou `Microsoft.AspNetCore.TestHost` balíčku je zahrnutý v projektu, budete moct vytvořit a nakonfigurovat `TestServer` v testy. Následující test ukazuje, jak ověřit, že požadavku odeslaného do kořenového adresáře webu vrátí "Hello, World!" a je třeba provést úspěšně u výchozí šablonu ASP.NET Core prázdný Web vytvořili pomocí sady Visual Studio.

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Tento test je pomocí vzoru Assert Act uspořádání. Uspořádat krok se provádí v konstruktor, který vytvoří instanci `TestServer`. Nakonfigurované `WebHostBuilder` se použije k vytvoření `TestHost`; v tomto příkladu `Configure` metoda ze systému v rámci testovací (SUT) `Startup` předaný – třída `WebHostBuilder`. Tato metoda se použije ke konfiguraci kanálu žádosti o služby `TestServer` stejně jako na tom, jak by SUT server konfigurován.

V části Akce testu je vznesen požadavek `TestServer` instance "/" cesty, a odpověď je načtení zpět do řetězce. Tento řetězec je porovnat s očekávanou řetězec "Hello, World!". Pokud se shodují, test se předá; jinak dojde k chybě.

Teď můžete přidat několik dalších integrace testů, abyste se ujistili, že prime kontrola funkce fungovat prostřednictvím webové aplikace:

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Všimněte si, který se snažíte není skutečně otestovat správnost kontrolu prime číslo s tyto testy ale spíš webové aplikace je to, co očekávat. Už máte pokrytí test jednotky, poskytující spolehlivosti v `PrimeService`, jak je vidět tady:

![Průzkumník testů](integration-testing/_static/test-explorer.png)

Další informace o testování částí v [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) článku.


### <a name="integration-testing-mvcrazor"></a>Integrace testování Mvc nebo Razor

Vyžadovat projektů testů, které obsahují zobrazení syntaxe Razor `<PreserveCompilationContext>` nastavit na hodnotu true v *.csproj* souboru:


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

Projekty chybí tento element vygenerují chybu podobný následujícímu:
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Refaktoring používat middlewaru.

Refaktoring je proces změny kódu aplikace ke zlepšení návrh beze změny jeho chování. Je třeba jej provést v ideálním případě pokud je sada testů, předávání, protože tyto Nápověda zajistěte, aby že chování systému zůstává stejná před a po změně. Prohlížení způsob, ve kterém je implementováno prime Kontrola logiky ve webové aplikaci `Configure` metoda, se zobrazí:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

Tento kód funguje, ale je daleko od způsob implementace tento druh funkce v aplikaci ASP.NET Core i stejně jednoduché, protože tento. Představte si, co `Configure` metoda bude vypadat potřeby přidejte do ní tento mnohem kód pokaždé, když přidáte jiným koncovým bodem adresa URL!

Jednou z možností vzít v úvahu je přidání [MVC](xref:mvc/overview) aplikace a vytvoření řadiče pro zpracování prvotní kontrola. Ale za předpokladu, že aktuálně není třeba žádné další MVC funkce, která je chvíli overkill.

Můžete však využít výhod ASP.NET Core [middleware](xref:fundamentals/middleware), který vám pomůže nám zapouzdření prime kontrola logiku v její vlastní třída a dosáhnout lepší [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/) v `Configure` Metoda.

Chcete povolit cestu middleware použije zadat jako parametr, tak třída middleware očekává `RequestDelegate` a `PrimeCheckerOptions` instance v jeho konstruktoru. Pokud cesta požadavku se neshoduje se co tento middleware je nakonfigurován tak, aby se dalo očekávat, jednoduše volat další middleware v řetězci a žádná další akce. Zbytek implementace kód, který byl v `Configure` je teď ve `Invoke` metoda.

> [!NOTE]
> Vzhledem k tomu, že middleware závisí na `PrimeService` službu, kterou žádáte také instance této služby s konstruktorem. Rozhraní bude poskytovat této služby prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection), za předpokladu, že byl nakonfigurován, například v `ConfigureServices`.

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Vzhledem k tomu, že tento middleware funguje jako koncový bod v řetězu delegáta žádost, když odpovídá jeho cesty, je bez volání `_next.Invoke` když tento middleware zpracuje požadavek.

Pomocí tohoto middlewaru v místě a některé užitečné rozšiřující metody vytvoření usnadnění její konfiguraci, refactored `Configure` metoda vypadá takto:

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Následující tento refaktoring jste jisti, že webové aplikace stále funguje, jako předtím, protože integrace testů jsou všechny předávání.

> [!NOTE]
> Je vhodné po dokončení Refaktoring a předejte testy uložte provedené změny do správy zdrojového kódu. Pokud jste cvičení Test Driven Development [zvažte přidání potvrzení vaší Red zelená Refaktorovat cyklus](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Prostředky

* [Testování jednotek](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Middleware](xref:fundamentals/middleware)
* [Testování kontrolerů](xref:mvc/controllers/testing)
