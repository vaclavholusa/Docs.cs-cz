---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
title: "ModalPopup pomocí ovládacího prvku opakovače (C#) | Microsoft Docs"
author: wenz
description: "ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Je také možné použít tento sml..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d686d84a-1c58-492e-8a77-3eb5a0cfe918
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/using-modalpopup-with-a-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: d945b8decf4debbcbf415163e486e2bce8097701
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-modalpopup-with-a-repeater-control-c"></a><span data-ttu-id="57dfd-104">ModalPopup pomocí ovládacího prvku opakovače (C#)</span><span class="sxs-lookup"><span data-stu-id="57dfd-104">Using ModalPopup with a Repeater Control (C#)</span></span>
====================
<span data-ttu-id="57dfd-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="57dfd-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="57dfd-106">[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="57dfd-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup2CS.pdf)</span></span>

> <span data-ttu-id="57dfd-107">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="57dfd-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="57dfd-108">Je také možné použít tento ovládací prvek v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="57dfd-108">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="57dfd-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="57dfd-109">Overview</span></span>

<span data-ttu-id="57dfd-110">ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta.</span><span class="sxs-lookup"><span data-stu-id="57dfd-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="57dfd-111">Je také možné použít tento ovládací prvek v rámci prvku repeater.</span><span class="sxs-lookup"><span data-stu-id="57dfd-111">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="57dfd-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="57dfd-112">Steps</span></span>

<span data-ttu-id="57dfd-113">První řadě zdroj dat je vyžadován.</span><span class="sxs-lookup"><span data-stu-id="57dfd-113">First of all, a data source is required.</span></span> <span data-ttu-id="57dfd-114">Tato ukázka používá databázi AdventureWorks a Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="57dfd-114">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="57dfd-115">Databáze je volitelná součást instalaci sady Visual Studio (včetně express edition) a je také k dispozici jako samostatný soubor ke stažení v rámci [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="57dfd-115">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="57dfd-116">Databázi AdventureWorks je součástí sad SQL Server 2005 ukázky a ukázkové databáze (stáhnout na adrese [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="57dfd-116">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="57dfd-117">Nejjednodušší způsob, jak nastavit databáze je použití Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) a připojte `AdventureWorks.mdf` soubor databáze.</span><span class="sxs-lookup"><span data-stu-id="57dfd-117">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span> <span data-ttu-id="57dfd-118">Tato ukázka předpokládáme, že je název instance systému SQL Server 2005 Express Edition `SQLEXPRESS` a se nachází na stejném počítači jako webový server; toto je také výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="57dfd-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="57dfd-119">Pokud vaše instalace se liší, budete muset přizpůsobit informace o připojení pro databázi.</span><span class="sxs-lookup"><span data-stu-id="57dfd-119">If your setup differs, you have to adapt the connection information for the database.</span></span> <span data-ttu-id="57dfd-120">Chcete aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit kdekoli na stránce (ale uvnitř `<form>` element):</span><span class="sxs-lookup"><span data-stu-id="57dfd-120">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample1.aspx)]

<span data-ttu-id="57dfd-121">Pak přidejte zdroje dat na stránku.</span><span class="sxs-lookup"><span data-stu-id="57dfd-121">Then, add a data source to the page.</span></span> <span data-ttu-id="57dfd-122">Chcete-li použít omezené množství dat, jsme vybrat pouze prvních pět položek v tabulce dodavatele databázi AdventureWorks.</span><span class="sxs-lookup"><span data-stu-id="57dfd-122">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="57dfd-123">Pokud používáte pomocníkem pro Visual Studio k vytvoření zdroje dat, paměti, že chyby v aktuální verzi není předpony názvu tabulky (`Vendor`) s `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="57dfd-123">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="57dfd-124">Následující kód ukazuje správnou syntaxi:</span><span class="sxs-lookup"><span data-stu-id="57dfd-124">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample2.aspx)]

<span data-ttu-id="57dfd-125">Dál přidejte panel, který slouží jako modální místní.</span><span class="sxs-lookup"><span data-stu-id="57dfd-125">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="57dfd-126">Obsahuje `Button` řízení znova uzavřít překryvném okně:</span><span class="sxs-lookup"><span data-stu-id="57dfd-126">It contains a `Button` control to close the popup again:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample3.aspx)]

<span data-ttu-id="57dfd-127">Aby bylo možné místní pracovat v rámci opakovače, `ModalPopupExtender` řízení musíte umístit v rámci `<ItemTemplate>` části opakovače.</span><span class="sxs-lookup"><span data-stu-id="57dfd-127">In order to make the popup work within the repeater, the `ModalPopupExtender` control must be put within the `<ItemTemplate>` section of the repeater.</span></span> <span data-ttu-id="57dfd-128">Proto je panelu mimo opakovače, ale rozšiřujícího objektu je uvnitř.</span><span class="sxs-lookup"><span data-stu-id="57dfd-128">So the panel is outside the repeater, but the extender is inside.</span></span> <span data-ttu-id="57dfd-129">Zde je kód pro opakovače:</span><span class="sxs-lookup"><span data-stu-id="57dfd-129">Here is the markup for the repeater:</span></span>

[!code-aspx[Main](using-modalpopup-with-a-repeater-control-cs/samples/sample4.aspx)]

<span data-ttu-id="57dfd-130">Každá položka ve zdroji dat se potom zobrazí pomocí tlačítka vedle sebe, která aktivuje Modální místní.</span><span class="sxs-lookup"><span data-stu-id="57dfd-130">Then, every item in the data source is displayed with a button next to it that triggers the modal popup.</span></span>


<span data-ttu-id="57dfd-131">[![Modální překryvné okno se aktivuje pro každou položka zdroje dat](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="57dfd-131">[![The modal popup can be triggered for every data source entry](using-modalpopup-with-a-repeater-control-cs/_static/image2.png)](using-modalpopup-with-a-repeater-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="57dfd-132">Modální překryvné okno se aktivuje pro každou položka zdroje dat ([Kliknutím zobrazit obrázek v plné velikosti](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="57dfd-132">The modal popup can be triggered for every data source entry ([Click to view full-size image](using-modalpopup-with-a-repeater-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="57dfd-133">[Předchozí](launching-a-modal-popup-window-from-server-code-cs.md)
[další](handling-postbacks-from-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="57dfd-133">[Previous](launching-a-modal-popup-window-from-server-code-cs.md)
[Next](handling-postbacks-from-a-modalpopup-cs.md)</span></span>
