---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: "Aktualizace, odstraňování a vytváření dat pomocí vazby modelu a webové formuláře | Microsoft Docs"
author: tfitzmac
description: "Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 18c065b44524e7738c048b5908fa50c592188064
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualizace, odstraňování a vytváření dat pomocí vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Tento kurz ukazuje, jak vytvářet, aktualizovat a odstraňovat data s vazby modelu. Bude nastavte následující vlastnosti:
> 
> - Metoda DeleteMethod
> - Metodu InsertMethod
> - Metoda UpdateMethod
> 
> Tyto vlastnosti zobrazí název metody, která zpracovává odpovídající operaci. V rámci této metody poskytují logiku pro interakci s daty.
> 
> V tomto kurzu vychází projektu vytvořeného v prvním [část](retrieving-data.md) řady.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu budete:

1. Přidání šablony dynamických dat.
2. Povolit aktualizaci a odstraňování dat prostřednictvím metody vazby modelu
3. Použít pravidla ověření - povolit vytváření nový záznam v databázi

## <a name="add-dynamic-data-templates"></a>Přidání šablony dynamických dat.

K poskytování nejlepších výsledků a minimalizovat kód opakování, budete používat dynamická data šablony. Dynamická data předem připravených šablon můžete snadno integrovat do existující lokalitu pomocí instalace balíčku NuGet.

Z **spravovat balíčky NuGet**, nainstalujte **DynamicDataTemplatesCS**.

![Šablony dynamických dat.](updating-deleting-and-creating-data/_static/image1.png)

Všimněte si, že váš projekt nyní obsahuje složku s názvem **DynamicData**. V této složce zjistíte šablony, které budou automaticky použita pro dynamické ovládacích prvků v webových formulářů.

![Dynamická data složky](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Povolit aktualizace a odstranění

Uživatelé budou moct aktualizovat a odstranit záznamy v databázi je velmi podobný procesu pro načítání dat. V **metody UpdateMethod** a **metody DeleteMethod** vlastnosti, zadejte názvy metod, které provádí tyto operace. Ovládací rutina GridView můžete také zadat automatické generování upravit a odstranit tlačítka. Následující zvýrazněný kód ukazuje dodatky GridView kódu.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

V souboru kódu na pozadí, přidat, pomocí příkazu pro **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Pak přidejte následující aktualizace a odstranění metody.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

**TryUpdateModel** metoda platí odpovídající hodnoty vázané na data z webového formuláře pro datová položka. Datové položky se načítají podle hodnota parametru id.

## <a name="enforce-validation-requirements"></a>Vynutit požadavky na ověření

Vynutí použité pro vlastnosti FirstName, LastName a rok ve třídě Student atributy ověření se automaticky při aktualizaci data. Ovládací prvky DynamicField přidat klientských a serverových validátory na základě atributů ověření. Vlastnosti FirstName a LastName se vyžaduje. FirstName nesmí překročit 20 znaků a LastName nemůže být delší než 40 znaků. Rok musí být platná hodnota pro výčet AcademicYear.

Pokud uživatel je v rozporu mezi požadavky na ověřování, aktualizace nebude pokračovat. Pokud chcete zobrazit chybová zpráva, přidání ovládacího prvku ValidationSummary výše GridView. Chcete-li zobrazit chyby ověření z vazby modelu, nastavte **ShowModelStateErrors** vlastnost nastavena na hodnotu **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Spusťte webovou aplikaci a aktualizovat a odstranit všechny záznamy.

![Aktualizace dat](updating-deleting-and-creating-data/_static/image3.png)

Všimněte si, když v režimu úprav hodnotu pro vlastnost rok automaticky vykresleno jako rozevíracího seznamu. Vlastnost roku je hodnota výčtu a šablona dynamických dat pro hodnotu výčtu určuje rozevíracího seznamu pro úpravy. Tato šablona můžete najít tak, že otevřete **– výčet\_Edit.ascx** souboru v **DynamicData**/**FieldTemplates** složky.

Pokud jste zadali platné hodnoty, aktualizace se úspěšně dokončí. Pokud porušují mezi požadavky na ověřování, aktualizace nebude pokračovat a chybová zpráva se zobrazí nad mřížky.

![Chybová zpráva](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Přidat nové záznamy

Ovládací prvek GridView nezahrnuje **metodu InsertMethod** vlastnost a nelze jej proto použít pro přidávání nového záznamu s vazby modelu. Můžete najít vlastnost v metodu InsertMethod **FormView**, **DetailsView**, nebo **ListView** ovládací prvky. V tomto kurzu použijete v ovládacím prvku FormView přidejte nový záznam.

Nejprve přidejte odkaz na novou stránku, kterou vytvoříte pro přidávání nového záznamu. Nad ValidationSummary přidejte:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Nový odkaz se zobrazí v horní části obsahu pro studenty stránku.

![nové propojení](updating-deleting-and-creating-data/_static/image5.png)

Pak přidejte nový webový formulář pomocí hlavní stránky a pojmenujte ji **AddStudent**. Vyberte Site.Master jako stránky předlohy.

Učiní polí pro přidání nové student pomocí **DynamicEntity** ovládacího prvku. Ovládací prvek DynamicEntity vykreslí této upravovat vlastnosti ve třídě zadaná ve vlastnosti typ položky. Vlastnost StudentID byla označena **[ScaffoldColumn(false)]** atribut, není vykreslen. MainContent zástupného textu stránky AddStudent přidejte následující kód.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

V souboru kódu na pozadí (AddStudent.aspx.cs), přidejte **pomocí** údajů **ContosoUniversityModelBinding.Models** oboru názvů.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Pak přidejte následující metody, které určete, jak chcete-li vložit nový záznam a obslužné rutiny události pro tlačítko Storno.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Všechny změny uložte.

Spustit webovou aplikaci a vytvořit nový student.

![Přidat nový student](updating-deleting-and-creating-data/_static/image6.png)

Klikněte na tlačítko **vložit** a Všimněte si vytvořil nový student.

![Nový student zobrazení](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Závěr

V tomto kurzu jste povolili, aktualizaci, odstranění a vytvoření data. Můžete zajistit, že ověření pravidla se používají při interakci s daty.

V dalším [kurzu](sorting-paging-and-filtering-data.md) této série povolíte řazení, stránkování a filtrování dat.

>[!div class="step-by-step"]
[Předchozí](retrieving-data.md)
[další](sorting-paging-and-filtering-data.md)
