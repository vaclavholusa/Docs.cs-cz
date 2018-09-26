---
title: Vytvoření back-endových služeb pro nativní mobilní aplikace pomocí ASP.NET Core
author: ardalis
description: Zjistěte, jak vytvořit back-endových služeb pro podporu nativních mobilních aplikací pomocí ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 3ebd30ad1ffbd66b256e7f3954a07d682f76a754
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230175"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="73501-103">Vytvoření back-endových služeb pro nativní mobilní aplikace pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73501-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="73501-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="73501-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="73501-105">Mobilní aplikace můžete snadno komunikovat s back-endových služeb ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="73501-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="73501-106">Zobrazení nebo stažení ukázkového kódu služby back-endu</span><span class="sxs-lookup"><span data-stu-id="73501-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="73501-107">Ukázka nativní mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="73501-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="73501-108">Tento kurz ukazuje, jak vytvořit back-endových služeb pro podporu nativních mobilních aplikací pomocí ASP.NET Core MVC.</span><span class="sxs-lookup"><span data-stu-id="73501-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="73501-109">Používá [aplikace Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako jeho nativního klienta, který obsahuje samostatné nativní klienty pro zařízení s Androidem, iOS, Windows Universal a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="73501-109">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="73501-110">Vám může propojené kurzu a vytvořte nativní aplikaci (a nainstalujte potřebné bezplatné nástroje Xamarin), jakož i stáhnout ukázkové řešení Xamarin.</span><span class="sxs-lookup"><span data-stu-id="73501-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="73501-111">Ukázka Xamarin zahrnuje služby projektu aplikace ASP.NET Web API 2, nahradí aplikace ASP.NET Core v tomto článku (se nezmění klientů).</span><span class="sxs-lookup"><span data-stu-id="73501-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Proveďte zbývající aplikaci běžící v Androidu chytrém telefonu](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="73501-113">Funkce</span><span class="sxs-lookup"><span data-stu-id="73501-113">Features</span></span>

<span data-ttu-id="73501-114">Aplikace ToDoRest podporuje výpis, přidávání, odstraňování a aktualizace položek úkolů.</span><span class="sxs-lookup"><span data-stu-id="73501-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="73501-115">Každá položka má ID, název, poznámky a vlastnost určující, jestli se to se dělá ještě.</span><span class="sxs-lookup"><span data-stu-id="73501-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="73501-116">Hlavní zobrazení položek, jak je uvedeno výše, uvádí název každé položky a označuje, pokud se provádí pomocí značka zaškrtnutí.</span><span class="sxs-lookup"><span data-stu-id="73501-116">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="73501-117">Klepnutím `+` ikonu otevře dialogové okno Přidat položky:</span><span class="sxs-lookup"><span data-stu-id="73501-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Přidat položky dialogového okna](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="73501-119">Klepnutím na položku na obrazovce hlavní seznam otevře úpravy dialogové okno, kde název položky, poznámky a Hotovo nastavení můžete upravit, nebo můžete odstranit položky:</span><span class="sxs-lookup"><span data-stu-id="73501-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Upravit položky dialogového okna](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="73501-121">Tato ukázka je nakonfigurovaná ve výchozím nastavení používat back-endových služeb hostovaných developer.xamarin.com, které umožňují operacím jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="73501-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="73501-122">K otestování sami proti aplikace ASP.NET Core vytvořená v další části v počítači spuštěný, bude nutné aktualizovat aplikace `RestUrl` konstantní.</span><span class="sxs-lookup"><span data-stu-id="73501-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="73501-123">Přejděte `ToDoREST` projektu a otevřete *Constants.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="73501-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="73501-124">Nahradit `RestUrl` pomocí adresy URL, která obsahuje váš počítač IP adresa (ne localhost nebo 127.0.0.1, protože tato adresa se používá z emulátor zařízení, ne ze svého počítače).</span><span class="sxs-lookup"><span data-stu-id="73501-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="73501-125">Zahrnují číslo portu (5 000).</span><span class="sxs-lookup"><span data-stu-id="73501-125">Include the port number as well (5000).</span></span> <span data-ttu-id="73501-126">Pokud chcete otestovat, jestli vaše služby fungovat s zařízení, ujistěte se, že nemáte aktivní brána firewall blokuje přístup na tento port.</span><span class="sxs-lookup"><span data-stu-id="73501-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="73501-127">Vytváří se projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="73501-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="73501-128">Vytvořte novou webovou aplikaci ASP.NET Core v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73501-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="73501-129">Zvolte šablonu webového rozhraní API a bez ověřování.</span><span class="sxs-lookup"><span data-stu-id="73501-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="73501-130">Pojmenujte projekt *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="73501-130">Name the project *ToDoApi*.</span></span>

![Dialogové okno nové webové aplikace ASP.NET máte zvolenou šablonu projektu webového rozhraní API](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="73501-132">Aplikace by měl odpovědět na všechny žádosti na portu 5000.</span><span class="sxs-lookup"><span data-stu-id="73501-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="73501-133">Aktualizace *Program.cs* zahrnout `.UseUrls("http://*:5000")` dosáhnout:</span><span class="sxs-lookup"><span data-stu-id="73501-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="73501-134">Zajistěte, aby že přímo, namísto za službu IIS Express, která ve výchozím nastavení ignoruje požadavky na jiné než místní spuštění aplikace.</span><span class="sxs-lookup"><span data-stu-id="73501-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="73501-135">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) z příkazového řádku, nebo zvolte název profilu aplikace v rozevíracím seznamu cíl ladění na panelu nástrojů sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73501-135">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="73501-136">Přidejte třídu modelu k reprezentaci položek úkolů.</span><span class="sxs-lookup"><span data-stu-id="73501-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="73501-137">Označit požadovaných polí pomocí `[Required]` atribut:</span><span class="sxs-lookup"><span data-stu-id="73501-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="73501-138">Metody rozhraní API vyžadují způsob, jak pracovat s daty.</span><span class="sxs-lookup"><span data-stu-id="73501-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="73501-139">Použijte stejný `IToDoRepository` rozhraní původní Ukázka používá Xamarin:</span><span class="sxs-lookup"><span data-stu-id="73501-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="73501-140">V tomto příkladu používá implementaci soukromé kolekce položek:</span><span class="sxs-lookup"><span data-stu-id="73501-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="73501-141">Konfigurace implementace v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="73501-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="73501-142">V tuto chvíli jste připraveni vytvořit *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="73501-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="73501-143">Další informace o vytváření webových rozhraní API v [sestavení první webové rozhraní API pomocí ASP.NET Core MVC a sady Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="73501-143">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="73501-144">Vytvoření Kontroleru</span><span class="sxs-lookup"><span data-stu-id="73501-144">Creating the Controller</span></span>

<span data-ttu-id="73501-145">Přidat nový kontroler do projektu, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="73501-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="73501-146">By měla dědit z Microsoft.AspNetCore.Mvc.Controller.</span><span class="sxs-lookup"><span data-stu-id="73501-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="73501-147">Přidat `Route` atribut označuje, že kontroler bude zpracovávat požadavky na cesta začínající řetězcem `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="73501-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="73501-148">`[controller]` Token v této trase je nahrazena název kontroleru (vynechání `Controller` přípony) a je zvláště užitečné pro globální trasy.</span><span class="sxs-lookup"><span data-stu-id="73501-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="73501-149">Další informace o [směrování](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="73501-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="73501-150">Kontroler vyžaduje `IToDoRepository` na funkci; požádat o instance tohoto typu pomocí konstruktoru kontroleru.</span><span class="sxs-lookup"><span data-stu-id="73501-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="73501-151">Za běhu, tuto instanci vám poskytneme pomocí podpory rozhraní framework pro [injektáž závislostí](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="73501-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="73501-152">Toto rozhraní API podporuje čtyři různé příkazy HTTP k provádění operací CRUD (vytváření, čtení, Update, Delete) ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="73501-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="73501-153">Nejjednodušším z nich je operace čtení, které odpovídá požadavku HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="73501-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="73501-154">Čtení položky</span><span class="sxs-lookup"><span data-stu-id="73501-154">Reading Items</span></span>

<span data-ttu-id="73501-155">Probíhá vyžádání seznamu položek se provádí požadavek GET na `List` metody.</span><span class="sxs-lookup"><span data-stu-id="73501-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="73501-156">`[HttpGet]` Atribut na `List` metoda označuje, že tato akce jenom zpracování požadavků GET.</span><span class="sxs-lookup"><span data-stu-id="73501-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="73501-157">Trasy pro tuto akci je Zadaná trasa na kontroleru.</span><span class="sxs-lookup"><span data-stu-id="73501-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="73501-158">Není nutné nutně používat název akce jako součást trasy.</span><span class="sxs-lookup"><span data-stu-id="73501-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="73501-159">Stačí zajistit, že každá akce má jedinečný a jednoznačným trasy.</span><span class="sxs-lookup"><span data-stu-id="73501-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="73501-160">Směrování atributy lze použít na kontroleru a úrovně metody Vybudujte konkrétní trasy.</span><span class="sxs-lookup"><span data-stu-id="73501-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="73501-161">`List` Metoda vrátí kód odpovědi 200 OK a všech položek ToDo serializovanou jako JSON.</span><span class="sxs-lookup"><span data-stu-id="73501-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="73501-162">Můžete otestovat vaši novou metodu rozhraní API pomocí různých nástrojů, jako například [Postman](https://www.getpostman.com/docs/), je vidět tady:</span><span class="sxs-lookup"><span data-stu-id="73501-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Postman konzola znázorňující požadavek GET pro todoitems a text odpovědi zobrazují ve formátu JSON pro tři vrácených položek](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="73501-164">Vytvoření položky</span><span class="sxs-lookup"><span data-stu-id="73501-164">Creating Items</span></span>

<span data-ttu-id="73501-165">Podle konvence vytváření nové datové položky se mapuje na operaci HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="73501-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="73501-166">`Create` Metoda má `[HttpPost]` atribut použit a přijímá `ToDoItem` instance.</span><span class="sxs-lookup"><span data-stu-id="73501-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="73501-167">Protože `item` argument se předá v text příspěvku, tento parametr je upravená pomocí `[FromBody]` atribut.</span><span class="sxs-lookup"><span data-stu-id="73501-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="73501-168">Uvnitř metody je položka zaškrtnuta pro platnosti a předchozí existence v úložišti dat, a pokud dojde k žádné problémy, se přidá pomocí úložiště.</span><span class="sxs-lookup"><span data-stu-id="73501-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="73501-169">Kontrola `ModelState.IsValid` provádí [ověření modelu](../mvc/models/validation.md)a by mělo být provedeno v každé rozhraní API metody, která přijímá vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="73501-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="73501-170">Ukázka používá výčet s kódy chyb, které jsou předány do mobilního klienta:</span><span class="sxs-lookup"><span data-stu-id="73501-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="73501-171">Testování, přidání nové položky pomocí nástroje Postman výběrem operací POST poskytuje nový objekt ve formátu JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="73501-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="73501-172">Měli byste také přidat zadáním hlavičky požadavku `Content-Type` z `application/json`.</span><span class="sxs-lookup"><span data-stu-id="73501-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Postman konzola znázorňující POST a odpovědi](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="73501-174">Metoda vrátí nově vytvořenou položku v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73501-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="73501-175">Aktualizace položky</span><span class="sxs-lookup"><span data-stu-id="73501-175">Updating Items</span></span>

<span data-ttu-id="73501-176">Úprava záznamů se provádí pomocí žádosti HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="73501-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="73501-177">Než tuto změnu `Edit` metoda je téměř shodné s `Create`.</span><span class="sxs-lookup"><span data-stu-id="73501-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="73501-178">Všimněte si, že pokud není nalezen záznam, `Edit` akce vrátí `NotFound` odpovědi (404).</span><span class="sxs-lookup"><span data-stu-id="73501-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="73501-179">Testování pomocí nástroje Postman, změňte operaci Put.</span><span class="sxs-lookup"><span data-stu-id="73501-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="73501-180">Zadejte data aktualizovaného objektu do těla požadavku.</span><span class="sxs-lookup"><span data-stu-id="73501-180">Specify the updated object data in the Body of the request.</span></span>

![Postman konzola znázorňující PUT a odpovědi](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="73501-182">Tato metoda vrátí hodnotu `NoContent` (204) reakci v případě úspěchu pro zajištění konzistence s již existující rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="73501-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="73501-183">Odstranění položek</span><span class="sxs-lookup"><span data-stu-id="73501-183">Deleting Items</span></span>

<span data-ttu-id="73501-184">Odstranění záznamů se dosahuje tak provedení žádosti o odstranění služby a při předávání ID položky, která se má odstranit.</span><span class="sxs-lookup"><span data-stu-id="73501-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="73501-185">Jako s aktualizacemi, bude přijímat žádosti pro položky, které neexistují `NotFound` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73501-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="73501-186">V opačném případě se zobrazí úspěšné žádosti `NoContent` (204) odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73501-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="73501-187">Mějte na paměti, že při testování funkce odstraňování, nic se vyžaduje v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="73501-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Postman konzola znázorňující DELETE a odpovědi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="73501-189">Běžné konvence webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="73501-189">Common Web API Conventions</span></span>

<span data-ttu-id="73501-190">Při vývoji back-endových služeb pro vaši aplikaci, můžete přijít s konzistentní sadu vytváření názvů nebo zásady pro nakládání s vyskytující aspekty.</span><span class="sxs-lookup"><span data-stu-id="73501-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="73501-191">Například ve službě výše uvedené požadavky na konkrétních záznamů, které nebyly nalezeny přijatých `NotFound` odpovědi, spíše než `BadRequest` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="73501-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="73501-192">Podobně, příkazy na tuto službu, které předáno vázána k modelu typy vždy zaškrtnuto `ModelState.IsValid` a vrátí `BadRequest` pro typy modelu je neplatný.</span><span class="sxs-lookup"><span data-stu-id="73501-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="73501-193">Jakmile identifikujete běžné zásady pro vaše rozhraní API, můžete obvykle zapouzdřit ho [filtr](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="73501-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="73501-194">Další informace o [jak zapouzdřit běžných zásad rozhraní API aplikace ASP.NET Core MVC](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="73501-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73501-195">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="73501-195">Additional resources</span></span>

* [<span data-ttu-id="73501-196">Ověřování a autorizace</span><span class="sxs-lookup"><span data-stu-id="73501-196">Authentication and Authorization</span></span>](/xamarin/xamarin-forms/enterprise-application-patterns/authentication-and-authorization)
