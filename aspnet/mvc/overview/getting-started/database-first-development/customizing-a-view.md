---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: přizpůsobení zobrazení | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867655"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a>Databázi EF nejprve s architekturou ASP.NET MVC: přizpůsobení zobrazení
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na změnu zobrazení automaticky generované pro zlepšení prezentaci.


## <a name="add-enrolled-courses-to-student-details"></a>Přidat zaregistrovaná kurzy student podrobnosti o

Generovaný kód poskytuje dobrý výchozí bod pro vaši aplikaci, ale neposkytuje nutně všechny funkce, které potřebujete ve vaší aplikaci. Můžete upravit kód, který splňovat požadavky na konkrétní aplikace. V současné době aplikace zaregistrované kurzy pro vybrané student nezobrazí. V této části přidáte zaregistrovaná kurzy pro každý studenty do **podrobnosti** zobrazení pro student.

Otevřete **Students/Details.cshtml**a pod poslední &lt;/dl&gt; kartě, ale před uzavírací &lt;/div&gt; značky, přidejte následující kód.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Tento kód vytvoří tabulku, která zobrazí řádek pro každý záznam v tabulce registrace pro vybrané student. **Zobrazení** metoda vykreslí HTML pro objekt (typem modelItem), který reprezentuje výraz. Použít metodu zobrazení (nikoli jednoduše vložení hodnoty vlastnosti v kódu) a ujistěte se, hodnota formátována správně na základě jeho typu a šablonu pro daný typ. V tomto příkladu každý výraz vrací jedinou vlastnost z aktuální záznam v smyčky a hodnoty jsou primitivní typy, které se vykresluje jako text.

Přejděte do zobrazení studenty nebo Index znovu a vyberte **podrobnosti** pro jednu na studentů. Uvidíte, že neobsahuje zaregistrovaná kurzy v zobrazení.

![Student s registrací.](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> [Předchozí](changing-the-database.md)
> [další](enhancing-data-validation.md)
