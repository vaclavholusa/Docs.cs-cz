---
title: Autor pomocných rutin značek v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvářet pomocných rutin značek v ASP.NET Core.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/20/2018
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 3bad02c650c717b33386f028cb223d14c0a34ff9
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477628"
---
# <a name="author-tag-helpers-in-aspnet-core"></a><span data-ttu-id="7c0ca-103">Autor pomocných rutin značek v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c0ca-103">Author Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="7c0ca-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7c0ca-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7c0ca-105">[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7c0ca-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="7c0ca-106">Začínáme s pomocných rutin značek</span><span class="sxs-lookup"><span data-stu-id="7c0ca-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="7c0ca-107">Tento kurz obsahuje úvod do programování pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="7c0ca-108">[Úvod do pomocné rutiny značek](intro.md) popisuje výhody, které poskytují pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="7c0ca-109">Pomocná rutina značky se jakékoli třídy, která implementuje `ITagHelper` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="7c0ca-110">Ale při vytváření pomocné rutiny značky obecně odvozujete od `TagHelper`, to tedy dává vám přístup k `Process` metoda.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="7c0ca-111">Vytvořit nový projekt ASP.NET Core s názvem **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="7c0ca-112">Nemusíte ověřování pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="7c0ca-113">Vytvořte složku pro uložení pomocné rutiny značek volá *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="7c0ca-114">*TagHelpers* složka je *není* povinné, ale je rozumné konvence.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="7c0ca-115">Nyní můžeme začít zápis pomocné rutiny některé jednoduché značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="7c0ca-116">Minimální pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="7c0ca-116">A minimal Tag Helper</span></span>

<span data-ttu-id="7c0ca-117">V této části napíšete pomocné rutiny značky, která aktualizuje značku e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="7c0ca-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-118">For example:</span></span>

```html
<email>Support</email>
```

<span data-ttu-id="7c0ca-119">Server bude používat naše pomocné rutiny značky e-mailu převést do následujících značek:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="7c0ca-120">To znamená značku ukotvení díky tomu se tento odkaz na e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="7c0ca-121">Můžete to provést, pokud píšete modul blogu a potřebovat k odeslání e-mailu pro marketing, podpory a další kontakty, všechny ke stejné doméně.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1. <span data-ttu-id="7c0ca-122">Přidejte následující `EmailTagHelper` třídu *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
   <span data-ttu-id="7c0ca-123">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-123">**Notes:**</span></span>
    
   * <span data-ttu-id="7c0ca-124">Použít zásady vytváření názvů, který cílí na prvky názvu kořenové třídy pomocných rutin značek (minus *Taghelperu* část názvu třídy).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="7c0ca-125">V tomto příkladu názvem kořenového **e-mailu**Taghelperu. je *e-mailu*, takže `<email>` budou cílem značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="7c0ca-126">Tyto zásady vytváření názvů by mělo fungovat pro většinu pomocných rutin značek, později ukážeme postup jeho přepsání.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
   * <span data-ttu-id="7c0ca-127">`EmailTagHelper` Třída odvozena z `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="7c0ca-128">`TagHelper` Třída poskytuje metody a vlastnosti pro zápis pomocné rutiny značek.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
   * <span data-ttu-id="7c0ca-129">Přepsané `Process` metodu ovládá, co dělá pomocné rutiny značky při spuštění.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="7c0ca-130">`TagHelper` Třída rovněž poskytuje asynchronní verze (`ProcessAsync`) se stejnými parametry.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
   * <span data-ttu-id="7c0ca-131">Kontextový parametr pro `Process` (a `ProcessAsync`) obsahuje informace o přidružené k provádění aktuální značku HTML.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
   * <span data-ttu-id="7c0ca-132">Výstupní parametr `Process` (a `ProcessAsync`) obsahuje stavové prvek HTML reprezentativní původního zdroje sloužící ke generování značky HTML a obsah služby.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
   * <span data-ttu-id="7c0ca-133">Naše název třídy má příponu **Taghelperu.**, což je *není* povinné, ale se považuje za osvědčených postupů konvence.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="7c0ca-134">Můžete deklarovat třídu jako:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-134">You could declare the class as:</span></span>
    
   ```csharp
   public class Email : TagHelper
   ```

2. <span data-ttu-id="7c0ca-135">Chcete-li `EmailTagHelper` , do třídy k dispozici pro všechny naše zobrazeními Razor `addTagHelper` direktivu *Views/_ViewImports.cshtml* souboru: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="7c0ca-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
   <span data-ttu-id="7c0ca-136">Výše uvedený kód pomocí syntaxe zástupných znaků určuje, že všechny pomocných rutin značek v našich sestavení bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="7c0ca-137">První řetězec, který po `@addTagHelper` určuje pomocné rutiny značky k načtení (použijte "\*" pro všechny pomocných rutin značek), a druhý řetězec "AuthoringTagHelpers" Určuje sestavení, je pomocná rutina značek v.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="7c0ca-138">Všimněte si také, že druhý řádek přináší pomocných rutin značek ASP.NET Core MVC pomocí syntaxe zástupných znaků (tyto pomocné rutiny jsou popsány v [Úvod do pomocné rutiny značek](intro.md).) Je `@addTagHelper` direktiva, která zpřístupní pomocné rutiny značky do zobrazení Razor.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="7c0ca-139">Alternativně je můžete zadat plně kvalifikovaný název (FQN) pomocné rutiny značky jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="7c0ca-140">Pomocné rutiny značky do zobrazení pomocí FQN, nejprve přidáte FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="7c0ca-141">Většina vývojářů budou chtít použít syntaxe zástupných znaků.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="7c0ca-142">[Úvod do pomocné rutiny značek](intro.md) obsahuje podrobnosti o syntaxi pro přidání, odebrání, hierarchie a zástupný znak pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3. <span data-ttu-id="7c0ca-143">Aktualizace značky *Views/Home/Contact.cshtml* soubor se tyto změny:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4. <span data-ttu-id="7c0ca-144">Spusťte aplikaci a použijte svůj oblíbený prohlížeč Chcete-li zobrazit zdrojový kód HTML to vám umožní ověřit e-mailu značky jsou nahrazeny ukotvení značky (například `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="7c0ca-145">*Podpora* a *marketingové* jsou vykresleny jako odkazy, ale nebudou mít `href` atribut, aby se daly funkční.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="7c0ca-146">To Změníme v další části.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-146">We'll fix that in the next section.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="7c0ca-147">SetAttribute a SetContent</span><span class="sxs-lookup"><span data-stu-id="7c0ca-147">SetAttribute and SetContent</span></span>

<span data-ttu-id="7c0ca-148">V této části, aktualizujeme `EmailTagHelper` tak, aby se vytvoří platné značky pro e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-148">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="7c0ca-149">K provedení informace ze zobrazení Razor aktualizujeme (ve formě `mail-to` atribut), který budete používat při generování ukotvení.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-149">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="7c0ca-150">Aktualizace `EmailTagHelper` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-150">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="7c0ca-151">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-151">**Notes:**</span></span>

* <span data-ttu-id="7c0ca-152">Jazyka Pascal – třídy a vlastnosti názvy pro pomocné rutiny značek jsou přeloženy do jejich [snížit kebab případ](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-152">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="7c0ca-153">Proto použít `MailTo` atributu, budete používat `<email mail-to="value"/>` ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-153">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="7c0ca-154">Poslední řádek nastaví dokončené obsah pro naše pomocné rutiny značky minimálně funkční.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-154">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="7c0ca-155">Zvýrazněný řádek ukazuje syntaxi pro přidávání atributů:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-155">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="7c0ca-156">Tento postup funguje u atributu "href" tak dlouho, dokud aktuálně neexistuje v kolekci atributů.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-156">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="7c0ca-157">Můžete také použít `output.Attributes.Add` způsob, jak přidat na konec kolekce atributy značky pomocný atribut příznaku.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-157">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1. <span data-ttu-id="7c0ca-158">Aktualizace značky *Views/Home/Contact.cshtml* soubor se tyto změny: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="7c0ca-158">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2. <span data-ttu-id="7c0ca-159">Spusťte aplikaci a ověřte, že se vygeneruje správné odkazy.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-159">Run the app and verify that it generates the correct links.</span></span>

<a name="self-closing"></a>
    
   > [!NOTE]
   > <span data-ttu-id="7c0ca-160">Pokud byste chtěli zapsat e-mailu značky samouzavírací (`<email mail-to="Rick" />`), by také být samouzavírací závěrečný výstup.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-160">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="7c0ca-161">Povolit možnost zapisovat značka s počáteční značce (`<email mail-to="Rick">`) musí uspořádání třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-161">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
   > 
   > [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
   <span data-ttu-id="7c0ca-162">S samouzavírací e-mailu pomocné rutiny značky, výstup by měl `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-162">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="7c0ca-163">Samouzavírací značky ukotvení nejsou platné HTML, proto není vhodné vytvořit, ale můžete chtít vytvořit pomocné rutiny značky, který je samouzavírací.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-163">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="7c0ca-164">Nastavení typu pomocných rutin značek `TagMode` vlastnost po přečtení značku.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-164">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="7c0ca-165">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="7c0ca-165">ProcessAsync</span></span>

<span data-ttu-id="7c0ca-166">V této části budeme psát pomocné rutiny asynchronní e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-166">In this section, we'll write an asynchronous email helper.</span></span>

1. <span data-ttu-id="7c0ca-167">Nahraďte `EmailTagHelper` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-167">Replace the `EmailTagHelper` class with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

   <span data-ttu-id="7c0ca-168">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-168">**Notes:**</span></span>

   * <span data-ttu-id="7c0ca-169">Tato verze používá asynchronní `ProcessAsync` metody.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-169">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="7c0ca-170">Asynchronní `GetChildContentAsync` vrátí `Task` obsahující `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-170">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

   * <span data-ttu-id="7c0ca-171">Použití `output` parametr načíst obsah elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-171">Use the `output` parameter to get contents of the HTML element.</span></span>

2. <span data-ttu-id="7c0ca-172">Proveďte následující změnu *Views/Home/Contact.cshtml* souboru pomocné rutiny značky můžete získat cílové e-mailu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-172">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3. <span data-ttu-id="7c0ca-173">Spusťte aplikaci a ověřte, že generuje odkazy platnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-173">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="7c0ca-174">RemoveAll, PreContent.SetHtmlContent a PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="7c0ca-174">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1. <span data-ttu-id="7c0ca-175">Přidejte následující `BoldTagHelper` třídu *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-175">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

   <span data-ttu-id="7c0ca-176">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-176">**Notes:**</span></span>
    
   * <span data-ttu-id="7c0ca-177">`[HtmlTargetElement]` Atribut předá parametr atributu, který určuje, že libovolný prvek HTML, který obsahuje atribut HTML s názvem "bold" bude odpovídat, a `Process` spustí metodu přepsání metody ve třídě.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-177">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="7c0ca-178">V naší ukázce `Process` metoda odebere atribut "bold" a obklopuje obsahující kód s `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-178">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
   * <span data-ttu-id="7c0ca-179">Vzhledem k tomu, že nechcete nahradit existující značky obsahu, je nutné napsat otevírání `<strong>` označit `PreContent.SetHtmlContent` metoda a uzavírací `</strong>` označit `PostContent.SetHtmlContent` – metoda.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-179">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2. <span data-ttu-id="7c0ca-180">Upravit *About.cshtml* zobrazení tak, aby obsahovala `bold` hodnotu atributu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-180">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="7c0ca-181">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-181">The completed code is shown below.</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3. <span data-ttu-id="7c0ca-182">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-182">Run the app.</span></span> <span data-ttu-id="7c0ca-183">Svůj oblíbený prohlížeč můžete použít ke kontrole zdroji a ověřte kód.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-183">You can use your favorite browser to inspect the source and verify the markup.</span></span>

   <span data-ttu-id="7c0ca-184">`[HtmlTargetElement]` Výše uvedený atribut se zaměřuje pouze kód HTML, který obsahuje název atributu "tučného písma".</span><span class="sxs-lookup"><span data-stu-id="7c0ca-184">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="7c0ca-185">`<bold>` Prvek nebyl změněn pomocí pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-185">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="7c0ca-186">Okomentujte `[HtmlTargetElement]` tak, aby se výchozí atribut řádku a `<bold>` značky, to znamená, že kód HTML ve formátu `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-186">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="7c0ca-187">Nezapomeňte, že výchozí zásady vytváření názvů bude shodovat s názvem třídy **tučné**Taghelperu. k `<bold>` značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-187">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="7c0ca-188">Spusťte aplikaci a ověřte, zda `<bold>` značka je zpracován pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-188">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="7c0ca-189">Upravení třídy s několika `[HtmlTargetElement]` atributy výsledky v logického operátoru OR cílů.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-189">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="7c0ca-190">Například pomocí níže uvedeného kódu, odpovídat tučné značku nebo atribut tučného písma.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-190">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="7c0ca-191">Při přidávání více atributů na stejný příkaz, modul runtime považuje za ně logickým operátorem a.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-191">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="7c0ca-192">Například v následujícím kódu HTML elementu musí mít název "bold" s atributem s názvem "bold" (`<bold bold />`) tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-192">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="7c0ca-193">Můžete také použít `[HtmlTargetElement]` můžete změnit název cílový element.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-193">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="7c0ca-194">Například pokud jste chtěli `BoldTagHelper` k cíli `<MyBold>` značky, můžete využít následující atribut:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-194">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="7c0ca-195">Předání modelu do pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="7c0ca-195">Pass a model to a Tag Helper</span></span>

1. <span data-ttu-id="7c0ca-196">Přidat *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-196">Add a *Models* folder.</span></span>

2. <span data-ttu-id="7c0ca-197">Přidejte následující `WebsiteContext` třídu *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-197">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3. <span data-ttu-id="7c0ca-198">Přidejte následující `WebsiteInformationTagHelper` třídu *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-198">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
   <span data-ttu-id="7c0ca-199">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="7c0ca-199">**Notes:**</span></span>
    
   * <span data-ttu-id="7c0ca-200">Jak už bylo zmíněno dříve, pomocných rutin značek převádí názvy tříd-jazyka Pascal – C# a vlastnosti pro pomocné rutiny značky do [snížit kebab případ](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-200">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="7c0ca-201">Proto použít `WebsiteInformationTagHelper` v Razor, napíšete `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-201">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
   * <span data-ttu-id="7c0ca-202">Nejsou explicitně určení cílového elementu s `[HtmlTargetElement]` atribut, tak výchozí `website-information` budou cílem.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-202">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="7c0ca-203">Pokud jste provedli následující atribut (Poznámka: není případ kebab ale odpovídá názvu třídy):</span><span class="sxs-lookup"><span data-stu-id="7c0ca-203">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
   ```csharp
   [HtmlTargetElement("WebsiteInformation")]
   ```
    
   <span data-ttu-id="7c0ca-204">Nižší kebab případu značka `<website-information />` shodě.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-204">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="7c0ca-205">Pokud chcete použít `[HtmlTargetElement]` atribut, můžete využít kebab případu, jak je znázorněno níže:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-205">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
   ```csharp
   [HtmlTargetElement("Website-Information")]
   ```
    
   * <span data-ttu-id="7c0ca-206">Prvky, které jsou samouzavírací nemají žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-206">Elements that are self-closing have no content.</span></span> <span data-ttu-id="7c0ca-207">V tomto příkladu kód Razor používat samouzavírací značky, ale pomocné rutiny značky vytvářet [části](http://www.w3.org/TR/html5/sections.html#the-section-element) – element (což není samouzavírací a vy píšete obsah uvnitř `section` element).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-207">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="7c0ca-208">Proto je potřeba nastavit `TagMode` k `StartTagAndEndTag` k zápisu výstupu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-208">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="7c0ca-209">Alternativně můžete Zakomentovat řádek nastavení `TagMode` a psát kód s koncovou značku.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-209">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="7c0ca-210">(Příklad kódu je zadáno později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-210">(Example markup is provided later in this tutorial.)</span></span>
    
   * <span data-ttu-id="7c0ca-211">`$` (Znak dolaru) na následujícím řádku používá [interpolovaný řetězec](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="7c0ca-211">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
   ```cshtml
   $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
   ```

4. <span data-ttu-id="7c0ca-212">Přidejte následující kód k *About.cshtml* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-212">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="7c0ca-213">Zvýrazněná značka se zobrazí informace webu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-213">The highlighted markup displays the web site information.</span></span>
    
   [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
   > [!NOTE]
   > <span data-ttu-id="7c0ca-214">V kódu Razor, vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-214">In the Razor markup shown below:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
   > 
   > <span data-ttu-id="7c0ca-215">Razor ví, `info` atribut je třída, není řetězec, a že chcete napsat kód jazyka C#.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-215">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="7c0ca-216">Žádné pomocný atribut příznaku jiné než řetězec by měl napsat bez `@` znak.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-216">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5. <span data-ttu-id="7c0ca-217">Spusťte aplikaci a přejděte do zobrazení o zobrazíte informace o webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-217">Run the app, and navigate to the About view to see the web site information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="7c0ca-218">Můžete použít následující kód s koncovou značku a odebrání řádku s `TagMode.StartTagAndEndTag` v pomocné rutiny značky:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-218">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
   > 
   > [!code-html[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="7c0ca-219">Podmínka vyhodnocena jako pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="7c0ca-219">Condition Tag Helper</span></span>

<span data-ttu-id="7c0ca-220">Pomocná rutina značky podmínku vykreslí výstup při předání hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-220">The condition tag helper renders output when passed a true value.</span></span>

1. <span data-ttu-id="7c0ca-221">Přidejte následující `ConditionTagHelper` třídu *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-221">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2. <span data-ttu-id="7c0ca-222">Nahraďte obsah *Views/Home/Index.cshtml* souboru následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-222">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Index.cshtml)]

3. <span data-ttu-id="7c0ca-223">Nahradit `Index` metoda ve `Home` řadiče s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-223">Replace the `Index` method in the `Home` controller with the following code:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4. <span data-ttu-id="7c0ca-224">Spusťte aplikaci a přejděte na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-224">Run the app and browse to the home page.</span></span> <span data-ttu-id="7c0ca-225">Kód v podmíněnou `div` se nevykreslí.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-225">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="7c0ca-226">Připojte řetězec dotazu `?approved=true` na adresu URL (například `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-226">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="7c0ca-227">`approved` je nastavena na hodnotu true a podmíněným zobrazí značky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-227">`approved` is set to true and the conditional markup will be displayed.</span></span>

> [!NOTE]
> <span data-ttu-id="7c0ca-228">Použití [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor zadat atribut na cílové místo zadání řetězce, jako jste to udělali s pomocné rutiny tučné značky:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-228">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
> 
> [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
> 
> <span data-ttu-id="7c0ca-229">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor bude chránit kód byste je někdy možné Refaktorovat (chceme změnit název, který má `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-229">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="7c0ca-230">Nedocházelo ke konfliktům pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="7c0ca-230">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="7c0ca-231">V této části napíšete pár automatické propojování pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-231">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="7c0ca-232">První nahradí značek obsahující adresy URL začínající HTTP do HTML anchor značky obsahují stejnou adresu URL (a tedy získávání odkaz na adresu URL).</span><span class="sxs-lookup"><span data-stu-id="7c0ca-232">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="7c0ca-233">Druhá bude totéž proveďte pro adresu URL začínající WWW.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-233">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="7c0ca-234">Protože tyto dvě pomocné rutiny jsou úzce souvisí a je může v budoucnu Refaktorovat, budeme je ve stejném souboru.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-234">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1. <span data-ttu-id="7c0ca-235">Přidejte následující `AutoLinkerHttpTagHelper` třídu *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-235">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

   >[!NOTE]
   ><span data-ttu-id="7c0ca-236">`AutoLinkerHttpTagHelper` Třídy cíle `p` prvků a používá [regulární výraz](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) vytvořit ukotvení.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-236">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2. <span data-ttu-id="7c0ca-237">Přidejte následující kód do konce *Views/Home/Contact.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-237">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

   [!code-html[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3. <span data-ttu-id="7c0ca-238">Spusťte aplikaci a ověřte, že pomocné rutiny značky správně vykresluje ukotvení.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-238">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4. <span data-ttu-id="7c0ca-239">Aktualizace `AutoLinker` třídy, aby obsahoval `AutoLinkerWwwTagHelper` www text který se převede na značku ukotvení, obsahující původní text www.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-239">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="7c0ca-240">Aktualizovaný kód je zvýrazněn níže:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-240">The updated code is highlighted below:</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5. <span data-ttu-id="7c0ca-241">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-241">Run the app.</span></span> <span data-ttu-id="7c0ca-242">Všimněte si, že www text je vykreslen jako odkaz, ale není HTTP text.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-242">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="7c0ca-243">Když vložíte přerušení v obou třídách, uvidíte, že třída pomocné rutiny značky HTTP spustí první.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-243">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="7c0ca-244">Problém je, že výstup pomocné rutiny značky do mezipaměti, a při spuštění pomocné rutiny značky WWW přepíše uložené v mezipaměti výstup z pomocné rutiny značky protokolu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-244">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="7c0ca-245">V pozdější části kurzu uvidíme, jak řídit pořadí, ve kterém pomocných rutin značek spustit v.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-245">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="7c0ca-246">Opravíme kód následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-246">We'll fix the code with the following:</span></span>

   [!code-csharp[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

   > [!NOTE]
   > <span data-ttu-id="7c0ca-247">V první verzi pomocných rutin značek automatické propojování obdržela obsah cíle s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-247">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
   > 
   > <span data-ttu-id="7c0ca-248">To znamená, že zavoláte `GetChildContentAsync` pomocí `TagHelperOutput` předán `ProcessAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-248">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="7c0ca-249">Jak bylo zmíněno dříve, protože je v mezipaměti výstupu, poslední značku pomocné rutiny pro spuštění služby wins.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-249">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="7c0ca-250">Jste opravili tohoto problému s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-250">You fixed that problem with the following code:</span></span>
   > 
   > [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
   > 
   > <span data-ttu-id="7c0ca-251">Výše uvedený kód zkontroluje, zda byl změněn obsah, a pokud ano, získá obsah z výstupní vyrovnávací paměť.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-251">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6. <span data-ttu-id="7c0ca-252">Spusťte aplikaci a ověřte, že dva odkazy fungovat podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-252">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="7c0ca-253">Může se zdát, že je naše pomocné rutiny značky linkeru automaticky správnosti a úplnosti, to je drobný problém.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-253">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="7c0ca-254">Pokud se spustí první pomocné rutiny značky WWW, webové odkazy nebudou správná.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-254">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="7c0ca-255">Aktualizovat kód tak, že přidáte `Order` přetížení k řízení, na kterém běží značky v pořadí.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-255">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="7c0ca-256">`Order` Vlastnost určuje pořadí zpracování relativně k cílení na stejný prvek jiných pomocných rutin značek.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-256">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="7c0ca-257">Výchozí hodnota pořadí je nula a instancí s nižšími hodnotami jsou nejprve spuštěn.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-257">The default order value is zero and instances with lower values are executed first.</span></span>

   [!code-csharp[](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
   <span data-ttu-id="7c0ca-258">Výše uvedený kód zaručí, že pomocné rutiny značky HTTP spuštěn dříve, než pomocné rutiny značky WWW.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-258">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="7c0ca-259">Změna `Order` k `MaxValue` a ověřte, zda je nesprávný kód vygeneruje pro značku WWW.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-259">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="7c0ca-260">Kontrolovat a načítat obsah podřízených</span><span class="sxs-lookup"><span data-stu-id="7c0ca-260">Inspect and retrieve child content</span></span>

<span data-ttu-id="7c0ca-261">Pomocné rutiny značky poskytují několik vlastností k načtení obsahu.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-261">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="7c0ca-262">Výsledek `GetChildContentAsync` lze připojit k `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-262">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="7c0ca-263">Můžete si prohlédnout výsledek `GetChildContentAsync` s `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-263">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="7c0ca-264">Pokud upravíte `output.Content`, tělo Taghelperu. nebudou provedeny nebo vykreslení Pokud zavoláte `GetChildContentAsync` stejně jako v naší ukázce automaticky linkeru:</span><span class="sxs-lookup"><span data-stu-id="7c0ca-264">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="7c0ca-265">Více volání `GetChildContentAsync` vrací stejnou hodnotu a nebude znovu spouštět `TagHelper` subjektu, pokud nepředáte v false parametr určující nepoužívat výsledky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="7c0ca-265">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
