---
title: Pomocné rutiny značek v ASP.NET Core
author: rick-anderson
description: Zjistěte, jaké jsou pomocné rutiny značek a jejich použití v ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 2/14/2018
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: c2af9099fe439e1cdbf9ba86ffae3b2b0f67391e
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751908"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="d4b35-103">Pomocné rutiny značek v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4b35-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="d4b35-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d4b35-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="d4b35-105">Co jsou pomocné rutiny značek?</span><span class="sxs-lookup"><span data-stu-id="d4b35-105">What are Tag Helpers?</span></span>

<span data-ttu-id="d4b35-106">Pomocné rutiny značek povolit kód na straně serveru k účasti na vytváření a vykreslování prvků HTML v souborech Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="d4b35-107">Například předdefinované `ImageTagHelper` číslo verze můžete připojit k názvu image.</span><span class="sxs-lookup"><span data-stu-id="d4b35-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="d4b35-108">Pokaždé, když se změní na obrázku, server vygeneruje nové jedinečné verze pro bitovou kopii, proto je zaručeno, že klienti získat aktuální image (namísto zastaralých bitové kopie v mezipaměti).</span><span class="sxs-lookup"><span data-stu-id="d4b35-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="d4b35-109">Existuje mnoho integrovaných pomocných rutin značek pro běžné úlohy – například vytváření formulářů, odkazy, načítání prostředků a další – a ještě větší počet je dostupný ve veřejných úložištích GitHub a jako NuGet balíčky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="d4b35-110">Pomocné rutiny značek jsou vytvořené v jazyce C# a cílí na základě název elementu, atributu nebo nadřazené značky elementů HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b35-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="d4b35-111">Například předdefinované `LabelTagHelper` HTML můžete cílit na `<label>` element při `LabelTagHelper` atributy jsou použity.</span><span class="sxs-lookup"><span data-stu-id="d4b35-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="d4b35-112">Pokud jste obeznámeni s [pomocných rutin HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), pomocných rutin značek snížit explicitní přechody mezi HTML a C# v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="d4b35-113">V mnoha případech pomocných rutin HTML poskytnout alternativní způsob konkrétní pomocné rutiny značky, ale je důležité uvědomit si, že pomocné rutiny značek nemáte nahradit pomocné rutiny HTML a není k dispozici pomocné rutiny značky pro každý pomocné rutiny HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b35-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="d4b35-114">[Ve srovnání s pomocných rutin HTML pomocné rutiny značky](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly mezi více podrobností.</span><span class="sxs-lookup"><span data-stu-id="d4b35-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="d4b35-115">Co poskytuje pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="d4b35-115">What Tag Helpers provide</span></span>

<span data-ttu-id="d4b35-116">**Vývojové prostředí podporou HTML** pro největší část kódu Razor pomocí pomocných rutin značek vypadá jako standardní HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b35-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="d4b35-117">Front-endu návrháři obeznámen s HTML/CSS a JavaScript můžete upravit Razor bez znalosti syntaxe jazyka C# Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="d4b35-118">**Bohaté prostředí IntelliSense pro vytváření značky HTML a Razor** jde sharp oproti pomocné rutiny HTML, předchozí přístup k serverové vytváření značky v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="d4b35-119">[Ve srovnání s pomocných rutin HTML pomocné rutiny značky](#tag-helpers-compared-to-html-helpers) vysvětluje rozdíly mezi více podrobností.</span><span class="sxs-lookup"><span data-stu-id="d4b35-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="d4b35-120">[Podpora IntelliSense pro pomocné rutiny značek](#intellisense-support-for-tag-helpers) vysvětluje prostředí IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="d4b35-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="d4b35-121">Zaznamenal se syntaxí Razor C# i vývojáře zvýší se produktivita použití pomocných rutin značek než psaní kódu jazyka C# Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="d4b35-122">**Způsob, jak budete produktivnější a nedokáže vytvořit robustnější a spolehlivé a udržovatelný kód pomocí informace, které jsou k dispozici pouze na serveru** v minulosti mantrou na aktualizace bitových kopií byl například chcete-li změnit název obrázku při změně na obrázku.</span><span class="sxs-lookup"><span data-stu-id="d4b35-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="d4b35-123">Obrázky agresivně do mezipaměti kvůli výkonu a pokud změníte název image, riskujete získat kopii zastaralých klientů.</span><span class="sxs-lookup"><span data-stu-id="d4b35-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="d4b35-124">V minulosti po obrázku se upraví, museli změnit název a každý odkaz na obrázek ve webové aplikaci potřeba aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="d4b35-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="d4b35-125">Nejenže je to velmi práce že náročné, je také náchylné k chybám (vám může přijít o odkaz, neúmyslném nesprávný řetězec atd.) Předdefinované `ImageTagHelper` můžete to udělal za vás automaticky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="d4b35-126">`ImageTagHelper` Připojit verze čísla do image s názvem, tak pokaždé, když se změní na obrázku, server automaticky vygeneruje nové jedinečné verze pro bitovou kopii.</span><span class="sxs-lookup"><span data-stu-id="d4b35-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="d4b35-127">Je zaručeno, že klienti získat aktuální image.</span><span class="sxs-lookup"><span data-stu-id="d4b35-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="d4b35-128">Tato úspory odolnosti a práci dodává v podstatě bezplatné pomocí `ImageTagHelper`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="d4b35-129">Nejvíce integrovaných pomocných rutin značek cílit na standardní elementy jazyka HTML a poskytovat na straně serveru atributy pro element.</span><span class="sxs-lookup"><span data-stu-id="d4b35-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="d4b35-130">Například `<input>` použít v mnoha zobrazení v prvku *zobrazení/účet* složka obsahuje `asp-for` atribut.</span><span class="sxs-lookup"><span data-stu-id="d4b35-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="d4b35-131">Tento atribut extrahuje název vlastnosti zadaný model zobrazený HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b35-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="d4b35-132">Vezměte v úvahu zobrazení Razor s modelem následující:</span><span class="sxs-lookup"><span data-stu-id="d4b35-132">Consider a Razor view with the following model:</span></span>

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

<span data-ttu-id="d4b35-133">Následující kód Razor:</span><span class="sxs-lookup"><span data-stu-id="d4b35-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="d4b35-134">Generuje následující kód HTML:</span><span class="sxs-lookup"><span data-stu-id="d4b35-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="d4b35-135">`asp-for` Atribut je k dispozici ve `For` vlastnost [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span><span class="sxs-lookup"><span data-stu-id="d4b35-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d4b35-136">Zobrazit [pomocných rutin značek Autor](xref:mvc/views/tag-helpers/authoring) Další informace.</span><span class="sxs-lookup"><span data-stu-id="d4b35-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="d4b35-137">Správa pomocné rutiny značky oboru</span><span class="sxs-lookup"><span data-stu-id="d4b35-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="d4b35-138">Obor pomocné rutiny značky je řízen pomocí kombinace `@addTagHelper`, `@removeTagHelper`a "!" znak odhlásit.</span><span class="sxs-lookup"><span data-stu-id="d4b35-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="d4b35-139">`@addTagHelper` zpřístupňuje pomocných rutin značek</span><span class="sxs-lookup"><span data-stu-id="d4b35-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="d4b35-140">Pokud vytvoříte novou webovou aplikaci ASP.NET Core s názvem *AuthoringTagHelpers*, následující *Views/_ViewImports.cshtml* soubor bude přidán do projektu:</span><span class="sxs-lookup"><span data-stu-id="d4b35-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers*, the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="d4b35-141">`@addTagHelper` – Direktiva zpřístupní pomocných rutin značek k zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d4b35-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="d4b35-142">V takovém případě je soubor zobrazení *Pages/_ViewImports.cshtml*, která ve výchozím nastavení zdědí všechny soubory v *stránky* složky a podsložky; zpřístupnění pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="d4b35-142">In this case, the view file is *Pages/_ViewImports.cshtml*, which by default is inherited by all files in the *Pages* folder and sub-folders; making Tag Helpers available.</span></span> <span data-ttu-id="d4b35-143">Výše uvedený kód používá syntaxe zástupných znaků ("\*") určíte, že všechny pomocných rutin značek v zadaném sestavení (*Microsoft.AspNetCore.Mvc.TagHelpers*) bude k dispozici pro každý soubor zobrazení v *zobrazení* nebo dílčí adresáři.</span><span class="sxs-lookup"><span data-stu-id="d4b35-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="d4b35-144">První parametr po `@addTagHelper` určuje pomocných rutin značek k načtení (používáme "\*" pro všechny pomocných rutin značek), a druhý parametr "Microsoft.AspNetCore.Mvc.TagHelpers" Určuje sestavení, který obsahuje pomocné rutiny značek.</span><span class="sxs-lookup"><span data-stu-id="d4b35-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="d4b35-145">*Microsoft.AspNetCore.Mvc.TagHelpers* je sestavení pro integrované pomocné rutiny značek základní technologie ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="d4b35-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="d4b35-146">K vystavení všechny pomocných rutin značek v tomto projektu (která vytvoří sestavení s názvem *AuthoringTagHelpers*), můžete využít následující:</span><span class="sxs-lookup"><span data-stu-id="d4b35-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="d4b35-147">Pokud váš projekt obsahuje `EmailTagHelper` s výchozí obor názvů (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), můžete zadat plně kvalifikovaný název (FQN) pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="d4b35-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="d4b35-148">Pomocné rutiny značky do zobrazení pomocí FQN, nejprve přidáte FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="d4b35-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="d4b35-149">Většina vývojářů dávají přednost používání "\*" syntaxe zástupných znaků.</span><span class="sxs-lookup"><span data-stu-id="d4b35-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="d4b35-150">Syntaxe zástupných znaků umožňuje vložit zástupného znaku "\*" jako v FQN příponu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="d4b35-151">Například některé z následující direktivy přinese `EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="d4b35-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="d4b35-152">Jak už bylo zmíněno dříve, přidání `@addTagHelper` direktivu *Views/_ViewImports.cshtml* souboru zpřístupní pomocné rutiny značky na všechny soubory zobrazit v *zobrazení* adresáře a podadresářů.</span><span class="sxs-lookup"><span data-stu-id="d4b35-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="d4b35-153">Můžete použít `@addTagHelper` direktiv v souborech konkrétního zobrazení, pokud chcete vyjádřit výslovný souhlas pro vystavení pomocné rutiny značky do jenom tato zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d4b35-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="d4b35-154">`@removeTagHelper` Odebere pomocných rutin značek</span><span class="sxs-lookup"><span data-stu-id="d4b35-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="d4b35-155">`@removeTagHelper` Má dva stejné parametry jako `@addTagHelper`, a odebere pomocné rutiny značky, který byl dříve přidán.</span><span class="sxs-lookup"><span data-stu-id="d4b35-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="d4b35-156">Například `@removeTagHelper` platí pro konkrétní zobrazení odebere zadané pomocné rutiny značky ze zobrazení.</span><span class="sxs-lookup"><span data-stu-id="d4b35-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="d4b35-157">Pomocí `@removeTagHelper` v *Views/Folder/_ViewImports.cshtml* soubor odebere ze všech zobrazení v zadané pomocné rutiny značky *složky*.</span><span class="sxs-lookup"><span data-stu-id="d4b35-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="d4b35-158">Řízení pomocné rutiny značky oboru *_ViewImports.cshtml* souboru</span><span class="sxs-lookup"><span data-stu-id="d4b35-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="d4b35-159">Můžete přidat *_ViewImports.cshtml* do libovolné složky zobrazení a zobrazení, použije modul direktivy z obou tento soubor a *Views/_ViewImports.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="d4b35-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d4b35-160">Pokud jste přidali prázdná *Views/Home/_ViewImports.cshtml* souboru *Domů* zobrazení, by existovat žádná změna protože *_ViewImports.cshtml* soubor je sčítání.</span><span class="sxs-lookup"><span data-stu-id="d4b35-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="d4b35-161">Žádné `@addTagHelper` direktivy, které přidáte do *Views/Home/_ViewImports.cshtml* souboru (, které nejsou ve výchozím *Views/_ViewImports.cshtml* souboru) by vystavit tyto pomocné rutiny značek k zobrazení pouze v *Domů* složky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="d4b35-162">Výslovné odhlášení z jednotlivých prvků</span><span class="sxs-lookup"><span data-stu-id="d4b35-162">Opting out of individual elements</span></span>

<span data-ttu-id="d4b35-163">Můžete zakázat pomocné rutiny značky na úrovni prvku odhlásit znakem pomocné rutiny značky ("!").</span><span class="sxs-lookup"><span data-stu-id="d4b35-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="d4b35-164">Například `Email` ověření je zakázané v `<span>` odhlásit znakem pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="d4b35-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="d4b35-165">Musíte použít znak odhlásit pomocné rutiny značky pro počáteční a koncovou značku.</span><span class="sxs-lookup"><span data-stu-id="d4b35-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="d4b35-166">(Editoru sady Visual Studio automaticky přidá znak výslovného nesouhlasu s uzavírací značku při přidejte jej do počáteční značka).</span><span class="sxs-lookup"><span data-stu-id="d4b35-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="d4b35-167">Po přidání znak výslovného nesouhlasu s prvkem a pomocné rutiny značky atributy se už nezobrazují rozlišovací písmem.</span><span class="sxs-lookup"><span data-stu-id="d4b35-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="d4b35-168">Pomocí `@tagHelperPrefix` provést explicitní použití pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="d4b35-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="d4b35-169">`@tagHelperPrefix` – Direktiva umožňuje zadat řetězec předpony značky k povolení podpory pomocné rutiny značky a aby explicitní použití pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="d4b35-170">Můžete například přidat následující kód k *Views/_ViewImports.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="d4b35-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="d4b35-171">Na následujícím obrázku kód předpony pomocné rutiny značky nastavena na `th:`, takže pouze elementy pomocí předpony `th:` podporují pomocné rutiny značek (pomocné rutiny značky povolena elementy mají zvláštní písma).</span><span class="sxs-lookup"><span data-stu-id="d4b35-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="d4b35-172">`<label>` a `<input>` elementy mají předponu pomocné rutiny značky a povolenými pomocné rutiny značky při `<span>` není element.</span><span class="sxs-lookup"><span data-stu-id="d4b35-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![obrázek](intro/_static/thp.png)

<span data-ttu-id="d4b35-174">Stejná pravidla hierarchie, které se vztahují `@addTagHelper` platí také pro `@tagHelperPrefix`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="d4b35-175">Podpora IntelliSense pro pomocné rutiny značek</span><span class="sxs-lookup"><span data-stu-id="d4b35-175">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="d4b35-176">Když vytvoříte novou webovou aplikaci ASP.NET Core v sadě Visual Studio, přidá balíček NuGet "Microsoft.AspNetCore.Razor.Tools".</span><span class="sxs-lookup"><span data-stu-id="d4b35-176">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="d4b35-177">Toto je balíček, který přidá nástroje pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-177">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="d4b35-178">Zvažte vytvoření HTML `<label>` elementu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-178">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="d4b35-179">Jakmile zadáte `<l` IntelliSense v editoru sady Visual Studio zobrazí odpovídající prvky:</span><span class="sxs-lookup"><span data-stu-id="d4b35-179">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obrázek](intro/_static/label.png)

<span data-ttu-id="d4b35-181">Nejen že získáte Nápověda jazyka HTML, ale na ikonu ("@" symbol with "<>" je pod ním).</span><span class="sxs-lookup"><span data-stu-id="d4b35-181">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![obrázek](intro/_static/tagSym.png)

<span data-ttu-id="d4b35-183">identifikuje prvek jako cílem pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="d4b35-183">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="d4b35-184">Čistě prvky jazyka HTML (například `fieldset`) zobrazuje ikona "<>".</span><span class="sxs-lookup"><span data-stu-id="d4b35-184">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="d4b35-185">Prostý jazyk HTML `<label>` značky zobrazí značky HTML (s výchozí barevný motiv sady Visual Studio) Hnědý písmem, atributy červeně, a atribut hodnoty modrou barvu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-185">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![obrázek](intro/_static/LableHtmlTag.png)

<span data-ttu-id="d4b35-187">Po zadání `<label`, IntelliSense vypíše dostupné atributy HTML/CSS a atributy cílené pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="d4b35-187">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![obrázek](intro/_static/labelattr.png)

<span data-ttu-id="d4b35-189">Doplňování technologie IntelliSense umožňuje zadat klávesu tab k dokončení příkazu s vybranou hodnotu:</span><span class="sxs-lookup"><span data-stu-id="d4b35-189">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![obrázek](intro/_static/stmtcomplete.png)

<span data-ttu-id="d4b35-191">Poté, co je zadán atribut pomocné rutiny značky, změňte značku a atribut písma.</span><span class="sxs-lookup"><span data-stu-id="d4b35-191">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="d4b35-192">Použití výchozí sady Visual Studio "Blue" nebo "výraz Light" barevný motiv, je písmo tučné nachová.</span><span class="sxs-lookup"><span data-stu-id="d4b35-192">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="d4b35-193">Pokud používáte "Tmavý" motiv je písmo tučné šedozelená.</span><span class="sxs-lookup"><span data-stu-id="d4b35-193">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="d4b35-194">Obrázky v tomto dokumentu se nevytvořily pomocí výchozí motiv.</span><span class="sxs-lookup"><span data-stu-id="d4b35-194">The images in this document were taken using the default theme.</span></span>

![obrázek](intro/_static/labelaspfor2.png)

<span data-ttu-id="d4b35-196">Visual Studio můžete zadat *CompleteWord* místní (Ctrl + mezerník je [výchozí](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) do dvojitých uvozovek (""), a je nyní v jazyce C#, stejně jako by měly být v třída jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="d4b35-196">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="d4b35-197">Technologie IntelliSense zobrazí všechny metody a vlastnosti v modelu stránky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-197">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="d4b35-198">Metody a vlastnosti jsou k dispozici, protože je vlastnost typu `ModelExpression`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-198">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="d4b35-199">Na obrázku níže, můžu jsem úpravy `Register` zobrazení, takže `RegisterViewModel` je k dispozici.</span><span class="sxs-lookup"><span data-stu-id="d4b35-199">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![obrázek](intro/_static/intellemail.png)

<span data-ttu-id="d4b35-201">Technologie IntelliSense jsou uvedené vlastnosti a metody dostupné pro model na stránce.</span><span class="sxs-lookup"><span data-stu-id="d4b35-201">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="d4b35-202">Bohaté prostředí IntelliSense vám pomůže vybrat třídu šablony stylů CSS:</span><span class="sxs-lookup"><span data-stu-id="d4b35-202">The rich IntelliSense environment helps you select the CSS class:</span></span>

![obrázek](intro/_static/iclass.png)

![obrázek](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="d4b35-205">Pomocných rutin značek ve srovnání s pomocných rutin HTML</span><span class="sxs-lookup"><span data-stu-id="d4b35-205">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="d4b35-206">Pomocné rutiny značek připojit k elementů HTML v zobrazení syntaxe Razor, zatímco [pomocných rutin HTML](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) jsou vyvolány jako metody spolu s HTML v zobrazení syntaxe Razor.</span><span class="sxs-lookup"><span data-stu-id="d4b35-206">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="d4b35-207">Vezměte v úvahu následující kód Razor, která vytvoří popisek HTML pomocí třídy šablony stylů CSS "titulek":</span><span class="sxs-lookup"><span data-stu-id="d4b35-207">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="d4b35-208">Na (`@`) symbol říká Razor, je toto začátek kódu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-208">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="d4b35-209">Následující dva parametry ("FirstName" a "jméno:") jsou řetězce, takže [IntelliSense](/visualstudio/ide/using-intellisense) nemůže pomoct.</span><span class="sxs-lookup"><span data-stu-id="d4b35-209">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="d4b35-210">Poslední argument:</span><span class="sxs-lookup"><span data-stu-id="d4b35-210">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="d4b35-211">Je anonymní objekt používá k reprezentování atributy.</span><span class="sxs-lookup"><span data-stu-id="d4b35-211">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="d4b35-212">Protože <strong>třídy</strong> je vyhrazené klíčové slovo v jazyce C#, můžete použít `@` symbolů pro vynucení jazyka C# pro interpretaci "@class=" jako symbol (název vlastnosti).</span><span class="sxs-lookup"><span data-stu-id="d4b35-212">Because <strong>class</strong> is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="d4b35-213">Pro front-endu návrháře (někdo zkušenosti s HTML/CSS a JavaScript a dalších technologií klienta, ale nejsou obeznámeni s C# a Razor), většina řádku je cizí.</span><span class="sxs-lookup"><span data-stu-id="d4b35-213">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="d4b35-214">Celý řádek musí být vytvořen žádný pomocí technologie IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="d4b35-214">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="d4b35-215">Použití `LabelTagHelper`, stejné značky lze zapsat jako:</span><span class="sxs-lookup"><span data-stu-id="d4b35-215">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![obrázek](intro/_static/label2.png)

<span data-ttu-id="d4b35-217">Pomocná rutina značky verzi, jakmile zadáte `<l` IntelliSense v editoru sady Visual Studio zobrazí odpovídající prvky:</span><span class="sxs-lookup"><span data-stu-id="d4b35-217">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![obrázek](intro/_static/label.png)

<span data-ttu-id="d4b35-219">Technologie IntelliSense umožňuje napsat celý řádek.</span><span class="sxs-lookup"><span data-stu-id="d4b35-219">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="d4b35-220">`LabelTagHelper` Také výchozí hodnoty pro nastavení obsah `asp-for` atribut hodnotu ("FirstName") "Jméno"; Převede-ve formátu camelCase vlastnosti větu skládá z názvu vlastnosti s místem, kde dochází k každý nový velké písmeno.</span><span class="sxs-lookup"><span data-stu-id="d4b35-220">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="d4b35-221">V následujícím kódu:</span><span class="sxs-lookup"><span data-stu-id="d4b35-221">In the following markup:</span></span>

![obrázek](intro/_static/label2.png)

<span data-ttu-id="d4b35-223">vygeneruje:</span><span class="sxs-lookup"><span data-stu-id="d4b35-223">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="d4b35-224">-Ve formátu camelCase na obsah věty malými a velkými písmeny se nepoužívá, pokud chcete přidat obsah tak, aby `<label>`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-224">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="d4b35-225">Příklad:</span><span class="sxs-lookup"><span data-stu-id="d4b35-225">For example:</span></span>

![obrázek](intro/_static/1stName.png)

<span data-ttu-id="d4b35-227">vygeneruje:</span><span class="sxs-lookup"><span data-stu-id="d4b35-227">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="d4b35-228">Následující kód obrázek ukazuje část formuláře *Views/Account/Register.cshtml* zobrazení Razor vygenerované ze starší verze šablony 4.5.x MVC ASP.NET, kde je součástí sady Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="d4b35-228">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![obrázek](intro/_static/regCS.png)

<span data-ttu-id="d4b35-230">Editor sady Visual Studio zobrazuje šedé pozadí kód C#.</span><span class="sxs-lookup"><span data-stu-id="d4b35-230">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="d4b35-231">Například `AntiForgeryToken` pomocné rutiny HTML:</span><span class="sxs-lookup"><span data-stu-id="d4b35-231">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="d4b35-232">Zobrazí se šedé pozadí.</span><span class="sxs-lookup"><span data-stu-id="d4b35-232">is displayed with a grey background.</span></span> <span data-ttu-id="d4b35-233">Většina kódu v registru zobrazení je C#.</span><span class="sxs-lookup"><span data-stu-id="d4b35-233">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="d4b35-234">Porovnejte ji s ekvivalentní postup použití pomocných rutin značek:</span><span class="sxs-lookup"><span data-stu-id="d4b35-234">Compare that to the equivalent approach using Tag Helpers:</span></span>

![obrázek](intro/_static/regTH.png)

<span data-ttu-id="d4b35-236">Kód je mnohem přehlednější a čitelnější, upravit a udržovat než pomocných rutin HTML.</span><span class="sxs-lookup"><span data-stu-id="d4b35-236">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="d4b35-237">Kód jazyka C# je snížit na minimum, kterou je potřeba vědět o serveru.</span><span class="sxs-lookup"><span data-stu-id="d4b35-237">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="d4b35-238">Editor sady Visual Studio zobrazí značky cílem pomocné rutiny značky rozlišovací písmem.</span><span class="sxs-lookup"><span data-stu-id="d4b35-238">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="d4b35-239">Vezměte v úvahu *e-mailu* skupiny:</span><span class="sxs-lookup"><span data-stu-id="d4b35-239">Consider the *Email* group:</span></span>

[!code-csharp[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="d4b35-240">Každý atribut "asp-" má hodnotu "Email", ale není řetězce "E-mail".</span><span class="sxs-lookup"><span data-stu-id="d4b35-240">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="d4b35-241">V tomto kontextu "Email" je C# model výrazu vlastnost `RegisterViewModel`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-241">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="d4b35-242">Editor sady Visual Studio vám pomůže psát **všechny** značek v pomocné rutiny značky přístup register formuláře, zatímco Visual Studio poskytuje většinu kódu v metodě pomocných rutin HTML není žádná nápověda.</span><span class="sxs-lookup"><span data-stu-id="d4b35-242">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="d4b35-243">[Podpora IntelliSense pro pomocné rutiny značek](#intellisense-support-for-tag-helpers) obsahuje podrobnosti o práci s pomocných rutin značek v editoru sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d4b35-243">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="d4b35-244">Pomocných rutin značek ve srovnání s ovládací prvky webového serveru</span><span class="sxs-lookup"><span data-stu-id="d4b35-244">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="d4b35-245">Pomocné rutiny značek nevlastníte prvek, na který jste spojené s; jednoduše účast při vykreslování element a obsahu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-245">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="d4b35-246">ASP.NET [ovládací prvky webového serveru](https://msdn.microsoft.com/library/7698y1f0.aspx) deklarovány a vyvolána na stránce.</span><span class="sxs-lookup"><span data-stu-id="d4b35-246">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="d4b35-247">[Ovládací prvky webového serveru](https://msdn.microsoft.com/library/zsyt68f1.aspx) mají netriviální životní cyklus, který může ztěžovat vývoji a ladění.</span><span class="sxs-lookup"><span data-stu-id="d4b35-247">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="d4b35-248">Ovládací prvky webového serveru umožňují přidat funkce na prvky Document Object Model (DOM) klient pomocí ovládacího prvku klienta.</span><span class="sxs-lookup"><span data-stu-id="d4b35-248">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="d4b35-249">Pomocné rutiny značek mít žádné modelu DOM.</span><span class="sxs-lookup"><span data-stu-id="d4b35-249">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="d4b35-250">Ovládací prvky webového serveru zahrnují automatickou detekci prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="d4b35-250">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="d4b35-251">Pomocné rutiny značek nemají žádné informace o prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="d4b35-251">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="d4b35-252">Několik pomocných rutin značek může fungovat u stejného elementu (naleznete v tématu [vyhnout pomocné rutiny značky konfliktů](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) zatímco obvykle nelze vytvořit ovládací prvky webového serveru.</span><span class="sxs-lookup"><span data-stu-id="d4b35-252">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="d4b35-253">Můžete upravit značky a obsah prvků HTML, která jsou omezená na pomocných rutin značek, ale nemusíte upravovat přímo cokoli, na stránce.</span><span class="sxs-lookup"><span data-stu-id="d4b35-253">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="d4b35-254">Ovládací prvky webového serveru mají méně konkrétní rozsah a můžete provádět akce, které ovlivňují ostatní části stránky; povolení nezamýšlenými vedlejšími účinky.</span><span class="sxs-lookup"><span data-stu-id="d4b35-254">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="d4b35-255">Ovládací prvky webového serveru pomocí převaděče typů pro převod řetězců na objekty.</span><span class="sxs-lookup"><span data-stu-id="d4b35-255">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="d4b35-256">Pomocné rutiny značek můžete pracovat nativně v jazyce C#, takže není nutné pro převod typu.</span><span class="sxs-lookup"><span data-stu-id="d4b35-256">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="d4b35-257">Použijte ovládací prvky webového serveru [System.ComponentModel](/dotnet/api/system.componentmodel) k implementaci chování za běhu a návrhu komponent a ovládacích prvků.</span><span class="sxs-lookup"><span data-stu-id="d4b35-257">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="d4b35-258">`System.ComponentModel` obsahuje základní třídy a rozhraní pro implementaci atributy a převaděče typů vazeb na zdroje dat a součásti licencování.</span><span class="sxs-lookup"><span data-stu-id="d4b35-258">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="d4b35-259">Kontrast, který pomocných rutin značek, které obvykle jsou odvozeny z `TagHelper`a `TagHelper` základní třída poskytuje pouze dvě metody `Process` a `ProcessAsync`.</span><span class="sxs-lookup"><span data-stu-id="d4b35-259">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="d4b35-260">Přizpůsobení písma elementu pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="d4b35-260">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="d4b35-261">Můžete přizpůsobit písma a barevné zvýraznění z **nástroje** > **možnosti** > **prostředí** > **písma a barvy**:</span><span class="sxs-lookup"><span data-stu-id="d4b35-261">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![obrázek](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="d4b35-263">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="d4b35-263">Additional resources</span></span>

* [<span data-ttu-id="d4b35-264">Autor pomocných rutin značek</span><span class="sxs-lookup"><span data-stu-id="d4b35-264">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="d4b35-265">Práce s formuláři </span><span class="sxs-lookup"><span data-stu-id="d4b35-265">Working with Forms </span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="d4b35-266">[TagHelperSamples na Githubu](https://github.com/dpaquette/TagHelperSamples) obsahuje pomocné rutiny značky ukázky pro práci s [Bootstrap](http://getbootstrap.com/).</span><span class="sxs-lookup"><span data-stu-id="d4b35-266">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>
