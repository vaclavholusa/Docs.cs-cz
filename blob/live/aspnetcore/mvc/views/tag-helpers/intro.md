---
title: "Pomocné rutiny značky v ASP.NET Core"
author: rick-anderson
description: "Zjistěte, jaké jsou pomocné rutiny značky a jejich použití v ASP.NET Core."
keywords: "ASP.NET Core, značka pomocné rutiny"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 78d9fdbd32eae29d7714507498d5d68b86c6d12e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="3f7a7-104">Úvod do pomocné rutiny značky v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f7a7-104">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="3f7a7-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3f7a7-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="3f7a7-106">Jaké jsou pomocné rutiny značky?</span><span class="sxs-lookup"><span data-stu-id="3f7a7-106">What are Tag Helpers?</span></span>

<span data-ttu-id="3f7a7-107">Pomocníci značky povolit kódu na straně serveru k účasti ve vytváření a vykreslení elementů HTML v souborech Razor.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-107">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="3f7a7-108">Například předdefinovaná `ImageTagHelper` můžete připojit číslo verze pro název bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-108">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="3f7a7-109">Při každé změně bitovou kopii, server vygeneruje nové jedinečné verze pro bitovou kopii, tak je zaručeno, že klienti získat aktuální image (ne zastaralé bitové kopie v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-109">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="3f7a7-110">Existuje mnoho předdefinovaných značky pomocníci pro běžné úlohy -, jako je například vytváření formulářů, odkazy, načítání prostředků a další - a i další dostupné ve veřejných úložišť GitHub a jako NuGet balíčky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-110">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="3f7a7-111">Pomocníci značky vytvořené v C# a jejich cílových elementů HTML na základě název elementu, název atributu nebo nadřazené značky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-111">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="3f7a7-112">Například předdefinovaná `LabelTagHelper` HTML, můžete vybrat `<label>` element při `LabelTagHelper` jsou použity atributy.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-112">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="3f7a7-113">Pokud jste obeznámeni s [pomocné objekty HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), značka Pomocníci snížit explicitní přechodů mezi HTML a C# v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-113">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="3f7a7-114">V mnoha případech pomocné objekty HTML poskytnout alternativní způsob konkrétní pomocné rutiny značky, ale je důležité vědět, že značka Pomocníci nenahrazují pomocné rutiny HTML a neexistuje žádná značka Pomocník pro každý pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-114">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="3f7a7-115">[Značka pomocné rutiny ve srovnání s pomocné objekty HTML](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly podrobněji.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-115">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="3f7a7-116">Co zadejte značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="3f7a7-116">What Tag Helpers provide</span></span>

<span data-ttu-id="3f7a7-117">**Vývoj prostředí HTML-friendly** pro nejvíce část kódu Razor pomocí značky Pomocníci vypadá jako standardní HTML.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-117">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="3f7a7-118">Front-endu Designer obeznámen s HTML + CSS + JavaScript můžete upravit Razor bez učení syntaxi Razor C#.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-118">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="3f7a7-119">**Bohaté prostředí technologie IntelliSense pro vytvoření značky HTML a Razor** jde sharp naproti tomu do pomocné rutiny HTML, předchozí postup k vytvoření serverové značek v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-119">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="3f7a7-120">[Značka pomocné rutiny ve srovnání s pomocné objekty HTML](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly podrobněji.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-120">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="3f7a7-121">[Podporu technologie IntelliSense pro značku Pomocníci](#intellisense-support-for-tag-helpers) vysvětluje IntelliSense prostředí.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-121">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="3f7a7-122">I vývojáři došlo se syntaxí Razor C# jsou zvýšit produktivitu při používání značky Pomocníci než psaní kódu jazyka C# Razor.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-122">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="3f7a7-123">**Tak, aby vás zvýšit produktivitu a moct vytvářet robustnější a spolehlivé a udržovatelného kódu pomocí informace, které jsou k dispozici pouze na serveru** v minulosti mantra o aktualizaci bitové kopie byl například změnit název obrázku při změně obrázek.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-123">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="3f7a7-124">Bitové kopie by měl být intenzivně do mezipaměti z důvodů výkonu, a pokud změníte název bitové kopie, riskujete, že klienti získávání kopii zastaralé.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-124">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="3f7a7-125">V minulosti po obrázku bylo upraveno, musel změnit název a každý odkaz na obrázek ve webové aplikaci potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-125">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="3f7a7-126">Ne jenom je to velmi práce že náročné, je také náchylný (můžete může neproběhly odkaz, zadejte omylem nesprávný řetězec atd.) Integrované `ImageTagHelper` lze provést pro vás automaticky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-126">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="3f7a7-127">`ImageTagHelper` Můžete připojit z verze číslo název bitové kopie, takže při každé změně bitovou kopii, server automaticky vygeneruje nové verze jedinečný pro bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-127">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="3f7a7-128">Klienti jsou zaručit získat aktuální image.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-128">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="3f7a7-129">Tato úspory odolné a práci se dodává v podstatě volné pomocí `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-129">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="3f7a7-130">Většina předdefinované značky Pomocníci cíli stávající elementy HTML a poskytuje serverové atributy elementu.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-130">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="3f7a7-131">Například `<input>` element používán v mnoha zobrazení *zobrazení nebo účet* složka obsahuje `asp-for` atribut, který extrahuje název vlastnosti zadaného modelu do vykreslené kódu HTML.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-131">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="3f7a7-132">Následující kód Razor:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-132">The following Razor markup:</span></span>

```cshtml
<label asp-for="Email"></label>
```

<span data-ttu-id="3f7a7-133">Generuje následující HTML:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-133">Generates the following HTML:</span></span>

```cshtml
<label for="Email">Email</label>
```

<span data-ttu-id="3f7a7-134">`asp-for` Atribut je k dispozici ve `For` vlastnost `LabelTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-134">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="3f7a7-135">V tématu [vytváření Pomocníci značky](authoring.md) Další informace.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-135">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="3f7a7-136">Správa značka pomocná oboru</span><span class="sxs-lookup"><span data-stu-id="3f7a7-136">Managing Tag Helper scope</span></span>

<span data-ttu-id="3f7a7-137">Značka pomocné rutiny oboru řídí kombinaci `@addTagHelper`, `@removeTagHelper`a "!" výslovný nesouhlas s znak.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-137">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="3f7a7-138">`@addTagHelper`zpřístupní značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="3f7a7-138">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="3f7a7-139">Pokud vytvoříte novou webovou aplikaci ASP.NET Core s názvem *AuthoringTagHelpers* (bez jakéhokoli ověřování), následující *Views/_ViewImports.cshtml* soubor bude přidán do projektu:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-139">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="3f7a7-140">`@addTagHelper` – Direktiva zpřístupní Pomocníci značky k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-140">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="3f7a7-141">V takovém případě je soubor zobrazení *Views/_ViewImports.cshtml*, který ve výchozím nastavení dědí všechny soubory zobrazení v *zobrazení* složku a podadresářích; zpřístupnění značky pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-141">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="3f7a7-142">Výše uvedený kód používá syntaxi zástupný znak ("\*") Chcete-li určit, že všechny značky pomocníky v zadaném sestavení (*Microsoft.AspNetCore.Mvc.TagHelpers*) bude k dispozici pro každý soubor zobrazení v *zobrazení* nebo dílčí adresáři.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-142">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="3f7a7-143">První parametr po `@addTagHelper` určuje značky Pomocníci načíst (používáme "\*" pro všechny značky pomocníky), a druhý parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Určuje sestavení obsahující pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-143">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="3f7a7-144">*Microsoft.AspNetCore.Mvc.TagHelpers* je sestavení pro předdefinované Pomocníci značky základní technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-144">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="3f7a7-145">Ke zveřejnění všechny značky Pomocníci v tomto projektu (která vytvoří sestavení s názvem *AuthoringTagHelpers*), byste použili následující:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-145">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="3f7a7-146">Pokud projekt obsahuje `EmailTagHelper` s výchozí obor názvů (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), můžete zadat plně kvalifikovaný název (FQN) Helper značky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-146">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="3f7a7-147">Přidání značka pomocné rutiny zobrazení pomocí FQN, je nejprve přidat FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-147">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="3f7a7-148">Většina vývojářů dávají přednost používání "\*" syntaxi se zástupnými znaky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-148">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="3f7a7-149">Zástupný znak syntaxe umožňuje zástupný znak "\*" jako příponu v FQN.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-149">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="3f7a7-150">Například některé z následujících direktivy budou předány `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-150">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="3f7a7-151">Jak je uvedeno nahoře, přidání `@addTagHelper` direktivy k *Views/_ViewImports.cshtml* souboru zpřístupní pomocná značky ke všem souborům zobrazení v *zobrazení* adresář a dílčí adresáře.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-151">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="3f7a7-152">Můžete použít `@addTagHelper` v souborech konkrétní zobrazení, pokud se chcete přihlásit k vystavení pomocníka značky, který má jenom tato zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-152">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="3f7a7-153">`@removeTagHelper`Odebere značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="3f7a7-153">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="3f7a7-154">`@removeTagHelper` Má stejné dva parametry jako `@addTagHelper`, a odebere značky pomocné rutiny, která byla přidána dříve.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-154">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="3f7a7-155">Například `@removeTagHelper` použít konkrétní zobrazení odebere zadané pomocné rutiny značky ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-155">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="3f7a7-156">Pomocí `@removeTagHelper` v *Views/Folder/_ViewImports.cshtml* souboru odebere zadané pomocné rutiny značku ze všech zobrazení v *složky*.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-156">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="3f7a7-157">Řízení obor značky Pomocník s *_ViewImports.cshtml* souboru</span><span class="sxs-lookup"><span data-stu-id="3f7a7-157">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="3f7a7-158">Můžete přidat *_ViewImports.cshtml* libovolné složky zobrazení a zobrazení, použije modul direktivy z obou tento soubor a *Views/_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-158">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="3f7a7-159">Pokud jste přidali prázdnou *Views/Home/_ViewImports.cshtml* souboru *Domů* zobrazení, by žádná změna protože *_ViewImports.cshtml* soubor sčítání.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-159">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="3f7a7-160">Všechny `@addTagHelper` direktivy přidáte *Views/Home/_ViewImports.cshtml* souboru (které nejsou ve výchozím *Views/_ViewImports.cshtml* souboru) by vystavit tyto značky pomocné rutiny pro zobrazení pouze v *Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-160">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="3f7a7-161">Zrušení jednotlivé elementy</span><span class="sxs-lookup"><span data-stu-id="3f7a7-161">Opting out of individual elements</span></span>

<span data-ttu-id="3f7a7-162">Můžete zakázat pomocné rutiny značky na úrovni element výslovný nesouhlas s znakem pomocná značky ("!").</span><span class="sxs-lookup"><span data-stu-id="3f7a7-162">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="3f7a7-163">Například `Email` ověření je zakázané v `<span>` znakem výslovný nesouhlas s pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-163">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="3f7a7-164">Znak výslovný nesouhlas s pomocná značky je nutné použít pro počáteční a koncovou značku.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-164">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="3f7a7-165">(Editoru Visual Studio automaticky přidá znak vyjádření výslovného nesouhlasu s uzavírací značku při přidávání jednoho ke značce otevírání).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-165">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="3f7a7-166">Po přidání znak vyjádření výslovného nesouhlasu s elementu a atributů značky Pomocník se už nezobrazuje v rozlišovací písmo.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-166">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="3f7a7-167">Pomocí `@tagHelperPrefix` aby explicitní použití pomocníka značky</span><span class="sxs-lookup"><span data-stu-id="3f7a7-167">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="3f7a7-168">`@tagHelperPrefix` – Direktiva umožňuje zadat řetězec předpony značky, chcete-li povolit podporu pomocníků značky a vytvoření značky pomocná využití explicitní.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-168">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="3f7a7-169">Můžete například přidat následující kód do *Views/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-169">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="3f7a7-170">Následující obrázek kód předponu značky pomocná nastavena na `th:`, takže jenom elementy, pomocí předponu `th:` podporu pomocníků značky (povolené značky pomocné prvky mít rozlišovací písmo).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-170">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="3f7a7-171">`<label>` a `<input>` prvky mít předponu značky pomocná a povolenými značky Pomocník při `<span>` element neexistuje.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-171">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element does not.</span></span>

![obrázek](intro/_static/thp.png)

<span data-ttu-id="3f7a7-173">Stejná hierarchie pravidla, které platí pro `@addTagHelper` platí také pro `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-173">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="3f7a7-174">Podporu technologie IntelliSense pro značku pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="3f7a7-174">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="3f7a7-175">Když vytvoříte novou webovou aplikaci ASP.NET v sadě Visual Studio, přidává balíček NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="3f7a7-175">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="3f7a7-176">Toto je balíček, který přidá značku pomocné nástroje.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-176">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="3f7a7-177">Vezměte v úvahu zápis HTML `<label>` elementu.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-177">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="3f7a7-178">Jakmile zadáte `<l` v editoru Visual Studio IntelliSense zobrazí odpovídající prvky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-178">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obrázek](intro/_static/label.png)

<span data-ttu-id="3f7a7-180">Nejenže můžete získat HTML – Nápověda, ale na ikonu ("@" symbol with "<>" v něm).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-180">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![obrázek](intro/_static/tagSym.png)

<span data-ttu-id="3f7a7-182">identifikuje element jako cílem pomocníků značky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-182">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="3f7a7-183">Čistý elementů HTML (například `fieldset`) ikona "<>" zobrazení.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-183">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="3f7a7-184">Prostý jazyk HTML `<label>` značka zobrazí značky HTML (s výchozí motiv sady Visual Studio barvu) hnědá písmeny, atributy červeně, a atribut hodnoty modře.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-184">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![obrázek](intro/_static/LableHtmlTag.png)

<span data-ttu-id="3f7a7-186">Po zadání `<label`, IntelliSense zobrazí seznam dostupné atributy HTML nebo šablon stylů CSS a atributy cílové pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-186">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![obrázek](intro/_static/labelattr.png)

<span data-ttu-id="3f7a7-188">Dokončování IntelliSense umožňuje zadat klávesy tab můžete dokončit příkaz s vybranou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-188">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![obrázek](intro/_static/stmtcomplete.png)

<span data-ttu-id="3f7a7-190">Jakmile je zadán pomocný značka atribut, značky a atribut písem změnit.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-190">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="3f7a7-191">Použití výchozí sady Visual Studio "Blue" nebo "Light" barvu motivu, písmo je tučné fialová.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-191">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="3f7a7-192">Pokud používáte "Tmavý" motiv je písmo tučné šedozelená.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-192">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="3f7a7-193">Bitové kopie v tomto dokumentu byly pořízeny pomocí výchozí motiv.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-193">The images in this document were taken using the default theme.</span></span>

![obrázek](intro/_static/labelaspfor2.png)

<span data-ttu-id="3f7a7-195">Můžete zadat sady Visual Studio *CompleteWord* zástupce (Ctrl + mezerník je [výchozí](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) uvnitř dvojitých uvozovek (""), a je nyní v jazyce C#, stejně, jako by byl v třídě C#.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-195">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="3f7a7-196">IntelliSense zobrazí všechny metody a vlastnosti v modelu stránky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-196">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="3f7a7-197">Metody a vlastnosti jsou k dispozici, protože je vlastnost typu `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-197">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="3f7a7-198">Na obrázku níže I mě úpravy `Register` zobrazení, proto `RegisterViewModel` je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-198">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![obrázek](intro/_static/intellemail.png)

<span data-ttu-id="3f7a7-200">IntelliSense zobrazí seznam vlastností a metod, které jsou k dispozici pro model na stránce.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-200">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="3f7a7-201">Bohaté prostředí IntelliSense pomáhá vyberte třídu CSS:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-201">The rich IntelliSense environment helps you select the CSS class:</span></span>

![obrázek](intro/_static/iclass.png)

![obrázek](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="3f7a7-204">Pomocníci značky ve srovnání s pomocné objekty HTML</span><span class="sxs-lookup"><span data-stu-id="3f7a7-204">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="3f7a7-205">Pomocníci značky přiřadit elementů HTML v zobrazení syntaxe Razor, zatímco [pomocné rutiny HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) jsou vyvolány jako metody spolu s HTML v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-205">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="3f7a7-206">Vezměte v úvahu následující kód Razor, který vytvoří popisek HTML pomocí třídy CSS "Popisek":</span><span class="sxs-lookup"><span data-stu-id="3f7a7-206">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="3f7a7-207">Na (`@`) symbol informuje Razor začátek kódu.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-207">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="3f7a7-208">Následující dva parametry ("FirstName" a "křestní jméno:") jsou řetězce, takže [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) nemůže pomoct.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-208">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="3f7a7-209">Poslední argument:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-209">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="3f7a7-210">Slouží k reprezentaci atributy anonymní objekt.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-210">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="3f7a7-211">Protože **– třída** je vyhrazené klíčové slovo v jazyce C#, můžete použít `@` symbol, který má vynutit C# interpretovat "@class=" jako symbol (název vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-211">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="3f7a7-212">Do front-endu návrháře (někdo obeznámeni s HTML + CSS + JavaScript a další technologie klienta, ale není obeznámeni s C# a Razor), je většina řádku cizí.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-212">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="3f7a7-213">Celý řádek musí být vytvořené bez pomoci z IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-213">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="3f7a7-214">Pomocí `LabelTagHelper`, lze zapsat značku stejné jako:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-214">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![obrázek](intro/_static/label2.png)

<span data-ttu-id="3f7a7-216">S verzí značky Helper, jakmile zadáte `<l` v editoru Visual Studio IntelliSense zobrazí odpovídající prvky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-216">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obrázek](intro/_static/label.png)

<span data-ttu-id="3f7a7-218">IntelliSense umožňuje zapsat celý řádek.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-218">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="3f7a7-219">`LabelTagHelper` Také výchozí hodnoty nastavení obsah `asp-for` atribut hodnotu ("FirstName") "Jméno"; Převede ve formátu camelCase vlastnosti věty skládá z názvu vlastnosti s místem, kde dochází k každý nový velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-219">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="3f7a7-220">V následující kód:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-220">In the following markup:</span></span>

![obrázek](intro/_static/label2.png)

<span data-ttu-id="3f7a7-222">generuje:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-222">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="3f7a7-223">Ve formátu camelCase-k obsahu použita větu nepoužívá, pokud přidáte obsah tak, aby `<label>`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-223">The camel-cased to sentence-cased content is not used if you add content to the `<label>`.</span></span> <span data-ttu-id="3f7a7-224">Příklad:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-224">For example:</span></span>

![obrázek](intro/_static/1stName.png)

<span data-ttu-id="3f7a7-226">generuje:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-226">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="3f7a7-227">Následující obrázek kód ukazuje části formuláře *Views/Account/Register.cshtml* zobrazení syntaxe Razor generované ze starší verze šablony 4.5.x MVC ASP.NET, kde je zahrnutá v sadě Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-227">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![obrázek](intro/_static/regCS.png)

<span data-ttu-id="3f7a7-229">Editoru Visual Studio zobrazí kód C# s šedé pozadí.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-229">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="3f7a7-230">Například `AntiForgeryToken` pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-230">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="3f7a7-231">Zobrazí se šedé pozadí se.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-231">is displayed with a grey background.</span></span> <span data-ttu-id="3f7a7-232">Většinu kódu značek v zobrazení registrace je C#.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-232">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="3f7a7-233">Porovnání, ekvivalentní přístup pomocí pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-233">Compare that to the equivalent approach using Tag Helpers:</span></span>

![obrázek](intro/_static/regTH.png)

<span data-ttu-id="3f7a7-235">Kód je mnohem čisticí a snadněji číst, upravit a udržovat než přístup pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-235">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="3f7a7-236">Kód jazyka C# se snižuje na minimum, který potřebujete vědět o serveru.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-236">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="3f7a7-237">Editoru Visual Studio zobrazí značek cílem pomocné rutiny značky rozlišovací písmeny.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-237">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="3f7a7-238">Vezměte v úvahu *e-mailu* skupiny:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-238">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="3f7a7-239">Jednotlivým atributům "asp-" má hodnotu "E-mailu", ale "E-mailu" není řetězec.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-239">Each of the "asp-" attributes has a value of "Email", but "Email" is not a string.</span></span> <span data-ttu-id="3f7a7-240">V tomto kontextu je "E-mailu" C# výraz vlastnost modelu pro `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-240">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="3f7a7-241">Editoru Visual Studio umožňuje zapsat **všechny** z kódu v metodě značky pomocná registrace formuláře, zatímco Visual Studio poskytuje žádná nápověda pro většinu kódu v metodě pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-241">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="3f7a7-242">[Podporu technologie IntelliSense pro značku Pomocníci](#intellisense-support-for-tag-helpers) obsahuje podrobnosti o práci s Pomocníci značky v editoru Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-242">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="3f7a7-243">Pomocníci značky ve srovnání s ovládací prvky webového serveru</span><span class="sxs-lookup"><span data-stu-id="3f7a7-243">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="3f7a7-244">Pomocníci značky nevlastníte na element, který jste spojené s; jednoduše účastnit při vykreslování element a obsahu.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-244">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="3f7a7-245">ASP.NET [ovládací prvky webového serveru](https://msdn.microsoft.com/library/7698y1f0.aspx) deklarovat a vyvolat na stránce.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-245">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="3f7a7-246">[Ovládací prvky webového serveru](https://msdn.microsoft.com/library/zsyt68f1.aspx) mít netriviální životního cyklu, které mohou ztížit vývoji a ladění.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-246">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="3f7a7-247">Ovládací prvky webového serveru můžete přidat další funkce elementů modelu DOM (Document Object) klient pomocí ovládacího prvku klienta.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-247">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="3f7a7-248">Pomocníci značky mít žádné modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-248">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="3f7a7-249">Ovládací prvky webového serveru zahrnují automatickou detekci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-249">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="3f7a7-250">Pomocníci značky nemají žádné informace o prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-250">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="3f7a7-251">Více Pomocníci značka může fungovat u stejného elementu (v tématu [zabraňující pomocná značky konflikty](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) při obvykle nelze vytvořit ovládací prvky webového serveru.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-251">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="3f7a7-252">Značka pomocné rutiny můžete upravit značku a obsah prvků HTML, kterou jste rozsah, ale nemáte upravit přímo cokoliv jiného na stránce.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-252">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="3f7a7-253">Ovládací prvky webového serveru mají méně konkrétní obor a můžou provádět akce, které ovlivňují dalších částí vaší stránky; povolení nezamýšleným vedlejší účinky.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-253">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="3f7a7-254">Ovládací prvky webového serveru pomocí převaděče typu převod řetězců na objekty.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-254">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="3f7a7-255">S značky podpůrných rutin pracujete nativně v jazyce C#, takže nemusíte typ – převod.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-255">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="3f7a7-256">Ovládací prvky webového serveru pomocí [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) k implementaci chování za běhu a návrhu součásti a ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-256">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="3f7a7-257">`System.ComponentModel`obsahuje základní třídy a rozhraní pro implementace atributů, převaděčů typů vazeb na zdroje dat a licencování součásti.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-257">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="3f7a7-258">Kontrastu, do pomocné rutiny značky, které obvykle odvozena od `TagHelper`a `TagHelper` základní třída zpřístupňuje pouze dvě metody, `Process` a `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="3f7a7-258">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="3f7a7-259">Přizpůsobení písma elementu značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="3f7a7-259">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="3f7a7-260">Můžete přizpůsobit písma a zabarvení z **nástroje** > **možnosti** > **prostředí** > **písem a barvy**:</span><span class="sxs-lookup"><span data-stu-id="3f7a7-260">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![obrázek](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="3f7a7-262">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="3f7a7-262">Additional Resources</span></span>

* [<span data-ttu-id="3f7a7-263">Vytváření Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="3f7a7-263">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="3f7a7-264">Práce s formuláři</span><span class="sxs-lookup"><span data-stu-id="3f7a7-264">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="3f7a7-265">[TagHelperSamples na Githubu](https://github.com/dpaquette/TagHelperSamples) obsahuje ukázky značky Pomocníka pro práci s [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="3f7a7-265">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
