---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: "Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013 | Microsoft Docs"
author: tfitzmac
description: "Generování uživatelského rozhraní ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="8f9bc-103">Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8f9bc-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="8f9bc-104">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="8f9bc-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="8f9bc-105">Generování uživatelského rozhraní ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="8f9bc-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="8f9bc-106">Overview</span></span>

<span data-ttu-id="8f9bc-107">Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="8f9bc-108">Visual Studio 2013 zahrnuje generátory předem nainstalovaná kódu pro projekty MVC a webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="8f9bc-109">Pokud chcete rychle přidáte kód, který komunikuje s datové modely přidáte do projektu generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="8f9bc-110">Pomocí generování uživatelského rozhraní, můžete snížit množství času k vývoji operace standardní dat ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="8f9bc-111">Ve výchozím nastavení Visual Studio 2013 nepodporuje generování kódu pro projekt webové formuláře, ale můžete generování uživatelského rozhraní s webovými formuláři přidávání k projektu závislosti MVC nebo instalaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="8f9bc-112">Níže jsou uvedeny obou přístupů.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-112">Both approaches are shown below.</span></span>

<span data-ttu-id="8f9bc-113">Visual Studio 2013 Update 2 (aktuálně RC) poskytuje možnost rozšířit ASP.NET generování uživatelského rozhraní pro splnění požadavků váš scénář.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="8f9bc-114">Pomocí této funkce můžete vytvořit šablonu přizpůsobené generování uživatelského rozhraní a přidejte ho do dialogového okna Přidat vygenerované uživatelské rozhraní nového.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="8f9bc-115">V rámci vlastní šablony zadejte kód, který se vygeneruje, když přidání vygenerované položky.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="8f9bc-116">Další informace najdete v tématu [vytváření vlastní Scaffolder pro sadu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f9bc-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8f9bc-117">Prerequisites</span></span>

<span data-ttu-id="8f9bc-118">Pokud chcete používat ASP.NET generování uživatelského rozhraní, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="8f9bc-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="8f9bc-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="8f9bc-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="8f9bc-120">Webové nástroje pro vývojáře (součást instalace Visual Studio 2013 výchozí)</span><span class="sxs-lookup"><span data-stu-id="8f9bc-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="8f9bc-121">Webové rozhraní ASP.NET a nástroje 2013 (součást instalace Visual Studio 2013 výchozí)</span><span class="sxs-lookup"><span data-stu-id="8f9bc-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="8f9bc-122">Přidání vygenerované položky MVC nebo webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="8f9bc-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="8f9bc-123">Chcete-li přidat vygenerované uživatelské rozhraní, klikněte pravým tlačítkem na projekt nebo složku v rámci projektu a vyberte **přidat** – **novou vygenerovanou položku**, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Přidat vygenerované uživatelské rozhraní položky](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="8f9bc-125">Z **přidat vygenerované uživatelské rozhraní** okně vyberte typ vygenerované uživatelské rozhraní pro přidání.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Vyberte typ vygenerované uživatelské rozhraní](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="8f9bc-127">**Přidat kontroler** okno vám dává možnost vybrat možnosti generování řadič, včetně toho, jestli chcete používat nové funkce asynchronní z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Přidání kontroleru](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="8f9bc-129">Pro váš scénář se vytvoří příslušné třídy a stránky.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="8f9bc-130">Například následující obrázek ukazuje MVC jsou řadič MVC a zobrazení, které byly vytvořeny pomocí generování uživatelského rozhraní pro třídu modelu s názvem filmy.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Vytvořené soubory](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="8f9bc-132">Přidání vygenerované položky do webových formulářů</span><span class="sxs-lookup"><span data-stu-id="8f9bc-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="8f9bc-133">Pokud chcete přidat generování uživatelského rozhraní, která generuje kód webových formulářů, musíte nainstalovat rozšíření pro Visual Studio nebo přidat závislosti MVC.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="8f9bc-134">Níže jsou uvedeny oba přístupy, ale potřebujete proveďte jednu z těchto přístupů.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="8f9bc-135">Webové formuláře generování uživatelského rozhraní rozšíření</span><span class="sxs-lookup"><span data-stu-id="8f9bc-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="8f9bc-136">Můžete nainstalovat rozšíření sady Visual Studio, které vám umožní pomocí generování uživatelského rozhraní s projektem webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="8f9bc-137">V sadě Visual Studio, vyberte **nástroje** a potom **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="8f9bc-138">Z toto dialogové okno hledání Galerii Visual Studio pro **Web Forms generování uživatelského rozhraní**.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![Instalace webové formuláře generování uživatelského rozhraní](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="8f9bc-140">Další informace najdete v tématu [Web Forms generování uživatelského rozhraní](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="8f9bc-141">Závislosti MVC</span><span class="sxs-lookup"><span data-stu-id="8f9bc-141">MVC Dependencies</span></span>

<span data-ttu-id="8f9bc-142">Chcete-li přidat závislosti MVC, vyberte **přidat** - **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="8f9bc-143">V okně Přidat vygenerované uživatelské rozhraní, vyberte **závislosti MVC**, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Přidat závislosti MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="8f9bc-145">Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="8f9bc-146">Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="8f9bc-147">Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="8f9bc-148">Chcete-li snadno použít generování uživatelského rozhraní, vyberte úplné závislosti.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-148">To easily use scaffolding, select Full dependencies.</span></span>

![Vyberte úplný závislosti](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="8f9bc-150">Po přidání závislostí, zobrazí se **readme.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="8f9bc-151">Pečlivě postupujte podle pokynů v tomto souboru a ujistěte se, že váš projekt funguje správně.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="8f9bc-152">Pokud jste dokončili kroky v souboru readme.txt, můžete přidat nové vygenerované položky, jak je uvedeno v předchozí části o MVC a webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="8f9bc-153">Automaticky generované zobrazení a řadič bude fungovat správně v rámci projektu.</span><span class="sxs-lookup"><span data-stu-id="8f9bc-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="8f9bc-154">Kurzy</span><span class="sxs-lookup"><span data-stu-id="8f9bc-154">Tutorials</span></span>

<span data-ttu-id="8f9bc-155">Vytvoření vlastní scaffolder naleznete v tématu [vytváření vlastní Scaffolder pro sadu Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="8f9bc-156">Pokud chcete přizpůsobit generované soubory, najdete v části [postup přizpůsobení generované soubory z tohoto dialogového okna novou vygenerovanou položku](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="8f9bc-157">Příklad pomocí generování uživatelského rozhraní s **Database First vývoj**, najdete v části [EF Database First s architekturou ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="8f9bc-158">Příklad pomocí generování uživatelského rozhraní v **MVC** projektu najdete v tématu [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="8f9bc-159">Příklad pomocí generování uživatelského rozhraní v **webového rozhraní API** projektu najdete v tématu [vytvořit rozhraní REST API s atribut směrování ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="8f9bc-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
