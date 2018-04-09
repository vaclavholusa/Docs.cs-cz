---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: Testování částí aplikací SignalR | Microsoft Docs
author: pfletcher
description: Tento článek popisuje, jak používat funkce testování částí 2.0 SignalR.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cff866716cb1179e02b930f33cb0f8c33d4a6cf0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="unit-testing-signalr-applications"></a><span data-ttu-id="cf9d5-103">Aplikací SignalR testování částí</span><span class="sxs-lookup"><span data-stu-id="cf9d5-103">Unit Testing SignalR Applications</span></span>
====================
<span data-ttu-id="cf9d5-104">podle [Patrik Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="cf9d5-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="cf9d5-105">Tento článek popisuje použití funkce SignalR 2 testování částí.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-105">This article describes using the Unit Testing features of SignalR 2.</span></span> 
> 
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="cf9d5-106">Verze softwaru použitým v tomto tématu</span><span class="sxs-lookup"><span data-stu-id="cf9d5-106">Software versions used in this topic</span></span>
> 
> 
> - [<span data-ttu-id="cf9d5-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="cf9d5-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="cf9d5-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="cf9d5-108">.NET 4.5</span></span>
> - <span data-ttu-id="cf9d5-109">SignalR verze 2</span><span class="sxs-lookup"><span data-stu-id="cf9d5-109">SignalR version 2</span></span>
>   
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="cf9d5-110">Dotazy a připomínky</span><span class="sxs-lookup"><span data-stu-id="cf9d5-110">Questions and comments</span></span>
> 
> <span data-ttu-id="cf9d5-111">Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-111">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="cf9d5-112">Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="cf9d5-112">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a><span data-ttu-id="cf9d5-113">Testování částí aplikací SignalR</span><span class="sxs-lookup"><span data-stu-id="cf9d5-113">Unit testing SignalR applications</span></span>

<span data-ttu-id="cf9d5-114">Funkce testů jednotek v SignalR 2 slouží k vytvoření testů jednotek pro aplikace SignalR.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-114">You can use the unit test features in SignalR 2 to create unit tests for your SignalR application.</span></span> <span data-ttu-id="cf9d5-115">SignalR 2 zahrnuje [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) rozhraní, které slouží k vytvoření mock objektu k simulaci vaší metod rozbočovače pro testování.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-115">SignalR 2 includes the [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) interface, which can be used to create a mock object to simulate your hub methods for testing.</span></span>

<span data-ttu-id="cf9d5-116">V této části přidáte testy částí pro aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí [XUnit.net](https://github.com/xunit/xunit) a [Moq](https://github.com/Moq/moq4).</span><span class="sxs-lookup"><span data-stu-id="cf9d5-116">In this section, you'll add unit tests for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using [XUnit.net](https://github.com/xunit/xunit) and [Moq](https://github.com/Moq/moq4).</span></span>

<span data-ttu-id="cf9d5-117">XUnit.net se použije k řízení test; Moq se použije k vytvoření [model](http://en.wikipedia.org/wiki/Mock_object) objekt pro testování.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-117">XUnit.net will be used to control the test; Moq will be used to create a [mock](http://en.wikipedia.org/wiki/Mock_object) object for testing.</span></span> <span data-ttu-id="cf9d5-118">Další mocking architektury lze v případě potřeby; [NSubstitute](http://nsubstitute.github.io/) je také vhodné použít.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-118">Other mocking frameworks can be used if desired; [NSubstitute](http://nsubstitute.github.io/) is also a good choice.</span></span> <span data-ttu-id="cf9d5-119">Tento kurz ukazuje, jak nastavit mock objektu dvěma způsoby: nejdřív pomocí `dynamic` objektu (dostupné v rozhraní .NET Framework 4) a druhý, přes rozhraní.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-119">This tutorial demonstrates how to set up the mock object in two ways: First, using a `dynamic` object (introduced in .NET Framework 4), and second, using an interface.</span></span>

### <a name="contents"></a><span data-ttu-id="cf9d5-120">Obsah</span><span class="sxs-lookup"><span data-stu-id="cf9d5-120">Contents</span></span>

<span data-ttu-id="cf9d5-121">Tento kurz obsahuje následující části.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-121">This tutorial contains the following sections.</span></span>

- [<span data-ttu-id="cf9d5-122">Testování částí pomocí dynamické</span><span class="sxs-lookup"><span data-stu-id="cf9d5-122">Unit testing with Dynamic</span></span>](#dynamic)
- [<span data-ttu-id="cf9d5-123">Podle typu testování částí</span><span class="sxs-lookup"><span data-stu-id="cf9d5-123">Unit testing by type</span></span>](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a><span data-ttu-id="cf9d5-124">Testování částí pomocí dynamické</span><span class="sxs-lookup"><span data-stu-id="cf9d5-124">Unit testing with Dynamic</span></span>

<span data-ttu-id="cf9d5-125">V této části přidáte testů jednotek pro aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí dynamických objektů.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-125">In this section, you'll add a unit test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using a dynamic object.</span></span>

1. <span data-ttu-id="cf9d5-126">Nainstalujte [XUnit Runner rozšíření](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) pro Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-126">Install the [XUnit Runner extension](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) for Visual Studio 2013.</span></span>
2. <span data-ttu-id="cf9d5-127">Buď dokončení [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md), nebo stáhnout hotová aplikace z [galerie kódů MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span><span class="sxs-lookup"><span data-stu-id="cf9d5-127">Either complete the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md), or download the completed application from [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).</span></span>
3. <span data-ttu-id="cf9d5-128">Pokud používáte verzi stažení aplikace Začínáme, otevřete **Konzola správce balíčků** a klikněte na tlačítko **obnovení** přidejte balíček SignalR do projektu.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-128">If you are using the download version of the Getting Started application, open **Package Manager Console** and click **Restore** to add the SignalR package to the project.</span></span>

    ![Obnovení balíčků](unit-testing-signalr-applications/_static/image1.png)
4. <span data-ttu-id="cf9d5-130">Přidáte projekt do řešení pro testování částí.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-130">Add a project to the solution for the unit test.</span></span> <span data-ttu-id="cf9d5-131">Klikněte pravým tlačítkem na řešení v **Průzkumníku řešení** a vyberte **přidat**, **nový projekt...** . V části **C#** uzlu, vyberte **Windows** uzlu.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-131">Right-click your solution in **Solution Explorer** and select **Add**, **New Project...**. Under the **C#** node, select the **Windows** node.</span></span> <span data-ttu-id="cf9d5-132">Vyberte **třídy knihovny**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-132">Select **Class Library**.</span></span> <span data-ttu-id="cf9d5-133">Název nového projektu **TestLibrary** a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-133">Name the new project **TestLibrary** and click **OK**.</span></span>

    ![Vytvořit testovací knihovny](unit-testing-signalr-applications/_static/image2.png)
5. <span data-ttu-id="cf9d5-135">Přidáte odkaz v projektu knihovny testu do projektu SignalRChat.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-135">Add a reference in the test library project to the SignalRChat project.</span></span> <span data-ttu-id="cf9d5-136">Klikněte pravým tlačítkem myši **TestLibrary** projektu a vyberte **přidat**, **odkaz...** . Vyberte **projekty** pod uzlem **řešení** uzlu a zkontrolujte **SignalRChat**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-136">Right-click the **TestLibrary** project and select **Add**, **Reference...**. Select the **Projects** node under the **Solution** node, and check **SignalRChat**.</span></span> <span data-ttu-id="cf9d5-137">Click **OK**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-137">Click **OK**.</span></span>

    ![Přidat odkaz na projekt](unit-testing-signalr-applications/_static/image3.png)
6. <span data-ttu-id="cf9d5-139">Přidat SignalR, Moq a XUnit balíčky, do kterých **TestLibrary** projektu.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-139">Add the SignalR, Moq, and XUnit packages to the **TestLibrary** project.</span></span> <span data-ttu-id="cf9d5-140">V **Konzola správce balíčků**, nastavte **výchozí projekt** rozevírací k **TestLibrary**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-140">In the **Package Manager Console**, set the **Default Project** dropdown to **TestLibrary**.</span></span> <span data-ttu-id="cf9d5-141">V okně konzoly spusťte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="cf9d5-141">Run the following commands in the console window:</span></span>

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![Instalovat balíčky](unit-testing-signalr-applications/_static/image4.png)
7. <span data-ttu-id="cf9d5-143">Vytvoření testovacího souboru.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-143">Create the test file.</span></span> <span data-ttu-id="cf9d5-144">Klikněte pravým tlačítkem myši **TestLibrary** projektu a klikněte na tlačítko **přidat...** , **Třída**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-144">Right-click the **TestLibrary** project and click **Add...**, **Class**.</span></span> <span data-ttu-id="cf9d5-145">Pojmenujte novou třídu **Tests.cs**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-145">Name the new class **Tests.cs**.</span></span>
8. <span data-ttu-id="cf9d5-146">Obsah Tests.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-146">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    <span data-ttu-id="cf9d5-147">Ve výše uvedeném kódu, je vytvořený pomocí testovacího klienta `Mock` objektu z [Moq](https://github.com/Moq/moq4) knihovny typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (v SignalR 2.1 přiřadit `dynamic` pro typ parametr.) `IHubCallerConnectionContext` Rozhraní je objekt proxy, pomocí kterého můžete volat metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-147">In the code above, a test client is created using the `Mock` object from the [Moq](https://github.com/Moq/moq4) library, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span> <span data-ttu-id="cf9d5-148">`broadcastMessage` Funkce je pak definované pro imitované klienta tak, aby ji můžete volat `ChatHub` třídy.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-148">The `broadcastMessage` function is then defined for the mock client so that it can be called by the `ChatHub` class.</span></span> <span data-ttu-id="cf9d5-149">Modul testu pak zavolá `Send` metodu `ChatHub` třída, která volá mocked `broadcastMessage` funkce.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-149">The test engine then calls the `Send` method of the `ChatHub` class, which in turn calls the mocked `broadcastMessage` function.</span></span>
9. <span data-ttu-id="cf9d5-150">Sestavte řešení stisknutím **F6**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-150">Build the solution by pressing **F6**.</span></span>
10. <span data-ttu-id="cf9d5-151">Spuštění testování částí.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-151">Run the unit test.</span></span> <span data-ttu-id="cf9d5-152">V sadě Visual Studio, vyberte **Test**, **Windows**, **Průzkumníka testů**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-152">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="cf9d5-153">V okně Průzkumníka testů, klikněte pravým tlačítkem na **HubsAreMockableViaDynamic** a vyberte **spuštění testů vybrané**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-153">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Průzkumník testů](unit-testing-signalr-applications/_static/image5.png)
11. <span data-ttu-id="cf9d5-155">Ověřte, že test proběhl úspěšně kontrolou dolním podokně, v okně Průzkumníka testů.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-155">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="cf9d5-156">Okno se zobrazí, že test proběhl úspěšně.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-156">The window will show that the test passed.</span></span>

    ![Test proběhl úspěšně.](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a><span data-ttu-id="cf9d5-158">Podle typu testování částí</span><span class="sxs-lookup"><span data-stu-id="cf9d5-158">Unit testing by type</span></span>

<span data-ttu-id="cf9d5-159">V této části přidáte testu pro vytvořené v aplikaci [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí rozhraní, které obsahuje metodu má být testována.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-159">In this section, you'll add a test for the application created in the [Getting Started tutorial](../getting-started/tutorial-getting-started-with-signalr.md) using an interface that contains the method to be tested.</span></span>

1. <span data-ttu-id="cf9d5-160">Dokončete kroky 1-7 v [testování částí pomocí dynamické](#dynamic) kurzu výše.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-160">Complete steps 1-7 in the [Unit testing with Dynamic](#dynamic) tutorial above.</span></span>
2. <span data-ttu-id="cf9d5-161">Obsah Tests.cs nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-161">Replace the contents of Tests.cs with the following code.</span></span>

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    <span data-ttu-id="cf9d5-162">Ve výše uvedeném kódu, se vytvoří rozhraní definování podpis `broadcastMessage` metoda, pro který modul testu vytvoří imitované klienta.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-162">In the code above, an interface is created defining the signature of the `broadcastMessage` method for which the test engine will create a mock client.</span></span> <span data-ttu-id="cf9d5-163">Imitované klienta je poté jste vytvořili pomocí `Mock` objektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (v SignalR 2.1 přiřadit `dynamic` pro parametr typu.) `IHubCallerConnectionContext` Rozhraní je objekt proxy, pomocí kterého můžete volat metody na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-163">A mock client is then created using the `Mock` object, of type [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (in SignalR 2.1, assign `dynamic` for the type parameter.) The `IHubCallerConnectionContext` interface is the proxy object with which you invoke methods on the client.</span></span>

    <span data-ttu-id="cf9d5-164">Test potom vytvoří instanci `ChatHub`a potom vytvoří na cvičnou verzi `broadcastMessage` metoda, která zase se vyvolá při volání `Send` metoda rozbočovače.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-164">The test then creates an instance of `ChatHub`, and then creates a mock version of the `broadcastMessage` method, which in turn is invoked by calling the `Send` method on the hub.</span></span>
3. <span data-ttu-id="cf9d5-165">Sestavte řešení stisknutím **F6**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-165">Build the solution by pressing **F6**.</span></span>
4. <span data-ttu-id="cf9d5-166">Spuštění testování částí.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-166">Run the unit test.</span></span> <span data-ttu-id="cf9d5-167">V sadě Visual Studio, vyberte **Test**, **Windows**, **Průzkumníka testů**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-167">In Visual Studio, select **Test**, **Windows**, **Test Explorer**.</span></span> <span data-ttu-id="cf9d5-168">V okně Průzkumníka testů, klikněte pravým tlačítkem na **HubsAreMockableViaDynamic** a vyberte **spuštění testů vybrané**.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-168">In the Test Explorer window, right-click **HubsAreMockableViaDynamic** and select **Run Selected Tests**.</span></span>

    ![Průzkumník testů](unit-testing-signalr-applications/_static/image7.png)
5. <span data-ttu-id="cf9d5-170">Ověřte, že test proběhl úspěšně kontrolou dolním podokně, v okně Průzkumníka testů.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-170">Verify that the test passed by checking the lower pane in the Test Explorer window.</span></span> <span data-ttu-id="cf9d5-171">Okno se zobrazí, že test proběhl úspěšně.</span><span class="sxs-lookup"><span data-stu-id="cf9d5-171">The window will show that the test passed.</span></span>

    ![Test proběhl úspěšně.](unit-testing-signalr-applications/_static/image8.png)
