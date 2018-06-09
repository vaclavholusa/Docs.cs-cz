---
uid: mvc/overview/advanced/custom-mvc-templates
title: MVC vlastní šablony | Microsoft Docs
author: joeloff
description: Vytvořte šablonu jako rozšíření VSIX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034604"
---
<a name="custom-mvc-template"></a>Šablona vlastní MVC
====================
podle [Jacques Eloff](https://github.com/joeloff)

Verze MVC 3 nástroje aktualizace pro sadu Visual Studio 2010 zavedl samostatné projektu průvodce pro projekty MVC. Tato změna byla doprovází dva faktory. Nejprve zavedení nové šablony MVC 3 a podpora pro moduly další zobrazení, jako je například Razor způsobit zatarasování dialogové okno Nový projekt v sadě Visual Studio. Druhý zákazníci měli požadující body rozšiřitelnosti a Průvodce vytvořením projektu MVC by poskytnout, nám příležitost odpovědět na tyto požadavky.

Přidání vlastních šablon se náročnou proces, který spoléhali na pomocí klíče registru, aby byly nové šablony viditelné pro Průvodce projektem MVC. Vytvořit nové šablony museli zabalení uvnitř MSI zajistit, že nezbytné položky registru by se vytvořily během instalace. Alternativním bylo zkontrolujte soubor ZIP obsahující dostupné šablony a mít koncový uživatel ručně vytvořit požadované položky registru.

Žádná z výše uvedených přístupy je ideální, takže jsme se rozhodli využít některé z existující infrastruktury služby [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) rozšíření, aby bylo snazší autorovi, distribuce a nainstalovat vlastní šablony MVC počínaje MVC 4 pro sadu Visual Studio 2012. Jsou některé výhody tohoto přístupu:

- Rozšíření VSIX může obsahovat několik šablon, které podporují různé jazyky (C# a Visual Basic) a více moduly zobrazení (ASPX a Razor).
- Rozšíření VSIX, můžete vybrat více SKU aplikace Visual Studio včetně Express SKU.
- [Galerie sady Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) usnadňuje distribuci rozšíření cílové.
- Lze upgradovat VSIX rozšíření usnadňují vytváření opravy a aktualizace vlastní šablony.

## <a name="prerequisites"></a>Požadavky

- Uživatelé musí být obeznámeni s vytváření šablon projektu, včetně požadované značky pro vstemplate – soubory atd.
- Uživatelé budou muset mít Visual Studio Professional a vyšší nainstalován. Expresní SKU nepodporují vytváření projektů VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) nainstalována.

## <a name="example"></a>Příklad

Prvním krokem je vytvoření nového projektu VSIX pomocí jazyka C# nebo Visual Basic. Vyberte **soubor > Nový projekt**, pak klikněte na tlačítko **rozšiřitelnost** v levém podokně a vyberte **projektu VSIX**.

![Nový projekt](custom-mvc-templates/_static/image1.jpg)

Po vytvoření projektu se otevře návrháře VSIX.

![Metadata Návrhář projektu](custom-mvc-templates/_static/image2.jpg)

Návrháře lze upravit některé obecné vlastnosti rozšíření, který se bude zobrazovat uživatelům při instalaci rozšíření nebo procházet nainstalovaná rozšíření v sadě Visual Studio (**nástroje > rozšíření a aktualizace**). Po dokončení klikněte na na obecné informace **nainstalovat cíle karta**.

![Cíle projektu návrháře instalace](custom-mvc-templates/_static/image3.jpg)

Na této kartě lze určit SKU a verzí sady Visual Studio, které jsou podporovány vaší rozšíření. Zaškrtněte políčko **tento VSIX je instalována pro všechny uživatele** povolit instalaci na počítač nástroje VSIX. Klikněte na **nový** tlačítko na pravé straně, chcete-li přidat další SKU například Web Developer Express (VWD).

![Přidat nový cíl instalace](custom-mvc-templates/_static/image4.jpg)

Pokud máte v úmyslu podporovat všechny Professional a vyšší skladové jednotky (Professional, Premium a Ultimate) potřebujete jenom v dané rodině, vyberte minimální SKU **Microsoft.VisualStudio.Pro**. Nezapomeňte uložit všechny změny, po dokončení instalace cíle.

![Cíle projektu návrháře instalace](custom-mvc-templates/_static/image5.jpg)

**Prostředky** karta slouží k přidání všechny soubory obsahu do VSIX. Vzhledem k tomu, že rozhraní MVC vyžaduje vlastních metadat je upravována nezpracované XML souboru manifestu VSIX místo použití **prostředky** kartě přidání obsahu. Začněte přidáním obsah šablony do projektu VSIX. Je důležité, aby strukturu složky a obsah zrcadlení rozložení projektu. Následující příklad obsahuje čtyři šablony projektů, které byly odvozeny od základní MVC v šabloně projektů. Ujistěte se, že všechny soubory, které tvoří vaše šablona projektu (vše pod složce ProjectTemplates) jsou přidány do **obsahu** itemgroup ve VSIX projektu souborové služby a zda obsahuje jednotlivé položky  **CopyToOutputDirectory** a **IncludeInVsix** metadata nastavte, jak je znázorněno v následujícím příkladu.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;vždy&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;A obsah&gt;

Pokud ne, rozhraní IDE se pokusí Kompilovat obsah šablony při sestavování VSIX a zobrazí se vám pravděpodobně k chybě. Soubory kódu v šablonách často obsahují speciální [parametry šablony](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) využívá sada Visual Studio, když v šabloně projektů je vytvořena instance a proto nelze kompilovat, v prostředí IDE.

![Průzkumník řešení](custom-mvc-templates/_static/image6.jpg)

Zavřít návrháře VSIX a potom klikněte pravým tlačítkem na **source.extension.manifest** souboru v **Průzkumníku řešení** a vyberte **otevřít v** a zvolte **XML ( Editor text)** možnost.

![Otevřít v programu dialogové okno](custom-mvc-templates/_static/image7.jpg)

Vytvoření **&lt;prostředky&gt;** elementu a přidejte **&lt;Asset&gt;** element pro každý soubor, který musí být obsažené ve VSIX. **Typ** atribut jednotlivých **&lt;Asset&gt;** element musí být nastaven na **Microsoft.VisualStudio.Mvc.Template**. Toto je vlastní obor názvů, který plně chápe pouze v Průvodci projektem MVC. Naleznete v dokumentaci VSIX 2.0 schématu pro další informace o struktuře a rozložení souboru manifestu.

Stačí přidat soubory do VSIX není dostatečná k registraci šablony pomocí Průvodce MVC. Je třeba zadat informace, jako je název šablony, popis, moduly podporované zobrazení a programovací jazyk pro Průvodce MVC. Tyto informace se provádí v vlastní atributy přidružené **&lt;Asset&gt;** element pro každou **vstemplate** souboru.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;souboru&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Jazyk =&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;vlastní základní webové aplikace&quot;

Popis =&quot;vlastní šablony je odvozen z webové aplikace MVC základní (Razor)&quot;

Verze =&quot;4.0&quot;/&gt;

Zde jsou vysvětlení vlastních atributů, které se musí vyskytovat:

- **ProjectType –** musí být nastavena na MVC.
- **Jazyk** označí vývoj jazyk podporovaný šablony. Platné hodnoty jsou buď C# nebo VB.
- **ViewEngine** označí zobrazovací modul nepodporuje šablony například Aspx nebo Razor. Můžete zadat vlastní hodnotu tohoto pole.
- **TemplateId** slouží k seskupení šablony. Hodnota odpovídá existující ID šablony, bude-li přepište dříve registrován u Průvodce MVC šablony.
- **Název** označí krátký popis zobrazovaný v Průvodci MVC pod Každá šablona projektu.
- **Popis** označí více podrobný popis šablony.

Poté, co jste přidali všechny soubory k manifestu a uložíte ho, si všimnete, který **prostředky** kartě v návrháři se zobrazí všechny soubory, ale není vlastní atributy, můžete přidat do **&lt;Asset&gt;** prvky pro **vstemplate** soubory.

![Prostředky Návrhář projektu](custom-mvc-templates/_static/image8.jpg)

Teď už jen zbývá kompilace projektu VSIX a nainstalujte ji.

Ujistěte se, aby byly zavřeny všechny instance sady Visual Studio v počítači kde máte v úmyslu testování rozšíření VSIX. Visual Studio hledá nová rozšíření při spuštění, takže pokud je při instalaci VSIX otevřete prostředí IDE musíte restartovat Visual Studio. V Průzkumníku, dvakrát klikněte na soubor VSIX ke spuštění **instalační program VSIX**, klikněte na tlačítko **nainstalovat** a pak spusťte sadu Visual Studio.

![Instalační program VSIX](custom-mvc-templates/_static/image9.jpg)

V nabídce vyberte **nástroje > rozšíření a aktualizace** potvrďte, že je rozšíření nainstalované. Pokud instalační program VSIX nezaznamenal chyby při instalaci rozšíření můžete zobrazit protokol instalační program VSIX pro další informace. V protokolu je obvykle vytvořeny v **% temp %** složky uživatele, který nainstalované rozšíření, například **C:\Users\Bob\AppData\Local\Temp**.

![Rozšíření a aktualizace](custom-mvc-templates/_static/image10.jpg)

Po zavření okna můžete vytvořit projekt MVC 4 zda nové šablony jsou zobrazeny v Průvodci MVC.

![Nový projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Omezení

1. Průvodce MVC nepodporuje lokalizované vlastní šablony.
2. Průvodce nebudou podávat všechny chyby, pokud se nepodaří najít vlastní šablony. Pokud chybí některé požadované vlastních atributů, šablona by jednoduše vyloučen z průvodce.
