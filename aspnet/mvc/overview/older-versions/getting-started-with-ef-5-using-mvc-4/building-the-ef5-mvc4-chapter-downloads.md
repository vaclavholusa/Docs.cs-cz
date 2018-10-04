---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Vytváření v kapitole soubory ke stažení pro EF 5 MVC 4 kurzy | Dokumentace Microsoftu
author: Rick-Anderson
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: fa018ea12929efa742a323d96938d372b634fdd7
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577060"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="05f15-103">Vytváření v kapitole soubory ke stažení pro EF 5 MVC 4 kurzy</span><span class="sxs-lookup"><span data-stu-id="05f15-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="05f15-104">Podle [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="05f15-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="05f15-105">Stáhnout dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="05f15-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="05f15-106">Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="05f15-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="05f15-107">Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="05f15-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="05f15-108">Stažení jednotlivých kapitol</span><span class="sxs-lookup"><span data-stu-id="05f15-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="05f15-109">Stáhněte a rozbalte soubor zip ukázkový projekt.</span><span class="sxs-lookup"><span data-stu-id="05f15-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="05f15-110">V balíčku rozzipovaný ke stažení najdete zip další soubory, jeden pro dokončení každá kapitola.</span><span class="sxs-lookup"><span data-stu-id="05f15-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="05f15-111">Klikněte pravým tlačítkem na požadovaný komprimovaného souboru, klikněte na tlačítko **vlastnosti**a klikněte na tlačítko **Odblokovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="05f15-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="05f15-112">Rozbalte tento soubor.</span><span class="sxs-lookup"><span data-stu-id="05f15-112">Unzip the file.</span></span>
4. <span data-ttu-id="05f15-113">Dvakrát klikněte *CUx.sln* souboru ke spuštění sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05f15-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="05f15-114">Z **nástroje** nabídky, klikněte na tlačítko **Správce balíčků knihoven**, pak **Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="05f15-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="05f15-115">V balíčku správce konzoly (konzolu PMC), klikněte na tlačítko **obnovení**.</span><span class="sxs-lookup"><span data-stu-id="05f15-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="05f15-116">Ukončení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05f15-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="05f15-117">Restartujte sadu Visual Studio, otevřete soubor řešení uzavřeny v předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="05f15-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="05f15-118">V balíčku správce konzoly (konzolu PMC), zadejte `Update-Database` příkaz:</span><span class="sxs-lookup"><span data-stu-id="05f15-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="05f15-119">Pokud se zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="05f15-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="05f15-120">*Termín 'Aktualizace databáze' nebyl rozpoznán jako název rutiny, funkce, soubor skriptu nebo spustitelného programu. Zkontrolujte, zda název, nebo pokud cesty byl zahrnut, ověřte správnost cesty a zkuste to znovu.*</span><span class="sxs-lookup"><span data-stu-id="05f15-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="05f15-121">Ukončete a restartujte aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="05f15-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="05f15-122">Bude spouštět každou migraci a potom spustí metodu počáteční hodnoty.</span><span class="sxs-lookup"><span data-stu-id="05f15-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="05f15-123">Nyní můžete spustit aplikaci.</span><span class="sxs-lookup"><span data-stu-id="05f15-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="05f15-124">Předchozí</span><span class="sxs-lookup"><span data-stu-id="05f15-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
