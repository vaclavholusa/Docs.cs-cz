---
title: Stránky Razor s EF jádra ASP.NET Core - Model dat – 5 8
author: rick-anderson
description: V tomto kurzu přidejte další entity a vztahy a přizpůsobit datový model zadáním formátování, ověření a pravidla mapování.
manager: wpickett
ms.author: riande
ms.date: 10/25/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/complex-data-model
ms.openlocfilehash: bf643cab3b82a9e2138403c6c61d1c15c85abfbd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---data-model---5-of-8"></a><span data-ttu-id="f1447-103">Stránky Razor s EF jádra ASP.NET Core - Model dat – 5 8</span><span class="sxs-lookup"><span data-stu-id="f1447-103">Razor Pages with EF Core in ASP.NET Core - Data Model - 5 of 8</span></span>

<span data-ttu-id="f1447-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f1447-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f1447-105">Předchozí kurzy pracovali s základní datový model, který se skládá z tři entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-105">The previous tutorials worked with a basic data model that was composed of three entities.</span></span> <span data-ttu-id="f1447-106">V tomto kurzu:</span><span class="sxs-lookup"><span data-stu-id="f1447-106">In this tutorial:</span></span>

* <span data-ttu-id="f1447-107">Se přidají další entity a vztahy.</span><span class="sxs-lookup"><span data-stu-id="f1447-107">More entities and relationships are added.</span></span>
* <span data-ttu-id="f1447-108">Datový model je přizpůsobit tak, že zadáte formátování, ověřování a pravidla mapování databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-108">The data model is customized by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="f1447-109">Třídy entity pro dokončené datový model je vidět na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="f1447-109">The entity classes for the completed data model is shown in the following illustration:</span></span>

![Entity diagram](complex-data-model/_static/diagram.png)

<span data-ttu-id="f1447-111">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span><span class="sxs-lookup"><span data-stu-id="f1447-111">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part5-complex).</span></span>

## <a name="customize-the-data-model-with-attributes"></a><span data-ttu-id="f1447-112">Přizpůsobení datového modelu s atributy</span><span class="sxs-lookup"><span data-stu-id="f1447-112">Customize the data model with attributes</span></span>

<span data-ttu-id="f1447-113">V této části je datový model přizpůsobit pomocí atributů.</span><span class="sxs-lookup"><span data-stu-id="f1447-113">In this section, the data model is customized using attributes.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="f1447-114">Datový typ atributu</span><span class="sxs-lookup"><span data-stu-id="f1447-114">The DataType attribute</span></span>

<span data-ttu-id="f1447-115">Na stránkách student aktuálně zobrazí čas, datum registrace.</span><span class="sxs-lookup"><span data-stu-id="f1447-115">The student pages currently displays the time of the enrollment date.</span></span> <span data-ttu-id="f1447-116">Obvykle polí data zobrazit pouze datum a není čas.</span><span class="sxs-lookup"><span data-stu-id="f1447-116">Typically, date fields show only the date and not the time.</span></span>

<span data-ttu-id="f1447-117">Aktualizace *Models/Student.cs* s následujícími službami zvýrazněná kódu:</span><span class="sxs-lookup"><span data-stu-id="f1447-117">Update *Models/Student.cs* with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="f1447-118">[Datový typ](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) atribut určuje datový typ, který je specifičtější než vnitřní typ databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-118">The [DataType](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatypeattribute?view=netframework-4.7.1) attribute specifies a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="f1447-119">V tomto případě kterou má být zobrazen pouze data, není datum a čas.</span><span class="sxs-lookup"><span data-stu-id="f1447-119">In this case only the date should be displayed, not the date and time.</span></span> <span data-ttu-id="f1447-120">[Datový typ výčtu](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress, atd. `DataType` Atributu můžete také povolit aplikaci automaticky získávat specifické pro typ funkce.</span><span class="sxs-lookup"><span data-stu-id="f1447-120">The [DataType Enumeration](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.datatype?view=netframework-4.7.1) provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, etc. The `DataType` attribute can also enable the app to automatically provide type-specific features.</span></span> <span data-ttu-id="f1447-121">Příklad:</span><span class="sxs-lookup"><span data-stu-id="f1447-121">For example:</span></span>

* <span data-ttu-id="f1447-122">`mailto:` Propojení se automaticky vytvoří pro `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="f1447-122">The `mailto:` link is automatically created for `DataType.EmailAddress`.</span></span>
* <span data-ttu-id="f1447-123">Je k dispozici pro výběr data `DataType.Date` ve většině prohlížečů.</span><span class="sxs-lookup"><span data-stu-id="f1447-123">The date selector is provided for `DataType.Date` in most browsers.</span></span>

<span data-ttu-id="f1447-124">`DataType` Atribut vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které využívají standardu HTML 5 prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="f1447-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="f1447-125">`DataType` Atributy neposkytují ověření.</span><span class="sxs-lookup"><span data-stu-id="f1447-125">The `DataType` attributes don't provide validation.</span></span>

<span data-ttu-id="f1447-126">`DataType.Date` neuvádí formát data, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="f1447-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="f1447-127">Ve výchozím nastavení, zobrazí se pole datum podle výchozích formátů podle serveru [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span><span class="sxs-lookup"><span data-stu-id="f1447-127">By default, the date field is displayed according to the default formats based on the server's [CultureInfo](https://docs.microsoft.com/aspnet/core/fundamentals/localization#provide-localized-resources-for-the-languages-and-cultures-you-support).</span></span>

<span data-ttu-id="f1447-128">`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:</span><span class="sxs-lookup"><span data-stu-id="f1447-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="f1447-129">`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito pro úpravy uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="f1447-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied to the edit UI.</span></span> <span data-ttu-id="f1447-130">Některá pole neměli používat `ApplyFormatInEditMode`.</span><span class="sxs-lookup"><span data-stu-id="f1447-130">Some fields shouldn't use `ApplyFormatInEditMode`.</span></span> <span data-ttu-id="f1447-131">Například by se neměly symbolu měny zobrazovat obecně do textového pole na Upravit.</span><span class="sxs-lookup"><span data-stu-id="f1447-131">For example, the currency symbol should generally not be displayed in an edit text box.</span></span>

<span data-ttu-id="f1447-132">`DisplayFormat` Atribut lze použít samostatně.</span><span class="sxs-lookup"><span data-stu-id="f1447-132">The `DisplayFormat` attribute can be used by itself.</span></span> <span data-ttu-id="f1447-133">Obecně je vhodné používat `DataType` atribut s `DisplayFormat` atribut.</span><span class="sxs-lookup"><span data-stu-id="f1447-133">It's generally a good idea to use the `DataType` attribute with the `DisplayFormat` attribute.</span></span> <span data-ttu-id="f1447-134">`DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce.</span><span class="sxs-lookup"><span data-stu-id="f1447-134">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen.</span></span> <span data-ttu-id="f1447-135">`DataType` Atribut poskytuje následující výhody, které nejsou k dispozici v `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="f1447-135">The `DataType` attribute provides the following benefits that are not available in `DisplayFormat`:</span></span>

* <span data-ttu-id="f1447-136">V prohlížeči můžete povolit funkce HTML5.</span><span class="sxs-lookup"><span data-stu-id="f1447-136">The browser can enable HTML5 features.</span></span> <span data-ttu-id="f1447-137">Například zobrazit ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, vstupní ověřování na straně klienta, atd.</span><span class="sxs-lookup"><span data-stu-id="f1447-137">For example, show a calendar control, the locale-appropriate currency symbol, email links, client-side input validation, etc.</span></span>
* <span data-ttu-id="f1447-138">Ve výchozím prohlížeči vykreslí data ve správném formátu podle národního prostředí.</span><span class="sxs-lookup"><span data-stu-id="f1447-138">By default, the browser renders data using the correct format based on the locale.</span></span>

<span data-ttu-id="f1447-139">Další informace najdete v tématu [ \<vstupní > Pomocník značky dokumentace](xref:mvc/views/working-with-forms#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f1447-139">For more information, see the [\<input> Tag Helper documentation](xref:mvc/views/working-with-forms#the-input-tag-helper).</span></span>

<span data-ttu-id="f1447-140">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1447-140">Run the app.</span></span> <span data-ttu-id="f1447-141">Přejděte na stránku studenty Index.</span><span class="sxs-lookup"><span data-stu-id="f1447-141">Navigate to the Students Index page.</span></span> <span data-ttu-id="f1447-142">Časy se už nezobrazují.</span><span class="sxs-lookup"><span data-stu-id="f1447-142">Times are no longer displayed.</span></span> <span data-ttu-id="f1447-143">Každé zobrazení, která používá `Student` modelu zobrazuje datum bez čas.</span><span class="sxs-lookup"><span data-stu-id="f1447-143">Every view that uses the `Student` model displays the date without time.</span></span>

![Studenti, kteří index stránky zobrazující data bez časů](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="f1447-145">Atribut StringLength</span><span class="sxs-lookup"><span data-stu-id="f1447-145">The StringLength attribute</span></span>

<span data-ttu-id="f1447-146">S atributy lze zadat pravidla ověření dat a chybové zprávy ověření.</span><span class="sxs-lookup"><span data-stu-id="f1447-146">Data validation rules and validation error messages can be specified with attributes.</span></span> <span data-ttu-id="f1447-147">[StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) atribut určuje minimální a maximální délku znaků, které jsou v datovém poli.</span><span class="sxs-lookup"><span data-stu-id="f1447-147">The [StringLength](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.stringlengthattribute?view=netframework-4.7.1) attribute specifies the minimum and maximum length of characters that are allowed in a data field.</span></span> <span data-ttu-id="f1447-148">`StringLength` Atribut poskytuje také ověření na straně klienta a na straně serveru.</span><span class="sxs-lookup"><span data-stu-id="f1447-148">The `StringLength` attribute also provides client-side and server-side validation.</span></span> <span data-ttu-id="f1447-149">Minimální hodnota nemá žádný vliv na schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-149">The minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="f1447-150">Aktualizace `Student` modelu s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-150">Update the `Student` model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="f1447-151">Předchozí kód omezuje názvy k více než 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="f1447-151">The preceding code limits names to no more than 50 characters.</span></span> <span data-ttu-id="f1447-152">`StringLength` Atribut nemá uživatel zabránit v přechodu do prázdných znaků pro název.</span><span class="sxs-lookup"><span data-stu-id="f1447-152">The `StringLength` attribute doesn't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="f1447-153">[Regulární výraz](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) atribut se používá k aplikování omezení na vstup.</span><span class="sxs-lookup"><span data-stu-id="f1447-153">The [RegularExpression](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.regularexpressionattribute?view=netframework-4.7.1) attribute is used to apply restrictions to the input.</span></span> <span data-ttu-id="f1447-154">Například následující kód vyžaduje první znak, který má být velkými písmeny a zbývající znaků, které mají být abecední:</span><span class="sxs-lookup"><span data-stu-id="f1447-154">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="f1447-155">Spuštění aplikace:</span><span class="sxs-lookup"><span data-stu-id="f1447-155">Run the app:</span></span>

* <span data-ttu-id="f1447-156">Přejděte na stránku studenty.</span><span class="sxs-lookup"><span data-stu-id="f1447-156">Navigate to the Students page.</span></span>
* <span data-ttu-id="f1447-157">Vyberte **vytvořit nový**a zadejte název, který je delší než 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="f1447-157">Select **Create New**, and enter a name longer than 50 characters.</span></span>
* <span data-ttu-id="f1447-158">Vyberte **vytvořit**, zobrazuje chybovou zprávu ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="f1447-158">Select **Create**, client-side validation shows an error message.</span></span>

![Studenti, kteří index stránky zobrazující chyby délka řetězce](complex-data-model/_static/string-length-errors.png)

<span data-ttu-id="f1447-160">V **Průzkumník objektů systému SQL Server** (SSOX), otevřete návrháře tabulky Student dvojitým kliknutím **Student** tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-160">In **SQL Server Object Explorer** (SSOX), open the Student table designer by double-clicking the **Student** table.</span></span>

![Studenti, kteří tabulku v SSOX před migrací](complex-data-model/_static/ssox-before-migration.png)

<span data-ttu-id="f1447-162">Předchozí obrázek znázorňuje schéma pro `Student` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-162">The preceding image shows the schema for the `Student` table.</span></span> <span data-ttu-id="f1447-163">Název pole mít typ `nvarchar(MAX)` vzhledem k tomu, že migrace nebyl spuštěn v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-163">The name fields have type `nvarchar(MAX)` because migrations has not been run on the DB.</span></span> <span data-ttu-id="f1447-164">Když migrace se spustí později v tomto kurzu, názvu pole se stanou `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="f1447-164">When migrations are run later in this tutorial, the name fields become `nvarchar(50)`.</span></span>

### <a name="the-column-attribute"></a><span data-ttu-id="f1447-165">Atribut sloupce</span><span class="sxs-lookup"><span data-stu-id="f1447-165">The Column attribute</span></span>

<span data-ttu-id="f1447-166">Atributy můžete řídit, jak třídy a vlastnosti jsou mapované na databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-166">Attributes can control how classes and properties are mapped to the database.</span></span> <span data-ttu-id="f1447-167">V této části `Column` atribut se používá k mapování názvu `FirstMidName` vlastnost "FirstName" v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-167">In this section, the `Column` attribute is used to map the name of the `FirstMidName` property to "FirstName" in the DB.</span></span>

<span data-ttu-id="f1447-168">Při vytváření databáze názvy vlastností na modelu se používají pro názvy sloupců (s výjimkou, kdy `Column` je použit atribut).</span><span class="sxs-lookup"><span data-stu-id="f1447-168">When the DB is created, property names on the model are used for column names (except when the `Column` attribute is used).</span></span>

<span data-ttu-id="f1447-169">`Student` Model používá `FirstMidName` pro první název pole, protože pole může obsahovat také křestní jméno.</span><span class="sxs-lookup"><span data-stu-id="f1447-169">The `Student` model uses `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span>

<span data-ttu-id="f1447-170">Aktualizace *Student.cs* soubor s následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="f1447-170">Update the *Student.cs* file with the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="f1447-171">S předchozí změny `Student.FirstMidName` v aplikaci mapuje `FirstName` sloupec `Student` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-171">With the preceding change, `Student.FirstMidName` in the app maps to the `FirstName` column of the `Student` table.</span></span>

<span data-ttu-id="f1447-172">Přidání `Column` základní model, změní se atribut `SchoolContext`.</span><span class="sxs-lookup"><span data-stu-id="f1447-172">The addition of the `Column` attribute changes the model backing the `SchoolContext`.</span></span> <span data-ttu-id="f1447-173">Základní model `SchoolContext` již neodpovídá databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-173">The model backing the `SchoolContext` no longer matches the database.</span></span> <span data-ttu-id="f1447-174">Pokud aplikace běží před použitím migrace, vygeneruje se následující výjimky:</span><span class="sxs-lookup"><span data-stu-id="f1447-174">If the app is run before applying migrations, the following exception is generated:</span></span>

```SQL
SqlException: Invalid column name 'FirstName'.
```
<span data-ttu-id="f1447-175">Aktualizace databáze:</span><span class="sxs-lookup"><span data-stu-id="f1447-175">To update the DB:</span></span>

* <span data-ttu-id="f1447-176">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="f1447-176">Build the project.</span></span>
* <span data-ttu-id="f1447-177">Otevřete okno příkazového řádku ve složce projektu.</span><span class="sxs-lookup"><span data-stu-id="f1447-177">Open a command window in the project folder.</span></span> <span data-ttu-id="f1447-178">Zadejte následující příkazy k vytvoření nové migrace a aktualizaci databáze:</span><span class="sxs-lookup"><span data-stu-id="f1447-178">Enter the following commands to create a new migration and update the DB:</span></span>

    ```console
    dotnet ef migrations add ColumnFirstName
    dotnet ef database update
    ```

<span data-ttu-id="f1447-179">`dotnet ef migrations add ColumnFirstName` Příkaz generuje následující upozornění:</span><span class="sxs-lookup"><span data-stu-id="f1447-179">The `dotnet ef migrations add ColumnFirstName` command generates the following warning message:</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
```

<span data-ttu-id="f1447-180">Upozornění je vygenerovat, protože název pole jsou nyní omezena na 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="f1447-180">The warning is generated because the name fields are now limited to 50 characters.</span></span> <span data-ttu-id="f1447-181">Pokud název v databázi více než 50 znaků, by dojít ke ztrátě 51 poslední znak.</span><span class="sxs-lookup"><span data-stu-id="f1447-181">If a name in the DB had more than 50 characters, the 51 to last character would be lost.</span></span>

* <span data-ttu-id="f1447-182">Testování aplikací.</span><span class="sxs-lookup"><span data-stu-id="f1447-182">Test the app.</span></span>

<span data-ttu-id="f1447-183">Tabulka Student otevřete v SSOX:</span><span class="sxs-lookup"><span data-stu-id="f1447-183">Open the Student table in SSOX:</span></span>

![Studenti, kteří tabulky v SSOX po migrace](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="f1447-185">Před použitím migrace, byl měla název sloupce typu [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span><span class="sxs-lookup"><span data-stu-id="f1447-185">Before migration was applied, the name columns were of type [nvarchar(MAX)](https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql).</span></span> <span data-ttu-id="f1447-186">Název sloupce jsou nyní `nvarchar(50)`.</span><span class="sxs-lookup"><span data-stu-id="f1447-186">The name columns are now `nvarchar(50)`.</span></span> <span data-ttu-id="f1447-187">Název sloupce změněn z `FirstMidName` k `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="f1447-187">The column name has changed from `FirstMidName` to `FirstName`.</span></span>

> [!Note]
> <span data-ttu-id="f1447-188">V následující části sestavení aplikace ve některé fázích generuje chyby kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="f1447-188">In the following section, building the app at some stages generates compiler errors.</span></span> <span data-ttu-id="f1447-189">Podle pokynů určete, kdy k sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="f1447-189">The instructions specify when to build the app.</span></span>

## <a name="student-entity-update"></a><span data-ttu-id="f1447-190">Aktualizace entity studenty</span><span class="sxs-lookup"><span data-stu-id="f1447-190">Student entity update</span></span>

![Student entity](complex-data-model/_static/student-entity.png)

<span data-ttu-id="f1447-192">Aktualizace *Models/Student.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-192">Update *Models/Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="f1447-193">Požadovaný atribut</span><span class="sxs-lookup"><span data-stu-id="f1447-193">The Required attribute</span></span>

<span data-ttu-id="f1447-194">`Required` Atribut je povinná pole název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1447-194">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="f1447-195">`Required` Atribut není nutný pro použití hodnot Null typy, jako jsou typy hodnot (`DateTime`, `int`, `double`atd.).</span><span class="sxs-lookup"><span data-stu-id="f1447-195">The `Required` attribute isn't needed for non-nullable types such as value types (`DateTime`, `int`, `double`, etc.).</span></span> <span data-ttu-id="f1447-196">Typy, které nemůže mít hodnotu null, jsou automaticky považovány za povinná pole.</span><span class="sxs-lookup"><span data-stu-id="f1447-196">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="f1447-197">`Required` Atribut může být nahrazena minimální délku parametru v `StringLength` atribut:</span><span class="sxs-lookup"><span data-stu-id="f1447-197">The `Required` attribute could be replaced with a minimum length parameter in the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="f1447-198">Atribut zobrazení</span><span class="sxs-lookup"><span data-stu-id="f1447-198">The Display attribute</span></span>

<span data-ttu-id="f1447-199">`Display` Atribut určuje, že titulek pro do textových polí musí být "Jméno", "Příjmení", "Úplný název" a "Datum registrace."</span><span class="sxs-lookup"><span data-stu-id="f1447-199">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date."</span></span> <span data-ttu-id="f1447-200">Titulky výchozí neobsahoval mezeru dělení slova, například "Příjmení".</span><span class="sxs-lookup"><span data-stu-id="f1447-200">The default captions had no space dividing the words, for example "Lastname."</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="f1447-201">Vlastnost FullName vypočítat</span><span class="sxs-lookup"><span data-stu-id="f1447-201">The FullName calculated property</span></span>

<span data-ttu-id="f1447-202">`FullName` je počítané vlastnosti, která vrátí hodnotu, která se vytvoří zřetězením dva další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1447-202">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="f1447-203">`FullName` Nelze nastavit, že obsahuje pouze přistupující objekt get.</span><span class="sxs-lookup"><span data-stu-id="f1447-203">`FullName` cannot be set, it has only a get accessor.</span></span> <span data-ttu-id="f1447-204">Ne `FullName` sloupec se vytvoří v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-204">No `FullName` column is created in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="f1447-205">Vytvořit entitu lektorem</span><span class="sxs-lookup"><span data-stu-id="f1447-205">Create the Instructor Entity</span></span>

![Entitu lektorem](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="f1447-207">Vytvoření *Models/Instructor.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-207">Create *Models/Instructor.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="f1447-208">Všimněte si, že několik vlastností, které jsou stejné ve `Student` a `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-208">Notice that several properties are the same in the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="f1447-209">V tomto kurzu implementace dědičnosti později z této série tento kód je teď vyčleněný eliminovat redundance.</span><span class="sxs-lookup"><span data-stu-id="f1447-209">In the Implementing Inheritance tutorial later in this series, this code is refactored to eliminate the redundancy.</span></span>

<span data-ttu-id="f1447-210">Více atributů může být na jednom řádku.</span><span class="sxs-lookup"><span data-stu-id="f1447-210">Multiple attributes can be on one line.</span></span> <span data-ttu-id="f1447-211">`HireDate` Atributy může zapsat takto:</span><span class="sxs-lookup"><span data-stu-id="f1447-211">The `HireDate` attributes could be written as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="f1447-212">CourseAssignments a OfficeAssignment navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f1447-212">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="f1447-213">`CourseAssignments` a `OfficeAssignment` vlastnosti jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1447-213">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="f1447-214">Lektorem můžete naučit libovolný počet kurzy, takže `CourseAssignments` je definována jako kolekce.</span><span class="sxs-lookup"><span data-stu-id="f1447-214">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f1447-215">Pokud navigační vlastnost obsahuje více entit:</span><span class="sxs-lookup"><span data-stu-id="f1447-215">If a navigation property holds multiple entities:</span></span>

* <span data-ttu-id="f1447-216">Musí být typu seznamu kde položky lze přidat, odstranit a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="f1447-216">It must be a list type where the entries can be added, deleted, and updated.</span></span>

<span data-ttu-id="f1447-217">Navigační vlastnost typy patří:</span><span class="sxs-lookup"><span data-stu-id="f1447-217">Navigation property types include:</span></span>

* `ICollection<T>`
*  `List<T>`
*  `HashSet<T>`

<span data-ttu-id="f1447-218">Pokud `ICollection<T>` není zadaný, vytvoří základní EF `HashSet<T>` kolekce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="f1447-218">If `ICollection<T>` is specified, EF Core creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="f1447-219">`CourseAssignment` Entity je popsáno v části u relací m: n.</span><span class="sxs-lookup"><span data-stu-id="f1447-219">The `CourseAssignment` entity is explained in the section on many-to-many relationships.</span></span>

<span data-ttu-id="f1447-220">Contoso univerzity obchodní pravidla stavu, že lektorem může mít maximálně jeden office.</span><span class="sxs-lookup"><span data-stu-id="f1447-220">Contoso University business rules state that an instructor can have at most one office.</span></span> <span data-ttu-id="f1447-221">`OfficeAssignment` Vlastnost obsahuje jeden `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-221">The `OfficeAssignment` property holds a single `OfficeAssignment` entity.</span></span> <span data-ttu-id="f1447-222">`OfficeAssignment` má hodnotu null, pokud není přiřazena žádná office.</span><span class="sxs-lookup"><span data-stu-id="f1447-222">`OfficeAssignment` is null if no office is assigned.</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="f1447-223">Vytvořit entitu OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="f1447-223">Create the OfficeAssignment entity</span></span>

![OfficeAssignment entity](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="f1447-225">Vytvoření *Models/OfficeAssignment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-225">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="f1447-226">Klíč atributu</span><span class="sxs-lookup"><span data-stu-id="f1447-226">The Key attribute</span></span>

<span data-ttu-id="f1447-227">`[Key]` Atribut se používá k identifikaci vlastnost jako primární klíč (Primárníklíč) Pokud je vlastnost název něco než classnameID nebo ID.</span><span class="sxs-lookup"><span data-stu-id="f1447-227">The `[Key]` attribute is used to identify a property as the primary key (PK) when the property name is something other than classnameID or ID.</span></span>

<span data-ttu-id="f1447-228">Je--nula nebo 1 vztah mezi `Instructor` a `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-228">There's a one-to-zero-or-one relationship between the `Instructor` and `OfficeAssignment` entities.</span></span> <span data-ttu-id="f1447-229">Přiřazení office existuje pouze ve vztahu k lektorem, které je přiřazen.</span><span class="sxs-lookup"><span data-stu-id="f1447-229">An office assignment only exists in relation to the instructor it's assigned to.</span></span> <span data-ttu-id="f1447-230">`OfficeAssignment` Primárníklíč je také jeho cizí klíč (Cizíklíč) `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-230">The `OfficeAssignment` PK is also its foreign key (FK) to the `Instructor` entity.</span></span> <span data-ttu-id="f1447-231">Základní EF nelze rozpoznat automaticky `InstructorID` jako Primárníklíč z `OfficeAssignment` protože:</span><span class="sxs-lookup"><span data-stu-id="f1447-231">EF Core can't automatically recognize `InstructorID` as the PK of `OfficeAssignment` because:</span></span>

* <span data-ttu-id="f1447-232">`InstructorID` není podle ID nebo classnameID zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="f1447-232">`InstructorID` doesn't follow the ID or classnameID naming convention.</span></span>

<span data-ttu-id="f1447-233">Proto `Key` atribut se používá k identifikaci `InstructorID` jako primárnímu Klíči:</span><span class="sxs-lookup"><span data-stu-id="f1447-233">Therefore, the `Key` attribute is used to identify `InstructorID` as the PK:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="f1447-234">Ve výchozím nastavení EF základní klíče jsou považovány za jiný databáze generované protože sloupec je sloupec pro identifikační vztah.</span><span class="sxs-lookup"><span data-stu-id="f1447-234">By default, EF Core treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="f1447-235">Vlastnost navigace lektorem</span><span class="sxs-lookup"><span data-stu-id="f1447-235">The Instructor navigation property</span></span>

<span data-ttu-id="f1447-236">`OfficeAssignment` Navigační vlastnost pro `Instructor` entita může mít hodnotu Null protože:</span><span class="sxs-lookup"><span data-stu-id="f1447-236">The `OfficeAssignment` navigation property for the `Instructor` entity is nullable because:</span></span>

* <span data-ttu-id="f1447-237">Referenční typy (například třídy jsou s možnou hodnotou Null).</span><span class="sxs-lookup"><span data-stu-id="f1447-237">Reference types (such as classes are nullable).</span></span>
* <span data-ttu-id="f1447-238">Lektorem nemusí mít přiřazení office.</span><span class="sxs-lookup"><span data-stu-id="f1447-238">An instructor might not have an office assignment.</span></span>


<span data-ttu-id="f1447-239">`OfficeAssignment` Entita má hodnotou Null `Instructor` navigační vlastnost protože:</span><span class="sxs-lookup"><span data-stu-id="f1447-239">The `OfficeAssignment` entity has a non-nullable `Instructor` navigation property because:</span></span>

* <span data-ttu-id="f1447-240">`InstructorID` je použití hodnot Null.</span><span class="sxs-lookup"><span data-stu-id="f1447-240">`InstructorID` is non-nullable.</span></span>
* <span data-ttu-id="f1447-241">Přiřazení office nemůže existovat bez lektorem.</span><span class="sxs-lookup"><span data-stu-id="f1447-241">An office assignment can't exist without an instructor.</span></span>

<span data-ttu-id="f1447-242">Když `Instructor` entita má související `OfficeAssignment` entity, každá entita odkazuje na jinou v její navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="f1447-242">When an `Instructor` entity has a related `OfficeAssignment` entity, each entity has a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="f1447-243">`[Required]` Atribut může být použit na `Instructor` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="f1447-243">The `[Required]` attribute could be applied to the `Instructor` navigation property:</span></span>

```csharp
[Required]
public Instructor Instructor { get; set; }
```

<span data-ttu-id="f1447-244">Předchozí kód určuje, že musí být související lektorem.</span><span class="sxs-lookup"><span data-stu-id="f1447-244">The preceding code specifies that there must be a related instructor.</span></span> <span data-ttu-id="f1447-245">Předchozí kód není nutný, protože `InstructorID` cizí klíč (což je také primárnímu Klíči) je použití hodnot Null.</span><span class="sxs-lookup"><span data-stu-id="f1447-245">The preceding code is unnecessary because the `InstructorID` foreign key (which is also the PK) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="f1447-246">Upravit kurzu Entity</span><span class="sxs-lookup"><span data-stu-id="f1447-246">Modify the Course Entity</span></span>

![Během entity](complex-data-model/_static/course-entity.png)

<span data-ttu-id="f1447-248">Aktualizace *Models/Course.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-248">Update *Models/Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="f1447-249">`Course` Entity má vlastnosti cizího klíče (Cizíklíč) `DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="f1447-249">The `Course` entity has a foreign key (FK) property `DepartmentID`.</span></span> <span data-ttu-id="f1447-250">`DepartmentID` odkazuje na související `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-250">`DepartmentID` points to the related `Department` entity.</span></span> <span data-ttu-id="f1447-251">`Course` Entita, která má `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f1447-251">The `Course` entity has a `Department` navigation property.</span></span>

<span data-ttu-id="f1447-252">Základní EF nevyžaduje vlastnosti cizího klíče u datového modelu, když model má navigační vlastnost pro související entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-252">EF Core doesn't require a FK property for a data model when the model has a navigation property for a related entity.</span></span>

<span data-ttu-id="f1447-253">FKs EF základní automaticky vytvoří v databázi, bez ohledu na jejich jste potřeby.</span><span class="sxs-lookup"><span data-stu-id="f1447-253">EF Core automatically creates FKs in the database wherever they're needed.</span></span> <span data-ttu-id="f1447-254">Vytvoří základní EF [stínové vlastnosti](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pro automaticky vytvořené FKs.</span><span class="sxs-lookup"><span data-stu-id="f1447-254">EF Core creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for automatically created FKs.</span></span> <span data-ttu-id="f1447-255">S cizího klíče v datovém modelu, můžete provést aktualizace teď jednodušší a efektivnější.</span><span class="sxs-lookup"><span data-stu-id="f1447-255">Having the FK in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="f1447-256">Představte si třeba model kde vlastnost cizího klíče `DepartmentID` je *není* zahrnuty.</span><span class="sxs-lookup"><span data-stu-id="f1447-256">For example, consider a model where the FK property `DepartmentID` is *not* included.</span></span> <span data-ttu-id="f1447-257">Pokud je během entity načtených upravit:</span><span class="sxs-lookup"><span data-stu-id="f1447-257">When a course entity is fetched to edit:</span></span>

* <span data-ttu-id="f1447-258">`Department` Entity má hodnotu null, pokud není výslovně načtení.</span><span class="sxs-lookup"><span data-stu-id="f1447-258">The `Department` entity is null if it's not explicitly loaded.</span></span>
* <span data-ttu-id="f1447-259">K aktualizaci entity kurzu `Department` nejprve musí být získána entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-259">To update the course entity, the `Department` entity must first be fetched.</span></span>

<span data-ttu-id="f1447-260">Když vlastnost cizího klíče `DepartmentID` je zahrnutá v datovém modelu, není nutné načíst `Department` entity před aktualizace.</span><span class="sxs-lookup"><span data-stu-id="f1447-260">When the FK property `DepartmentID` is included in the data model, there's no need to fetch the `Department` entity before an update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="f1447-261">Atribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="f1447-261">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="f1447-262">`[DatabaseGenerated(DatabaseGeneratedOption.None)]` Atribut určuje, že primárnímu Klíči je součástí aplikace místo generované databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-262">The `[DatabaseGenerated(DatabaseGeneratedOption.None)]` attribute specifies that the PK is provided by the application rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="f1447-263">Ve výchozím nastavení EF základní předpokládá, že hodnoty Primárníklíč generované databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-263">By default, EF Core assumes that PK values are generated by the DB.</span></span> <span data-ttu-id="f1447-264">DB generované Primárníklíč hodnoty je obecně nejlepší metodou.</span><span class="sxs-lookup"><span data-stu-id="f1447-264">DB generated PK values is generally the best approach.</span></span> <span data-ttu-id="f1447-265">Pro `Course` určuje entity, uživatel PK.</span><span class="sxs-lookup"><span data-stu-id="f1447-265">For `Course` entities, the user specifies the PK.</span></span> <span data-ttu-id="f1447-266">Například během číslo jako řadu 1000 oddělení matematické, 2000 řady pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="f1447-266">For example, a course number such as a 1000 series for the math department, a 2000 series for the English department.</span></span>

<span data-ttu-id="f1447-267">`DatabaseGenerated` Atribut můžete také použít ke generování výchozí hodnoty.</span><span class="sxs-lookup"><span data-stu-id="f1447-267">The `DatabaseGenerated` attribute can also be used to generate default values.</span></span> <span data-ttu-id="f1447-268">Například databáze může automaticky generovat pole pro datum k zaznamenání datum vytvoření nebo aktualizaci řádku.</span><span class="sxs-lookup"><span data-stu-id="f1447-268">For example, the DB can automatically generate a date field to record the date a row was created or updated.</span></span> <span data-ttu-id="f1447-269">Další informace najdete v tématu [generované vlastnosti](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="f1447-269">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f1447-270">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f1447-270">Foreign key and navigation properties</span></span>

<span data-ttu-id="f1447-271">Vlastnosti cizího klíče (Cizíklíč) a navigačních vlastností v `Course` entity podle následující relace:</span><span class="sxs-lookup"><span data-stu-id="f1447-271">The foreign key (FK) properties and navigation properties in the `Course` entity reflect the following relationships:</span></span>

<span data-ttu-id="f1447-272">Kurz je přiřazena k jedné oddělení, takže není `DepartmentID` cizího klíče a `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f1447-272">A course is assigned to one department, so there's a `DepartmentID` FK and a `Department` navigation property.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="f1447-273">Kurz může mít libovolný počet studenty zaregistrované v něm proto `Enrollments` navigační vlastnost je kolekce:</span><span class="sxs-lookup"><span data-stu-id="f1447-273">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="f1447-274">Kurz může výukové ve více vyučující, proto `CourseAssignments` navigační vlastnost je kolekce:</span><span class="sxs-lookup"><span data-stu-id="f1447-274">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection:</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="f1447-275">`CourseAssignment` je vysvětlen [později](#many-to-many-relationships).</span><span class="sxs-lookup"><span data-stu-id="f1447-275">`CourseAssignment` is explained [later](#many-to-many-relationships).</span></span>

## <a name="create-the-department-entity"></a><span data-ttu-id="f1447-276">Vytvořit entitu oddělení</span><span class="sxs-lookup"><span data-stu-id="f1447-276">Create the Department entity</span></span>

![Oddělení entity](complex-data-model/_static/department-entity.png)

<span data-ttu-id="f1447-278">Vytvoření *Models/Department.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-278">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="f1447-279">Atribut sloupce</span><span class="sxs-lookup"><span data-stu-id="f1447-279">The Column attribute</span></span>

<span data-ttu-id="f1447-280">Dříve `Column` atribut se použít ke změně mapování název sloupce.</span><span class="sxs-lookup"><span data-stu-id="f1447-280">Previously the `Column` attribute was used to change column name mapping.</span></span> <span data-ttu-id="f1447-281">V kódu `Department` entity, `Column` atribut se používá ke změně mapování datového typu SQL.</span><span class="sxs-lookup"><span data-stu-id="f1447-281">In the code for the `Department` entity, the `Column` attribute is used to change SQL data type mapping.</span></span> <span data-ttu-id="f1447-282">`Budget` Sloupec je definovaný typ money systému SQL Server v databázi:</span><span class="sxs-lookup"><span data-stu-id="f1447-282">The `Budget` column is defined using the SQL Server money type in the DB:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="f1447-283">Mapování sloupce není obvykle nutné.</span><span class="sxs-lookup"><span data-stu-id="f1447-283">Column mapping is generally not required.</span></span> <span data-ttu-id="f1447-284">Základní EF obecně zvolí příslušného typu dat systému SQL Server, na základě typu CLR pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="f1447-284">EF Core generally chooses the appropriate SQL Server data type based on the CLR type for the property.</span></span> <span data-ttu-id="f1447-285">Modul CLR `decimal` zadejte map k systému SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="f1447-285">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="f1447-286">`Budget` je currency, a datový typ money je vhodnější pro měny.</span><span class="sxs-lookup"><span data-stu-id="f1447-286">`Budget` is for currency, and the money data type is more appropriate for currency.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f1447-287">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f1447-287">Foreign key and navigation properties</span></span>

<span data-ttu-id="f1447-288">Vlastnosti cizího klíče a navigační odráží následující relace:</span><span class="sxs-lookup"><span data-stu-id="f1447-288">The FK and navigation properties reflect the following relationships:</span></span>

* <span data-ttu-id="f1447-289">Oddělení může nebo nemusí mít správcem.</span><span class="sxs-lookup"><span data-stu-id="f1447-289">A department may or may not have an administrator.</span></span>
* <span data-ttu-id="f1447-290">Správce je vždy lektorem.</span><span class="sxs-lookup"><span data-stu-id="f1447-290">An administrator is always an instructor.</span></span> <span data-ttu-id="f1447-291">Proto `InstructorID` vlastnost je zahrnuta jako cizího klíče na `Instructor` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-291">Therefore the `InstructorID` property is included as the FK to the `Instructor` entity.</span></span>

<span data-ttu-id="f1447-292">Navigační vlastnost jmenuje `Administrator` , ale obsahuje `Instructor` entity:</span><span class="sxs-lookup"><span data-stu-id="f1447-292">The navigation property is named `Administrator` but holds an `Instructor` entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="f1447-293">Otazník (?) v předchozím kódu určuje, jestli že vlastnost umožňuje hodnotu Null.</span><span class="sxs-lookup"><span data-stu-id="f1447-293">The question mark (?) in the preceding code specifies the property is nullable.</span></span>

<span data-ttu-id="f1447-294">Oddělení může mít mnoho kurzy, takže není navigační vlastnost kurzy:</span><span class="sxs-lookup"><span data-stu-id="f1447-294">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

<span data-ttu-id="f1447-295">Poznámka: Pomocí konvence, umožňuje EF základní kaskádové odstranění pro použití hodnot Null FKs a relace m: n.</span><span class="sxs-lookup"><span data-stu-id="f1447-295">Note: By convention, EF Core enables cascade delete for non-nullable FKs and for many-to-many relationships.</span></span> <span data-ttu-id="f1447-296">Kaskádové odstranění může způsobit cyklické cascade odstranit pravidla.</span><span class="sxs-lookup"><span data-stu-id="f1447-296">Cascading delete can result in circular cascade delete rules.</span></span> <span data-ttu-id="f1447-297">Cyklické cascade odstranit pravidla způsobí výjimku při přidání migrace.</span><span class="sxs-lookup"><span data-stu-id="f1447-297">Circular cascade delete rules causes an exception when a migration is added.</span></span>

<span data-ttu-id="f1447-298">Například pokud `Department.InstructorID` vlastnost nebyla definována jako s možnou hodnotou NULL:</span><span class="sxs-lookup"><span data-stu-id="f1447-298">For example, if the `Department.InstructorID` property wasn't defined as nullable:</span></span>

* <span data-ttu-id="f1447-299">Základní EF nakonfiguruje kaskádové odstranění pravidlo můžete odstranit lektorem, když je odstraněn z oddělení.</span><span class="sxs-lookup"><span data-stu-id="f1447-299">EF Core configures a cascade delete rule to delete the instructor when the department is deleted.</span></span>
* <span data-ttu-id="f1447-300">Odstraňování lektorem při odstranění tohoto oddělení nejde o záměrné chování.</span><span class="sxs-lookup"><span data-stu-id="f1447-300">Deleting the instructor when the department is deleted isn't the intended behavior.</span></span>

<span data-ttu-id="f1447-301">V případě potřeby obchodní pravidla `InstructorID` vlastnost mít hodnotu Null, použijte následující příkaz fluent API:</span><span class="sxs-lookup"><span data-stu-id="f1447-301">If business rules required the `InstructorID` property be non-nullable, use the following fluent API statement:</span></span>

 ```csharp
 modelBuilder.Entity<Department>()
    .HasOne(d => d.Administrator)
    .WithMany()
    .OnDelete(DeleteBehavior.Restrict)
 ```

<span data-ttu-id="f1447-302">Předchozí kód zakáže kaskádové odstranění v relaci lektorem oddělení.</span><span class="sxs-lookup"><span data-stu-id="f1447-302">The preceding code disables cascade delete on the department-instructor relationship.</span></span>

## <a name="update-the-enrollment-entity"></a><span data-ttu-id="f1447-303">Aktualizace entity registrace</span><span class="sxs-lookup"><span data-stu-id="f1447-303">Update the Enrollment entity</span></span>

<span data-ttu-id="f1447-304">Záznam zápisu je jeden kurzu provedenou jeden student.</span><span class="sxs-lookup"><span data-stu-id="f1447-304">An enrollment record is for a one course taken by one student.</span></span>

![Registrace entity](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="f1447-306">Aktualizace *Models/Enrollment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-306">Update *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="f1447-307">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="f1447-307">Foreign key and navigation properties</span></span>

<span data-ttu-id="f1447-308">Vlastnosti cizího klíče a navigačních vlastností odráží následující relace:</span><span class="sxs-lookup"><span data-stu-id="f1447-308">The FK properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="f1447-309">Záznam zápisu je pro jeden kurzu, takže není `CourseID` vlastnosti cizího klíče a `Course` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="f1447-309">An enrollment record is for one course, so there's a `CourseID` FK property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="f1447-310">Záznam zápisu je pro jeden student, takže není `StudentID` vlastnosti cizího klíče a `Student` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="f1447-310">An enrollment record is for one student, so there's a `StudentID` FK property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="f1447-311">Relace m: N</span><span class="sxs-lookup"><span data-stu-id="f1447-311">Many-to-Many Relationships</span></span>

<span data-ttu-id="f1447-312">Je vztah m: n mezi `Student` a `Course` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-312">There's a many-to-many relationship between the `Student` and `Course` entities.</span></span> <span data-ttu-id="f1447-313">`Enrollment` Entity funguje jako tabulku spojení m: n *s odebranou datovou částí* v databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-313">The `Enrollment` entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="f1447-314">"S odebranou datovou částí" znamená, že `Enrollment` tabulka obsahuje další údaje kromě FKs pro spojené tabulky (v tomto případě primárnímu Klíči a `Grade`).</span><span class="sxs-lookup"><span data-stu-id="f1447-314">"With payload" means that the `Enrollment` table contains additional data besides FKs for the joined tables (in this case, the PK and `Grade`).</span></span>

<span data-ttu-id="f1447-315">Následující obrázek znázorňuje, jak tyto relace vypadat v diagramu entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-315">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="f1447-316">(Tento diagram byl vygenerován pomocí EF výkonné nástroje pro EF 6.x.</span><span class="sxs-lookup"><span data-stu-id="f1447-316">(This diagram was generated using EF Power Tools for EF 6.x.</span></span> <span data-ttu-id="f1447-317">Vytváření diagramu není součástí tohoto kurzu.)</span><span class="sxs-lookup"><span data-stu-id="f1447-317">Creating the diagram isn't part of the tutorial.)</span></span>

![Během student mnoho k mnoha relace](complex-data-model/_static/student-course.png)

<span data-ttu-id="f1447-319">Každý řádek vztahů je 1 na jeden element end a znak hvězdičky (\*) v dalších, která určuje vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="f1447-319">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="f1447-320">Pokud `Enrollment` tabulky nezahrnuli úrovni informace, jenom třeba, aby obsahovat dvě FKs (`CourseID` a `StudentID`).</span><span class="sxs-lookup"><span data-stu-id="f1447-320">If the `Enrollment` table didn't include grade information, it would only need to contain the two FKs (`CourseID` and `StudentID`).</span></span> <span data-ttu-id="f1447-321">Tabulku spojení m: n bez datové části se někdy nazývá čistou vazební tabulku (PJT).</span><span class="sxs-lookup"><span data-stu-id="f1447-321">A many-to-many join table without payload is sometimes called a pure join table (PJT).</span></span>

<span data-ttu-id="f1447-322">`Instructor` a `Course` entit mít vztah m: n pomocí čistou vazební tabulku.</span><span class="sxs-lookup"><span data-stu-id="f1447-322">The `Instructor` and `Course` entities have a many-to-many relationship using a pure join table.</span></span>

<span data-ttu-id="f1447-323">Poznámka: EF 6.x podporuje implicitní spojení tabulky pro relace m: n, ale základní EF nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="f1447-323">Note: EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="f1447-324">Další informace najdete v tématu [m: n vztahy v EF základní 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span><span class="sxs-lookup"><span data-stu-id="f1447-324">For more information, see [Many-to-many relationships in EF Core 2.0](https://blog.oneunicorn.com/2017/09/25/many-to-many-relationships-in-ef-core-2-0-part-1-the-basics/).</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="f1447-325">CourseAssignment entity</span><span class="sxs-lookup"><span data-stu-id="f1447-325">The CourseAssignment entity</span></span>

![CourseAssignment entity](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="f1447-327">Vytvoření *Models/CourseAssignment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="f1447-327">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="instructor-to-courses"></a><span data-ttu-id="f1447-328">Lektorem kurzy</span><span class="sxs-lookup"><span data-stu-id="f1447-328">Instructor-to-Courses</span></span>

![M:M lektorem kurzy](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f1447-330">Relace m: n lektorem kurzy:</span><span class="sxs-lookup"><span data-stu-id="f1447-330">The Instructor-to-Courses many-to-many relationship:</span></span>

* <span data-ttu-id="f1447-331">Vyžaduje připojení k tabulku, která musí být reprezentovaná sadu entit.</span><span class="sxs-lookup"><span data-stu-id="f1447-331">Requires a join table that must be represented by an entity set.</span></span>
* <span data-ttu-id="f1447-332">Je tabulka čistý spojení (tabulky bez datovou část).</span><span class="sxs-lookup"><span data-stu-id="f1447-332">Is a pure join table (table without payload).</span></span>

<span data-ttu-id="f1447-333">Je běžné název entity spojení `EntityName1EntityName2`.</span><span class="sxs-lookup"><span data-stu-id="f1447-333">It's common to name a join entity `EntityName1EntityName2`.</span></span> <span data-ttu-id="f1447-334">Je třeba připojení k tabulce lektorem kurzy s využitím tohoto vzoru `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="f1447-334">For example, the Instructor-to-Courses join table using this pattern is `CourseInstructor`.</span></span> <span data-ttu-id="f1447-335">Doporučujeme však používat název, který popisuje relace.</span><span class="sxs-lookup"><span data-stu-id="f1447-335">However, we recommend using a name that describes the relationship.</span></span>

<span data-ttu-id="f1447-336">Datové modely spustí jednoduchou a zvýší.</span><span class="sxs-lookup"><span data-stu-id="f1447-336">Data models start out simple and grow.</span></span> <span data-ttu-id="f1447-337">Žádné datové spojení (PJTs) se často momentální zahrnout datové části.</span><span class="sxs-lookup"><span data-stu-id="f1447-337">No-payload joins (PJTs) frequently evolve to include payload.</span></span> <span data-ttu-id="f1447-338">Od entity popisný název, název nemusí změnit, když se změní v tabulce spojení.</span><span class="sxs-lookup"><span data-stu-id="f1447-338">By starting with a descriptive entity name, the name doesn't need to change when the join table changes.</span></span> <span data-ttu-id="f1447-339">Připojení entity v ideálním případě by mít svůj vlastní přirozené název (pravděpodobně jednoslovné) v doméně obchodní.</span><span class="sxs-lookup"><span data-stu-id="f1447-339">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="f1447-340">Například může knihy a zákazníky propojené s názvem hodnocení spojovací entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-340">For example, Books and Customers could be linked with a join entity called Ratings.</span></span> <span data-ttu-id="f1447-341">Pro relaci n: n lektorem kurzy `CourseAssignment` upřednostňuje přes `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="f1447-341">For the Instructor-to-Courses many-to-many relationship, `CourseAssignment` is preferred over `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="f1447-342">Složený klíč</span><span class="sxs-lookup"><span data-stu-id="f1447-342">Composite key</span></span>

<span data-ttu-id="f1447-343">FKs nejsou s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="f1447-343">FKs are not nullable.</span></span> <span data-ttu-id="f1447-344">Dva FKs v `CourseAssignment` (`InstructorID` a `CourseID`) společně jednoznačně každý řádek `CourseAssignment` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-344">The two FKs in `CourseAssignment` (`InstructorID` and `CourseID`) together uniquely identify each row of the `CourseAssignment` table.</span></span> <span data-ttu-id="f1447-345">`CourseAssignment` nevyžaduje vyhrazený PK.</span><span class="sxs-lookup"><span data-stu-id="f1447-345">`CourseAssignment` doesn't require a dedicated PK.</span></span> <span data-ttu-id="f1447-346">`InstructorID` a `CourseID` vlastnosti fungovat jako složený PK.</span><span class="sxs-lookup"><span data-stu-id="f1447-346">The `InstructorID` and `CourseID` properties function as a composite PK.</span></span> <span data-ttu-id="f1447-347">Je jediný způsob, jak určit složené PKs na jádro EF s *rozhraní fluent API*.</span><span class="sxs-lookup"><span data-stu-id="f1447-347">The only way to specify composite PKs to EF Core is with the *fluent API*.</span></span> <span data-ttu-id="f1447-348">V další části ukazuje, jak nakonfigurovat složené PK.</span><span class="sxs-lookup"><span data-stu-id="f1447-348">The next section shows how to configure the composite PK.</span></span>

<span data-ttu-id="f1447-349">Složený klíč zajistí:</span><span class="sxs-lookup"><span data-stu-id="f1447-349">The composite key ensures:</span></span>

* <span data-ttu-id="f1447-350">Pro jeden kurzu jsou povoleny více řádků.</span><span class="sxs-lookup"><span data-stu-id="f1447-350">Multiple rows are allowed for one course.</span></span>
* <span data-ttu-id="f1447-351">Pro jeden lektorem jsou povoleny více řádků.</span><span class="sxs-lookup"><span data-stu-id="f1447-351">Multiple rows are allowed for one instructor.</span></span>
* <span data-ttu-id="f1447-352">Více řádků pro stejný lektorem a kurzu není povoleno.</span><span class="sxs-lookup"><span data-stu-id="f1447-352">Multiple rows for the same instructor and course isn't allowed.</span></span>

<span data-ttu-id="f1447-353">`Enrollment` Entity spojení definuje vlastní Primárníklíč, je možné, duplikáty toto řazení.</span><span class="sxs-lookup"><span data-stu-id="f1447-353">The `Enrollment` join entity defines its own PK, so duplicates of this sort are possible.</span></span> <span data-ttu-id="f1447-354">Aby se tyto duplikáty:</span><span class="sxs-lookup"><span data-stu-id="f1447-354">To prevent such duplicates:</span></span>

* <span data-ttu-id="f1447-355">Přidejte jedinečný index na pole cizího klíče nebo</span><span class="sxs-lookup"><span data-stu-id="f1447-355">Add a unique index on the FK fields, or</span></span>
* <span data-ttu-id="f1447-356">Konfigurace `Enrollment` s primární složený klíč podobná `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="f1447-356">Configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="f1447-357">Další informace najdete v tématu [indexy](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="f1447-357">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-db-context"></a><span data-ttu-id="f1447-358">Aktualizovat kontext databáze</span><span class="sxs-lookup"><span data-stu-id="f1447-358">Update the DB context</span></span>

<span data-ttu-id="f1447-359">Přidejte následující zvýrazněný kód, který *Data/SchoolContext.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1447-359">Add the following highlighted code to *Data/SchoolContext.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="f1447-360">Předchozí kód přidá nové entity a nakonfiguruje `CourseAssignment` složené PK. entity</span><span class="sxs-lookup"><span data-stu-id="f1447-360">The preceding code adds the new entities and configures the `CourseAssignment` entity's composite PK.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="f1447-361">Fluent API alternativní atributů</span><span class="sxs-lookup"><span data-stu-id="f1447-361">Fluent API alternative to attributes</span></span>

<span data-ttu-id="f1447-362">`OnModelCreating` Metoda v předchozím kód používá *rozhraní fluent API* ke konfiguraci základní EF chování.</span><span class="sxs-lookup"><span data-stu-id="f1447-362">The `OnModelCreating` method in the preceding code uses the *fluent API* to configure EF Core behavior.</span></span> <span data-ttu-id="f1447-363">Rozhraní API se nazývá "fluent", protože se často používají v rozvádět řadu volání metod společně do jednoho příkazu.</span><span class="sxs-lookup"><span data-stu-id="f1447-363">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement.</span></span> <span data-ttu-id="f1447-364">[Následující kód](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) je příkladem rozhraní fluent API:</span><span class="sxs-lookup"><span data-stu-id="f1447-364">The [following code](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration) is an example of the fluent API:</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="f1447-365">V tomto kurzu se používá rozhraní fluent API jenom pro mapování DB, který nelze provést s atributy.</span><span class="sxs-lookup"><span data-stu-id="f1447-365">In this tutorial, the fluent API is used only for DB mapping that can't be done with attributes.</span></span> <span data-ttu-id="f1447-366">Rozhraní fluent API můžete však zadat většinu formátování, ověření a pravidla mapování, které lze provést s atributy.</span><span class="sxs-lookup"><span data-stu-id="f1447-366">However, the fluent API can specify most of the formatting, validation, and mapping rules that can be done with attributes.</span></span>

<span data-ttu-id="f1447-367">Některé atributy, jako `MinimumLength` nelze použít s rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="f1447-367">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="f1447-368">`MinimumLength` nepodporuje změnu schématu, vztahuje se pouze minimální délka ověřovacího pravidla.</span><span class="sxs-lookup"><span data-stu-id="f1447-368">`MinimumLength` doesn't change the schema, it only applies a minimum length validation rule.</span></span>

<span data-ttu-id="f1447-369">Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "čistou."</span><span class="sxs-lookup"><span data-stu-id="f1447-369">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="f1447-370">Atributy a rozhraní fluent API můžete kombinovat.</span><span class="sxs-lookup"><span data-stu-id="f1447-370">Attributes and the fluent API can be mixed.</span></span> <span data-ttu-id="f1447-371">Existují některé konfigurace, které lze provádět jen pomocí rozhraní fluent API (výběr složené Primárníklíč).</span><span class="sxs-lookup"><span data-stu-id="f1447-371">There are some configurations that can only be done with the fluent API (specifying a composite PK).</span></span> <span data-ttu-id="f1447-372">Existují některé konfigurace, které lze provést pouze s atributy (`MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="f1447-372">There are some configurations that can only be done with attributes (`MinimumLength`).</span></span> <span data-ttu-id="f1447-373">Doporučený postup pro používání fluent API nebo atributy:</span><span class="sxs-lookup"><span data-stu-id="f1447-373">The recommended practice for using fluent API or attributes:</span></span>

* <span data-ttu-id="f1447-374">Vyberte jednu z těchto dvou přístupů.</span><span class="sxs-lookup"><span data-stu-id="f1447-374">Choose one of these two approaches.</span></span>
* <span data-ttu-id="f1447-375">Použijte konzistentně co nejvíce zvolený způsob.</span><span class="sxs-lookup"><span data-stu-id="f1447-375">Use the chosen approach consistently as much as possible.</span></span>

<span data-ttu-id="f1447-376">Některé atributy používané v tomto kurzu se používají pro:</span><span class="sxs-lookup"><span data-stu-id="f1447-376">Some of the attributes used in the this tutorial are used for:</span></span>

* <span data-ttu-id="f1447-377">Pouze ověřování (například `MinimumLength`).</span><span class="sxs-lookup"><span data-stu-id="f1447-377">Validation only (for example, `MinimumLength`).</span></span>
* <span data-ttu-id="f1447-378">Pouze EF základní konfiguraci (například `HasKey`).</span><span class="sxs-lookup"><span data-stu-id="f1447-378">EF Core configuration only (for example, `HasKey`).</span></span>
* <span data-ttu-id="f1447-379">Ověření a EF základní konfiguraci (například `[StringLength(50)]`).</span><span class="sxs-lookup"><span data-stu-id="f1447-379">Validation and EF Core configuration (for example, `[StringLength(50)]`).</span></span>

<span data-ttu-id="f1447-380">Další informace o atributech oproti rozhraní fluent API najdete v tématu [metody konfigurace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="f1447-380">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="f1447-381">Diagram znázorňující entitami</span><span class="sxs-lookup"><span data-stu-id="f1447-381">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="f1447-382">Následující obrázek znázorňuje diagram, který vytvoření EF výkonné nástroje pro dokončené školní modelu.</span><span class="sxs-lookup"><span data-stu-id="f1447-382">The following illustration shows the diagram that EF Power Tools create for the completed School model.</span></span>

![Entity diagram](complex-data-model/_static/diagram.png)

<span data-ttu-id="f1447-384">Na předchozím obrázku uvádí:</span><span class="sxs-lookup"><span data-stu-id="f1447-384">The preceding diagram shows:</span></span>

* <span data-ttu-id="f1447-385">Několik řádků vztah jeden mnoho (1 \*).</span><span class="sxs-lookup"><span data-stu-id="f1447-385">Several one-to-many relationship lines (1 to \*).</span></span>
* <span data-ttu-id="f1447-386">Řádek vztah jeden pro žádná nebo jedna (1-0.1) mezi `Instructor` a `OfficeAssignment` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-386">The one-to-zero-or-one relationship line (1 to 0..1) between the `Instructor` and `OfficeAssignment` entities.</span></span>
* <span data-ttu-id="f1447-387">Řádek vztahů nula nebo 1 n (0.1 k \*) mezi `Instructor` a `Department` entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-387">The zero-or-one-to-many relationship line (0..1 to \*) between the `Instructor` and `Department` entities.</span></span>

## <a name="seed-the-db-with-test-data"></a><span data-ttu-id="f1447-388">Počáteční hodnoty databáze s testovací Data</span><span class="sxs-lookup"><span data-stu-id="f1447-388">Seed the DB with Test Data</span></span>

<span data-ttu-id="f1447-389">Aktualizujte kód v *Data/DbInitializer.cs*:</span><span class="sxs-lookup"><span data-stu-id="f1447-389">Update the code in *Data/DbInitializer.cs*:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="f1447-390">Předchozí kód poskytuje data počáteční hodnoty pro nové entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-390">The preceding code provides seed data for the new entities.</span></span> <span data-ttu-id="f1447-391">Většina tento kód vytvoří nové entity objekty a načte ukázková data.</span><span class="sxs-lookup"><span data-stu-id="f1447-391">Most of this code creates new entity objects and loads sample data.</span></span> <span data-ttu-id="f1447-392">Ukázková data se používají pro testování.</span><span class="sxs-lookup"><span data-stu-id="f1447-392">The sample data is used for testing.</span></span> <span data-ttu-id="f1447-393">Předchozí kód vytvoří následující relace m: n:</span><span class="sxs-lookup"><span data-stu-id="f1447-393">The preceding code creates the following many-to-many relationships:</span></span>

* `Enrollments`
* `CourseAssignment`

<span data-ttu-id="f1447-394">Poznámka: [EF základní 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) bude podporovat [synchronizace replik indexů data](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span><span class="sxs-lookup"><span data-stu-id="f1447-394">Note: [EF Core 2.1](https://github.com/aspnet/EntityFrameworkCore/wiki/Roadmap) will support [data seeding](https://github.com/aspnet/EntityFrameworkCore/issues/629).</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="f1447-395">Přidat migrace</span><span class="sxs-lookup"><span data-stu-id="f1447-395">Add a migration</span></span>

<span data-ttu-id="f1447-396">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="f1447-396">Build the project.</span></span> <span data-ttu-id="f1447-397">Otevřete okno příkazového řádku ve složce projektu a zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1447-397">Open a command window in the project folder and enter the following command:</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="f1447-398">Předchozí příkaz zobrazí varování týkající se možná ztráta dat.</span><span class="sxs-lookup"><span data-stu-id="f1447-398">The preceding command displays a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data.
Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="f1447-399">Pokud `database update` příkaz spustí, vytváří k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="f1447-399">If the `database update` command is run, the following error is produced:</span></span>

```text
The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID". The conflict occurred in
database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.
```

<span data-ttu-id="f1447-400">Pokud migrace spouštějí s existujícími daty, může být omezení cizího klíče, které nejsou splněné existujícímu daty.</span><span class="sxs-lookup"><span data-stu-id="f1447-400">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="f1447-401">V tomto kurzu se vytvoří nové databáze, takže neexistují žádné narušení omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="f1447-401">For this tutorial, a new DB is created, so there are no FK constraint violations.</span></span> <span data-ttu-id="f1447-402">V tématu [opravě omezení cizích klíčů s starší data](#fk) pokyny o tom, jak opravit porušení cizího klíče v aktuální databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-402">See [Fixing foreign key constraints with legacy data](#fk) for instructions on how to fix the FK violations on the current DB.</span></span>

## <a name="change-the-connection-string-and-update-the-db"></a><span data-ttu-id="f1447-403">Změňte připojovací řetězec a aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="f1447-403">Change the connection string and update the DB</span></span>

<span data-ttu-id="f1447-404">Kód v aktualizaci `DbInitializer` přidá počáteční hodnoty dat pro nové entity.</span><span class="sxs-lookup"><span data-stu-id="f1447-404">The code in the updated `DbInitializer` adds seed data for the new entities.</span></span> <span data-ttu-id="f1447-405">Chcete-li vynutit EF jádra k vytvoření nové prázdné databáze:</span><span class="sxs-lookup"><span data-stu-id="f1447-405">To force EF Core to create a new empty DB:</span></span>

* <span data-ttu-id="f1447-406">Změňte název připojovacího řetězce DB v *appSettings.JSON určený* k ContosoUniversity3.</span><span class="sxs-lookup"><span data-stu-id="f1447-406">Change the DB connection string name in *appsettings.json* to ContosoUniversity3.</span></span> <span data-ttu-id="f1447-407">Nový název musí být název, který nebyl použit v počítači.</span><span class="sxs-lookup"><span data-stu-id="f1447-407">The new name must be a name that hasn't been used on the computer.</span></span>

    ```json
    {
      "ConnectionStrings": {
        "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
      },
    ```

* <span data-ttu-id="f1447-408">Můžete taky odstraňte pomocí DB:</span><span class="sxs-lookup"><span data-stu-id="f1447-408">Alternatively, delete the DB using:</span></span>

  * <span data-ttu-id="f1447-409">**SQL Server Object Explorer** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="f1447-409">**SQL Server Object Explorer** (SSOX).</span></span>
  * <span data-ttu-id="f1447-410">`database drop` Rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="f1447-410">The `database drop` CLI command:</span></span>

    ```console
    dotnet ef database drop
    ```

<span data-ttu-id="f1447-411">Spustit `database update` v příkazovém okně:</span><span class="sxs-lookup"><span data-stu-id="f1447-411">Run `database update` in the command window:</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="f1447-412">Předchozí příkaz spustí všechny migrace.</span><span class="sxs-lookup"><span data-stu-id="f1447-412">The preceding command runs all the migrations.</span></span>

<span data-ttu-id="f1447-413">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="f1447-413">Run the app.</span></span> <span data-ttu-id="f1447-414">Spuštění aplikace běží `DbInitializer.Initialize` metoda.</span><span class="sxs-lookup"><span data-stu-id="f1447-414">Running the app runs the `DbInitializer.Initialize` method.</span></span> <span data-ttu-id="f1447-415">`DbInitializer.Initialize` Naplní nové databáze.</span><span class="sxs-lookup"><span data-stu-id="f1447-415">The `DbInitializer.Initialize` populates the new DB.</span></span>

<span data-ttu-id="f1447-416">Otevřete databázi v SSOX:</span><span class="sxs-lookup"><span data-stu-id="f1447-416">Open the DB in SSOX:</span></span>

* <span data-ttu-id="f1447-417">Rozbalte **tabulky** uzlu.</span><span class="sxs-lookup"><span data-stu-id="f1447-417">Expand the **Tables** node.</span></span> <span data-ttu-id="f1447-418">Zobrazí se vytvořené tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-418">The created tables are displayed.</span></span>
* <span data-ttu-id="f1447-419">Pokud SSOX byl dříve otevřen, klikněte **aktualizovat** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="f1447-419">If SSOX was opened previously, click the **Refresh** button.</span></span>

![Tabulky v SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="f1447-421">Zkontrolujte **CourseAssignment** tabulky:</span><span class="sxs-lookup"><span data-stu-id="f1447-421">Examine the **CourseAssignment** table:</span></span>

* <span data-ttu-id="f1447-422">Klikněte pravým tlačítkem myši **CourseAssignment** tabulky a vyberte **Data zobrazení**.</span><span class="sxs-lookup"><span data-stu-id="f1447-422">Right-click the **CourseAssignment** table and select **View Data**.</span></span>
* <span data-ttu-id="f1447-423">Ověřte **CourseAssignment** tabulka obsahuje data.</span><span class="sxs-lookup"><span data-stu-id="f1447-423">Verify the **CourseAssignment** table contains data.</span></span>

![CourseAssignment data v SSOX](complex-data-model/_static/ssox-ci-data.png)

<a name="fk"></a>

## <a name="fixing-foreign-key-constraints-with-legacy-data"></a><span data-ttu-id="f1447-425">Oprava omezení cizích klíčů daty starší verze</span><span class="sxs-lookup"><span data-stu-id="f1447-425">Fixing foreign key constraints with legacy data</span></span>

<span data-ttu-id="f1447-426">Tato část je volitelné.</span><span class="sxs-lookup"><span data-stu-id="f1447-426">This section is optional.</span></span>

<span data-ttu-id="f1447-427">Pokud migrace spouštějí s existujícími daty, může být omezení cizího klíče, které nejsou splněné existujícímu daty.</span><span class="sxs-lookup"><span data-stu-id="f1447-427">When migrations are run with existing data, there may be FK constraints that are not satisfied with the exiting data.</span></span> <span data-ttu-id="f1447-428">S použitím provozních dat je učinit migrovat existující data.</span><span class="sxs-lookup"><span data-stu-id="f1447-428">With production data, steps must be taken to migrate the existing data.</span></span> <span data-ttu-id="f1447-429">Tato část poskytuje příklad opravit porušení omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="f1447-429">This section provides an example of fixing FK constraint violations.</span></span> <span data-ttu-id="f1447-430">Nevytvářejte tyto změny kódu bez předchozího provedení zálohy.</span><span class="sxs-lookup"><span data-stu-id="f1447-430">Don't make these code changes without a backup.</span></span> <span data-ttu-id="f1447-431">Nevytvářejte tyto změny kódu, pokud byla dokončena v předchozí části a aktualizovat databázi.</span><span class="sxs-lookup"><span data-stu-id="f1447-431">Don't make these code changes if you completed the previous section and updated the database.</span></span>

<span data-ttu-id="f1447-432">*{Timestamp}_ComplexDataModel.cs* soubor obsahuje následující kód:</span><span class="sxs-lookup"><span data-stu-id="f1447-432">The *{timestamp}_ComplexDataModel.cs* file contains the following code:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_DepartmentID)]

<span data-ttu-id="f1447-433">Předchozí kód přidá hodnotou Null `DepartmentID` cizího klíče na `Course` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-433">The preceding code adds a non-nullable `DepartmentID` FK to the `Course` table.</span></span> <span data-ttu-id="f1447-434">Databáze z předchozí kurz obsahuje řádky v `Course`, takže tato tabulka nelze aktualizovat pomocí migrace.</span><span class="sxs-lookup"><span data-stu-id="f1447-434">The DB from the previous tutorial contains rows in `Course`, so that table cannot be updated by migrations.</span></span>

<span data-ttu-id="f1447-435">Chcete-li `ComplexDataModel` migrace práci s existujícími daty:</span><span class="sxs-lookup"><span data-stu-id="f1447-435">To make the `ComplexDataModel` migration work with existing data:</span></span>

* <span data-ttu-id="f1447-436">Změnit kód umožnit nového sloupce (`DepartmentID`) výchozí hodnotu.</span><span class="sxs-lookup"><span data-stu-id="f1447-436">Change the code to give the new column (`DepartmentID`) a default value.</span></span>
* <span data-ttu-id="f1447-437">Vytvořte falešných oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení.</span><span class="sxs-lookup"><span data-stu-id="f1447-437">Create a fake department named "Temp" to act as the default department.</span></span>

### <a name="fix-the-foreign-key-constraints"></a><span data-ttu-id="f1447-438">Opravte omezení cizího klíče</span><span class="sxs-lookup"><span data-stu-id="f1447-438">Fix the foreign key constraints</span></span>

<span data-ttu-id="f1447-439">Aktualizace `ComplexDataModel` třídy `Up` metoda:</span><span class="sxs-lookup"><span data-stu-id="f1447-439">Update the `ComplexDataModel` classes `Up` method:</span></span>

* <span data-ttu-id="f1447-440">Otevřete *{timestamp}_ComplexDataModel.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="f1447-440">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>
* <span data-ttu-id="f1447-441">Komentář řádek kódu, který přidá `DepartmentID` sloupec, který se `Course` tabulky.</span><span class="sxs-lookup"><span data-stu-id="f1447-441">Comment out the line of code that adds the `DepartmentID` column to the `Course` table.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

<span data-ttu-id="f1447-442">Přidejte následující zvýrazněný kód.</span><span class="sxs-lookup"><span data-stu-id="f1447-442">Add the following highlighted code.</span></span> <span data-ttu-id="f1447-443">Nový kód přejde po `.CreateTable( name: "Department"` bloku: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span><span class="sxs-lookup"><span data-stu-id="f1447-443">The new code goes after the `.CreateTable( name: "Department"` block: [!code-csharp[](intro/samples/cu/Migrations/20171027005808_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]</span></span>

<span data-ttu-id="f1447-444">S předchozí změny, existující `Course` řádky budou související s oddělení "Temp" po `ComplexDataModel` `Up` metoda spustí.</span><span class="sxs-lookup"><span data-stu-id="f1447-444">With the preceding changes, existing `Course` rows will be related to the "Temp" department after the `ComplexDataModel` `Up` method runs.</span></span>

<span data-ttu-id="f1447-445">Produkční aplikace bude:</span><span class="sxs-lookup"><span data-stu-id="f1447-445">A production app would:</span></span>

* <span data-ttu-id="f1447-446">Přidat kód nebo skripty, které chcete přidat `Department` řádky a související `Course` řádky do nového `Department` řádků.</span><span class="sxs-lookup"><span data-stu-id="f1447-446">Include code or scripts to add `Department` rows and related `Course` rows to the new `Department` rows.</span></span>
* <span data-ttu-id="f1447-447">Nepoužívat oddělení "Temp" nebo výchozí hodnotu pro `Course.DepartmentID`.</span><span class="sxs-lookup"><span data-stu-id="f1447-447">Not use the "Temp" department or the default value for `Course.DepartmentID`.</span></span>

<span data-ttu-id="f1447-448">Další kurz se zaměřuje na související data.</span><span class="sxs-lookup"><span data-stu-id="f1447-448">The next tutorial covers related data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f1447-449">[Předchozí](xref:data/ef-rp/migrations)
> [další](xref:data/ef-rp/read-related-data)</span><span class="sxs-lookup"><span data-stu-id="f1447-449">[Previous](xref:data/ef-rp/migrations)
[Next](xref:data/ef-rp/read-related-data)</span></span>
