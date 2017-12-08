---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "ASP.NET Identity: Pomocí MySQL úložiště pomocí zprostředkovatele MySQL EntityFramework (C#) | Microsoft Docs"
author: maumar
description: "V tomto kurzu se dozvíte, jak nahradit výchozího mechanismu úložiště dat pro ASP.NET Identity EntityFramework (zprostředkovatel SQL klienta) s MySQL zajištění..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: ac254abcb756d048d159a9b67967a581f35ac871
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="c5e21-103">ASP.NET Identity: Pomocí MySQL úložiště pomocí zprostředkovatele EntityFramework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="c5e21-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="c5e21-104">podle [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Roberta Mcmurrayho](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="c5e21-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="c5e21-105">V tomto kurzu se dozvíte, jak nahradit výchozího mechanismu úložiště dat pro [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) s EntityFramework (zprostředkovatel SQL klienta) s poskytovatelem MySQL.</span><span class="sxs-lookup"><span data-stu-id="c5e21-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="c5e21-106">V následujících tématech se budeme v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="c5e21-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="c5e21-107">Vytvoření databáze MySQL na Azure</span><span class="sxs-lookup"><span data-stu-id="c5e21-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="c5e21-108">Vytvoření aplikace MVC pomocí šablony Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="c5e21-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="c5e21-109">Konfigurace objektu EntityFramework pro práci s poskytovatele databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="c5e21-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="c5e21-110">Spuštění aplikace ověření výsledků</span><span class="sxs-lookup"><span data-stu-id="c5e21-110">Running the application to verify the results</span></span>

<span data-ttu-id="c5e21-111">Na konci tohoto kurzu budete mít aplikace MVC pomocí ASP.NET Identity úložiště používající databázi MySQL, která je hostovaná v Azure.</span><span class="sxs-lookup"><span data-stu-id="c5e21-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="c5e21-112">Vytvoření instance databáze MySQL na Azure</span><span class="sxs-lookup"><span data-stu-id="c5e21-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="c5e21-113">Přihlaste se k [portál Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="c5e21-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="c5e21-114">Klikněte na tlačítko **nový** v dolní části stránky a potom vyberte **úložiště**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="c5e21-115">V **zvolte a rozšíření** průvodce, vyberte **databáze MySQL ClearDB**a pak klikněte na tlačítko **Další** šipky v dolní části rámečku:</span><span class="sxs-lookup"><span data-stu-id="c5e21-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="c5e21-116">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-116">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-117">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="c5e21-118">Ponechte výchozí **volné** plánování, změnit **název** k **IdentityMySQLDatabase**, vyberte oblast, která je nejblíže je a pak klikněte na tlačítko **Další** šipku v dolní části rámečku:</span><span class="sxs-lookup"><span data-stu-id="c5e21-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="c5e21-119">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-119">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-120">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="c5e21-121">Klikněte **nákupu** zaškrtnutí vytvoření databáze.</span><span class="sxs-lookup"><span data-stu-id="c5e21-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="c5e21-122">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-122">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-123">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="c5e21-124">Po vytvoření databáze, můžete spravovat z **doplňky** kartě v portálu pro správu.</span><span class="sxs-lookup"><span data-stu-id="c5e21-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="c5e21-125">Chcete-li načíst informace o připojení pro vaši databázi, klikněte na tlačítko **informace o připojení** v dolní části stránky:</span><span class="sxs-lookup"><span data-stu-id="c5e21-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="c5e21-126">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-126">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-127">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="c5e21-128">Zkopírujte připojovací řetězec kliknutím na tlačítko Kopírovat, pokud pomocí **CONNECTIONSTRING** pole a uložit ho; tyto informace dále v tomto kurzu budete používat pro aplikace MVC:</span><span class="sxs-lookup"><span data-stu-id="c5e21-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="c5e21-129">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-129">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-130">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="c5e21-131">Vytvoření projektu aplikace MVC</span><span class="sxs-lookup"><span data-stu-id="c5e21-131">Creating an MVC application project</span></span>

<span data-ttu-id="c5e21-132">Pokud chcete provést kroky v této části kurzu, bude nejprve musíte nainstalovat [Visual Studio Express 2013 pro Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="c5e21-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="c5e21-133">Po instalaci sady Visual Studio vytvořte nový projekt aplikace MVC pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="c5e21-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="c5e21-134">Otevřete Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="c5e21-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="c5e21-135">Klikněte na tlačítko **nový projekt** z **spustit** stránky, nebo můžete kliknout na tlačítko **soubor** nabídce a potom **nový projekt**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="c5e21-136">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-136">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-137">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="c5e21-138">Když **nový projekt** se zobrazí dialogové okno, rozbalte položku **Visual C#** v seznamu šablon, pak klikněte na **webové**a vyberte **webové aplikace ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="c5e21-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="c5e21-139">Název projektu **IdentityMySQLDemo** a pak klikněte na **OK**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="c5e21-140">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-140">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-141">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="c5e21-142">V **nový projekt ASP.NET** dialogovém okně, vyberte **MVC** šablonyPomocí výchozí možnosti; tím se konfigurace **jednotlivé uživatelské účty** jako metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="c5e21-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="c5e21-143">Klikněte na tlačítko **OK**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="c5e21-144">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-144">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-145">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="c5e21-146">Konfigurace objektu EntityFramework pro práci s databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="c5e21-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="c5e21-147">Aktualizace sestavení Entity Framework pro váš projekt</span><span class="sxs-lookup"><span data-stu-id="c5e21-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="c5e21-148">Aplikace MVC, který byl vytvořen z šablony sady Visual Studio 2013 obsahuje odkaz na [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) balíček, ale musí byla aktualizace do tohoto sestavení od jeho vydání obsahujících významné vylepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="c5e21-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="c5e21-149">Chcete-li použít tyto nejnovější aktualizace ve vaší aplikaci, použijte následující kroky.</span><span class="sxs-lookup"><span data-stu-id="c5e21-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="c5e21-150">Otevřete projekt MVC v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c5e21-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="c5e21-151">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="c5e21-152">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-152">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-153">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="c5e21-154">**Konzola správce balíčků** se zobrazí v dolní části sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c5e21-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="c5e21-155">Typ &quot; **balíček aktualizace EntityFramework** &quot; a stiskněte klávesu Enter:</span><span class="sxs-lookup"><span data-stu-id="c5e21-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="c5e21-156">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-156">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-157">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="c5e21-158">Nainstalujte zprostředkovatele MySQL pro EntityFramework</span><span class="sxs-lookup"><span data-stu-id="c5e21-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="c5e21-159">Aby EntityFramework pro připojení k databázi MySQL budete muset nainstalovat poskytovatele MySQL.</span><span class="sxs-lookup"><span data-stu-id="c5e21-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="c5e21-160">Chcete-li to provést, otevřete **Konzola správce balíčků** a typ &quot; **MySql.Data.Entity Install-Package - předběžné**&quot;, a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="c5e21-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="c5e21-161">Toto je předběžná verze sestavení a jako takový ho může obsahovat chyby.</span><span class="sxs-lookup"><span data-stu-id="c5e21-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="c5e21-162">Předběžná verze zprostředkovatele byste neměli používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="c5e21-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="c5e21-163">[Kliknutím na následující obrázek a rozbalte ji.]</span><span class="sxs-lookup"><span data-stu-id="c5e21-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="c5e21-164">Provádění změn konfigurace projektu v souboru Web.config vaší aplikace</span><span class="sxs-lookup"><span data-stu-id="c5e21-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="c5e21-165">V této části nakonfigurujete rozhraní Entity Framework pro použití poskytovatele MySQL, který jste právě nainstalovali zaregistrujte objekt pro vytváření zprostředkovatelů MySQL a přidejte připojovací řetězec z Azure.</span><span class="sxs-lookup"><span data-stu-id="c5e21-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="c5e21-166">Následující příklady obsahují verzi konkrétní sestavení pro MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="c5e21-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="c5e21-167">Pokud se změní verze sestavení, musíte změnit nastavení odpovídající konfiguraci se správnou verzí.</span><span class="sxs-lookup"><span data-stu-id="c5e21-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="c5e21-168">Otevřete soubor Web.config pro projekt v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c5e21-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="c5e21-169">Vyhledejte následující nastavení konfigurace, které definují výchozí zprostředkovatel databáze a objektu pro vytváření rozhraní Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="c5e21-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="c5e21-170">Nahraďte následující text, který nakonfiguruje rozhraní Entity Framework pro použití poskytovatele MySQL těchto nastavení:</span><span class="sxs-lookup"><span data-stu-id="c5e21-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="c5e21-171">Vyhledejte &lt;connectionStrings&gt; části a nahraďte ji následujícím kódem, který definuje připojovací řetězec pro vaši databázi MySQL, která je hostovaná v Azure (Všimněte si, že hodnota providerName také byla změněna z hodnoty původní):</span><span class="sxs-lookup"><span data-stu-id="c5e21-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="c5e21-172">Přidání vlastních MigrationHistory kontextu</span><span class="sxs-lookup"><span data-stu-id="c5e21-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="c5e21-173">Entity Framework Code First používá **MigrationHistory** tabulky ke sledování změn modelu a zajistit konzistenci mezi schéma databáze a konceptuálního schématu.</span><span class="sxs-lookup"><span data-stu-id="c5e21-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="c5e21-174">Ale v této tabulce nefunguje pro databázi MySQL ve výchozím nastavení protože primární klíč je příliš velká.</span><span class="sxs-lookup"><span data-stu-id="c5e21-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="c5e21-175">Chcete-li opravit tuto situaci, musíte zmenšit velikost klíče pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="c5e21-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="c5e21-176">Uděláte to tak, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="c5e21-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="c5e21-177">Informace o schématu pro tuto tabulku je zachycené v **HistoryContext**, což může být upravena jako libovolný jiný **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="c5e21-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="c5e21-178">Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlHistoryContext.cs** do projektu a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c5e21-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="c5e21-179">Potom budete muset nakonfigurovat rozhraní Entity Framework použít upravenou **HistoryContext**, namísto výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="c5e21-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="c5e21-180">Tento krok můžete provést s využitím funkce konfigurace založené na kódu.</span><span class="sxs-lookup"><span data-stu-id="c5e21-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="c5e21-181">Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlConfiguration.cs** do projektu a nahraďte jeho obsah s:</span><span class="sxs-lookup"><span data-stu-id="c5e21-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="c5e21-182">Vytváření vlastních inicializátoru objektu EntityFramework pro ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="c5e21-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="c5e21-183">MySQL zprostředkovatele, který bude uvedena v tomto kurzu aktuálně nepodporuje migraci Entity Framework, takže budete muset použít model inicializátory připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="c5e21-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="c5e21-184">Protože v tomto kurzu je pomocí instance databáze MySQL na Azure, bude třeba potřebujete vytvořit vlastní inicializátoru Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c5e21-184">Because this tutorial is using a MySQL instance on Azure, you will need need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="c5e21-185">Tento krok není vyžaduje, pokud se připojujete k instanci systému SQL Server v Azure nebo pokud používáte databázi, která je hostovaná místně.</span><span class="sxs-lookup"><span data-stu-id="c5e21-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="c5e21-186">Chcete-li vytvořit vlastní inicializátoru Entity Framework pro databázi MySQL, použijte následující kroky:</span><span class="sxs-lookup"><span data-stu-id="c5e21-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="c5e21-187">Přidat nový soubor třídy s názvem **MySqlInitializer.cs** projektu a nahraďte je obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="c5e21-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="c5e21-188">Otevřete **IdentityModels.cs** soubor pro svůj projekt, který je umístěný ve **modely** adresář a nahraďte jeho obsah následujícím textem:</span><span class="sxs-lookup"><span data-stu-id="c5e21-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="c5e21-189">Spuštění aplikace a ověřování databáze</span><span class="sxs-lookup"><span data-stu-id="c5e21-189">Running the application and verifying the database</span></span>

<span data-ttu-id="c5e21-190">Po dokončení kroků v předchozí části, měli byste otestovat vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="c5e21-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="c5e21-191">Uděláte to tak, použijte následující postup:</span><span class="sxs-lookup"><span data-stu-id="c5e21-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="c5e21-192">Stiskněte klávesu **kombinaci kláves Ctrl + F5** sestavení a spuštění webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c5e21-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="c5e21-193">Klikněte **zaregistrovat** karty v horní části stránky:</span><span class="sxs-lookup"><span data-stu-id="c5e21-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="c5e21-194">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-194">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-195">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="c5e21-196">Zadejte nové uživatelské jméno a heslo a pak klikněte na tlačítko **zaregistrovat**:</span><span class="sxs-lookup"><span data-stu-id="c5e21-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="c5e21-197">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-197">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-198">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="c5e21-199">V tomto okamžiku jsou vytvořeny ASP.NET Identity tabulky v databázi MySQL, a uživatel je zaregistrován a přihlášení do aplikace:</span><span class="sxs-lookup"><span data-stu-id="c5e21-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="c5e21-200">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-200">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-201">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="c5e21-202">Instalace nástroje MySQL Workbench ověřit data</span><span class="sxs-lookup"><span data-stu-id="c5e21-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="c5e21-203">Nainstalujte **MySQL Workbench** nástroje z [MySQL stáhne stránky](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="c5e21-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="c5e21-204">V Průvodci instalací: **výběr funkce** vyberte **MySQL Workbench** pod **aplikace** části.</span><span class="sxs-lookup"><span data-stu-id="c5e21-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="c5e21-205">Spusťte aplikaci a přidejte nové připojení pomocí připojení řetězcovými daty z databáze MySQL na Azure, kterou jste vytvořili v účelem úspěchu při žebrání tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="c5e21-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="c5e21-206">Po navázání připojení, zkontrolujte **ASP.NET Identity** tabulky na vytvořit **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="c5e21-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="c5e21-207">Zobrazí se, že všechny ASP.NET Identity požadované tabulky se vytváří, jak je znázorněno na obrázku níže:</span><span class="sxs-lookup"><span data-stu-id="c5e21-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="c5e21-208">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-208">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-209">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="c5e21-210">Zkontrolujte **aspnetusers** tabulky pro instanci zkontrolujte položky při registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="c5e21-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="c5e21-211">[Kliknutím na následující obrázek, rozbalte ho.</span><span class="sxs-lookup"><span data-stu-id="c5e21-211">[Click the following image to expand it.</span></span> <span data-ttu-id="c5e21-212">]</span><span class="sxs-lookup"><span data-stu-id="c5e21-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
