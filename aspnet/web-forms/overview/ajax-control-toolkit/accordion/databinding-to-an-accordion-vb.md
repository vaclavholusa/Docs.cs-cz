---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
title: Datová vazba ovládacího prvku Accordion (VB) | Dokumentace Microsoftu
author: wenz
description: Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou. Panely jsou obvykle deklarované w...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: b19f0875-7d3e-4ecf-baa1-a0c693c765b3
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-vb
msc.type: authoredcontent
ms.openlocfilehash: 948741e3b8618a642c440a527fddef825faf595e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752452"
---
<a name="databinding-to-an-accordion-vb"></a><span data-ttu-id="61aed-104">Datová vazba ovládacího prvku Accordion (VB)</span><span class="sxs-lookup"><span data-stu-id="61aed-104">Databinding to an Accordion (VB)</span></span>
====================
<span data-ttu-id="61aed-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="61aed-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="61aed-106">[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) nebo [stahovat PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="61aed-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1VB.pdf)</span></span>

> <span data-ttu-id="61aed-107">Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou.</span><span class="sxs-lookup"><span data-stu-id="61aed-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="61aed-108">Panely jsou obvykle deklarované v rámci samotné stránky, ale vazba ke zdroji dat nabízí větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="61aed-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="61aed-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="61aed-109">Overview</span></span>

<span data-ttu-id="61aed-110">Ovládacího prvku Accordion sadou nástrojů AJAX Control Toolkit poskytuje více podoken a umožňuje uživateli zobrazit jeden z nich najednou.</span><span class="sxs-lookup"><span data-stu-id="61aed-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="61aed-111">Panely jsou obvykle deklarované v rámci samotné stránky, ale vazba ke zdroji dat nabízí větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="61aed-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="61aed-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="61aed-112">Steps</span></span>

<span data-ttu-id="61aed-113">Za prvé zdroj dat je povinný.</span><span class="sxs-lookup"><span data-stu-id="61aed-113">First of all, a data source is required.</span></span> <span data-ttu-id="61aed-114">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="61aed-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="61aed-115">Databáze je volitelná součást instalace sady Visual Studio (včetně express edition) a jsou také dostupné jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="61aed-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="61aed-116">Databáze AdventureWorks je součástí sad SQL Server 2005 ukázky a Sample Databases (stáhnout na [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="61aed-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="61aed-117">Nejjednodušší způsob, jak nastavit databázi je použít Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojit `AdventureWorks.mdf` databázový soubor.</span><span class="sxs-lookup"><span data-stu-id="61aed-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="61aed-118">V tomto příkladu předpokládáme, že název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a je umístěn ve stejném počítači jako webový server; to je taky výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="61aed-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="61aed-119">Pokud vaše nastavení se liší, je nutné upravit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="61aed-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="61aed-120">K aktivaci funkce technologie ASP.NET AJAX a Control Toolkit `ScriptManager` ovládací prvek je třeba umístit kdekoli na stránce (ale v rámci `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="61aed-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample1.aspx)]

<span data-ttu-id="61aed-121">Zadejte zdroj dat na stránku.</span><span class="sxs-lookup"><span data-stu-id="61aed-121">Then, add a data source to the page.</span></span> <span data-ttu-id="61aed-122">Chcete-li použít omezené množství dat, vybereme pouze prvních pět záznamů v tabulce dodavatele databáze AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="61aed-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="61aed-123">Pokud používáte Pomocníka s nastavením Visual Studio k vytvoření zdroje dat, mějte na paměti, že chyby v aktuální verzi předponu názvu tabulky (`Vendor`) s `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="61aed-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="61aed-124">Následující kód ukazuje správná syntaxe:</span><span class="sxs-lookup"><span data-stu-id="61aed-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample2.aspx)]

<span data-ttu-id="61aed-125">Pamatujte název zdroje dat (ID).</span><span class="sxs-lookup"><span data-stu-id="61aed-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="61aed-126">Tato velmi identifikace musí použít ve `DataSourceID` vlastnost ovládacího prvku typu Accordion:</span><span class="sxs-lookup"><span data-stu-id="61aed-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample3.aspx)]

<span data-ttu-id="61aed-127">V ovládacím prvku typu Accordion může poskytnout šablony pro různé části ovládacího prvku, včetně hlavičky (`<HeaderTemplate>`) a obsah (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="61aed-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="61aed-128">V rámci těchto prvků právě výstupu zobrazí data z dat zdroj, pomocí `DataBinder.Eval()` metody:</span><span class="sxs-lookup"><span data-stu-id="61aed-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample4.aspx)]

<span data-ttu-id="61aed-129">Když je stránka načtená, zdroj dat musí být vázán na prvku typu accordion s tímto kódem na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="61aed-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-vb/samples/sample5.aspx)]

<span data-ttu-id="61aed-130">Dokončete tento příklad, musíte definovat dvě třídy CSS, které jsou odkazovány v ovládacím prvku typu Accordion (ve vlastnostech `HeaderCssClass` a `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="61aed-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="61aed-131">Vložte následující kód `<head>` části stránky:</span><span class="sxs-lookup"><span data-stu-id="61aed-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-vb/samples/sample6.css)]


<span data-ttu-id="61aed-132">[![Data v prvku typu accordion pocházejí přímo ze zdroje dat.](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="61aed-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-vb/_static/image2.png)](databinding-to-an-accordion-vb/_static/image1.png)</span></span>

<span data-ttu-id="61aed-133">Data v prvku typu accordion pocházejí přímo ze zdroje dat ([kliknutím ji zobrazíte obrázek v plné velikosti](databinding-to-an-accordion-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="61aed-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="61aed-134">[Předchozí](dynamically-adding-an-accordion-pane-cs.md)
> [další](dynamically-adding-an-accordion-pane-vb.md)</span><span class="sxs-lookup"><span data-stu-id="61aed-134">[Previous](dynamically-adding-an-accordion-pane-cs.md)
[Next](dynamically-adding-an-accordion-pane-vb.md)</span></span>
