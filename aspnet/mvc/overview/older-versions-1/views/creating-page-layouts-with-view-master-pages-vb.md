---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: "Vytváření rozložení stránek s zobrazit stránky předlohy (VB) | Microsoft Docs"
author: microsoft
description: "V tomto kurzu zjistěte, jak vytvořit běžné rozložení stránky pro více stránek v aplikaci a využívají k zobrazení stránky předlohy. Můžete použít..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/16/2008
ms.topic: article
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 5466ea8a33bd2ccfe36c0f01b6b474bbb8d540a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="creating-page-layouts-with-view-master-pages-vb"></a>Vytváření rozložení stránek s zobrazit stránky předlohy (VB)
====================
podle [Microsoft](https://github.com/microsoft)

[Stáhnout PDF](http://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> V tomto kurzu zjistěte, jak vytvořit běžné rozložení stránky pro více stránek v aplikaci a využívají k zobrazení stránky předlohy. Můžete zobrazit stránku předlohy, například definovat rozložení stránky dva sloupce a použití dvou sloupcích rozložení pro všechny stránky ve vaší webové aplikaci.


## <a name="creating-page-layouts-with-view-master-pages"></a>Vytváření rozložení stránek s zobrazit stránky předlohy

V tomto kurzu zjistěte, jak vytvořit běžné rozložení stránky pro více stránek v aplikaci a využívají k zobrazení stránky předlohy. Můžete zobrazit stránku předlohy, například definovat rozložení stránky dva sloupce a použití dvou sloupcích rozložení pro všechny stránky ve vaší webové aplikaci.

Také můžete využít zobrazení stránky předlohy sdílet společný obsah na více stránkách v aplikaci. Například můžete umístit vaše logo webu, navigačních odkazů a reklamy v zobrazení stránky předlohy. Tímto způsobem každé stránky v aplikaci by zobrazit tento obsah automaticky.

V tomto kurzu zjistěte, jak vytvořit novou stránku předlohy zobrazení a vytvářet nové stránky obsahu zobrazení v závislosti na hlavní stránce.

### <a name="creating-a-view-master-page"></a>Vytvoření stránky hlavního zobrazení

Začněme vytvořením zobrazení stránky předlohy, která definuje rozložení dvou sloupců. Můžete přidat nové zobrazení stránku předlohy do projektu MVC kliknutím pravým tlačítkem na složku Views\Shared, výběrem možnosti nabídky **přidat, nové položky**a výběrem šablony stránky předlohy pro zobrazení MVC (viz obrázek 1).


[![Přidání stránky hlavního zobrazení](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Obrázek 01**: Přidání stránky hlavního zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))


Můžete vytvořit více než jednu stránku předlohy zobrazení v aplikaci. Každé zobrazení hlavní stránce můžete definovat různé stránky rozložení. Můžete například určité stránky tak, aby měl rozložení dvou sloupců a dalších stránek tak, aby měl zobrazení tři sloupce.

Hlavní stránka zobrazení vypadá hodně podobá standardní zobrazení ASP.NET MVC. Ale na rozdíl od normální zobrazení stránky předlohy zobrazení obsahuje jeden nebo více `<asp:ContentPlaceHolder>` značky. `<contentplaceholder>` Značky slouží k označení oblasti hlavní stránky, která mohou být přepsána nastaveními v jednotlivé stránky obsahu.

Například stránky předlohy zobrazení v výpis 1 definuje rozložení dvou sloupců. Obsahuje dva `<contentplaceholder>` značky. Jeden `<ContentPlaceHolder>` pro každý sloupec.

**Výpis 1 –`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

Tělo zobrazení stránky předlohy v výpis 1 obsahuje dva `<div>` značky, které odpovídají dva sloupce. Třídy sloupec stylů CSS je použít pro obě `<div>` značky. Tato třída je definována v šabloně stylů deklarované v horní části stránky předlohy. Můžete zobrazit náhled vykreslení stránky předlohy zobrazení přepnutím do zobrazení návrhu. Klikněte na kartu návrh v levém dolním rohu editoru zdrojového kódu (viz obrázek 2).


[![Zobrazení náhledu na hlavní stránce v Návrháři](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Obrázek 02**: zobrazení náhledu na hlavní stránce v Návrháři ([Kliknutím zobrazit obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))


### <a name="creating-a-view-content-page"></a>Vytvoření zobrazení stránky obsahu

Po vytvoření zobrazení stránky předlohy, můžete vytvořit jeden nebo více zobrazení obsahu stránky v závislosti na hlavní stránku zobrazení. Například můžete vytvořit Index zobrazení obsahu stránku pro řadič domovské kliknutím pravým tlačítkem na složku Views\Home, výběr **přidat, nové položky**, vyberete **obsah stránka zobrazení MVC** šablony, které zadáte Název Index.aspx a kliknutím na tlačítko Přidat tlačítko (viz obrázek 3).


[![Přidání zobrazení obsahu stránky](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Obrázek 03**: Přidání obsahu stránky zobrazení ([Kliknutím zobrazit obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))


Po kliknutí na tlačítko Přidat dialogové okno Nový zobrazí umožňující vyberte stránku předlohy zobrazení přidružených obsahu stránce zobrazení (viz obrázek 4). Můžete přejít na stránku předlohy Site.master zobrazení, který jsme vytvořili v předchozí části.


[![Vyberte stránku předlohy](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Obrázek 04**: Výběr stránky předlohy ([Kliknutím zobrazit obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))


Po vytvoření nové stránky obsahu zobrazení v závislosti na hlavní stránce Site.master, můžete si stáhnout soubor v výpis 2.

**Výpis 2 –`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Všimněte si, že toto zobrazení obsahuje `<asp:Content>` značku, která odpovídá ke každému `<asp:ContentPlaceHolder>` značky v zobrazení stránky předlohy. Každý `<asp:Content>` značka zahrnuje ContentPlaceHolderID atribut, který odkazuje na konkrétní `<asp:ContentPlaceHolder>` , přepíše.

Všimněte si kromě toho, že stránka zobrazení obsahu v výpis 2 neobsahuje žádná normální otevírání a HTML ukončovací značky. Například neobsahuje otevření a zavření `<html>` nebo `<head>` značky. Všechny normální otevírání a ukončovací značky jsou obsažené v zobrazení stránky předlohy.

Veškerý obsah, který chcete zobrazit v zobrazení stránky obsahu musí být umístěny v rámci `<asp:Content>` značky. Pokud jste žádné HTML nebo další obsah i mimo tyto značky, pak bude dojde k chybě při pokusu o zobrazení stránky.

Nemusíte přepsání každé `<asp:ContentPlaceHolder>` značky ze stránky předlohy na zobrazení obsahu stránce. Potřebujete přepsat `<asp:ContentPlaceHolder>` značky, pokud chcete nahradit konkrétní obsah značky.

Například upravené zobrazení indexu v výpis 3 obsahuje pouze dva `<asp:Content>` značky. Každý z `<asp:Content>` značky zahrnuje nějaký text.

**Výpis 3 –`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Pokud se požaduje zobrazení v výpis 3, vykreslí stránku na obrázku 5. Všimněte si, že zobrazení vykreslí stránku s dva sloupce. Všimněte si kromě toho, že obsah ze stránky obsahu zobrazení je sloučen s obsah ze stránky hlavního zobrazení.


[![Index stránky zobrazení obsahu](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Obrázek 05**: indexu zobrazení obsahu stránce ([Kliknutím zobrazit obrázek v plné velikosti](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))


### <a name="modifying-view-master-page-content"></a>Úprava obsahu stránky hlavního zobrazení

Jedním z problémů, dojde k téměř okamžitě při práci s hlavní stránky zobrazení je problém úpravy zobrazení stránky předlohy obsahu, pokud jsou požadovány jiné zobrazení obsahu stránky. Například chcete každé stránce ve vaší webové aplikaci tak, aby měl jedinečný název. Název je však uvedená v zobrazení stránky předlohy a není v zobrazení stránky obsahu. Ano jak můžete přizpůsobit název stránky pro jednotlivé stránky zobrazení obsahu?

Existují dva způsoby, které lze upravit nadpis zobrazí při zobrazení stránky obsahu. Nejprve je možné přiřadit název stránky do atribut title `<%@ page %>` – direktiva deklarované v horní části stránky obsahu zobrazení. Například pokud chcete přiřadit název stránky "Super skvělé webu" zobrazení Index, potom můžete zahrnout následující – direktiva v horní části zobrazení Index:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Když zobrazení indexu je vykresleno do prohlížeče, požadovaný název se zobrazí v záhlaví prohlížeče:


[![Záhlaví prohlížeče](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)


Neexistuje jeden důležité požadavek, který v pořadí pro atribut title fungovat, musí vyhovovat zobrazení stránky předlohy. Musí obsahovat zobrazení stránky předlohy `<head runat="server">` značky místo normální `<head>` značky pro jeho záhlaví. Pokud `<head>` značky nezahrnuje runat = "server" atribut pak název nezobrazí. Výchozí zobrazení stránky předlohy zahrnuje požadované `<head runat="server">` značky.

Alternativní způsob úpravy obsahu stránky předlohy ze stránky obsahu jednotlivých zobrazení je zabalit oblast, kterou chcete upravit v `<asp:ContentPlaceHolder>` značky. Představte si například, že chcete změnit pouze název, ale také značky meta pro vykreslení zobrazení stránky předlohy. Tato stránka předlohy výpis 4 obsahuje `<asp:ContentPlaceHolder>` označit jeho `<head>` značky.

**Výpis 4 –`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Všimněte si, že `<asp:ContentPlaceHolder>` značku výpis 4 obsahuje výchozí obsah: výchozí název a výchozí značky meta. Pokud není toto přepsat `<asp:ContentPlaceHolder>` označení jednotlivých zobrazení obsahu stránce, pak se zobrazí výchozí obsah.

Stránka zobrazení obsahu v výpis 5 přepíše `<asp:ContentPlaceHolder>` značky, aby bylo možné zobrazit vlastní název a vlastní značky meta.

**Výpis 5 –`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Souhrn

V tomto kurzu poskytl základní informace o zobrazit stránky předlohy a stránky obsahu. Jste zjistili, jak vytvořit nové zobrazení hlavní stránky a vytvářet zobrazení obsahu stránky na jejich základě. Rovněž zkoumány, jak je možné upravit obsah zobrazení stránky předlohy z konkrétní zobrazení obsahu stránky.

>[!div class="step-by-step"]
[Předchozí](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
[další](passing-data-to-view-master-pages-vb.md)
