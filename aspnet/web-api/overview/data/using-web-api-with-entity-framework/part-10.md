---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publikujte aplikaci do Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: cc8a9199144e9fac041435938ea8899374ea199f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867811"
---
<a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="2b17c-102">Publikujte aplikaci do Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2b17c-102">Publish the App to Azure Azure App Service</span></span>
====================
<span data-ttu-id="2b17c-103">podle [Wasson Jan](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="2b17c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="2b17c-104">Stáhněte si dokončený projekt</span><span class="sxs-lookup"><span data-stu-id="2b17c-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="2b17c-105">Jako poslední krok budete publikovat aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="2b17c-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="2b17c-106">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="2b17c-107">Kliknutím na tlačítko **publikovat** vyvolá **Publikovat Web** dialogové okno.</span><span class="sxs-lookup"><span data-stu-id="2b17c-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="2b17c-108">Pokud je zaškrtnuto **hostitel v cloudu** při prvním vytvoření projektu a potom připojení a nastavení jsou již nakonfigurována.</span><span class="sxs-lookup"><span data-stu-id="2b17c-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="2b17c-109">V takovém případě stačí kliknout na **nastavení** kartě a zkontrolujte &quot;spustit migrace Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="2b17c-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="2b17c-110">(Pokud nebyl zkontrolujte **hostitel v cloudu** na začátku, postupujte podle pokynů v [další části](#new-website).)</span><span class="sxs-lookup"><span data-stu-id="2b17c-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="2b17c-111">Chcete-li nasadit aplikaci, klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="2b17c-112">Publikování postup v lze zobrazit **aktivity publikování webového** okno.</span><span class="sxs-lookup"><span data-stu-id="2b17c-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="2b17c-113">(Z **zobrazení** nabídce vyberte možnost **ostatní okna**, pak vyberte **aktivity publikování webového**.)</span><span class="sxs-lookup"><span data-stu-id="2b17c-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="2b17c-114">Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč na adresu URL nasazené webové stránky a aplikace, kterou jste vytvořili je nyní spuštěna v cloudu.</span><span class="sxs-lookup"><span data-stu-id="2b17c-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="2b17c-115">Adresu URL v adresním řádku prohlížeče ukazuje, že je právě načítán lokality z Internetu.</span><span class="sxs-lookup"><span data-stu-id="2b17c-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="2b17c-116">Nasazení na nový web</span><span class="sxs-lookup"><span data-stu-id="2b17c-116">Deploying to a New Website</span></span>

<span data-ttu-id="2b17c-117">Pokud jste nezaškrtli políčko **hostitel v cloudu** při prvním vytvoření projektu, můžete nakonfigurovat nové webové aplikace teď.</span><span class="sxs-lookup"><span data-stu-id="2b17c-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="2b17c-118">V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="2b17c-119">Vyberte **profil** a klikněte na **webové stránky společnosti Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="2b17c-120">Pokud nejste aktuálně přihlášení do Azure, zobrazí se výzva k přihlášení.</span><span class="sxs-lookup"><span data-stu-id="2b17c-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="2b17c-121">V **existující weby** dialogové okno, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="2b17c-122">Zadejte název webového serveru.</span><span class="sxs-lookup"><span data-stu-id="2b17c-122">Enter a site name.</span></span> <span data-ttu-id="2b17c-123">Vyberte předplatné Azure a oblast.</span><span class="sxs-lookup"><span data-stu-id="2b17c-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="2b17c-124">V části **databázový server**, vyberte **vytvořit nový Server**, nebo vyberte existující server.</span><span class="sxs-lookup"><span data-stu-id="2b17c-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="2b17c-125">Klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="2b17c-126">Klikněte **nastavení** kartě a zkontrolujte &quot;spustit migrace Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="2b17c-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="2b17c-127">Pak klikněte na tlačítko **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="2b17c-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="2b17c-128">Předchozí</span><span class="sxs-lookup"><span data-stu-id="2b17c-128">Previous</span></span>](part-9.md)
