---
uid: whitepapers/request-validation
title: "Žádosti o ověření - prevence útoků skriptu | Microsoft Docs"
author: rick-anderson
description: "Tento dokument popisuje funkci žádosti o ověření technologie ASP.NET ve výchozím nastavení, kde aplikace je zabránit v zpracování nekódovaného HTML obsahu submitt..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="bc628-103">Žádosti o ověření - prevence útoků skriptu</span><span class="sxs-lookup"><span data-stu-id="bc628-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="bc628-104">Tento dokument popisuje funkci žádosti o ověření technologie ASP.NET ve výchozím nastavení, kde aplikace je zabránit v zpracování nekódovaného obsah HTML odeslána na server.</span><span class="sxs-lookup"><span data-stu-id="bc628-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="bc628-105">Tato žádost o ověření funkce lze zakázat, když aplikace byl navržen tak, aby bezpečně zpracování dat ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="bc628-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="bc628-106">Platí pro technologii ASP.NET 1.1 a ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="bc628-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="bc628-107">Ověření žádosti, funkce technologie ASP.NET od verze 1.1, zabrání serveru přijetí obsahu obsahující bez kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="bc628-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="bc628-108">Tato funkce je určena k zabránění některými útoky vložení skriptu, při kterém kód skriptu klienta nebo HTML můžete nechtěně odeslat na server, ukládat a poté jsou předloženy ostatním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="bc628-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="bc628-109">Stále důrazně doporučujeme, aby ověření všech vstupních dat a jeho v případě nutnosti kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="bc628-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="bc628-110">Například můžete vytvořit webovou stránku, která požaduje e-mailovou adresu uživatele a potom úložiště, která e-mailovou adresu v databázi.</span><span class="sxs-lookup"><span data-stu-id="bc628-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="bc628-111">Pokud uživatel zadá &lt;skriptu&gt;výstrahy ("hello ze skriptu")&lt;/SCRIPT&gt; místo platnou e-mailovou adresu, když se tato data, tento skript můžete provést Pokud obsah nebyl správně kódovaný.</span><span class="sxs-lookup"><span data-stu-id="bc628-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="bc628-112">Funkce ověření žádosti technologie ASP.NET to zabrání situaci.</span><span class="sxs-lookup"><span data-stu-id="bc628-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="bc628-113">Proč se tato funkce je užitečná</span><span class="sxs-lookup"><span data-stu-id="bc628-113">Why this feature is useful</span></span>

<span data-ttu-id="bc628-114">Mnoho webů nejsou vědět, že jsou otevřené pro útok prostřednictvím injektáže jednoduché skriptu.</span><span class="sxs-lookup"><span data-stu-id="bc628-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="bc628-115">Zda účel útoků těchto je autorskou webu zobrazením HTML nebo potenciálně spuštění skriptu klienta k lokalitě se hacker přesměrovat uživatele, jsou prostřednictvím injektáže skriptu k problému, který se musí soupeří vývojářům webů.</span><span class="sxs-lookup"><span data-stu-id="bc628-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="bc628-116">Útok prostřednictvím injektáže skriptu jsou zájmem všechny vývojářům webů, zda používají ASP.NET, ASP ani jiné technologie vývoj pro web.</span><span class="sxs-lookup"><span data-stu-id="bc628-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="bc628-117">Funkce ASP.NET žádost o ověření proaktivně zabraňuje tyto útoky tím, že nepovolí nekódovaného obsah HTML, které mají být zpracovány serverem, pokud vývojář se rozhodne povolit tento obsah.</span><span class="sxs-lookup"><span data-stu-id="bc628-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="bc628-118">Co můžete očekávat: chybová stránka</span><span class="sxs-lookup"><span data-stu-id="bc628-118">What to expect: Error Page</span></span>

<span data-ttu-id="bc628-119">Následující kopie obrazovky zobrazuje ukázkový kód ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="bc628-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="bc628-120">Používá tento kód výsledky v jednoduchá stránka, která umožňuje zadat text do textového pole, klikněte na tlačítko a zobrazit text v ovládacím prvku popisek:</span><span class="sxs-lookup"><span data-stu-id="bc628-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="bc628-121">Ale byly JavaScript, jako například `<script>alert("hello!")</script>` zadali a odeslání by se nám získat výjimku:</span><span class="sxs-lookup"><span data-stu-id="bc628-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="bc628-122">Chybová zpráva uvádí, že 'potenciálně nebezpečná Request.Form byla zjištěna hodnota a poskytuje další podrobnosti v popisu, přesně co se stalo a jak můžete změnit chování.</span><span class="sxs-lookup"><span data-stu-id="bc628-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="bc628-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="bc628-123">For example:</span></span>

<span data-ttu-id="bc628-124">Ověření žádosti zjistil vstupní hodnoty potenciálně nebezpečná klienta a zpracování žádosti bylo přerušeno.</span><span class="sxs-lookup"><span data-stu-id="bc628-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="bc628-125">Tato hodnota může znamenat pokus o ohrožení zabezpečení vaší aplikace, jako je například útoku skriptování webů.</span><span class="sxs-lookup"><span data-stu-id="bc628-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="bc628-126">Zakážete ověření požadavku nastavením `validateRequest=false` v direktivě stránky nebo v konfiguračním oddílu.</span><span class="sxs-lookup"><span data-stu-id="bc628-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="bc628-127">Ale je důrazně doporučujeme, aby vaše aplikace explicitně zkontrolovala všechny vstupní hodnoty v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="bc628-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="bc628-128">Zakázání ověření požadavku na stránce.</span><span class="sxs-lookup"><span data-stu-id="bc628-128">Disabling request validation on a page</span></span>

<span data-ttu-id="bc628-129">Zakážete ověření požadavku na stránce, musíte nastavit `validateRequest` direktivy stránky k `false`:</span><span class="sxs-lookup"><span data-stu-id="bc628-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="bc628-130">Pokud je ověření žádosti je zakázáno, obsah lze odesílat na stránku. je zodpovědností vývojář stránky zajistit, že obsah je správně kódovaný nebo zpracovat.</span><span class="sxs-lookup"><span data-stu-id="bc628-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="bc628-131">Zakázání ověření žádosti pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="bc628-131">Disabling request validation for your application</span></span>

<span data-ttu-id="bc628-132">Zakázat ověření žádosti pro vaši aplikaci, musíte upravit nebo vytvořit soubor Web.config pro vaši aplikaci a nastavit atribut validateRequest `<pages />` části k `false`:</span><span class="sxs-lookup"><span data-stu-id="bc628-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="bc628-133">Pokud chcete zakázat ověření žádosti pro všechny aplikace na vašem serveru, můžete provést tato úprava k souboru Machine.config.</span><span class="sxs-lookup"><span data-stu-id="bc628-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="bc628-134">Pokud je ověření žádosti je zakázáno, obsah lze odesílat do aplikace; je zodpovědností vývojář aplikace zajistit, že obsah je správně kódovaný nebo zpracovat.</span><span class="sxs-lookup"><span data-stu-id="bc628-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="bc628-135">Následující kód je upravit tak, aby vypnout ověření žádosti:</span><span class="sxs-lookup"><span data-stu-id="bc628-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="bc628-136">Nyní, pokud byl zadán následující JavaScript do textového pole `<script>alert("hello!")</script>` výsledkem bude:</span><span class="sxs-lookup"><span data-stu-id="bc628-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="bc628-137">K tomu nedocházelo, s ověření žádosti vypnutý, budeme potřebovat do formátu HTML kódování obsahu.</span><span class="sxs-lookup"><span data-stu-id="bc628-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="bc628-138">Jak do formátu HTML kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="bc628-138">How to HTML encode content</span></span>

<span data-ttu-id="bc628-139">Pokud je zakázána ověření žádosti, je dobrým zvykem kódování HTML obsah, který se uloží pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="bc628-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="bc628-140">Kódování HTML bude automaticky nahradit libovolný '&lt;'nebo'&gt;' (spolu s několik dalších symboly) s jejich odpovídající HTML kódovaný reprezentace.</span><span class="sxs-lookup"><span data-stu-id="bc628-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="bc628-141">Například '&lt;"nahrazuje podle"&amp;lt;' a '&gt;"nahrazuje podle"&amp;gt;'.</span><span class="sxs-lookup"><span data-stu-id="bc628-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="bc628-142">Tyto speciální kódy v prohlížečích se používá k zobrazení '&lt;"nebo"&gt;se v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="bc628-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="bc628-143">Obsah lze snadno kódovaný jazykem HTML na serveru pomocí `Server.HtmlEncode(string)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bc628-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="bc628-144">Obsah může být také snadno HTML-dekódovat, který je vráceno zpět na standardní HTML pomocí `Server.HtmlDecode(string)` metoda.</span><span class="sxs-lookup"><span data-stu-id="bc628-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="bc628-145">Výsledkem je:</span><span class="sxs-lookup"><span data-stu-id="bc628-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
