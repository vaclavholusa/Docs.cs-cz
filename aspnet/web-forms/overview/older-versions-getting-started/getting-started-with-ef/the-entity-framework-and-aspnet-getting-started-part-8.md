---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Začínáme s databáze Entity Framework 4.0 nejprve a ASP.NET 4 webové formuláře – část 8 | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET používající rozhraní Entity Framework. Vzorová aplikace je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 035cce022d1b3697b825a96487529dbc9675d90e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887886"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="08a1f-104">Začínáme s databáze Entity Framework 4.0 nejprve a 4 webových formulářů ASP.NET - část 8</span><span class="sxs-lookup"><span data-stu-id="08a1f-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="08a1f-105">Podle [tní Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="08a1f-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="08a1f-106">Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace webových formulářů ASP.NET pomocí sady Visual Studio 2010 a Entity Framework 4.0.</span><span class="sxs-lookup"><span data-stu-id="08a1f-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="08a1f-107">Informace o kurzu řady najdete v tématu [první kurz v řadě](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="08a1f-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="08a1f-108">Pomocí funkce Dynamická Data můžete naformátovat a ověřování dat</span><span class="sxs-lookup"><span data-stu-id="08a1f-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="08a1f-109">V tomto kurzu předchozí implementována uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="08a1f-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="08a1f-110">Tento kurz vám ukáže, jak funkce Dynamická Data můžou poskytovat následující výhody:</span><span class="sxs-lookup"><span data-stu-id="08a1f-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="08a1f-111">Pole jsou automaticky formátovaná pro zobrazení na základě jejich datového typu.</span><span class="sxs-lookup"><span data-stu-id="08a1f-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="08a1f-112">Pole se ověří automaticky na základě jejich datového typu.</span><span class="sxs-lookup"><span data-stu-id="08a1f-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="08a1f-113">Můžete přidat metadata do datového modelu k přizpůsobení chování formátování a ověřování.</span><span class="sxs-lookup"><span data-stu-id="08a1f-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="08a1f-114">Když to uděláte, můžete přidat pravidla formátování a ověřování na jenom jednom místě a jste automaticky použít všude, kde je přístup pole použití ovládacích prvků Dynamická Data.</span><span class="sxs-lookup"><span data-stu-id="08a1f-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="08a1f-115">Pokud chcete zobrazit, jak to funguje, změníte ovládací prvky můžete použít k zobrazení a úprava polí v existující *Students.aspx* stránky a vy budete přidat formátování a ověřování metadata do pole název a datum `Student` typ entity.</span><span class="sxs-lookup"><span data-stu-id="08a1f-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="08a1f-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="08a1f-117">Pomocí DynamicField a DynamicControl ovládací prvky</span><span class="sxs-lookup"><span data-stu-id="08a1f-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="08a1f-118">Otevřete *Students.aspx* stránky a v `StudentsGridView` nahradit ovládacího prvku **název** a **datum registrace** `TemplateField` prvky s následující kód:</span><span class="sxs-lookup"><span data-stu-id="08a1f-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="08a1f-119">Tento kód používá `DynamicControl` řídí místě `TextBox` a `Label` šablony pole pro název ovládacích prvků v student a používá `DynamicField` ovládací prvek pro datum registrace.</span><span class="sxs-lookup"><span data-stu-id="08a1f-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="08a1f-120">Nejsou zadány žádné řetězce formátu.</span><span class="sxs-lookup"><span data-stu-id="08a1f-120">No format strings are specified.</span></span>

<span data-ttu-id="08a1f-121">Přidat `ValidationSummary` řídit po `StudentsGridView` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="08a1f-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="08a1f-122">V `SearchGridView` nahraďte kód pro ovládací prvek **název** a **datum registrace** v sadě sloupců, jako je `StudentsGridView` řídit, s výjimkou vynechejte `EditItemTemplate` element.</span><span class="sxs-lookup"><span data-stu-id="08a1f-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="08a1f-123">`Columns` Element `SearchGridView` řízení nyní obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="08a1f-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="08a1f-124">Otevřete *Students.aspx.cs* a přidejte následující `using` příkaz:</span><span class="sxs-lookup"><span data-stu-id="08a1f-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="08a1f-125">Přidání obslužné rutiny pro stránky `Init` událostí:</span><span class="sxs-lookup"><span data-stu-id="08a1f-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="08a1f-126">Tento kód určuje, zda Dynamická Data vám poskytne formátování a ověřování v tyto ovládací prvky vázané na data pro pole `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="08a1f-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="08a1f-127">Pokud se zobrazí chybová zpráva jako v následujícím příkladu, při spuštění stránky, obvykle znamená jste zapomněli k volání `EnableDynamicData` metoda v `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="08a1f-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="08a1f-128">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="08a1f-128">Run the page.</span></span>

<span data-ttu-id="08a1f-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="08a1f-130">V **datum registrace** sloupce, čas se zobrazí spolu s datum, protože je vlastnost typu `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="08a1f-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="08a1f-131">Později, budete opravte.</span><span class="sxs-lookup"><span data-stu-id="08a1f-131">You'll fix that later.</span></span>

<span data-ttu-id="08a1f-132">Teď Všimněte si, že dynamická Data automaticky poskytuje základní data ověření.</span><span class="sxs-lookup"><span data-stu-id="08a1f-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="08a1f-133">Klikněte například na **upravit**, zrušte zaškrtnutí políčka datum, klikněte na **aktualizace**, a zjistíte, že dynamická Data automaticky provede to povinné pole vzhledem k tomu, že hodnota není null v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="08a1f-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="08a1f-134">Na stránce se zobrazuje hvězdička po pole a chybovou zprávu ve `ValidationSummary` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="08a1f-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="08a1f-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="08a1f-136">Může vynechat `ValidationSummary` řídit, protože také můžete podržet ukazatel myši nad hvězdičky zobrazíte chybovou zprávu:</span><span class="sxs-lookup"><span data-stu-id="08a1f-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="08a1f-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="08a1f-138">Dynamická Data budou také ověřit data zadaná v **datum registrace** pole je platné datum:</span><span class="sxs-lookup"><span data-stu-id="08a1f-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="08a1f-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="08a1f-140">Jak vidíte, toto je obecnou chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="08a1f-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="08a1f-141">V další části se zobrazí postup přizpůsobení formátování pravidel a také ověřování a zprávy.</span><span class="sxs-lookup"><span data-stu-id="08a1f-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="08a1f-142">Přidání metadat do datového modelu</span><span class="sxs-lookup"><span data-stu-id="08a1f-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="08a1f-143">Obvykle který chcete přizpůsobit funkce poskytované službou Dynamická Data.</span><span class="sxs-lookup"><span data-stu-id="08a1f-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="08a1f-144">Například můžete změnit způsob zobrazení dat a obsah chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="08a1f-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="08a1f-145">Je obvykle také přizpůsobit pravidla ověření dat více funkcí než Dynamická Data poskytují automaticky podle datové typy.</span><span class="sxs-lookup"><span data-stu-id="08a1f-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="08a1f-146">K tomu, vytvoření částečné třídy, které odpovídají typy entit.</span><span class="sxs-lookup"><span data-stu-id="08a1f-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="08a1f-147">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **přidat odkaz na**a přidejte odkaz na `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="08a1f-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="08a1f-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="08a1f-149">V *DAL* složku, vytvořte nový soubor třídy a pojmenujte ji *Student.cs*a nahraďte kód šablony v ní následující kód.</span><span class="sxs-lookup"><span data-stu-id="08a1f-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="08a1f-150">Tento kód vytvoří pro konkrétní třídu `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="08a1f-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="08a1f-151">`MetadataType` Použít pro tuto konkrétní třídu atribut určuje třídu, která používáte k určení metadat.</span><span class="sxs-lookup"><span data-stu-id="08a1f-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="08a1f-152">Třídy metadat může mít libovolný název, ale pomocí název entity plus "Metadata" je běžnou praxi.</span><span class="sxs-lookup"><span data-stu-id="08a1f-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="08a1f-153">Atributy použité na vlastnosti ve třídě metadat zadejte formátování, ověření, pravidla a chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="08a1f-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="08a1f-154">Atributy zobrazeny zde bude mít následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="08a1f-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="08a1f-155">`EnrollmentDate` Zobrazí datum (bez čas).</span><span class="sxs-lookup"><span data-stu-id="08a1f-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="08a1f-156">Obě název pole musí být 25 znaků nebo méně v délku a vlastní chybové zprávy je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="08a1f-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="08a1f-157">Jsou požadovány obě pole název a vlastní chybové zprávy je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="08a1f-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="08a1f-158">Spustit *Students.aspx* stránku znovu a zjistíte, že data se nyní zobrazují bez časy:</span><span class="sxs-lookup"><span data-stu-id="08a1f-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="08a1f-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="08a1f-160">Upravte řádek a zkuste vymazání hodnot v polích pro název.</span><span class="sxs-lookup"><span data-stu-id="08a1f-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="08a1f-161">Hvězdičky označující pole chyb se zobrazí, jakmile necháte pole, před kliknutím na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="08a1f-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="08a1f-162">Když kliknete na tlačítko **aktualizace**, na stránce se zobrazí text chybové zprávy, jste zadali.</span><span class="sxs-lookup"><span data-stu-id="08a1f-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="08a1f-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="08a1f-164">Zkuste zadat názvy, které jsou delší než 25 znaků, klikněte na tlačítko **aktualizace**, a na stránce se zobrazí text chybové zprávy, jste zadali.</span><span class="sxs-lookup"><span data-stu-id="08a1f-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="08a1f-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="08a1f-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="08a1f-166">Teď, když jste nastavili tato pravidla formátování a ověřování v metadatech datového modelu, pravidla se automaticky použijí na každé stránce, která zobrazuje nebo umožňuje změny těchto polí, dokud bude používat `DynamicControl` nebo `DynamicField` ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="08a1f-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="08a1f-167">Tím se snižuje množství redundantní kód, který máte k zápisu, který provede programování a testování jednodušší, a zajišťuje, že jsou konzistentní v rámci aplikace formátování data a ověření.</span><span class="sxs-lookup"><span data-stu-id="08a1f-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="08a1f-168">Další informace</span><span class="sxs-lookup"><span data-stu-id="08a1f-168">More Information</span></span>

<span data-ttu-id="08a1f-169">Tím končí tato série kurzů na Začínáme s platformou Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="08a1f-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="08a1f-170">Další zdroje můžete Naučte se používat rozhraní Entity Framework, pokračujte [z prvního kurzu další kurz řady Entity Framework](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) nebo najdete na následujících stránkách:</span><span class="sxs-lookup"><span data-stu-id="08a1f-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="08a1f-171">Rozhraní Entity Framework – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="08a1f-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="08a1f-172">Blog týmu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="08a1f-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="08a1f-173">Rozhraní Entity Framework v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="08a1f-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="08a1f-174">Rozhraní Entity Framework v Centru pro vývojáře MSDN dat</span><span class="sxs-lookup"><span data-stu-id="08a1f-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="08a1f-175">Přehled ovládacího prvku EntityDataSource webového serveru v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="08a1f-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="08a1f-176">Ovládací prvek EntityDataSource referenční dokumentace rozhraní API v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="08a1f-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="08a1f-177">Fóra Entity Framework na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="08a1f-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="08a1f-178">Blog Julie Lerman</span><span class="sxs-lookup"><span data-stu-id="08a1f-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="08a1f-179">Předchozí</span><span class="sxs-lookup"><span data-stu-id="08a1f-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
