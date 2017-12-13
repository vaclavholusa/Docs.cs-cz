---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
title: "Část 8: Poslední stránky, výjimek a uzavření | Microsoft Docs"
author: JoeStagner
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 8 přidá kontaktní stránky, o stránku a výjimky..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5aeadf8f-39f3-4f07-a78f-1c310c64fb23
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-8
msc.type: authoredcontent
ms.openlocfilehash: 0dd1717ff1051f18a78fe77402c7603008b9b486
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-8-final-pages-exception-handling-and-conclusion"></a><span data-ttu-id="e9614-104">Část 8: Poslední stránky, výjimek a uzavření</span><span class="sxs-lookup"><span data-stu-id="e9614-104">Part 8: Final Pages, Exception Handling, and Conclusion</span></span>
====================
<span data-ttu-id="e9614-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="e9614-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="e9614-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="e9614-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="e9614-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="e9614-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="e9614-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="e9614-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="e9614-109">Část 8 přidá kontaktní stránky, o stránku a výjimek.</span><span class="sxs-lookup"><span data-stu-id="e9614-109">Part 8 adds a contact page, about page, and exception handling.</span></span> <span data-ttu-id="e9614-110">Toto je uzavření řady.</span><span class="sxs-lookup"><span data-stu-id="e9614-110">This is the conclusion of the series.</span></span>


## <a id="_Toc260221680"></a><span data-ttu-id="e9614-111">Obraťte se na stránce (odesílající e-mailu z prostředí ASP.NET)</span><span class="sxs-lookup"><span data-stu-id="e9614-111">Contact Page (Sending email from ASP.NET)</span></span>

<span data-ttu-id="e9614-112">Vytvořit novou stránku s názvem ContactUs.aspx</span><span class="sxs-lookup"><span data-stu-id="e9614-112">Create a new page named ContactUs.aspx</span></span>

<span data-ttu-id="e9614-113">Pomocí návrháře, vytvořte následující formulář vyjádření speciální ToolkitScriptManager a z AjaxdControlToolkit ovládacího prvku Editor.</span><span class="sxs-lookup"><span data-stu-id="e9614-113">Using the designer, create the following form taking special note to include the ToolkitScriptManager and the Editor control from the AjaxdControlToolkit.</span></span> <span data-ttu-id="e9614-114">.</span><span class="sxs-lookup"><span data-stu-id="e9614-114">.</span></span>

![](tailspin-spyworks-part-8/_static/image1.jpg)

<span data-ttu-id="e9614-115">Dvakrát klikněte na tlačítko "Odeslat" generování obslužnou rutinu události kliknutí v souboru kódu a implementovat metodu pro odeslání kontaktní informace jako e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e9614-115">Double click on the "Submit" button to generate a click event handler in the code behind file and implement a method to send the contact information as an email.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample1.cs)]

<span data-ttu-id="e9614-116">Tento kód vyžaduje, aby souboru web.config obsahuje položku v konfiguračním oddílu, který určuje serveru SMTP pro odesílání e-mailu.</span><span class="sxs-lookup"><span data-stu-id="e9614-116">This code requires that your web.config file contain an entry in the configuration section that specifies the SMTP server to use for sending mail.</span></span>

[!code-xml[Main](tailspin-spyworks-part-8/samples/sample2.xml)]

## <a id="_Toc260221681"></a><span data-ttu-id="e9614-117">O stránku</span><span class="sxs-lookup"><span data-stu-id="e9614-117">About Page</span></span>

<span data-ttu-id="e9614-118">Vytvořte stránku s názvem AboutUs.aspx a přidejte libovolný obsah se vám líbí.</span><span class="sxs-lookup"><span data-stu-id="e9614-118">Create a page named AboutUs.aspx and add whatever content you like.</span></span>

## <a id="_Toc260221682"></a><span data-ttu-id="e9614-119">Globální obslužné rutiny výjimek</span><span class="sxs-lookup"><span data-stu-id="e9614-119">Global Exception Handler</span></span>

<span data-ttu-id="e9614-120">Nakonec v celé aplikaci jsme mít vyvolání výjimky a existují nepředvídatelnými to cold také příčina neošetřené výjimky v našem webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="e9614-120">Lastly, throughout the application we have thrown exceptions and there are unforeseen circumstances that cold also cause unhandled exceptions in our web application.</span></span>

<span data-ttu-id="e9614-121">Chceme nikdy k neošetřené výjimce, který se má zobrazit na webu návštěvníka.</span><span class="sxs-lookup"><span data-stu-id="e9614-121">We never want an unhandled exception to be displayed to a web site visitor.</span></span>

![](tailspin-spyworks-part-8/_static/image2.jpg)

<span data-ttu-id="e9614-122">Kromě se strašlivých uživatelské prostředí neošetřených výjimek lze také potíže se zabezpečením.</span><span class="sxs-lookup"><span data-stu-id="e9614-122">Apart from being a terrible user experience unhandled exceptions can also be a security problem.</span></span>

<span data-ttu-id="e9614-123">Chcete-li vyřešit tento problém bude implementaci globální obslužné rutiny výjimek.</span><span class="sxs-lookup"><span data-stu-id="e9614-123">To solve this problem we will implement a global exception handler.</span></span>

<span data-ttu-id="e9614-124">Chcete-li to provést, otevřete soubor Global.asax a poznamenejte si obslužná rutina následující předem vygenerovaných událostí.</span><span class="sxs-lookup"><span data-stu-id="e9614-124">To do this, open the Global.asax file and note the following pre-generated event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample3.cs)]

<span data-ttu-id="e9614-125">Přidejte kód pro implementaci aplikace\_obslužnou rutinu chyby následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="e9614-125">Add code to implement the Application\_Error handler as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample4.cs)]

<span data-ttu-id="e9614-126">Pak přidejte stránku s názvem Error.aspx k řešení a přidejte tento fragment značkového kódu.</span><span class="sxs-lookup"><span data-stu-id="e9614-126">Then add a page named Error.aspx to the solution and add this markup snippet.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-8/samples/sample5.aspx)]

<span data-ttu-id="e9614-127">Nyní na stránce\_načíst extrakce obslužné rutiny událostí chybové zprávy z objektu žádosti.</span><span class="sxs-lookup"><span data-stu-id="e9614-127">Now in the Page\_Load event handler extract the error messages from the Request Object.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-8/samples/sample6.cs)]

## <a id="_Toc260221683"></a><span data-ttu-id="e9614-128">Uzavření</span><span class="sxs-lookup"><span data-stu-id="e9614-128">Conclusion</span></span>

<span data-ttu-id="e9614-129">Jsme viděli, že webových formulářů ASP.NET umožňuje snadno vytvořit sofistikované web s přístupem k databázi, členství, AJAX, atd.</span><span class="sxs-lookup"><span data-stu-id="e9614-129">We've seen that that ASP.NET WebForms makes it easy to create a sophisticated website with database access, membership, AJAX, etc.</span></span> <span data-ttu-id="e9614-130">poměrně rychle.</span><span class="sxs-lookup"><span data-stu-id="e9614-130">pretty quickly.</span></span>

<span data-ttu-id="e9614-131">Zpravidla v tomto kurzu jste obdrželi nástroje, které potřebujete, abyste mohli začít vytváření vlastních webových formulářů ASP.NET aplikací!</span><span class="sxs-lookup"><span data-stu-id="e9614-131">Hopefully this tutorial has given you the tools you need to get started building your own ASP.NET WebForms applications!</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="e9614-132">Předchozí</span><span class="sxs-lookup"><span data-stu-id="e9614-132">Previous</span></span>](tailspin-spyworks-part-7.md)
