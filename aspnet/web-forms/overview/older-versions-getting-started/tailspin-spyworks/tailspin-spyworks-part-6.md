---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Část 6: Členství technologie ASP.NET | Microsoft Docs'
author: JoeStagner
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. Část 6 přidá členství technologie ASP.NET.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: 83e9bc780ea8face3e0f55fdf8c00e13b60f80a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886807"
---
<a name="part-6-aspnet-membership"></a><span data-ttu-id="02e72-104">Část 6: Členství technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="02e72-104">Part 6: ASP.NET Membership</span></span>
====================
<span data-ttu-id="02e72-105">podle [Stagner Jan](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="02e72-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="02e72-106">Tailspin Spyworks ukazuje, jak neobvykle jednoduché je vytvoření efektivní a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="02e72-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="02e72-107">Zobrazuje vypnout používání skvělé nové funkce v rozhraní ASP.NET 4 k sestavení online úložiště, včetně nákupy, najdete v článku věnovaném a správu.</span><span class="sxs-lookup"><span data-stu-id="02e72-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="02e72-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="02e72-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="02e72-109">Část 6 přidá členství technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="02e72-109">Part 6 adds ASP.NET Membership.</span></span>


## <a id="_Toc260221672"></a>  <span data-ttu-id="02e72-110">Práce s členství technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="02e72-110">Working with ASP.NET Membership</span></span>

![](tailspin-spyworks-part-6/_static/image1.png)

<span data-ttu-id="02e72-111">Klikněte na možnost zabezpečení</span><span class="sxs-lookup"><span data-stu-id="02e72-111">Click Security</span></span>

![](tailspin-spyworks-part-6/_static/image1.jpg)

<span data-ttu-id="02e72-112">Ujistěte se, že používáme ověřování pomocí formulářů.</span><span class="sxs-lookup"><span data-stu-id="02e72-112">Make sure that we are using forms authentication.</span></span>

![](tailspin-spyworks-part-6/_static/image2.jpg)

<span data-ttu-id="02e72-113">Chcete-li vytvořit několik uživatelů pomocí odkazu "Vytvořit uživatele".</span><span class="sxs-lookup"><span data-stu-id="02e72-113">Use the "Create User" link to create a couple of users.</span></span>

![](tailspin-spyworks-part-6/_static/image3.jpg)

<span data-ttu-id="02e72-114">Až budete hotoví, naleznete v okně Průzkumníka řešení a obnovte zobrazení.</span><span class="sxs-lookup"><span data-stu-id="02e72-114">When done, refer to the Solution Explorer window and refresh the view.</span></span>

![](tailspin-spyworks-part-6/_static/image2.png)

<span data-ttu-id="02e72-115">Všimněte si, že ASPNETDB. MDF jemné byla vytvořena.</span><span class="sxs-lookup"><span data-stu-id="02e72-115">Note that the ASPNETDB.MDF fine has been created.</span></span> <span data-ttu-id="02e72-116">Tento soubor obsahuje tabulky, které podporují základní služby technologie ASP.NET, jako je členství.</span><span class="sxs-lookup"><span data-stu-id="02e72-116">This file contains the tables to support the core ASP.NET services like membership.</span></span>

<span data-ttu-id="02e72-117">Nyní jsme zahájí proces najdete v článku věnovaném implementace.</span><span class="sxs-lookup"><span data-stu-id="02e72-117">Now we can begin implementing the checkout process.</span></span>

<span data-ttu-id="02e72-118">Začněte vytvořením CheckOut.aspx stránky.</span><span class="sxs-lookup"><span data-stu-id="02e72-118">Begin by creating a CheckOut.aspx page.</span></span>

<span data-ttu-id="02e72-119">Stránka CheckOut.aspx by měl být k dispozici pouze uživatelům, kteří se přihlásili, jsme se omezení přístupu k přihlášení uživatelů a přesměrovává uživatele, kteří nejsou přihlášeni na přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="02e72-119">The CheckOut.aspx page should only be available to users who are logged in so we will restrict access to logged in users and redirect users who are not logged in to the LogIn page.</span></span>

<span data-ttu-id="02e72-120">K tomu přidáme následující konfigurační oddíl naše souboru web.config.</span><span class="sxs-lookup"><span data-stu-id="02e72-120">To do this we'll add the following to the configuration section of our web.config file.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

<span data-ttu-id="02e72-121">Šablona pro aplikace webových formulářů ASP.NET automaticky přidán k ověřování části našich souboru web.config a vytvořit výchozí přihlašovací stránku.</span><span class="sxs-lookup"><span data-stu-id="02e72-121">The template for ASP.NET Web Forms applications automatically added an authentication section to our web.config file and established the default login page.</span></span>

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

<span data-ttu-id="02e72-122">Jsme musíte upravit Login.aspx souboru migrace anonymní nákupní košík, když se uživatel přihlásí kódu.</span><span class="sxs-lookup"><span data-stu-id="02e72-122">We must modify the Login.aspx code behind file to migrate an anonymous shopping cart when the user logs in.</span></span> <span data-ttu-id="02e72-123">Změnit stránce\_událostí načtení následujícím způsobem.</span><span class="sxs-lookup"><span data-stu-id="02e72-123">Change the Page\_Load event as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

<span data-ttu-id="02e72-124">Pak přidejte obslužnou rutinu události "LoggedIn" takto nastavte název relace na nově přihlášeného uživatele a změnit id dočasné relace v nákupní košík na tohoto uživatele pomocí volání metody MigrateCart Naše třída MyShoppingCart.</span><span class="sxs-lookup"><span data-stu-id="02e72-124">Then add a "LoggedIn" event handler like this to set the session name to the newly logged in user and change the temporary session id in the shopping cart to that of the user by calling the MigrateCart method in our MyShoppingCart class.</span></span> <span data-ttu-id="02e72-125">(Implementována v souboru .cs)</span><span class="sxs-lookup"><span data-stu-id="02e72-125">(Implemented in the .cs file)</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

<span data-ttu-id="02e72-126">Implementujte metodu MigrateCart() podobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="02e72-126">Implement the MigrateCart() method like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

<span data-ttu-id="02e72-127">V checkout.aspx použijeme EntityDataSource a GridView v našem podívejte se na stránku tolik, jako jsme to udělali v naší stránce s nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="02e72-127">In checkout.aspx we'll use an EntityDataSource and a GridView in our check out page much as we did in our shopping cart page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

<span data-ttu-id="02e72-128">Všimněte si, že naše ovládací prvek GridView určuje obslužné rutiny události "ondatabound" s názvem MyList\_RowDataBound můžeme implementovat této obslužné rutiny události takto.</span><span class="sxs-lookup"><span data-stu-id="02e72-128">Note that our GridView control specifies an "ondatabound" event handler named MyList\_RowDataBound so let's implement that event handler like this.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

<span data-ttu-id="02e72-129">Tato metoda udržuje spuštění úplného nákupního košíku jako každý řádek je vázán a aktualizuje dolní řádek GridView.</span><span class="sxs-lookup"><span data-stu-id="02e72-129">This method keeps a running total of the shopping cart as each row is bound and updates the bottom row of the GridView.</span></span>

<span data-ttu-id="02e72-130">V této fázi jste implementovali jsme prezentace "kontrola" aby bylo možné umístit.</span><span class="sxs-lookup"><span data-stu-id="02e72-130">At this stage we have implemented a "review" presentation of the order to be placed.</span></span>

<span data-ttu-id="02e72-131">Umožňuje zpracování scénářem prázdný košíku přidáním několika řádků kódu do naší stránce s\_zatížení událostí:</span><span class="sxs-lookup"><span data-stu-id="02e72-131">Let's handle an empty cart scenario by adding a few lines of code to our Page\_Load event:</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

<span data-ttu-id="02e72-132">Když uživatel klikne na tlačítko "Odeslat" jsme se spusťte následující kód v obslužné rutině události klikněte na tlačítko Odeslat.</span><span class="sxs-lookup"><span data-stu-id="02e72-132">When the user clicks on the "Submit" button we will execute the following code in the Submit Button Click Event handler.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

<span data-ttu-id="02e72-133">"Maso" procesu odeslání objednávky se provádí v metodě SubmitOrder() naše MyShoppingCart třídy.</span><span class="sxs-lookup"><span data-stu-id="02e72-133">The "meat" of the order submission process is to be implemented in the SubmitOrder() method of our MyShoppingCart class.</span></span>

<span data-ttu-id="02e72-134">SubmitOrder bude:</span><span class="sxs-lookup"><span data-stu-id="02e72-134">SubmitOrder will:</span></span>

- <span data-ttu-id="02e72-135">Proveďte všechny položky řádku nákupní košík a použít je k vytvoření nového záznamu pořadí a přidružené záznamy Rozpis objednávek.</span><span class="sxs-lookup"><span data-stu-id="02e72-135">Take all the line items in the shopping cart and use them to create a new Order Record and the associated OrderDetails records.</span></span>
- <span data-ttu-id="02e72-136">Vypočítejte přesouvání datum.</span><span class="sxs-lookup"><span data-stu-id="02e72-136">Calculate Shipping Date.</span></span>
- <span data-ttu-id="02e72-137">Clear – nákupní košík.</span><span class="sxs-lookup"><span data-stu-id="02e72-137">Clear the shopping cart.</span></span>


[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

<span data-ttu-id="02e72-138">Pro účely této ukázkové aplikaci jsme vám vypočítat datum expedice stačí přidat dva dny na aktuální datum.</span><span class="sxs-lookup"><span data-stu-id="02e72-138">For the purposes of this sample application we'll calculate a ship date by simply adding two days to the current date.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

<span data-ttu-id="02e72-139">Spuštění aplikace teď bude umožňují nám otestování procesu nákupní od začátku do konce.</span><span class="sxs-lookup"><span data-stu-id="02e72-139">Running the application now will permit us to test the shopping process from start to finish.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="02e72-140">[Předchozí](tailspin-spyworks-part-5.md)
> [další](tailspin-spyworks-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="02e72-140">[Previous](tailspin-spyworks-part-5.md)
[Next](tailspin-spyworks-part-7.md)</span></span>
