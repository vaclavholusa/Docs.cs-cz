---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: "Vytváření kapitoly soubory ke stažení pro EF 5 MVC 4 kurzy | Microsoft Docs"
author: Rick-Anderson
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="39ddb-103">Vytváření kapitoly soubory ke stažení pro EF 5 MVC 4 kurzy</span><span class="sxs-lookup"><span data-stu-id="39ddb-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="39ddb-104">Podle [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="39ddb-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="39ddb-105">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="39ddb-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="39ddb-106">Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="39ddb-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="39ddb-107">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="39ddb-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="39ddb-108">Vytváření kapitoly stahování</span><span class="sxs-lookup"><span data-stu-id="39ddb-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="39ddb-109">Stáhněte a rozbalte soubor zip ukázkový projekt.</span><span class="sxs-lookup"><span data-stu-id="39ddb-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="39ddb-110">V rozbalené stažení balíčku najdete další zip soubory, jeden pro dokončení každé kapitoly.</span><span class="sxs-lookup"><span data-stu-id="39ddb-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="39ddb-111">Klikněte pravým tlačítkem na soubor zip požadované, klikněte na tlačítko **vlastnosti**a klikněte na **Odblokovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="39ddb-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="39ddb-112">Rozbalte soubor.</span><span class="sxs-lookup"><span data-stu-id="39ddb-112">Unzip the file.</span></span>
4. <span data-ttu-id="39ddb-113">Dvakrát klikněte *CUx.sln* soubor spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39ddb-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="39ddb-114">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="39ddb-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="39ddb-115">V balíček správce konzoly (pomocí PMC), klikněte na tlačítko **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="39ddb-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="39ddb-116">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39ddb-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="39ddb-117">Restartujte Visual Studio, otevřete soubor řešení uzavřený v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="39ddb-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="39ddb-118">V balíček správce konzoly (pomocí PMC), zadejte `Update-Database` příkaz:</span><span class="sxs-lookup"><span data-stu-id="39ddb-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="39ddb-119">Pokud dojde k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="39ddb-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="39ddb-120">*Termín 'Update-Database' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud byl zahrnut cestu, ověřte, zda je cesta správná a zkuste to znovu.*</span><span class="sxs-lookup"><span data-stu-id="39ddb-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="39ddb-121">Ukončete a restartujte Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="39ddb-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="39ddb-122">Spustí každý migrace a potom se spustí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="39ddb-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="39ddb-123">Teď můžete spustit aplikace.</span><span class="sxs-lookup"><span data-stu-id="39ddb-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[<span data-ttu-id="39ddb-124">Předchozí</span><span class="sxs-lookup"><span data-stu-id="39ddb-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
