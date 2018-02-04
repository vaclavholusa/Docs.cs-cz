---
title: "Vytváření Pomocníci značky v ASP.NET Core"
author: rick-anderson
description: "Naučte se vytvářet značky pomocné rutiny v ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/authoring
ms.openlocfilehash: 80426d7825ab9d4f64c12a2feee97b89b5375045
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/03/2018
---
# <a name="author-tag-helpers-in-aspnet-core-a-walkthrough-with-samples"></a><span data-ttu-id="fb6ac-103">Autor značky pomocné rutiny v ASP.NET Core, návod s ukázky</span><span class="sxs-lookup"><span data-stu-id="fb6ac-103">Author Tag Helpers in ASP.NET Core, a walkthrough with samples</span></span>

<span data-ttu-id="fb6ac-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb6ac-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fb6ac-105">[Zobrazit nebo stáhnout ukázkový kód](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([stažení](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fb6ac-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/tag-helpers/authoring/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="get-started-with-tag-helpers"></a><span data-ttu-id="fb6ac-106">Začínáme s značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="fb6ac-106">Get started with Tag Helpers</span></span>

<span data-ttu-id="fb6ac-107">Tento kurz obsahuje úvod do pomocné rutiny programování značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-107">This tutorial provides an introduction to programming Tag Helpers.</span></span> <span data-ttu-id="fb6ac-108">[Úvod do pomocné rutiny značky](intro.md) popisuje výhody, které poskytují pomocné rutiny značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-108">[Introduction to Tag Helpers](intro.md) describes the benefits that Tag Helpers provide.</span></span>

<span data-ttu-id="fb6ac-109">Pomocné rutiny značku je žádné třídu, která implementuje `ITagHelper` rozhraní.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-109">A tag helper is any class that implements the `ITagHelper` interface.</span></span> <span data-ttu-id="fb6ac-110">Ale při vytváření značky pomocné rutiny, obecně odvozujete od `TagHelper`, tak dává vám přístup k provádění `Process` metoda.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-110">However, when you author a tag helper, you generally derive from `TagHelper`, doing so gives you access to the `Process` method.</span></span>

1. <span data-ttu-id="fb6ac-111">Vytvořit nový projekt ASP.NET Core s názvem **AuthoringTagHelpers**.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-111">Create a new ASP.NET Core project called **AuthoringTagHelpers**.</span></span> <span data-ttu-id="fb6ac-112">Ověřování nebudete potřebovat pro tento projekt.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-112">You won't need authentication for this project.</span></span>

2. <span data-ttu-id="fb6ac-113">Vytvořte složku pro uložení Pomocníci značky názvem *TagHelpers*.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-113">Create a folder to hold the Tag Helpers called *TagHelpers*.</span></span> <span data-ttu-id="fb6ac-114">*TagHelpers* složka je *není* požadováno, ale je možné logicky konvence.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-114">The *TagHelpers* folder is *not* required, but it's a reasonable convention.</span></span> <span data-ttu-id="fb6ac-115">Nyní můžeme začít zápis několik jednoduchých značek pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-115">Now let's get started writing some simple tag helpers.</span></span>

## <a name="a-minimal-tag-helper"></a><span data-ttu-id="fb6ac-116">Minimální pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="fb6ac-116">A minimal Tag Helper</span></span>

<span data-ttu-id="fb6ac-117">V této části napíšete značky pomocné rutiny, která aktualizuje značku e-mailu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-117">In this section, you write a tag helper that updates an email tag.</span></span> <span data-ttu-id="fb6ac-118">Příklad:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-118">For example:</span></span>

```html
<email>Support</email>
   ```

<span data-ttu-id="fb6ac-119">Server bude používat naše pomocná značky e-mailu k převedení tohoto kódu do následující:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-119">The server will use our email tag helper to convert that markup into the following:</span></span>

```html
<a href="mailto:Support@contoso.com">Support@contoso.com</a>
```

<span data-ttu-id="fb6ac-120">To znamená značku ukotvení pak tento odkaz e-mailu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-120">That is, an anchor tag that makes this an email link.</span></span> <span data-ttu-id="fb6ac-121">Můžete to udělat, pokud jsou psaní modul blog a potřebovat k odeslání e-mailu pro marketing, podpory a další kontakty, všechny ke stejné doméně.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-121">You might want to do this if you are writing a blog engine and need it to send email for marketing, support, and other contacts, all to the same domain.</span></span>

1.  <span data-ttu-id="fb6ac-122">Přidejte následující `EmailTagHelper` třídy k *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-122">Add the following `EmailTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1EmailTagHelperCopy.cs)]
    
    <span data-ttu-id="fb6ac-123">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="fb6ac-123">**Notes:**</span></span>
    
    * <span data-ttu-id="fb6ac-124">Pomocníci značky použít zásady vytváření názvů zacílený elementy název kořenové třídy (minus *TagHelper* část názvu třídy).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-124">Tag helpers use a naming convention that targets elements of the root class name (minus the *TagHelper* portion of the class name).</span></span> <span data-ttu-id="fb6ac-125">V tomto příkladu název kořenového **e-mailu**TagHelper je *e-mailu*, proto `<email>` budou cílem značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-125">In this example, the root name of **Email**TagHelper is *email*, so the `<email>` tag will be targeted.</span></span> <span data-ttu-id="fb6ac-126">Tyto zásady vytváření názvů by měla fungovat pro většinu značky pomocné rutiny, později na zobrazí postup přepsat.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-126">This naming convention should work for most tag helpers, later on I'll show how to override it.</span></span>
    
    * <span data-ttu-id="fb6ac-127">`EmailTagHelper` Třída odvozená z `TagHelper`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-127">The `EmailTagHelper` class derives from `TagHelper`.</span></span> <span data-ttu-id="fb6ac-128">`TagHelper` Třída poskytuje metody a vlastnosti pro zápis značky pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-128">The `TagHelper` class provides methods and properties for writing Tag Helpers.</span></span>
    
    * <span data-ttu-id="fb6ac-129">Přepsané `Process` ovládací prvky metoda helper značky jaké jsou při spuštění.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-129">The overridden `Process` method controls what the tag helper does when executed.</span></span> <span data-ttu-id="fb6ac-130">`TagHelper` Třída rovněž poskytuje asynchronní verzi (`ProcessAsync`) se stejnými parametry.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-130">The `TagHelper` class also provides an asynchronous version (`ProcessAsync`) with the same parameters.</span></span>
    
    * <span data-ttu-id="fb6ac-131">Kontext parametru pro `Process` (a `ProcessAsync`) obsahuje informace související s prováděním aktuální značky HTML.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-131">The context parameter to `Process` (and `ProcessAsync`) contains information associated with the execution of the current HTML tag.</span></span>
    
    * <span data-ttu-id="fb6ac-132">Výstupní parametr k `Process` (a `ProcessAsync`) obsahuje stavová prvek HTML reprezentativní sloužící ke generování služby značky HTML a obsah původního zdroje.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-132">The output parameter to `Process` (and `ProcessAsync`) contains a stateful HTML element representative of the original source used to generate an HTML tag and content.</span></span>
    
    * <span data-ttu-id="fb6ac-133">Naše název třídy má příponu **TagHelper**, což je *není* požadováno, ale považuje za osvědčený postup konvence.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-133">Our class name has a suffix of **TagHelper**, which is *not* required, but it's considered a best practice convention.</span></span> <span data-ttu-id="fb6ac-134">Může deklarovat třídu jako:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-134">You could declare the class as:</span></span>
    
    ```csharp
    public class Email : TagHelper
    ```

2.  <span data-ttu-id="fb6ac-135">Chcete-li `EmailTagHelper` třídy k dispozici pro všechny naše zobrazení syntaxe Razor, přidejte `addTagHelper` direktivy k *Views/_ViewImports.cshtml* souboru:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span><span class="sxs-lookup"><span data-stu-id="fb6ac-135">To make the `EmailTagHelper` class available to all our Razor views, add the `addTagHelper` directive to the *Views/_ViewImports.cshtml* file: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopyEmail.cshtml?highlight=2,3)]</span></span>
    
    <span data-ttu-id="fb6ac-136">Výše uvedený kód používá syntaxi zástupný znak můžete určit že všechny značky nápovědy v našem sestavení bude k dispozici.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-136">The code above uses the wildcard syntax to specify all the tag helpers in our assembly will be available.</span></span> <span data-ttu-id="fb6ac-137">První řetězec po `@addTagHelper` určuje značky pomocná načíst (použijte "\*" pro všechny značky pomocníky), a druhý řetězec "AuthoringTagHelpers" Určuje sestavení Pomocník značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-137">The first string after `@addTagHelper` specifies the tag helper to load (Use "\*" for all tag helpers), and the second string "AuthoringTagHelpers" specifies the assembly the tag helper is in.</span></span> <span data-ttu-id="fb6ac-138">Všimněte si také, že na druhém řádku přináší v ASP.NET MVC základní pomocníky značky pomocí syntaxe zástupný znak (tyto pomocné rutiny, které jsou popsané v [Úvod do pomocné rutiny značky](intro.md).) Je `@addTagHelper` direktiva, která zpřístupňuje značky pomocné rutiny do zobrazení Razor.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-138">Also, note that the second line brings in the ASP.NET Core MVC tag helpers using the wildcard syntax (those helpers are discussed in [Introduction to Tag Helpers](intro.md).) It's the `@addTagHelper` directive that makes the tag helper available to the Razor view.</span></span> <span data-ttu-id="fb6ac-139">Alternativně můžete zadat plně kvalifikovaný název (FQN) značky Helper, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-139">Alternatively, you can provide the fully qualified name (FQN) of a tag helper as shown below:</span></span>
    
```csharp
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```
    
<!--
the following snippet uses TagHelpers3 and should use TagHelpers (not the 3)
    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImports.cshtml?highlight=3&range=1-3)]
-->
    
<span data-ttu-id="fb6ac-140">Přidání značka pomocné rutiny zobrazení pomocí FQN, je nejprve přidat FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) a potom název sestavení (*AuthoringTagHelpers*).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-140">To add a tag helper to a view using a FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="fb6ac-141">Většina vývojářů bude radši chtěli použít zástupný znak syntaxe.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-141">Most developers will prefer to use the wildcard syntax.</span></span> <span data-ttu-id="fb6ac-142">[Úvod do pomocné rutiny značky](intro.md) přejde do podrobností na přidání, odebrání, hierarchie a zástupný znak syntaxe pomocná značek.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-142">[Introduction to Tag Helpers](intro.md) goes into detail on tag helper adding, removing, hierarchy, and wildcard syntax.</span></span>
    
3.  <span data-ttu-id="fb6ac-143">Aktualizovat kód v *Views/Home/Contact.cshtml* souboru se tyto změny:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-143">Update the markup in the *Views/Home/Contact.cshtml* file with these changes:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

4.  <span data-ttu-id="fb6ac-144">Spusťte aplikaci a pomocí oblíbeném prohlížeči zobrazíte zdrojový kód HTML a ověřte značek e-mailu jsou nahrazeny značka ukotvení (například `<a>Support</a>`).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-144">Run the app and use your favorite browser to view the HTML source so you can verify that the email tags are replaced with anchor markup (For example, `<a>Support</a>`).</span></span> <span data-ttu-id="fb6ac-145">*Podpora* a *Marketing* se vykresluje jako odkazy, ale nemají `href` atribut tak, aby byly funkční.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-145">*Support* and *Marketing* are rendered as a links, but they don't have an `href` attribute to make them functional.</span></span> <span data-ttu-id="fb6ac-146">To jsme budete opravíme v další části.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-146">We'll fix that in the next section.</span></span>

<span data-ttu-id="fb6ac-147">Poznámka: Jako značky HTML a atributy, značky, názvy tříd a atributů v Razor a C# nejsou malá a velká písmena.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-147">Note: Like HTML tags and attributes, tags, class names and attributes in Razor, and C# are not case-sensitive.</span></span>

## <a name="setattribute-and-setcontent"></a><span data-ttu-id="fb6ac-148">SetAttribute a SetContent</span><span class="sxs-lookup"><span data-stu-id="fb6ac-148">SetAttribute and SetContent</span></span>

<span data-ttu-id="fb6ac-149">V této části budete aktualizujeme `EmailTagHelper` tak, aby se vytvoří značku ukotvení platné e-mailu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-149">In this section, we'll update the `EmailTagHelper` so that it will create a valid anchor tag for email.</span></span> <span data-ttu-id="fb6ac-150">Aktualizujeme budete ji tak, aby informace ze zobrazení syntaxe Razor (ve formě `mail-to` atribut) a použít jej v generování ukotvení.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-150">We'll update it to take information from a Razor view (in the form of a `mail-to` attribute) and use that in generating the anchor.</span></span>

<span data-ttu-id="fb6ac-151">Aktualizace `EmailTagHelper` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-151">Update the `EmailTagHelper` class with the following:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?range=6-22)]

<span data-ttu-id="fb6ac-152">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="fb6ac-152">**Notes:**</span></span>

* <span data-ttu-id="fb6ac-153">Jsou použita Pascal třídy a vlastnosti názvy pro značku Pomocníci přeložit na jejich [nižší případ kebab](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-153">Pascal-cased class and property names for tag helpers are translated into their [lower kebab case](https://stackoverflow.com/questions/11273282/whats-the-name-for-dash-separated-case/12273101).</span></span> <span data-ttu-id="fb6ac-154">Proto používat `MailTo` atributů, které budete používat `<email mail-to="value"/>` ekvivalentní.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-154">Therefore, to use the `MailTo` attribute, you'll use `<email mail-to="value"/>` equivalent.</span></span>

* <span data-ttu-id="fb6ac-155">Poslední řádek nastaví dokončené obsah naše pomocníka minimálně funkční značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-155">The last line sets the completed content for our minimally functional tag helper.</span></span>

* <span data-ttu-id="fb6ac-156">Zvýrazněný řádek ukazuje syntaxi pro přidání atributy:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-156">The highlighted line shows the syntax for adding attributes:</span></span>

[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailTo.cs?highlight=6&range=14-21)]

<span data-ttu-id="fb6ac-157">Tento přístup funguje pro atribut "href", dokud aktuálně neexistuje v kolekci atributů.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-157">That approach works for the attribute "href" as long as it doesn't currently exist in the attributes collection.</span></span> <span data-ttu-id="fb6ac-158">Můžete také `output.Attributes.Add` metody Přidat pomocný atribut příznaku na konec kolekce atributů značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-158">You can also use the `output.Attributes.Add` method to add a tag helper attribute to the end of the collection of tag attributes.</span></span>

1.  <span data-ttu-id="fb6ac-159">Aktualizovat kód v *Views/Home/Contact.cshtml* souboru se tyto změny:[!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span><span class="sxs-lookup"><span data-stu-id="fb6ac-159">Update the markup in the *Views/Home/Contact.cshtml* file with these changes: [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/ContactCopy.cshtml?highlight=15,16)]</span></span>

2.  <span data-ttu-id="fb6ac-160">Spusťte aplikaci a ověřte, že vygeneruje správné odkazy.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-160">Run the app and verify that it generates the correct links.</span></span>
    
    > [!NOTE]
    ><span data-ttu-id="fb6ac-161">Pokud byste chtěli zápis, e-mailu značky samoobslužné zavření (`<email mail-to="Rick" />`), by také být samouzavírací finální výstup.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-161">If you were to write the email tag self-closing (`<email mail-to="Rick" />`), the final output would also be self-closing.</span></span> <span data-ttu-id="fb6ac-162">Chcete-li povolit možnost zapisovat značky s počáteční značky (`<email mail-to="Rick">`) musí uspořádání třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-162">To enable the ability to write the tag with only a start tag (`<email mail-to="Rick">`) you must decorate the class with the following:</span></span>
    >
    > [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelperMailVoid.cs?highlight=1&range=6-10)]
    
    <span data-ttu-id="fb6ac-163">S samouzavírací značky pomocníka e-mailu, bude výstup `<a href="mailto:Rick@contoso.com" />`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-163">With a self-closing email tag helper, the output would be `<a href="mailto:Rick@contoso.com" />`.</span></span> <span data-ttu-id="fb6ac-164">Samouzavírací značky ukotvení nejsou platné HTML, proto byste neměli chtít vytvořit, ale můžete chtít vytvořit značku pomocné rutiny, která je samouzavírací.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-164">Self-closing anchor tags are not valid HTML, so you wouldn't want to create one, but you might want to create a tag helper that's self-closing.</span></span> <span data-ttu-id="fb6ac-165">Pomocníci značky nastavit typ `TagMode` vlastnost po přečtení značku.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-165">Tag helpers set the type of the `TagMode` property after reading a tag.</span></span>
    
### <a name="processasync"></a><span data-ttu-id="fb6ac-166">ProcessAsync</span><span class="sxs-lookup"><span data-stu-id="fb6ac-166">ProcessAsync</span></span>

<span data-ttu-id="fb6ac-167">V této části jsme budete zápisu Pomocník asynchronní e-mailu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-167">In this section, we'll write an asynchronous email helper.</span></span>

1.  <span data-ttu-id="fb6ac-168">Nahraďte `EmailTagHelper` třídy následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-168">Replace the `EmailTagHelper` class with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/EmailTagHelper.cs?range=6-17)]

    <span data-ttu-id="fb6ac-169">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="fb6ac-169">**Notes:**</span></span>

    * <span data-ttu-id="fb6ac-170">Tato verze používá asynchronní `ProcessAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-170">This version uses the asynchronous `ProcessAsync` method.</span></span> <span data-ttu-id="fb6ac-171">Asynchronní `GetChildContentAsync` vrátí `Task` obsahující `TagHelperContent`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-171">The asynchronous `GetChildContentAsync` returns a `Task` containing the `TagHelperContent`.</span></span>

    * <span data-ttu-id="fb6ac-172">Použití `output` parametr načíst obsah elementu HTML.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-172">Use the `output` parameter to get contents of the HTML element.</span></span>

2.  <span data-ttu-id="fb6ac-173">Proveďte následující změny k *Views/Home/Contact.cshtml* , značka pomocné rutiny můžete získat e-mailu, cílový soubor.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-173">Make the following change to the *Views/Home/Contact.cshtml* file so the tag helper can get the target email.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=15,16&range=1-17)]

3.  <span data-ttu-id="fb6ac-174">Spusťte aplikaci a ověřte, že se generuje odkazy platnou e-mailovou.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-174">Run the app and verify that it generates valid email links.</span></span>

### <a name="removeall-precontentsethtmlcontent-and-postcontentsethtmlcontent"></a><span data-ttu-id="fb6ac-175">RemoveAll, PreContent.SetHtmlContent a PostContent.SetHtmlContent</span><span class="sxs-lookup"><span data-stu-id="fb6ac-175">RemoveAll, PreContent.SetHtmlContent and PostContent.SetHtmlContent</span></span>

1.  <span data-ttu-id="fb6ac-176">Přidejte následující `BoldTagHelper` třídy k *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-176">Add the following `BoldTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/BoldTagHelper.cs)]

    <span data-ttu-id="fb6ac-177">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="fb6ac-177">**Notes:**</span></span>
    
    * <span data-ttu-id="fb6ac-178">`[HtmlTargetElement]` Atribut předává atribut parametr, který určuje, že libovolný prvek HTML, která obsahuje atribut HTML s názvem "bold" bude odpovídat, a `Process` přepsání metody ve třídě, poběží.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-178">The `[HtmlTargetElement]` attribute passes an attribute parameter that specifies that any HTML element that contains an HTML attribute named "bold" will match, and the `Process` override method in the class will run.</span></span> <span data-ttu-id="fb6ac-179">Ve výběru `Process` metoda odebere atribut "bold" a obklopuje kód obsahující s `<strong></strong>`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-179">In our sample, the `Process` method removes the "bold" attribute and surrounds the containing markup with `<strong></strong>`.</span></span>
    
    * <span data-ttu-id="fb6ac-180">Vzhledem k tomu, že nechcete nahradit stávající značky obsahu, musíte napsat otevření `<strong>` značku s `PreContent.SetHtmlContent` metoda a zavření `</strong>` značku s `PostContent.SetHtmlContent` metoda.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-180">Because you don't want to replace the existing tag content, you must write the opening `<strong>` tag with the `PreContent.SetHtmlContent` method and the closing `</strong>` tag with the `PostContent.SetHtmlContent` method.</span></span>
    
2.  <span data-ttu-id="fb6ac-181">Změnit *About.cshtml* zobrazení tak, aby obsahovala `bold` hodnota atributu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-181">Modify the *About.cshtml* view to contain a `bold` attribute value.</span></span> <span data-ttu-id="fb6ac-182">Dokončený kód je uveden níže.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-182">The completed code is shown below.</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutBoldOnly.cshtml?highlight=7)]

3.  <span data-ttu-id="fb6ac-183">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-183">Run the app.</span></span> <span data-ttu-id="fb6ac-184">Oblíbeném prohlížeči můžete použít ke kontrole zdroji a ověřit kód.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-184">You can use your favorite browser to inspect the source and verify the markup.</span></span>

    <span data-ttu-id="fb6ac-185">`[HtmlTargetElement]` Výše uvedený atribut pouze cílem jazyka HTML, který poskytuje název atributu z "bold".</span><span class="sxs-lookup"><span data-stu-id="fb6ac-185">The `[HtmlTargetElement]` attribute above only targets HTML markup that provides an attribute name of "bold".</span></span> <span data-ttu-id="fb6ac-186">`<bold>` Element nebyla upravena podle značky pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-186">The `<bold>` element wasn't modified by the tag helper.</span></span>

4. <span data-ttu-id="fb6ac-187">Komentář `[HtmlTargetElement]` atribut řádku a použije výchozí cílení na `<bold>` značky, který je značka jazyka HTML ve formátu `<bold>`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-187">Comment out the `[HtmlTargetElement]` attribute line and it will default to targeting `<bold>` tags, that is, HTML markup of the form `<bold>`.</span></span> <span data-ttu-id="fb6ac-188">Pamatujte si, že výchozí zásady vytváření názvů bude shodovat s názvem třídy **Bold**TagHelper k `<bold>` značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-188">Remember, the default naming convention will match the class name **Bold**TagHelper to `<bold>` tags.</span></span>

5. <span data-ttu-id="fb6ac-189">Spusťte aplikaci a ověřte, zda `<bold>` značky zpracovává pomocná značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-189">Run the app and verify that the `<bold>` tag is processed by the tag helper.</span></span>

<span data-ttu-id="fb6ac-190">Architekturu třídu s více `[HtmlTargetElement]` atributy výsledky v logické nebo cíle.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-190">Decorating a class with multiple `[HtmlTargetElement]` attributes results in a logical-OR of the targets.</span></span> <span data-ttu-id="fb6ac-191">Například pomocí kódu níže, tučné značky nebo tučné atribut bude odpovídat.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-191">For example, using the code below, a bold tag or a bold attribute will match.</span></span>

[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zBoldTagHelperCopy.cs?highlight=1,2&range=5-15)]

<span data-ttu-id="fb6ac-192">Po přidání více atributů stejný příkaz, modul runtime je považuje za logickým operátorem a.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-192">When multiple attributes are added to the same statement, the runtime treats them as a logical-AND.</span></span> <span data-ttu-id="fb6ac-193">Například následující kód HTML element musí mít název "bold" s atributem s názvem "bold" (`<bold bold />`) tak, aby odpovídaly.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-193">For example, in the code below, an HTML element must be named "bold" with an attribute named "bold" (`<bold bold />`) to match.</span></span>

```csharp
[HtmlTargetElement("bold", Attributes = "bold")]
   ```

<span data-ttu-id="fb6ac-194">Můžete také `[HtmlTargetElement]` Chcete-li změnit název cílové elementu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-194">You can also use the `[HtmlTargetElement]` to change the name of the targeted element.</span></span> <span data-ttu-id="fb6ac-195">Například pokud jste chtěli `BoldTagHelper` cíl `<MyBold>` značky, byste použili následující atribut:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-195">For example if you wanted the `BoldTagHelper` to target `<MyBold>` tags, you would use the following attribute:</span></span>

```csharp
[HtmlTargetElement("MyBold")]
   ```

## <a name="pass-a-model-to-a-tag-helper"></a><span data-ttu-id="fb6ac-196">Model předat značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="fb6ac-196">Pass a model to a Tag Helper</span></span>

1.  <span data-ttu-id="fb6ac-197">Přidat *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-197">Add a *Models* folder.</span></span>

2.  <span data-ttu-id="fb6ac-198">Přidejte následující `WebsiteContext` třídy k *modely* složky:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-198">Add the following `WebsiteContext` class to the *Models* folder:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Models/WebsiteContext.cs)]

3.  <span data-ttu-id="fb6ac-199">Přidejte následující `WebsiteInformationTagHelper` třídy k *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-199">Add the following `WebsiteInformationTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/WebsiteInformationTagHelper.cs)]
    
    <span data-ttu-id="fb6ac-200">**Poznámky:**</span><span class="sxs-lookup"><span data-stu-id="fb6ac-200">**Notes:**</span></span>
    
    * <span data-ttu-id="fb6ac-201">Jak je uvedeno nahoře, značka pomocné překládá názvy tříd použita Pascal C# a vlastnosti pro značku pomocné rutiny do [nižší případ kebab](http://wiki.c2.com/?KebabCase).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-201">As mentioned previously, tag helpers translates Pascal-cased C# class names and properties for tag helpers into [lower kebab case](http://wiki.c2.com/?KebabCase).</span></span> <span data-ttu-id="fb6ac-202">Proto používat `WebsiteInformationTagHelper` v Razor, napíšete `<website-information />`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-202">Therefore, to use the `WebsiteInformationTagHelper` in Razor, you'll write `<website-information />`.</span></span>
    
    * <span data-ttu-id="fb6ac-203">Cílový element s nejsou explicitně identifikace `[HtmlTargetElement]` atribut, tak výchozí `website-information` budou cílem.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-203">You are not explicitly identifying the target element with the `[HtmlTargetElement]` attribute, so the default of `website-information` will be targeted.</span></span> <span data-ttu-id="fb6ac-204">Pokud jste provedli následující atribut (Poznámka: není kebab případ ale odpovídá názvu třídy):</span><span class="sxs-lookup"><span data-stu-id="fb6ac-204">If you applied the following attribute (note it's not kebab case but matches the class name):</span></span>
    
    ```csharp
    [HtmlTargetElement("WebsiteInformation")]
    ```
    
    <span data-ttu-id="fb6ac-205">Nižší případu značky kebab `<website-information />` nebude shodovat.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-205">The lower kebab case tag `<website-information />` wouldn't match.</span></span> <span data-ttu-id="fb6ac-206">Pokud chcete použít `[HtmlTargetElement]` atribut byste použili kebab případu, jak je uvedeno níže:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-206">If you want use the `[HtmlTargetElement]` attribute, you would use kebab case as shown below:</span></span>
    
    ```csharp
    [HtmlTargetElement("Website-Information")]
    ```
    
    * <span data-ttu-id="fb6ac-207">Prvky, které jsou samouzavírací nemají žádný obsah.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-207">Elements that are self-closing have no content.</span></span> <span data-ttu-id="fb6ac-208">V tomto příkladu kódu Razor používat samouzavírací značky, ale bude vytvářet pomocná značky [části](http://www.w3.org/TR/html5/sections.html#the-section-element) element (který není samouzavírací a jsou zápisu obsahu uvnitř `section` element).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-208">For this example, the Razor markup will use a self-closing tag, but the tag helper will be creating a [section](http://www.w3.org/TR/html5/sections.html#the-section-element) element (which isn't self-closing and you are writing content inside the `section` element).</span></span> <span data-ttu-id="fb6ac-209">Proto je nutné nastavit `TagMode` k `StartTagAndEndTag` k zápisu výstupu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-209">Therefore, you need to set `TagMode` to `StartTagAndEndTag` to write output.</span></span> <span data-ttu-id="fb6ac-210">Alternativně můžete Zakomentovat řádek nastavení `TagMode` a zapsat značku s ukončovací značku.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-210">Alternatively, you can comment out the line setting `TagMode` and write markup with a closing tag.</span></span> <span data-ttu-id="fb6ac-211">(Příklad značek je zadáno později v tomto kurzu).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-211">(Example markup is provided later in this tutorial.)</span></span>
    
    * <span data-ttu-id="fb6ac-212">`$` (Dolaru) na následujícím řádku používá [interpolované řetězce](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span><span class="sxs-lookup"><span data-stu-id="fb6ac-212">The `$` (dollar sign) in the following line uses an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings):</span></span>
    
    ```cshtml
    $@"<ul><li><strong>Version:</strong> {Info.Version}</li>
    ```

4.  <span data-ttu-id="fb6ac-213">Přidejte následující kód do *About.cshtml* zobrazení.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-213">Add the following markup to the *About.cshtml* view.</span></span> <span data-ttu-id="fb6ac-214">Zvýrazněná značka zobrazí informace o tomto webu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-214">The highlighted markup displays the web site information.</span></span>
    
    [!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?highlight=1,12-999)]
    
    >[!NOTE]
    > <span data-ttu-id="fb6ac-215">V kódu Razor, vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-215">In the Razor markup shown below:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/About.cshtml?range=13-17)]
    > 
    ><span data-ttu-id="fb6ac-216">Zná Razor `info` atribut je třída, není řetězec, a vy chcete napsat kód C#.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-216">Razor knows the `info` attribute is a class, not a string, and you want to write C# code.</span></span> <span data-ttu-id="fb6ac-217">Zasílány žádné pomocný atribut příznaku jiné než řetězec bez `@` znak.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-217">Any non-string tag helper attribute should be written without the `@` character.</span></span>
    
5.  <span data-ttu-id="fb6ac-218">Spusťte aplikaci a přejděte do zobrazení o chcete zobrazit informace o webovém serveru.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-218">Run the app, and navigate to the About view to see the web site information.</span></span>

    >[!NOTE]
    ><span data-ttu-id="fb6ac-219">Můžete použít následující kód s uzavírací značku a odebrat řádek s `TagMode.StartTagAndEndTag` v pomocná značky:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-219">You can use the following markup with a closing tag and remove the line with `TagMode.StartTagAndEndTag` in the tag helper:</span></span>
    >
    >[!code-html[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/AboutNotSelfClosing.cshtml?range=13-18)]

## <a name="condition-tag-helper"></a><span data-ttu-id="fb6ac-220">Podmínka vyhodnocena jako značka pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="fb6ac-220">Condition Tag Helper</span></span>

<span data-ttu-id="fb6ac-221">Pomocník značky podmínku vykreslí výstup, když uplyne hodnotu true.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-221">The condition tag helper renders output when passed a true value.</span></span>

1.  <span data-ttu-id="fb6ac-222">Přidejte následující `ConditionTagHelper` třídy k *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-222">Add the following `ConditionTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/ConditionTagHelper.cs)]

2.  <span data-ttu-id="fb6ac-223">Nahraďte obsah *Views/Home/Index.cshtml* soubor s následující kód:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-223">Replace the contents of the *Views/Home/Index.cshtml* file with the following markup:</span></span>

    ```cshtml
    @using AuthoringTagHelpers.Models
    @model WebsiteContext
    
    @{
        ViewData["Title"] = "Home Page";
    }
    
    <div>
        <h3>Information about our website (outdated):</h3>
        <website-information info=@Model />
        <div condition="@Model.Approved">
            <p>
                This website has <strong surround="em"> @Model.Approved </strong> been approved yet.
                Visit www.contoso.com for more information.
            </p>
        </div>
    </div>
    ```
    
3.  <span data-ttu-id="fb6ac-224">Nahraďte `Index` metoda v `Home` řadiče následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-224">Replace the `Index` method in the `Home` controller with the following code:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Controllers/HomeController.cs?range=9-18)]

4.  <span data-ttu-id="fb6ac-225">Spusťte aplikaci a přejděte na domovskou stránku.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-225">Run the app and browse to the home page.</span></span> <span data-ttu-id="fb6ac-226">Kód v podmínku `div` nebude vykreslen.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-226">The markup in the conditional `div` won't be rendered.</span></span> <span data-ttu-id="fb6ac-227">Připojit řetězec dotazu `?approved=true` na adresu URL (například `http://localhost:1235/Home/Index?approved=true`).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-227">Append the query string `?approved=true` to the URL (for example, `http://localhost:1235/Home/Index?approved=true`).</span></span> <span data-ttu-id="fb6ac-228">`approved`je nastavena na hodnotu true a podmíněného zobrazí značek.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-228">`approved` is set to true and the conditional markup will be displayed.</span></span>

>[!NOTE]
><span data-ttu-id="fb6ac-229">Použití [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor zadat atribut, který se cílové místo zadání řetězec, jako jste to udělali s pomocnou rutinou tučné značky:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-229">Use the [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator to specify the attribute to target rather than specifying a string as you did with the bold tag helper:</span></span>
>
>[!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/zConditionTagHelperCopy.cs?highlight=1,2,5&range=5-18)]
>
><span data-ttu-id="fb6ac-230">[Nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operátor bude chránit kód by se někdy být rozdělili (chceme změnit název, který má `RedCondition`).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-230">The [nameof](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/nameof) operator will protect the code should it ever be refactored (we might want to change the name to `RedCondition`).</span></span>

### <a name="avoid-tag-helper-conflicts"></a><span data-ttu-id="fb6ac-231">Nedocházelo ke konfliktům značky pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="fb6ac-231">Avoid Tag Helper conflicts</span></span>

<span data-ttu-id="fb6ac-232">V této části napíšete pár automatické propojení Pomocníci značky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-232">In this section, you write a pair of auto-linking tag helpers.</span></span> <span data-ttu-id="fb6ac-233">První nahradí značek, který obsahuje adresu URL začínající HTTP na HTML anchor značky obsahující stejnou adresu URL (a proto je odkaz na adresu URL).</span><span class="sxs-lookup"><span data-stu-id="fb6ac-233">The first will replace markup containing a URL starting with HTTP to an HTML anchor tag containing the same URL (and thus yielding a link to the URL).</span></span> <span data-ttu-id="fb6ac-234">Druhý bude totéž proveďte pro adresu URL začínající WWW.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-234">The second will do the same for a URL starting with WWW.</span></span>

<span data-ttu-id="fb6ac-235">Protože tyto dvě pomocné rutiny jsou úzce související a je může v budoucnu Refaktorovat, jsme budete zachovat ve stejném souboru.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-235">Because these two helpers are closely related and you may refactor them in the future, we'll keep them in the same file.</span></span>

1.  <span data-ttu-id="fb6ac-236">Přidejte následující `AutoLinkerHttpTagHelper` třídy k *TagHelpers* složky.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-236">Add the following `AutoLinkerHttpTagHelper` class to the *TagHelpers* folder.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=7-19)]

    >[!NOTE]
    ><span data-ttu-id="fb6ac-237">`AutoLinkerHttpTagHelper` Třídy cíle `p` elementy a používá [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) vytvořit ukotvení.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-237">The `AutoLinkerHttpTagHelper` class targets `p` elements and uses [Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expression-language-quick-reference) to create the anchor.</span></span>

2.  <span data-ttu-id="fb6ac-238">Přidejte následující kód do konce *Views/Home/Contact.cshtml* souboru:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-238">Add the following markup to the end of the *Views/Home/Contact.cshtml* file:</span></span>

    [!code-html[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/Home/Contact.cshtml?highlight=19)]

3.  <span data-ttu-id="fb6ac-239">Spusťte aplikaci a ověřte pomocná značky správně vykreslí ukotvení.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-239">Run the app and verify that the tag helper renders the anchor correctly.</span></span>

4.  <span data-ttu-id="fb6ac-240">Aktualizace `AutoLinker` třída zahrnout `AutoLinkerWwwTagHelper` který převede www text značku ukotvení, který také obsahuje původní text www.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-240">Update the `AutoLinker` class to include the `AutoLinkerWwwTagHelper` which will convert www text to an anchor tag that also contains the original www text.</span></span> <span data-ttu-id="fb6ac-241">Aktualizovaný kódu je označený níže:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-241">The updated code is highlighted below:</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?highlight=15-34&range=7-34)]

5.  <span data-ttu-id="fb6ac-242">Spusťte aplikaci.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-242">Run the app.</span></span> <span data-ttu-id="fb6ac-243">Všimněte si www text je vykreslen jako odkaz, ale nemá HTTP text.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-243">Notice the www text is rendered as a link but the HTTP text isn't.</span></span> <span data-ttu-id="fb6ac-244">Když vložíte přerušení v obou třídy, se zobrazí pomocná třída značky HTTP nejprve spustí.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-244">If you put a break point in both classes, you can see that the HTTP tag helper class runs first.</span></span> <span data-ttu-id="fb6ac-245">Problém je, že pomocná výstup značky se uloží do mezipaměti a při spuštění Pomocníka značky WWW přepíše uložené v mezipaměti výstup z pomocníka značky HTTP.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-245">The problem is that the tag helper output is cached, and when the WWW tag helper is run, it overwrites the cached output from the HTTP tag helper.</span></span> <span data-ttu-id="fb6ac-246">Později v tomto kurzu vidíte postup řízení značky Pomocníci spustit v pořadí.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-246">Later in the tutorial we'll see how to control the order that tag helpers run in.</span></span> <span data-ttu-id="fb6ac-247">Kód jsme budete oprava s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-247">We'll fix the code with the following:</span></span>

    [!code-csharp[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10,21,22,26&range=8-37)]

    >[!NOTE]
    ><span data-ttu-id="fb6ac-248">V prvním vydání značky pomocníků automatické propojení vy máte obsah cílového následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-248">In the first edition of the auto-linking tag helpers, you got the content of the target with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinker.cs?range=12)]
    >
    ><span data-ttu-id="fb6ac-249">To znamená, že zavoláte `GetChildContentAsync` pomocí `TagHelperOutput` předaný do `ProcessAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-249">That is, you call `GetChildContentAsync` using the `TagHelperOutput` passed into the `ProcessAsync` method.</span></span> <span data-ttu-id="fb6ac-250">Jak je uvedeno dříve, protože výstup se uloží do mezipaměti, poslední označit pomocná rutina pro spuštění služby wins.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-250">As mentioned previously, because the output is cached, the last tag helper to run wins.</span></span> <span data-ttu-id="fb6ac-251">Vyřešeny, že problém s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-251">You fixed that problem with the following code:</span></span>
    >
    >[!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?range=34-35)]
    >
    ><span data-ttu-id="fb6ac-252">Výše uvedený kód kontroluje, zda změnil obsah, a pokud ano, získá obsah z výstupní vyrovnávací paměť.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-252">The code above checks to see if the content has been modified, and if it has, it gets the content from the output buffer.</span></span>

6.  <span data-ttu-id="fb6ac-253">Spusťte aplikaci a ověřte, že dva odkazy fungují podle očekávání.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-253">Run the app and verify that the two links work as expected.</span></span> <span data-ttu-id="fb6ac-254">Když se může objevit, že je naše automaticky linkeru značky pomocná správnosti a úplnosti, má jemně problém.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-254">While it might appear our auto linker tag helper is correct and complete, it has a subtle problem.</span></span> <span data-ttu-id="fb6ac-255">Pokud se spustí Pomocník značky WWW první, nebudou webové odkazy správné.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-255">If the WWW tag helper runs first, the www links won't be correct.</span></span> <span data-ttu-id="fb6ac-256">Aktualizujte kód tak, že přidáte `Order` přetížení řídit pořadí spuštěnou ve značce.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-256">Update the code by adding the `Order` overload to control the order that the tag runs in.</span></span> <span data-ttu-id="fb6ac-257">`Order` Vlastnost určuje pořadí zpracování relativně k další Pomocníci značky cílení na stejného elementu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-257">The `Order` property determines the execution order relative to other tag helpers targeting the same element.</span></span> <span data-ttu-id="fb6ac-258">Výchozí hodnota pořadí je nulová a instancí s nižšími hodnotami jsou nejprve spustit.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-258">The default order value is zero and instances with lower values are executed first.</span></span>

    [!code-csharp[Main](authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z2AutoLinkerCopy.cs?highlight=5,6,7,8&range=8-15)]
    
    <span data-ttu-id="fb6ac-259">Výše uvedený kód bude zaručit, pomocná značky HTTP spuštěná před pomocná značky WWW.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-259">The above code will guarantee that the HTTP tag helper runs before the WWW tag helper.</span></span> <span data-ttu-id="fb6ac-260">Změna `Order` k `MaxValue` a ověřte, zda kód vygeneruje pro značku WWW je nesprávný.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-260">Change `Order` to `MaxValue` and verify that the markup generated for the WWW tag is incorrect.</span></span>

## <a name="inspect-and-retrieve-child-content"></a><span data-ttu-id="fb6ac-261">Zkontrolujte a načítat obsah podřízené</span><span class="sxs-lookup"><span data-stu-id="fb6ac-261">Inspect and retrieve child content</span></span>

<span data-ttu-id="fb6ac-262">Pomocníci značky poskytují několik vlastností, které k načtení obsahu.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-262">The tag helpers provide several properties to retrieve content.</span></span>

-  <span data-ttu-id="fb6ac-263">Výsledek `GetChildContentAsync` můžete připojit k `output.Content`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-263">The result of `GetChildContentAsync` can be appended to `output.Content`.</span></span>
-  <span data-ttu-id="fb6ac-264">Si můžete prohlédnout výsledek `GetChildContentAsync` s `GetContent`.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-264">You can inspect the result of `GetChildContentAsync` with `GetContent`.</span></span>
-  <span data-ttu-id="fb6ac-265">Pokud změníte `output.Content`, těle TagHelper nebude proveden nebo vykresluje Pokud zavoláte `GetChildContentAsync` jako naše ukázka linkeru automaticky:</span><span class="sxs-lookup"><span data-stu-id="fb6ac-265">If you modify `output.Content`, the TagHelper body won't be executed or rendered unless you call `GetChildContentAsync` as in our auto-linker sample:</span></span>

[!code-csharp[Main](../../views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/TagHelpers/z1AutoLinkerCopy.cs?highlight=5,6,10&range=8-21)]

-  <span data-ttu-id="fb6ac-266">Více volá, aby se `GetChildContentAsync` vrací stejnou hodnotu a nebude znovu spustit `TagHelper` body Pokud předáte hodnotu false parametr označující nepoužívat výsledky uložené v mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="fb6ac-266">Multiple calls to `GetChildContentAsync` returns the same value and doesn't re-execute the `TagHelper` body unless you pass in a false parameter indicating not to use the cached result.</span></span>
