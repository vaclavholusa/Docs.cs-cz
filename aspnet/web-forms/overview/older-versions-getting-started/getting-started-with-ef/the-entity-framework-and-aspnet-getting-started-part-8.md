---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Začínáme s Entity Framework 4.0 Database First a technologie ASP.NET 4 webových formulářů – 8. část | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Frameworku. Ukázková aplikace je...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 249677c9e0eca248710bf730e57a7aeca5a9b5ce
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754303"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a><span data-ttu-id="fbd48-104">Začínáme s Entity Framework 4.0 Database First a 4 webových formulářů ASP.NET – 8. část</span><span class="sxs-lookup"><span data-stu-id="fbd48-104">Getting Started with Entity Framework 4.0 Database First and ASP.NET 4 Web Forms - Part 8</span></span>
====================
<span data-ttu-id="fbd48-105">podle [Petr Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="fbd48-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="fbd48-106">Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvořit aplikace webových formulářů ASP.NET pomocí Entity Framework 4.0 a Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="fbd48-106">The Contoso University sample web application demonstrates how to create ASP.NET Web Forms applications using the Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="fbd48-107">Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](the-entity-framework-and-aspnet-getting-started-part-1.md)</span><span class="sxs-lookup"><span data-stu-id="fbd48-107">For information about the tutorial series, see [the first tutorial in the series](the-entity-framework-and-aspnet-getting-started-part-1.md)</span></span>


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a><span data-ttu-id="fbd48-108">Funkce dynamických dat pro formátování a ověřování dat</span><span class="sxs-lookup"><span data-stu-id="fbd48-108">Using Dynamic Data Functionality to Format and Validate Data</span></span>

<span data-ttu-id="fbd48-109">V předchozím kurzu jste implementovali uložené procedury.</span><span class="sxs-lookup"><span data-stu-id="fbd48-109">In the previous tutorial you implemented stored procedures.</span></span> <span data-ttu-id="fbd48-110">Tomto kurzu se dozvíte, jak mohou funkci Dynamická Data poskytuje následující výhody:</span><span class="sxs-lookup"><span data-stu-id="fbd48-110">This tutorial will show you how Dynamic Data functionality can provide the following benefits:</span></span>

- <span data-ttu-id="fbd48-111">Pole jsou automaticky formátovat k zobrazení na základě jeho datového typu.</span><span class="sxs-lookup"><span data-stu-id="fbd48-111">Fields are automatically formatted for display based on their data type.</span></span>
- <span data-ttu-id="fbd48-112">Pole se automaticky ověří na základě jeho datového typu.</span><span class="sxs-lookup"><span data-stu-id="fbd48-112">Fields are automatically validated based on their data type.</span></span>
- <span data-ttu-id="fbd48-113">Přidat metadata do datového modelu můžete přizpůsobit chování formátování i ověřování.</span><span class="sxs-lookup"><span data-stu-id="fbd48-113">You can add metadata to the data model to customize formatting and validation behavior.</span></span> <span data-ttu-id="fbd48-114">Když to uděláte, můžete přidat pravidla formátování a validace na jenom jednom místě a jste automaticky použít všude, kde máte přístup k poli pomocí dynamické databázové ovládací prvky.</span><span class="sxs-lookup"><span data-stu-id="fbd48-114">When you do this, you can add the formatting and validation rules in just one place, and they're automatically applied everywhere you access the fields using Dynamic Data controls.</span></span>

<span data-ttu-id="fbd48-115">Pokud chcete zobrazit, jak to funguje, změníte ovládací prvky, které použijete k zobrazení a úprava polí v existujícím *Students.aspx* stránky a přidejte formátování i ověřování metadat na pole name a date `Student` typu entity.</span><span class="sxs-lookup"><span data-stu-id="fbd48-115">To see how this works, you'll change the controls you use to display and edit fields in the existing *Students.aspx* page, and you'll add formatting and validation metadata to the name and date fields of the `Student` entity type.</span></span>

<span data-ttu-id="fbd48-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-116">[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)</span></span>

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a><span data-ttu-id="fbd48-117">Použití objektu DynamicControl ovládací prvky a DynamicField</span><span class="sxs-lookup"><span data-stu-id="fbd48-117">Using DynamicField and DynamicControl Controls</span></span>

<span data-ttu-id="fbd48-118">Otevřít *Students.aspx* stránku a `StudentsGridView` nahraďte ovládací prvek **název** a **datum registrace** `TemplateField` prvky s následující kód:</span><span class="sxs-lookup"><span data-stu-id="fbd48-118">Open the *Students.aspx* page and in the `StudentsGridView` control replace the **Name** and **Enrollment Date** `TemplateField` elements with the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

<span data-ttu-id="fbd48-119">Tento kód používá `DynamicControl` řídí místo `TextBox` a `Label` ovládacích prvků v studenta šablony pole pro název serveru a používá `DynamicField` ovládací prvek pro datum registrace.</span><span class="sxs-lookup"><span data-stu-id="fbd48-119">This markup uses `DynamicControl` controls in place of `TextBox` and `Label` controls in the student name template field, and it uses a `DynamicField` control for the enrollment date.</span></span> <span data-ttu-id="fbd48-120">Nejsou zadány žádné řetězce formátu.</span><span class="sxs-lookup"><span data-stu-id="fbd48-120">No format strings are specified.</span></span>

<span data-ttu-id="fbd48-121">Přidat `ValidationSummary` řídit po `StudentsGridView` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="fbd48-121">Add a `ValidationSummary` control after the `StudentsGridView` control.</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

<span data-ttu-id="fbd48-122">V `SearchGridView` nahradit kód pro ovládací prvek **název** a **datum registrace** sloupců, jako jste to udělali v `StudentsGridView` řídit, s výjimkou vynechat, nechte `EditItemTemplate` elementu.</span><span class="sxs-lookup"><span data-stu-id="fbd48-122">In the `SearchGridView` control replace the markup for the **Name** and **Enrollment Date** columns as you did in the `StudentsGridView` control, except omit the `EditItemTemplate` element.</span></span> <span data-ttu-id="fbd48-123">`Columns` Elementu `SearchGridView` ovládací prvek nyní obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="fbd48-123">The `Columns` element of the `SearchGridView` control now contains the following markup:</span></span>

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

<span data-ttu-id="fbd48-124">Otevřít *Students.aspx.cs* a přidejte následující `using` – příkaz:</span><span class="sxs-lookup"><span data-stu-id="fbd48-124">Open *Students.aspx.cs* and add the following `using` statement:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

<span data-ttu-id="fbd48-125">Přidání obslužné rutiny pro stránky `Init` události:</span><span class="sxs-lookup"><span data-stu-id="fbd48-125">Add a handler for the page's `Init` event:</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

<span data-ttu-id="fbd48-126">Tento kód určuje, jestli Dynamická Data poskytne formátování i ověřování v těchto ovládacích prvků vázaných na data pro pole `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="fbd48-126">This code specifies that Dynamic Data will provide formatting and validation in these data-bound controls for fields of the `Student` entity.</span></span> <span data-ttu-id="fbd48-127">Pokud se zobrazí chybová zpráva jako v následujícím příkladu, při spuštění stránky, obvykle to znamená, jste zapomněli volat `EnableDynamicData` metoda ve `Page_Init`:</span><span class="sxs-lookup"><span data-stu-id="fbd48-127">If you get an error message like the following example when you run the page, it typically means you've forgotten to call the `EnableDynamicData` method in `Page_Init`:</span></span>

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

<span data-ttu-id="fbd48-128">Spuštění stránky.</span><span class="sxs-lookup"><span data-stu-id="fbd48-128">Run the page.</span></span>

<span data-ttu-id="fbd48-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-129">[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)</span></span>

<span data-ttu-id="fbd48-130">V **datum registrace** sloupce, čas se zobrazí spolu s datem, protože je vlastnost typu `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="fbd48-130">In the **Enrollment Date** column, the time is displayed along with the date because the property type is `DateTime`.</span></span> <span data-ttu-id="fbd48-131">Opravíte to později.</span><span class="sxs-lookup"><span data-stu-id="fbd48-131">You'll fix that later.</span></span>

<span data-ttu-id="fbd48-132">Nyní Všimněte si, dynamická Data automaticky poskytuje základní data ověření.</span><span class="sxs-lookup"><span data-stu-id="fbd48-132">For now, notice that Dynamic Data automatically provides basic data validation.</span></span> <span data-ttu-id="fbd48-133">Klikněte například na **upravit**, zrušte zaškrtnutí pole data, klikněte na tlačítko **aktualizace**, a uvidíte, že dynamických dat automaticky provede to povinné pole. protože hodnota není s možnou hodnotou Null v datovém modelu.</span><span class="sxs-lookup"><span data-stu-id="fbd48-133">For example, click **Edit**, clear the date field, click **Update**, and you see that Dynamic Data automatically makes this a required field because the value is not nullable in the data model.</span></span> <span data-ttu-id="fbd48-134">Na stránce se zobrazí hvězdička po pole a chybovou zprávu ve `ValidationSummary` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="fbd48-134">The page displays an asterisk after the field and an error message in the `ValidationSummary` control:</span></span>

<span data-ttu-id="fbd48-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-135">[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)</span></span>

<span data-ttu-id="fbd48-136">Může vynechat `ValidationSummary` ovládací prvek, protože můžete také podržíte ukazatel myši nad na hvězdičku chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="fbd48-136">You could omit the `ValidationSummary` control, because you can also hold the mouse pointer over the asterisk to see the error message:</span></span>

<span data-ttu-id="fbd48-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-137">[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)</span></span>

<span data-ttu-id="fbd48-138">Dynamická Data také ověří data zadaná v **datum registrace** pole je platné datum:</span><span class="sxs-lookup"><span data-stu-id="fbd48-138">Dynamic Data will also validate that data entered in the **Enrollment Date** field is a valid date:</span></span>

<span data-ttu-id="fbd48-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-139">[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)</span></span>

<span data-ttu-id="fbd48-140">Jak vidíte, toto je obecné chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fbd48-140">As you can see, this is a generic error message.</span></span> <span data-ttu-id="fbd48-141">V další části uvidíte, jak přizpůsobit zprávy stejně jako ověřování a formátování pravidla.</span><span class="sxs-lookup"><span data-stu-id="fbd48-141">In the next section you'll see how to customize messages as well as validation and formatting rules.</span></span>

## <a name="adding-metadata-to-the-data-model"></a><span data-ttu-id="fbd48-142">Přidání metadat do datového modelu</span><span class="sxs-lookup"><span data-stu-id="fbd48-142">Adding Metadata to the Data Model</span></span>

<span data-ttu-id="fbd48-143">Obvykle budete chtít přizpůsobit funkce poskytované službou Dynamická Data.</span><span class="sxs-lookup"><span data-stu-id="fbd48-143">Typically, you want to customize the functionality provided by Dynamic Data.</span></span> <span data-ttu-id="fbd48-144">Například můžete změnit způsob zobrazení dat a obsahu chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fbd48-144">For example, you might change how data is displayed and the content of error messages.</span></span> <span data-ttu-id="fbd48-145">Obvykle také přizpůsobíte data ověřovacích pravidel poskytnout více funkcí, než jaké poskytuje dynamických dat automaticky na základě typu dat.</span><span class="sxs-lookup"><span data-stu-id="fbd48-145">You typically also customize data validation rules to provide more functionality than what Dynamic Data provides automatically based on data types.</span></span> <span data-ttu-id="fbd48-146">K tomuto účelu vytvoříte částečné třídy, které odpovídají typy entit.</span><span class="sxs-lookup"><span data-stu-id="fbd48-146">To do this, you create partial classes that correspond to entity types.</span></span>

<span data-ttu-id="fbd48-147">V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projekt, vyberte **přidat odkaz**a přidejte odkaz na `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="fbd48-147">In **Solution Explorer**, right-click the **ContosoUniversity** project, select **Add Reference**, and add a reference to `System.ComponentModel.DataAnnotations`.</span></span>

<span data-ttu-id="fbd48-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-148">[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)</span></span>

<span data-ttu-id="fbd48-149">V *DAL* složku, vytvořte nový soubor třídy, pojmenujte ho *Student.cs*a nahraďte kód šablony do něj následující kód.</span><span class="sxs-lookup"><span data-stu-id="fbd48-149">In the *DAL* folder, create a new class file, name it *Student.cs*, and replace the template code in it with the following code.</span></span>

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

<span data-ttu-id="fbd48-150">Tento kód vytvoří částečnou třídu pro `Student` entity.</span><span class="sxs-lookup"><span data-stu-id="fbd48-150">This code creates a partial class for the `Student` entity.</span></span> <span data-ttu-id="fbd48-151">`MetadataType` Použije pro tuto částečnou třídu Určuje třídu, která používáte k určení metadat.</span><span class="sxs-lookup"><span data-stu-id="fbd48-151">The `MetadataType` attribute applied to this partial class identifies the class that you're using to specify metadata.</span></span> <span data-ttu-id="fbd48-152">Metadata třída může mít libovolný název, ale pomocí "Metadat" a název entity je běžnou praxí.</span><span class="sxs-lookup"><span data-stu-id="fbd48-152">The metadata class can have any name, but using the entity name plus "Metadata" is a common practice.</span></span>

<span data-ttu-id="fbd48-153">Atributy použité na vlastnosti ve třídě metadat určit formátování, ověření, pravidla a chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fbd48-153">The attributes applied to properties in the metadata class specify formatting, validation, rules, and error messages.</span></span> <span data-ttu-id="fbd48-154">Atributy uvedené v tomto poli mít následující výsledky:</span><span class="sxs-lookup"><span data-stu-id="fbd48-154">The attributes shown here will have the following results:</span></span>

- <span data-ttu-id="fbd48-155">`EnrollmentDate` Zobrazí se jako datum (bez času).</span><span class="sxs-lookup"><span data-stu-id="fbd48-155">`EnrollmentDate` will display as a date (without a time).</span></span>
- <span data-ttu-id="fbd48-156">Musí být obě pole Název 25 znaků nebo méně, a vlastní chybové zprávy je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fbd48-156">Both name fields must be 25 characters or less in length, and a custom error message is provided.</span></span>
- <span data-ttu-id="fbd48-157">Jsou požadovány obě pole název a vlastní chybové zprávy je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fbd48-157">Both name fields are required, and a custom error message is provided.</span></span>

<span data-ttu-id="fbd48-158">Spustit *Students.aspx* stránku znovu nezobrazovat a uvidíte, že data se teď zobrazují bez časy:</span><span class="sxs-lookup"><span data-stu-id="fbd48-158">Run the *Students.aspx* page again, and you see that the dates are now displayed without times:</span></span>

<span data-ttu-id="fbd48-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-159">[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)</span></span>

<span data-ttu-id="fbd48-160">Upravit řádek a zkuste vymažte hodnoty v poli název.</span><span class="sxs-lookup"><span data-stu-id="fbd48-160">Edit a row and try to clear the values in the name fields.</span></span> <span data-ttu-id="fbd48-161">Hvězdičky označující chyby pole se zobrazí, co nejdříve nechte pole, před kliknutím na **aktualizace**.</span><span class="sxs-lookup"><span data-stu-id="fbd48-161">The asterisks indicating field errors appear as soon as you leave a field, before you click **Update**.</span></span> <span data-ttu-id="fbd48-162">Po kliknutí na **aktualizace**, na stránce se zobrazí zadaný text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fbd48-162">When you click **Update**, the page displays the error message text you specified.</span></span>

<span data-ttu-id="fbd48-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-163">[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)</span></span>

<span data-ttu-id="fbd48-164">Zkuste zadat názvy, které jsou maximálně 25 znaků, klikněte na tlačítko **aktualizace**, a na stránce se zobrazí zadaný text chybové zprávy.</span><span class="sxs-lookup"><span data-stu-id="fbd48-164">Try to enter names that are longer than 25 characters, click **Update**, and the page displays the error message text you specified.</span></span>

<span data-ttu-id="fbd48-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="fbd48-165">[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)</span></span>

<span data-ttu-id="fbd48-166">Teď, když jste nastavili tato pravidla formátování i ověřování v metadatech datového modelu, pravidla se automaticky použijí na každou stránku, která zobrazí nebo umožňuje, aby změny těchto polí, tak dlouho, dokud používáte `DynamicControl` nebo `DynamicField` ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="fbd48-166">Now that you've set up these formatting and validation rules in the data model metadata, the rules will automatically be applied on every page that displays or allows changes to these fields, so long as you use `DynamicControl` or `DynamicField` controls.</span></span> <span data-ttu-id="fbd48-167">To snižuje množství redundantní kód, který musíte napsat, takže programování a testování jednodušší a zajišťuje, že data formátování i ověřování jsou konzistentní vzhledem k aplikacím v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fbd48-167">This reduces the amount of redundant code you have to write, which makes programming and testing easier, and it ensures that data formatting and validation are consistent throughout an application.</span></span>

## <a name="more-information"></a><span data-ttu-id="fbd48-168">Další informace</span><span class="sxs-lookup"><span data-stu-id="fbd48-168">More Information</span></span>

<span data-ttu-id="fbd48-169">Tím končí této sérii kurzů na Začínáme s Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="fbd48-169">This concludes this series of tutorials on Getting Started with the Entity Framework.</span></span> <span data-ttu-id="fbd48-170">Další zdroje vám pomůžou získat informace tom, jak používat rozhraní Entity Framework, pokračujte [z prvního kurzu další Entity Framework sérii](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) nebo naleznete na následujících stránkách:</span><span class="sxs-lookup"><span data-stu-id="fbd48-170">For more resources to help you learn how to use the Entity Framework, continue with [the first tutorial in the next Entity Framework tutorial series](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md) or visit the following sites:</span></span>

- [<span data-ttu-id="fbd48-171">Entity Framework – nejčastější dotazy</span><span class="sxs-lookup"><span data-stu-id="fbd48-171">Entity Framework FAQ</span></span>](http://www.ef-faq.org/introduction.html)
- [<span data-ttu-id="fbd48-172">Blog týmu Entity Framework</span><span class="sxs-lookup"><span data-stu-id="fbd48-172">The Entity Framework Team Blog</span></span>](https://blogs.msdn.com/b/adonet/)
- [<span data-ttu-id="fbd48-173">Entity Framework v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="fbd48-173">Entity Framework in the MSDN Library</span></span>](https://msdn.microsoft.com/library/bb399572.aspx)
- [<span data-ttu-id="fbd48-174">Entity Framework v datovém centru pro vývojáře MSDN</span><span class="sxs-lookup"><span data-stu-id="fbd48-174">Entity Framework in the MSDN Data Developer Center</span></span>](https://msdn.microsoft.com/data/ef.aspx)
- [<span data-ttu-id="fbd48-175">Přehled ovládacího prvku EntityDataSource webového serveru v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="fbd48-175">EntityDataSource Web Server Control Overview in the MSDN Library</span></span>](https://msdn.microsoft.com/library/cc488502.aspx)
- [<span data-ttu-id="fbd48-176">Ovládací prvek EntityDataSource reference k rozhraní API v knihovně MSDN</span><span class="sxs-lookup"><span data-stu-id="fbd48-176">EntityDataSource control API reference in the MSDN Library</span></span>](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [<span data-ttu-id="fbd48-177">Entity Framework fóra na webu MSDN</span><span class="sxs-lookup"><span data-stu-id="fbd48-177">Entity Framework Forums on MSDN</span></span>](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [<span data-ttu-id="fbd48-178">Julie Lerman blogu</span><span class="sxs-lookup"><span data-stu-id="fbd48-178">Julie Lerman's blog</span></span>](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [<span data-ttu-id="fbd48-179">Předchozí</span><span class="sxs-lookup"><span data-stu-id="fbd48-179">Previous</span></span>](the-entity-framework-and-aspnet-getting-started-part-7.md)
