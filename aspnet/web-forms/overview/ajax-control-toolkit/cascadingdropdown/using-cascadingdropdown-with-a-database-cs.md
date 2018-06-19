---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: Pomocí CascadingDropDown s databází (C#) | Microsoft Docs
author: wenz
description: Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby změny v jedné rozevírací seznam zatížení přidružené hodnoty v anoth...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879124"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="2362d-103">Pomocí CascadingDropDown s databází (C#)</span><span class="sxs-lookup"><span data-stu-id="2362d-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="2362d-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2362d-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2362d-105">[Stáhněte si kód](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2362d-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="2362d-106">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="2362d-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2362d-107">Aby tato možnost fungovala musí být vytvořeny speciální webové služby.</span><span class="sxs-lookup"><span data-stu-id="2362d-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="2362d-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="2362d-108">Overview</span></span>

<span data-ttu-id="2362d-109">Ovládací prvek CascadingDropDown v Toolkitu AJAX rozšiřuje ovládací prvek rozevírací seznam tak, aby se změny v jedné rozevírací seznam zatížení přidružené hodnoty v jiné rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="2362d-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="2362d-110">(Například jeden seznam obsahuje seznam nám stavy a další seznamu je pak vyplněn hlavní města v tomto stavu.) Aby tato možnost fungovala musí být vytvořeny speciální webové služby.</span><span class="sxs-lookup"><span data-stu-id="2362d-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="2362d-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="2362d-111">Steps</span></span>

<span data-ttu-id="2362d-112">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="2362d-112">First of all, a data source is required.</span></span> <span data-ttu-id="2362d-113">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="2362d-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="2362d-114">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="2362d-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="2362d-115">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="2362d-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="2362d-116">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="2362d-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="2362d-117">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="2362d-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="2362d-118">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="2362d-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="2362d-119">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř &lt; `form` &gt; element):</span><span class="sxs-lookup"><span data-stu-id="2362d-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="2362d-120">V dalším kroku jsou potřebné dvě prvky rozevírací seznam.</span><span class="sxs-lookup"><span data-stu-id="2362d-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="2362d-121">V této ukázce používáme dodavatele a kontaktních informací z AdventureWorks, proto jsme vytvořit jeden seznam pro jsou k dispozici dodavatelé a jeden pro kontakty, k dispozici:</span><span class="sxs-lookup"><span data-stu-id="2362d-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="2362d-122">Dva Extender CascadingDropDown pak, je nutné přidat na stránku.</span><span class="sxs-lookup"><span data-stu-id="2362d-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="2362d-123">Jeden doplní v prvním seznamu (dodavatelé) a jiného výplní seznamu druhý (kontakty).</span><span class="sxs-lookup"><span data-stu-id="2362d-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="2362d-124">Musí být nastavena následující atributy:</span><span class="sxs-lookup"><span data-stu-id="2362d-124">The following attributes must be set:</span></span>

- <span data-ttu-id="2362d-125">`ServicePath`: Adresa URL webové služby doručování položky seznamu</span><span class="sxs-lookup"><span data-stu-id="2362d-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="2362d-126">`ServiceMethod`: Webové metody doručování položky seznamu</span><span class="sxs-lookup"><span data-stu-id="2362d-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="2362d-127">`TargetControlID`: ID rozevíracího seznamu</span><span class="sxs-lookup"><span data-stu-id="2362d-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="2362d-128">`Category`: Informace o kategoriích, které je odeslána do webové metody při volání</span><span class="sxs-lookup"><span data-stu-id="2362d-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="2362d-129">`PromptText`: Text zobrazí v případě, že asynchronní načítání seznamu dat ze serveru</span><span class="sxs-lookup"><span data-stu-id="2362d-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="2362d-130">`ParentControlID`: (nepovinný) nadřazené rozevírací seznam této aktivační události načítání aktuálního seznamu.</span><span class="sxs-lookup"><span data-stu-id="2362d-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="2362d-131">V závislosti na programovací jazyk použitý změní se název příslušné webové služby, ale všechny ostatní hodnoty atributů jsou stejné.</span><span class="sxs-lookup"><span data-stu-id="2362d-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="2362d-132">Tady je element CascadingDropDown pro v prvním rozevíracím seznamu:</span><span class="sxs-lookup"><span data-stu-id="2362d-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="2362d-133">Ovládací prvek Extender pro druhý seznamu je nutné nastavit `ParentControlID` atributů tak, že vyberete položku v seznamu aktivačních událostí dodavatelé načítání přidružených elementů v seznamu kontaktů.</span><span class="sxs-lookup"><span data-stu-id="2362d-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="2362d-134">Ve webové službě, která je nastavena takto se pak provádí samotnou práci.</span><span class="sxs-lookup"><span data-stu-id="2362d-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="2362d-135">Všimněte si, že `[ScriptService]` je použit atribut, jinak prvku ASP.NET AJAX nemůže vytvořit proxy server JavaScript pro přístup k webové metody z kódu klientský skript.</span><span class="sxs-lookup"><span data-stu-id="2362d-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="2362d-136">Podpis metody webové volá CascadingDropDown vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="2362d-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="2362d-137">Takže návratová hodnota musí být pole typu `CascadingDropDownNameValue` je definované Toolkitu.</span><span class="sxs-lookup"><span data-stu-id="2362d-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="2362d-138">`GetVendors()` Metoda je poměrně snadno implementovat: kód připojuje k databázi AdventureWorks a dotazy na prvních 25 dodavatele.</span><span class="sxs-lookup"><span data-stu-id="2362d-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="2362d-139">První parametr ve `CascadingDropDownNameValue` konstruktor popisek položky seznamu, druhý je jeho hodnota (hodnota atributu ve formátu HTML na &lt; `option` &gt; element).</span><span class="sxs-lookup"><span data-stu-id="2362d-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="2362d-140">Zde je kód:</span><span class="sxs-lookup"><span data-stu-id="2362d-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="2362d-141">Získávání přidružené kontakty pro dodavatele (název metody: `GetContactsForVendor()`) je trochu trickier.</span><span class="sxs-lookup"><span data-stu-id="2362d-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="2362d-142">Nejprve je třeba stanovit dodavatele, který byl vybrán v prvním rozevíracím seznamu.</span><span class="sxs-lookup"><span data-stu-id="2362d-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="2362d-143">Toolkitu definuje Pomocná metoda pro tuto úlohu: `ParseKnownCategoryValuesString()` metoda vrátí `StringDictionary` element s daty rozevíracího seznamu:</span><span class="sxs-lookup"><span data-stu-id="2362d-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="2362d-144">Z bezpečnostních důvodů musí nejdřív ověřit tato data.</span><span class="sxs-lookup"><span data-stu-id="2362d-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="2362d-145">Ano, pokud je položka dodavatele (protože `Category` první CascadingDropDown elementu je nastavena na `"Vendor"`), může načíst ID vybraného dodavatele:</span><span class="sxs-lookup"><span data-stu-id="2362d-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="2362d-146">Zbývající část metody je poměrně jednoduché, potom.</span><span class="sxs-lookup"><span data-stu-id="2362d-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="2362d-147">ID dodavatele slouží jako parametr pro dotaz SQL, který načte všechny přidružené kontakty pro příslušného dodavatele.</span><span class="sxs-lookup"><span data-stu-id="2362d-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="2362d-148">Ještě jednou, metoda vrátí pole typu `CascadingDropDownNameValue`.</span><span class="sxs-lookup"><span data-stu-id="2362d-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="2362d-149">Načtení stránky ASP.NET a po nějakou dobu seznamu dodavatele vyplněno 25 položky.</span><span class="sxs-lookup"><span data-stu-id="2362d-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="2362d-150">Vyberte jednu položku a Všimněte si, jak je druhý rozevíracího seznamu vyplněn data.</span><span class="sxs-lookup"><span data-stu-id="2362d-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="2362d-151">[![V prvním seznamu se vyplní automaticky](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2362d-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="2362d-152">V prvním seznamu se vyplní automaticky ([Kliknutím zobrazit obrázek v plné velikosti](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2362d-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="2362d-153">[![Druhý seznamu je vyplněna podle výběru v seznamu první](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2362d-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="2362d-154">Druhý seznamu je vyplněna podle výběru v seznamu první ([Kliknutím zobrazit obrázek v plné velikosti](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2362d-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2362d-155">[Předchozí](filling-a-list-using-cascadingdropdown-cs.md)
> [další](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="2362d-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
