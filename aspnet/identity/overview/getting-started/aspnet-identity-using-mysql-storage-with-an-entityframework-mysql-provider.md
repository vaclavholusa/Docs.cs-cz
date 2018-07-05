---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Použití úložiště MySQL zprostředkovatele EntityFramework MySQL (C#) | Dokumentace Microsoftu'
author: maumar
description: V tomto kurzu se dozvíte, jak nahradit výchozí mechanismus úložiště dat pro ASP.NET Identity objektu EntityFramework (zprostředkovatel SQL klient) s MySQL zajištění...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6b1d57c65cb4197d1b20175415ee73b3e81cb53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383036"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="acf58-103">ASP.NET Identity: Použití úložiště MySQL zprostředkovatele EntityFramework MySQL (C#)</span><span class="sxs-lookup"><span data-stu-id="acf58-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="acf58-104">podle [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="acf58-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="acf58-105">V tomto kurzu se dozvíte, jak nahradit výchozí mechanismem úložiště dat pro [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) s s MySQL zprostředkovatele EntityFramework (zprostředkovatel SQL klient).</span><span class="sxs-lookup"><span data-stu-id="acf58-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="acf58-106">V následujících tématech se budeme v tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="acf58-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="acf58-107">Vytváří se databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="acf58-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="acf58-108">Vytvoření aplikace MVC pomocí šablony Visual Studio 2013 MVC</span><span class="sxs-lookup"><span data-stu-id="acf58-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="acf58-109">Konfigurace objektu EntityFramework pro práci s poskytovatele databáze MySQL</span><span class="sxs-lookup"><span data-stu-id="acf58-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="acf58-110">Spuštění aplikace ověřit výsledky</span><span class="sxs-lookup"><span data-stu-id="acf58-110">Running the application to verify the results</span></span>

<span data-ttu-id="acf58-111">Na konci tohoto kurzu budete mít aplikace MVC s ASP.NET Identity ukládat, která používá databázi MySQL, který je hostován v Azure.</span><span class="sxs-lookup"><span data-stu-id="acf58-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="acf58-112">Vytvoření instance databáze MySQL v Azure</span><span class="sxs-lookup"><span data-stu-id="acf58-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="acf58-113">Přihlaste se k [webu Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="acf58-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="acf58-114">Klikněte na tlačítko **nový** v dolní části stránky a pak vyberte **úložiště**:</span><span class="sxs-lookup"><span data-stu-id="acf58-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="acf58-115">V **zvolit a doplněk** průvodce, vyberte **databázi ClearDB MySQL**a potom klikněte na tlačítko **Další** šipky v dolní části rámce:</span><span class="sxs-lookup"><span data-stu-id="acf58-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="acf58-116">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-116">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-117">]</span><span class="sxs-lookup"><span data-stu-id="acf58-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="acf58-118">Ponechte výchozí **Free** plánovat, změnit **název** k **IdentityMySQLDatabase**, vyberte oblast, která je vám nejblíže. a poté klikněte na tlačítko **Další** šipky v dolní části rámce:</span><span class="sxs-lookup"><span data-stu-id="acf58-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="acf58-119">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-119">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-120">]</span><span class="sxs-lookup"><span data-stu-id="acf58-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="acf58-121">Klikněte na tlačítko **nákupní** značku zaškrtnutí dokončete vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="acf58-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="acf58-122">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-122">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-123">]</span><span class="sxs-lookup"><span data-stu-id="acf58-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="acf58-124">Po vytvoření vaší databáze ji můžete spravovat z **doplňky** karta na portálu management portal.</span><span class="sxs-lookup"><span data-stu-id="acf58-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="acf58-125">Pokud chcete načíst informace o připojení pro vaši databázi, klikněte na tlačítko **informace o připojení** v dolní části stránky:</span><span class="sxs-lookup"><span data-stu-id="acf58-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="acf58-126">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-126">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-127">]</span><span class="sxs-lookup"><span data-stu-id="acf58-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="acf58-128">Zkopírujte připojovací řetězec po kliknutí na tlačítko kopírování podle **CONNECTIONSTRING** pole a uložte ho; tyto informace dále v tomto kurzu budete používat pro aplikaci MVC:</span><span class="sxs-lookup"><span data-stu-id="acf58-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="acf58-129">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-129">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-130">]</span><span class="sxs-lookup"><span data-stu-id="acf58-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="acf58-131">Vytvoření projektu aplikace MVC</span><span class="sxs-lookup"><span data-stu-id="acf58-131">Creating an MVC application project</span></span>

<span data-ttu-id="acf58-132">K dokončení kroků v této části kurzu, budete nejprve muset nainstalovat [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) nebo [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="acf58-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="acf58-133">Po instalaci sady Visual Studio, použijte následující postup k vytvoření nového projektu aplikace MVC:</span><span class="sxs-lookup"><span data-stu-id="acf58-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="acf58-134">Otevřete Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="acf58-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="acf58-135">Klikněte na tlačítko **nový projekt** z **Start** stránky, nebo můžete kliknout na **souboru** nabídky a pak **nový projekt**:</span><span class="sxs-lookup"><span data-stu-id="acf58-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="acf58-136">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-136">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-137">]</span><span class="sxs-lookup"><span data-stu-id="acf58-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="acf58-138">Při **nový projekt** se zobrazí dialogové okno, rozbalte **Visual C#** v seznamu šablon klikněte **webové**a vyberte **WebováaplikaceASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="acf58-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="acf58-139">Pojmenujte svůj projekt **IdentityMySQLDemo** a potom klikněte na tlačítko **OK**:</span><span class="sxs-lookup"><span data-stu-id="acf58-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="acf58-140">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-140">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-141">]</span><span class="sxs-lookup"><span data-stu-id="acf58-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="acf58-142">V **nový projekt ASP.NET** dialogového okna, vyberte **MVC** šablonyPomocí výchozí možnosti; tím se konfigurace **jednotlivé uživatelské účty** jako metodu ověřování.</span><span class="sxs-lookup"><span data-stu-id="acf58-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="acf58-143">Klikněte na tlačítko **OK**:</span><span class="sxs-lookup"><span data-stu-id="acf58-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="acf58-144">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-144">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-145">]</span><span class="sxs-lookup"><span data-stu-id="acf58-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="acf58-146">Konfigurace objektu EntityFramework pro práci s databází MySQL</span><span class="sxs-lookup"><span data-stu-id="acf58-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="acf58-147">Aktualizovat Entity Framework sestavení pro projekt</span><span class="sxs-lookup"><span data-stu-id="acf58-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="acf58-148">Aplikace MVC, který byl vytvořen z šablony sady Visual Studio 2013 obsahuje odkaz na [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) balíček, ale musí se na toto sestavení od jeho vydání aktualizace, které obsahují důležité vylepšení výkonu.</span><span class="sxs-lookup"><span data-stu-id="acf58-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="acf58-149">Chcete-li použít tyto nejnovější aktualizace ve vaší aplikaci, použijte následující postup.</span><span class="sxs-lookup"><span data-stu-id="acf58-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="acf58-150">Otevřete svůj projekt MVC v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="acf58-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="acf58-151">Klikněte na tlačítko **nástroje**, pak klikněte na tlačítko **Správce balíčků knihoven**a potom klikněte na tlačítko **Konzola správce balíčků**:</span><span class="sxs-lookup"><span data-stu-id="acf58-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="acf58-152">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-152">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-153">]</span><span class="sxs-lookup"><span data-stu-id="acf58-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="acf58-154">**Konzola správce balíčků** se zobrazí v dolní části sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="acf58-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="acf58-155">Typ &quot; **Update-Package EntityFramework** &quot; a stiskněte klávesu Enter:</span><span class="sxs-lookup"><span data-stu-id="acf58-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="acf58-156">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-156">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-157">]</span><span class="sxs-lookup"><span data-stu-id="acf58-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="acf58-158">Nainstalovat MySQL zprostředkovatele EntityFramework</span><span class="sxs-lookup"><span data-stu-id="acf58-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="acf58-159">Aby EntityFramework pro připojení k databázi MySQL budete muset nainstalovat MySQL zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="acf58-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="acf58-160">Chcete-li to provést, otevřete **Konzola správce balíčků** a typ &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, a potom stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="acf58-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="acf58-161">Toto je předběžná verze sestavení, a proto může obsahovat chyby.</span><span class="sxs-lookup"><span data-stu-id="acf58-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="acf58-162">Předběžná verze zprostředkovatele byste neměli používat v produkčním prostředí.</span><span class="sxs-lookup"><span data-stu-id="acf58-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="acf58-163">[Klikněte na tlačítko se rozbalí na následujícím obrázku.]</span><span class="sxs-lookup"><span data-stu-id="acf58-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="acf58-164">Provádění změn konfigurace projektu v souboru Web.config pro aplikaci</span><span class="sxs-lookup"><span data-stu-id="acf58-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="acf58-165">V této části budete konfigurovat Entity Framework pro použití MySQL poskytovatele, který jste právě nainstalovali, se zaregistrovat objekt pro vytváření zprostředkovatele MySQL a přidejte váš připojovací řetězec z Azure.</span><span class="sxs-lookup"><span data-stu-id="acf58-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="acf58-166">Následující příklady obsahují konkrétní sestavení verze pro MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="acf58-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="acf58-167">Pokud se změní verze sestavení, musíte změnit nastavení odpovídající konfigurace se správnou verzí.</span><span class="sxs-lookup"><span data-stu-id="acf58-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="acf58-168">Otevřete soubor Web.config pro váš projekt v sadě Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="acf58-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="acf58-169">Vyhledejte následující nastavení konfigurace, které definují výchozí databáze zprostředkovatelů a objektů factory pro Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="acf58-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="acf58-170">Nahraďte následující příkaz, který nakonfiguruje Entity Framework MySQL poskytovatel se použije tato nastavení konfigurace:</span><span class="sxs-lookup"><span data-stu-id="acf58-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="acf58-171">Vyhledejte &lt;connectionStrings&gt; části a nahraďte ho následujícím kódem, který bude definování připojovacího řetězce pro databázi MySQL, který je hostovaný v Azure (Všimněte si, že hodnota providerName také se změnil z původní):</span><span class="sxs-lookup"><span data-stu-id="acf58-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="acf58-172">Přidání vlastní místní MigrationHistory</span><span class="sxs-lookup"><span data-stu-id="acf58-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="acf58-173">Používá Entity Framework Code First **MigrationHistory** tabulce ke sledování změny modelu a zajistit konzistenci mezi schéma databáze a konceptuální schéma.</span><span class="sxs-lookup"><span data-stu-id="acf58-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="acf58-174">Ale tato tabulka nefunguje pro MySQL ve výchozím nastavení protože primární klíč je moc velká.</span><span class="sxs-lookup"><span data-stu-id="acf58-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="acf58-175">Chcete tuto situaci napravit, musíte zmenšit velikost klíče pro tuto tabulku.</span><span class="sxs-lookup"><span data-stu-id="acf58-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="acf58-176">Chcete-li to provést, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="acf58-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="acf58-177">Jsou zachyceny informace o schématu pro tuto tabulku **HistoryContext**, což může být změněna jako jakýkoli jiný **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="acf58-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="acf58-178">Uděláte to tak, přidejte nový soubor třídy s názvem **MySqlHistoryContext.cs** do projektu a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="acf58-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="acf58-179">Dále budete muset nakonfigurovat rozhraní Entity Framework pomocí upravené **HistoryContext**, namísto výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="acf58-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="acf58-180">To můžete udělat pomocí možnosti konfigurace založená na kódu.</span><span class="sxs-lookup"><span data-stu-id="acf58-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="acf58-181">To uděláte tak, přidáte nový soubor třídy s názvem **MySqlConfiguration.cs** do vašeho projektu a nahraďte jeho obsah se:</span><span class="sxs-lookup"><span data-stu-id="acf58-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="acf58-182">Vytváření vlastních inicializátor objektu EntityFramework pro ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="acf58-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="acf58-183">MySQL zprostředkovatele, který je vybrané v tomto kurzu aktuálně nepodporuje migrace Entity Framework, tak budete muset použít model inicializátory abyste se mohli připojit k databázi.</span><span class="sxs-lookup"><span data-stu-id="acf58-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="acf58-184">Protože v tomto kurzu používá instanci MySQL v Azure, musíte vytvořit vlastní Entity Framework inicializátor.</span><span class="sxs-lookup"><span data-stu-id="acf58-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="acf58-185">Tento krok není povinný, pokud se chcete připojit k instanci systému SQL Server v Azure nebo pokud používáte databázi, která je hostovaná v místním prostředí.</span><span class="sxs-lookup"><span data-stu-id="acf58-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="acf58-186">Pokud chcete vytvořit vlastní Entity Framework inicializátor pro MySQL, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="acf58-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="acf58-187">Přidejte nový soubor třídy s názvem **MySqlInitializer.cs** je do projektu a nahraďte obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="acf58-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="acf58-188">Otevřít **IdentityModels.cs** souboru projektu, který je umístěn v **modely** adresáře a nahraďte jeho obsah následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="acf58-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="acf58-189">Spuštění aplikace a ověřování databáze</span><span class="sxs-lookup"><span data-stu-id="acf58-189">Running the application and verifying the database</span></span>

<span data-ttu-id="acf58-190">Po dokončení kroků v předchozí části, měli byste otestovat vaši databázi.</span><span class="sxs-lookup"><span data-stu-id="acf58-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="acf58-191">Chcete-li to provést, postupujte následovně:</span><span class="sxs-lookup"><span data-stu-id="acf58-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="acf58-192">Stisknutím klávesy **Ctrl + F5** sestavíte a spustíte webovou aplikaci.</span><span class="sxs-lookup"><span data-stu-id="acf58-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="acf58-193">Klikněte na tlačítko **zaregistrovat** kartě v horní části stránky:</span><span class="sxs-lookup"><span data-stu-id="acf58-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="acf58-194">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-194">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-195">]</span><span class="sxs-lookup"><span data-stu-id="acf58-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="acf58-196">Zadejte nové uživatelské jméno a heslo a potom klikněte na tlačítko **zaregistrovat**:</span><span class="sxs-lookup"><span data-stu-id="acf58-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="acf58-197">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-197">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-198">]</span><span class="sxs-lookup"><span data-stu-id="acf58-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="acf58-199">V tomto okamžiku se vytvoří ASP.NET Identity tabulky v databázi MySQL a je uživatel zaregistrován a přihlášení do aplikace:</span><span class="sxs-lookup"><span data-stu-id="acf58-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="acf58-200">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-200">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-201">]</span><span class="sxs-lookup"><span data-stu-id="acf58-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="acf58-202">Instalace nástroje MySQL Workbench pro ověření dat</span><span class="sxs-lookup"><span data-stu-id="acf58-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="acf58-203">Nainstalujte **aplikace MySQL Workbench** nástroj z [stránky pro MySQL stažení](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="acf58-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="acf58-204">V Průvodci instalací: **výběr součástí** kartu, vyberte možnost **aplikace MySQL Workbench** pod **aplikací** oddílu.</span><span class="sxs-lookup"><span data-stu-id="acf58-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="acf58-205">Spusťte aplikaci a přidejte nové připojení pomocí připojení řetězcovými daty z databáze Azure MySQL, kterou jste vytvořili v účelem úspěchu při žebrání tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="acf58-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="acf58-206">Po navázání připojení, zkontrolujte **ASP.NET Identity** tabulky vytvořené na **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="acf58-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="acf58-207">Zobrazí se, že všechny ASP.NET Identity požadované tabulky vytvářejí, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="acf58-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="acf58-208">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-208">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-209">]</span><span class="sxs-lookup"><span data-stu-id="acf58-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="acf58-210">Zkontrolujte **aspnetusers** tabulky pro instanci Hledat položky při registraci nového uživatele.</span><span class="sxs-lookup"><span data-stu-id="acf58-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="acf58-211">[Klikněte na tlačítko se rozbalí na následujícím obrázku.</span><span class="sxs-lookup"><span data-stu-id="acf58-211">[Click the following image to expand it.</span></span> <span data-ttu-id="acf58-212">]</span><span class="sxs-lookup"><span data-stu-id="acf58-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
