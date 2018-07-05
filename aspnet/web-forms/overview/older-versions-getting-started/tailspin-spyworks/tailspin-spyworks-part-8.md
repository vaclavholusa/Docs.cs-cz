---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: '8. část: Závěrečné stránky, zpracování výjimek a závěr | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá stránku kontaktní informace o stránce a výjimek...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 3f1d855e157f6a58995d301a793e660925767fb4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380250"
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="7cfac-104">8. část: Závěrečné stránky, zpracování výjimek a závěr</span><span class="sxs-lookup"><span data-stu-id="7cfac-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="7cfac-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="7cfac-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="7cfac-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="7cfac-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="7cfac-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="7cfac-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="7cfac-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="7cfac-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="7cfac-109">Část 8 přidá stránku kontaktní informace o stránce a zpracování výjimek.</span><span class="sxs-lookup"><span data-stu-id="7cfac-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="7cfac-110">Toto je uzavření řady.</span><span class="sxs-lookup"><span data-stu-id="7cfac-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a>  <span data-ttu-id="7cfac-111">Obraťte se na stránku (odesílání e-mailu z technologie ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="7cfac-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="7cfac-112">Vytvoří novou stránku s názvem ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="7cfac-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="7cfac-113">Pomocí návrháře, vytvořte následující formulář s ohledem ToolkitScriptManager a ovládací prvek editoru z AjaxdControlToolkit zvláštní poznámka.</span><span class="sxs-lookup"><span data-stu-id="7cfac-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="7cfac-114">.</span><span class="sxs-lookup"><span data-stu-id="7cfac-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="7cfac-115">Dvakrát klikněte na tlačítko "Odeslat" a vygenerovat obslužnou rutinu události click v souboru kódu na pozadí a implementovat metodu pro odeslání kontaktní informace jako e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7cfac-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="7cfac-116">Tento kód vyžaduje, že soubor web.config obsahovat záznam v konfiguračním oddílu, který určuje název serveru SMTP pro odeslání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7cfac-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a>  <span data-ttu-id="7cfac-117">O stránku</span><span class="sxs-lookup"><span data-stu-id="7cfac-117">About Page</span></span>

<span data-ttu-id="7cfac-118">Vytvoření stránky s názvem AboutUs.aspx a přidejte libovolný obsah, který vám vyhovuje.</span><span class="sxs-lookup"><span data-stu-id="7cfac-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a>  <span data-ttu-id="7cfac-119">Globální obslužné rutiny výjimek</span><span class="sxs-lookup"><span data-stu-id="7cfac-119">Global Exception Handler</span></span>

<span data-ttu-id="7cfac-120">Nakonec v celé aplikaci budeme mít vyvolané výjimky a existují nepředvídatelnými to studenou také příčina neošetřené výjimky v naší webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7cfac-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="7cfac-121">Chceme, aby nikdy nezpracovanou výjimku, který se má zobrazit na webu návštěvníka.</span><span class="sxs-lookup"><span data-stu-id="7cfac-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="7cfac-122">Kromě ještěrů uživatelské prostředí se neošetřené výjimky lze také potíže se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="7cfac-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="7cfac-123">Pro vyřešení tohoto problému jsme implementuje globální obslužné rutiny výjimek.</span><span class="sxs-lookup"><span data-stu-id="7cfac-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="7cfac-124">Chcete-li to provést, otevřete soubor Global.asax a mějte na paměti následující obslužnou rutinu události předběžně vygenerované.</span><span class="sxs-lookup"><span data-stu-id="7cfac-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="7cfac-125">Přidejte kód k implementaci aplikace\_obslužná rutina chyb následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="7cfac-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="7cfac-126">Pak přidejte stránku s názvem Error.aspx k řešení a přidejte tento fragment kódu.</span><span class="sxs-lookup"><span data-stu-id="7cfac-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="7cfac-127">Teď na stránce\_načíst extrakce obslužné rutiny události chybové zprávy z objektu žádosti.</span><span class="sxs-lookup"><span data-stu-id="7cfac-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a>  <span data-ttu-id="7cfac-128">Uzavření</span><span class="sxs-lookup"><span data-stu-id="7cfac-128">Conclusion</span></span>

<span data-ttu-id="7cfac-129">Viděli jsme, že využitím součástí ASP.NET WebForms lze snadno vytvořit sofistikované web se přístup k databázi, členství, AJAX, atd.</span><span class="sxs-lookup"><span data-stu-id="7cfac-129">We've seen that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="7cfac-130">poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="7cfac-130">pretty quickly.</span></span>

<span data-ttu-id="7cfac-131">Snad v tomto kurzu, má udělené vám nástroje, které potřebujete, abyste mohli začít vytvářet své vlastní webových formulářů ASP.NET aplikace!</span><span class="sxs-lookup"><span data-stu-id="7cfac-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7cfac-132">Předchozí</span><span class="sxs-lookup"><span data-stu-id="7cfac-132">Previous</span></span>](tailspin-spyworks-part-7.md)
