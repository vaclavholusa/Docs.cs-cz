---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: rozšíření ověřování dat | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a><span data-ttu-id="d2da2-104">Databázi EF nejprve s architekturou ASP.NET MVC: rozšíření ověřování dat</span><span class="sxs-lookup"><span data-stu-id="d2da2-104">EF Database First with ASP.NET MVC: Enhancing Data Validation</span></span>
====================
<span data-ttu-id="d2da2-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="d2da2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="d2da2-106">Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi.</span><span class="sxs-lookup"><span data-stu-id="d2da2-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="d2da2-107">Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="d2da2-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="d2da2-108">Generovaný kód odpovídá sloupců v tabulce databáze.</span><span class="sxs-lookup"><span data-stu-id="d2da2-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="d2da2-109">Tato část řady se zaměřuje na přidání poznámek data do datového modelu k určení požadavků na ověření a zobrazení formátování.</span><span class="sxs-lookup"><span data-stu-id="d2da2-109">This part of the series focuses on adding data annotations to the data model to specify validation requirements and display formatting.</span></span> <span data-ttu-id="d2da2-110">Byl vylepšen na základě na zpětnou vazbu od uživatelů v sekci komentáře.</span><span class="sxs-lookup"><span data-stu-id="d2da2-110">It was improved based on feedback from users in the comments section.</span></span>


## <a name="add-data-annotations"></a><span data-ttu-id="d2da2-111">Přidat datových poznámek</span><span class="sxs-lookup"><span data-stu-id="d2da2-111">Add data annotations</span></span>

<span data-ttu-id="d2da2-112">Jak už jste viděli v dřívější tématu, některá pravidla ověření budou automaticky použita na vstup uživatele.</span><span class="sxs-lookup"><span data-stu-id="d2da2-112">As you saw in an earlier topic, some data validation rules are automatically applied to the user input.</span></span> <span data-ttu-id="d2da2-113">Například můžete jenom zadat číslo pro vlastnost úrovni.</span><span class="sxs-lookup"><span data-stu-id="d2da2-113">For example, you can only provide a number for the Grade property.</span></span> <span data-ttu-id="d2da2-114">Chcete-li určit další pravidla ověření, můžete přidat datových poznámek na třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="d2da2-114">To specify more data validation rules, you can add data annotations to your model class.</span></span> <span data-ttu-id="d2da2-115">Tyto poznámky platí v celé vaší webové aplikace pro zadanou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="d2da2-115">These annotations are applied throughout your web application for the specified property.</span></span> <span data-ttu-id="d2da2-116">Můžete taky použít formátování atributy, které Změna zobrazení vlastnosti; Změna, jako hodnota použitá pro popisky text.</span><span class="sxs-lookup"><span data-stu-id="d2da2-116">You can also apply formatting attributes that change how the properties are displayed; such as, changing the value used for text labels.</span></span>

<span data-ttu-id="d2da2-117">V tomto kurzu budete přidávat poznámky data omezit délka zadané vlastnosti FirstName, LastName a MiddleName hodnoty.</span><span class="sxs-lookup"><span data-stu-id="d2da2-117">In this tutorial, you will add data annotations to restrict the length of the values provided for the FirstName, LastName, and MiddleName properties.</span></span> <span data-ttu-id="d2da2-118">V databázi jsou tyto hodnoty omezení na 50 znaků; Nicméně ve webové aplikaci tento limit pro počet znaků není aktuálně používá.</span><span class="sxs-lookup"><span data-stu-id="d2da2-118">In the database, these values are limited to 50 characters; however, in your web application that character limit is currently not enforced.</span></span> <span data-ttu-id="d2da2-119">Pokud uživatel poskytuje více než 50 znaků pro jednu z těchto hodnot, dojde k chybě stránky při pokusu o uložení hodnotu do databáze.</span><span class="sxs-lookup"><span data-stu-id="d2da2-119">If a user provides more than 50 characters for one of those values, the page will crash when attempting to save the value to the database.</span></span> <span data-ttu-id="d2da2-120">Úrovni bude také omezit na hodnoty od 0 do 4.</span><span class="sxs-lookup"><span data-stu-id="d2da2-120">You will also restrict Grade to values between 0 and 4.</span></span>

<span data-ttu-id="d2da2-121">Otevřete **Student.cs** v soubor **modely** složky.</span><span class="sxs-lookup"><span data-stu-id="d2da2-121">Open the **Student.cs** file in the **Models** folder.</span></span> <span data-ttu-id="d2da2-122">Přidejte do třídy následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="d2da2-122">Add the following highlighted code to the class.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

<span data-ttu-id="d2da2-123">Enrollment.cs přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="d2da2-123">In Enrollment.cs, add the following highlighted code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

<span data-ttu-id="d2da2-124">Sestavte řešení.</span><span class="sxs-lookup"><span data-stu-id="d2da2-124">Build the solution.</span></span>

<span data-ttu-id="d2da2-125">Přejděte na stránku pro úpravu nebo vytváření student.</span><span class="sxs-lookup"><span data-stu-id="d2da2-125">Browse to a page for editing or creating a student.</span></span> <span data-ttu-id="d2da2-126">Pokud se pokusíte zadat více než 50 znaků, se zobrazí chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="d2da2-126">If you attempt to enter more than 50 characters, an error message is displayed.</span></span>

![zobrazit chybová zpráva](enhancing-data-validation/_static/image1.png)

<span data-ttu-id="d2da2-128">Přejděte na stránku pro úpravy registrace a pokus o poskytují úrovni výše 4.</span><span class="sxs-lookup"><span data-stu-id="d2da2-128">Browse to the page for editing enrollments, and attempt to provide a grade above 4.</span></span>

![Chyba úrovni rozsahu](enhancing-data-validation/_static/image2.png)

<span data-ttu-id="d2da2-130">Úplný seznam datových poznámek ověření můžete provést u třídy a vlastnosti, najdete v části [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span><span class="sxs-lookup"><span data-stu-id="d2da2-130">For a full list of data validation annotations you can apply to properties and classes, see [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).</span></span>

## <a name="add-metadata-classes"></a><span data-ttu-id="d2da2-131">Přidání třídy metadat</span><span class="sxs-lookup"><span data-stu-id="d2da2-131">Add metadata classes</span></span>

<span data-ttu-id="d2da2-132">Přidání atributy ověření přímo do třídy modelu funguje, když neočekáváte databázi chcete změnit; ale pokud vaše změny v databázi a je nutné znovu vygenerovat třídu modelu, ztratíte všechny atributy, které měl použít pro třídu modelu.</span><span class="sxs-lookup"><span data-stu-id="d2da2-132">Adding the validation attributes directly to the model class works when you do not expect the database to change; however, if your database changes and you need to regenerate the model class, you will lose all of the attributes you had applied to the model class.</span></span> <span data-ttu-id="d2da2-133">Tento přístup může být velmi neefektivní a náchylné k došlo ke ztrátě důležitých ověřovacích pravidel.</span><span class="sxs-lookup"><span data-stu-id="d2da2-133">This approach can be very inefficient and prone to losing important validation rules.</span></span>

<span data-ttu-id="d2da2-134">Chcete-li se tomuto problému vyhnout, můžete přidat třídu metadat, která obsahuje atributy.</span><span class="sxs-lookup"><span data-stu-id="d2da2-134">To avoid this problem, you can add a metadata class that contains the attributes.</span></span> <span data-ttu-id="d2da2-135">Pokud přidružíte třídy modelu pro třídu metadat, jsou tyto atributy použité pro model.</span><span class="sxs-lookup"><span data-stu-id="d2da2-135">When you associate the model class to the metadata class, those attributes are applied to the model.</span></span> <span data-ttu-id="d2da2-136">V tento postup mohou vytvořit třídu modelu znovu bez ztráty všechny atributy, které byly použity pro třídu metadat.</span><span class="sxs-lookup"><span data-stu-id="d2da2-136">In this approach, the model class can be regenerated without losing all of the attributes that have been applied to the metadata class.</span></span>

<span data-ttu-id="d2da2-137">V **modely** složky, přidejte třídu s názvem **Metadata.cs**.</span><span class="sxs-lookup"><span data-stu-id="d2da2-137">In the **Models** folder, add a class named **Metadata.cs**.</span></span>

![Přidání třídy metadat](enhancing-data-validation/_static/image3.png)

<span data-ttu-id="d2da2-139">Nahraďte kód v Metadata.cs následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="d2da2-139">Replace the code in Metadata.cs with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

<span data-ttu-id="d2da2-140">Tyto třídy metadata obsahují všechny atributy ověřování, které jste použili předtím do třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="d2da2-140">These metadata classes contain all of the validation attributes that you had previously applied to the model classes.</span></span> <span data-ttu-id="d2da2-141">**Zobrazení** atribut slouží ke změně hodnoty použít pro popisky text.</span><span class="sxs-lookup"><span data-stu-id="d2da2-141">The **Display** attribute is used to change the value used for text labels.</span></span>

<span data-ttu-id="d2da2-142">Nyní je třeba přidružit třídy modelu třídy metadat.</span><span class="sxs-lookup"><span data-stu-id="d2da2-142">Now, you must associate the model classes with the metadata classes.</span></span>

<span data-ttu-id="d2da2-143">V **modely** složky, přidejte třídu s názvem **PartialClasses.cs**.</span><span class="sxs-lookup"><span data-stu-id="d2da2-143">In the **Models** folder, add a class named **PartialClasses.cs**.</span></span>

<span data-ttu-id="d2da2-144">Obsah souboru nahraďte následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="d2da2-144">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

<span data-ttu-id="d2da2-145">Všimněte si, že každá třída je označena jako `partial` třída a všechny odpovídající název a oboru názvů jako třída, která se automaticky vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="d2da2-145">Notice that each class is marked as a `partial` class, and each matches the name and namespace as the class that is automatically generated.</span></span> <span data-ttu-id="d2da2-146">Použitím atribut metadat třídu zajistíte, že atributy ověření dat se použije k třídě automaticky generovány.</span><span class="sxs-lookup"><span data-stu-id="d2da2-146">By applying the metadata attribute to the partial class, you ensure that the data validation attributes will be applied to the automatically-generated class.</span></span> <span data-ttu-id="d2da2-147">Tyto atributy nebudou ztracena při opětovném generování tříd modelu, protože atribut metadat se používá v částečné třídy, které nejsou znovu vygeneroval.</span><span class="sxs-lookup"><span data-stu-id="d2da2-147">These attributes will not be lost when you regenerate the model classes because the metadata attribute is applied in partial classes that are not regenerated.</span></span>

<span data-ttu-id="d2da2-148">Chcete-li znovu vygenerovat automaticky vygenerované třídy, otevřete soubor ContosoModel.edmx.</span><span class="sxs-lookup"><span data-stu-id="d2da2-148">To regenerate the automatically-generated classes, open the ContosoModel.edmx file.</span></span> <span data-ttu-id="d2da2-149">Znovu klikněte pravým tlačítkem na návrhové plochy a vyberte možnost **aktualizovat Model z databáze**.</span><span class="sxs-lookup"><span data-stu-id="d2da2-149">Once again, right-click on the design surface and select **Update Model from Database**.</span></span> <span data-ttu-id="d2da2-150">I když jste nezměnili databáze, tento proces se znova vygeneruje třídy.</span><span class="sxs-lookup"><span data-stu-id="d2da2-150">Even though you have not changed the database, this process will regenerate the classes.</span></span> <span data-ttu-id="d2da2-151">V **aktualizovat** vyberte **tabulky** a **Dokončit**.</span><span class="sxs-lookup"><span data-stu-id="d2da2-151">In the **Refresh** tab, select **Tables** and **Finish**.</span></span>

![Aktualizovat tabulky](enhancing-data-validation/_static/image4.png)

<span data-ttu-id="d2da2-153">Uložte soubor ContosoModel.edmx tak, aby se změny projevily.</span><span class="sxs-lookup"><span data-stu-id="d2da2-153">Save the ContosoModel.edmx file to apply the changes.</span></span>

<span data-ttu-id="d2da2-154">Otevřete soubor Student.cs nebo soubor Enrollment.cs a Všimněte si, že atributy ověření dat, které jste provedli dříve již nejsou v souboru.</span><span class="sxs-lookup"><span data-stu-id="d2da2-154">Open the Student.cs file or the Enrollment.cs file, and notice that the data validation attributes you applied earlier are no longer in the file.</span></span> <span data-ttu-id="d2da2-155">Ale spusťte aplikaci a Všimněte si, že ověřovací pravidla stále použít při zadávání dat.</span><span class="sxs-lookup"><span data-stu-id="d2da2-155">However, run the application, and notice that the validation rules are still applied when you enter data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2da2-156">[Předchozí](customizing-a-view.md)
> [další](publish-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="d2da2-156">[Previous](customizing-a-view.md)
[Next](publish-to-azure.md)</span></span>
