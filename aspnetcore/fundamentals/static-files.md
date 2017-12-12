---
title: "Práce s statické soubory v ASP.NET Core"
author: rick-anderson
description: "Naučte se pracovat s statické soubory v ASP.NET Core."
keywords: "ASP.NET Core, statické soubory, statické prostředky, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="aa573-104">Práce s statické soubory v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa573-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="aa573-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="aa573-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="aa573-106">Statické soubory, jako je například HTML, CSS, image a JavaScript, jsou prostředky, které může aplikace ASP.NET Core poskytují přímo klientům.</span><span class="sxs-lookup"><span data-stu-id="aa573-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="aa573-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa573-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="aa573-108">Zpracování statických souborů.</span><span class="sxs-lookup"><span data-stu-id="aa573-108">Serving static files</span></span>

<span data-ttu-id="aa573-109">Statické soubory jsou obvykle umístěny v `web root` (*\<obsah kořenové > / wwwroot*) složky.</span><span class="sxs-lookup"><span data-stu-id="aa573-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="aa573-110">V tématu [obsahu kořenové](xref:fundamentals/index#content-root) a [kořenový Web](xref:fundamentals/index#web-root) Další informace.</span><span class="sxs-lookup"><span data-stu-id="aa573-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="aa573-111">Obecně nastavíte kořenu obsahu na aktuálním adresáři tak, aby váš projekt `web root` budou nacházet v vývoj.</span><span class="sxs-lookup"><span data-stu-id="aa573-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="aa573-112">Statické soubory mohou být uloženy v libovolné složky pod `web root` a přístupu k nim s relativní cestou tohoto kořenového adresáře.</span><span class="sxs-lookup"><span data-stu-id="aa573-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="aa573-113">Například při vytváření projektu výchozí webové aplikace pomocí sady Visual Studio existuje několik složek v rámci vytvořili *wwwroot* složky - *šablon stylů css*, *bitové kopie*, a *js*.</span><span class="sxs-lookup"><span data-stu-id="aa573-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="aa573-114">Identifikátor URI pro přístup k obrazu v *bitové kopie* podsložky:</span><span class="sxs-lookup"><span data-stu-id="aa573-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="aa573-115">Aby mohl obsluhovat statické soubory, musíte nakonfigurovat [Middleware](middleware.md) přidat statické soubory do kanálu.</span><span class="sxs-lookup"><span data-stu-id="aa573-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="aa573-116">Middleware se statickými soubory lze nastavit tak, že přidáte závislost na *Microsoft.AspNetCore.StaticFiles* balíčku do projektu a pak volání `UseStaticFiles` metoda rozšíření z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="aa573-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="aa573-117">`app.UseStaticFiles();`vytvoří soubory v `web root` (*wwwroot* ve výchozím nastavení) servable.</span><span class="sxs-lookup"><span data-stu-id="aa573-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="aa573-118">Později zobrazí jak provádět další obsah adresáře servable s `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="aa573-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="aa573-119">Musí zahrnovat balíček NuGet "Microsoft.AspNetCore.StaticFiles".</span><span class="sxs-lookup"><span data-stu-id="aa573-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="aa573-120">`web root`použije se výchozí hodnota *wwwroot* adresáře, ale můžete nastavit `web root` adresáři s `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="aa573-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="aa573-121">Předpokládejme, že máte projekt hierarchii, kde jsou statické soubory, které chcete poskytovat mimo `web root`.</span><span class="sxs-lookup"><span data-stu-id="aa573-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="aa573-122">Příklad:</span><span class="sxs-lookup"><span data-stu-id="aa573-122">For example:</span></span>

* <span data-ttu-id="aa573-123">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="aa573-123">wwwroot</span></span>
  * <span data-ttu-id="aa573-124">šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="aa573-124">css</span></span>
  * <span data-ttu-id="aa573-125">obrázky</span><span class="sxs-lookup"><span data-stu-id="aa573-125">images</span></span>
  * <span data-ttu-id="aa573-126">...</span><span class="sxs-lookup"><span data-stu-id="aa573-126">...</span></span>
* <span data-ttu-id="aa573-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="aa573-127">MyStaticFiles</span></span>
  * <span data-ttu-id="aa573-128">test.PNG</span><span class="sxs-lookup"><span data-stu-id="aa573-128">test.png</span></span>

<span data-ttu-id="aa573-129">Pro žádost o přístup k *test.png*, nakonfigurujte middleware statické soubory takto:</span><span class="sxs-lookup"><span data-stu-id="aa573-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="aa573-130">Požadavek na `http://<app>/StaticFiles/test.png` bude sloužit *test.png* souboru.</span><span class="sxs-lookup"><span data-stu-id="aa573-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="aa573-131">`StaticFileOptions()`můžete nastavit hlavičky odpovědi.</span><span class="sxs-lookup"><span data-stu-id="aa573-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="aa573-132">Například následující kód nastaví z obsluhu statických souborů *wwwroot* složku a nastaví `Cache-Control` záhlaví, aby byly veřejně lze uložit do mezipaměti po dobu 10 minut (600 sekund):</span><span class="sxs-lookup"><span data-stu-id="aa573-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="aa573-133">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda má k dispozici [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="aa573-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="aa573-134">Přidat `using Microsoft.AspNetCore.Http;` k vaší *csharp* soubor, pokud je metoda není k dispozici.</span><span class="sxs-lookup"><span data-stu-id="aa573-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Byla přidána hlavičky odpovědi zobrazující hlavička Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="aa573-136">Statický soubor autorizace</span><span class="sxs-lookup"><span data-stu-id="aa573-136">Static file authorization</span></span>

<span data-ttu-id="aa573-137">Modul statických souborů poskytuje **žádné** kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="aa573-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="aa573-138">Všechny soubory obsluhovat, včetně těch na *wwwroot* jsou veřejně dostupné.</span><span class="sxs-lookup"><span data-stu-id="aa573-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="aa573-139">Do mají soubory podle autorizace:</span><span class="sxs-lookup"><span data-stu-id="aa573-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="aa573-140">Uchováváte mimo *wwwroot* a libovolného adresáře, které jsou přístupné pro middleware se statickými soubory **a**</span><span class="sxs-lookup"><span data-stu-id="aa573-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="aa573-141">Je poskytovat prostřednictvím akce kontroleru, vrácení `FileResult` v případě použití autorizace</span><span class="sxs-lookup"><span data-stu-id="aa573-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="aa573-142">Povolit procházení adresářů</span><span class="sxs-lookup"><span data-stu-id="aa573-142">Enabling directory browsing</span></span>

<span data-ttu-id="aa573-143">Procházení adresářů umožňuje uživatelům vaší webové aplikace najdete v seznamu adresářů a souborů v rámci zadaného adresáře.</span><span class="sxs-lookup"><span data-stu-id="aa573-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="aa573-144">Procházení adresářů je ve výchozím nastavení z bezpečnostních důvodů zakázán (viz [aspekty](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="aa573-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="aa573-145">Chcete-li povolit procházení adresářů, zavolejte `UseDirectoryBrowser` metoda rozšíření z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="aa573-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="aa573-146">A přidat požadované služby voláním `AddDirectoryBrowser` metoda rozšíření z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="aa573-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="aa573-147">Výše uvedený kód umožňuje z procházení adresářů *wwwroot nebo obrázky* složku pomocí adresu URL http://\<aplikace > / MyImages s odkazy na všechny soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="aa573-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![procházení adresářů](static-files/_static/dir-browse.png)

<span data-ttu-id="aa573-149">V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení.</span><span class="sxs-lookup"><span data-stu-id="aa573-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="aa573-150">Poznamenejte si dva `app.UseStaticFiles` volání.</span><span class="sxs-lookup"><span data-stu-id="aa573-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="aa573-151">První z nich je potřeba poskytovat šablon stylů CSS, bitové kopie a JavaScript v *wwwroot* složku a druhé volání pro procházení adresářů z *wwwroot nebo obrázky* složku pomocí adresu URL http://\<aplikace > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="aa573-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="aa573-152">Obsluhující výchozí dokument</span><span class="sxs-lookup"><span data-stu-id="aa573-152">Serving a default document</span></span>

<span data-ttu-id="aa573-153">Nastavení výchozí domovskou stránku poskytuje místo, kde můžete spustit při návštěvě webu návštěvníky.</span><span class="sxs-lookup"><span data-stu-id="aa573-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="aa573-154">V pořadí pro webovou aplikaci, která bude sloužit výchozí stránku aniž uživatel k plnému určení identifikátor URI, zavolejte `UseDefaultFiles` metoda rozšíření z `Startup.Configure` následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="aa573-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="aa573-155">`UseDefaultFiles`musí být volána před provedením `UseStaticFiles` k obsluze výchozí soubor.</span><span class="sxs-lookup"><span data-stu-id="aa573-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="aa573-156">`UseDefaultFiles`je adresa URL opakované zapisovač, který není ve skutečnosti tento soubor zpracovat.</span><span class="sxs-lookup"><span data-stu-id="aa573-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="aa573-157">Je nutné povolit middleware se statickými soubory (`UseStaticFiles`) pro tento soubor zpracovat.</span><span class="sxs-lookup"><span data-stu-id="aa573-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="aa573-158">S `UseDefaultFiles`, bude hledat požadavky do složky:</span><span class="sxs-lookup"><span data-stu-id="aa573-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="aa573-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="aa573-159">default.htm</span></span>
* <span data-ttu-id="aa573-160">default.HTML</span><span class="sxs-lookup"><span data-stu-id="aa573-160">default.html</span></span>
* <span data-ttu-id="aa573-161">index.htm</span><span class="sxs-lookup"><span data-stu-id="aa573-161">index.htm</span></span>
* <span data-ttu-id="aa573-162">index.HTML</span><span class="sxs-lookup"><span data-stu-id="aa573-162">index.html</span></span>

<span data-ttu-id="aa573-163">První soubor najde v seznamu se zpracuje, jako kdyby požadavek nebyl plně kvalifikovaného identifikátoru URI (i když adresu URL prohlížeče budou nadále zobrazit požadovaný identifikátor URI).</span><span class="sxs-lookup"><span data-stu-id="aa573-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="aa573-164">Následující kód ukazuje, jak změnit výchozí název souboru do *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="aa573-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="aa573-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="aa573-165">UseFileServer</span></span>

<span data-ttu-id="aa573-166">`UseFileServer`kombinuje funkce `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="aa573-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="aa573-167">Následující kód umožňuje statické soubory a výchozí soubor ke zpracování, ale není povoleno procházení adresářů:</span><span class="sxs-lookup"><span data-stu-id="aa573-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="aa573-168">Následující kód umožňuje statické soubory, výchozí soubory a procházení adresářů:</span><span class="sxs-lookup"><span data-stu-id="aa573-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="aa573-169">V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení.</span><span class="sxs-lookup"><span data-stu-id="aa573-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="aa573-170">Stejně jako u `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`, pokud chcete poskytovat souborů, které existují mimo `web root`, můžete vytvořit a nakonfigurovat `FileServerOptions` objekt, který můžete předat jako parametr pro `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="aa573-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="aa573-171">Například uděleno u následující hierarchie directory ve vaší webové aplikaci:</span><span class="sxs-lookup"><span data-stu-id="aa573-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="aa573-172">Wwwroot</span><span class="sxs-lookup"><span data-stu-id="aa573-172">wwwroot</span></span>

  * <span data-ttu-id="aa573-173">šablon stylů CSS</span><span class="sxs-lookup"><span data-stu-id="aa573-173">css</span></span>

  * <span data-ttu-id="aa573-174">obrázky</span><span class="sxs-lookup"><span data-stu-id="aa573-174">images</span></span>

  * <span data-ttu-id="aa573-175">...</span><span class="sxs-lookup"><span data-stu-id="aa573-175">...</span></span>

* <span data-ttu-id="aa573-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="aa573-176">MyStaticFiles</span></span>

  * <span data-ttu-id="aa573-177">test.PNG</span><span class="sxs-lookup"><span data-stu-id="aa573-177">test.png</span></span>

  * <span data-ttu-id="aa573-178">default.HTML</span><span class="sxs-lookup"><span data-stu-id="aa573-178">default.html</span></span>

<span data-ttu-id="aa573-179">Pomocí výše uvedený příklad hierarchie, můžete chtít povolit statické soubory, výchozí soubory a procházení `MyStaticFiles` adresáře.</span><span class="sxs-lookup"><span data-stu-id="aa573-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="aa573-180">V následující fragment kódu, který se provádí na základě jednoho volání do `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="aa573-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="aa573-181">Pokud `enableDirectoryBrowsing` je nastaven na `true` je nutné volat `AddDirectoryBrowser` metoda rozšíření z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="aa573-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="aa573-182">Pomocí souboru hierarchie a výše uvedený kód:</span><span class="sxs-lookup"><span data-stu-id="aa573-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="aa573-183">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="aa573-183">URI</span></span>            |                             <span data-ttu-id="aa573-184">Odpověď</span><span class="sxs-lookup"><span data-stu-id="aa573-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="aa573-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="aa573-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="aa573-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="aa573-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="aa573-187">Pokud žádná výchozí hodnota s názvem soubory *MyStaticFiles* adresář, http://\<aplikace > / StaticFiles vrátí seznam s odkazy, můžete kliknout adresářů:</span><span class="sxs-lookup"><span data-stu-id="aa573-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Seznam statické soubory](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="aa573-189">`UseDefaultFiles`a `UseDirectoryBrowser` bude trvat adresu url http://\<aplikace > / StaticFiles bez koncové lomítko a příčina straně klienta přesměrovat na http://\<aplikace > /StaticFiles/ (přidání do adresy koncové lomítko).</span><span class="sxs-lookup"><span data-stu-id="aa573-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="aa573-190">Bez koncové lomítko relativní adresy URL v rámci dokumenty by být nesprávné.</span><span class="sxs-lookup"><span data-stu-id="aa573-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="aa573-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="aa573-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="aa573-192">`FileExtensionContentTypeProvider` Třída obsahuje kolekci, která mapuje přípony souborů obsahu typy MIME.</span><span class="sxs-lookup"><span data-stu-id="aa573-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="aa573-193">V následující ukázce několik přípony souborů, které jsou registrovány k známé typy MIME, budou nahrazeny ".rtf" a ".mp4" se odebere.</span><span class="sxs-lookup"><span data-stu-id="aa573-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="aa573-194">V tématu [obsahu typy MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="aa573-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="aa573-195">Nestandardní typy obsahu</span><span class="sxs-lookup"><span data-stu-id="aa573-195">Non-standard content types</span></span>

<span data-ttu-id="aa573-196">Middleware se statickými soubory technologii ASP.NET rozumí téměř 400 typy obsahu známému souboru.</span><span class="sxs-lookup"><span data-stu-id="aa573-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="aa573-197">Pokud uživatel požaduje soubor neznámý typ souborů, middleware se statickými soubory vrátí odpověď HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="aa573-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="aa573-198">Pokud je povoleno procházení adresářů, zobrazí se odkaz na soubor, ale identifikátor URI vrátí chybu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="aa573-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="aa573-199">Následující kód umožňuje obsluhující neznámé typy a vykreslí Neznámý soubor jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="aa573-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="aa573-200">Kód výše bude vrácen žádost o soubor s Neznámý typ obsahu jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="aa573-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="aa573-201">Povolení `ServeUnknownFileTypes` představuje bezpečnostní riziko a jeho použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="aa573-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="aa573-202">`FileExtensionContentTypeProvider`(vysvětlené dřív) poskytuje bezpečnější alternativu k obsluhovat soubory s nestandardní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="aa573-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="aa573-203">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="aa573-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="aa573-204">`UseDirectoryBrowser`a `UseStaticFiles` může se nevrací tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="aa573-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="aa573-205">Doporučujeme vám **není** v produkčním prostředí pro procházení adresářů povolte.</span><span class="sxs-lookup"><span data-stu-id="aa573-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="aa573-206">Dávejte pozor, o které adresáře povolíte s `UseStaticFiles` nebo `UseDirectoryBrowser` jako celý adresář a všechny podadresářích budou přístupné.</span><span class="sxs-lookup"><span data-stu-id="aa573-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="aa573-207">Doporučujeme zachovat veřejnému obsahu v její vlastní adresář, jako  *\<obsahu kořenové > / wwwroot*, mimo zobrazení aplikací, konfigurační soubory, atd.</span><span class="sxs-lookup"><span data-stu-id="aa573-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="aa573-208">Adresy URL pro obsah vystaveny s `UseDirectoryBrowser` a `UseStaticFiles` podléhají malá a velká písmena a znak omezení jejich podkladový systém souborů.</span><span class="sxs-lookup"><span data-stu-id="aa573-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="aa573-209">Například systému Windows je malá a velká písmena, ale nejsou Mac a Linux.</span><span class="sxs-lookup"><span data-stu-id="aa573-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="aa573-210">Aplikace ASP.NET Core hostované ve službě IIS použít modul ASP.NET Core předávat všechny požadavky na aplikaci, včetně žádostí pro statické soubory.</span><span class="sxs-lookup"><span data-stu-id="aa573-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="aa573-211">Obslužné rutiny statických souborů služby IIS není použít, protože ho nepodporuje získat možnost zpracování požadavků, než jsou zpracovávány modul ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa573-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="aa573-212">Postup odebrání obslužné rutiny statických souborů služby IIS (na úrovni serveru nebo webu):</span><span class="sxs-lookup"><span data-stu-id="aa573-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="aa573-213">Přejděte na **moduly** funkce</span><span class="sxs-lookup"><span data-stu-id="aa573-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="aa573-214">Vyberte **iisver** v seznamu</span><span class="sxs-lookup"><span data-stu-id="aa573-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="aa573-215">Klepněte na **odebrat** v **akce** bočním panelu</span><span class="sxs-lookup"><span data-stu-id="aa573-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="aa573-216">Pokud je povoleno obslužné rutiny statických souborů služby IIS **a** ASP.NET Core modulu (ANCM) není správně nakonfigurována (například pokud *web.config* nebyla nasazena), se zpracuje statické soubory.</span><span class="sxs-lookup"><span data-stu-id="aa573-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="aa573-217">Soubory kódu (včetně c# a Razor) musí být umístěny mimo projekt aplikace `web root` (*wwwroot* ve výchozím nastavení).</span><span class="sxs-lookup"><span data-stu-id="aa573-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="aa573-218">Tím se vytvoří čisté oddělení mezi vaší aplikace obsah straně klienta a serveru straně zdrojový kód, který brání úniku kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="aa573-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="aa573-219">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="aa573-219">Additional Resources</span></span>

* [<span data-ttu-id="aa573-220">Middleware</span><span class="sxs-lookup"><span data-stu-id="aa573-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="aa573-221">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa573-221">Introduction to ASP.NET Core</span></span>](../index.md)
