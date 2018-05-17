---
title: Statické soubory v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak a zabezpečení statické soubory a nakonfigurovat statický soubor, který je hostitelem chování middlewaru ve webové aplikaci ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: f0d34b5b64235d136f7df1b3ffdbb9fb10eca316
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/12/2018
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="3bfd4-103">Statické soubory v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bfd4-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="3bfd4-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT) a [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="3bfd4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="3bfd4-105">Statické soubory, jako je například HTML, CSS, Image a JavaScript, jsou prostředky, které aplikace ASP.NET Core slouží přímo na klienty.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="3bfd4-106">Některé konfigurace je potřeba povolit ke zpracování těchto souborů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="3bfd4-107">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3bfd4-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="3bfd4-108">Poskytovat statické soubory</span><span class="sxs-lookup"><span data-stu-id="3bfd4-108">Serve static files</span></span>

<span data-ttu-id="3bfd4-109">Statické soubory jsou uloženy v kořenovém adresáři projektu na web.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="3bfd4-110">Výchozí adresář je  *\<content_root > / wwwroot*, ale může být změněna prostřednictvím [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) metoda.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="3bfd4-111">V tématu [obsahu kořenové](xref:fundamentals/index#content-root) a [kořenový Web](xref:fundamentals/index#web-root) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="3bfd4-112">Hostitel webové aplikace musí být upozorněni obsahu kořenového adresáře.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bfd4-113">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bfd4-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="3bfd4-114">`WebHost.CreateDefaultBuilder` Metoda nastaví kořenu obsahu k aktuálnímu adresáři:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bfd4-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bfd4-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="3bfd4-116">Nastavit kořenu obsahu k aktuálnímu adresáři vyvoláním [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) uvnitř `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="3bfd4-117">Statické soubory jsou přístupné prostřednictvím cesta relativní vůči kořenovému adresáři web.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="3bfd4-118">Například **webové aplikace** šablona projektu obsahuje několik složek v rámci *wwwroot* složky:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="3bfd4-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-119">**wwwroot**</span></span>
  * <span data-ttu-id="3bfd4-120">**šablon stylů CSS**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-120">**css**</span></span>
  * <span data-ttu-id="3bfd4-121">**Bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-121">**images**</span></span>
  * <span data-ttu-id="3bfd4-122">**js**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-122">**js**</span></span>

<span data-ttu-id="3bfd4-123">Pro přístup k souboru ve formátu URI *bitové kopie* je podsložka *http://\<server_address > /images/\<image_file_name >*.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="3bfd4-124">Například *http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="3bfd4-125">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="3bfd4-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="3bfd4-126">Pokud cílení na rozhraní .NET Framework, přidejte [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="3bfd4-127">Pokud cílení na .NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) zahrnuje tento balíček.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="3bfd4-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="3bfd4-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="3bfd4-129">Přidat [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) balíčku do projektu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="3bfd4-130">Konfigurace [middleware](xref:fundamentals/middleware/index) který povolí zpracování statických souborů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-130">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="3bfd4-131">Soubory uvnitř kořenový web poskytovat</span><span class="sxs-lookup"><span data-stu-id="3bfd4-131">Serve files inside of web root</span></span>

<span data-ttu-id="3bfd4-132">Vyvolání [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metoda v rámci `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="3bfd4-133">Bezparametrový `UseStaticFiles` přetížení metody označí soubory v kořenové webové jako servable.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="3bfd4-134">Následující odkazy značek *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="3bfd4-135">Soubory mimo kořenový web poskytovat</span><span class="sxs-lookup"><span data-stu-id="3bfd4-135">Serve files outside of web root</span></span>

<span data-ttu-id="3bfd4-136">Vezměte v úvahu hierarchie adresářů, ve kterém obsluhovat statické soubory nacházejí mimo kořenový web:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="3bfd4-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-137">**wwwroot**</span></span>
  * <span data-ttu-id="3bfd4-138">**šablon stylů CSS**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-138">**css**</span></span>
  * <span data-ttu-id="3bfd4-139">**Bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-139">**images**</span></span>
  * <span data-ttu-id="3bfd4-140">**js**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-140">**js**</span></span>
* <span data-ttu-id="3bfd4-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="3bfd4-142">**Bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-142">**images**</span></span>
      * <span data-ttu-id="3bfd4-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-143">*banner1.svg*</span></span>

<span data-ttu-id="3bfd4-144">Žádost o přístup *banner1.svg* souboru nakonfigurováním middleware se statickými soubory:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="3bfd4-145">V předchozí kód *MyStaticFiles* hierarchie adresářů je vystaven veřejně prostřednictvím *StaticFiles* segment identifikátoru URI.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="3bfd4-146">Požadavek na *http://\<server_address > /StaticFiles/images/banner1.svg* slouží *banner1.svg* souboru.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="3bfd4-147">Následující odkazy značek *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="3bfd4-148">Nastavit hlavičky HTTP odpovědi</span><span class="sxs-lookup"><span data-stu-id="3bfd4-148">Set HTTP response headers</span></span>

<span data-ttu-id="3bfd4-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objekt můžete použít k nastavení hlavičky HTTP odpovědi.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="3bfd4-150">Kromě konfigurace statických souborů slouží z kořenový web, následující kód nastaví `Cache-Control` hlavičky:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="3bfd4-151">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) metoda nachází [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) balíčku.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="3bfd4-152">Soubory byly provedeny veřejně lze uložit do mezipaměti po dobu 10 minut (600 sekund):</span><span class="sxs-lookup"><span data-stu-id="3bfd4-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Byla přidána hlavičky odpovědi zobrazující hlavička Cache-Control](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="3bfd4-154">Statický soubor autorizace</span><span class="sxs-lookup"><span data-stu-id="3bfd4-154">Static file authorization</span></span>

<span data-ttu-id="3bfd4-155">Middleware se statickými soubory neposkytuje kontroly autorizace.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="3bfd4-156">Všechny soubory obsluhovat, včetně těch na *wwwroot*, jsou veřejně přístupné.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="3bfd4-157">Do mají soubory podle autorizace:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="3bfd4-158">Uchováváte mimo *wwwroot* a libovolného adresáře, které jsou přístupné pro middleware se statickými soubory **a**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="3bfd4-159">Sloužit je prostřednictvím metody akce, ke které se použije autorizace.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="3bfd4-160">Vrátí [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objektu:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="3bfd4-161">Povolit procházení adresářů</span><span class="sxs-lookup"><span data-stu-id="3bfd4-161">Enable directory browsing</span></span>

<span data-ttu-id="3bfd4-162">Procházení adresářů umožňuje uživatelům vaší webové aplikace najdete v seznamu adresářů a souborů v rámci zadaného adresáře.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="3bfd4-163">Procházení adresářů je ve výchozím nastavení z bezpečnostních důvodů zakázán (viz [aspekty](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="3bfd4-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="3bfd4-164">Povolit procházení vyvoláním adresářů [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) metoda v `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="3bfd4-165">Přidat požadované služby vyvoláním [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) metoda z `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="3bfd4-166">Předchozí kód umožňuje z procházení adresářů *wwwroot nebo obrázky* složky pomocí adresy URL *http://\<server_address > / MyImages*, s odkazy na všechny soubory a složky:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![procházení adresářů](static-files/_static/dir-browse.png)

<span data-ttu-id="3bfd4-168">V tématu [aspekty](#considerations) na bezpečnostní rizika při povolování procházení.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="3bfd4-169">Poznamenejte si dva `UseStaticFiles` volá v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="3bfd4-170">Povolí zpracování statických souborů v první volání *wwwroot* složky.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="3bfd4-171">Druhé volání umožňuje z procházení adresářů *wwwroot nebo obrázky* složky pomocí adresy URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="3bfd4-172">Výchozí dokument</span><span class="sxs-lookup"><span data-stu-id="3bfd4-172">Serve a default document</span></span>

<span data-ttu-id="3bfd4-173">Nastavení výchozí domovskou stránku poskytuje návštěvníky logické výchozí bod při návštěvě webu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="3bfd4-174">Chcete-li poskytovat výchozí stránku bez plně kvalifikovaného identifikátoru URI uživatel, volejte [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) metoda z `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="3bfd4-175">`UseDefaultFiles` musí být volána před provedením `UseStaticFiles` k obsluze výchozí soubor.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="3bfd4-176">`UseDefaultFiles` je RW adresu URL, která ve skutečnosti není tento soubor zpracovat.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="3bfd4-177">Povolit middleware se statickými soubory přes `UseStaticFiles` pro tento soubor zpracovat.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="3bfd4-178">S `UseDefaultFiles`, požadavky na složku Hledat:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="3bfd4-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-179">*default.htm*</span></span>
* <span data-ttu-id="3bfd4-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-180">*default.html*</span></span>
* <span data-ttu-id="3bfd4-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-181">*index.htm*</span></span>
* <span data-ttu-id="3bfd4-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-182">*index.html*</span></span>

<span data-ttu-id="3bfd4-183">První soubor najde v seznamu je zpracovat, jako by byl plně kvalifikovaného identifikátoru URI požadavku.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="3bfd4-184">Adresu URL prohlížeče pokračuje tak, aby odrážela požadovaný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="3bfd4-185">Následující kód změní výchozí název souboru do *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="3bfd4-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="3bfd4-186">UseFileServer</span></span>

<span data-ttu-id="3bfd4-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) kombinuje funkce `UseStaticFiles`, `UseDefaultFiles`, a `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="3bfd4-188">Následující kód povolí zpracování statických souborů a výchozí soubor.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="3bfd4-189">Procházení adresářů není povoleno.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="3bfd4-190">Následující kód staví na bez parametrů přetížení povolením procházení adresářů:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="3bfd4-191">Vezměte v úvahu následující hierarchii:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="3bfd4-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-192">**wwwroot**</span></span>
  * <span data-ttu-id="3bfd4-193">**šablon stylů CSS**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-193">**css**</span></span>
  * <span data-ttu-id="3bfd4-194">**Bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-194">**images**</span></span>
  * <span data-ttu-id="3bfd4-195">**js**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-195">**js**</span></span>
* <span data-ttu-id="3bfd4-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="3bfd4-197">**Bitové kopie**</span><span class="sxs-lookup"><span data-stu-id="3bfd4-197">**images**</span></span>
      * <span data-ttu-id="3bfd4-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-198">*banner1.svg*</span></span>
  * <span data-ttu-id="3bfd4-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-199">*default.html*</span></span>

<span data-ttu-id="3bfd4-200">Následující kód umožňuje statické soubory, výchozí soubory a procházení adresářů z `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="3bfd4-201">`AddDirectoryBrowser` musí být voláno, když `EnableDirectoryBrowsing` hodnota vlastnosti je `true`:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="3bfd4-202">Použití souboru hierarchie a předchozím kódem, adresy URL přeložit takto:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="3bfd4-203">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="3bfd4-203">URI</span></span>            |                             <span data-ttu-id="3bfd4-204">Odpověď</span><span class="sxs-lookup"><span data-stu-id="3bfd4-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="3bfd4-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="3bfd4-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="3bfd4-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="3bfd4-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="3bfd4-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="3bfd4-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="3bfd4-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="3bfd4-209">Pokud neexistuje žádný soubor s názvem výchozí v *MyStaticFiles* directory *http://\<server_address > / StaticFiles* vrátí seznam s odkazy, můžete kliknout adresářů:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Seznam statické soubory](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="3bfd4-211">`UseDefaultFiles` a `UseDirectoryBrowser` použijte adresu URL *http://\<server_address > / StaticFiles* bez do adresy koncové lomítko k aktivaci straně klienta přesměrovat na *http://\<server_address > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="3bfd4-212">Všimněte si, přidání do adresy koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="3bfd4-213">Relativní adresy URL v rámci dokumenty jsou považovány za neplatné bez koncové lomítko.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="3bfd4-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="3bfd4-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="3bfd4-215">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) třída obsahuje `Mappings` vlastnost slouží jako mapování přípony souborů obsahu typy MIME.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="3bfd4-216">V následující ukázce je několik přípony souborů zaregistrovaný známé typy MIME.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="3bfd4-217">*.Rtf* rozšíření je nahrazena, a *.mp4* se odebere.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="3bfd4-218">V tématu [obsahu typy MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="3bfd4-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="3bfd4-219">Nestandardní typy obsahu</span><span class="sxs-lookup"><span data-stu-id="3bfd4-219">Non-standard content types</span></span>

<span data-ttu-id="3bfd4-220">Middleware se statickými soubory rozumí téměř 400 typy obsahu známému souboru.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="3bfd4-221">Pokud uživatel požaduje soubor neznámý typ souborů, middleware se statickými soubory vrátí odpověď HTTP 404 (není nalezena).</span><span class="sxs-lookup"><span data-stu-id="3bfd4-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="3bfd4-222">Pokud je povoleno procházení adresářů, zobrazí se odkaz na soubor.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="3bfd4-223">Identifikátor URI vrátí chybu HTTP 404.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="3bfd4-224">Následující kód umožňuje obsluhující neznámé typy a vykreslí Neznámý soubor jako obrázek:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="3bfd4-225">Předchozí kód je žádost o soubor s Neznámý typ obsahu vrácena jako obrázek.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="3bfd4-226">Povolení [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) představuje bezpečnostní riziko.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="3bfd4-227">Ve výchozím nastavení vypnutá, a jeho použití se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="3bfd4-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) poskytuje bezpečnější alternativu k obsluhovat soubory s nestandardní rozšíření.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="3bfd4-229">Důležité informace</span><span class="sxs-lookup"><span data-stu-id="3bfd4-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="3bfd4-230">`UseDirectoryBrowser` a `UseStaticFiles` může se nevrací tajných klíčů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="3bfd4-231">Důrazně doporučujeme zakázání v produkčním prostředí pro procházení adresářů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="3bfd4-232">Pečlivě zkontrolujte adresáře, které jsou povolené prostřednictvím `UseStaticFiles` nebo `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="3bfd4-233">Celý adresář a jeho dílčí adresáře budou veřejně přístupná.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="3bfd4-234">Úložiště soubory vhodný pro slouží veřejnosti vyhrazené adresáře, například  *\<content_root > / wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="3bfd4-235">Oddělte tyto soubory z zobrazení MVC, stránky Razor (pouze 2.x), konfiguračních souborů, atd.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="3bfd4-236">Adresy URL pro obsah vystaveny s `UseDirectoryBrowser` a `UseStaticFiles` podléhají malá a velká písmena a znak omezení podkladový systém souborů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="3bfd4-237">Například je malá a velká písmena Windows&mdash;systému macOS a Linux nejsou.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-237">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="3bfd4-238">Aplikace ASP.NET Core hostované služby IIS používá [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) předávat všechny požadavky na aplikace, včetně žádostí statických souborů.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="3bfd4-239">Obslužné rutiny statických souborů služby IIS se nepoužije.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="3bfd4-240">Nemá žádné šanci na zpracování požadavků, než se zpracovávají modulem.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-240">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="3bfd4-241">Pomocí následujících kroků ve Správci služby IIS k odebrání obslužné rutiny statických souborů služby IIS na úrovni serveru nebo webu:</span><span class="sxs-lookup"><span data-stu-id="3bfd4-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="3bfd4-242">Přejděte na **moduly** funkce.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="3bfd4-243">Vyberte **iisver** v seznamu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="3bfd4-244">Klikněte na tlačítko **odebrat** v **akce** bočním panelu.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="3bfd4-245">Pokud je povoleno obslužné rutiny statických souborů služby IIS **a** modul základní technologie ASP.NET není správně nakonfigurovaná, jsou obsluhovat statické soubory.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-245">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="3bfd4-246">K tomu dojde, například pokud *web.config* soubor není nasazená.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="3bfd4-247">Umístěte soubory kódu (včetně *.cs* a *.cshtml*) mimo projekt aplikace kořenový web.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="3bfd4-248">Proto vytvoří logického oddělení se mezi obsah klientské a serverové kód aplikace.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="3bfd4-249">To brání úniku kódu na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="3bfd4-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3bfd4-250">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="3bfd4-250">Additional resources</span></span>

* [<span data-ttu-id="3bfd4-251">Middleware</span><span class="sxs-lookup"><span data-stu-id="3bfd4-251">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="3bfd4-252">Úvod do ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3bfd4-252">Introduction to ASP.NET Core</span></span>](xref:index)
