---
title: Vytvoření služby back-end pro nativní mobilní aplikace pomocí ASP.NET Core
author: ardalis
description: Naučte se vytvářet back-end služby pomocí ASP.NET MVC jádra pro podporu nativních mobilní aplikace.
ms.author: riande
ms.date: 10/14/2016
uid: mobile/native-mobile-backend
ms.openlocfilehash: 27051cd3c4e2c3aa1ebf6d5510db4645651120e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276123"
---
# <a name="create-backend-services-for-native-mobile-apps-with-aspnet-core"></a><span data-ttu-id="631ed-103">Vytvoření služby back-end pro nativní mobilní aplikace pomocí ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="631ed-103">Create backend services for native mobile apps with ASP.NET Core</span></span>

<span data-ttu-id="631ed-104">Podle [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="631ed-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="631ed-105">Mobilní aplikace můžete snadno komunikovat se službami ASP.NET Core back-end.</span><span class="sxs-lookup"><span data-stu-id="631ed-105">Mobile apps can easily communicate with ASP.NET Core backend services.</span></span>

[<span data-ttu-id="631ed-106">Zobrazit nebo stáhnout ukázkový kód služby back-end</span><span class="sxs-lookup"><span data-stu-id="631ed-106">View or download sample backend services code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mobile/native-mobile-backend/sample)

## <a name="the-sample-native-mobile-app"></a><span data-ttu-id="631ed-107">Ukázka nativní mobilní aplikace</span><span class="sxs-lookup"><span data-stu-id="631ed-107">The Sample Native Mobile App</span></span>

<span data-ttu-id="631ed-108">Tento kurz ukazuje, jak vytvořit back-end služby pomocí ASP.NET MVC jádra pro podporu nativních mobilní aplikace.</span><span class="sxs-lookup"><span data-stu-id="631ed-108">This tutorial demonstrates how to create backend services using ASP.NET Core MVC to support native mobile apps.</span></span> <span data-ttu-id="631ed-109">Použije [aplikace Xamarin Forms ToDoRest](/xamarin/xamarin-forms/data-cloud/consuming/rest) jako nativní klient, který zahrnuje samostatné nativní klientů pro zařízení Android, iOS, univerzální pro Windows a Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="631ed-109">It uses the [Xamarin Forms ToDoRest app](/xamarin/xamarin-forms/data-cloud/consuming/rest) as its native client, which includes separate native clients for Android, iOS, Windows Universal, and Window Phone devices.</span></span> <span data-ttu-id="631ed-110">Vám může postupovat v kurzu propojené vytvoření nativní aplikaci (a instalace nástroje Xamarin nezbytné volné), stejně jako stažení ukázkové řešení Xamarin.</span><span class="sxs-lookup"><span data-stu-id="631ed-110">You can follow the linked tutorial to create the native app (and install the necessary free Xamarin tools), as well as download the Xamarin sample solution.</span></span> <span data-ttu-id="631ed-111">Ukázka Xamarin zahrnuje služby projektu ASP.NET Web API 2, které tento článek aplikace ASP.NET Core nahradí (bez nutnosti klientem změn).</span><span class="sxs-lookup"><span data-stu-id="631ed-111">The Xamarin sample includes an ASP.NET Web API 2 services project, which this article's ASP.NET Core app replaces (with no changes required by the client).</span></span>

![Pro aplikaci Rest se systémem Android smartphone](native-mobile-backend/_static/todo-android.png)

### <a name="features"></a><span data-ttu-id="631ed-113">Funkce</span><span class="sxs-lookup"><span data-stu-id="631ed-113">Features</span></span>

<span data-ttu-id="631ed-114">Aplikace ToDoRest podporuje výpis, přidávání, odstraňování a aktualizace položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="631ed-114">The ToDoRest app supports listing, adding, deleting, and updating To-Do items.</span></span> <span data-ttu-id="631ed-115">Každá položka má ID, název, poznámky a vlastnost, která určuje, zda byla dokončí ještě.</span><span class="sxs-lookup"><span data-stu-id="631ed-115">Each item has an ID, a Name, Notes, and a property indicating whether it's been Done yet.</span></span>

<span data-ttu-id="631ed-116">Hlavní zobrazení položek, jako v příkladu nahoře, uvádí název každé položky a označuje, pokud se provádí se zaškrtnutím.</span><span class="sxs-lookup"><span data-stu-id="631ed-116">The main view of the items, as shown above, lists each item's name and indicates if it's done with a checkmark.</span></span>

<span data-ttu-id="631ed-117">Klepnutím `+` ikona se otevře dialogové okno Přidat položky:</span><span class="sxs-lookup"><span data-stu-id="631ed-117">Tapping the `+` icon opens an add item dialog:</span></span>

![Přidat položku – dialogové okno](native-mobile-backend/_static/todo-android-new-item.png)

<span data-ttu-id="631ed-119">Klepnutím na obrazovce hlavní seznamu položku otevře upravit dialogové okno, kde můžete upravit název, poznámky a Done nastavení položky, položka lze nebo:</span><span class="sxs-lookup"><span data-stu-id="631ed-119">Tapping an item on the main list screen opens up an edit dialog where the item's Name, Notes, and Done settings can be modified, or the item can be deleted:</span></span>

![Položka dialogové okno Upravit](native-mobile-backend/_static/todo-android-edit-item.png)

<span data-ttu-id="631ed-121">Tato ukázka je nakonfigurované ve výchozím nastavení použít back-end službu hostovanou na developer.xamarin.com, umožňujících operacím jen pro čtení.</span><span class="sxs-lookup"><span data-stu-id="631ed-121">This sample is configured by default to use backend services hosted at developer.xamarin.com, which allow read-only operations.</span></span> <span data-ttu-id="631ed-122">K testování si sami proti aplikace ASP.NET Core vytvořená v další části běžícího na vašem počítači, budete muset aktualizovat aplikace `RestUrl` konstantní.</span><span class="sxs-lookup"><span data-stu-id="631ed-122">To test it out yourself against the ASP.NET Core app created in the next section running on your computer, you'll need to update the app's `RestUrl` constant.</span></span> <span data-ttu-id="631ed-123">Přejděte na `ToDoREST` projektu a otevřete *Constants.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="631ed-123">Navigate to the `ToDoREST` project and open the *Constants.cs* file.</span></span> <span data-ttu-id="631ed-124">Nahraďte `RestUrl` s adresou URL, která obsahuje váš počítač IP adres (ne localhost nebo 127.0.0.1, protože tato adresa se používá z zařízení emulátoru, není z vašeho počítače).</span><span class="sxs-lookup"><span data-stu-id="631ed-124">Replace the `RestUrl` with a URL that includes your machine's IP address (not localhost or 127.0.0.1, since this address is used from the device emulator, not from your machine).</span></span> <span data-ttu-id="631ed-125">Zahrnout také číslo portu (5000).</span><span class="sxs-lookup"><span data-stu-id="631ed-125">Include the port number as well (5000).</span></span> <span data-ttu-id="631ed-126">Chcete-li otestovat, že vaše služby fungovat s zařízení, ujistěte se, že nemáte aktivní brána firewall blokuje přístup na tento port.</span><span class="sxs-lookup"><span data-stu-id="631ed-126">In order to test that your services work with a device, ensure you don't have an active firewall blocking access to this port.</span></span>

```csharp
// URL of REST service (Xamarin ReadOnly Service)
//public static string RestUrl = "http://developer.xamarin.com:8081/api/todoitems{0}";

// use your machine's IP address
public static string RestUrl = "http://192.168.1.207:5000/api/todoitems/{0}";
```

## <a name="creating-the-aspnet-core-project"></a><span data-ttu-id="631ed-127">Vytvoření projektu ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="631ed-127">Creating the ASP.NET Core Project</span></span>

<span data-ttu-id="631ed-128">Vytvořte novou webovou aplikaci ASP.NET Core v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="631ed-128">Create a new ASP.NET Core Web Application in Visual Studio.</span></span> <span data-ttu-id="631ed-129">Zvolte šablonu webového rozhraní API a bez ověřování.</span><span class="sxs-lookup"><span data-stu-id="631ed-129">Choose the Web API template and No Authentication.</span></span> <span data-ttu-id="631ed-130">Název projektu *ToDoApi*.</span><span class="sxs-lookup"><span data-stu-id="631ed-130">Name the project *ToDoApi*.</span></span>

![Dialogové okno nové webové aplikace ASP.NET s vybraná šablona projektu webového rozhraní API](native-mobile-backend/_static/web-api-template.png)

<span data-ttu-id="631ed-132">Aplikace má odpovědět na všechny požadavky na port 5000.</span><span class="sxs-lookup"><span data-stu-id="631ed-132">The application should respond to all requests made to port 5000.</span></span> <span data-ttu-id="631ed-133">Aktualizace *Program.cs* zahrnout `.UseUrls("http://*:5000")` k dosažení tohoto cíle:</span><span class="sxs-lookup"><span data-stu-id="631ed-133">Update *Program.cs* to include `.UseUrls("http://*:5000")` to achieve this:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Program.cs?range=10-16&highlight=3)]

> [!NOTE]
> <span data-ttu-id="631ed-134">Zajistěte, aby že aplikaci spouštíte přímo, nikoli za služby IIS Express, které nejsou místní požadavky ve výchozím nastavení ignoruje.</span><span class="sxs-lookup"><span data-stu-id="631ed-134">Make sure you run the application directly, rather than behind IIS Express, which ignores non-local requests by default.</span></span> <span data-ttu-id="631ed-135">Spustit [dotnet spustit](/dotnet/core/tools/dotnet-run) z příkazového řádku, nebo vyberte název profilu aplikace z rozevíracího seznamu cíl ladění na panelu nástrojů Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="631ed-135">Run [dotnet run](/dotnet/core/tools/dotnet-run) from a command prompt, or choose the application name profile from the Debug Target dropdown in the Visual Studio toolbar.</span></span>

<span data-ttu-id="631ed-136">Přidejte třídu modelu představují položkami seznamu úkolů.</span><span class="sxs-lookup"><span data-stu-id="631ed-136">Add a model class to represent To-Do items.</span></span> <span data-ttu-id="631ed-137">Označit požadovaná pole pomocí `[Required]` atribut:</span><span class="sxs-lookup"><span data-stu-id="631ed-137">Mark required fields using the `[Required]` attribute:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Models/ToDoItem.cs)]

<span data-ttu-id="631ed-138">Metody rozhraní API vyžadují některé způsob, jak pracovat s daty.</span><span class="sxs-lookup"><span data-stu-id="631ed-138">The API methods require some way to work with data.</span></span> <span data-ttu-id="631ed-139">Použijte stejný `IToDoRepository` rozhraní původní Ukázka používá Xamarin:</span><span class="sxs-lookup"><span data-stu-id="631ed-139">Use the same `IToDoRepository` interface the original Xamarin sample uses:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Interfaces/IToDoRepository.cs)]

<span data-ttu-id="631ed-140">Tato ukázka implementace právě používá privátní kolekce položek:</span><span class="sxs-lookup"><span data-stu-id="631ed-140">For this sample, the implementation just uses a private collection of items:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Services/ToDoRepository.cs)]

<span data-ttu-id="631ed-141">Konfigurace v implementaci v *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="631ed-141">Configure the implementation in *Startup.cs*:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Startup.cs?highlight=6&range=29-35)]

<span data-ttu-id="631ed-142">V tomto okamžiku jste připraveni vytvořit *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="631ed-142">At this point, you're ready to create the *ToDoItemsController*.</span></span>

> [!TIP]
> <span data-ttu-id="631ed-143">Další informace o vytváření webovým rozhraním API v [vytvoření vašeho prvního rozhraní API webové s ASP.NET MVC jádra a sady Visual Studio](../tutorials/first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="631ed-143">Learn more about creating web APIs in [Build your first Web API with ASP.NET Core MVC and Visual Studio](../tutorials/first-web-api.md).</span></span>

## <a name="creating-the-controller"></a><span data-ttu-id="631ed-144">Vytvoření Kontroleru</span><span class="sxs-lookup"><span data-stu-id="631ed-144">Creating the Controller</span></span>

<span data-ttu-id="631ed-145">Přidejte nový řadič do projektu, *ToDoItemsController*.</span><span class="sxs-lookup"><span data-stu-id="631ed-145">Add a new controller to the project, *ToDoItemsController*.</span></span> <span data-ttu-id="631ed-146">Z Microsoft.AspNetCore.Mvc.Controller musí dědit.</span><span class="sxs-lookup"><span data-stu-id="631ed-146">It should inherit from Microsoft.AspNetCore.Mvc.Controller.</span></span> <span data-ttu-id="631ed-147">Přidat `Route` atribut k označení, že řadičem bude zpracovávat požadavky na cesty počínaje `api/todoitems`.</span><span class="sxs-lookup"><span data-stu-id="631ed-147">Add a `Route` attribute to indicate that the controller will handle requests made to paths starting with `api/todoitems`.</span></span> <span data-ttu-id="631ed-148">`[controller]` Token v trasy, která je nahrazena název kontroleru (vynechání `Controller` příponu) a je obzvláště užitečné pro globální trasy.</span><span class="sxs-lookup"><span data-stu-id="631ed-148">The `[controller]` token in the route is replaced by the name of the controller (omitting the `Controller` suffix), and is especially helpful for global routes.</span></span> <span data-ttu-id="631ed-149">Další informace o [směrování](../fundamentals/routing.md).</span><span class="sxs-lookup"><span data-stu-id="631ed-149">Learn more about [routing](../fundamentals/routing.md).</span></span>

<span data-ttu-id="631ed-150">Vyžaduje kontroleru `IToDoRepository` do funkce; požadavku instance tohoto typu pomocí konstruktoru kontroleru.</span><span class="sxs-lookup"><span data-stu-id="631ed-150">The controller requires an `IToDoRepository` to function; request an instance of this type through the controller's constructor.</span></span> <span data-ttu-id="631ed-151">Za běhu, tato instance bude poskytnuta pomocí rozhraní framework podporu pro [vkládání závislostí](../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="631ed-151">At runtime, this instance will be provided using the framework's support for [dependency injection](../fundamentals/dependency-injection.md).</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=1-17&highlight=9,14)]

<span data-ttu-id="631ed-152">Toto rozhraní API podporuje čtyři jiné příkazy HTTP k provádění operací CRUD (vytvořit, číst, Update, Delete) na datovém zdroji.</span><span class="sxs-lookup"><span data-stu-id="631ed-152">This API supports four different HTTP verbs to perform CRUD (Create, Read, Update, Delete) operations on the data source.</span></span> <span data-ttu-id="631ed-153">Nejjednodušší z nich je operace čtení, který odpovídá na požadavek HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="631ed-153">The simplest of these is the Read operation, which corresponds to an HTTP GET request.</span></span>

### <a name="reading-items"></a><span data-ttu-id="631ed-154">Čtení položek</span><span class="sxs-lookup"><span data-stu-id="631ed-154">Reading Items</span></span>

<span data-ttu-id="631ed-155">Požadavku na seznam položek provádí pomocí požadavek GET na `List` metoda.</span><span class="sxs-lookup"><span data-stu-id="631ed-155">Requesting a list of items is done with a GET request to the `List` method.</span></span> <span data-ttu-id="631ed-156">`[HttpGet]` Atributu u `List` metoda určuje, že tato akce pouze zpracování požadavků GET.</span><span class="sxs-lookup"><span data-stu-id="631ed-156">The `[HttpGet]` attribute on the `List` method indicates that this action should only handle GET requests.</span></span> <span data-ttu-id="631ed-157">Trasy pro tuto akci je trasy zadaný na řadiči.</span><span class="sxs-lookup"><span data-stu-id="631ed-157">The route for this action is the route specified on the controller.</span></span> <span data-ttu-id="631ed-158">Nepotřebujete nutně použití název akce v rámci trasy.</span><span class="sxs-lookup"><span data-stu-id="631ed-158">You don't necessarily need to use the action name as part of the route.</span></span> <span data-ttu-id="631ed-159">Potřebujete zajistěte, aby měla každá akce jedinečný a jednoznačné trasy.</span><span class="sxs-lookup"><span data-stu-id="631ed-159">You just need to ensure each action has a unique and unambiguous route.</span></span> <span data-ttu-id="631ed-160">Atributy směrování lze použít v kontroleru a úrovně metody vybudovat konkrétní trasy.</span><span class="sxs-lookup"><span data-stu-id="631ed-160">Routing attributes can be applied at both the controller and method levels to build up specific routes.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=19-23)]

<span data-ttu-id="631ed-161">`List` Metoda vrátí kód odpovědi 200 OK a všechny položky ToDo serializovanou jako JSON.</span><span class="sxs-lookup"><span data-stu-id="631ed-161">The `List` method returns a 200 OK response code and all of the ToDo items, serialized as JSON.</span></span>

<span data-ttu-id="631ed-162">Můžete otestovat způsob nové rozhraní API pomocí různých nástrojů, jako třeba [Postman](https://www.getpostman.com/docs/), znázorněno zde:</span><span class="sxs-lookup"><span data-stu-id="631ed-162">You can test your new API method using a variety of tools, such as [Postman](https://www.getpostman.com/docs/), shown here:</span></span>

![Postman konzola znázorňující požadavek GET pro todoitems a text odpovědi zobrazující JSON pro vráceny tři položky.](native-mobile-backend/_static/postman-get.png)

### <a name="creating-items"></a><span data-ttu-id="631ed-164">Vytváření položek</span><span class="sxs-lookup"><span data-stu-id="631ed-164">Creating Items</span></span>

<span data-ttu-id="631ed-165">Podle konvence vytváření nových položek dat je namapována na příkazu HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="631ed-165">By convention, creating new data items is mapped to the HTTP POST verb.</span></span> <span data-ttu-id="631ed-166">`Create` Metoda má `[HttpPost]` atribut použije na ni a přijímá `ToDoItem` instance.</span><span class="sxs-lookup"><span data-stu-id="631ed-166">The `Create` method has an `[HttpPost]` attribute applied to it, and accepts a `ToDoItem` instance.</span></span> <span data-ttu-id="631ed-167">Vzhledem k tomu, `item` argument budou předány v textu v příspěvku, tento parametr je upraven pomocí `[FromBody]` atribut.</span><span class="sxs-lookup"><span data-stu-id="631ed-167">Since the `item` argument will be passed in the body of the POST, this parameter is decorated with the `[FromBody]` attribute.</span></span>

<span data-ttu-id="631ed-168">Uvnitř metody položka je kontrola platnosti a předchozí existence v úložišti dat a pokud dojde k žádné problémy, se přidá pomocí úložiště.</span><span class="sxs-lookup"><span data-stu-id="631ed-168">Inside the method, the item is checked for validity and prior existence in the data store, and if no issues occur, it's added using the repository.</span></span> <span data-ttu-id="631ed-169">Kontrola `ModelState.IsValid` provede [modelu ověření](../mvc/models/validation.md)a by mělo být provedeno v každé metody rozhraní API, která podporuje vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="631ed-169">Checking `ModelState.IsValid` performs [model validation](../mvc/models/validation.md), and should be done in every API method that accepts user input.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=25-46)]

<span data-ttu-id="631ed-170">Ukázka používá výčet obsahující kódy chyb, které se předávají do mobilního klienta:</span><span class="sxs-lookup"><span data-stu-id="631ed-170">The sample uses an enum containing error codes that are passed to the mobile client:</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=91-99)]

<span data-ttu-id="631ed-171">Otestovat, přidávání nových položek pomocí Postman tak, že zvolíte příkaz POST poskytuje nový objekt ve formátu JSON v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="631ed-171">Test adding new items using Postman by choosing the POST verb providing the new object in JSON format in the Body of the request.</span></span> <span data-ttu-id="631ed-172">Měli byste také přidat zadání hlavičky požadavku `Content-Type` z `application/json`.</span><span class="sxs-lookup"><span data-stu-id="631ed-172">You should also add a request header specifying a `Content-Type` of `application/json`.</span></span>

![Postman konzola znázorňující POST a odpovědi](native-mobile-backend/_static/postman-post.png)

<span data-ttu-id="631ed-174">Metoda vrátí nově vytvořenou položku v odpovědi.</span><span class="sxs-lookup"><span data-stu-id="631ed-174">The method returns the newly created item in the response.</span></span>

### <a name="updating-items"></a><span data-ttu-id="631ed-175">Aktualizace položek</span><span class="sxs-lookup"><span data-stu-id="631ed-175">Updating Items</span></span>

<span data-ttu-id="631ed-176">Úpravy záznamů se provádí pomocí požadavků HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="631ed-176">Modifying records is done using HTTP PUT requests.</span></span> <span data-ttu-id="631ed-177">Než tuto změnu `Edit` metoda je téměř shodné s `Create`.</span><span class="sxs-lookup"><span data-stu-id="631ed-177">Other than this change, the `Edit` method is almost identical to `Create`.</span></span> <span data-ttu-id="631ed-178">Všimněte si, že pokud se nenajde záznamu, `Edit` akce vrátí `NotFound` odpovědi (404).</span><span class="sxs-lookup"><span data-stu-id="631ed-178">Note that if the record isn't found, the `Edit` action will return a `NotFound` (404) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=48-69)]

<span data-ttu-id="631ed-179">Testování s Postman, změňte příkaz na PUT.</span><span class="sxs-lookup"><span data-stu-id="631ed-179">To test with Postman, change the verb to PUT.</span></span> <span data-ttu-id="631ed-180">Určete data, aktualizovaného objektu v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="631ed-180">Specify the updated object data in the Body of the request.</span></span>

![Postman konzola znázorňující PUT a odpovědi](native-mobile-backend/_static/postman-put.png)

<span data-ttu-id="631ed-182">Tato metoda vrátí hodnotu `NoContent` (204) odpovědi, při úspěšné, z důvodu konzistence s existující rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="631ed-182">This method returns a `NoContent` (204) response when successful, for consistency with the pre-existing API.</span></span>

### <a name="deleting-items"></a><span data-ttu-id="631ed-183">Odstraňování položek</span><span class="sxs-lookup"><span data-stu-id="631ed-183">Deleting Items</span></span>

<span data-ttu-id="631ed-184">Odstraňování záznamů dosahuje tím, že žádosti o odstranění ke službě a předávání ID položky pro odstranění.</span><span class="sxs-lookup"><span data-stu-id="631ed-184">Deleting records is accomplished by making DELETE requests to the service, and passing the ID of the item to be deleted.</span></span> <span data-ttu-id="631ed-185">Jako s aktualizacemi, se zobrazí požadavky pro položky, které neexistují `NotFound` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="631ed-185">As with updates, requests for items that don't exist will receive `NotFound` responses.</span></span> <span data-ttu-id="631ed-186">Jinak bude úspěšné žádosti získat `NoContent` (204) odpovědi.</span><span class="sxs-lookup"><span data-stu-id="631ed-186">Otherwise, a successful request will get a `NoContent` (204) response.</span></span>

[!code-csharp[](native-mobile-backend/sample/ToDoApi/src/ToDoApi/Controllers/ToDoItemsController.cs?range=71-88)]

<span data-ttu-id="631ed-187">Při testování funkci odstranění, nic je vyžadována v textu požadavku.</span><span class="sxs-lookup"><span data-stu-id="631ed-187">Note that when testing the delete functionality, nothing is required in the Body of the request.</span></span>

![Postman konzola znázorňující DELETE a odpovědi](native-mobile-backend/_static/postman-delete.png)

## <a name="common-web-api-conventions"></a><span data-ttu-id="631ed-189">Běžné konvence webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="631ed-189">Common Web API Conventions</span></span>

<span data-ttu-id="631ed-190">Když budete vyvíjet služby back-end pro vaši aplikaci, můžete spolu s konzistentní sadu názvů nebo zásady pro nakládání s mezi vyjímání otázky.</span><span class="sxs-lookup"><span data-stu-id="631ed-190">As you develop the backend services for your app, you will want to come up with a consistent set of conventions or policies for handling cross-cutting concerns.</span></span> <span data-ttu-id="631ed-191">Například ve výše uvedeném službě požadavky pro konkrétní záznamů, které nebyly nalezeny přijatých `NotFound` odpověď, a ne `BadRequest` odpovědi.</span><span class="sxs-lookup"><span data-stu-id="631ed-191">For example, in the service shown above, requests for specific records that weren't found received a `NotFound` response, rather than a `BadRequest` response.</span></span> <span data-ttu-id="631ed-192">Podobně příkazy provedené v této služby, která předaná vázána k modelu typy vždy zaškrtnuto `ModelState.IsValid` a vrátí `BadRequest` pro typy neplatný model.</span><span class="sxs-lookup"><span data-stu-id="631ed-192">Similarly, commands made to this service that passed in model bound types always checked `ModelState.IsValid` and returned a `BadRequest` for invalid model types.</span></span>

<span data-ttu-id="631ed-193">Jakmile jste zformulovali běžné zásady pro vaše rozhraní API, můžete obvykle zapouzdřit v [filtru](../mvc/controllers/filters.md).</span><span class="sxs-lookup"><span data-stu-id="631ed-193">Once you've identified a common policy for your APIs, you can usually encapsulate it in a [filter](../mvc/controllers/filters.md).</span></span> <span data-ttu-id="631ed-194">Další informace o [jak zapouzdřit běžné zásady rozhraní API do aplikací ASP.NET MVC základní](https://msdn.microsoft.com/magazine/mt767699.aspx).</span><span class="sxs-lookup"><span data-stu-id="631ed-194">Learn more about [how to encapsulate common API policies in ASP.NET Core MVC applications](https://msdn.microsoft.com/magazine/mt767699.aspx).</span></span>
