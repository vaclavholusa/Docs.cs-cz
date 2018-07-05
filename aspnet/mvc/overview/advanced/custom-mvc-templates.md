---
uid: mvc/overview/advanced/custom-mvc-templates
title: Vlastní šablona MVC | Dokumentace Microsoftu
author: joeloff
description: Vytvoření šablony jako rozšíření VSIX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 715990e2d7151ad1ce8288169ef4bdd5c4243369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367241"
---
<a name="custom-mvc-template"></a><span data-ttu-id="65999-103">Vlastní šablona MVC</span><span class="sxs-lookup"><span data-stu-id="65999-103">Custom MVC Template</span></span>
====================
<span data-ttu-id="65999-104">podle [Jacques Eloff](https://github.com/joeloff)</span><span class="sxs-lookup"><span data-stu-id="65999-104">by [Jacques Eloff](https://github.com/joeloff)</span></span>

<span data-ttu-id="65999-105">Verze MVC 3 nástroje Update pro sadu Visual Studio 2010 zavedené Průvodce samostatných projektů pro projekty MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-105">The release of MVC 3 Tools Update for Visual Studio 2010 introduced a separate project wizard for MVC projects.</span></span> <span data-ttu-id="65999-106">Tato změna byla využitím dva faktory.</span><span class="sxs-lookup"><span data-stu-id="65999-106">The change was driven by two factors.</span></span> <span data-ttu-id="65999-107">Nejprve způsobit zavedení nové šablony MVC 3 a podpora pro zobrazení dalších modulů, jako je Razor zatarasování dialogu Nový projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65999-107">First, the introduction of new templates in MVC 3 and support for additional view engines such as Razor lead to overcrowding the New Project dialog in Visual Studio.</span></span> <span data-ttu-id="65999-108">Za druhé zákazníci měli už dlouho žádali bodů rozšiřitelnosti a Průvodce novým projektem MVC by si dovolit nám moci odpovědět na tyto požadavky.</span><span class="sxs-lookup"><span data-stu-id="65999-108">Second, customers had been asking for extensibility points and the new MVC project wizard would afford us the opportunity to respond to these requests.</span></span>

<span data-ttu-id="65999-109">Přidání vlastních šablon byla namáhavý proces, který spoléhal na zviditelnit nové šablony do Průvodce vytvořením projektu MVC pomocí registru.</span><span class="sxs-lookup"><span data-stu-id="65999-109">Adding custom templates was an arduous process that relied on using the registry to make new templates visible to the MVC project wizard.</span></span> <span data-ttu-id="65999-110">Vytvořit novou šablonu došlo ke sbalení uvnitř MSI a ověřte, že nezbytné položky registru bude vytvořen v době instalace.</span><span class="sxs-lookup"><span data-stu-id="65999-110">The author of a new template had to wrap it inside an MSI to ensure that the necessary registry entries would be created at install time.</span></span> <span data-ttu-id="65999-111">Alternativním byl soubor ZIP, který obsahuje šablonu, která je k dispozici a mít koncový uživatel ručně vytvořit požadované položky registru.</span><span class="sxs-lookup"><span data-stu-id="65999-111">The alternative was to make a ZIP file containing the template available and have the end-user create the required registry entries by hand.</span></span>

<span data-ttu-id="65999-112">Žádná z výše uvedených dvou přístupů je ideální, takže jsme se rozhodli využít některé z existující infrastrukturu [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) rozšíření zjednodušit Autor, distribuci a instalaci vlastní šablony MVC počínaje MVC 4 pro sadu Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="65999-112">Neither of the aforementioned approaches is ideal so we decided to leverage some of the existing infrastructure provided by [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensions to make it easier to author, distribute and install custom MVC templates starting with MVC 4 for Visual Studio 2012.</span></span> <span data-ttu-id="65999-113">Mezi výhody tohoto přístupu patří:</span><span class="sxs-lookup"><span data-stu-id="65999-113">Some of the benefits provided by this approach are:</span></span>

- <span data-ttu-id="65999-114">Rozšíření VSIX může obsahovat více šablon, které zajistit podporu různých jazyků (C# a Visual Basic) a více moduly zobrazení (ASPX a Razor).</span><span class="sxs-lookup"><span data-stu-id="65999-114">A VSIX extension can contain multiple templates that support different languages (C# and Visual Basic) and multiple view engines (ASPX and Razor).</span></span>
- <span data-ttu-id="65999-115">Rozšíření VSIX mohou cílit na více SKU sady Visual Studio včetně Express SKU.</span><span class="sxs-lookup"><span data-stu-id="65999-115">A VSIX extension can target multiple SKUs of Visual Studio including Express SKUs.</span></span>
- <span data-ttu-id="65999-116">[Galerie sady Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) usnadňuje distribuci rozšíření pro širokou cílovou skupinu.</span><span class="sxs-lookup"><span data-stu-id="65999-116">The [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) facilitates distributing the extension to a wide audience.</span></span>
- <span data-ttu-id="65999-117">Rozšíření VSIX je možné upgradovat, což usnadňuje vytváření opravy a aktualizace vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-117">VSIX extensions can be upgraded making it easier to author corrections and updates to your custom templates.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="65999-118">Požadavky</span><span class="sxs-lookup"><span data-stu-id="65999-118">Prerequisites</span></span>

- <span data-ttu-id="65999-119">Uživatelé musí být obeznámeni s vytvořením šablony projektů, včetně požadované značky souborů vstemplate atd.</span><span class="sxs-lookup"><span data-stu-id="65999-119">Users need to be familiar with authoring project templates, including the required markup for vstemplate files, etc.</span></span>
- <span data-ttu-id="65999-120">Uživatelé musí mít Visual Studio Professional a vyšší.</span><span class="sxs-lookup"><span data-stu-id="65999-120">Users will need to have Visual Studio Professional and higher installed.</span></span> <span data-ttu-id="65999-121">Express SKU nepodporuje vytváření projektů VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-121">Express SKUs do not support creating VSIX projects.</span></span>
- <span data-ttu-id="65999-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) nainstalované.</span><span class="sxs-lookup"><span data-stu-id="65999-122">[Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installed.</span></span>

## <a name="example"></a><span data-ttu-id="65999-123">Příklad</span><span class="sxs-lookup"><span data-stu-id="65999-123">Example</span></span>

<span data-ttu-id="65999-124">Prvním krokem je vytvoření nového projektu VSIX pomocí jazyka C# nebo Visual Basic.</span><span class="sxs-lookup"><span data-stu-id="65999-124">The first step is to create a new VSIX project using either C# or Visual Basic.</span></span> <span data-ttu-id="65999-125">Vyberte **soubor > Nový projekt**, pak klikněte na tlačítko **rozšiřitelnost** v levém podokně a vyberte **projekt VSIX**.</span><span class="sxs-lookup"><span data-stu-id="65999-125">Select **File > New Project**, then click **Extensibility** in the left pane and select the **VSIX Project**.</span></span>

![Nový projekt](custom-mvc-templates/_static/image1.jpg)

<span data-ttu-id="65999-127">Po vytvoření projektu, otevře se Návrhář VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-127">After the project is created, the VSIX designer will be opened.</span></span>

![Metadata Návrháře projektu](custom-mvc-templates/_static/image2.jpg)

<span data-ttu-id="65999-129">Návrhář umožňuje upravit některé obecné vlastnosti rozšíření, které se bude zobrazovat uživatelům při instalaci rozšíření nebo procházet nainstalovaná rozšíření v sadě Visual Studio (**nástroje > rozšíření a aktualizace**).</span><span class="sxs-lookup"><span data-stu-id="65999-129">The designer can be used to edit some of the general properties of the extension that will be shown to users when they install the extension or browse the installed extensions in Visual Studio (**Tools > Extensions and Updates**).</span></span> <span data-ttu-id="65999-130">Po dokončení klikněte na obecné informace o **cíle instalace kartu**.</span><span class="sxs-lookup"><span data-stu-id="65999-130">Once you have completed the general information click on the **Install Targets tab**.</span></span>

![Cíle instalace Návrháře projektu](custom-mvc-templates/_static/image3.jpg)

<span data-ttu-id="65999-132">Tato karta slouží k určení SKU a verze sady Visual Studio, které vaše rozšíření podporuje.</span><span class="sxs-lookup"><span data-stu-id="65999-132">This tab is used to specify the SKUs and versions of Visual Studio that are supported by your extension.</span></span> <span data-ttu-id="65999-133">Zaškrtněte políčko pro **tento VSIX se nainstalují pro všechny uživatele** umožňující vázaná na počítač nainstaluje rozšíření VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-133">Select the checkbox for **This VSIX is installed for all users** to enable per-machine installs of the VSIX.</span></span> <span data-ttu-id="65999-134">Klikněte na **nový** tlačítko na pravé straně, chcete-li přidat další skladové položky jako je Web Developer Express (VWD).</span><span class="sxs-lookup"><span data-stu-id="65999-134">Click on the **New** button on the right to add additional SKUs such as Web Developer Express (VWD).</span></span>

![Přidat nový cíl instalace](custom-mvc-templates/_static/image4.jpg)

<span data-ttu-id="65999-136">Pokud máte v úmyslu podporují všechny Professional a vyšší skladové jednotky (Professional, Premium a Ultimate) stačí vybrat minimální SKU v rodině, **Microsoft.VisualStudio.Pro**.</span><span class="sxs-lookup"><span data-stu-id="65999-136">If you intend to support all the Professional and higher SKUs (Professional, Premium and Ultimate) you only need to select the minimum SKU in the family, **Microsoft.VisualStudio.Pro**.</span></span> <span data-ttu-id="65999-137">Nezapomeňte si uložte všechny provedené změny po dokončení cíle instalace.</span><span class="sxs-lookup"><span data-stu-id="65999-137">Remember to save all your changes once you have completed the Install Targets.</span></span>

![Cíle instalace Návrháře projektu](custom-mvc-templates/_static/image5.jpg)

<span data-ttu-id="65999-139">**Prostředky** karta slouží k přidání všechny soubory obsahu do VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-139">The **Assets** tab is used to add all your content files to the VSIX.</span></span> <span data-ttu-id="65999-140">Protože MVC požaduje metadata vlastní je upravována nezpracovaném kódu XML souboru manifestu VSIX namísto použití **prostředky** kartu k přidání obsahu.</span><span class="sxs-lookup"><span data-stu-id="65999-140">Since MVC requires custom metadata you will be editing the raw XML of the VSIX manifest file instead of using the **Assets** tab to add content.</span></span> <span data-ttu-id="65999-141">Začněte přidáním obsah šablony do projektu VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-141">Start by adding the template contents to the VSIX project.</span></span> <span data-ttu-id="65999-142">Je důležité, že struktura složky a obsah zrcadlí rozložení projektu.</span><span class="sxs-lookup"><span data-stu-id="65999-142">It is important that the structure of the folder and the contents mirror the layout of the project.</span></span> <span data-ttu-id="65999-143">Následující příklad obsahuje čtyři šablony projektů, které byly získány z šablony projektu základní MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-143">The example below contains four project templates that were derived from the Basic MVC project template.</span></span> <span data-ttu-id="65999-144">Ujistěte se, že všechny soubory, které tvoří vaši šablonu projektu (všechno pod ní složce ProjectTemplates) jsou přidány do **obsahu** itemgroup ve VSIX projekt Souborová služba a že obsahuje každou položku  **CopyToOutputDirectory** a **IncludeInVsix** metadat nastavit, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="65999-144">Make sure that all the files that comprise your project template (everything underneath the ProjectTemplates folder) are added to the **Content** itemgroup in the VSIX project file and that each item contains the **CopyToOutputDirectory** and **IncludeInVsix** metadata set as shown in the example below.</span></span>

<span data-ttu-id="65999-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span><span class="sxs-lookup"><span data-stu-id="65999-145">&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;</span></span>

<span data-ttu-id="65999-146">&lt;CopyToOutputDirectory&gt;vždy&lt;/CopyToOutputDirectory&gt;</span><span class="sxs-lookup"><span data-stu-id="65999-146">&lt;CopyToOutputDirectory&gt;Always&lt;/CopyToOutputDirectory&gt;</span></span>

<span data-ttu-id="65999-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span><span class="sxs-lookup"><span data-stu-id="65999-147">&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;</span></span>

<span data-ttu-id="65999-148">&lt;/ Obsahu&gt;</span><span class="sxs-lookup"><span data-stu-id="65999-148">&lt;/Content&gt;</span></span>

<span data-ttu-id="65999-149">Pokud ne, rozhraní IDE se pokusí zkompilovat obsah šablony při vytváření VSIX a pravděpodobně se zobrazí chyba.</span><span class="sxs-lookup"><span data-stu-id="65999-149">If not, the IDE will try to compile the contents of the template when you build the VSIX and you will likely see an error.</span></span> <span data-ttu-id="65999-150">Soubory kódu v šablonách často obsahují speciální [parametry šablony](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) používá sada Visual Studio, když je vytvořena instance šablony projektu a proto nemohou být zkompilovány v integrovaném vývojovém prostředí.</span><span class="sxs-lookup"><span data-stu-id="65999-150">Code files in templates often contain special [template parameters](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) used by Visual Studio when the project template is instantiated and therefore cannot be compiled in the IDE.</span></span>

![Průzkumník řešení](custom-mvc-templates/_static/image6.jpg)

<span data-ttu-id="65999-152">Zavřít Návrhář VSIX, a klikněte pravým tlačítkem na **source.extension.manifest** ve **Průzkumníku řešení** a vyberte **otevřít v** a zvolte **XML ( Editor text)** možnost.</span><span class="sxs-lookup"><span data-stu-id="65999-152">Close the VSIX designer, then right click on the **source.extension.manifest** file in **Solution Explorer** and select **Open With** and choose the **XML (Text) Editor** option.</span></span>

![Otevřít pomocí dialogového okna](custom-mvc-templates/_static/image7.jpg)

<span data-ttu-id="65999-154">Vytvoření **&lt;prostředky&gt;** prvek a přidat **&lt;Asset&gt;** – element pro každý soubor, který musí být obsažená ve VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-154">Create an **&lt;Assets&gt;** element and add an **&lt;Asset&gt;** element for each file that must be included in the VSIX.</span></span> <span data-ttu-id="65999-155">**Typ** atribut jednotlivých **&lt;Asset&gt;** element musí být nastaven na **Microsoft.VisualStudio.Mvc.Template**.</span><span class="sxs-lookup"><span data-stu-id="65999-155">The **Type** attribute of each **&lt;Asset&gt;** element must be set to **Microsoft.VisualStudio.Mvc.Template**.</span></span> <span data-ttu-id="65999-156">Toto je vlastní obor názvů, která analyzuje pouze Průvodce projektem MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-156">This is a custom namespace that only the MVC project wizard understands.</span></span> <span data-ttu-id="65999-157">Naleznete v dokumentaci VSIX 2.0 Schema pro další informace o struktuře a rozložení souboru manifestu.</span><span class="sxs-lookup"><span data-stu-id="65999-157">Refer to the VSIX 2.0 Schema documentation for additional information on the structure and layout of the manifest file.</span></span>

<span data-ttu-id="65999-158">Stačí přidat soubory do VSIX není dostatečná k registraci šablony MVC průvodce.</span><span class="sxs-lookup"><span data-stu-id="65999-158">Just adding the files to the VSIX is not sufficient to register the templates with the MVC wizard.</span></span> <span data-ttu-id="65999-159">Budete muset poskytnout informace, jako je název šablony, popis, moduly podporované zobrazení a programovací jazyk do Průvodce vytvořením MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-159">You need to provide information such as the template name, description, supported view engines and programming language to the MVC wizard.</span></span> <span data-ttu-id="65999-160">Tyto informace se přenášejí v vlastní atributy přidružené k **&lt;Asset&gt;** – element pro každé **vstemplate** souboru.</span><span class="sxs-lookup"><span data-stu-id="65999-160">This information is carried in custom attributes associated with the **&lt;Asset&gt;** element for each **vstemplate** file.</span></span>

<span data-ttu-id="65999-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-161">&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;</span></span>

<span data-ttu-id="65999-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-162">Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;</span></span>

<span data-ttu-id="65999-163">d:Source =&quot;souboru&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-163">d:Source=&quot;File&quot;</span></span>

<span data-ttu-id="65999-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-164">Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;</span></span>

<span data-ttu-id="65999-165">ProjectType=&quot;MVC&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-165">ProjectType=&quot;MVC&quot;</span></span>

<span data-ttu-id="65999-166">Jazyk =&quot;C#&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-166">Language=&quot;C#&quot;</span></span>

<span data-ttu-id="65999-167">ViewEngine=&quot;Aspx&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-167">ViewEngine=&quot;Aspx&quot;</span></span>

<span data-ttu-id="65999-168">TemplateId – =&quot;MyMvcApplication&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-168">TemplateId=&quot;MyMvcApplication&quot;</span></span>

<span data-ttu-id="65999-169">Název =&quot;vlastní základní webové aplikace&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-169">Title=&quot;Custom Basic Web Application&quot;</span></span>

<span data-ttu-id="65999-170">Popis =&quot;vlastní šablony je odvozen z webové aplikace základní MVC (Razor)&quot;</span><span class="sxs-lookup"><span data-stu-id="65999-170">Description=&quot;A custom template derived from a Basic MVC web application (Razor)&quot;</span></span>

<span data-ttu-id="65999-171">Verze =&quot;4.0&quot;/&gt;</span><span class="sxs-lookup"><span data-stu-id="65999-171">Version=&quot;4.0&quot;/&gt;</span></span>

<span data-ttu-id="65999-172">Následuje vysvětlení vlastních atributů, které musí být k dispozici:</span><span class="sxs-lookup"><span data-stu-id="65999-172">Below is an explanation of the custom attributes that must be present:</span></span>

- <span data-ttu-id="65999-173">**ProjectType** musí být nastaveno na MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-173">**ProjectType** must be set to MVC.</span></span>
- <span data-ttu-id="65999-174">**Jazyk** označuje vývojový jazyk nepodporuje šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-174">**Language** designates the development language supported by the template.</span></span> <span data-ttu-id="65999-175">Platné hodnoty jsou buď C# nebo VB.</span><span class="sxs-lookup"><span data-stu-id="65999-175">Valid values are either C# or VB.</span></span>
- <span data-ttu-id="65999-176">**ViewEngine** označí zobrazovací modul, který podporuje šablony, jako je například Aspx nebo Razor.</span><span class="sxs-lookup"><span data-stu-id="65999-176">**ViewEngine** designates the view engine supported by the template such as Aspx or Razor.</span></span> <span data-ttu-id="65999-177">Můžete zadat vlastní hodnotu pro toto pole.</span><span class="sxs-lookup"><span data-stu-id="65999-177">You can specify a custom value for this field.</span></span>
- <span data-ttu-id="65999-178">**TemplateId –** je určena k seskupování šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-178">**TemplateId** is used for grouping the templates.</span></span> <span data-ttu-id="65999-179">Hodnota odpovídá existující ID šablony, bude-li přepište dříve zaregistrovali pomocí Průvodce MVC šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-179">If the value matches an existing template ID it will be override templates previously registered with the MVC wizard.</span></span>
- <span data-ttu-id="65999-180">**Název** určuje stručný popis zobrazovaný v Průvodci MVC pod každou šablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="65999-180">**Title** designates the short description displayed in the MVC wizard beneath each project template.</span></span>
- <span data-ttu-id="65999-181">**Popis** určuje podrobnější popis šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-181">**Description** designates a more verbose description of the template.</span></span>

<span data-ttu-id="65999-182">Poté, co jste přidali všechny soubory manifestu a ho uložili, všimnete si, který **prostředky** kartě v návrháři se zobrazí všechny soubory, ale ne vlastní atributy jste přidali do **&lt;Asset&gt;** prvky pro **vstemplate** soubory.</span><span class="sxs-lookup"><span data-stu-id="65999-182">After you have added all the files to the manifest and saved it, you will notice that the **Assets** tab in the designer will display all the files, but not the custom attributes you added to the **&lt;Asset&gt;** elements for the **vstemplate** files.</span></span>

![Prostředky Návrháře projektu](custom-mvc-templates/_static/image8.jpg)

<span data-ttu-id="65999-184">Teď už jen zbývá ke kompilaci projektu VSIX a nainstalujte ji.</span><span class="sxs-lookup"><span data-stu-id="65999-184">All that remains now is to compile the VSIX project and install it.</span></span>

<span data-ttu-id="65999-185">Ujistěte se, že všechny instance sady Visual Studio zavřená na počítači Pokud máte v úmyslu testování rozšíření VSIX.</span><span class="sxs-lookup"><span data-stu-id="65999-185">Make sure that all instances of Visual Studio are closed on the machine where you intend to test the VSIX extension.</span></span> <span data-ttu-id="65999-186">Visual Studio vyhledá nová rozšíření během spouštění, tedy pokud při instalaci rozšíření VSIX je otevřen integrovaném vývojovém prostředí bude nutné restartovat Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65999-186">Visual Studio scans for new extensions during startup, so if the IDE is open while installing a VSIX you will need to restart Visual Studio.</span></span> <span data-ttu-id="65999-187">V Průzkumníku, poklikejte na soubor VSIX ke spuštění **instalátor VSIX**, klikněte na tlačítko **nainstalovat** a pak spusťte sadu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="65999-187">In Explorer, double click on the VSIX file to launch the **VSIX Installer**, click **Install** and then launch Visual Studio.</span></span>

![Instalátor VSIX](custom-mvc-templates/_static/image9.jpg)

<span data-ttu-id="65999-189">V nabídce vyberte **nástroje > rozšíření a aktualizace** potvrďte, že je rozšíření nainstalované.</span><span class="sxs-lookup"><span data-stu-id="65999-189">From the menu, select **Tools > Extensions and Updates** to confirm that your extension was installed.</span></span> <span data-ttu-id="65999-190">Je-li instalátor VSIX hlášené žádné chyby během instalace rozšíření se zobrazí v instalačním programu VSIX protokolu pro další informace.</span><span class="sxs-lookup"><span data-stu-id="65999-190">If the VSIX Installer reported any errors during the installation of the extension you can view the VSIX Installer log for more information.</span></span> <span data-ttu-id="65999-191">Protokol je obvykle vytvořeny v **% temp %** složky uživatele, kterého je nainstalované rozšíření, například **C:\Users\Bob\AppData\Local\Temp**.</span><span class="sxs-lookup"><span data-stu-id="65999-191">The log is usually created in the **%temp%** folder of the user that installed the extension, for example **C:\Users\Bob\AppData\Local\Temp**.</span></span>

![Rozšíření a aktualizace](custom-mvc-templates/_static/image10.jpg)

<span data-ttu-id="65999-193">Po zavření okna můžete vytvořit projekt MVC 4 k, jestli vaše nové šablony jsou uvedené v Průvodci MVC.</span><span class="sxs-lookup"><span data-stu-id="65999-193">After closing the window you can create an MVC 4 project to see whether your new templates are shown in the MVC wizard.</span></span>

![Nový projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a><span data-ttu-id="65999-195">Omezení</span><span class="sxs-lookup"><span data-stu-id="65999-195">Limitations</span></span>

1. <span data-ttu-id="65999-196">Průvodce MVC nepodporuje lokalizované vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-196">The MVC wizard does not support localized custom templates.</span></span>
2. <span data-ttu-id="65999-197">Průvodce nebudou podávat zprávy o chybách Pokud se nepodaří najít vlastní šablony.</span><span class="sxs-lookup"><span data-stu-id="65999-197">The wizard will not report any errors if it fails to locate custom templates.</span></span> <span data-ttu-id="65999-198">Pokud chybí některé požadované vlastních atributů, šablona by jednoduše vyloučené z průvodce.</span><span class="sxs-lookup"><span data-stu-id="65999-198">If any of the required custom attributes are absent, the template would simply be excluded from the Wizard.</span></span>
