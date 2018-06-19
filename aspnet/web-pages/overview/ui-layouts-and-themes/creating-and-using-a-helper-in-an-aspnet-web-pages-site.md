---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Vytváření a používání pomocné rutiny v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje postup vytvoření pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). Pomocné rutiny je opakovaně použitelné komponenty, která obsahuje kód a značky k výkonu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573304"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="6baf8-104">Vytváření a používání pomocné rutiny v stránku ASP.NET Web Pages (Razor)</span><span class="sxs-lookup"><span data-stu-id="6baf8-104">Creating and Using a Helper in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="6baf8-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="6baf8-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="6baf8-106">Tento článek popisuje postup vytvoření pomocné rutiny na webu technologie ASP.NET Web Pages (Razor).</span><span class="sxs-lookup"><span data-stu-id="6baf8-106">This article describes how to create a helper in an ASP.NET Web Pages (Razor) website.</span></span> <span data-ttu-id="6baf8-107">A *pomocná* je znovu použitelné komponentní, která obsahuje kód a značky provést úlohu, která může být zdlouhavé nebo komplexní.</span><span class="sxs-lookup"><span data-stu-id="6baf8-107">A *helper* is a reusable component that includes code and markup to perform a task that might be tedious or complex.</span></span>
> 
> <span data-ttu-id="6baf8-108">**Získáte informace:**</span><span class="sxs-lookup"><span data-stu-id="6baf8-108">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="6baf8-109">Postup vytvoření a použití jednoduché pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-109">How to create and use a simple helper.</span></span>
> 
> <span data-ttu-id="6baf8-110">Toto jsou nové v článku funkce ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="6baf8-110">These are the ASP.NET features introduced in the article:</span></span>
> 
> - <span data-ttu-id="6baf8-111">`@helper` Syntaxe.</span><span class="sxs-lookup"><span data-stu-id="6baf8-111">The `@helper` syntax.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="6baf8-112">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="6baf8-112">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="6baf8-113">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="6baf8-113">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="6baf8-114">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.</span><span class="sxs-lookup"><span data-stu-id="6baf8-114">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="overview-of-helpers"></a><span data-ttu-id="6baf8-115">Přehled pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="6baf8-115">Overview of Helpers</span></span>

<span data-ttu-id="6baf8-116">Pokud potřebujete k provádění stejných úloh na různých stránkách ve vaší lokalitě, můžete použít pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-116">If you need to perform the same tasks on different pages in your site, you can use a helper.</span></span> <span data-ttu-id="6baf8-117">Rozhraní ASP.NET Web Pages zahrnuje několik pomocné rutiny, a neexistují další, které můžete stáhnout a nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="6baf8-117">ASP.NET Web Pages includes a number of helpers, and there are many more that you can download and install.</span></span> <span data-ttu-id="6baf8-118">(Seznam předdefinovaných Pomocníci na webových stránkách ASP.NET je uvedena ve [Stručná referenční příručka rozhraní API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907).) Pokud žádná z existující Pomocníci nevyhovuje vašim potřebám, můžete vytvořit vlastní pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-118">(A list of the built-in helpers in ASP.NET Web Pages is listed in the [ASP.NET API Quick Reference](https://go.microsoft.com/fwlink/?LinkId=202907).) If none of the existing helpers meet your needs, you can create your own helper.</span></span>

<span data-ttu-id="6baf8-119">Pomocné rutiny vám umožní používat běžné bloku kódu na více stránkách.</span><span class="sxs-lookup"><span data-stu-id="6baf8-119">A helper lets you use a common block of code across multiple pages.</span></span> <span data-ttu-id="6baf8-120">Předpokládejme, že na stránce často chcete vytvořit položku Poznámka, který je nastavený kromě Normální odstavce.</span><span class="sxs-lookup"><span data-stu-id="6baf8-120">Suppose that in your page you often want to create a note item that's set apart from normal paragraphs.</span></span> <span data-ttu-id="6baf8-121">Možná je poznámka vytvořen jako `<div>` element který naformátovat jako pole s ohraničení.</span><span class="sxs-lookup"><span data-stu-id="6baf8-121">Perhaps the note is created as a `<div>` element that's styled as a box with a border.</span></span> <span data-ttu-id="6baf8-122">Místo přidat tento stejný kód na stránku pokaždé, když chcete zobrazit Poznámka, můžete balíček kód jako pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-122">Rather than add this same markup to a page every time you want to display a note, you can package the markup as a helper.</span></span> <span data-ttu-id="6baf8-123">Pak můžete vložit Poznámka s jedním řádkem kódu potřebujete kdekoli.</span><span class="sxs-lookup"><span data-stu-id="6baf8-123">You can then insert the note with a single line of code anywhere you need it.</span></span>

<span data-ttu-id="6baf8-124">Pomocí pomocné rutiny takto mohou kód v jednotlivých stránek, jednoduchost a lepší přehlednost ke čtení.</span><span class="sxs-lookup"><span data-stu-id="6baf8-124">Using a helper like this makes the code in each of your pages simpler and easier to read.</span></span> <span data-ttu-id="6baf8-125">Je také usnadňuje udržovat lokalitu, protože pokud potřebujete změnit vzhled poznámky, můžete změnit kód na jednom místě.</span><span class="sxs-lookup"><span data-stu-id="6baf8-125">It also makes it easier to maintain your site, because if you need to change how the notes look, you can change the markup in one place.</span></span>

## <a name="creating-a-helper"></a><span data-ttu-id="6baf8-126">Vytváření pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="6baf8-126">Creating a Helper</span></span>

<span data-ttu-id="6baf8-127">Tento postup ukazuje, jak vytvořit pomocné rutiny, která vytvoří Všimněte si, jak je popsáno právě.</span><span class="sxs-lookup"><span data-stu-id="6baf8-127">This procedure shows you how to create the helper that creates the note, as just described.</span></span> <span data-ttu-id="6baf8-128">Toto je jednoduchý příklad, ale vlastního pomocného objektu může zahrnovat všechny značek a kódu ASP.NET, které potřebujete.</span><span class="sxs-lookup"><span data-stu-id="6baf8-128">This is a simple example, but the custom helper can include any markup and ASP.NET code that you need.</span></span>

1. <span data-ttu-id="6baf8-129">V kořenové složce webové stránky, vytvořte složku s názvem *aplikace\_kód*.</span><span class="sxs-lookup"><span data-stu-id="6baf8-129">In the root folder of the website, create a folder named *App\_Code*.</span></span> <span data-ttu-id="6baf8-130">Toto je název vyhrazené složky v technologii ASP.NET, kde můžete ukládat kód pro komponenty, jako jsou pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-130">This is a reserved folder name in ASP.NET where you can put code for components like helpers.</span></span>
2. <span data-ttu-id="6baf8-131">V *aplikace\_kód* vytvořte novou složku *.cshtml* souboru a pojmenujte ji *MyHelpers.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6baf8-131">In the *App\_Code* folder create a new *.cshtml* file and name it *MyHelpers.cshtml*.</span></span>
3. <span data-ttu-id="6baf8-132">Nahradí existující obsah s následujícími službami:</span><span class="sxs-lookup"><span data-stu-id="6baf8-132">Replace the existing content with the following:</span></span>

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    <span data-ttu-id="6baf8-133">Kód používá `@helper` syntaxe deklarace nového Pomocníka s názvem `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="6baf8-133">The code uses the `@helper` syntax to declare a new helper named `MakeNote`.</span></span> <span data-ttu-id="6baf8-134">Tato konkrétní Pomocník umožňuje předat parametr s názvem `content` obsahující kombinaci text a značku.</span><span class="sxs-lookup"><span data-stu-id="6baf8-134">This particular helper lets you pass a parameter named `content` that can contain a combination of text and markup.</span></span> <span data-ttu-id="6baf8-135">Pomocné rutiny vloží text do pomocí textu Poznámka `@content` proměnné.</span><span class="sxs-lookup"><span data-stu-id="6baf8-135">The helper inserts the string into the note body using the `@content` variable.</span></span>

    <span data-ttu-id="6baf8-136">Všimněte si, že je soubor s názvem *MyHelpers.cshtml*, ale pomocné rutiny jmenuje `MakeNote`.</span><span class="sxs-lookup"><span data-stu-id="6baf8-136">Notice that the file is named *MyHelpers.cshtml*, but the helper is named `MakeNote`.</span></span> <span data-ttu-id="6baf8-137">Více vlastní pomocné rutiny můžete umístit do jednoho souboru.</span><span class="sxs-lookup"><span data-stu-id="6baf8-137">You can put multiple custom helpers into a single file.</span></span>
4. <span data-ttu-id="6baf8-138">Soubor uložte a zavřete.</span><span class="sxs-lookup"><span data-stu-id="6baf8-138">Save and close the file.</span></span>

## <a name="using-the-helper-in-a-page"></a><span data-ttu-id="6baf8-139">Na stránce využitím pomocné rutiny</span><span class="sxs-lookup"><span data-stu-id="6baf8-139">Using the Helper in a Page</span></span>

1. <span data-ttu-id="6baf8-140">V kořenové složce vytvořte nový prázdný soubor s názvem *TestHelper.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="6baf8-140">In the root folder, create a new blank file called *TestHelper.cshtml*.</span></span>
2. <span data-ttu-id="6baf8-141">Přidejte následující kód do souboru:</span><span class="sxs-lookup"><span data-stu-id="6baf8-141">Add the following code to the file:</span></span>

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    <span data-ttu-id="6baf8-142">K volání pomocné rutiny, které jste vytvořili, použijte `@` následuje název souboru, kde pomocné rutiny jsou, tečku a potom název pomocné rutiny.</span><span class="sxs-lookup"><span data-stu-id="6baf8-142">To call the helper you created, use `@` followed by the file name where the helper is, a dot, and then the helper name.</span></span> <span data-ttu-id="6baf8-143">(Pokud jste měli více složek *aplikace\_kód* složky, můžete použít syntaxi `@FolderName.FileName.HelperName` volat pomáhající v rámci všechny vnořené složky úroveň).</span><span class="sxs-lookup"><span data-stu-id="6baf8-143">(If you had multiple folders in the *App\_Code* folder, you could use the syntax `@FolderName.FileName.HelperName` to call your helper within any nested folder level).</span></span> <span data-ttu-id="6baf8-144">Text, který přidáte do uvozovek v závorkách je text, který pomocníka, který se zobrazí jako součást Poznámka na webové stránce.</span><span class="sxs-lookup"><span data-stu-id="6baf8-144">The text that you add in quotation marks within the parentheses is the text that the helper will display as part of the note in the web page.</span></span>
3. <span data-ttu-id="6baf8-145">Uložit stránky a spusťte ji v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="6baf8-145">Save the page and run it in a browser.</span></span> <span data-ttu-id="6baf8-146">Generuje položku Poznámky právo kde jste volali metodu pomocný pomocné rutiny: mezi dva odstavce.</span><span class="sxs-lookup"><span data-stu-id="6baf8-146">The helper generates the note item right where you called the helper: between the two paragraphs.</span></span>

    ![Snímek obrazovky se stránkou v prohlížeči a způsob, jakým pomocné rutiny vygeneruje kód, který převádí pole kolem zadaný text.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a><span data-ttu-id="6baf8-148">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="6baf8-148">Additional Resources</span></span>


<span data-ttu-id="6baf8-149">[Vodorovné nabídky jako pomocné rutiny Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span><span class="sxs-lookup"><span data-stu-id="6baf8-149">[Horizontal menu as a Razor helper](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341).</span></span> <span data-ttu-id="6baf8-150">Tuto položku blogu podle Karel Pope ukazuje, jak vytvořit vodorovné nabídky jako pomocné rutiny pomocí značek, CSS a kódu.</span><span class="sxs-lookup"><span data-stu-id="6baf8-150">This blog entry by Mike Pope shows how to create a horizontal menu as a helper using markup, CSS, and code.</span></span>

<span data-ttu-id="6baf8-151">[Využití HTML5 v rozhraní ASP.NET Web Pages pomocníci pro službu WebMatrix a ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span><span class="sxs-lookup"><span data-stu-id="6baf8-151">[Leveraging HTML5 in ASP.NET Web Pages Helpers for WebMatrix and ASP.NET MVC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx).</span></span> <span data-ttu-id="6baf8-152">Tuto položku blogu podle Sam Jirásek ukazuje pomocné rutiny, který vykreslí HTML5 `Canvas` elementu.</span><span class="sxs-lookup"><span data-stu-id="6baf8-152">This blog entry by Sam Abraham shows a helper that renders an HTML5 `Canvas` element.</span></span>

<span data-ttu-id="6baf8-153">[Rozdíl mezi @Helpers a @Functions ve službě WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span><span class="sxs-lookup"><span data-stu-id="6baf8-153">[The Difference Between @Helpers and @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix).</span></span> <span data-ttu-id="6baf8-154">Popisuje tato položka blogu podle Karel Brind `@helper` syntaxe a `@function` syntaxe a kdy každý použít.</span><span class="sxs-lookup"><span data-stu-id="6baf8-154">This blog entry by Mike Brind describes `@helper` syntax and `@function` syntax and when to use each.</span></span>
