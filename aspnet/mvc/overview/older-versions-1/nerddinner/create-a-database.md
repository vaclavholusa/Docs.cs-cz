---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Vytvoření databáze | Microsoft Docs
author: microsoft
description: Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny večeře a RSVP data pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869137"
---
<a name="create-a-database"></a><span data-ttu-id="15fe8-103">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="15fe8-103">Create a Database</span></span>
====================
<span data-ttu-id="15fe8-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="15fe8-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="15fe8-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="15fe8-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="15fe8-106">Toto je krok 2 ze bezplatný [kurz aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , nevystavíte slabé stránky zabezpečení – prostřednictvím postup sestavení malá, ale dokončení, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="15fe8-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="15fe8-107">Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny večeře a RSVP data pro naši aplikaci NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="15fe8-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="15fe8-108">Pokud používáte ASP.NET MVC 3, doporučujeme provedením [získávání spuštěna s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Hudba úložiště](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="15fe8-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="15fe8-109">NerdDinner krok 2: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="15fe8-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="15fe8-110">Budeme k ukládání všech dat večeři a zasílání zpráv rysy pro naši aplikaci NerdDinner používat databázi.</span><span class="sxs-lookup"><span data-stu-id="15fe8-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="15fe8-111">Následující postup popisuje vytvoření databáze pomocí edice free SQL Server Express (které si můžete snadno nainstalovat pomocí V2 z [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="15fe8-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="15fe8-112">Všechny kód jsme budete zápisu funguje pro SQL Server Express a úplným SQL serverem.</span><span class="sxs-lookup"><span data-stu-id="15fe8-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="15fe8-113">Vytvoření nové databáze SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="15fe8-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="15fe8-114">Jsme budete začněte tím, že pravým tlačítkem myši na našem webový projekt a pak vyberte **Add -&gt;nová položka** příkazu v nabídce:</span><span class="sxs-lookup"><span data-stu-id="15fe8-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="15fe8-115">Tím se otevře dialogové okno "Přidat novou položku" sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15fe8-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="15fe8-116">Jsme budete filtrovat podle kategorie "Data" a vyberte šablonu, položka "Databáze systému SQL Server":</span><span class="sxs-lookup"><span data-stu-id="15fe8-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="15fe8-117">Jsme budete název databáze systému SQL Server Express, kterou chcete vytvořit "NerdDinner.mdf" a stiskněte tlačítko ok.</span><span class="sxs-lookup"><span data-stu-id="15fe8-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="15fe8-118">Visual Studio budou vyzváni k zadání Pokud nám chcete přidat tento soubor do našich \App\_adresář dat (která adresáře je již instalace s oprávnění ke čtení a zápis přístupu (ACL)):</span><span class="sxs-lookup"><span data-stu-id="15fe8-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="15fe8-119">Budete kliknete na tlačítko Ano, a naše nová databáze bude vytvořen a přidán do našich Průzkumníku řešení:</span><span class="sxs-lookup"><span data-stu-id="15fe8-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="15fe8-120">Vytváření tabulek v rámci naší databáze</span><span class="sxs-lookup"><span data-stu-id="15fe8-120">Creating Tables within our Database</span></span>

<span data-ttu-id="15fe8-121">Nyní je k dispozici novou prázdnou databázi.</span><span class="sxs-lookup"><span data-stu-id="15fe8-121">We now have a new empty database.</span></span> <span data-ttu-id="15fe8-122">Přidejte do něj umožňuje některé tabulky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-122">Let's add some tables to it.</span></span>

<span data-ttu-id="15fe8-123">K tomu budete jsme přejděte do okna Karta "Průzkumník serveru" v sadě Visual Studio, která umožňuje nám ke správě databází a serverů.</span><span class="sxs-lookup"><span data-stu-id="15fe8-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="15fe8-124">SQL Server Express databáze uložené ve \App\_Data složky naše aplikace se automaticky zobrazí v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="15fe8-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="15fe8-125">Jsme můžete volitelně použít ikonu "Připojení k databázi" horní okno "Průzkumníka serveru" přidat do seznamu také další databáze systému SQL Server (místní i vzdálený):</span><span class="sxs-lookup"><span data-stu-id="15fe8-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="15fe8-126">Do databáze hotový NerdDinner – jeden pro uložení naše večeří a dalších sledovat přijetí zasílání zpráv rysy jim přidáme dvě tabulky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="15fe8-127">Pravým tlačítkem na složku "Tabulky" v rámci naší databáze a zvolením příkazu nabídky "Přidat novou tabulku" můžeme vytvořit nové tabulky:</span><span class="sxs-lookup"><span data-stu-id="15fe8-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="15fe8-128">Tím se otevře návrháře tabulky, které umožňuje konfigurovat schéma naše tabulky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="15fe8-129">Pro naše "Večeří" tabulku přidáme 10 sloupce dat:</span><span class="sxs-lookup"><span data-stu-id="15fe8-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="15fe8-130">Chceme mít jedinečné primární klíč pro tabulku sloupce "DinnerID".</span><span class="sxs-lookup"><span data-stu-id="15fe8-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="15fe8-131">To jsme můžete nakonfigurovat tak, že kliknete pravým tlačítkem na sloupec "DinnerID" a vyberete položku nabídky "Nastavení primární klíč":</span><span class="sxs-lookup"><span data-stu-id="15fe8-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="15fe8-132">Kromě vytváření DinnerID primární klíč, chceme také ji nakonfigurovat jako sloupec "identity", jehož hodnota je zvýšena automaticky při přidání nové řádky dat do tabulky (tj. první řádek vloženého večeři bude mít DinnerID 1, druhý vložit řádek bude mít DinnerID 2 atd).</span><span class="sxs-lookup"><span data-stu-id="15fe8-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="15fe8-133">Můžeme provést zaškrtnutím sloupci "DinnerID" a pomocí editoru "Vlastnosti sloupce" nastavte vlastnost "(je Identity)" ve sloupci "Ano".</span><span class="sxs-lookup"><span data-stu-id="15fe8-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="15fe8-134">Budeme používat standardní identity výchozí hodnoty (začínají znakem 1 a zvýšit 1 každý nový řádek večeři):</span><span class="sxs-lookup"><span data-stu-id="15fe8-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="15fe8-135">Budete pak uložíme naše tabulku zadáním Ctrl-S nebo pomocí **souboru -&gt;Uložit** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="15fe8-136">Zobrazí se výzva nám název tabulky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-136">This will prompt us to name the table.</span></span> <span data-ttu-id="15fe8-137">Jsme budete pojmenujte ji "Večeří":</span><span class="sxs-lookup"><span data-stu-id="15fe8-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="15fe8-138">Naše nová tabulka večeří se potom zobrazí v rámci naší databáze v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="15fe8-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="15fe8-139">Potom jsme budete zopakujte výše uvedené kroky a vytvořit tabulku "Zasílání zpráv rysy".</span><span class="sxs-lookup"><span data-stu-id="15fe8-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="15fe8-140">Tato tabulka s mít 3 sloupce.</span><span class="sxs-lookup"><span data-stu-id="15fe8-140">This table with have 3 columns.</span></span> <span data-ttu-id="15fe8-141">Nemůžeme nastavit sloupci RsvpID jako primární klíč a způsobují, že sloupec identity:</span><span class="sxs-lookup"><span data-stu-id="15fe8-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="15fe8-142">Jsme budete ji uložit a pojmenujte ho názvu "Zasílání zpráv rysy".</span><span class="sxs-lookup"><span data-stu-id="15fe8-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="15fe8-143">Nastavení cizí klíč relace mezi tabulkami</span><span class="sxs-lookup"><span data-stu-id="15fe8-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="15fe8-144">Nyní je k dispozici dvě tabulky v rámci naší databáze.</span><span class="sxs-lookup"><span data-stu-id="15fe8-144">We now have two tables within our database.</span></span> <span data-ttu-id="15fe8-145">Naše posledním krokem návrhu schématu bude nastavení "jedna k mnoha" relaci mezi těmito dvěma tabulkami – tak, aby jsme každý řádek večeři přidružit počtu nula či více řádků zasílání zpráv rysy, které se použije na ni.</span><span class="sxs-lookup"><span data-stu-id="15fe8-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="15fe8-146">Uděláme konfigurace sloupce v tabulce zasílání zpráv rysy "DinnerID" tak, aby měl relaci cizího klíče na sloupec "DinnerID" v tabulce "Večeří".</span><span class="sxs-lookup"><span data-stu-id="15fe8-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="15fe8-147">K tomu otevřeme zasílání zpráv rysy tabulku v Návrháři tabulky dvojitým kliknutím na soubor v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="15fe8-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="15fe8-148">Budete zvolte sloupci "DinnerID" v něm, klikněte pravým tlačítkem a vyberte "Relationshps..."</span><span class="sxs-lookup"><span data-stu-id="15fe8-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="15fe8-149">příkaz nabídky kontextu:</span><span class="sxs-lookup"><span data-stu-id="15fe8-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="15fe8-150">Tím se otevře dialogové okno, které jsme slouží k nastavení relace mezi tabulkami:</span><span class="sxs-lookup"><span data-stu-id="15fe8-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="15fe8-151">Jsme budete kliknutím na tlačítko "Přidat" Přidat nový vztah do dialogu.</span><span class="sxs-lookup"><span data-stu-id="15fe8-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="15fe8-152">Po přidání vztahu jsme budete rozbalte uzel zobrazení stromu "Tabulky a sloupce Specification" v rámci mřížku vlastností vpravo dialogového okna a potom klikněte na tlačítko "..." napravo od ho:</span><span class="sxs-lookup"><span data-stu-id="15fe8-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="15fe8-153">Další dialog, který umožňuje určit, které tabulky a sloupce se účastní relace, jakož i název relace nám umožňují zobrazíte kliknutím na tlačítko "...".</span><span class="sxs-lookup"><span data-stu-id="15fe8-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="15fe8-154">Změníme na "Večeří" primární klíč tabulky a vyberte sloupec "DinnerID" v tabulce večeří jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="15fe8-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="15fe8-155">Naše zasílání zpráv rysy tabulka bude cizího klíče tabulky a zasílání zpráv rysy. Sloupec DinnerID bude přiřazen jako cizí klíč:</span><span class="sxs-lookup"><span data-stu-id="15fe8-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="15fe8-156">Každý řádek v tabulce zasílání zpráv rysy teď bude přidružen řádek v tabulce večeři.</span><span class="sxs-lookup"><span data-stu-id="15fe8-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="15fe8-157">Systému SQL Server bude udržovat pro nás – referenční integrity a zabránit nám v přidání nového řádku zasílání zpráv rysy, pokud neukazuje na platném řádku večeři.</span><span class="sxs-lookup"><span data-stu-id="15fe8-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="15fe8-158">Také nás nebude možné z odstraňování večeři řádku, pokud jsou stále RSVP řádků na ně odkazovat.</span><span class="sxs-lookup"><span data-stu-id="15fe8-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="15fe8-159">Přidání dat do našich tabulky</span><span class="sxs-lookup"><span data-stu-id="15fe8-159">Adding Data to our Tables</span></span>

<span data-ttu-id="15fe8-160">Umožňuje dokončit přidáním ukázková data do našich večeří tabulky.</span><span class="sxs-lookup"><span data-stu-id="15fe8-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="15fe8-161">Budeme-li přidat data do tabulky, pravým tlačítkem myši na něm v Průzkumníku serveru a zvolte příkaz "Zobrazit Data tabulky":</span><span class="sxs-lookup"><span data-stu-id="15fe8-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="15fe8-162">Přidáme pár řádků večeři dat, které jsme můžete použít později, jako jsme počáteční implementace aplikace:</span><span class="sxs-lookup"><span data-stu-id="15fe8-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="15fe8-163">Další krok</span><span class="sxs-lookup"><span data-stu-id="15fe8-163">Next Step</span></span>

<span data-ttu-id="15fe8-164">Jsme dokončili vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="15fe8-164">We've finished creating our database.</span></span> <span data-ttu-id="15fe8-165">Nyní vytvoříme třídy modelu, které jsme můžete použít k dotazu a aktualizovat ji.</span><span class="sxs-lookup"><span data-stu-id="15fe8-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="15fe8-166">[Předchozí](create-a-new-aspnet-mvc-project.md)
> [další](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="15fe8-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
