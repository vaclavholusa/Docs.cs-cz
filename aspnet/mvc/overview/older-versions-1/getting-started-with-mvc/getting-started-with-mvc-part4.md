---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Vytvoření databáze | Microsoft Docs
author: shanselman
description: Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoření jednoduché webové aplikace, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: ff2a41803cd31ce50bbf79e630d827b6de441ba3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-database"></a>Vytvoření databáze
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Je toto Začátečník kurz, který představuje základní informace o architektuře ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro rozhraní ASP.NET MVC](../../../index.md) kurzy a ukázky najdete další ASP.NET MVC.


V této části jsme se chystáte vytvořit novou databázi SQL Express, jsme budete používat k ukládání a načítání naše film data. V prostředí Visual Web Developer IDE, vyberte zobrazení | Průzkumník serveru. Klikněte pravým tlačítkem na připojení dat a klikněte na tlačítko Přidat připojení...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

V dialogovém okně Zvolit zdroj dat vyberte server Microsoft SQL Server a vyberte pokračovat.

![](getting-started-with-mvc-part4/_static/image2.png)

V dialogovém okně Přidat připojení zadejte ". \SQLEXPRESS" pro název serveru a zadejte "Filmy" jako název nové databáze.

[![Přidat dialogové okno připojení](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Klikněte na tlačítko OK a zobrazí se dotaz, pokud chcete vytvořit tuto databázi. Vyberte možnost Ano.

[![Vytvořit filmy?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Nyní máte v Průzkumníku serveru k dispozici prázdnou databázi.

![Přidat novou tabulku](getting-started-with-mvc-part4/_static/image7.png)

Klikněte pravým tlačítkem na tabulky a klikněte na tlačítko Přidat tabulku. Zobrazí se návrháře tabulky. Přidáte sloupce pro Id, názvu, ReleaseDate, Genre a ceny. Klikněte pravým tlačítkem na sloupec ID a klikněte na tlačítko Nastavit primární klíč. Zde je můj návrh oblasti, které vypadá jako.

[![Editor tabulek databáze](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Vyberte sloupec Id také a v části Vlastnosti sloupce níže změňte "Specifikace Identity" na "Ano".

[![IsIdentity – vlastnosti sloupce](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Když vám to provést, klikněte na ikonu Uložit na panelu nástrojů nebo vyberte soubor | Uložit v nabídce a název nové tabulky "**film**" (singulární). My jsme databázi a tabulku!

[![Zvolte název](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Přejděte zpět do Průzkumníka serveru a klikněte pravým tlačítkem v tabulce film a pak vyberte příkaz "Zobrazit Data tabulky". Zadejte pár filmy, takže databáze má některá data.

[![Úpravy tabulek databáze](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Vytvoření modelu

Nyní přejděte zpět do Průzkumníka řešení na pravé straně rozhraní IDE a klikněte pravým tlačítkem na složku modely a vyberte Přidat | Nová položka.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

Vytvoříme pro vytvoření modelu Entity z naší nové databáze. Sadu tříd, se přidá do našich projekt, který lze snadno nám pro dotazování a pracovat s daty v rámci naší databáze. Vyberte datový uzel na levé straně dialogového okna a potom vyberte šablonu, položka ADO.NET Entity Data Model. Pojmenujte ji Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Kliknutím na tlačítko "Přidat". Tím se pak spustí "Entity Data Model průvodce".

V dialogu Nový, která se objeví vyberte generování z databáze. Vzhledem k tomu, že jsme provedli právě databáze, budeme potřebovat pouze říct o našem novou databázi a její tabulkou rozhraní Entity Framework. Klikněte na tlačítko vedle uložit naše připojení k databázi v konfiguraci naše webové aplikace. Teď, zkontrolujte tabulky a film zaškrtávací políčko a klikněte na tlačítko Dokončit.

[![Průvodce Model dat entity](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Nyní jsme v tématu naše nová tabulka film v Návrháři Entity Framework a k němu přístup z kódu.

[![Filmy - sadu Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na návrhovou plochu, která se zobrazí třídu "Film". Tato třída se mapuje na tabulku "Film" v naší databázi, a každou vlastnost v něm mapuje na sloupec v tabulce. Každá instance třídy "Film" bude odpovídat na řádek v tabulce "Film".

Pokud chcete výchozí pojmenovávání a mapování názvů používá rozhraní Entity Framework, můžete změnit nebo si je přizpůsobit návrháře Entity Framework. Pro tuto aplikaci jsme budete používat výchozí hodnoty a právě uložte soubor jako-je.

Teď umožňuje pracovat s některé reálná data!

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part3.md)
> [další](getting-started-with-mvc-part5.md)
