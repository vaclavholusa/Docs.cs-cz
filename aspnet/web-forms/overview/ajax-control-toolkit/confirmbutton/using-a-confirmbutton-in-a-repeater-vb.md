---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: Použití ConfirmButton v prvku Repeater (VB) | Microsoft Docs
author: wenz
description: Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně). Pouze v případě Ano je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869449"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="e24f1-104">Použití ConfirmButton v prvku Repeater (VB)</span><span class="sxs-lookup"><span data-stu-id="e24f1-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="e24f1-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e24f1-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e24f1-106">[Stáhněte si kód](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e24f1-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="e24f1-107">Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně).</span><span class="sxs-lookup"><span data-stu-id="e24f1-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="e24f1-108">Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena.</span><span class="sxs-lookup"><span data-stu-id="e24f1-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="e24f1-109">To je možné v prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="e24f1-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="e24f1-110">Přehled</span><span class="sxs-lookup"><span data-stu-id="e24f1-110">Overview</span></span>

<span data-ttu-id="e24f1-111">Rozšiřujícího objektu ConfirmButton v Toolkitu AJAX vytvoří Ano žádné místní, když uživatel klikne na tlačítko LinkButton řízení (včetně).</span><span class="sxs-lookup"><span data-stu-id="e24f1-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="e24f1-112">Pouze Ano po kliknutí na tlačítka provedení akce, jinak zrušena.</span><span class="sxs-lookup"><span data-stu-id="e24f1-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="e24f1-113">To je možné v prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="e24f1-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="e24f1-114">Kroky</span><span class="sxs-lookup"><span data-stu-id="e24f1-114">Steps</span></span>

<span data-ttu-id="e24f1-115">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="e24f1-115">First of all, a data source is required.</span></span> <span data-ttu-id="e24f1-116">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="e24f1-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="e24f1-117">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="e24f1-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="e24f1-118">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="e24f1-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="e24f1-119">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="e24f1-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="e24f1-120">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="e24f1-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="e24f1-121">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="e24f1-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="e24f1-122">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="e24f1-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="e24f1-123">Potom zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="e24f1-123">Then, a data source is required.</span></span> <span data-ttu-id="e24f1-124">Z důvodu zjednodušení jsou načteny pouze prvních pět položky v tabulce Dodavatelé AdventureWorks'.</span><span class="sxs-lookup"><span data-stu-id="e24f1-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="e24f1-125">Všimněte si, že při použití průvodce Visual Studio k vytvoření zdroje dat, název tabulky (`Vendors`) není aktuálně předponu správně `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="e24f1-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="e24f1-126">Následující kód je tu správnou:</span><span class="sxs-lookup"><span data-stu-id="e24f1-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="e24f1-127">Tento zdroj dat lze potom použít v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="e24f1-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="e24f1-128">Obvyklým způsobem `DataBinder.Eval()` metoda načítá data z datového zdroje.</span><span class="sxs-lookup"><span data-stu-id="e24f1-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="e24f1-129">`ConfirmButtonExtender` Ovládací prvek musí být umístěn pak v rámci `<ItemTemplate>` části opakovače, které se zobrazí se pro každou položku ve zdroji dat.</span><span class="sxs-lookup"><span data-stu-id="e24f1-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="e24f1-130">[![Tlačítko potvrzení se zobrazí vedle každé položky ze zdroje dat.](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e24f1-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="e24f1-131">U každé položky ze zdroje dat se zobrazí tlačítko Potvrdit ([Kliknutím zobrazit obrázek v plné velikosti](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e24f1-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e24f1-132">Předchozí</span><span class="sxs-lookup"><span data-stu-id="e24f1-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
