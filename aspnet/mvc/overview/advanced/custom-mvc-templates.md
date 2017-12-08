---
uid: mvc/overview/advanced/custom-mvc-templates
title: "MVC vlastní šablony | Microsoft Docs"
author: joeloff
description: "Vytvořte šablonu jako rozšíření VSIX."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: a1fe1844e582f402a1eed9ddf10ee249e856b083
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="custom-mvc-template"></a><span data-ttu-id="efa4a-103">Šablona vlastní MVC</span><span class="sxs-lookup"><span data-stu-id="efa4a-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="efa4a-104">podle [Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="efa4a-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="efa4a-105">Verze MVC 3 nástroje aktualizace pro sadu Visual Studio 2010 zavedl samostatné projektu průvodce pro projekty MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="efa4a-106">Tato změna byla doprovází dva faktory.</span><span class="sxs-lookup"><span data-stu-id="efa4a-106">The change was driven by two factors.</span></span> <span data-ttu-id="efa4a-107">Nejprve zavedení nové šablony MVC 3 a podpora pro moduly další zobrazení, jako je například Razor způsobit zatarasování dialogové okno Nový projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efa4a-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="efa4a-108">Druhý zákazníci měli požadující body rozšiřitelnosti a Průvodce vytvořením projektu MVC by poskytnout, nám příležitost odpovědět na tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="efa4a-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="efa4a-109">Přidání vlastních šablon se náročnou proces, který spoléhali na pomocí klíče registru, aby byly nové šablony viditelné pro Průvodce projektem MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="efa4a-110">Vytvořit nové šablony museli zabalení uvnitř MSI zajistit, že nezbytné položky registru by se vytvořily během instalace.</span><span class="sxs-lookup"><span data-stu-id="efa4a-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="efa4a-111">Alternativním bylo zkontrolujte soubor ZIP obsahující dostupné šablony a mít koncový uživatel ručně vytvořit požadované položky registru.</span><span class="sxs-lookup"><span data-stu-id="efa4a-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="efa4a-112">Žádná z výše uvedených přístupy je ideální, takže jsme se rozhodli využít některé z existující infrastruktury služby [VSIX](https://msdn.microsoft.com/en-us/library/ff363239.aspx) rozšíření, aby bylo snazší autorovi, distribuce a nainstalovat vlastní šablony MVC počínaje MVC 4 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="efa4a-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/en-us/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="efa4a-113">Jsou některé výhody tohoto přístupu:</span><span class="sxs-lookup"><span data-stu-id="efa4a-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="efa4a-114">Rozšíření VSIX může obsahovat několik šablon, které podporují různé jazyky (C# a Visual Basic) a více moduly zobrazení (ASPX a Razor).</span><span class="sxs-lookup"><span data-stu-id="efa4a-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="efa4a-115">Rozšíření VSIX, můžete vybrat více SKU aplikace Visual Studio včetně Express SKU.</span><span class="sxs-lookup"><span data-stu-id="efa4a-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="efa4a-116">[Galerie sady Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) usnadňuje distribuci rozšíření cílové.</span><span class="sxs-lookup"><span data-stu-id="efa4a-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="efa4a-117">Lze upgradovat VSIX rozšíření usnadňují vytváření opravy a aktualizace vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efa4a-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="efa4a-118">Prerequisites</span></span>

- <span data-ttu-id="efa4a-119">Uživatelé musí být obeznámeni s vytváření šablon projektu, včetně požadované značky pro vstemplate – soubory atd.</span><span class="sxs-lookup"><span data-stu-id="efa4a-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="efa4a-120">Uživatelé budou muset mít Visual Studio Professional a vyšší nainstalován.</span><span class="sxs-lookup"><span data-stu-id="efa4a-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="efa4a-121">Expresní SKU nepodporují vytváření projektů VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="efa4a-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) nainstalována.</span><span class="sxs-lookup"><span data-stu-id="efa4a-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="efa4a-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="efa4a-123">Example</span></span>

<span data-ttu-id="efa4a-124">Prvním krokem je vytvoření nového projektu VSIX pomocí jazyka C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="efa4a-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="efa4a-125">Vyberte **soubor > Nový projekt**, pak klikněte na tlačítko **rozšiřitelnost** v levém podokně a vyberte **projektu VSIX**.</span><span class="sxs-lookup"><span data-stu-id="efa4a-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![Nový projekt](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="efa4a-127">Po vytvoření projektu se otevře návrháře VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-127">After the project is created, the VSIX designer will be opened.</span></span>

![Metadata Návrhář projektu](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="efa4a-129">Návrháře lze upravit některé obecné vlastnosti rozšíření, který se bude zobrazovat uživatelům při instalaci rozšíření nebo procházet nainstalovaná rozšíření v sadě Visual Studio (**nástroje > rozšíření a aktualizace**).</span><span class="sxs-lookup"><span data-stu-id="efa4a-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="efa4a-130">Po dokončení klikněte na na obecné informace **nainstalovat cíle karta**.</span><span class="sxs-lookup"><span data-stu-id="efa4a-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![Cíle projektu návrháře instalace](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="efa4a-132">Na této kartě lze určit SKU a verzí sady Visual Studio, které jsou podporovány vaší rozšíření.</span><span class="sxs-lookup"><span data-stu-id="efa4a-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="efa4a-133">Zaškrtněte políčko **tento VSIX je instalována pro všechny uživatele** povolit instalaci na počítač nástroje VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="efa4a-134">Klikněte na **nový** tlačítko na pravé straně, chcete-li přidat další SKU například Web Developer Express (VWD).</span><span class="sxs-lookup"><span data-stu-id="efa4a-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![Přidat nový cíl instalace](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="efa4a-136">Pokud máte v úmyslu podporovat všechny Professional a vyšší skladové jednotky (Professional, Premium a Ultimate) potřebujete jenom v dané rodině, vyberte minimální SKU **Microsoft.VisualStudio.Pro**.</span><span class="sxs-lookup"><span data-stu-id="efa4a-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="efa4a-137">Nezapomeňte uložit všechny změny, po dokončení instalace cíle.</span><span class="sxs-lookup"><span data-stu-id="efa4a-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![Cíle projektu návrháře instalace](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="efa4a-139">**Prostředky** karta slouží k přidání všechny soubory obsahu do VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="efa4a-140">Vzhledem k tomu, že rozhraní MVC vyžaduje vlastních metadat je upravována nezpracované XML souboru manifestu VSIX místo použití **prostředky** kartě přidání obsahu.</span><span class="sxs-lookup"><span data-stu-id="efa4a-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="efa4a-141">Začněte přidáním obsah šablony do projektu VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="efa4a-142">Je důležité, aby strukturu složky a obsah zrcadlení rozložení projektu.</span><span class="sxs-lookup"><span data-stu-id="efa4a-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="efa4a-143">Následující příklad obsahuje čtyři šablony projektů, které byly odvozeny od základní MVC v šabloně projektů.</span><span class="sxs-lookup"><span data-stu-id="efa4a-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="efa4a-144">Ujistěte se, že všechny soubory, které tvoří vaše šablona projektu (vše pod složce ProjectTemplates) jsou přidány do **obsahu** itemgroup ve VSIX projektu souborové služby a zda obsahuje jednotlivé položky  **CopyToOutputDirectory** a **IncludeInVsix** metadata nastavte, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="efa4a-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="efa4a-145">&lt;Zahrnout obsah =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="efa4a-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="efa4a-146">&lt;CopyToOutputDirectory&gt;vždy&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="efa4a-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="efa4a-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="efa4a-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="efa4a-148">&lt;A obsah&gt;</span><span class="sxs-lookup"><span data-stu-id="efa4a-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="efa4a-149">Pokud ne, rozhraní IDE se pokusí Kompilovat obsah šablony při sestavování VSIX a zobrazí se vám pravděpodobně k chybě.</span><span class="sxs-lookup"><span data-stu-id="efa4a-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="efa4a-150">Soubory kódu v šablonách často obsahují speciální [parametry šablony](https://msdn.microsoft.com/en-us/library/eehb4faa(v=vs.110).aspx) využívá sada Visual Studio, když v šabloně projektů je vytvořena instance a proto nelze kompilovat, v prostředí IDE.</span><span class="sxs-lookup"><span data-stu-id="efa4a-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/en-us/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![Průzkumník řešení](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="efa4a-152">Zavřít návrháře VSIX a potom klikněte pravým tlačítkem na **source.extension.manifest** souboru v **Průzkumníku řešení** a vyberte **otevřít v** a zvolte **XML ( Editor text)** možnost.</span><span class="sxs-lookup"><span data-stu-id="efa4a-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![Otevřít v programu dialogové okno](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="efa4a-154">Vytvoření  **&lt;prostředky&gt;**  elementu a přidejte  **&lt;Asset&gt;**  element pro každý soubor, který musí být obsažené ve VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="efa4a-155">**Typ** atribut jednotlivých  **&lt;Asset&gt;**  element musí být nastaven na **Microsoft.VisualStudio.Mvc.Template**.</span><span class="sxs-lookup"><span data-stu-id="efa4a-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="efa4a-156">Toto je vlastní obor názvů, který plně chápe pouze v Průvodci projektem MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="efa4a-157">Naleznete v dokumentaci VSIX 2.0 schématu pro další informace o struktuře a rozložení souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="efa4a-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="efa4a-158">Stačí přidat soubory do VSIX není dostatečná k registraci šablony pomocí Průvodce MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="efa4a-159">Je třeba zadat informace, jako je název šablony, popis, moduly podporované zobrazení a programovací jazyk pro Průvodce MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="efa4a-160">Tyto informace se provádí v vlastní atributy přidružené  **&lt;Asset&gt;**  element pro každou **vstemplate** souboru.</span><span class="sxs-lookup"><span data-stu-id="efa4a-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="efa4a-161">&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="efa4a-162">Typ =&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="efa4a-163">d:Source =&quot;souboru&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="efa4a-164">Cesta =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="efa4a-165">ProjectType – =&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="efa4a-166">Jazyk =&quot;C#&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="efa4a-167">ViewEngine =&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="efa4a-168">TemplateId =&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="efa4a-169">Title =&quot;vlastní základní webové aplikace&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="efa4a-170">Popis =&quot;vlastní šablony je odvozen z webové aplikace MVC základní (Razor)&quot;</span><span class="sxs-lookup"><span data-stu-id="efa4a-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="efa4a-171">Verze =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="efa4a-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="efa4a-172">Zde jsou vysvětlení vlastních atributů, které se musí vyskytovat:</span><span class="sxs-lookup"><span data-stu-id="efa4a-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="efa4a-173">**ProjectType –** musí být nastavena na MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="efa4a-174">**Jazyk** označí vývoj jazyk podporovaný šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="efa4a-175">Platné hodnoty jsou buď C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="efa4a-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="efa4a-176">**ViewEngine** označí zobrazovací modul nepodporuje šablony například Aspx nebo Razor.</span><span class="sxs-lookup"><span data-stu-id="efa4a-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="efa4a-177">Můžete zadat vlastní hodnotu tohoto pole.</span><span class="sxs-lookup"><span data-stu-id="efa4a-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="efa4a-178">**TemplateId** slouží k seskupení šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="efa4a-179">Hodnota odpovídá existující ID šablony, bude-li přepište dříve registrován u Průvodce MVC šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="efa4a-180">**Název** označí krátký popis zobrazovaný v Průvodci MVC pod Každá šablona projektu.</span><span class="sxs-lookup"><span data-stu-id="efa4a-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="efa4a-181">**Popis** označí více podrobný popis šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="efa4a-182">Poté, co jste přidali všechny soubory k manifestu a uložíte ho, si všimnete, který **prostředky** kartě v návrháři se zobrazí všechny soubory, ale není vlastní atributy, můžete přidat do  **&lt;Asset&gt;**  prvky pro **vstemplate** soubory.</span><span class="sxs-lookup"><span data-stu-id="efa4a-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![Prostředky Návrhář projektu](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="efa4a-184">Teď už jen zbývá kompilace projektu VSIX a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="efa4a-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="efa4a-185">Ujistěte se, aby byly zavřeny všechny instance sady Visual Studio v počítači kde máte v úmyslu testování rozšíření VSIX.</span><span class="sxs-lookup"><span data-stu-id="efa4a-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="efa4a-186">Visual Studio hledá nová rozšíření při spuštění, takže pokud je při instalaci VSIX otevřete prostředí IDE musíte restartovat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efa4a-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="efa4a-187">V Průzkumníku, dvakrát klikněte na soubor VSIX ke spuštění **instalační program VSIX**, klikněte na tlačítko **nainstalovat** a pak spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="efa4a-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![Instalační program VSIX](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="efa4a-189">V nabídce vyberte **nástroje > rozšíření a aktualizace** potvrďte, že je rozšíření nainstalované.</span><span class="sxs-lookup"><span data-stu-id="efa4a-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="efa4a-190">Pokud instalační program VSIX nezaznamenal chyby při instalaci rozšíření můžete zobrazit protokol instalační program VSIX pro další informace.</span><span class="sxs-lookup"><span data-stu-id="efa4a-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="efa4a-191">V protokolu je obvykle vytvořeny v **% temp %** složky uživatele, který nainstalované rozšíření, například **C:\Users\Bob\AppData\Local\Temp**.</span><span class="sxs-lookup"><span data-stu-id="efa4a-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![Rozšíření a aktualizace](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="efa4a-193">Po zavření okna můžete vytvořit projekt MVC 4 zda nové šablony jsou zobrazeny v Průvodci MVC.</span><span class="sxs-lookup"><span data-stu-id="efa4a-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![Nový projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="efa4a-195">Omezení</span><span class="sxs-lookup"><span data-stu-id="efa4a-195">Limitations</span></span>

1. <span data-ttu-id="efa4a-196">Průvodce MVC nepodporuje lokalizované vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="efa4a-197">Průvodce nebudou podávat všechny chyby, pokud se nepodaří najít vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="efa4a-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="efa4a-198">Pokud chybí některé požadované vlastních atributů, šablona by jednoduše vyloučen z průvodce.</span><span class="sxs-lookup"><span data-stu-id="efa4a-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
