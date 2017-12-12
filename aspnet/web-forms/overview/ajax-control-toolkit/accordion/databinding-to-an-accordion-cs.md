---
uid: web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
title: "Datové vazby k typu Accordion (C#) | Microsoft Docs"
author: wenz
description: "Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy. Panely jsou obvykle deklarovány w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 9c8f0054-e319-46f8-80c0-35b606d2fbd4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/databinding-to-an-accordion-cs
msc.type: authoredcontent
ms.openlocfilehash: a8250f58655b8fe8638d8e7a7b084ee9c33fe986
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="databinding-to-an-accordion-c"></a><span data-ttu-id="de9ca-104">Datové vazby k typu Accordion (C#)</span><span class="sxs-lookup"><span data-stu-id="de9ca-104">Databinding to an Accordion (C#)</span></span>
====================
<span data-ttu-id="de9ca-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="de9ca-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="de9ca-106">[Stáhněte si kód](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="de9ca-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion1CS.pdf)</span></span>

> <span data-ttu-id="de9ca-107">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="de9ca-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="de9ca-108">Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="de9ca-108">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>


## <a name="overview"></a><span data-ttu-id="de9ca-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="de9ca-109">Overview</span></span>

<span data-ttu-id="de9ca-110">Ovládacího prvku typu Accordion prvku Toolkitu AJAX poskytuje více podokna a umožňuje uživatelům zobrazit jeden z nich vždy.</span><span class="sxs-lookup"><span data-stu-id="de9ca-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="de9ca-111">Panely jsou obvykle deklarované v rámci vlastní stránky, ale vazby ke zdroji dat nabízí větší flexibilitu.</span><span class="sxs-lookup"><span data-stu-id="de9ca-111">Panels are usually declared within the page itself, but binding to a data source offers more flexibility.</span></span>

## <a name="steps"></a><span data-ttu-id="de9ca-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="de9ca-112">Steps</span></span>

<span data-ttu-id="de9ca-113">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="de9ca-113">First of all, a data source is required.</span></span> <span data-ttu-id="de9ca-114">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="de9ca-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="de9ca-115">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="de9ca-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="de9ca-116">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="de9ca-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="de9ca-117">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="de9ca-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="de9ca-118">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="de9ca-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="de9ca-119">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="de9ca-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="de9ca-120">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="de9ca-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample1.aspx)]

<span data-ttu-id="de9ca-121">Pak přidejte zdroje dat na stránku.</span><span class="sxs-lookup"><span data-stu-id="de9ca-121">Then, add a data source to the page.</span></span> <span data-ttu-id="de9ca-122">Chcete-li použít omezené množství dat, jsme vybrat pouze prvních pět položek v tabulce dodavatele databázi AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="de9ca-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="de9ca-123">Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="de9ca-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="de9ca-124">Následující kód ukazuje správnou syntaxi:</span><span class="sxs-lookup"><span data-stu-id="de9ca-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample2.aspx)]

<span data-ttu-id="de9ca-125">Pamatujte název zdroje dat (ID).</span><span class="sxs-lookup"><span data-stu-id="de9ca-125">Remember the name (ID) of the data source.</span></span> <span data-ttu-id="de9ca-126">Tato velmi identifikace pak použije v `DataSourceID` vlastností ovládacího prvku typu Accordion:</span><span class="sxs-lookup"><span data-stu-id="de9ca-126">This very identification must then be used in the `DataSourceID` property of the Accordion control:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample3.aspx)]

<span data-ttu-id="de9ca-127">V ovládacím prvku typu Accordion, můžete zadat šablony pro různé části ovládacího prvku, včetně hlavičky (`<HeaderTemplate>`) a obsah (`<ContentTemplate>`).</span><span class="sxs-lookup"><span data-stu-id="de9ca-127">Within the Accordion control, you can provide templates for various parts of the control, including the header (`<HeaderTemplate>`) and the content (`<ContentTemplate>`).</span></span> <span data-ttu-id="de9ca-128">V rámci těchto prvků právě výstupní data z dat zdroje, pomocí `DataBinder.Eval()` metoda:</span><span class="sxs-lookup"><span data-stu-id="de9ca-128">Within these elements, just output the data from the data source, using the `DataBinder.Eval()` method:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample4.aspx)]

<span data-ttu-id="de9ca-129">Při načtení stránky zdroje dat musí být vázána na accordion s tímto kódem na straně serveru:</span><span class="sxs-lookup"><span data-stu-id="de9ca-129">When the page is loaded, the data source must be bound to the accordion with this server-side code:</span></span>

[!code-aspx[Main](databinding-to-an-accordion-cs/samples/sample5.aspx)]

<span data-ttu-id="de9ca-130">Dokončete tuto ukázku, musíte definovat dvě třídy CSS, které se odkazuje v ovládacím prvku typu Accordion (v jeho vlastnosti `HeaderCssClass` a `ContentCssClass`).</span><span class="sxs-lookup"><span data-stu-id="de9ca-130">To conclude this sample, you need to define the two CSS classes that are referenced in the Accordion control (in its properties `HeaderCssClass` and `ContentCssClass`).</span></span> <span data-ttu-id="de9ca-131">Uveďte následující kód do `<head>` části stránky:</span><span class="sxs-lookup"><span data-stu-id="de9ca-131">Put the following markup in the `<head>` section of the page:</span></span>

[!code-css[Main](databinding-to-an-accordion-cs/samples/sample6.css)]


<span data-ttu-id="de9ca-132">[![Data v prvku typu accordion pochází přímo ze zdroje dat.](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="de9ca-132">[![The data in the accordion comes directly from the data source](databinding-to-an-accordion-cs/_static/image2.png)](databinding-to-an-accordion-cs/_static/image1.png)</span></span>

<span data-ttu-id="de9ca-133">Data v prvku typu accordion pochází přímo ze zdroje dat ([Kliknutím zobrazit obrázek v plné velikosti](databinding-to-an-accordion-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="de9ca-133">The data in the accordion comes directly from the data source ([Click to view full-size image](databinding-to-an-accordion-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="de9ca-134">Další</span><span class="sxs-lookup"><span data-stu-id="de9ca-134">Next</span></span>](dynamically-adding-an-accordion-pane-cs.md)
