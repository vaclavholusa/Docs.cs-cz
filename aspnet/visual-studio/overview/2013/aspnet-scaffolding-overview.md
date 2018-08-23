---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013 | Dokumentace Microsoftu
author: tfitzmac
description: Generování uživatelského rozhraní technologie ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755246"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="d1ccc-103">Generování uživatelského rozhraní ASP.NET v sadě Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d1ccc-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="d1ccc-104">podle [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d1ccc-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d1ccc-105">Generování uživatelského rozhraní technologie ASP.NET je nová funkce, která je zahrnutá v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="d1ccc-106">Přehled</span><span class="sxs-lookup"><span data-stu-id="d1ccc-106">Overview</span></span>

<span data-ttu-id="d1ccc-107">ASP.NET generování uživatelského rozhraní je architektura generování kódu pro webové aplikace ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="d1ccc-108">Visual Studio 2013 obsahuje generátory předinstalované kódu pro projekty MVC a webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="d1ccc-109">Generování uživatelského rozhraní do svého projektu přidat, pokud chcete rychle přidat kód, který komunikuje s datovými modely.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="d1ccc-110">Pomocí generování uživatelského rozhraní můžete snížit množství doba k rozvoji standardních datových operací ve vašem projektu.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="d1ccc-111">Ve výchozím nastavení Visual Studio 2013 nepodporuje generování kódu pro projekt webových formulářů, ale generování uživatelského rozhraní můžete použít s webovými formuláři do projektu přidat závislosti MVC nebo instalaci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="d1ccc-112">Oba přístupy jsou uvedeny níže.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-112">Both approaches are shown below.</span></span>

<span data-ttu-id="d1ccc-113">Visual Studio 2013 Update 2 (aktuálně RC) poskytuje možnost rozšíření ASP.NET generování uživatelského rozhraní pro splnění požadavků vašeho scénáře.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="d1ccc-114">Pomocí této funkce můžete vytvořit šablonu přizpůsobené generování uživatelského rozhraní a přidejte ho do dialogového okna Přidat vygenerované uživatelské rozhraní nového.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="d1ccc-115">V rámci vlastní šablony zadejte kód, který se generuje při přidání vygenerované položky.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="d1ccc-116">Další informace najdete v tématu [vytváří generátor vlastní pro Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1ccc-117">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d1ccc-117">Prerequisites</span></span>

<span data-ttu-id="d1ccc-118">Pokud chcete používat ASP.NET generování uživatelského rozhraní, musíte mít:</span><span class="sxs-lookup"><span data-stu-id="d1ccc-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="d1ccc-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="d1ccc-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="d1ccc-120">Webové nástroje pro vývojáře (součást výchozí instalace sady Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="d1ccc-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="d1ccc-121">ASP.NET Web Frameworks and Tools 2013 (součást výchozí instalace sady Visual Studio 2013)</span><span class="sxs-lookup"><span data-stu-id="d1ccc-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="d1ccc-122">Přidání vygenerované položky MVC nebo webového rozhraní API</span><span class="sxs-lookup"><span data-stu-id="d1ccc-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="d1ccc-123">Chcete-li přidat vygenerované uživatelské rozhraní, klikněte pravým tlačítkem na projekt nebo složku v rámci projektu a vyberte **přidat** – **novou vygenerovanou položku**, jak je znázorněno na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![Přidat vygenerované uživatelské rozhraní položky](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="d1ccc-125">Z **přidat vygenerované uživatelské rozhraní** okna, vyberte typ vygenerované uživatelské rozhraní pro přidání.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![Vyberte typ vygenerované uživatelské rozhraní](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="d1ccc-127">**Přidat kontroler** okno poskytuje možnost vybrat možnosti pro vytvoření kontroleru, včetně toho, jestli chcete používat nové asynchronní funkce z Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Přidání kontroleru](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="d1ccc-129">Příslušných tříd a stránky jsou vytvářeny pro váš scénář.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="d1ccc-130">Například následující obrázek ukazuje kontroler MVC a zobrazení, které byly vytvořené pomocí generování uživatelského rozhraní pro třídu modelu s názvem videa.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Vytvořené soubory](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="d1ccc-132">Přidání vygenerované položky do webových formulářů</span><span class="sxs-lookup"><span data-stu-id="d1ccc-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="d1ccc-133">Pokud chcete přidat generování uživatelského rozhraní, který generuje kód webových formulářů, musíte nainstalovat rozšíření sady Visual Studio nebo přidat závislosti MVC.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="d1ccc-134">Oba přístupy jsou uvedeny níže, ale je potřeba jenom proveďte jednu z těchto přístupů.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="d1ccc-135">Webové formuláře, generování uživatelského rozhraní rozšíření</span><span class="sxs-lookup"><span data-stu-id="d1ccc-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="d1ccc-136">Můžete nainstalovat rozšíření sady Visual Studio, který vám umožní používat generování uživatelského rozhraní se projekt webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="d1ccc-137">V sadě Visual Studio, vyberte **nástroje** a potom **rozšíření a aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="d1ccc-138">Z tohoto dialogového okna Vyhledat Galerie sady Visual Studio pro **generování uživatelského rozhraní webových formulářů**.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![instalace generování uživatelského rozhraní webových formulářů](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="d1ccc-140">Další informace najdete v tématu [generování uživatelského rozhraní webových formulářů](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="d1ccc-141">Závislosti MVC</span><span class="sxs-lookup"><span data-stu-id="d1ccc-141">MVC Dependencies</span></span>

<span data-ttu-id="d1ccc-142">Chcete-li přidat závislosti MVC, **přidat** - **novou vygenerovanou položku**.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="d1ccc-143">V okně Přidat vygenerované uživatelské rozhraní vyberte **závislosti MVC**, jak je znázorněno níže.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![Přidat závislosti MVC](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="d1ccc-145">Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="d1ccc-146">Pokud vyberete minimální, pouze balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do vašeho projektu.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="d1ccc-147">Pokud vyberete možnost Úplná minimální závislosti jsou přidány, a také požadované soubory obsahu pro projekt MVC.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="d1ccc-148">Snadno používat generování uživatelského rozhraní, vyberte úplný závislosti.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-148">To easily use scaffolding, select Full dependencies.</span></span>

![Vyberte úplný závislosti](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="d1ccc-150">Po přidání závislostí, uvidíte **readme.txt** souboru.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="d1ccc-151">Pečlivě postupujte podle pokynů v tomto souboru a ujistěte se, že projekt pracuje správně.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="d1ccc-152">Po dokončení kroků v souboru readme.txt můžete přidat nová vygenerovaná položka, jak je znázorněno v předchozí části o MVC a webového rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="d1ccc-153">Automaticky generované zobrazení a kontroler bude fungovat správně v rámci svého projektu.</span><span class="sxs-lookup"><span data-stu-id="d1ccc-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="d1ccc-154">Kurzy</span><span class="sxs-lookup"><span data-stu-id="d1ccc-154">Tutorials</span></span>

<span data-ttu-id="d1ccc-155">Vytvoření vlastní generátor najdete v tématu [vytváří generátor vlastní pro Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="d1ccc-156">Přizpůsobit generované soubory, přečtěte si článek [přizpůsobení generované soubory z tohoto dialogového okna novou vygenerovanou položku](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="d1ccc-157">Příklad použití generování uživatelského rozhraní s **Database First vývoj**, naleznete v tématu [EF Database First s ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="d1ccc-158">Příklad použití generování uživatelského rozhraní v **MVC** projektu naleznete v tématu [Začínáme s ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="d1ccc-159">Příklad použití generování uživatelského rozhraní v **webového rozhraní API** projektu naleznete v tématu [vytvořit rozhraní REST API se směrováním atributů ve webovém rozhraní API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="d1ccc-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
