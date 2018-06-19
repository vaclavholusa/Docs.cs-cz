---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Pomocí TextBoxWatermark s ovládacími prvky ověření (C#) | Microsoft Docs
author: wenz
description: TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole. Když uživatel klikne do pole, je možné...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: b5cc7974f3444b54770cba54b991aab7b103f753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868877"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a><span data-ttu-id="a5fab-104">Pomocí TextBoxWatermark s ovládacími prvky ověření (C#)</span><span class="sxs-lookup"><span data-stu-id="a5fab-104">Using TextBoxWatermark With Validation Controls (C#)</span></span>
====================
<span data-ttu-id="a5fab-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="a5fab-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="a5fab-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="a5fab-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)</span></span>

> <span data-ttu-id="a5fab-107">TextBoxWatermark ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole.</span><span class="sxs-lookup"><span data-stu-id="a5fab-107">The TextBoxWatermark control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="a5fab-108">Když uživatel klikne do pole, je vyprázdnit.</span><span class="sxs-lookup"><span data-stu-id="a5fab-108">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="a5fab-109">Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu.</span><span class="sxs-lookup"><span data-stu-id="a5fab-109">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="a5fab-110">To může dojít ke konfliktu s ovládacími prvky ověřování ASP.NET na stejné stránce, ale mohou být tyto problémy překonat.</span><span class="sxs-lookup"><span data-stu-id="a5fab-110">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>


## <a name="overview"></a><span data-ttu-id="a5fab-111">Přehled</span><span class="sxs-lookup"><span data-stu-id="a5fab-111">Overview</span></span>

<span data-ttu-id="a5fab-112">`TextBoxWatermark` Ovládacího prvku Toolkitu AJAX rozšiřuje textové pole tak, aby text se zobrazí v rámci pole.</span><span class="sxs-lookup"><span data-stu-id="a5fab-112">The `TextBoxWatermark` control in the AJAX Control Toolkit extends a text box so that a text is displayed within the box.</span></span> <span data-ttu-id="a5fab-113">Když uživatel klikne do pole, je vyprázdnit.</span><span class="sxs-lookup"><span data-stu-id="a5fab-113">When a user clicks into the box, it is emptied.</span></span> <span data-ttu-id="a5fab-114">Pokud uživatel ponechá bez nutnosti zadávat text do pole, zobrazí se předem vyplněných text znovu.</span><span class="sxs-lookup"><span data-stu-id="a5fab-114">If the user leaves the box without entering text, the prefilled text reappears.</span></span> <span data-ttu-id="a5fab-115">To může dojít ke konfliktu s ovládacími prvky ověřování ASP.NET na stejné stránce, ale mohou být tyto problémy překonat.</span><span class="sxs-lookup"><span data-stu-id="a5fab-115">This may collide with ASP.NET Validation Controls on the same page, but these issues may be overcome.</span></span>

## <a name="steps"></a><span data-ttu-id="a5fab-116">Kroky</span><span class="sxs-lookup"><span data-stu-id="a5fab-116">Steps</span></span>

<span data-ttu-id="a5fab-117">Základní nastavení vzorku je následující: `TextBox` ovládací prvek je vodoznakem pomocí `TextBoxWatermarkExtender` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="a5fab-117">The basic setup of the sample is the following: a `TextBox` control is watermarked using a `TextBoxWatermarkExtender` control.</span></span> <span data-ttu-id="a5fab-118">Tlačítko zpětné volání se aktivuje a bude možné později použít k aktivaci ověřovací ovládací prvky na stránce.</span><span class="sxs-lookup"><span data-stu-id="a5fab-118">A button triggers a postback and will later be used to trigger the validation controls on the page.</span></span> <span data-ttu-id="a5fab-119">Navíc `ScriptManager` řízení se vyžaduje k chybě při inicializaci prvku ASP.NET AJAX:</span><span class="sxs-lookup"><span data-stu-id="a5fab-119">Also, a `ScriptManager` control is required to initialize ASP.NET AJAX:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="a5fab-120">Nyní přidejte `RequiredFieldValidator` ovládací prvek, který kontroluje, zda je text v poli při odeslání formuláře.</span><span class="sxs-lookup"><span data-stu-id="a5fab-120">Now add a `RequiredFieldValidator` control that checks whether there is text in the field when the form is submitted.</span></span> <span data-ttu-id="a5fab-121">`InitialValue` Validátoru musí být nastavena na stejnou hodnotu, která se používá v `TextBoxWatermarkExtender` ovládacího prvku: po odeslání formuláře hodnota beze změny textové pole je hodnota vodoznaku v něm:</span><span class="sxs-lookup"><span data-stu-id="a5fab-121">The `InitialValue` property of the validator must be set to the same value that is used in the `TextBoxWatermarkExtender` control: When the form is submitted, the value of an unchanged textbox is the watermark value within it:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="a5fab-122">Ale je jedním z problémů s tímto přístupem: Jestliže klient zakáže JavaScript, není pole text předem s vodoznakového textu, proto `RequiredFieldValidator` neaktivuje chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="a5fab-122">However there is one problem with this approach: If the client disables JavaScript, the text field is not prefilled with the watermark text, therefore the `RequiredFieldValidator` does not trigger an error message.</span></span> <span data-ttu-id="a5fab-123">Proto druhý `RequiredFieldValidator` ovládací prvek je vyžadován, která vyhledává prázdné textové pole (vynechání `InitialValue` atribut).</span><span class="sxs-lookup"><span data-stu-id="a5fab-123">Therefore, a second `RequiredFieldValidator` control is required which checks for an empty text box (omitting the `InitialValue` attribute).</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="a5fab-124">Vzhledem k tomu použít i validátory `Display` = `"Dynamic"`, koncový uživatel nemůže odlišit od vzhled, které ze dvou validátory byla aktivována; místo toho to vypadá, došlo jenom jeden z nich.</span><span class="sxs-lookup"><span data-stu-id="a5fab-124">Since both validators use `Display`=`"Dynamic"`, the end user cannot distinguish from the visual appearance which of the two validators was fired; instead, it looks like there was only one of them.</span></span>

<span data-ttu-id="a5fab-125">Nakonec přidejte některé kódu na straně serveru k vypsání text v poli, pokud žádné program pro ověření objeví chybová zpráva:</span><span class="sxs-lookup"><span data-stu-id="a5fab-125">Finally, add some server-side code to output the text in the field if no validator issued an error message:</span></span>

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


<span data-ttu-id="a5fab-126">[![Validátor complains, že neexistuje žádný text v poli](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a5fab-126">[![The validator complains that there is no text in the field](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="a5fab-127">Validátor complains, že neexistuje žádný text v poli ([Kliknutím zobrazit obrázek v plné velikosti](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="a5fab-127">The validator complains that there is no text in the field ([Click to view full-size image](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a5fab-128">[Předchozí](using-textboxwatermark-in-a-formview-cs.md)
> [další](using-textboxwatermark-in-a-formview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a5fab-128">[Previous](using-textboxwatermark-in-a-formview-cs.md)
[Next](using-textboxwatermark-in-a-formview-vb.md)</span></span>
