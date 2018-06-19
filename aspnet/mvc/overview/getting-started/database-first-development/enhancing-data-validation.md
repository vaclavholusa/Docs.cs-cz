---
uid: mvc/overview/getting-started/database-first-development/enhancing-data-validation
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: rozšíření ověřování dat | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 0ed5e67a-34c0-4b57-84a6-802b0fb3cd00
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/enhancing-data-validation
msc.type: authoredcontent
ms.openlocfilehash: 8ea2e94db7956b76c5ccf0a139ac024e38910b49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879605"
---
<a name="ef-database-first-with-aspnet-mvc-enhancing-data-validation"></a>Databázi EF nejprve s architekturou ASP.NET MVC: rozšíření ověřování dat
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na přidání poznámek data do datového modelu k určení požadavků na ověření a zobrazení formátování. Byl vylepšen na základě na zpětnou vazbu od uživatelů v sekci komentáře.


## <a name="add-data-annotations"></a>Přidat datových poznámek

Jak už jste viděli v dřívější tématu, některá pravidla ověření budou automaticky použita na vstup uživatele. Například můžete jenom zadat číslo pro vlastnost úrovni. Chcete-li určit další pravidla ověření, můžete přidat datových poznámek na třídu modelu. Tyto poznámky platí v celé vaší webové aplikace pro zadanou vlastnost. Můžete taky použít formátování atributy, které Změna zobrazení vlastnosti; Změna, jako hodnota použitá pro popisky text.

V tomto kurzu budete přidávat poznámky data omezit délka zadané vlastnosti FirstName, LastName a MiddleName hodnoty. V databázi jsou tyto hodnoty omezení na 50 znaků; Nicméně ve webové aplikaci tento limit pro počet znaků není aktuálně používá. Pokud uživatel poskytuje více než 50 znaků pro jednu z těchto hodnot, dojde k chybě stránky při pokusu o uložení hodnotu do databáze. Úrovni bude také omezit na hodnoty od 0 do 4.

Otevřete **Student.cs** v soubor **modely** složky. Přidejte do třídy následující zvýrazněný kód.

[!code-csharp[Main](enhancing-data-validation/samples/sample1.cs?highlight=5,15,17,20)]

Enrollment.cs přidejte následující zvýrazněný kód.

[!code-csharp[Main](enhancing-data-validation/samples/sample2.cs?highlight=5,10)]

Sestavte řešení.

Přejděte na stránku pro úpravu nebo vytváření student. Pokud se pokusíte zadat více než 50 znaků, se zobrazí chybová zpráva.

![zobrazit chybová zpráva](enhancing-data-validation/_static/image1.png)

Přejděte na stránku pro úpravy registrace a pokus o poskytují úrovni výše 4.

![Chyba úrovni rozsahu](enhancing-data-validation/_static/image2.png)

Úplný seznam datových poznámek ověření můžete provést u třídy a vlastnosti, najdete v části [System.ComponentModel.DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx).

## <a name="add-metadata-classes"></a>Přidání třídy metadat

Přidání atributy ověření přímo do třídy modelu funguje, když neočekáváte databázi chcete změnit; ale pokud vaše změny v databázi a je nutné znovu vygenerovat třídu modelu, ztratíte všechny atributy, které měl použít pro třídu modelu. Tento přístup může být velmi neefektivní a náchylné k došlo ke ztrátě důležitých ověřovacích pravidel.

Chcete-li se tomuto problému vyhnout, můžete přidat třídu metadat, která obsahuje atributy. Pokud přidružíte třídy modelu pro třídu metadat, jsou tyto atributy použité pro model. V tento postup mohou vytvořit třídu modelu znovu bez ztráty všechny atributy, které byly použity pro třídu metadat.

V **modely** složky, přidejte třídu s názvem **Metadata.cs**.

![Přidání třídy metadat](enhancing-data-validation/_static/image3.png)

Nahraďte kód v Metadata.cs následujícím kódem.

[!code-csharp[Main](enhancing-data-validation/samples/sample3.cs)]

Tyto třídy metadata obsahují všechny atributy ověřování, které jste použili předtím do třídy modelu. **Zobrazení** atribut slouží ke změně hodnoty použít pro popisky text.

Nyní je třeba přidružit třídy modelu třídy metadat.

V **modely** složky, přidejte třídu s názvem **PartialClasses.cs**.

Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](enhancing-data-validation/samples/sample4.cs)]

Všimněte si, že každá třída je označena jako `partial` třída a všechny odpovídající název a oboru názvů jako třída, která se automaticky vygeneroval. Použitím atribut metadat třídu zajistíte, že atributy ověření dat se použije k třídě automaticky generovány. Tyto atributy nebudou ztracena při opětovném generování tříd modelu, protože atribut metadat se používá v částečné třídy, které nejsou znovu vygeneroval.

Chcete-li znovu vygenerovat automaticky vygenerované třídy, otevřete soubor ContosoModel.edmx. Znovu klikněte pravým tlačítkem na návrhové plochy a vyberte možnost **aktualizovat Model z databáze**. I když jste nezměnili databáze, tento proces se znova vygeneruje třídy. V **aktualizovat** vyberte **tabulky** a **Dokončit**.

![Aktualizovat tabulky](enhancing-data-validation/_static/image4.png)

Uložte soubor ContosoModel.edmx tak, aby se změny projevily.

Otevřete soubor Student.cs nebo soubor Enrollment.cs a Všimněte si, že atributy ověření dat, které jste provedli dříve již nejsou v souboru. Ale spusťte aplikaci a Všimněte si, že ověřovací pravidla stále použít při zadávání dat.

> [!div class="step-by-step"]
> [Předchozí](customizing-a-view.md)
> [další](publish-to-azure.md)
