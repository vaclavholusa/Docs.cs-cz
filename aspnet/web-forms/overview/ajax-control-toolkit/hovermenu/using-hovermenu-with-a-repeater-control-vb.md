---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: "HoverMenu pomocí ovládacího prvku opakovače (VB) | Microsoft Docs"
author: wenz
description: "HoverMenu ovládacího prvku AJAX Control Toolkit poskytuje jednoduché místní vliv: při umístění ukazatele myši nad elementem, se zobrazí místní okno na specifi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 77759afe00f341e15ee8e0f52a469d3b7c08ea89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="2615f-103">HoverMenu pomocí ovládacího prvku opakovače (VB)</span><span class="sxs-lookup"><span data-stu-id="2615f-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="2615f-104">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2615f-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2615f-105">[Stáhněte si kód](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2615f-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="2615f-106">HoverMenu ovládacího prvku AJAX Control Toolkit poskytuje jednoduché místní vliv: při umístění ukazatele myši nad elementem, se zobrazí místní okno na zadané pozici.</span><span class="sxs-lookup"><span data-stu-id="2615f-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="2615f-107">Je také možné použít tento ovládací prvek v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="2615f-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="2615f-108">Přehled</span><span class="sxs-lookup"><span data-stu-id="2615f-108">Overview</span></span>

<span data-ttu-id="2615f-109">`HoverMenu` Ovládacího prvku AJAX Control Toolkit poskytuje jednoduché místní vliv: při umístění ukazatele myši nad elementem, se zobrazí místní okno na zadané pozici.</span><span class="sxs-lookup"><span data-stu-id="2615f-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="2615f-110">Je také možné použít tento ovládací prvek v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="2615f-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="2615f-111">Kroky</span><span class="sxs-lookup"><span data-stu-id="2615f-111">Steps</span></span>

<span data-ttu-id="2615f-112">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="2615f-112">First of all, a data source is required.</span></span> <span data-ttu-id="2615f-113">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="2615f-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="2615f-114">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="2615f-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="2615f-115">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="2615f-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="2615f-116">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="2615f-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="2615f-117">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="2615f-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="2615f-118">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="2615f-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="2615f-119">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="2615f-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2615f-120">Pak přidejte zdroje dat na stránku.</span><span class="sxs-lookup"><span data-stu-id="2615f-120">Then, add a data source to the page.</span></span> <span data-ttu-id="2615f-121">Chcete-li použít omezené množství dat, jsme vybrat pouze prvních pět položek v tabulce dodavatele databázi AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="2615f-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="2615f-122">Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="2615f-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="2615f-123">Následující kód ukazuje správnou syntaxi:</span><span class="sxs-lookup"><span data-stu-id="2615f-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2615f-124">V dalším kroku přidejte panel, který slouží jako modální překryvné okno:</span><span class="sxs-lookup"><span data-stu-id="2615f-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="2615f-125">Nyní `HoverMenuExtender` dodává do play.</span><span class="sxs-lookup"><span data-stu-id="2615f-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="2615f-126">Tak, aby každý prvek ve zdroji dat získá svůj vlastní místní, musíte být v rámci opakovače umístit rozšiřujícího objektu `<ItemTemplate>` části.</span><span class="sxs-lookup"><span data-stu-id="2615f-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="2615f-127">Zde je kód:</span><span class="sxs-lookup"><span data-stu-id="2615f-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="2615f-128">Teď každá položka v datovém zdroji, zobrazí místní okno vpravo (`PopupPosition` atribut) po prodlevě 50 milisekund (`PopDelay` atributu).</span><span class="sxs-lookup"><span data-stu-id="2615f-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="2615f-129">[![V nabídce hover se zobrazí vedle každé položky v opakovači](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2615f-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2615f-130">V nabídce hover se zobrazí vedle každé položky v opakovači ([Kliknutím zobrazit obrázek v plné velikosti](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2615f-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="2615f-131">Předchozí</span><span class="sxs-lookup"><span data-stu-id="2615f-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
