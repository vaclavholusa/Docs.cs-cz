---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hostování na vlastním rozhraní ASP.NET Web API 1 (C#) | Dokumentace Microsoftu
author: MikeWasson
description: Rozhraní ASP.NET Web API nevyžaduje, aby služba IIS. Webové rozhraní API můžete samoobslužné hostování ve vlastním procesu hostitele. Tento kurz ukazuje postupy při hostování webového rozhraní API uvnitř applic konzoly...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912695"
---
<a name="self-host-aspnet-web-api-1-c"></a><span data-ttu-id="3f857-105">Hostování na vlastním rozhraní ASP.NET Web API 1 (C#)</span><span class="sxs-lookup"><span data-stu-id="3f857-105">Self-Host ASP.NET Web API 1 (C#)</span></span>
====================
<span data-ttu-id="3f857-106">podle [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3f857-106">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3f857-107">Rozhraní ASP.NET Web API nevyžaduje, aby služba IIS.</span><span class="sxs-lookup"><span data-stu-id="3f857-107">ASP.NET Web API does not require IIS.</span></span> <span data-ttu-id="3f857-108">Webové rozhraní API můžete samoobslužné hostování ve vlastním procesu hostitele.</span><span class="sxs-lookup"><span data-stu-id="3f857-108">You can self-host a web API in your own host process.</span></span> <span data-ttu-id="3f857-109">Tento kurz ukazuje postupy při hostování webového rozhraní API v konzolové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="3f857-109">This tutorial shows how to host a web API inside a console application.</span></span>
> 
> <span data-ttu-id="3f857-110">**Nová aplikace by měly používat OWIN k samoobslužnému hostování webového rozhraní API.**</span><span class="sxs-lookup"><span data-stu-id="3f857-110">**New applications should use OWIN to self-host Web API.**</span></span> <span data-ttu-id="3f857-111">Zobrazit [použití rozhraní OWIN k samoobslužnému hostování webového rozhraní API 2 ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="3f857-111">See [Use OWIN to Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3f857-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="3f857-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3f857-113">Webové rozhraní API 1</span><span class="sxs-lookup"><span data-stu-id="3f857-113">Web API 1</span></span>
> - <span data-ttu-id="3f857-114">Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="3f857-114">Visual Studio 2012</span></span>


## <a name="create-the-console-application-project"></a><span data-ttu-id="3f857-115">Vytvořte projekt konzolové aplikace</span><span class="sxs-lookup"><span data-stu-id="3f857-115">Create the Console Application Project</span></span>

<span data-ttu-id="3f857-116">Spusťte sadu Visual Studio a vyberte **nový projekt** z **Start** stránky.</span><span class="sxs-lookup"><span data-stu-id="3f857-116">Start Visual Studio and select **New Project** from the **Start** page.</span></span> <span data-ttu-id="3f857-117">Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.</span><span class="sxs-lookup"><span data-stu-id="3f857-117">Or, from the **File** menu, select **New** and then **Project**.</span></span>

<span data-ttu-id="3f857-118">V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu.</span><span class="sxs-lookup"><span data-stu-id="3f857-118">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="3f857-119">V části **Visual C#** vyberte **Windows**.</span><span class="sxs-lookup"><span data-stu-id="3f857-119">Under **Visual C#**, select **Windows**.</span></span> <span data-ttu-id="3f857-120">V seznamu šablon projektu vyberte **konzolovou aplikaci**.</span><span class="sxs-lookup"><span data-stu-id="3f857-120">In the list of project templates, select **Console Application**.</span></span> <span data-ttu-id="3f857-121">Pojmenujte projekt &quot;SelfHost&quot; a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f857-121">Name the project &quot;SelfHost&quot; and click **OK**.</span></span>

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a><span data-ttu-id="3f857-122">Nastavit cílové rozhraní (Visual Studio 2010)</span><span class="sxs-lookup"><span data-stu-id="3f857-122">Set the Target Framework (Visual Studio 2010)</span></span>

<span data-ttu-id="3f857-123">Pokud používáte Visual Studio 2010, změňte cílovou architekturu na .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="3f857-123">If you are using Visual Studio 2010, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="3f857-124">(Ve výchozím nastavení šablona cíle projektu [rozhraní .net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span><span class="sxs-lookup"><span data-stu-id="3f857-124">(By default, the project template targets the [.Net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)</span></span>

<span data-ttu-id="3f857-125">V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**.</span><span class="sxs-lookup"><span data-stu-id="3f857-125">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="3f857-126">V **Cílová architektura** rozevírací seznam, změnit cílovou architekturu na .NET Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="3f857-126">In the **Target framework** dropdown list, change the target framework to .NET Framework 4.0.</span></span> <span data-ttu-id="3f857-127">Po zobrazení výzvy na použití změny, klikněte na tlačítko **Ano**.</span><span class="sxs-lookup"><span data-stu-id="3f857-127">When prompted to apply the change, click **Yes**.</span></span>

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a><span data-ttu-id="3f857-128">Instalace Správce balíčků NuGet</span><span class="sxs-lookup"><span data-stu-id="3f857-128">Install NuGet Package Manager</span></span>

<span data-ttu-id="3f857-129">Správce balíčků NuGet je nejjednodušší způsob, jak přidat sestavení webového rozhraní API do projektu – technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f857-129">The NuGet Package Manager is the easiest way to add the Web API assemblies to a non-ASP.NET project.</span></span>

<span data-ttu-id="3f857-130">Pokud chcete zkontrolovat, jestli je nainstalovaný Správce balíčků NuGet, klikněte na tlačítko **nástroje** nabídky v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f857-130">To check if NuGet Package Manager is installed, click the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="3f857-131">Pokud se zobrazí nabídka položek volá **Správce balíčků NuGet**, pak máte Správce balíčků NuGet.</span><span class="sxs-lookup"><span data-stu-id="3f857-131">If you see a menu item called **NuGet Package Manager**, then you have NuGet Package Manager.</span></span>

<span data-ttu-id="3f857-132">Instalace Správce balíčků NuGet:</span><span class="sxs-lookup"><span data-stu-id="3f857-132">To install NuGet Package Manager:</span></span>

1. <span data-ttu-id="3f857-133">Spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f857-133">Start Visual Studio.</span></span>
2. <span data-ttu-id="3f857-134">Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="3f857-134">From the **Tools** menu, select **Extensions and Updates**.</span></span>
3. <span data-ttu-id="3f857-135">V **rozšíření a aktualizace** dialogového okna, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="3f857-135">In the **Extensions and Updates** dialog, select **Online**.</span></span>
4. <span data-ttu-id="3f857-136">Pokud nevidíte "Správce balíčků NuGet", zadejte do vyhledávacího pole "Správce balíčků nuget".</span><span class="sxs-lookup"><span data-stu-id="3f857-136">If you don't see "NuGet Package Manager", type "nuget package manager" in the search box.</span></span>
5. <span data-ttu-id="3f857-137">Vyberte Správce balíčků NuGet a klikněte na tlačítko **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="3f857-137">Select the NuGet Package Manager and click **Download**.</span></span>
6. <span data-ttu-id="3f857-138">Až se stahování dokončí, zobrazí se výzva k instalaci.</span><span class="sxs-lookup"><span data-stu-id="3f857-138">After the download completes, you will be prompted to install.</span></span>
7. <span data-ttu-id="3f857-139">Po dokončení instalace, vám může zobrazit výzva k restartování sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f857-139">After the installation completes, you might be prompted to restart Visual Studio.</span></span>

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a><span data-ttu-id="3f857-140">Přidat webový balíček NuGet rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3f857-140">Add the Web API NuGet Package</span></span>

<span data-ttu-id="3f857-141">Po dokončení instalace Správce balíčků NuGet do projektu přidejte balíček Self-Host webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f857-141">After NuGet Package Manager is installed, add the Web API Self-Host package to your project.</span></span>

1. <span data-ttu-id="3f857-142">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3f857-142">From the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="3f857-143">*Poznámka:*: Pokud se vám nezobrazí tato nabídka položek, ujistěte se, že tento správce balíčků NuGet správně nainstalován.</span><span class="sxs-lookup"><span data-stu-id="3f857-143">*Note*: If do you not see this menu item, make sure that NuGet Package Manager installed correctly.</span></span>
2. <span data-ttu-id="3f857-144">Vyberte **spravovat balíčky NuGet pro řešení**</span><span class="sxs-lookup"><span data-stu-id="3f857-144">Select **Manage NuGet Packages for Solution**</span></span>
3. <span data-ttu-id="3f857-145">V **Správa balíčků Nuget** dialogového okna, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="3f857-145">In the **Manage NugGet Packages** dialog, select **Online**.</span></span>
4. <span data-ttu-id="3f857-146">Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span><span class="sxs-lookup"><span data-stu-id="3f857-146">In the search box, type &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.</span></span>
5. <span data-ttu-id="3f857-147">Vyberte balíček ASP.NET Web API Self hostitele a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="3f857-147">Select the ASP.NET Web API Self Host package and click **Install**.</span></span>
6. <span data-ttu-id="3f857-148">Po instalaci balíčku, klikněte na tlačítko **zavřete** zavřete dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="3f857-148">After the package installs, click **Close** to close the dialog.</span></span>

> [!NOTE]
> <span data-ttu-id="3f857-149">Ujistěte se, že k instalaci balíčku s názvem Microsoft.AspNet.WebApi.SelfHost, ne AspNetWebApi.SelfHost.</span><span class="sxs-lookup"><span data-stu-id="3f857-149">Make sure to install the package named Microsoft.AspNet.WebApi.SelfHost, not AspNetWebApi.SelfHost.</span></span>

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a><span data-ttu-id="3f857-150">Vytvoření modelu a kontroler</span><span class="sxs-lookup"><span data-stu-id="3f857-150">Create the Model and Controller</span></span>

<span data-ttu-id="3f857-151">Tento kurz používá stejné třídy modelu a kontroler jako [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="3f857-151">This tutorial uses the same model and controller classes as the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span>

<span data-ttu-id="3f857-152">Přidejte veřejnou třídu s názvem `Product`.</span><span class="sxs-lookup"><span data-stu-id="3f857-152">Add a public class named `Product`.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

<span data-ttu-id="3f857-153">Přidejte veřejnou třídu s názvem `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="3f857-153">Add a public class named `ProductsController`.</span></span> <span data-ttu-id="3f857-154">Odvození z této třídy **System.Web.Http.ApiController**.</span><span class="sxs-lookup"><span data-stu-id="3f857-154">Derive this class from **System.Web.Http.ApiController**.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

<span data-ttu-id="3f857-155">Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="3f857-155">For more information about the code in this controller, see the [Getting Started](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) tutorial.</span></span> <span data-ttu-id="3f857-156">Tento kontroler definuje tři akce GET:</span><span class="sxs-lookup"><span data-stu-id="3f857-156">This controller defines three GET actions:</span></span>

| <span data-ttu-id="3f857-157">Identifikátor URI</span><span class="sxs-lookup"><span data-stu-id="3f857-157">URI</span></span> | <span data-ttu-id="3f857-158">Popis</span><span class="sxs-lookup"><span data-stu-id="3f857-158">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3f857-159">/ api/produkty</span><span class="sxs-lookup"><span data-stu-id="3f857-159">/api/products</span></span> | <span data-ttu-id="3f857-160">Získání seznamu všech produktů.</span><span class="sxs-lookup"><span data-stu-id="3f857-160">Get a list of all products.</span></span> |
| <span data-ttu-id="3f857-161">/ webové rozhraníAPI/produkty/*id*</span><span class="sxs-lookup"><span data-stu-id="3f857-161">/api/products/*id*</span></span> | <span data-ttu-id="3f857-162">Získání produktu podle ID.</span><span class="sxs-lookup"><span data-stu-id="3f857-162">Get a product by ID.</span></span> |
| <span data-ttu-id="3f857-163">/api/products/?category=*category*</span><span class="sxs-lookup"><span data-stu-id="3f857-163">/api/products/?category=*category*</span></span> | <span data-ttu-id="3f857-164">Získáte seznam produktů podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="3f857-164">Get a list of products by category.</span></span> |

## <a name="host-the-web-api"></a><span data-ttu-id="3f857-165">Hostování webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="3f857-165">Host the Web API</span></span>

<span data-ttu-id="3f857-166">Otevřete soubor Program.cs a přidejte následující příkazy using:</span><span class="sxs-lookup"><span data-stu-id="3f857-166">Open the file Program.cs and add the following using statements:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

<span data-ttu-id="3f857-167">Přidejte následující kód, který **Program** třídy.</span><span class="sxs-lookup"><span data-stu-id="3f857-167">Add the following code to the **Program** class.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a><span data-ttu-id="3f857-168">(Volitelné) Přidat rezervaci Namespace adresy URL protokolu HTTP</span><span class="sxs-lookup"><span data-stu-id="3f857-168">(Optional) Add an HTTP URL Namespace Reservation</span></span>

<span data-ttu-id="3f857-169">Tato aplikace naslouchá na `http://localhost:8080/`.</span><span class="sxs-lookup"><span data-stu-id="3f857-169">This application listens to `http://localhost:8080/`.</span></span> <span data-ttu-id="3f857-170">Ve výchozím nastavení naslouchání na konkrétní adrese HTTP vyžaduje oprávnění správce.</span><span class="sxs-lookup"><span data-stu-id="3f857-170">By default, listening at a particular HTTP address requires administrator privileges.</span></span> <span data-ttu-id="3f857-171">Při spuštění tohoto kurzu, proto se může zobrazit tato chyba: "protokol HTTP nemohl zaregistrovat adresu URL http://+:8080/" existují dva způsoby, jak se vyhnout se této chybě:</span><span class="sxs-lookup"><span data-stu-id="3f857-171">When you run the tutorial, therefore, you may get this error: "HTTP could not register URL http://+:8080/" There are two ways to avoid this error:</span></span>

- <span data-ttu-id="3f857-172">Spuštění sady Visual Studio s oprávněními zvýšenými na úroveň správce, nebo</span><span class="sxs-lookup"><span data-stu-id="3f857-172">Run Visual Studio with elevated administrator permissions, or</span></span>
- <span data-ttu-id="3f857-173">Pomocí Netsh.exe udělte vašeho účtu oprávnění k rezervaci adresy URL.</span><span class="sxs-lookup"><span data-stu-id="3f857-173">Use Netsh.exe to give your account permissions to reserve the URL.</span></span>

<span data-ttu-id="3f857-174">Pokud chcete použít Netsh.exe, otevřete příkazový řádek s oprávněními správce a zadejte následující příkaz: následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="3f857-174">To use Netsh.exe, open a command prompt with administrator privileges and enter the following command:following command:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

<span data-ttu-id="3f857-175">kde *počítač\uživatelské_jméno* je váš uživatelský účet.</span><span class="sxs-lookup"><span data-stu-id="3f857-175">where *machine\username* is your user account.</span></span>

<span data-ttu-id="3f857-176">Až budete hotovi s vlastním hostováním, je potřeba odstranit rezervaci:</span><span class="sxs-lookup"><span data-stu-id="3f857-176">When you are finished self-hosting, be sure to delete the reservation:</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a><span data-ttu-id="3f857-177">Volání webového rozhraní API z klientské aplikace (C#)</span><span class="sxs-lookup"><span data-stu-id="3f857-177">Call the Web API from a Client Application (C#)</span></span>

<span data-ttu-id="3f857-178">Napíšeme jednoduchou konzolovou aplikaci, která volá webové rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="3f857-178">Let's write a simple console application that calls the web API.</span></span>

<span data-ttu-id="3f857-179">Přidáte do řešení nový projekt konzolové aplikace:</span><span class="sxs-lookup"><span data-stu-id="3f857-179">Add a new console application project to the solution:</span></span>

- <span data-ttu-id="3f857-180">V Průzkumníku řešení klikněte pravým tlačítkem myši na řešení a vyberte **přidat nový projekt**.</span><span class="sxs-lookup"><span data-stu-id="3f857-180">In Solution Explorer, right-click the solution and select **Add New Project**.</span></span>
- <span data-ttu-id="3f857-181">Vytvořte novou konzolovou aplikaci s názvem &quot;ClientApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="3f857-181">Create a new console application named &quot;ClientApp&quot;.</span></span>

![](self-host-a-web-api/_static/image5.png)

<span data-ttu-id="3f857-182">Použití Správce balíčků NuGet pro přidání balíčku ASP.NET Web API základní knihovny:</span><span class="sxs-lookup"><span data-stu-id="3f857-182">Use NuGet Package Manager to add the ASP.NET Web API Core Libraries package:</span></span>

- <span data-ttu-id="3f857-183">V nabídce Nástroje vyberte **Správce balíčků NuGet**.</span><span class="sxs-lookup"><span data-stu-id="3f857-183">From the Tools menu, select **NuGet Package Manager**.</span></span>
- <span data-ttu-id="3f857-184">Vyberte **spravovat balíčky NuGet pro řešení**</span><span class="sxs-lookup"><span data-stu-id="3f857-184">Select **Manage NuGet Packages for Solution**</span></span>
- <span data-ttu-id="3f857-185">V **spravovat balíčky NuGet** dialogového okna, vyberte **Online**.</span><span class="sxs-lookup"><span data-stu-id="3f857-185">In the **Manage NuGet Packages** dialog, select **Online**.</span></span>
- <span data-ttu-id="3f857-186">Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.Client&quot;.</span><span class="sxs-lookup"><span data-stu-id="3f857-186">In the search box, type &quot;Microsoft.AspNet.WebApi.Client&quot;.</span></span>
- <span data-ttu-id="3f857-187">Vyberte balíček Microsoft ASP.NET Web API klientské knihovny a klikněte na tlačítko **nainstalovat**.</span><span class="sxs-lookup"><span data-stu-id="3f857-187">Select the Microsoft ASP.NET Web API Client Libraries package and click **Install**.</span></span>

<span data-ttu-id="3f857-188">Přidáte odkaz v ClientApp do projektu hostitel ve vlastním procesu:</span><span class="sxs-lookup"><span data-stu-id="3f857-188">Add a reference in ClientApp to the SelfHost project:</span></span>

- <span data-ttu-id="3f857-189">V Průzkumníku řešení klikněte pravým tlačítkem na projekt ClientApp.</span><span class="sxs-lookup"><span data-stu-id="3f857-189">In Solution Explorer, right-click the ClientApp project.</span></span>
- <span data-ttu-id="3f857-190">Vyberte **přidat odkaz na**.</span><span class="sxs-lookup"><span data-stu-id="3f857-190">Select **Add Reference**.</span></span>
- <span data-ttu-id="3f857-191">V **správce odkazů** dialogového okna, v části **řešení**vyberte **projekty**.</span><span class="sxs-lookup"><span data-stu-id="3f857-191">In the **Reference Manager** dialog, under **Solution**, select **Projects**.</span></span>
- <span data-ttu-id="3f857-192">Vyberte projekt, hostitel ve vlastním procesu.</span><span class="sxs-lookup"><span data-stu-id="3f857-192">Select the SelfHost project.</span></span>
- <span data-ttu-id="3f857-193">Klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="3f857-193">Click **OK**.</span></span>

![](self-host-a-web-api/_static/image6.png)

<span data-ttu-id="3f857-194">Otevřete soubor Client/Program.cs.</span><span class="sxs-lookup"><span data-stu-id="3f857-194">Open the Client/Program.cs file.</span></span> <span data-ttu-id="3f857-195">Přidejte následující **pomocí** – příkaz:</span><span class="sxs-lookup"><span data-stu-id="3f857-195">Add the following **using** statement:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

<span data-ttu-id="3f857-196">Přidání statického **HttpClient** instance:</span><span class="sxs-lookup"><span data-stu-id="3f857-196">Add a static **HttpClient** instance:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

<span data-ttu-id="3f857-197">Přidejte následující metody, které jsou uvedeny všechny produkty, seznam produktů podle ID a seznam produktů podle kategorie.</span><span class="sxs-lookup"><span data-stu-id="3f857-197">Add the following methods to list all products, list a product by ID, and list products by category.</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

<span data-ttu-id="3f857-198">Každá z těchto metod používá stejný vzor:</span><span class="sxs-lookup"><span data-stu-id="3f857-198">Each of these methods follows the same pattern:</span></span>

1. <span data-ttu-id="3f857-199">Volání **HttpClient.GetAsync** odešlete požadavek GET na odpovídající identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="3f857-199">Call **HttpClient.GetAsync** to send a GET request to the appropriate URI.</span></span>
2. <span data-ttu-id="3f857-200">Volání **HttpResponseMessage.EnsureSuccessStatusCode**.</span><span class="sxs-lookup"><span data-stu-id="3f857-200">Call **HttpResponseMessage.EnsureSuccessStatusCode**.</span></span> <span data-ttu-id="3f857-201">Tato metoda vyvolá výjimku, pokud je stav odpovědi HTTP chybový kód.</span><span class="sxs-lookup"><span data-stu-id="3f857-201">This method throws an exception if the HTTP response status is an error code.</span></span>
3. <span data-ttu-id="3f857-202">Volání **ReadAsAsync&lt;T&gt;**  deserializovat typ CLR z odpovědi HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f857-202">Call **ReadAsAsync&lt;T&gt;** to deserialize a CLR type from the HTTP response.</span></span> <span data-ttu-id="3f857-203">Tato metoda je metoda rozšiřující, definované v **System.Net.Http.HttpContentExtensions**.</span><span class="sxs-lookup"><span data-stu-id="3f857-203">This method is an extension method, defined in **System.Net.Http.HttpContentExtensions**.</span></span>

<span data-ttu-id="3f857-204">**GetAsync** a **ReadAsAsync** metody jsou asynchronní.</span><span class="sxs-lookup"><span data-stu-id="3f857-204">The **GetAsync** and **ReadAsAsync** methods are both asynchronous.</span></span> <span data-ttu-id="3f857-205">Vrátí **úloh** objekty, které představují asynchronní operace.</span><span class="sxs-lookup"><span data-stu-id="3f857-205">They return **Task** objects that represent the asynchronous operation.</span></span> <span data-ttu-id="3f857-206">Začínáme **výsledek** vlastnost blokuje vlákno, dokud se operace dokončí.</span><span class="sxs-lookup"><span data-stu-id="3f857-206">Getting the **Result** property blocks the thread until the operation completes.</span></span>

<span data-ttu-id="3f857-207">Další informace o používání HttpClient, včetně postupu provádění neblokující volání, naleznete v tématu [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="3f857-207">For more information about using HttpClient, including how to make non-blocking calls, see [Calling a Web API From a .NET Client](../advanced/calling-a-web-api-from-a-net-client.md).</span></span>

<span data-ttu-id="3f857-208">Před voláním těchto metod, nastavte vlastnost BaseAddress nastavte na instanci HttpClient "`http://localhost:8080`".</span><span class="sxs-lookup"><span data-stu-id="3f857-208">Before calling these methods, set the BaseAddress property on the HttpClient instance to "`http://localhost:8080`".</span></span> <span data-ttu-id="3f857-209">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3f857-209">For example:</span></span>

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

<span data-ttu-id="3f857-210">To by měl výstupu následující.</span><span class="sxs-lookup"><span data-stu-id="3f857-210">This should output the following.</span></span> <span data-ttu-id="3f857-211">(Nezapomeňte nejprve spusťte aplikaci SelfHost).</span><span class="sxs-lookup"><span data-stu-id="3f857-211">(Remember to run the SelfHost application first.)</span></span>

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
