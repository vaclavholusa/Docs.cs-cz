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
ms.openlocfilehash: 8c28f1b4f66433eaebd9e474e784ecf3f1ac271b
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/02/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="0a6f8-103">Integrace testování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a6f8-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="0a6f8-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="0a6f8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="0a6f8-105">Testování integrace zajistí, že součásti aplikace fungovat správně při sestaví společně.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="0a6f8-106">Integrace podporuje ASP.NET Core testování pomocí systémů testování částí a integrované testovací webového hostitele, který lze použít ke zpracování požadavků bez nároky na síť.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="0a6f8-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0a6f8-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="0a6f8-108">Úvod do integrace testování</span><span class="sxs-lookup"><span data-stu-id="0a6f8-108">Introduction to integration testing</span></span>

<span data-ttu-id="0a6f8-109">Integrace testy ověřte správné fungování společně různé části aplikace.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="0a6f8-110">Na rozdíl od [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integrace testy často zahrnují aspekty infrastruktury aplikace, jako je například databáze, systém souborů, síťové prostředky, nebo webové požadavky a odpovědi.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="0a6f8-111">Testování částí používat fakes nebo mock objektů místo tyto otázky, ale účelem testů integrace je potvrďte, že systém funguje podle očekávání s těmito systémy.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="0a6f8-112">Integrace testy, protože využijí větší segmenty kódu a proto spoléhají na prvky infrastruktury jsou obvykle pořadí podle velikosti pomalejší než jednotka testy.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="0a6f8-113">Proto je vhodné omezit kolik testů integrace, že napíšete, zejména v případě, že stejné chování můžete otestovat s testování částí.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="0a6f8-114">Pokud některé chování lze otestovat pomocí testování částí nebo test integrace, raději testování částí vzhledem k tomu, že je téměř vždy být rychlejší.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="0a6f8-115">Můžete mít desítek nebo stovky testování částí s mnoha různými vstupy, ale jen někteří z nich integrace testy pokrývajících nejdůležitější scénáře.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="0a6f8-116">Testování logiky v rámci vlastní metody, je obvykle doménu testování částí.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="0a6f8-117">Testování, jak vaše aplikace funguje v rámci, například pomocí ASP.NET Core nebo s databází je kde integrace testy začalo play.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="0a6f8-118">Neberou v příliš mnoho integrace testů, abyste se ujistili, že dokážete zapsat řádek do databáze a přečtěte si ho zpátky.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="0a6f8-119">Nemusíte otestovat všechny možné Permutace přístupový kód dat – stačí pouze k testování dostatek získáte jistotu, zda aplikace pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="0a6f8-120">Integrace testování ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0a6f8-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="0a6f8-121">Pokud chcete získat nastavena tak, aby integrace spuštění testů, budete potřebovat pro vytvoření projektu testů, přidejte odkaz na webový projekt ASP.NET Core a instalaci spuštění testu.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="0a6f8-122">Tento proces je popsán v [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) dokumentaci podrobnější pokyny ke spuštění testů a doporučení pro pojmenovávání testů a testovací třídy.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="0a6f8-123">Jednotlivé testy částí a integrace testů pomocí různých projektů.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="0a6f8-124">To pomáhá zajistit, že nemáte zavedete omylem riziko z hlediska infrastruktury do testů jednotek a umožňuje snadno vybrat sadu testů ke spuštění.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="0a6f8-125">Testovacího hostitele</span><span class="sxs-lookup"><span data-stu-id="0a6f8-125">The Test Host</span></span>

<span data-ttu-id="0a6f8-126">ASP.NET Core zahrnuje hostitele test, který lze přidat do projektů testování integrace a používané k hostiteli ASP.NET Core aplikace, slouží test požadavky bez nutnosti skutečné webového hostitele.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="0a6f8-127">Zadaný vzorek zahrnuje integrace testovacího projektu, na který byl nakonfigurován na použití [xUnit](https://xunit.github.io) a otestovat hostitele.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="0a6f8-128">Použije `Microsoft.AspNetCore.TestHost` balíček NuGet.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="0a6f8-129">Jednou `Microsoft.AspNetCore.TestHost` balíčku je zahrnutý v projektu, budete moct vytvořit a nakonfigurovat `TestServer` v testy.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="0a6f8-130">Následující test ukazuje, jak ověřit, že požadavku odeslaného do kořenového adresáře webu vrátí "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="0a6f8-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="0a6f8-131">a je třeba provést úspěšně u výchozí šablonu ASP.NET Core prázdný Web vytvořili pomocí sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="0a6f8-132">Tento test je pomocí vzoru Assert Act uspořádání.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="0a6f8-133">Uspořádat krok se provádí v konstruktor, který vytvoří instanci `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="0a6f8-134">Nakonfigurované `WebHostBuilder` se použije k vytvoření `TestHost`; v tomto příkladu `Configure` metoda ze systému v rámci testovací (SUT) `Startup` předaný – třída `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="0a6f8-135">Tato metoda se použije ke konfiguraci kanálu žádosti o služby `TestServer` stejně jako na tom, jak by SUT server konfigurován.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="0a6f8-136">V části Akce testu je vznesen požadavek `TestServer` instance "/" cesty, a odpověď je načtení zpět do řetězce.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="0a6f8-137">Tento řetězec je porovnat s očekávanou řetězec "Hello, World!".</span><span class="sxs-lookup"><span data-stu-id="0a6f8-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="0a6f8-138">Pokud se shodují, test se předá; jinak dojde k chybě.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="0a6f8-139">Teď můžete přidat několik dalších integrace testů, abyste se ujistili, že prime kontrola funkce fungovat prostřednictvím webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="0a6f8-140">Všimněte si, který se snažíte není skutečně otestovat správnost kontrolu prime číslo s tyto testy ale spíš webové aplikace je to, co očekávat.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="0a6f8-141">Už máte pokrytí test jednotky, poskytující spolehlivosti v `PrimeService`, jak je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Průzkumník testů](integration-testing/_static/test-explorer.png)

<span data-ttu-id="0a6f8-143">Další informace o testování částí v [testování částí](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) článku.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="0a6f8-144">Integrace testování Mvc nebo Razor</span><span class="sxs-lookup"><span data-stu-id="0a6f8-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="0a6f8-145">Vyžadovat projektů testů, které obsahují zobrazení syntaxe Razor `<PreserveCompilationContext>` nastavit na hodnotu true v *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="0a6f8-146">Projekty chybí tento element vygenerují chybu podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="0a6f8-147">Refaktoring používat middlewaru.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-147">Refactoring to use middleware</span></span>

<span data-ttu-id="0a6f8-148">Refaktoring je proces změny kódu aplikace ke zlepšení návrh beze změny jeho chování.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="0a6f8-149">Je třeba jej provést v ideálním případě pokud je sada testů, předávání, protože tyto Nápověda zajistěte, aby že chování systému zůstává stejná před a po změně.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-149">It should ideally be done when there's a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="0a6f8-150">Prohlížení způsob, ve kterém je implementováno prime Kontrola logiky ve webové aplikaci `Configure` metoda, se zobrazí:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

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

<span data-ttu-id="0a6f8-151">Tento kód funguje, ale je daleko od způsob implementace tento druh funkce v aplikaci ASP.NET Core i stejně jednoduché, protože tento.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="0a6f8-152">Představte si, co `Configure` metoda bude vypadat potřeby přidejte do ní tento mnohem kód pokaždé, když přidáte jiným koncovým bodem adresa URL!</span><span class="sxs-lookup"><span data-stu-id="0a6f8-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="0a6f8-153">Jednou z možností vzít v úvahu je přidání [MVC](xref:mvc/overview) aplikace a vytvoření řadiče pro zpracování prvotní kontrola.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="0a6f8-154">Ale za předpokladu, že aktuálně není třeba žádné další MVC funkce, která je chvíli overkill.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="0a6f8-155">Můžete však využít výhod ASP.NET Core [middleware](xref:fundamentals/middleware/index), který vám pomůže nám zapouzdření prime kontrola logiku v její vlastní třída a dosáhnout lepší [oddělené oblasti zájmu](http://deviq.com/separation-of-concerns/) v `Configure` Metoda.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware/index), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="0a6f8-156">Chcete povolit cestu middleware použije zadat jako parametr, tak třída middleware očekává `RequestDelegate` a `PrimeCheckerOptions` instance v jeho konstruktoru.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="0a6f8-157">Pokud cesta požadavku se neshoduje se co tento middleware je nakonfigurován tak, aby se dalo očekávat, jednoduše volat další middleware v řetězci a žádná další akce.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="0a6f8-158">Zbytek implementace kód, který byl v `Configure` je teď ve `Invoke` metoda.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="0a6f8-159">Vzhledem k tomu, že middleware závisí na `PrimeService` službu, kterou žádáte také instance této služby s konstruktorem.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="0a6f8-160">Rozhraní bude poskytovat této služby prostřednictvím [vkládání závislostí](xref:fundamentals/dependency-injection), za předpokladu, že byl nakonfigurován, například v `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="0a6f8-161">Vzhledem k tomu, že tento middleware funguje jako koncový bod v řetězu delegáta žádost, když odpovídá jeho cesty, je bez volání `_next.Invoke` když tento middleware zpracuje požadavek.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there's no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="0a6f8-162">Pomocí tohoto middlewaru v místě a některé užitečné rozšiřující metody vytvoření usnadnění její konfiguraci, refactored `Configure` metoda vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="0a6f8-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="0a6f8-163">Následující tento refaktoring jste jisti, že webové aplikace stále funguje, jako předtím, protože integrace testů jsou všechny předávání.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="0a6f8-164">Je vhodné po dokončení Refaktoring a předejte testy uložte provedené změny do správy zdrojového kódu.</span><span class="sxs-lookup"><span data-stu-id="0a6f8-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="0a6f8-165">Pokud jste cvičení Test Driven Development [zvažte přidání potvrzení vaší Red zelená Refaktorovat cyklus](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="0a6f8-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="0a6f8-166">Prostředky</span><span class="sxs-lookup"><span data-stu-id="0a6f8-166">Resources</span></span>

* [<span data-ttu-id="0a6f8-167">Testování jednotek</span><span class="sxs-lookup"><span data-stu-id="0a6f8-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="0a6f8-168">Middleware</span><span class="sxs-lookup"><span data-stu-id="0a6f8-168">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="0a6f8-169">Testování kontrolerů</span><span class="sxs-lookup"><span data-stu-id="0a6f8-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
