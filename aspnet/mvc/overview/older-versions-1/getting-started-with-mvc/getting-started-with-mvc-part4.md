---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: "Vytvoření databáze | Microsoft Docs"
author: shanselman
description: "Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 6a9617b8056f883d6be2547588b13063a58abd0e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-database"></a><span data-ttu-id="c0133-104">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="c0133-104">Creating a Database</span></span>
====================
<span data-ttu-id="c0133-105">podle [Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="c0133-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="c0133-106">Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c0133-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="c0133-107">Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.</span><span class="sxs-lookup"><span data-stu-id="c0133-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="c0133-108">Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="c0133-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="c0133-109">V této části jsme se chystáte vytvořit novou databázi SQL Express, jsme budete používat k ukládání a načítání naše film data.</span><span class="sxs-lookup"><span data-stu-id="c0133-109">In this section we are going to create a new SQL Express database that we'll use to store and retrieve our movie data.</span></span> <span data-ttu-id="c0133-110">V prostředí Visual Web Developer IDE, vyberte zobrazení | Průzkumník serveru.</span><span class="sxs-lookup"><span data-stu-id="c0133-110">From within the Visual Web Developer IDE, select View | Server Explorer.</span></span> <span data-ttu-id="c0133-111">Klikněte pravým tlačítkem na připojení dat a klikněte na tlačítko Přidat připojení...</span><span class="sxs-lookup"><span data-stu-id="c0133-111">Right click on Data Connections and click Add Connection...</span></span>

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

<span data-ttu-id="c0133-113">V dialogovém okně Zvolit zdroj dat vyberte server Microsoft SQL Server a vyberte pokračovat.</span><span class="sxs-lookup"><span data-stu-id="c0133-113">In the Choose Data Source dialog, select Microsoft SQL Server and select Continue.</span></span>

![](getting-started-with-mvc-part4/_static/image2.png)

<span data-ttu-id="c0133-114">V dialogovém okně Přidat připojení zadejte ". \SQLEXPRESS" pro název serveru a zadejte "Filmy" jako název nové databáze.</span><span class="sxs-lookup"><span data-stu-id="c0133-114">In the Add Connection dialog, enter ".\SQLEXPRESS" for your Server Name, and enter "Movies" as the name for your new database.</span></span>

<span data-ttu-id="c0133-115">[![Přidat dialogové okno připojení](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-115">[![Add Connection dialog](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)</span></span>

<span data-ttu-id="c0133-116">Klikněte na tlačítko OK a zobrazí se dotaz, pokud chcete vytvořit tuto databázi.</span><span class="sxs-lookup"><span data-stu-id="c0133-116">Click OK and you'll be asked if you want to create that database.</span></span> <span data-ttu-id="c0133-117">Vyberte možnost Ano.</span><span class="sxs-lookup"><span data-stu-id="c0133-117">Select yes.</span></span>

<span data-ttu-id="c0133-118">[![Vytvořit filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-118">[![Create Movies?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)</span></span>

<span data-ttu-id="c0133-119">Nyní máte v Průzkumníku serveru k dispozici prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="c0133-119">Now you've got an empty database in Server Explorer.</span></span>

![Přidat novou tabulku](getting-started-with-mvc-part4/_static/image7.png)

<span data-ttu-id="c0133-121">Klikněte pravým tlačítkem na tabulky a klikněte na tlačítko Přidat tabulku.</span><span class="sxs-lookup"><span data-stu-id="c0133-121">Right click on Tables and click Add Table.</span></span> <span data-ttu-id="c0133-122">Zobrazí se návrháře tabulky.</span><span class="sxs-lookup"><span data-stu-id="c0133-122">The Table Designer will appear.</span></span> <span data-ttu-id="c0133-123">Přidáte sloupce pro Id, názvu, ReleaseDate, Genre a ceny.</span><span class="sxs-lookup"><span data-stu-id="c0133-123">Add columns for Id, Title, ReleaseDate, Genre, and Price.</span></span> <span data-ttu-id="c0133-124">Klikněte pravým tlačítkem na sloupec ID a klikněte na tlačítko Nastavit primární klíč.</span><span class="sxs-lookup"><span data-stu-id="c0133-124">Right click on the ID column and click set Primary Key.</span></span> <span data-ttu-id="c0133-125">Zde je můj návrh oblasti, které vypadá jako.</span><span class="sxs-lookup"><span data-stu-id="c0133-125">Here's what my design areas looks like.</span></span>

<span data-ttu-id="c0133-126">[![Editor tabulek databáze](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-126">[![Database Table Editor](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)</span></span>

<span data-ttu-id="c0133-127">Vyberte sloupec Id také a v části Vlastnosti sloupce níže změňte "Specifikace Identity" na "Ano".</span><span class="sxs-lookup"><span data-stu-id="c0133-127">Also, select the Id column and under Column Properties below change "Identity Specification" to "Yes."</span></span>

<span data-ttu-id="c0133-128">[![IsIdentity – vlastnosti sloupce](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-128">[![IsIdentity - Column Properties](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)</span></span>

<span data-ttu-id="c0133-129">Když vám to provést, klikněte na ikonu Uložit na panelu nástrojů nebo vyberte soubor | Uložit v nabídce a název nové tabulky "**film**" (singulární).</span><span class="sxs-lookup"><span data-stu-id="c0133-129">When you've got it done, click the Save icon in the toolbar or select File | Save from the menu, and name your table "**Movie**" (singular).</span></span> <span data-ttu-id="c0133-130">My jsme databázi a tabulku!</span><span class="sxs-lookup"><span data-stu-id="c0133-130">We've got a database and a table!</span></span>

<span data-ttu-id="c0133-131">[![Zvolte název](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-131">[![Choose Name](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)</span></span>

<span data-ttu-id="c0133-132">Přejděte zpět do Průzkumníka serveru a klikněte pravým tlačítkem v tabulce film a pak vyberte příkaz "Zobrazit Data tabulky".</span><span class="sxs-lookup"><span data-stu-id="c0133-132">Go back to Server Explorer and right click the Movie table, then select "Show Table Data."</span></span> <span data-ttu-id="c0133-133">Zadejte pár filmy, takže databáze má některá data.</span><span class="sxs-lookup"><span data-stu-id="c0133-133">Enter a few movies so our database has some data.</span></span>

<span data-ttu-id="c0133-134">[![Úpravy tabulek databáze](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-134">[![Database Table Editing](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)</span></span>

## <a name="creating-a-model"></a><span data-ttu-id="c0133-135">Vytvoření modelu</span><span class="sxs-lookup"><span data-stu-id="c0133-135">Creating a Model</span></span>

<span data-ttu-id="c0133-136">Nyní přejděte zpět do Průzkumníka řešení na pravé straně rozhraní IDE a klikněte pravým tlačítkem na složku modely a vyberte Přidat | Nová položka.</span><span class="sxs-lookup"><span data-stu-id="c0133-136">Now, switch back to the Solution Explorer on the right hand side of the IDE and right-click on the Models folder and select Add | New Item.</span></span>

<span data-ttu-id="c0133-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-137">[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)</span></span>

<span data-ttu-id="c0133-138">Vytvoříme pro vytvoření modelu Entity z naší nové databáze.</span><span class="sxs-lookup"><span data-stu-id="c0133-138">We're going to create an Entity Model from our new database.</span></span> <span data-ttu-id="c0133-139">Sadu tříd, se přidá do našich projekt, který lze snadno nám pro dotazování a pracovat s daty v rámci naší databáze.</span><span class="sxs-lookup"><span data-stu-id="c0133-139">This will add a set of classes to our project that makes it easy for us to query and manipulate the data within our database.</span></span> <span data-ttu-id="c0133-140">Vyberte datový uzel na levé straně dialogového okna a potom vyberte šablonu, položka ADO.NET Entity Data Model.</span><span class="sxs-lookup"><span data-stu-id="c0133-140">Select the Data node on the left-hand side of the dialog, and then select the ADO.NET Entity Data Model item template.</span></span> <span data-ttu-id="c0133-141">Pojmenujte ji Movies.edmx.</span><span class="sxs-lookup"><span data-stu-id="c0133-141">Name it Movies.edmx.</span></span>

<span data-ttu-id="c0133-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-142">[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)</span></span>

<span data-ttu-id="c0133-143">Kliknutím na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="c0133-143">Click the "Add" button.</span></span> <span data-ttu-id="c0133-144">Tím se pak spustí "Entity Data Model průvodce".</span><span class="sxs-lookup"><span data-stu-id="c0133-144">This will then launch the "Entity Data Model Wizard".</span></span>

<span data-ttu-id="c0133-145">V dialogu Nový, která se objeví vyberte generování z databáze.</span><span class="sxs-lookup"><span data-stu-id="c0133-145">In the new dialog that pops up, select Generate from Database.</span></span> <span data-ttu-id="c0133-146">Vzhledem k tomu, že jsme provedli právě databáze, budeme potřebovat pouze říct o našem novou databázi a její tabulkou rozhraní Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0133-146">Since we've just made a database, we'll only need to tell the Entity Framework about our new database and its table.</span></span> <span data-ttu-id="c0133-147">Klikněte na tlačítko vedle uložit naše připojení k databázi v konfiguraci naše webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="c0133-147">Click Next to save our database connection in our web application's configuration.</span></span> <span data-ttu-id="c0133-148">Teď, zkontrolujte tabulky a film zaškrtávací políčko a klikněte na tlačítko Dokončit.</span><span class="sxs-lookup"><span data-stu-id="c0133-148">Now, check the Tables and Movie checkbox and click Finish.</span></span>

<span data-ttu-id="c0133-149">[![Průvodce Model dat entity](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-149">[![Entity Data Model Wizard](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)</span></span>

<span data-ttu-id="c0133-150">Nyní jsme v tématu naše nová tabulka film v Návrháři Entity Framework a k němu přístup z kódu.</span><span class="sxs-lookup"><span data-stu-id="c0133-150">Now we can see our new Movie table in the Entity Framework Designer and access it from code.</span></span>

<span data-ttu-id="c0133-151">[![Filmy - sadu Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="c0133-151">[![Movies - Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)</span></span>

<span data-ttu-id="c0133-152">Na návrhovou plochu, která se zobrazí třídu "Film".</span><span class="sxs-lookup"><span data-stu-id="c0133-152">On the design surface you can see a "Movie" class.</span></span> <span data-ttu-id="c0133-153">Tato třída se mapuje na tabulku "Film" v naší databázi, a každou vlastnost v něm mapuje na sloupec v tabulce.</span><span class="sxs-lookup"><span data-stu-id="c0133-153">This class maps to the "Movie" table in our database, and each property within it maps to a column with the table.</span></span> <span data-ttu-id="c0133-154">Každá instance třídy "Film" bude odpovídat na řádek v tabulce "Film".</span><span class="sxs-lookup"><span data-stu-id="c0133-154">Each instance of a "Movie" class will correspond to a row within the "Movie" table.</span></span>

<span data-ttu-id="c0133-155">Pokud chcete výchozí pojmenovávání a mapování názvů používá rozhraní Entity Framework, můžete změnit nebo si je přizpůsobit návrháře Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c0133-155">If you don't like the default naming and mapping conventions used by the Entity Framework, you can use the Entity Framework designer to change or customize them.</span></span> <span data-ttu-id="c0133-156">Pro tuto aplikaci jsme budete používat výchozí hodnoty a právě uložte soubor jako-je.</span><span class="sxs-lookup"><span data-stu-id="c0133-156">For this application we'll use the defaults and just save the file as-is.</span></span>

<span data-ttu-id="c0133-157">Teď umožňuje pracovat s některé reálná data!</span><span class="sxs-lookup"><span data-stu-id="c0133-157">Now, let's work with some real data!</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="c0133-158">[Předchozí](getting-started-with-mvc-part3.md)
[další](getting-started-with-mvc-part5.md)</span><span class="sxs-lookup"><span data-stu-id="c0133-158">[Previous](getting-started-with-mvc-part3.md)
[Next](getting-started-with-mvc-part5.md)</span></span>
