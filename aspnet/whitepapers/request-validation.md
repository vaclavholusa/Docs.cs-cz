---
uid: whitepapers/request-validation
title: Ověření požadavku – obrana před Skriptovými útoky | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje žádosti o ověření funkce technologie ASP.NET, pokud ve výchozím nastavení, aplikace nebude zpracování nekódovaného submitt obsahu HTML...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0dfbfcae70792c57d530fc5e6fb73f8f96ec6e02
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809742"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="2052e-103">Ověření požadavku – obrana před Skriptovými útoky</span><span class="sxs-lookup"><span data-stu-id="2052e-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="2052e-104">Tento dokument popisuje žádosti o ověření funkce technologie ASP.NET, pokud ve výchozím nastavení, aplikace nebude zpracování nešifrovaného obsahu HTML odeslat na server.</span><span class="sxs-lookup"><span data-stu-id="2052e-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="2052e-105">Tato žádost o ověření funkce lze zakázat, když aplikace byla navržena tak, aby bezpečně zpracovávat data ve formátu HTML.</span><span class="sxs-lookup"><span data-stu-id="2052e-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="2052e-106">Platí pro technologii ASP.NET 1.1 a ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="2052e-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="2052e-107">Ověření požadavku, která je součástí technologie ASP.NET od verze 1.1, zabrání serveru přijetí obsahu obsahující bez kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="2052e-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="2052e-108">Tato funkce je určena k zabránění útoků prostřednictvím injektáže skriptu kterým kód skriptu klienta nebo HTML lze neúmyslně odeslané na server, ukládat a předloží ostatním uživatelům.</span><span class="sxs-lookup"><span data-stu-id="2052e-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="2052e-109">Stále důrazně doporučujeme, že ověření všech vstupních dat a to v případě potřeby použije kódování HTML.</span><span class="sxs-lookup"><span data-stu-id="2052e-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="2052e-110">Například můžete vytvořit webovou stránku, která požádá uživatele e-mailovou adresu a potom úložiště, která e-mailovou adresu v databázi.</span><span class="sxs-lookup"><span data-stu-id="2052e-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="2052e-111">Pokud uživatel zadá &lt;skript&gt;upozornění ("hello ze skriptu")&lt;/SCRIPT&gt; místo platné e-mailové adresy, když se tato data, tento skript může provést Pokud obsah není správně kódovaný.</span><span class="sxs-lookup"><span data-stu-id="2052e-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="2052e-112">Žádosti o ověření funkce technologie ASP.NET to zabraňuje děje.</span><span class="sxs-lookup"><span data-stu-id="2052e-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="2052e-113">Proč se tato funkce je užitečná</span><span class="sxs-lookup"><span data-stu-id="2052e-113">Why this feature is useful</span></span>

<span data-ttu-id="2052e-114">Mnoho serverů nejsou vědomi, že jsou otevřeny útocích prostřednictvím injektáže jednoduchý skript.</span><span class="sxs-lookup"><span data-stu-id="2052e-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="2052e-115">Zda účelem tyto útoky je autorskou webu zobrazením HTML nebo potenciálně spusťte skript klienta přesměrovat uživatele na web se hacker, útoky prostřednictvím injektáže skriptu jsou problém, který vývojářům webů musí potýkat s.</span><span class="sxs-lookup"><span data-stu-id="2052e-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="2052e-116">Útoků prostřednictvím injektáže skriptu jsou obavy z všechny webové vývojáře, zda používají technologie ASP.NET, ASP nebo jiným vývojovým technologiím, web.</span><span class="sxs-lookup"><span data-stu-id="2052e-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="2052e-117">Žádosti o ověření funkce ASP.NET proaktivně zabraňuje těmto útokům díky neumožňuje nekódovaného obsah ve formátu HTML ke zpracování serverem, pokud vývojář rozhodne povolit tento obsah.</span><span class="sxs-lookup"><span data-stu-id="2052e-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="2052e-118">Co můžete očekávat: chybovou stránku</span><span class="sxs-lookup"><span data-stu-id="2052e-118">What to expect: Error Page</span></span>

<span data-ttu-id="2052e-119">Níže uvedeném snímku obrazovky ukazuje některé ukázkový kód ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="2052e-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="2052e-120">Tento kód výsledky používané jednoduchá stránka, která umožňuje zadejte nějaký text do textového pole, klikněte na tlačítko a zobrazení textu v ovládacím prvku popisek:</span><span class="sxs-lookup"><span data-stu-id="2052e-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="2052e-121">Však byly JavaScriptu, jako například `<script>alert("hello!")</script>` zadali a odeslání, dostali bychom výjimku:</span><span class="sxs-lookup"><span data-stu-id="2052e-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="2052e-122">Chybová zpráva uvádí, že "potenciálně nebezpečné Request.Form byla zjištěna hodnota' a poskytuje další informace naleznete v popisu přesně co se stalo a jak změnit chování.</span><span class="sxs-lookup"><span data-stu-id="2052e-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="2052e-123">Příklad:</span><span class="sxs-lookup"><span data-stu-id="2052e-123">For example:</span></span>

<span data-ttu-id="2052e-124">Ověření požadavku byla zjištěna potenciálně nebezpečná klienta vstupní hodnota a zpracování žádosti bylo přerušeno.</span><span class="sxs-lookup"><span data-stu-id="2052e-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="2052e-125">Tato hodnota může znamenat pokus o narušení zabezpečení aplikace, například s útoky skriptování napříč weby.</span><span class="sxs-lookup"><span data-stu-id="2052e-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="2052e-126">Zakážete ověření požadavku tak, že nastavíte `validateRequest=false` v direktivě stránky nebo v konfiguračním oddílu.</span><span class="sxs-lookup"><span data-stu-id="2052e-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="2052e-127">Je však důrazně doporučujeme, aby vaše aplikace explicitně zkontrolovala všechny vstupy v tomto případě.</span><span class="sxs-lookup"><span data-stu-id="2052e-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="2052e-128">Zakázání ověření požadavku na stránce.</span><span class="sxs-lookup"><span data-stu-id="2052e-128">Disabling request validation on a page</span></span>

<span data-ttu-id="2052e-129">Zakázání ověření požadavku na stránce, je nutné nastavit `validateRequest` atribut direktivy stránky k `false`:</span><span class="sxs-lookup"><span data-stu-id="2052e-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="2052e-130">Při žádosti o ověření je zakázané, můžete odeslat obsah na stránku. je odpovědnost vývojáře stránky je zajistit, že obsah je správně kódovaný nebo zpracovány.</span><span class="sxs-lookup"><span data-stu-id="2052e-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="2052e-131">Zakázání ověření žádosti pro vaši aplikaci</span><span class="sxs-lookup"><span data-stu-id="2052e-131">Disabling request validation for your application</span></span>

<span data-ttu-id="2052e-132">Zakázání ověření žádosti pro vaši aplikaci, musíte upravit nebo vytvořit soubor Web.config pro vaši aplikaci a nastavte atribut validateRequest `<pages />` části `false`:</span><span class="sxs-lookup"><span data-stu-id="2052e-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="2052e-133">Pokud chcete zakázat ověření žádosti pro všechny aplikace na serveru, můžete provést tuto změny souboru Machine.config.</span><span class="sxs-lookup"><span data-stu-id="2052e-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="2052e-134">Při žádosti o ověření je zakázané, můžete obsah odeslat do aplikace; je starosti vývojář aplikace, aby tento obsah je správně kódovaný nebo zpracovány.</span><span class="sxs-lookup"><span data-stu-id="2052e-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="2052e-135">Následující kód je upravit tak, aby vypnout ověření žádosti:</span><span class="sxs-lookup"><span data-stu-id="2052e-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="2052e-136">Když teď následující JavaScript byla zadán do textového pole `<script>alert("hello!")</script>` výsledek by byl:</span><span class="sxs-lookup"><span data-stu-id="2052e-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="2052e-137">K tomu nedocházelo, s ověřením požadavku vypnuté, budeme potřebovat do formátu HTML kódování obsahu.</span><span class="sxs-lookup"><span data-stu-id="2052e-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="2052e-138">Jak do formátu HTML kódování obsahu</span><span class="sxs-lookup"><span data-stu-id="2052e-138">How to HTML encode content</span></span>

<span data-ttu-id="2052e-139">Pokud jste zakázali žádost o ověření, je vhodné pro obsah s kódováním HTML, který se uloží pro budoucí použití.</span><span class="sxs-lookup"><span data-stu-id="2052e-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="2052e-140">Kódování HTML se automaticky nahradit libovolný "&lt;'nebo'&gt;" (společně s několika dalších symbolů) s jejich odpovídající kód HTML kódovaný reprezentace.</span><span class="sxs-lookup"><span data-stu-id="2052e-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="2052e-141">Například "&lt;"nahrazuje"&amp;lt;' a '&gt;"nahrazuje"&amp;gt;".</span><span class="sxs-lookup"><span data-stu-id="2052e-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="2052e-142">Tyto speciální kódy zobrazíte pomocí prohlížeče "&lt;'nebo'&gt;" v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="2052e-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="2052e-143">Obsah je možné snadno kódovaný jazykem HTML na serveru pomocí `Server.HtmlEncode(string)` rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="2052e-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="2052e-144">Obsah může být také snadno HTML-dekódovat, to znamená, nastavení bylo vráceno zpět na standardní HTML pomocí `Server.HtmlDecode(string)` metody.</span><span class="sxs-lookup"><span data-stu-id="2052e-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="2052e-145">Výsledek:</span><span class="sxs-lookup"><span data-stu-id="2052e-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
