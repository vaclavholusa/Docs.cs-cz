---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
title: "Použití ConfirmButton v prvku Repeater (C#) | Microsoft Docs"
author: wenz
description: "Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně). Pouze v případě Ano je..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a973ed3e-400c-4925-ace2-0b086b479301
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 12fd3e4e58e6a73720ade38372caff496f7f2c97
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-a-confirmbutton-in-a-repeater-c"></a><span data-ttu-id="d4712-104">Použití ConfirmButton v prvku Repeater (C#)</span><span class="sxs-lookup"><span data-stu-id="d4712-104">Using a ConfirmButton In a Repeater (C#)</span></span>
====================
<span data-ttu-id="d4712-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d4712-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d4712-106">[Stáhněte si kód](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d4712-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1CS.pdf)</span></span>

> <span data-ttu-id="d4712-107">Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně).</span><span class="sxs-lookup"><span data-stu-id="d4712-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="d4712-108">Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena.</span><span class="sxs-lookup"><span data-stu-id="d4712-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="d4712-109">To je možné v prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="d4712-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="d4712-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="d4712-110">Overview</span></span>

<span data-ttu-id="d4712-111">Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně).</span><span class="sxs-lookup"><span data-stu-id="d4712-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="d4712-112">Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena.</span><span class="sxs-lookup"><span data-stu-id="d4712-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="d4712-113">To je možné v prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="d4712-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="d4712-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="d4712-114">Steps</span></span>

<span data-ttu-id="d4712-115">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="d4712-115">First of all, a data source is required.</span></span> <span data-ttu-id="d4712-116">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="d4712-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="d4712-117">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="d4712-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="d4712-118">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="d4712-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="d4712-119">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="d4712-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="d4712-120">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="d4712-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="d4712-121">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="d4712-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="d4712-122">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="d4712-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample1.aspx)]

<span data-ttu-id="d4712-123">Potom zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="d4712-123">Then, a data source is required.</span></span> <span data-ttu-id="d4712-124">Z důvodu zjednodušení jsou načteny pouze prvních pět položky v tabulce Dodavatelé AdventureWorks'.</span><span class="sxs-lookup"><span data-stu-id="d4712-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="d4712-125">Všimněte si, že při použití průvodce Visual Studio k vytvoření zdroje dat, název tabulky (`Vendors`) není aktuálně předponu správně `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="d4712-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="d4712-126">Následující kód je tu správnou:</span><span class="sxs-lookup"><span data-stu-id="d4712-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample2.aspx)]

<span data-ttu-id="d4712-127">Tento zdroj dat lze potom použít v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="d4712-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="d4712-128">Obvyklým způsobem `DataBinder.Eval()` metoda načítá data z datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="d4712-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="d4712-129">`ConfirmButtonExtender` Ovládací prvek musí být umístěn pak v rámci `<ItemTemplate>` části opakovače, které se zobrazí se pro každou položku ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="d4712-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-cs/samples/sample3.aspx)]


<span data-ttu-id="d4712-130">[![Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat.](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d4712-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-cs/_static/image2.png)](using-a-confirmbutton-in-a-repeater-cs/_static/image1.png)</span></span>

<span data-ttu-id="d4712-131">U každé položky ze zdroje dat se zobrazí tlačítko Potvrdit ([Kliknutím zobrazit obrázek v plné velikosti](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d4712-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d4712-132">Další</span><span class="sxs-lookup"><span data-stu-id="d4712-132">Next</span></span>](using-a-confirmbutton-in-a-repeater-vb.md)
