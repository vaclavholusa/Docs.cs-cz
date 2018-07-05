---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
title: Vytvoření rozložení stránek pomocí stránek předlohy pro zobrazení (C#) | Dokumentace Microsoftu
author: microsoft
description: V tomto kurzu se dozvíte, jak vytvořit společné rozložení stránky pro více stránek ve vaší aplikaci s využitím zobrazení stránky předlohy. Můžete použít...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: dff54fcb-68b1-4488-89a2-ca97532d6a4c
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: c23f397ad9dd5c26278d654ef2aec66d201166a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387845"
---
<a name="creating-page-layouts-with-view-master-pages-c"></a>Vytvoření rozložení stránek pomocí stránek předlohy pro zobrazení (C#)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_CS.pdf)

> V tomto kurzu se dozvíte, jak vytvořit společné rozložení stránky pro více stránek ve vaší aplikaci s využitím zobrazení stránky předlohy. Zobrazení stránky předlohy, můžete použít například k definování rozložení stránky dvěma sloupci a použití dvěma sloupci rozložení pro všechny stránky ve webové aplikaci.


## <a name="creating-page-layouts-with-view-master-pages"></a>Vytvoření rozložení stránek pomocí stránek předlohy pro zobrazení

V tomto kurzu se dozvíte, jak vytvořit společné rozložení stránky pro více stránek ve vaší aplikaci s využitím zobrazení stránky předlohy. Zobrazení stránky předlohy, můžete použít například k definování rozložení stránky dvěma sloupci a použití dvěma sloupci rozložení pro všechny stránky ve webové aplikaci.

Můžete také můžete využít výhod zobrazení stránky předlohy sdílet společný obsah mezi více stránek ve vaší aplikaci. Například můžete umístit logo webu, navigačních odkazů a reklamy v zobrazení stránky předlohy. Tímto způsobem každé stránky ve vaší aplikaci zobrazí tento obsah automaticky.

V tomto kurzu se dozvíte, jak vytvořit nové zobrazení stránky předlohy a vytvořit novou stránku obsahu zobrazení založené na hlavní stránce.

### <a name="creating-a-view-master-page"></a>Vytvoření stránky předlohy pro zobrazení

Začněme vytvořením zobrazení stránky předlohy, která definuje rozložení dvou sloupců. Přidáte novou hlavní stránku zobrazení do projektu aplikace MVC kliknutím pravým tlačítkem složku Views\Shared, vyberte možnost nabídky **přidat, nová položka**a výběr **stránky předlohy pro zobrazení MVC** šablony (viz obrázek 1).


[![Přidání zobrazení stránky předlohy](creating-page-layouts-with-view-master-pages-cs/_static/image2.png)](creating-page-layouts-with-view-master-pages-cs/_static/image1.png)

**Obrázek 01**: přidání na stránku předlohy pro zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-cs/_static/image3.png))


Můžete vytvořit více než jednu stránku předlohy zobrazení v aplikaci. Každé zobrazení hlavní stránce můžete definovat různá rozložení stránek. Můžete například některé stránky, které mají rozložení se dvěma sloupci a jiné stránky, které mají rozložení se třemi sloupci.

Hlavní stránka zobrazení vypadá velmi podobně jako standardní zobrazení ASP.NET MVC. Ale na rozdíl od normální zobrazení stránky předlohy zobrazení obsahuje jednu nebo více `<asp:ContentPlaceHolder>` značky. `<contentplaceholder>` Značky se používají k označení oblastí, které mohou být přepsána nastaveními v jednotlivé stránky obsahu stránky předlohy.

Například zobrazení stránky předlohy v informacích 1 definuje rozložení dvou sloupců. Obsahuje dva `<contentplaceholder>` značky. Jeden `<ContentPlaceHolder>` pro každý sloupec.

**Výpis 1 – `Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample1.aspx)]

Text zobrazení stránky předlohy v informacích 1 obsahuje dva `<div>` značky, které odpovídají dva sloupce. Třída sloupec kaskádová šablona stylů se použije pro obě `<div>` značky. Tato třída je definována v šabloně stylů, které jsou deklarovány v horní části stránky předlohy. Ve verzi preview vykreslení stránky předlohy zobrazení přepnutím do zobrazení návrhu. Klikněte na kartu návrh v levé dolní části editoru zdrojového kódu (viz obrázek 2).


[![Zobrazení náhledu na stránku předlohy v Návrháři](creating-page-layouts-with-view-master-pages-cs/_static/image5.png)](creating-page-layouts-with-view-master-pages-cs/_static/image4.png)

**Obrázek 02**: zobrazení náhledu na stránku předlohy v Návrháři ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-cs/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Vytvoření zobrazení obsahu stránky

Po vytvoření zobrazení stránky předlohy, můžete vytvořit jeden nebo více zobrazení obsahu stránky, které jsou založené na hlavní stránku zobrazení. Například můžete vytvořit Index zobrazení obsahu stránku pro kontroler Home kliknutím pravým tlačítkem složku Views\Home výběr **přidat, nová položka**, vyberete **obsah stránka zobrazení MVC** šablona zadávat Název Index.aspx a kliknutím **přidat** tlačítko (viz obrázek 3).


[![Přidání zobrazení obsahu stránky](creating-page-layouts-with-view-master-pages-cs/_static/image8.png)](creating-page-layouts-with-view-master-pages-cs/_static/image7.png)

**Obrázek 03**: Přidat stránku obsahu zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-cs/_static/image9.png))


Po kliknutí na tlačítko Přidat nové dialogové okno se zobrazí, která umožňuje vybrat hlavní stránku zobrazení pro přidružení k zobrazení obsahu stránky (viz obrázek 4). Můžete přejít na stránku předlohy Site.master zobrazení, kterou jsme vytvořili v předchozí části.


[![Výběr stránky předlohy](creating-page-layouts-with-view-master-pages-cs/_static/image11.png)](creating-page-layouts-with-view-master-pages-cs/_static/image10.png)

**Obrázek 04**: Výběr stránky předlohy ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-cs/_static/image12.png))


Jakmile vytvoříte novou stránku obsahu zobrazení založené na hlavní stránce Site.master, získáte soubor výpisu 2.

**Výpis 2 – `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample2.aspx)]

Všimněte si, že toto zobrazení obsahuje `<asp:Content>` značku, která odpovídá každému `<asp:ContentPlaceHolder>` značky na stránce předlohy pro zobrazení. Každý `<asp:Content>` značka obsahuje atribut ContentPlaceHolderID, který odkazuje na konkrétní `<asp:ContentPlaceHolder>` , který se přepíše.

Všimněte si kromě toho, že stránka zobrazení obsahu v informacích 2 nesmí obsahovat žádný z normální otevírací a zavírací HTML. Například neobsahuje otevírací a zavírací `<html>` nebo `<head>` značky. Všechny běžné počátečními a ukončovacími značkami jsou obsaženy v zobrazení stránky předlohy.

Veškerý obsah, který chcete zobrazit v zobrazení stránky obsahu musí být umístěn v rámci `<asp:Content>` značky. Pokud umístíte veškeré kódování HTML nebo další obsah i mimo tyto značky, pak bude obdržíte chybu při pokusu o zobrazení stránky.

Není nutné přepsat každé `<asp:ContentPlaceHolder>` značku ze stránky předlohy v zobrazení obsahu stránky. Je potřeba přepsat `<asp:ContentPlaceHolder>` značku, pokud chcete značku nahraďte konkrétní obsah.

Například upravené zobrazení indexu v 3 výpis obsahuje pouze dva `<asp:Content>` značky. Každá z `<asp:Content>` značky zahrnuje nějaký text.

**Výpis 3 – `Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample3.aspx)]

Pokud se požaduje zobrazení výpisu 3, vykreslující danou stránku na obrázku 5. Všimněte si, že zobrazení vykreslí stránku se dvěma sloupci. Všimněte si, kromě toho, že obsah z obsahu stránky zobrazení je sloučen s obsahem ze zobrazení stránky předlohy


[![Indexovou stránku obsahu zobrazení](creating-page-layouts-with-view-master-pages-cs/_static/image14.png)](creating-page-layouts-with-view-master-pages-cs/_static/image13.png)

**Obrázek 05**: indexovou stránku obsahu zobrazení ([kliknutím ji zobrazíte obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-cs/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Úprava obsahu stránky předlohy pro zobrazení

Jedním problémem, který narazíte na téměř okamžitě při práci s stránek předlohy pro zobrazení je problém úprava obsahu stránky předlohy pro zobrazení při požadavku na jiné zobrazení obsahu stránky. Například chcete, každá stránka ve webové aplikaci mít jedinečný název. Ale název je deklarován v zobrazení stránky předlohy a ne v obsahu stránky zobrazení. Ano jak můžete přizpůsobit název stránky pro každou stránku obsahu zobrazení?

Existují dva způsoby, které můžete upravit název, který zobrazí stránku obsahu zobrazení. Nejprve můžete přiřadit název stránky do názvu atributu `<%@ page %>` – direktiva deklarované v horní části stránky obsahu zobrazení. Například pokud chcete přiřadit název stránky "Super skvělé webu" k zobrazení indexu, potom můžete zahrnout následující direktiva v horní části zobrazení indexu:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample4.aspx)]

Při zobrazení Index se zobrazí v prohlížeči, požadovaný název se zobrazí v záhlaví okna prohlížeče:


[![Záhlaví prohlížeče](creating-page-layouts-with-view-master-pages-cs/_static/image17.png)](creating-page-layouts-with-view-master-pages-cs/_static/image16.png)


Je důležité požadavků, který zobrazení stránky předlohy musí splňovat, aby název atributu pro práci. Musí obsahovat zobrazení stránky předlohy `<head runat="server">` značky místo normální `<head>` značky pro jeho záhlaví. Pokud `<head>` značky nezahrnuje runat = "server" atribut, pak nebude zobrazovat název. Výchozí zobrazení obsahuje požadované stránky předlohy `<head runat="server">` značky.

Alternativním přístupem k úpravě obsahu stránky předlohy ze stránky obsahu jednotlivých zobrazení je oblast, kterou chcete upravit v zabalit `<asp:ContentPlaceHolder>` značky. Představte si například, že chcete změnit pouze název, ale také metaznaček vykreslení stránky předlohy. Stránka předlohy v informacích 4 obsahuje `<asp:ContentPlaceHolder>` značky v rámci jeho `<head>` značky.

**Část 4 – `Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample5.aspx)]

Všimněte si, že `<asp:ContentPlaceHolder>` značka ve výpisu 4 zahrnuje výchozí obsah: výchozí název a značky meta pro výchozí. Pokud nepřepíšete tím `<asp:ContentPlaceHolder>` označení na stránku obsahu zobrazení jednotlivých, pak se zobrazí výchozí obsah.

Stránka zobrazení obsahu v informacích 5 přepíše `<asp:ContentPlaceHolder>` značky, aby bylo možné zobrazit vlastní název a vlastní značky meta pro.

**Výpis 5 – `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-cs/samples/sample6.aspx)]

### <a name="summary"></a>Souhrn

V tomto kurzu vám poskytuje základní informace o zobrazení stránky předlohy a obsahu stránek zobrazení. Jste zjistili, jak vytvořit nové zobrazení stránky předlohy a stránky zobrazení obsahu na jejich základě vytvořit. Také prozkoumat, jak můžete upravovat obsah z obsahu stránky konkrétní zobrazení stránky předlohy zobrazení.

> [!div class="step-by-step"]
> [Předchozí](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
> [další](passing-data-to-view-master-pages-cs.md)
