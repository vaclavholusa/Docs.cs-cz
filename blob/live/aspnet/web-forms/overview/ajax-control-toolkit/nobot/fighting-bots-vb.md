---
uid: web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
title: "Požárů robotů (VB) | Microsoft Docs"
author: wenz
description: "Automatizované robotů sádra weblogů a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele. NoBot ovládacího prvku ASP.NET AJAX Con..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e9803150-452d-4521-97e3-d75d5599383c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/nobot/fighting-bots-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b786fd8605c7521a4aae8e49ca236363a71b572
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="fighting-bots-vb"></a><span data-ttu-id="50b7c-104">Požárů robotů (VB)</span><span class="sxs-lookup"><span data-stu-id="50b7c-104">Fighting Bots (VB)</span></span>
====================
<span data-ttu-id="50b7c-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="50b7c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="50b7c-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="50b7c-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/NoBot0.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/nobot0VB.pdf)</span></span>

> <span data-ttu-id="50b7c-107">Automatizované robotů sádra weblogů a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="50b7c-107">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="50b7c-108">NoBot ovládacího prvku ASP.NET AJAX Control Toolkit může pomoci boji s těmito robotů.</span><span class="sxs-lookup"><span data-stu-id="50b7c-108">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>


## <a name="overview"></a><span data-ttu-id="50b7c-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="50b7c-109">Overview</span></span>

<span data-ttu-id="50b7c-110">Automatizované robotů sádra weblogů a další weby s nevyžádanou poštou, odesílání formulářů komentář bez nutnosti zásahu uživatele.</span><span class="sxs-lookup"><span data-stu-id="50b7c-110">Automated bots plaster weblogs and other websites with spam, submitting comment forms without any user interaction.</span></span> <span data-ttu-id="50b7c-111">NoBot ovládacího prvku ASP.NET AJAX Control Toolkit může pomoci boji s těmito robotů.</span><span class="sxs-lookup"><span data-stu-id="50b7c-111">The NoBot control in the ASP.NET AJAX Control Toolkit can help fight those bots.</span></span>

## <a name="steps"></a><span data-ttu-id="50b7c-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="50b7c-112">Steps</span></span>

<span data-ttu-id="50b7c-113">Do jaké míry robotů jeden běžný postup je použití CAPTCHAs zcela automatizovat veřejné Turing testu s oznámením, počítače a od sebe člověka.</span><span class="sxs-lookup"><span data-stu-id="50b7c-113">One common approach to defeat bots is to use CAPTCHAs Completely Automated Public Turing test to tell Computers and Humans Apart.</span></span> <span data-ttu-id="50b7c-114">Turing test byl původně testu kde někdo třeba rozhodnout, zda je komunikace partnera lidské nebo počítač.</span><span class="sxs-lookup"><span data-stu-id="50b7c-114">A Turing test was originally a test where someone needed to decide whether a communication partner is a human or a machine.</span></span> <span data-ttu-id="50b7c-115">Na webu test CAPTCHA obvykle obsahuje bitovou kopii s některé zkreslený písmena na něm.</span><span class="sxs-lookup"><span data-stu-id="50b7c-115">In the web, a CAPTCHA usually consists of an image with some distorted letters on it.</span></span> <span data-ttu-id="50b7c-116">Cílem je, že pouze lidské může číst písmena na bitovou kopii, zatímco algoritmy rozpoznávání znaků se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="50b7c-116">The idea is that only a human can read the letters on the image, whereas OCR algorithms will fail.</span></span>

<span data-ttu-id="50b7c-117">Existuje několik výhod a nevýhod tohoto přístupu, avšak informace o to jsou nad rámec tohoto návodu.</span><span class="sxs-lookup"><span data-stu-id="50b7c-117">There are several advantages and disadvantages to this approach, but a discussion of this is beyond the scope of this tutorial.</span></span> <span data-ttu-id="50b7c-118">Ale ovládací prvek v sadě nástrojů ovládacího prvku ASP.NET AJAX, která poskytuje podobný postup: `NoBot`.</span><span class="sxs-lookup"><span data-stu-id="50b7c-118">There is however a control in the ASP.NET AJAX Control Toolkit which provides a similar approach: `NoBot`.</span></span> <span data-ttu-id="50b7c-119">Je snazší překonat než test CAPTCHA, ale je velmi snadno použitelný a tarify velmi dobře na webech jako blogy, kde považuje úspěšné, pokud většinu spamu pokusy jsou nepotlačí, což `NoBot` ovládacího prvku provést.</span><span class="sxs-lookup"><span data-stu-id="50b7c-119">It is easier to overcome than a CAPTCHA, but is very easy to use and fares extremely well on websites like blogs where it is considered a success if most spam attempts are defeated, which the `NoBot` control can do.</span></span>

<span data-ttu-id="50b7c-120">`NoBot`zpětné volání aktuálního webové formuláře ASP.NET zachytí při splnění alespoň jeden z těchto podmínek:</span><span class="sxs-lookup"><span data-stu-id="50b7c-120">`NoBot` intercepts the postback of the current ASP.NET web form if at least one of these conditions is met:</span></span>

- <span data-ttu-id="50b7c-121">V prohlížeči se nepodaří vyřešit stavebnice JavaScript (například při deaktivaci JavaScript)</span><span class="sxs-lookup"><span data-stu-id="50b7c-121">The browser fails to solve a JavaScript puzzle (for instance when JavaScript is deactivated)</span></span>
- <span data-ttu-id="50b7c-122">Odeslání formuláře pro rychlé uživatele</span><span class="sxs-lookup"><span data-stu-id="50b7c-122">The user submitted the form to fast</span></span>
- <span data-ttu-id="50b7c-123">IP adresa klienta odeslání formuláře příliš často v určitou dobu.</span><span class="sxs-lookup"><span data-stu-id="50b7c-123">The client IP address submitted the form too often in a certain period of time.</span></span>

<span data-ttu-id="50b7c-124">Chcete-li zkontrolovat pro tyto podmínky `NoBot` ovládacího prvku vyžaduje tyto atributy (všechny z nich volitelný):</span><span class="sxs-lookup"><span data-stu-id="50b7c-124">In order to check for these conditions, the `NoBot` control requires these attributes (all of them optional):</span></span>

- <span data-ttu-id="50b7c-125">`ResponseMinimumDelaySeconds`minimální množství sekund mezi zpětná vystavení</span><span class="sxs-lookup"><span data-stu-id="50b7c-125">`ResponseMinimumDelaySeconds` minimum amount of seconds between postbacks</span></span>
- <span data-ttu-id="50b7c-126">`CutoffWindowSeconds`Délka Časový interval, ve kterém jsou postback z jedna IP adresa míry</span><span class="sxs-lookup"><span data-stu-id="50b7c-126">`CutoffWindowSeconds` length of time interval in which postbacks from one IP are measures</span></span>
- <span data-ttu-id="50b7c-127">`CutoffMaximumInstances`maximální množství za časový interval v sekundách</span><span class="sxs-lookup"><span data-stu-id="50b7c-127">`CutoffMaximumInstances` maximum amount of seconds per time interval</span></span>

<span data-ttu-id="50b7c-128">Následující kód požadavky tohoto nejméně dvou sekund uplynout mezi postback a že jsou pouze pět postback nebo méně v intervalu 30 sekund:</span><span class="sxs-lookup"><span data-stu-id="50b7c-128">The following markup demands that at least two seconds elapse between postbacks and that there are only five postbacks or less within a 30 seconds interval:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample1.aspx)]

<span data-ttu-id="50b7c-129">Potom obvyklým nezapomeňte zahrnout `ScriptManager` na stránce tak, aby se načíst knihovnu ASP.NET AJAX a dá se použít Toolkitu:</span><span class="sxs-lookup"><span data-stu-id="50b7c-129">Then as usual make sure to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample2.aspx)]

<span data-ttu-id="50b7c-130">Protože většina kontrol `NoBot` provádí dojít na straně serveru, je potřeba zkontrolovat výsledek těchto ověření.</span><span class="sxs-lookup"><span data-stu-id="50b7c-130">Since most of the checks `NoBot` is doing occur on the server side, you need to check the result of these validations.</span></span> <span data-ttu-id="50b7c-131">To lze provést pomocí volání `NoBot`na `IsValid()` metoda.</span><span class="sxs-lookup"><span data-stu-id="50b7c-131">This can be done by calling `NoBot`'s `IsValid()` method.</span></span> <span data-ttu-id="50b7c-132">Má jeden argument (jako `out` parametr /`ByRef` parametr) je typu `NoBotState`.</span><span class="sxs-lookup"><span data-stu-id="50b7c-132">It has one argument (as an `out` parameter/`ByRef` parameter) which is of type `NoBotState`.</span></span> <span data-ttu-id="50b7c-133">Jeho řetězcovou reprezentaci obsahuje z důvodu selhání kontroly a `Valid` jinak.</span><span class="sxs-lookup"><span data-stu-id="50b7c-133">Its string representation contains the reason when the check fails and `Valid` otherwise.</span></span> <span data-ttu-id="50b7c-134">Následující kód výstupy zprávu podle `NoBot`je způsobit:</span><span class="sxs-lookup"><span data-stu-id="50b7c-134">The following code outputs a message according to `NoBot`'s result:</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample3.aspx)]

<span data-ttu-id="50b7c-135">Nakonec budete potřebovat formuláře pro odeslání a elementu label na výstup zprávu, a dokončení!</span><span class="sxs-lookup"><span data-stu-id="50b7c-135">Finally, you need a form to submit and a label element to output the message, and you are done!</span></span>

[!code-aspx[Main](fighting-bots-vb/samples/sample4.aspx)]

<span data-ttu-id="50b7c-136">Při spuštění tohoto skriptu a deaktivovat JavaScript nebo odesláním formuláře v rámci prvních dvou sekund nebo odesláním formuláře sedm časy během 30 sekund, zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="50b7c-136">When you run this script and deactivate JavaScript or submit the form within the first two seconds or submit the form seven times within thirty seconds, you will get an error message.</span></span> <span data-ttu-id="50b7c-137">Ale pomocí tohoto ovládacího prvku dobře, vzhledem k tomu, že pouze přibližně 90 95 % uživatelé mají JavaScript aktivovat, proto se nezdaří 5 až 10 % uživatelů `NoBot`je testování.</span><span class="sxs-lookup"><span data-stu-id="50b7c-137">However use this control wisely, since only about 90-95% of users have JavaScript activated, therefore 5-10% of users will fail `NoBot`'s test.</span></span>


<span data-ttu-id="50b7c-138">[![Tato chybová zpráva by mohla být způsobena robotu](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="50b7c-138">[![This error message could have been caused by a bot](fighting-bots-vb/_static/image2.png)](fighting-bots-vb/_static/image1.png)</span></span>

<span data-ttu-id="50b7c-139">Tato chybová zpráva by mohla být způsobena robotu ([Kliknutím zobrazit obrázek v plné velikosti](fighting-bots-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="50b7c-139">This error message could have been caused by a bot ([Click to view full-size image](fighting-bots-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="50b7c-140">Předchozí</span><span class="sxs-lookup"><span data-stu-id="50b7c-140">Previous</span></span>](fighting-bots-cs.md)
