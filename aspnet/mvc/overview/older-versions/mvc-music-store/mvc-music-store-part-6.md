---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: "Část 6: Pomocí datových poznámek pro ověření modelu | Microsoft Docs"
author: jongalloway
description: "Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 6 obsahuje pomocí datových poznámek pro Model V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: b2083d5604741a0142f504f92779c9f931d0d6d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="22c3c-104">Část 6: Pomocí datových poznámek pro ověření modelu</span><span class="sxs-lookup"><span data-stu-id="22c3c-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="22c3c-105">podle [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="22c3c-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="22c3c-106">Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.</span><span class="sxs-lookup"><span data-stu-id="22c3c-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="22c3c-107">Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.</span><span class="sxs-lookup"><span data-stu-id="22c3c-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="22c3c-108">Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště.</span><span class="sxs-lookup"><span data-stu-id="22c3c-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="22c3c-109">Část 6 obsahuje pomocí datových poznámek pro ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="22c3c-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="22c3c-110">Máme závažný problém s formuláři naše vytvořit a upravit: jejich nejsou probíhá žádné ověření.</span><span class="sxs-lookup"><span data-stu-id="22c3c-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="22c3c-111">Provedeme akce, jako je ponechat typ písmena a povinná pole prázdné pole Cena a první chyba, kterou jsme zobrazí je z databáze.</span><span class="sxs-lookup"><span data-stu-id="22c3c-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="22c3c-112">Můžete snadno přidáme ověření k naší aplikaci přidáním datových poznámek k naší třídy modelu.</span><span class="sxs-lookup"><span data-stu-id="22c3c-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="22c3c-113">Nám pro popis pravidla, která má být u naše vlastnosti modelu umožňují datových poznámek a ASP.NET MVC se postará o jejich vynucením a zobrazení příslušné zprávy pro naše uživatele.</span><span class="sxs-lookup"><span data-stu-id="22c3c-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="22c3c-114">Přidání ověřování do našich Album formulářů</span><span class="sxs-lookup"><span data-stu-id="22c3c-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="22c3c-115">Použijeme následující atributy datové poznámky:</span><span class="sxs-lookup"><span data-stu-id="22c3c-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="22c3c-116">**Požadované** – označuje, že vlastnost je povinné pole.</span><span class="sxs-lookup"><span data-stu-id="22c3c-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="22c3c-117">**DisplayName** – definuje text chceme použité na pole formuláře a ověřovacích zpráv</span><span class="sxs-lookup"><span data-stu-id="22c3c-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="22c3c-118">**StringLength** – definuje maximální délku pole řetězce</span><span class="sxs-lookup"><span data-stu-id="22c3c-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="22c3c-119">**Rozsah** – poskytuje maximální a minimální hodnotu pro číselné pole</span><span class="sxs-lookup"><span data-stu-id="22c3c-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="22c3c-120">**Vytvoření vazby** – obsahuje seznam polí pro vyloučení nebo zahrnutí při vytváření vazby parametru nebo formuláře hodnoty pro vlastnosti modelu</span><span class="sxs-lookup"><span data-stu-id="22c3c-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="22c3c-121">**ScaffoldColumn** – umožňuje skrytí pole z editoru formulářů</span><span class="sxs-lookup"><span data-stu-id="22c3c-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="22c3c-122">*Poznámka: Další informace o ověření modelu pomocí datové poznámky atributů najdete v dokumentaci k webu MSDN na*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="22c3c-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="22c3c-123">Otevřete Album třídu a přidejte následující *pomocí* příkazů do horní části.</span><span class="sxs-lookup"><span data-stu-id="22c3c-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="22c3c-124">Potom aktualizujte vlastnosti, které chcete přidat atributy zobrazení a ověření, jak je uvedeno níže.</span><span class="sxs-lookup"><span data-stu-id="22c3c-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="22c3c-125">Když jsme existuje, jsme vám také změnili Genre a umělcem virtuální vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="22c3c-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="22c3c-126">To umožňuje rozhraní Entity Framework opožděné zatížení je podle potřeby.</span><span class="sxs-lookup"><span data-stu-id="22c3c-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="22c3c-127">Po tyto atributy s přidá do našich modelu alb, naše vytvořit a upravit obrazovky okamžitě začne ověřování pole a pomocí zobrazované názvy jsme jste vybrali (například Album obrázky Url místo AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="22c3c-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="22c3c-128">Spusťte aplikaci a přejděte do /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="22c3c-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="22c3c-129">Dále budete jsme rozdělit některá pravidla ověřování.</span><span class="sxs-lookup"><span data-stu-id="22c3c-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="22c3c-130">Zadejte cena 0 a ponechejte pole název prázdné.</span><span class="sxs-lookup"><span data-stu-id="22c3c-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="22c3c-131">Když jsme klikněte na tlačítko pro vytvoření, vidíte formuláře, zobrazí se chybové zprávy ověření zobrazující pole, která nesplňuje pravidla ověření, že jsme definovali.</span><span class="sxs-lookup"><span data-stu-id="22c3c-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="22c3c-132">Testování ověřování straně klienta</span><span class="sxs-lookup"><span data-stu-id="22c3c-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="22c3c-133">Ověřování na straně serveru je velmi důležité z perspektivy aplikaci, protože uživatelé mohou obejít ověřování na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="22c3c-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="22c3c-134">Webová stránka formulářů, implementující pouze ověření na straně serveru však vykazovat tři významné problémy.</span><span class="sxs-lookup"><span data-stu-id="22c3c-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="22c3c-135">Uživatel má počkat pro daný formulář, který se má odeslat, ověřit na serveru a pro odpověď k odeslání do svého prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="22c3c-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="22c3c-136">Uživatel nemá získat okamžitou zpětnou vazbu, při jejich opravte pole tak, aby ho teď předá ověřovacích pravidel.</span><span class="sxs-lookup"><span data-stu-id="22c3c-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="22c3c-137">Nemůžeme se plýtvání prostředky serveru k provedení logiku ověření místo využívání prohlížeče uživatele.</span><span class="sxs-lookup"><span data-stu-id="22c3c-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="22c3c-138">Naštěstí šablony vygenerované uživatelské rozhraní ASP.NET MVC 3 mají ověřování na straně klienta integrovanou, vyžadování jakkoli další práci.</span><span class="sxs-lookup"><span data-stu-id="22c3c-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="22c3c-139">Zadáním jednoho písmeno v poli s názvem splňuje požadavky na ověření, zobrazí se zpráva ověření se okamžitě odebere.</span><span class="sxs-lookup"><span data-stu-id="22c3c-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


>[!div class="step-by-step"]
<span data-ttu-id="22c3c-140">[Předchozí](mvc-music-store-part-5.md)
[další](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="22c3c-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
