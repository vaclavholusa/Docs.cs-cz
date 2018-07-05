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
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367241"
---
<a name="custom-mvc-template"></a>Vlastní šablona MVC
====================
podle [Jacques Eloff](https://github.com/joeloff)

Verze MVC 3 nástroje Update pro sadu Visual Studio 2010 zavedené Průvodce samostatných projektů pro projekty MVC. Tato změna byla využitím dva faktory. Nejprve způsobit zavedení nové šablony MVC 3 a podpora pro zobrazení dalších modulů, jako je Razor zatarasování dialogu Nový projekt v sadě Visual Studio. Za druhé zákazníci měli už dlouho žádali bodů rozšiřitelnosti a Průvodce novým projektem MVC by si dovolit nám moci odpovědět na tyto požadavky.

Přidání vlastních šablon byla namáhavý proces, který spoléhal na zviditelnit nové šablony do Průvodce vytvořením projektu MVC pomocí registru. Vytvořit novou šablonu došlo ke sbalení uvnitř MSI a ověřte, že nezbytné položky registru bude vytvořen v době instalace. Alternativním byl soubor ZIP, který obsahuje šablonu, která je k dispozici a mít koncový uživatel ručně vytvořit požadované položky registru.

Žádná z výše uvedených dvou přístupů je ideální, takže jsme se rozhodli využít některé z existující infrastrukturu [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) rozšíření zjednodušit Autor, distribuci a instalaci vlastní šablony MVC počínaje MVC 4 pro sadu Visual Studio 2012. Mezi výhody tohoto přístupu patří:

- Rozšíření VSIX může obsahovat více šablon, které zajistit podporu různých jazyků (C# a Visual Basic) a více moduly zobrazení (ASPX a Razor).
- Rozšíření VSIX mohou cílit na více SKU sady Visual Studio včetně Express SKU.
- [Galerie sady Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) usnadňuje distribuci rozšíření pro širokou cílovou skupinu.
- Rozšíření VSIX je možné upgradovat, což usnadňuje vytváření opravy a aktualizace vlastní šablony.

## <a name="prerequisites"></a>Požadavky

- Uživatelé musí být obeznámeni s vytvořením šablony projektů, včetně požadované značky souborů vstemplate atd.
- Uživatelé musí mít Visual Studio Professional a vyšší. Express SKU nepodporuje vytváření projektů VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) nainstalované.

## <a name="example"></a>Příklad

Prvním krokem je vytvoření nového projektu VSIX pomocí jazyka C# nebo Visual Basic. Vyberte **soubor > Nový projekt**, pak klikněte na tlačítko **rozšiřitelnost** v levém podokně a vyberte **projekt VSIX**.

![Nový projekt](custom-mvc-templates/_static/image1.jpg)

Po vytvoření projektu, otevře se Návrhář VSIX.

![Metadata Návrháře projektu](custom-mvc-templates/_static/image2.jpg)

Návrhář umožňuje upravit některé obecné vlastnosti rozšíření, které se bude zobrazovat uživatelům při instalaci rozšíření nebo procházet nainstalovaná rozšíření v sadě Visual Studio (**nástroje > rozšíření a aktualizace**). Po dokončení klikněte na obecné informace o **cíle instalace kartu**.

![Cíle instalace Návrháře projektu](custom-mvc-templates/_static/image3.jpg)

Tato karta slouží k určení SKU a verze sady Visual Studio, které vaše rozšíření podporuje. Zaškrtněte políčko pro **tento VSIX se nainstalují pro všechny uživatele** umožňující vázaná na počítač nainstaluje rozšíření VSIX. Klikněte na **nový** tlačítko na pravé straně, chcete-li přidat další skladové položky jako je Web Developer Express (VWD).

![Přidat nový cíl instalace](custom-mvc-templates/_static/image4.jpg)

Pokud máte v úmyslu podporují všechny Professional a vyšší skladové jednotky (Professional, Premium a Ultimate) stačí vybrat minimální SKU v rodině, **Microsoft.VisualStudio.Pro**. Nezapomeňte si uložte všechny provedené změny po dokončení cíle instalace.

![Cíle instalace Návrháře projektu](custom-mvc-templates/_static/image5.jpg)

**Prostředky** karta slouží k přidání všechny soubory obsahu do VSIX. Protože MVC požaduje metadata vlastní je upravována nezpracovaném kódu XML souboru manifestu VSIX namísto použití **prostředky** kartu k přidání obsahu. Začněte přidáním obsah šablony do projektu VSIX. Je důležité, že struktura složky a obsah zrcadlí rozložení projektu. Následující příklad obsahuje čtyři šablony projektů, které byly získány z šablony projektu základní MVC. Ujistěte se, že všechny soubory, které tvoří vaši šablonu projektu (všechno pod ní složce ProjectTemplates) jsou přidány do **obsahu** itemgroup ve VSIX projekt Souborová služba a že obsahuje každou položku  **CopyToOutputDirectory** a **IncludeInVsix** metadat nastavit, jak je znázorněno v následujícím příkladu.

&lt;Content Include=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;vždy&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ Obsahu&gt;

Pokud ne, rozhraní IDE se pokusí zkompilovat obsah šablony při vytváření VSIX a pravděpodobně se zobrazí chyba. Soubory kódu v šablonách často obsahují speciální [parametry šablony](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) používá sada Visual Studio, když je vytvořena instance šablony projektu a proto nemohou být zkompilovány v integrovaném vývojovém prostředí.

![Průzkumník řešení](custom-mvc-templates/_static/image6.jpg)

Zavřít Návrhář VSIX, a klikněte pravým tlačítkem na **source.extension.manifest** ve **Průzkumníku řešení** a vyberte **otevřít v** a zvolte **XML ( Editor text)** možnost.

![Otevřít pomocí dialogového okna](custom-mvc-templates/_static/image7.jpg)

Vytvoření **&lt;prostředky&gt;** prvek a přidat **&lt;Asset&gt;** – element pro každý soubor, který musí být obsažená ve VSIX. **Typ** atribut jednotlivých **&lt;Asset&gt;** element musí být nastaven na **Microsoft.VisualStudio.Mvc.Template**. Toto je vlastní obor názvů, která analyzuje pouze Průvodce projektem MVC. Naleznete v dokumentaci VSIX 2.0 Schema pro další informace o struktuře a rozložení souboru manifestu.

Stačí přidat soubory do VSIX není dostatečná k registraci šablony MVC průvodce. Budete muset poskytnout informace, jako je název šablony, popis, moduly podporované zobrazení a programovací jazyk do Průvodce vytvořením MVC. Tyto informace se přenášejí v vlastní atributy přidružené k **&lt;Asset&gt;** – element pro každé **vstemplate** souboru.

&lt;Asset d:VsixSubPath=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;souboru&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType=&quot;MVC&quot;

Jazyk =&quot;C#&quot;

ViewEngine=&quot;Aspx&quot;

TemplateId – =&quot;MyMvcApplication&quot;

Název =&quot;vlastní základní webové aplikace&quot;

Popis =&quot;vlastní šablony je odvozen z webové aplikace základní MVC (Razor)&quot;

Verze =&quot;4.0&quot;/&gt;

Následuje vysvětlení vlastních atributů, které musí být k dispozici:

- **ProjectType** musí být nastaveno na MVC.
- **Jazyk** označuje vývojový jazyk nepodporuje šablony. Platné hodnoty jsou buď C# nebo VB.
- **ViewEngine** označí zobrazovací modul, který podporuje šablony, jako je například Aspx nebo Razor. Můžete zadat vlastní hodnotu pro toto pole.
- **TemplateId –** je určena k seskupování šablony. Hodnota odpovídá existující ID šablony, bude-li přepište dříve zaregistrovali pomocí Průvodce MVC šablony.
- **Název** určuje stručný popis zobrazovaný v Průvodci MVC pod každou šablonu projektu.
- **Popis** určuje podrobnější popis šablony.

Poté, co jste přidali všechny soubory manifestu a ho uložili, všimnete si, který **prostředky** kartě v návrháři se zobrazí všechny soubory, ale ne vlastní atributy jste přidali do **&lt;Asset&gt;** prvky pro **vstemplate** soubory.

![Prostředky Návrháře projektu](custom-mvc-templates/_static/image8.jpg)

Teď už jen zbývá ke kompilaci projektu VSIX a nainstalujte ji.

Ujistěte se, že všechny instance sady Visual Studio zavřená na počítači Pokud máte v úmyslu testování rozšíření VSIX. Visual Studio vyhledá nová rozšíření během spouštění, tedy pokud při instalaci rozšíření VSIX je otevřen integrovaném vývojovém prostředí bude nutné restartovat Visual Studio. V Průzkumníku, poklikejte na soubor VSIX ke spuštění **instalátor VSIX**, klikněte na tlačítko **nainstalovat** a pak spusťte sadu Visual Studio.

![Instalátor VSIX](custom-mvc-templates/_static/image9.jpg)

V nabídce vyberte **nástroje > rozšíření a aktualizace** potvrďte, že je rozšíření nainstalované. Je-li instalátor VSIX hlášené žádné chyby během instalace rozšíření se zobrazí v instalačním programu VSIX protokolu pro další informace. Protokol je obvykle vytvořeny v **% temp %** složky uživatele, kterého je nainstalované rozšíření, například **C:\Users\Bob\AppData\Local\Temp**.

![Rozšíření a aktualizace](custom-mvc-templates/_static/image10.jpg)

Po zavření okna můžete vytvořit projekt MVC 4 k, jestli vaše nové šablony jsou uvedené v Průvodci MVC.

![Nový projekt ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Omezení

1. Průvodce MVC nepodporuje lokalizované vlastní šablony.
2. Průvodce nebudou podávat zprávy o chybách Pokud se nepodaří najít vlastní šablony. Pokud chybí některé požadované vlastních atributů, šablona by jednoduše vyloučené z průvodce.
