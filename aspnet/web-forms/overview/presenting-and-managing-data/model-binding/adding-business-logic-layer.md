---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Přidání vrstvu obchodní logiky do projektu, který používá vazby modelu a webové formuláře | Microsoft Docs
author: tfitzmac
description: Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Interakce dat umožňuje vazby modelu další přímo-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Přidání vrstvu obchodní logiky do projektu, který používá vazby modelu a webové formuláře
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz řady ukazuje základní aspekty projektu webových formulářů ASP.NET pomocí vazby modelu. Vazby modelu umožňuje dat interakce více jednoduché než plánování práce s daty objekty zdrojů (například ObjectDataSource nebo SqlDataSource). Tato řada začíná úvodní informace a přesune do dalších pokročilých konceptů v dalších kurzech.
> 
> Tento kurz ukazuje, jak použít modelovou vazbu s vrstvu obchodní logiky. Nastavíte člen OnCallingDataMethods k určení, že objekt než aktuální stránku se používá k volání metody datového.
> 
> V tomto kurzu vychází vytvořené v projektu [starší](retrieving-data.md) částí řady.
> 
> Můžete [Stáhnout](https://go.microsoft.com/fwlink/?LinkId=286116) dokončený projekt v jazyce C# nebo VB. Kód ke stažení pracuje s Visual Studio 2012 nebo Visual Studio 2013. Používá šablony sady Visual Studio 2012, která se poněkud liší od šablony sady Visual Studio 2013 uvedené v tomto kurzu.


## <a name="what-youll-build"></a>Co budete sestavení

Vazby modelu umožňuje ukládat kód interakce data v souboru kódu pro webovou stránku nebo v samostatné obchodní logiky třídě. Předchozí kurzy ukázaly, jak používat soubory kódu pro kód interakce data. Tento postup funguje pro malé weby, ale může vést k kódu opakování a větší potíže při zachování velkých webů. Může být také velmi obtížné prostřednictvím kódu programu testování kódu, který se nachází v kódu na pozadí soubory, protože neexistuje žádný abstraktní vrstvu.

A centralizovat data interakce kód, můžete vytvořit vrstvu obchodní logiky, která obsahuje veškerá logiku pro interakci s daty. Potom zavolejte vrstvu obchodní logiky z webových stránek. Tento kurz ukazuje, jak přesunout všechny kód, který jste napsali v předchozí kurzech do vrstvy obchodní logiky a potom pomocí tohoto kódu ze stránky.

V tomto kurzu budete:

1. Přesun kód z kódu soubory do vrstvy obchodní logiky
2. Změnit vaše data vázaná ovládací prvky pro volání metody v vrstvu obchodní logiky

## <a name="create-business-logic-layer"></a>Vytvořte vrstvu obchodní logiky

Teď vytvoříte třídu, která je volána z webové stránky. Metody v této třídě vypadat podobně jako metody, které jste použili v předchozí kurzy a zahrnují atributy zprostředkovatele hodnot.

Nejprve přidejte novou složku s názvem **BLL**.

![Přidat složku](adding-business-logic-layer/_static/image1.png)

Ve složce BLL, vytvořte novou třídu s názvem **SchoolBL.cs**. Bude obsahovat všechny operace dat, které původně bydliště v soubory kódu. Metody jsou téměř stejné jako metody v souboru kódu na pozadí, ale bude obsahovat některé změny.

Je nejdůležitější změny si uvědomit, jsou už provádění kódu z v rámci instance **stránky** třídy. Obsahuje třídy stránky **TryUpdateModel** metoda a **ModelState** vlastnost. Pokud tento kód je přesunuta do vrstvy obchodní logiky, už mít instanci třídy stránky volat tito členové. Chcete-li získat chcete tento problém vyřešit, je nutné přidat **ModelMethodContext** do libovolné metody, která přistupuje k TryUpdateModel nebo ModelState parametr. Tento parametr ModelMethodContext použijete zavolat TryUpdateModel nebo načíst ModelState. Není nutné změnit žádnou na webové stránce, aby se zohlednily tento nový parametr.

Nahraďte kód v SchoolBL.cs následujícím kódem.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Zkontrolovat, jestli existující stránky k načtení dat z vrstvu obchodní logiky

Nakonec se převede na stránkách Students.aspx AddStudent.aspx a Courses.aspx z pomocí dotazů v souboru kódu pomocí vrstvu obchodní logiky.

Soubory kódu pro studenty, AddStudent a kurzy odstranit nebo komentář následující metody dotazu:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Nyní byste měli mít příslušné žádný kód v souboru kódu, který se vztahují na data operací.

**OnCallingDataMethods** obslužné rutiny události můžete určit objekt, který použijete pro metody data. V Students.aspx přidejte hodnotu této obslužné rutiny události a změnit názvy metod dat na názvy metod ve třídě obchodní logiku.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

V souboru kódu na pozadí pro Students.aspx definujte obslužné rutiny události pro událost CallingDataMethods. V této obslužné rutiny události zadejte třídě obchodní logiku pro datové operace.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

In AddStudent.aspx, make similar changes.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

V Courses.aspx proveďte podobné změny.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Spusťte aplikaci a Všimněte si, že všechny stránky fungovat jako měly dříve. Logiku ověření také funguje správně.

## <a name="conclusion"></a>Závěr

V tomto kurzu znovu strukturovaná aplikace k používání vrstva přístupu k datům a vrstvu obchodní logiky. Zadali jste, že ovládací prvky datových používat objekt, který není na aktuální stránce pro operace dat.

> [!div class="step-by-step"]
> [Předchozí](using-query-string-values-to-retrieve-data.md)
