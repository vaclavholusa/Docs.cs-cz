---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hostování na vlastním rozhraní ASP.NET Web API 1 (C#) | Microsoft Docs
author: MikeWasson
description: Rozhraní ASP.NET Web API nevyžaduje službu IIS. V procesu hostitele, může hostovat samoobslužné webové rozhraní API. Tento kurz ukazuje, jak k hostování webové rozhraní API uvnitř konzoly applic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 564f859e73a88ac9c5f27e9b8f7409ec126642f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="c7ac0-105">Hostování na vlastním rozhraní ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="c7ac0-106">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="c7ac0-107">Rozhraní ASP.NET Web API nevyžaduje službu IIS.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="c7ac0-108">V procesu hostitele, může hostovat samoobslužné webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="c7ac0-109">Tento kurz ukazuje, jak k hostování webové rozhraní API v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="c7ac0-110">**Nové aplikace by měly používat OWIN pro hostování na vlastním serveru webového rozhraní API.**</span><span class="sxs-lookup"><span data-stu-id="c7ac0-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="c7ac0-111">V tématu [použít OWIN k hostování na vlastním rozhraní ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac0-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c7ac0-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="c7ac0-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c7ac0-113">Webové rozhraní API 1</span><span class="sxs-lookup"><span data-stu-id="c7ac0-113">Web API 1</span></span>
> - <span data-ttu-id="c7ac0-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="c7ac0-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="c7ac0-115">Vytvořte projekt konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="c7ac0-115">Create the Console Application Project</span></span>

<span data-ttu-id="c7ac0-116">Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="c7ac0-117">Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="c7ac0-118">V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="c7ac0-119">V části **Visual C#**, vyberte **Windows**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="c7ac0-120">V seznamu šablon projektu, vyberte **konzolové aplikace**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="c7ac0-121">Název projektu &quot;SelfHost&quot; a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="c7ac0-122">Nastavení rozhraní Target Framework (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="c7ac0-123">Pokud používáte Visual Studio 2010, změňte cílový framework rozhraní .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="c7ac0-124">(Ve výchozím nastavení šablona cíle projektu [profilu rozhraní .net Framework klienta](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="c7ac0-125">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="c7ac0-126">V **cílové rozhraní** rozevíracího seznamu, změňte cílový framework na .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="c7ac0-127">Po zobrazení výzvy na použití změny, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="c7ac0-128">Instalace Správce balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="c7ac0-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="c7ac0-129">Správce balíčků NuGet je nejjednodušší způsob, jak přidat sestavení webového rozhraní API do projektu mimo technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="c7ac0-130">Zkontrolujte, zda je nainstalován Správce balíčků NuGet, klikněte na tlačítko **nástroje** nabídky v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="c7ac0-131">Pokud se zobrazí nabídky položky názvem **Správce balíčků knihoven**, pak máte Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-131">If you see a menu item called **Library Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="c7ac0-132">Instalace Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="c7ac0-133">Spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="c7ac0-134">Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="c7ac0-135">V **rozšíření a aktualizace** dialogovém okně, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="c7ac0-136">Pokud nevidíte "Správce balíčků NuGet", zadejte do vyhledávacího pole "Správce balíčků nuget".</span><span class="sxs-lookup"><span data-stu-id="c7ac0-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="c7ac0-137">Vyberte Správce balíčků NuGet a klikněte na **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="c7ac0-138">Po dokončení stahování, zobrazí se výzva k instalaci.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="c7ac0-139">Po dokončení instalace může být vyzvání k restartování sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="c7ac0-140">Přidejte balíček NuGet rozhraní API webové</span><span class="sxs-lookup"><span data-stu-id="c7ac0-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="c7ac0-141">Po instalaci Správce balíčků NuGet do projektu přidejte balíček Self-Host webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="c7ac0-142">Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-142">From the **Tools** menu, select **Library Package Manager**.</span></span> <span data-ttu-id="c7ac0-143">*Poznámka:*: Pokud není se tato nabídka položky, ujistěte se, že Správce balíčků NuGet správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="c7ac0-144">Vyberte **spravovat balíčky NuGet pro řešení...**</span><span class="sxs-lookup"><span data-stu-id="c7ac0-144">Select **Manage NuGet Packages for Solution...**</span></span>
3. <span data-ttu-id="c7ac0-145">V **Správa balíčků Nuget** dialogovém okně, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="c7ac0-146">Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="c7ac0-147">Vyberte balíček, ASP.NET Web API Self Host a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="c7ac0-148">Po instalaci balíčku, klikněte na tlačítko **zavřete** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ac0-149">Ujistěte se, že jste nainstalovali balíček s názvem Microsoft.AspNet.WebApi.SelfHost, není AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="c7ac0-150">Vytvoření modelu a řadiče</span><span class="sxs-lookup"><span data-stu-id="c7ac0-150">Create the Model and Controller</span></span>

<span data-ttu-id="c7ac0-151">Tento kurz používá stejný model a řadič třídy, jako [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="c7ac0-152">Přidejte veřejnou třídu s názvem `Product`.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="c7ac0-153">Přidejte veřejnou třídu s názvem `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="c7ac0-154">Odvození z této třídy **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="c7ac0-155">Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="c7ac0-156">Tento řadič definuje tři GET akce:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="c7ac0-157">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="c7ac0-157">URI</span></span> | <span data-ttu-id="c7ac0-158">Popis</span><span class="sxs-lookup"><span data-stu-id="c7ac0-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="c7ac0-159">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="c7ac0-159">/api/products</span></span> | <span data-ttu-id="c7ac0-160">Získání seznamu všech produktů.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-160">Get a list of all products.</span></span> |
| <span data-ttu-id="c7ac0-161">/api/products/*id*</span><span class="sxs-lookup"><span data-stu-id="c7ac0-161">/api/products/*id*</span></span> | <span data-ttu-id="c7ac0-162">Získání produktu podle ID.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-162">Get a product by ID.</span></span> |
| <span data-ttu-id="c7ac0-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="c7ac0-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="c7ac0-164">Získáte seznam produktů podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="c7ac0-165">Hostitel webové rozhraní API</span><span class="sxs-lookup"><span data-stu-id="c7ac0-165">Host the Web API</span></span>

<span data-ttu-id="c7ac0-166">Otevřete soubor Program.cs a přidejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="c7ac0-167">Přidejte následující kód, který **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="c7ac0-168">(Volitelné) Přidat rezervaci Namespace adresy URL protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="c7ac0-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="c7ac0-169">Tato aplikace naslouchá `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="c7ac0-170">Ve výchozím nastavení naslouchání na konkrétní adrese HTTP vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="c7ac0-171">Když spustíte tohoto kurzu, proto může se tato chyba: "Protokolu HTTP nebylo možné zaregistrovat URL http://+:8080/" existují dva způsoby, jak se vyhnout této chybě:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="c7ac0-172">Visual Studio spustit s oprávněními zvýšenými na úroveň správce, nebo</span><span class="sxs-lookup"><span data-stu-id="c7ac0-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="c7ac0-173">Pomocí Netsh.exe udělit oprávnění pro uživatelský účet tak, aby vyhradil adresu URL.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="c7ac0-174">Pokud chcete používat Netsh.exe, otevřete příkazový řádek s oprávněními správce a zadejte následující příkaz: následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="c7ac0-175">kde *machine\username* je váš uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="c7ac0-176">Po dokončení vlastní hostování, ujistěte se, jestli chcete odstranit rezervaci:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="c7ac0-177">Volání webového rozhraní API z klientské aplikace (C#)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="c7ac0-178">Můžete napsat jednoduchý konzolovou aplikaci, která volá webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="c7ac0-179">Do řešení přidáte nový projekt konzolové aplikace:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="c7ac0-180">V Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **přidat nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="c7ac0-181">Vytvořte novou konzolovou aplikaci s názvem &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="c7ac0-182">Správce balíčků NuGet použijte k přidání balíčku ASP.NET Web API Core Libraries:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="c7ac0-183">Z nabídky Nástroje, vyberte **Správce balíčků knihoven**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-183">From the Tools menu, select **Library Package Manager**.</span></span>
- <span data-ttu-id="c7ac0-184">Vyberte **spravovat balíčky NuGet pro řešení...**</span><span class="sxs-lookup"><span data-stu-id="c7ac0-184">Select **Manage NuGet Packages for Solution...**</span></span>
- <span data-ttu-id="c7ac0-185">V **spravovat balíčky NuGet** dialogovém okně, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="c7ac0-186">Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="c7ac0-187">Vyberte balíček Microsoft ASP.NET Web API Client Libraries a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="c7ac0-188">Přidejte odkaz na projekt SelfHost ClientApp:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="c7ac0-189">V Průzkumníku řešení klikněte pravým tlačítkem na projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="c7ac0-190">Vyberte **přidat odkaz**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="c7ac0-191">V **správce odkazů** dialogové okno, v části **řešení**, vyberte **projekty**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="c7ac0-192">Vyberte projekt SelfHost.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="c7ac0-193">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="c7ac0-194">Otevřete soubor Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="c7ac0-195">Přidejte následující **pomocí** příkaz:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="c7ac0-196">Přidání statického **HttpClient** instance:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="c7ac0-197">Přidejte následující metody k zobrazení seznamu všech produktů, seznam produktů podle ID a seznam produktů podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="c7ac0-198">Každá z těchto metod dodržuje stejného vzoru:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="c7ac0-199">Volání **HttpClient.GetAsync** odeslat požadavek GET na odpovídající identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="c7ac0-200">Volání **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="c7ac0-201">Tato metoda vyvolá výjimku, pokud je stav odpovědi HTTP chybový kód.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="c7ac0-202">Volání **ReadAsAsync&lt;T&gt;**  k deserializaci typ CLR z odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="c7ac0-203">Tato metoda je metody rozšíření, definované v **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="c7ac0-204">**GetAsync** a **ReadAsAsync** jsou oba asynchronní metody.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="c7ac0-205">Vracejí **úloh** objekty představující asynchronní operaci.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="c7ac0-206">Získávání **výsledek** vlastnost blokuje vlákno, dokud se operace nedokončí.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="c7ac0-207">Další informace o používání HttpClient, včetně toho, jak provádět volání neblokující najdete v části [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="c7ac0-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="c7ac0-208">Před voláním těchto metod, nastavte vlastnost BaseAddress na instanci systému na HttpClient "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="c7ac0-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="c7ac0-209">Příklad:</span><span class="sxs-lookup"><span data-stu-id="c7ac0-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="c7ac0-210">To by výstup následující.</span><span class="sxs-lookup"><span data-stu-id="c7ac0-210">This should output the following.</span></span> <span data-ttu-id="c7ac0-211">(Nezapomeňte nejprve spustit aplikaci SelfHost.)</span><span class="sxs-lookup"><span data-stu-id="c7ac0-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
