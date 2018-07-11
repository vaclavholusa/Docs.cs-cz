---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Vytvoření databáze | Dokumentace Microsoftu
author: microsoft
description: Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny společnosti dinner a potvrďte svou účast ještě data pro naši aplikaci NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818963"
---
<a name="create-a-database"></a><span data-ttu-id="65605-103">Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="65605-103">Create a Database</span></span>
====================
<span data-ttu-id="65605-104">podle [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="65605-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="65605-105">Stáhnout PDF</span><span class="sxs-lookup"><span data-stu-id="65605-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="65605-106">Toto je krok 2 bezplatné [kurz vývoje aplikace "NerdDinner"](introducing-the-nerddinner-tutorial.md) , který procházení procházení po tom, jak sestavit malý, ale bylo možné provést, webové aplikace pomocí ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="65605-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="65605-107">Krok 2 ukazuje postup vytvoření databáze, která uchovává všechny společnosti dinner a potvrďte svou účast ještě data pro naši aplikaci NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="65605-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="65605-108">Pokud používáte ASP.NET MVC 3, doporučujeme je provést [získávání začít s MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) nebo [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) kurzy.</span><span class="sxs-lookup"><span data-stu-id="65605-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="65605-109">NerdDinner krok 2: Vytvoření databáze</span><span class="sxs-lookup"><span data-stu-id="65605-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="65605-110">Popíšeme databázi k ukládání všech dat Dinner a RSVP pro naši aplikaci NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="65605-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="65605-111">Následující postup popisuje vytvoření databáze pomocí bezplatné edice systému SQL Server Express (které si můžete snadno nainstalovat pomocí verzí V2 [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="65605-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="65605-112">Budeme psát kód spolupracuje s SQL Server Express a plnou instalaci SQL serveru.</span><span class="sxs-lookup"><span data-stu-id="65605-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="65605-113">Vytvoření nové databáze systému SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="65605-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="65605-114">Použijeme začněte tím, že pravým tlačítkem myši na náš webový projekt a pak vyberte **Add -&gt;nová položka** příkazu nabídky:</span><span class="sxs-lookup"><span data-stu-id="65605-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="65605-115">Tím se otevře dialogové okno sady Visual Studio "Přidat novou položku".</span><span class="sxs-lookup"><span data-stu-id="65605-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="65605-116">Vytvoříme filtrovat podle kategorie "Data" a vyberte šablonu položky "SQL Server Database":</span><span class="sxs-lookup"><span data-stu-id="65605-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="65605-117">Používáme bude název databáze systému SQL Server Express, které chceme vytvořit "NerdDinner.mdf" a stiskněte ok.</span><span class="sxs-lookup"><span data-stu-id="65605-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="65605-118">Visual Studio se pak požádejte nás pokud chceme přidat tento soubor do našich \App\_adresář dat (což je adresář již instalace pomocí oprávnění ke čtení a zápis přístupu (ACL)):</span><span class="sxs-lookup"><span data-stu-id="65605-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="65605-119">Můžeme vám tlačítko "Ano" a naši novou databázi, bude vytvořen a přidán do našich Průzkumníku řešení:</span><span class="sxs-lookup"><span data-stu-id="65605-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="65605-120">Vytváření tabulek v rámci naší databázi</span><span class="sxs-lookup"><span data-stu-id="65605-120">Creating Tables within our Database</span></span>

<span data-ttu-id="65605-121">Nyní je k dispozici nové prázdné databáze.</span><span class="sxs-lookup"><span data-stu-id="65605-121">We now have a new empty database.</span></span> <span data-ttu-id="65605-122">Přidejme do ní několik tabulek.</span><span class="sxs-lookup"><span data-stu-id="65605-122">Let's add some tables to it.</span></span>

<span data-ttu-id="65605-123">K tomu budete přejdeme na kartě okna "Průzkumník serveru" v sadě Visual Studio, které umožňuje spravovat databáze a servery.</span><span class="sxs-lookup"><span data-stu-id="65605-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="65605-124">SQL Server Express databáze uložené ve \App\_složky dat naší aplikace se automaticky zobrazí v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="65605-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="65605-125">Jsme můžete volitelně použít ikonu "Připojení k databázi" v horní části okna "Průzkumník serveru" přidat další databáze systému SQL Server (místní a vzdálené) i do seznamu:</span><span class="sxs-lookup"><span data-stu-id="65605-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="65605-126">Přidáme dvě tabulky na naše databáze NerdDinner – jeden pro uložení naše večeří a druhou pro sledování přijetí reakce na ně.</span><span class="sxs-lookup"><span data-stu-id="65605-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="65605-127">Vytvoříme nové tabulky tak, že pravým tlačítkem na složku "Tabulky" v rámci naší databázi a zvolte příkaz "Přidat novou tabulku" nabídky:</span><span class="sxs-lookup"><span data-stu-id="65605-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="65605-128">Tím se otevře návrháře tabulky, která umožňuje konfigurovat schématu naší tabulky.</span><span class="sxs-lookup"><span data-stu-id="65605-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="65605-129">Pro náš "Večeří" přidáme 10 sloupce dat:</span><span class="sxs-lookup"><span data-stu-id="65605-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="65605-130">Chceme, aby sloupec "DinnerID" na jedinečné primární klíč pro tabulku.</span><span class="sxs-lookup"><span data-stu-id="65605-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="65605-131">Můžeme to nakonfigurovat tak, že kliknete pravým tlačítkem na sloupec "DinnerID" a zvolíte položku nabídky "Nastavit primární klíč":</span><span class="sxs-lookup"><span data-stu-id="65605-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="65605-132">Kromě toho DinnerID primární klíč, můžeme také vhodné ho nakonfigurovat jako sloupec "identity", jehož hodnota je automatický navýšeno při přidání nové řádky dat do tabulky (to znamená, první řádek vloženého Dinner bude mít DinnerID 1, druhý vložit řádek bude mít DinnerID 2 atd).</span><span class="sxs-lookup"><span data-stu-id="65605-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="65605-133">Můžeme to udělat tak, že vyberete sloupec "DinnerID" a potom použít editor "Vlastnosti sloupce" nastavit vlastnost "(je identita)" sloupce na "Ano".</span><span class="sxs-lookup"><span data-stu-id="65605-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="65605-134">Budeme používat standardní identity výchozí hodnoty (začínají znakem 1 a zvýší 1 na každý řádek novou Dinner):</span><span class="sxs-lookup"><span data-stu-id="65605-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="65605-135">Potom uložíme také našeho zadáním Ctrl + S nebo s použitím **souboru -&gt;Uložit** příkazu nabídky.</span><span class="sxs-lookup"><span data-stu-id="65605-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="65605-136">Zobrazí se výzva nám název tabulky.</span><span class="sxs-lookup"><span data-stu-id="65605-136">This will prompt us to name the table.</span></span> <span data-ttu-id="65605-137">Používáme bude název "Večeří":</span><span class="sxs-lookup"><span data-stu-id="65605-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="65605-138">Naše nová tabulka večeří se pak zobrazí v rámci naší databázi v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="65605-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="65605-139">Pak vytvoříme opakujte předchozí postup a vytvořte tabulku "RSVP".</span><span class="sxs-lookup"><span data-stu-id="65605-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="65605-140">Tato tabulka s obsahovat 3 sloupce.</span><span class="sxs-lookup"><span data-stu-id="65605-140">This table with have 3 columns.</span></span> <span data-ttu-id="65605-141">Budeme nastavit sloupec RsvpID jako primární klíč a také pomáhají zajistit sloupec identity:</span><span class="sxs-lookup"><span data-stu-id="65605-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="65605-142">Můžeme vám ho uložte a přiřaďte jí název "RSVP".</span><span class="sxs-lookup"><span data-stu-id="65605-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="65605-143">Vytvoření cizího klíče relace mezi tabulkami</span><span class="sxs-lookup"><span data-stu-id="65605-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="65605-144">Nyní je k dispozici dvě tabulky v databázi.</span><span class="sxs-lookup"><span data-stu-id="65605-144">We now have two tables within our database.</span></span> <span data-ttu-id="65605-145">Naše posledním krokem návrhu schématu bude nastavení "jedna k mnoha" relaci mezi těmito dvěma tabulkami – tak, aby nám každý řádek Dinner přidružit nula nebo více řádků reakce, vztahující se k němu.</span><span class="sxs-lookup"><span data-stu-id="65605-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="65605-146">Uděláme to tím, že nakonfigurujete tabulce RSVP "DinnerID" sloupci vztah cizího klíče na sloupci "DinnerID" v tabulce "Večeří".</span><span class="sxs-lookup"><span data-stu-id="65605-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="65605-147">Provedete to tak otevřeme RSVP tabulku v Návrháři tabulky na něj poklikejte v Průzkumníku serveru.</span><span class="sxs-lookup"><span data-stu-id="65605-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="65605-148">Pak vybereme sloupec "DinnerID" v ní, klikněte pravým tlačítkem a zvolte příkaz "Relationshps …" kontextové nabídky:</span><span class="sxs-lookup"><span data-stu-id="65605-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="65605-149">Tím se otevře dialogové okno, které můžete použít k nastavení relace mezi tabulkami:</span><span class="sxs-lookup"><span data-stu-id="65605-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="65605-150">Budete kliknete na tlačítko "Přidat" do dialogového okna Přidat novou relaci.</span><span class="sxs-lookup"><span data-stu-id="65605-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="65605-151">Po přidání relace jsme budete napravo od dialogového okna rozbalte uzel stromové zobrazení "Tabulky a sloupce specifikace" v mřížce vlastností a pak klikněte na tlačítko "..." napravo od jeho:</span><span class="sxs-lookup"><span data-stu-id="65605-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="65605-152">Kliknutím na tlačítko "..." se zobrazí další dialog, který umožňuje určit, které tabulky a sloupce jsou součástí relace, jakož i umožňují nám název relace.</span><span class="sxs-lookup"><span data-stu-id="65605-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="65605-153">Změníme na "Večeří" primární klíč tabulky a vyberte "DinnerID" sloupce v tabulce večeří jako primární klíč.</span><span class="sxs-lookup"><span data-stu-id="65605-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="65605-154">Naše reakce tabulka bude tabulka cizího klíče a RSVP. Sloupec DinnerID bude přiřazen jako cizí klíč:</span><span class="sxs-lookup"><span data-stu-id="65605-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="65605-155">Každý řádek v tabulce RSVP teď bude spojená s řádek v tabulce večeři.</span><span class="sxs-lookup"><span data-stu-id="65605-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="65605-156">SQL Server se udržují referenční integritu pro nás – a zabránit nám přidáváte nový řádek RSVP neukazuje na platném řádku Dinner.</span><span class="sxs-lookup"><span data-stu-id="65605-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="65605-157">To zároveň zabrání nám odstraňování Dinner řádku, pokud jsou stále potvrďte svou účast na něj odkazující řádky.</span><span class="sxs-lookup"><span data-stu-id="65605-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="65605-158">Přidání dat do naší tabulky</span><span class="sxs-lookup"><span data-stu-id="65605-158">Adding Data to our Tables</span></span>

<span data-ttu-id="65605-159">Pojďme dokončit tak, že přidáte nějaká ukázková data do našich večeří tabulky.</span><span class="sxs-lookup"><span data-stu-id="65605-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="65605-160">Můžeme přidat data do tabulky kliknutím pravým tlačítkem na něj v Průzkumníkovi serveru a zvolte příkaz "Zobrazit Data tabulky":</span><span class="sxs-lookup"><span data-stu-id="65605-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="65605-161">Přidáme několik řádků Dinner data, která můžeme použít později, jako jsme počáteční implementaci aplikace:</span><span class="sxs-lookup"><span data-stu-id="65605-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="65605-162">Dalším krokem</span><span class="sxs-lookup"><span data-stu-id="65605-162">Next Step</span></span>

<span data-ttu-id="65605-163">Jsme dokončili vytváření databáze.</span><span class="sxs-lookup"><span data-stu-id="65605-163">We've finished creating our database.</span></span> <span data-ttu-id="65605-164">Pojďme teď vytvořit tříd modelu, pomocí kterých můžeme pro dotazování a aktualizaci.</span><span class="sxs-lookup"><span data-stu-id="65605-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="65605-165">[Předchozí](create-a-new-aspnet-mvc-project.md)
> [další](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="65605-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
