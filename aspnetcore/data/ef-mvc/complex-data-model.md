---
title: Jádro ASP.NET MVC s EF Core - Model dat – 5 10
author: rick-anderson
description: V tomto kurzu přidejte další entity a vztahy a přizpůsobit datový model zadáním formátování, ověření a pravidla mapování.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 1d3c69c8c658b5ca2f0253b790b0dc75d44d3064
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093111"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="2d0f2-103">Jádro ASP.NET MVC s EF Core - Model dat – 5 10</span><span class="sxs-lookup"><span data-stu-id="2d0f2-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2d0f2-104">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2d0f2-105">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="2d0f2-106">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="2d0f2-107">V předchozích kurzech pracovali s jednoduché datového modelu, který se skládá z tři entity.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="2d0f2-108">V tomto kurzu přidáte další entity a vztahy a datový model budete přizpůsobit zadáním formátování, ověřování a pravidla mapování databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="2d0f2-109">Jakmile budete hotovi, bude tříd entit tvoří dokončené datový model, který je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Entity diagram](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="2d0f2-111">Přizpůsobení datového modelu s použitím atributů</span><span class="sxs-lookup"><span data-stu-id="2d0f2-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="2d0f2-112">V této části se zobrazí postup přizpůsobení datového modelu s použitím atributy, které určují, formátování, ověření a pravidla mapování databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="2d0f2-113">Potom v některé z těchto částí, které vytvoříte kompletní datový model školní přidáním atributy třídám jste již vytvořili a vytvoření nové třídy pro zbývající typy entit v modelu.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="2d0f2-114">Datový typ atributu</span><span class="sxs-lookup"><span data-stu-id="2d0f2-114">The DataType attribute</span></span>

<span data-ttu-id="2d0f2-115">Pro studenty registrace kalendářních dat všech webových stránek aktuálně zobrazuje čas, spolu s datem, i když všechny, které se zajímáte o pro toto pole je datum.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="2d0f2-116">Pomocí datové poznámky atributy, můžete si ho code změny, která bude opravte formát zobrazení v každé zobrazení, která zobrazuje data.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="2d0f2-117">Chcete-li zobrazit příklad, jak to provést, že přidáte do atribut `EnrollmentDate` vlastnost v `Student` třídy.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="2d0f2-118">V *Models/Student.cs*, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations` obor názvů a přidejte `DataType` a `DisplayFormat` atributů k `EnrollmentDate` vlastnost, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="2d0f2-119">`DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="2d0f2-120">V tomto případě chceme jenom udržování přehledu o datum, není datum a čas.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="2d0f2-121">`DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="2d0f2-122">`DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="2d0f2-123">Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`, a datum selektor lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="2d0f2-124">`DataType` Atribut vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které můžete porozumět standardu HTML 5 prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="2d0f2-125">`DataType` Atributy neposkytují žádné ověření.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="2d0f2-126">`DataType.Date` neuvádí formát data, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="2d0f2-127">Ve výchozím nastavení je datové pole zobrazí podle výchozích formátů podle serveru CultureInfo.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="2d0f2-128">`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="2d0f2-129">`ApplyFormatInEditMode` Nastavení určuje, že formátování, které také bude použito při hodnota je zobrazena v textovém poli pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="2d0f2-130">(Není vhodné, aby pro některá pole – například hodnoty měny, nemusí chcete symbolu měny do textového pole pro úpravy.)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="2d0f2-131">Můžete použít `DisplayFormat` atribut podle sám sebe, ale obecně je vhodné používat `DataType` také atribut.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="2d0f2-132">`DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s `DisplayFormat`:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="2d0f2-133">V prohlížeči můžete povolit funkce HTML5 (například k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazů některé klientské zadejte ověření atd.).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="2d0f2-134">Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="2d0f2-135">Další informace najdete v tématu [ \<vstupní > označení dokumentaci pomocná](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="2d0f2-136">Spusťte aplikaci, přejděte na stránku studenty Index a Všimněte si, že časy se už nezobrazují mezi daty registrace.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="2d0f2-137">Stejné bude platit pro všechna zobrazení, která používá Student model.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-137">The same will be true for any view that uses the Student model.</span></span>

![Studenti, kteří index stránky zobrazující data bez časů](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="2d0f2-139">Atribut StringLength</span><span class="sxs-lookup"><span data-stu-id="2d0f2-139">The StringLength attribute</span></span>

<span data-ttu-id="2d0f2-140">Můžete také zadat pravidla ověření dat a chybové zprávy ověření pomocí atributů.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="2d0f2-141">`StringLength` Atribut Nastaví maximální délku v databázi a poskytuje na straně klienta a na straně serveru ověřování pro architekturu ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="2d0f2-142">Minimální délka řetězce. můžete také zadat v tomto atributu, ale minimální hodnota nemá žádný vliv na schéma databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="2d0f2-143">Předpokládejme, že chcete zajistit, že uživatelé nezadávejte víc než 50 znaků pro název.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="2d0f2-144">Chcete-li přidat toto omezení, přidejte `StringLength` atributů k `LastName` a `FirstMidName` vlastnosti, jak je znázorněno v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="2d0f2-145">`StringLength` Atribut nebude uživatel zabránit v přechodu do prázdných znaků pro název.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="2d0f2-146">Můžete použít `RegularExpression` atribut použít omezení na vstup.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="2d0f2-147">Například následující kód vyžaduje první znak, který má být velkými písmeny a zbývající znaků, které mají být abecední:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="2d0f2-148">`MaxLength` Atribut poskytuje podobné funkce `StringLength` atribut, ale neposkytuje na straně klienta ověření.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="2d0f2-149">Model databáze se změnilo tak, že vyžaduje změnu schématu databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="2d0f2-150">Migrace budete používat k aktualizaci schématu bez ztráty dat, která jste mohli přidat do databáze pomocí aplikace uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="2d0f2-151">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-151">Save your changes and build the project.</span></span> <span data-ttu-id="2d0f2-152">Potom otevřete příkazové okno ve složce projektu a zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="2d0f2-153">`migrations add` Příkaz upozorní, že může dojít ke ztrátě dat, protože tato změna umožňuje maximální délku kratší pro dva sloupce.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="2d0f2-154">Migrace vytvoří soubor s názvem  *\<časové razítko > _MaxLengthOnNames.cs*.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="2d0f2-155">Tento soubor obsahuje kód `Up` metoda, která aktualizuje databázi tak, aby odpovídala aktuální datový model.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="2d0f2-156">`database update` Tento kód byl spuštěn příkaz.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="2d0f2-157">Časové razítko předponu k názvu souboru migrace je pomocí rozhraní Entity Framework sloužící k uspořádání byla migrace.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="2d0f2-158">Můžete vytvořit více migrací před spuštěním příkazu update-database, a potom všechny byla migrace se použijí v pořadí, ve kterém byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="2d0f2-159">Spuštění aplikace, vyberte **studenty** , klikněte na **vytvořit nový**a zadejte buď název delší než 50 znaků.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="2d0f2-160">Když kliknete na tlačítko **vytvořit**, ověřování na straně klienta zobrazí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-160">When you click **Create**, client side validation shows an error message.</span></span>

![Studenti, kteří index stránky zobrazující chyby délka řetězce](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="2d0f2-162">Atribut sloupce</span><span class="sxs-lookup"><span data-stu-id="2d0f2-162">The Column attribute</span></span>

<span data-ttu-id="2d0f2-163">Atributy můžete taky řídit, jak jsou mapovány třídy a vlastnosti do databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="2d0f2-164">Předpokládejme, že při použití názvu `FirstMidName` pro první název pole, protože pole může obsahovat také křestní jméno.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="2d0f2-165">Ale chcete sloupci databáze s názvem `FirstName`, protože jsou uživatelé, kteří budou být zápis dotazů ad-hoc v databázi zvykli tohoto názvu.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="2d0f2-166">Chcete-li toto mapování, můžete použít `Column` atribut.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="2d0f2-167">`Column` Atribut určuje, že při vytvoření databáze, sloupec `Student` tabulku, která se mapuje `FirstMidName` vlastnost bude mít název `FirstName`.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="2d0f2-168">Jinými slovy, pokud váš kód odkazuje na `Student.FirstMidName`, data budou pocházet z nebo aktualizovány v `FirstName` sloupec `Student` tabulky.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="2d0f2-169">Pokud nezadáte názvy sloupců, získá se stejný název jako název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="2d0f2-170">V *Student.cs* soubor, přidejte `using` příkaz pro `System.ComponentModel.DataAnnotations.Schema` a přidejte atribut název sloupce, který se `FirstMidName` vlastnost, jak je znázorněno v následující zvýrazněný kód:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="2d0f2-171">Přidání `Column` základní model, změní se atribut `SchoolContext`, takže nebude odpovídat databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="2d0f2-172">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-172">Save your changes and build the project.</span></span> <span data-ttu-id="2d0f2-173">Potom otevřete příkazové okno ve složce projektu a zadejte následující příkazy k vytvoření jiná migrace:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="2d0f2-174">V **Průzkumník objektů systému SQL Server**, otevřete návrháře tabulky Student dvojitým kliknutím **Student** tabulky.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Studenti, kteří tabulky v SSOX po migrace](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="2d0f2-176">Před použitím první dva migrace se název sloupce z typu nvarchar(MAX).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="2d0f2-177">Jsou nyní nvarchar(50) a název sloupce se změnil z FirstMidName na FirstName.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="2d0f2-178">Pokud se pokusíte zkompilovat před dokončení vytváření všechny třídy entity v následujících částech, může docházet k chybám kompilátoru.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="2d0f2-179">Poslední změny Student entity</span><span class="sxs-lookup"><span data-stu-id="2d0f2-179">Final changes to the Student entity</span></span>

![Student entity](complex-data-model/_static/student-entity.png)

<span data-ttu-id="2d0f2-181">V *Models/Student.cs*, nahraďte kód, který jste přidali dříve následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="2d0f2-182">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="2d0f2-183">Požadovaný atribut</span><span class="sxs-lookup"><span data-stu-id="2d0f2-183">The Required attribute</span></span>

<span data-ttu-id="2d0f2-184">`Required` Atribut je povinná pole název vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="2d0f2-185">`Required` Atribut není nutný pro použití hodnot Null typy, jako jsou typy hodnot (data a času, int, dvakrát, float, atd.).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="2d0f2-186">Typy, které nemůže mít hodnotu null, jsou automaticky považovány za povinná pole.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="2d0f2-187">Je vhodné odebrat `Required` atribut a nahraďte ji metodou minimální délku parametru pro `StringLength` atribut:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="2d0f2-188">Atribut zobrazení</span><span class="sxs-lookup"><span data-stu-id="2d0f2-188">The Display attribute</span></span>

<span data-ttu-id="2d0f2-189">`Display` Atribut určuje, že titulek pro do textových polí by měl být "Jméno", "Příjmení", "Úplný název" a "Registrace datum" namísto název vlastnosti v každé instanci (což je žádné místo dělení slova).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="2d0f2-190">Vlastnost FullName vypočítat</span><span class="sxs-lookup"><span data-stu-id="2d0f2-190">The FullName calculated property</span></span>

<span data-ttu-id="2d0f2-191">`FullName` je počítané vlastnosti, která vrátí hodnotu, která se vytvoří zřetězením dva další vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="2d0f2-192">Proto má pouze přistupující a ne `FullName` vygeneruje sloupec v databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="2d0f2-193">Vytvořit entitu lektorem</span><span class="sxs-lookup"><span data-stu-id="2d0f2-193">Create the Instructor Entity</span></span>

![Entitu lektorem](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="2d0f2-195">Vytvoření *Models/Instructor.cs*, nahraďte kód šablony s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="2d0f2-196">Všimněte si, že několik vlastností, které jsou stejné ve Student a lektorem entity.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="2d0f2-197">V [implementace dědičnosti](inheritance.md) kurz později z této série budete Refaktorovat tento kód eliminovat redundance.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="2d0f2-198">Můžete vložit více atributů na jeden řádek, můžete také napsat `HireDate` atributy následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="2d0f2-199">CourseAssignments a OfficeAssignment navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2d0f2-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="2d0f2-200">`CourseAssignments` a `OfficeAssignment` vlastnosti jsou navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="2d0f2-201">Lektorem můžete naučit libovolný počet kurzy, takže `CourseAssignments` je definována jako kolekce.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="2d0f2-202">Pokud vlastnost navigace mohou být uloženy více entit, její typ musí být seznam, ve kterém položky lze přidat, odstranit a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="2d0f2-203">Můžete zadat `ICollection<T>` , nebo typu, jako `List<T>` nebo `HashSet<T>`.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="2d0f2-204">Pokud zadáte `ICollection<T>`, vytvoří EF `HashSet<T>` kolekce ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="2d0f2-205">Důvod, proč se `CourseAssignment` entity je popsáno níže v části o relace m: n.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="2d0f2-206">Contoso univerzity obchodní pravidla o stavu lektorem může mít pouze maximálně jeden office, proto `OfficeAssignment` vlastnost obsahuje jednu entitu OfficeAssignment (který může mít hodnotu null, pokud není přiřazena žádná office).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="2d0f2-207">Vytvořit entitu OfficeAssignment</span><span class="sxs-lookup"><span data-stu-id="2d0f2-207">Create the OfficeAssignment entity</span></span>

![OfficeAssignment entity](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="2d0f2-209">Vytvoření *Models/OfficeAssignment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="2d0f2-210">Klíč atributu</span><span class="sxs-lookup"><span data-stu-id="2d0f2-210">The Key attribute</span></span>

<span data-ttu-id="2d0f2-211">Je--nula nebo 1 vztah mezi lektorem a entitami, OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="2d0f2-212">Přiřazení office existuje pouze ve vztahu k lektorem, které je přiřazen, a proto jeho primární klíč je také jeho cizí klíč entitu lektorem.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="2d0f2-213">Ale rozhraní Entity Framework nelze rozpoznat automaticky InstructorID jako primární klíč tuto entitu vzhledem k tomu, že její název není podle ID nebo classnameID zásady vytváření názvů.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="2d0f2-214">Proto `Key` atribut slouží k identifikaci jako klíč:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="2d0f2-215">Můžete také `Key` atribut Pokud entity mít svůj vlastní primární klíč, ale budete chtít název vlastnost něco jiného než classnameID nebo ID.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="2d0f2-216">Ve výchozím nastavení EF klíče jsou považovány za jiný databáze generované protože sloupec je sloupec pro identifikační vztah.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="2d0f2-217">Vlastnost navigace lektorem</span><span class="sxs-lookup"><span data-stu-id="2d0f2-217">The Instructor navigation property</span></span>

<span data-ttu-id="2d0f2-218">Lektorem entita, která má s možnou hodnotou Null `OfficeAssignment` navigační vlastnost (protože lektorem nemusí mít přiřazení office), a OfficeAssignment entita, která má hodnotou Null `Instructor` navigační vlastnost (protože nelze přiřazení office Existují bez lektorem – `InstructorID` neumožňuje hodnotu Null).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="2d0f2-219">Pokud entitu lektorem související entity OfficeAssignment, bude mít každá entita odkaz na další jeden v její navigační vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="2d0f2-220">By mohlo `[Required]` atribut na navigační vlastnost lektorem k určení, že musí být související lektorem, ale nemáte k tomu, protože `InstructorID` cizí klíč (což je také klíč, který chcete tuto tabulku) je použití hodnot Null.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="2d0f2-221">Upravit kurzu Entity</span><span class="sxs-lookup"><span data-stu-id="2d0f2-221">Modify the Course Entity</span></span>

![Během entity](complex-data-model/_static/course-entity.png)

<span data-ttu-id="2d0f2-223">V *Models/Course.cs*, nahraďte kód, který jste přidali dříve následujícím kódem.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="2d0f2-224">Změny se zvýrazněnou.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="2d0f2-225">Během entita má vlastností cizího klíče `DepartmentID` který odkazuje na související entity oddělení a má `Department` navigační vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="2d0f2-226">Rozhraní Entity Framework nevyžaduje, můžete přidat vlastností cizího klíče do datového modelu, když máte navigační vlastnost pro související entity.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="2d0f2-227">EF automaticky vytvoří cizí klíče v databázi bez ohledu na jejich jste potřeby a vytvoří [stínové vlastnosti](https://docs.microsoft.com/ef/core/modeling/shadow-properties) pro ně.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="2d0f2-228">Ale s cizí klíč v datovém modelu můžete provést aktualizace teď jednodušší a efektivnější.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="2d0f2-229">Například při fetch kurzu entity upravit oddělení entita je null. Pokud nemáte načíst ho, tak při aktualizaci entity kurzu, budete muset nejdřív načíst entity oddělení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="2d0f2-230">Pokud vlastnost cizího klíče `DepartmentID` je zahrnutá v datovém modelu, nemusíte načtení entity oddělení dřív, než je aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="2d0f2-231">Atribut DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="2d0f2-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="2d0f2-232">`DatabaseGenerated` Atribut s `None` parametr na `CourseID` vlastnost určuje, že hodnot primárního klíče jsou zadané uživatelem, nikoli generované databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="2d0f2-233">Ve výchozím nastavení rozhraní Entity Framework předpokládá, že hodnot primárního klíče generované databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="2d0f2-234">To je, co chcete ve většině scénářů.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="2d0f2-235">Pro entity kurzu, budete však použít několik zadán uživatel během například řadu 1000 oddělení jednu řadu 2000 pro jiné oddělení a tak dále.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="2d0f2-236">`DatabaseGenerated` Atribut můžete také použít ke generování výchozí hodnoty, jako v případě sloupců databáze slouží k záznamu datum řádek byl vytvořen nebo aktualizován.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="2d0f2-237">Další informace najdete v tématu [generované vlastnosti](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d0f2-238">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2d0f2-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d0f2-239">Vlastnosti cizího klíče a navigačních vlastností v entitě kurzu odráží následující relace:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="2d0f2-240">Kurz je přiřazena k jedné oddělení, takže není `DepartmentID` cizí klíč a `Department` navigační vlastnost z důvodů uvedených výše.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="2d0f2-241">Kurz může mít libovolný počet studenty zaregistrované v něm proto `Enrollments` navigační vlastnost je kolekce:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="2d0f2-242">Kurz může výukové ve více vyučující, proto `CourseAssignments` navigační vlastnost je kolekce (typ `CourseAssignment` je vysvětlen [později](#many-to-many-relationships)):</span><span class="sxs-lookup"><span data-stu-id="2d0f2-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="2d0f2-243">Vytvořit entitu oddělení</span><span class="sxs-lookup"><span data-stu-id="2d0f2-243">Create the Department entity</span></span>

![Oddělení entity](complex-data-model/_static/department-entity.png)


<span data-ttu-id="2d0f2-245">Vytvoření *Models/Department.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="2d0f2-246">Atribut sloupce</span><span class="sxs-lookup"><span data-stu-id="2d0f2-246">The Column attribute</span></span>

<span data-ttu-id="2d0f2-247">Dříve jste použili `Column` atribut změnit mapování název sloupce.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="2d0f2-248">V kódu pro entitu oddělení `Column` atribut je používána pro změnit SQL mapování datového typu, aby sloupec bude nutné definovat pomocí typ money systému SQL Server v databázi:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="2d0f2-249">Mapování sloupce není povinné, protože rozhraní Entity Framework vybere odpovídající typ dat systému SQL Server na základě typu CLR, které definujete pro vlastnost.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="2d0f2-250">Modul CLR `decimal` zadejte map k systému SQL Server `decimal` typu.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="2d0f2-251">Ale v takovém případě víte, že sloupec bude podržíte částky měny a je vhodnější pro tento datový typ money.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d0f2-252">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2d0f2-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d0f2-253">Vlastnosti cizího klíče a navigační odráží následující relace:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="2d0f2-254">Oddělení může nebo nemusí mít správce a správce je vždy lektorem.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="2d0f2-255">Proto `InstructorID` vlastnost je zahrnuta jako cizí klíč na entitu lektorem a otazník bude přidán za `int` zadejte označení označit vlastnost jako s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="2d0f2-256">Navigační vlastnost jmenuje `Administrator` , ale obsahuje entitu lektorem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="2d0f2-257">Oddělení může mít mnoho kurzy, takže není navigační vlastnost kurzy:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="2d0f2-258">Podle konvence rozhraní Entity Framework umožňuje kaskádové odstranění pro použití hodnot Null cizí klíče a pro relace m: n.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="2d0f2-259">Výsledkem může být Cyklické kaskádové odstranění pravidla, která způsobí výjimku při pokusu o přidání migrace.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="2d0f2-260">Například pokud vlastnost Department.InstructorID neuvedli jako s možnou hodnotou Null, EF byste nakonfigurovali odstranit lektorem při odstranění oddělení, která není, co se stane chcete odstranit pravidlo cascade.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="2d0f2-261">V případě potřeby obchodní pravidla `InstructorID` vlastnost, která má mít hodnotu Null, je třeba použít následující příkaz rozhraní API fluent zakázat kaskádové odstranění v relaci:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="2d0f2-262">Upravit registraci entity</span><span class="sxs-lookup"><span data-stu-id="2d0f2-262">Modify the Enrollment entity</span></span>

![Registrace entity](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="2d0f2-264">V *Models/Enrollment.cs*, nahraďte kód, který jste přidali dříve následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="2d0f2-265">Cizí klíč a navigační vlastnosti</span><span class="sxs-lookup"><span data-stu-id="2d0f2-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="2d0f2-266">Vlastnosti cizího klíče a navigačních vlastností odráží následující relace:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="2d0f2-267">Záznam zápisu je jeden kurzu, takže není `CourseID` vlastností cizího klíče a `Course` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="2d0f2-268">Záznam zápisu je pro jeden student, takže není `StudentID` vlastností cizího klíče a `Student` navigační vlastnost:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="2d0f2-269">Relace m: N</span><span class="sxs-lookup"><span data-stu-id="2d0f2-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="2d0f2-270">Je relace m: n mezi studenty a kurzu entity a entity registrace funguje jako tabulku spojení m: n *s odebranou datovou částí* v databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="2d0f2-271">"S odebranou datovou částí" znamená, že registrace tabulka obsahuje další údaje kromě cizí klíče pro spojené tabulky (v tomto případě primární klíč a vlastnost úrovni).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="2d0f2-272">Následující obrázek znázorňuje, jak tyto relace vypadat v diagramu entity.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="2d0f2-273">(Tento diagram byl vygenerován pomocí nástroje Entity Framework napájení pro EF 6.x vytváření diagramu není součástí tohoto kurzu, je právě používán sem jako obrázek.)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![Během student mnoho k mnoha relace](complex-data-model/_static/student-course.png)

<span data-ttu-id="2d0f2-275">Každý řádek vztahů je 1 na jeden element end a znak hvězdičky (\*) v dalších, která určuje vztah jeden mnoho.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="2d0f2-276">Pokud v tabulce registrace nezahrnuli úrovni informace, jenom třeba, aby obsahovat dvě cizí klíče CourseID a StudentID.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="2d0f2-277">V takovém případě je m: n spojení tabulku bez datová část (nebo čistou vazební tabulku) v databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="2d0f2-278">Entity lektorem a kurzu mají tento druh relace m: n a dalším krokem je vytvoření třídu entity jako tabulku spojení bez datové části.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="2d0f2-279">(EF 6.x podporuje implicitní spojení tabulky pro relace m: n, ale základní EF nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="2d0f2-280">Další informace najdete v tématu [diskuse v úložišti GitHub základní EF](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="2d0f2-281">CourseAssignment entity</span><span class="sxs-lookup"><span data-stu-id="2d0f2-281">The CourseAssignment entity</span></span>

![CourseAssignment entity](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="2d0f2-283">Vytvoření *Models/CourseAssignment.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="2d0f2-284">Připojení k názvy entit</span><span class="sxs-lookup"><span data-stu-id="2d0f2-284">Join entity names</span></span>

<span data-ttu-id="2d0f2-285">Je nutné připojení k tabulce v databázi pro vztah m: n lektorem kurzy a musí být reprezentovaná sadu entit.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="2d0f2-286">Je běžné název entity spojení `EntityName1EntityName2`, který v tomto případě bude `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="2d0f2-287">Nicméně doporučujeme, abyste zvolili název, který popisuje relace.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="2d0f2-288">Datové modely spustí jednoduchou a růst s žádné datové spojení často získávání datových částí později.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="2d0f2-289">Pokud začnete se entity popisný název, nebudete muset později změnit název.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="2d0f2-290">Připojení entity v ideálním případě by mít svůj vlastní přirozené název (pravděpodobně jednoslovné) v doméně obchodní.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="2d0f2-291">Například může knihy a zákazníky spojený prostřednictvím hodnocení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="2d0f2-292">Pro tento vztah `CourseAssignment` je vhodnější než `CourseInstructor`.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="2d0f2-293">Složený klíč</span><span class="sxs-lookup"><span data-stu-id="2d0f2-293">Composite key</span></span>

<span data-ttu-id="2d0f2-294">Vzhledem k tomu, že cizí klíče nejsou s možnou hodnotou Null a společně jednoznačně identifikovat každý řádek v tabulce, není nutné pro samostatný primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="2d0f2-295">*InstructorID* a *CourseID* vlastnosti by měla fungovat jako složený primární klíč.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="2d0f2-296">Jediný způsob, jak identifikovat složené primární klíče, aby EF je pomocí *rozhraní fluent API* (není možné provést pomocí atributů).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="2d0f2-297">Uvidíte, jak nakonfigurovat složené primární klíč v další části.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="2d0f2-298">Složený klíč zajistí, že i když můžete mít více řádků pro jeden kurzu a více řádků pro jeden lektorem, nemůže mít více řádků pro stejný lektorem a kurzu.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="2d0f2-299">`Enrollment` Spojení entity definuje vlastní primární klíč, takže je možné, duplikáty toto řazení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="2d0f2-300">Nechcete, aby tyto duplikáty, můžete přidat jedinečný index na pole cizího klíče, nebo nakonfigurovat `Enrollment` s primární složený klíč podobná `CourseAssignment`.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="2d0f2-301">Další informace najdete v tématu [indexy](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="2d0f2-302">Aktualizace kontext databáze</span><span class="sxs-lookup"><span data-stu-id="2d0f2-302">Update the database context</span></span>

<span data-ttu-id="2d0f2-303">Přidejte následující zvýrazněný kód, který *Data/SchoolContext.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="2d0f2-304">Tento kód přidá nové entity a nakonfiguruje složené primární klíče CourseAssignment entity.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="2d0f2-305">Fluent API alternativní atributů</span><span class="sxs-lookup"><span data-stu-id="2d0f2-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="2d0f2-306">Kód v `OnModelCreating` metodu `DbContext` třídy používá *rozhraní fluent API* pro konfiguraci EF chování.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="2d0f2-307">Rozhraní API se nazývá "fluent", protože se často používají v rozvádět do jednoho příkazu, jako v následujícím příkladě z řady volání metod společně [EF základní dokumentace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="2d0f2-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="2d0f2-308">V tomto kurzu používáte rozhraní fluent API jenom pro mapování databáze, které nemůžete dělat s atributy.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="2d0f2-309">Rozhraní fluent API však můžete také použít k určení většinu formátování, ověření a pravidla mapování, které můžete provést pomocí atributů.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="2d0f2-310">Některé atributy, jako `MinimumLength` nelze použít s rozhraní fluent API.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="2d0f2-311">Jak je uvedeno nahoře, `MinimumLength` nemění schématu, vztahuje se pouze klientské a serverové straně ověřovací pravidlo.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="2d0f2-312">Někteří vývojáři dávají přednost používání rozhraní fluent API výhradně tak, aby se zachovat jejich tříd entit "čistou."</span><span class="sxs-lookup"><span data-stu-id="2d0f2-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="2d0f2-313">Pokud chcete, a neexistují několik úprav, které lze provést pouze pomocí rozhraní fluent API je možné kombinovat atributy a rozhraní fluent API, ale obecně doporučený postup je vyberte jednu z těchto dvou přístupů a používání který konzistentně co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="2d0f2-314">Pokud používáte obě, Všimněte si vždy, když dojde ke konfliktu, rozhraní Fluent API přepsání atributy.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="2d0f2-315">Další informace o atributech oproti rozhraní fluent API najdete v tématu [metody konfigurace](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="2d0f2-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="2d0f2-316">Diagram znázorňující entitami</span><span class="sxs-lookup"><span data-stu-id="2d0f2-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="2d0f2-317">Následující obrázek znázorňuje diagram, který vytvořit výkonné nástroje Entity Framework pro dokončené školní model.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Entity diagram](complex-data-model/_static/diagram.png)

<span data-ttu-id="2d0f2-319">Kromě řádky vztah jeden mnoho (1 \*), můžete tady vidíte řádku vztah jeden pro žádná nebo jedna (1-0..1) mezi lektorem a OfficeAssignment entity a řádku nula nebo 1 n relace (0..1 k *) mezi Entity lektorem a oddělení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="2d0f2-320">Naplnit databázi daty testu</span><span class="sxs-lookup"><span data-stu-id="2d0f2-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="2d0f2-321">Nahraďte kód v *Data/DbInitializer.cs* souboru následujícím kódem, chcete-li poskytovat počáteční hodnoty dat pro nové entity, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="2d0f2-322">Jak už jste viděli v první kurz, většina tento kód jednoduše vytvoří nové entity objekty a načte ukázková data do vlastnosti podle potřeby pro testování.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="2d0f2-323">Všimněte si, jak jsou zpracovávány relace m: n: kód vytvoří vztahy vytvořením entity v `Enrollments` a `CourseAssignment` připojení sady entit.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="2d0f2-324">Přidat migrace</span><span class="sxs-lookup"><span data-stu-id="2d0f2-324">Add a migration</span></span>

<span data-ttu-id="2d0f2-325">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-325">Save your changes and build the project.</span></span> <span data-ttu-id="2d0f2-326">Potom otevřete příkazové okno ve složce projektu a zadejte `migrations add` příkazu (nedělají nic příkazu update-database ještě):</span><span class="sxs-lookup"><span data-stu-id="2d0f2-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="2d0f2-327">Zobrazí se upozornění o ztráta dat.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="2d0f2-328">Pokud jste se pokusili spustit `database update` příkaz v tomto okamžiku (není to je ještě), se zobrazí následující chyba:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="2d0f2-329">Příkaz ALTER TABLE konflikt s omezení FOREIGN KEY "FK_dbo. Course_dbo. Department_DepartmentID".</span><span class="sxs-lookup"><span data-stu-id="2d0f2-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="2d0f2-330">Ke konfliktu došlo v databázi "ContosoUniversity" tabulka "dbo. Oddělení", sloupec 'DepartmentID'.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="2d0f2-331">V případech, kdy můžete spustit migrace s existujícími daty, je třeba vložit se zakázaným inzerováním data do databáze, aby pokryl omezení cizího klíče.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="2d0f2-332">Generovaný kód v `Up` metoda přidá do tabulky během použití hodnot Null DepartmentID cizí klíč.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="2d0f2-333">Pokud již existují řádky v tabulce kurzu při spuštění kódu `AddColumn` operace selže, protože systém SQL Server není známo, jaké hodnoty Pokud chcete umístit do sloupce, který nemůže mít hodnotu null.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="2d0f2-334">Pro účely tohoto kurzu je potřeba spustit migraci na novou databázi, ale v praxi by musíte provádět migrace zpracovat existující data, takže zobrazit příklad toho, jak to udělat následující pokyny.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="2d0f2-335">Chcete-li tato migrace pracovat s existujícími daty, budete muset změnit kód umožnit nového sloupce výchozí hodnotu a vytvořit se zakázaným inzerováním oddělení s názvem "Temp" tak, aby fungoval jako výchozí oddělení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="2d0f2-336">V důsledku toho existující kurzu řádky budou všechny souviset oddělení "Temp" po `Up` metoda spustí.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="2d0f2-337">Otevřete *{timestamp}_ComplexDataModel.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>

* <span data-ttu-id="2d0f2-338">Zakomentovat řádek kódu, který přidá sloupec DepartmentID kurzu tabulky.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="2d0f2-339">Přidejte následující zvýrazněný kód po kód, který vytvoří tabulku oddělení:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="2d0f2-340">V případě produkční aplikace by napíšete kód nebo skripty k přidání oddělení řádky a řádky kurzu se týkají nové řádky oddělení.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="2d0f2-341">Potom už potřebovali byste oddělení "Temp" nebo výchozí hodnotu pro sloupec Course.DepartmentID.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="2d0f2-342">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="2d0f2-343">Změňte připojovací řetězec a aktualizaci databáze</span><span class="sxs-lookup"><span data-stu-id="2d0f2-343">Change the connection string and update the database</span></span>

<span data-ttu-id="2d0f2-344">Nyní máte nový kód `DbInitializer` třída, která přidává počáteční hodnoty dat pro nové entity pro prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="2d0f2-345">Chcete-li EF vytvoření nové prázdné databáze, změňte název databáze v připojovacím řetězci v *appSettings.JSON určený* ContosoUniversity3 nebo jiný název, který nepoužili na počítači, který používáte.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="2d0f2-346">Uložte změnu do *appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="2d0f2-347">Jako alternativu k změnit název databáze můžete odstranit databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="2d0f2-348">Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="2d0f2-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="2d0f2-349">Po jste změnili název databáze nebo odstranit databázi, spusťte `database update` příkazu v příkazovém okně spuštění migrace.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="2d0f2-350">Spusťte aplikaci způsobit, že `DbInitializer.Initialize` metoda spustit a přidat nové databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="2d0f2-351">Otevřít databázi v SSOX, jako jste to udělali dříve a rozbalte **tabulky** uzel pro zobrazení, že všechny tabulky byly vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="2d0f2-352">(Pokud máte SSOX otevřít z dřívější čas, klikněte na tlačítko **aktualizovat** tlačítko.)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabulky v SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="2d0f2-354">Spusťte aplikaci pro aktivaci inicializátoru kód, který doplňuje pro databázi.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="2d0f2-355">Klikněte pravým tlačítkem myši **CourseAssignment** tabulky a vyberte **Data zobrazení** ověřit, zda má data v ní.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![CourseAssignment data v SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="2d0f2-357">Souhrn</span><span class="sxs-lookup"><span data-stu-id="2d0f2-357">Summary</span></span>

<span data-ttu-id="2d0f2-358">Nyní máte složitější datový model a příslušné databáze.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="2d0f2-359">V následujícím kurzu se dozvíte informace o tom, jak získat přístup k datům související.</span><span class="sxs-lookup"><span data-stu-id="2d0f2-359">In the following tutorial, you'll learn more about how to access related data.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="2d0f2-360">[Předchozí](migrations.md)
> [další](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="2d0f2-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>
