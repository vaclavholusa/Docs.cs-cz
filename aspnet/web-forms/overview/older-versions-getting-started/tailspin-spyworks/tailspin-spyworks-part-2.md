---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: '– Část 2: Vrstva přístupu k datům | Dokumentace Microsoftu'
author: JoeStagner
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks. 2. část se věnuje přidání vrstvy přístupu k datům.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5b5ae08b157054bf0f3231782127ee3110f36d00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752437"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="88bd9-104">2. část: Vrstva přístupu k datům</span><span class="sxs-lookup"><span data-stu-id="88bd9-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="88bd9-105">podle [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="88bd9-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="88bd9-106">Tailspin Spyworks ukazuje, jak mimořádně jednoduché je vytvářet výkonné a škálovatelné aplikace pro platformu .NET.</span><span class="sxs-lookup"><span data-stu-id="88bd9-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="88bd9-107">Zobrazuje vypnout použití skvělých nových funkcí v technologii ASP.NET 4 k sestavení nebo online úložiště, včetně nákupu, Pokladna a správu.</span><span class="sxs-lookup"><span data-stu-id="88bd9-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="88bd9-108">V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="88bd9-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="88bd9-109">2. část se věnuje přidání vrstvy přístupu k datům.</span><span class="sxs-lookup"><span data-stu-id="88bd9-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="88bd9-110">Přidání vrstvy přístupu k datům</span><span class="sxs-lookup"><span data-stu-id="88bd9-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="88bd9-111">Naši aplikaci elektronického obchodování, bude záviset na dvě databáze.</span><span class="sxs-lookup"><span data-stu-id="88bd9-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="88bd9-112">Informace o zákaznících použijeme standardní databáze členství technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="88bd9-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="88bd9-113">Pro náš katalog nákupního košíku a produktu jsme budete následujícím způsobem implementace databáze SQL Express.</span><span class="sxs-lookup"><span data-stu-id="88bd9-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="88bd9-114">Databáze (Commerce.mdf) máte vytvořený v aplikaci aplikace\_složka dat je možné přejít k vytvoření naší vrstvy přístupu k datům pomocí Entity Frameworku .NET.</span><span class="sxs-lookup"><span data-stu-id="88bd9-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="88bd9-115">Vytvoříme složku s názvem "Data\_přístup" a je v této složce klikněte pravým tlačítkem myši a vyberte "Přidat novou položku".</span><span class="sxs-lookup"><span data-stu-id="88bd9-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="88bd9-116">"Nainstalované šablony" položky a pak vyberte "ADO.NET Entity Data Model" Zadejte EDM\_Commerce.edmx jako název a klikněte na tlačítko "Přidat".</span><span class="sxs-lookup"><span data-stu-id="88bd9-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="88bd9-117">Zvolte možnost "Generovat z databáze".</span><span class="sxs-lookup"><span data-stu-id="88bd9-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="88bd9-118">Uložit a sestavit.</span><span class="sxs-lookup"><span data-stu-id="88bd9-118">Save and build.</span></span>

<span data-ttu-id="88bd9-119">Nyní jsme připraveni pro přidání naši první funkci – nabídky kategorie produktu.</span><span class="sxs-lookup"><span data-stu-id="88bd9-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88bd9-120">[Předchozí](tailspin-spyworks-part-1.md)
> [další](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="88bd9-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
