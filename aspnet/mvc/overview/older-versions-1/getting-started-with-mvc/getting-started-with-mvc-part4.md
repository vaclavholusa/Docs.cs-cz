---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
title: Vytvoření databáze | Dokumentace Microsoftu
author: shanselman
description: Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvořte jednoduchou webovou aplikaci, která čte a zapisuje z databáze.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: 742df67f-484d-4ef3-af6b-8c791e556b43
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part4
msc.type: authoredcontent
ms.openlocfilehash: 0e72359095e4c40ef7e56f1290a45ded257143c9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362353"
---
<a name="creating-a-database"></a>Vytvoření databáze
====================
podle [Scott Hanselman](https://github.com/shanselman)

> Toto je kurz pro začátečníky, který vysvětluje základy ASP.NET MVC. Vytvoříte jednoduchou webovou aplikaci, která čte a zapisuje z databáze. Přejděte [výukové centrum pro ASP.NET MVC](../../../index.md) najít další technologie ASP.NET MVC, kurzů a ukázek.


V této části jsme se chystáte vytvořit novou databázi SQL Express, který použijeme k ukládání a načítání naše data o filmech. Z integrovaného vývojového prostředí Visual Web Developer, vyberte zobrazení | Průzkumník serveru. Klikněte pravým tlačítkem na datová připojení a klikněte na tlačítko Přidat připojení...

![AddConnection](getting-started-with-mvc-part4/_static/image1.png)

V dialogovém okně Zvolit zdroj dat vyberte Microsoft SQL Server a vyberte možnost pokračovat.

![](getting-started-with-mvc-part4/_static/image2.png)

V dialogovém okně Přidat připojení zadejte ". \SQLEXPRESS" pro název serveru a zadejte "Filmy" jako název pro novou databázi.

[![Přidat připojení – dialogové okno](getting-started-with-mvc-part4/_static/image4.png)](getting-started-with-mvc-part4/_static/image3.png)

Klikněte na tlačítko OK a zobrazí se dotaz, pokud chcete vytvořit databázi. Výběrem možnosti Ano.

[![Vytvářet videa?](getting-started-with-mvc-part4/_static/image6.png)](getting-started-with-mvc-part4/_static/image5.png)

Nyní máte v Průzkumníku serveru k dispozici prázdnou databázi.

![Přidat novou tabulku](getting-started-with-mvc-part4/_static/image7.png)

Klikněte pravým tlačítkem na tabulky a klikněte na tlačítko Přidat tabulku. Zobrazí se Návrhář tabulky. Přidání sloupce pro Id, název, ReleaseDate, rozšířením podle tematických a ceny. Klikněte pravým tlačítkem na sloupec ID a klikněte na nastavit primární klíč. Tady je Moje návrhu oblasti, které vypadá.

[![Editor tabulek databáze](getting-started-with-mvc-part4/_static/image9.png)](getting-started-with-mvc-part4/_static/image8.png)

Vyberte sloupec Id také a v části Vlastnosti sloupce níže změňte "Specifikace Identity" na "Ano".

[![IsIdentity – vlastnosti sloupce](getting-started-with-mvc-part4/_static/image11.png)](getting-started-with-mvc-part4/_static/image10.png)

Když máte to Hotovo, klikněte na ikonu Uložit na panelu nástrojů nebo si vybrat soubor | Uložte v nabídce a pojmenujte tabulku "**film**" (singulární). Máme databáze a tabulky!

[![Zvolte název](getting-started-with-mvc-part4/_static/image13.png)](getting-started-with-mvc-part4/_static/image12.png)

Přejděte zpět do Průzkumníka serveru a tabulky Movie klikněte pravým tlačítkem myši a pak vyberte "Zobrazit Data tabulky." Zadejte několik filmy, aby naše databáze má nějaká data.

[![Úpravy tabulek databáze](getting-started-with-mvc-part4/_static/image15.png)](getting-started-with-mvc-part4/_static/image14.png)

## <a name="creating-a-model"></a>Vytvoření modelu

Nyní, přepněte zpět do Průzkumníku řešení v integrovaném vývojovém prostředí pravé straně a klikněte pravým tlačítkem na složku modely a vyberte možnost Přidat | Nová položka.

[![addnewmodelitem](getting-started-with-mvc-part4/_static/image17.png)](getting-started-with-mvc-part4/_static/image16.png)

My budeme k vytvoření modelu Entity z naší nové databáze. Sada tříd bude přidán do projektu, který usnadňuje nám pro dotazování a manipulaci s daty v rámci naší databázi. Vyberte datový uzel na levé straně dialogového okna a pak vyberte šablonu položky ADO.NET Entity Data Model. Pojmenujte ji Movies.edmx.

[![AddNewDataModel](getting-started-with-mvc-part4/_static/image19.png)](getting-started-with-mvc-part4/_static/image18.png)

Klikněte na tlačítko "Přidat". Tím se spustí potom "Průvodce entitního modelu dat".

V dialogovém okně Nový, která se otevře vyberte možnost Generovat z databáze. Protože jsme právě databázi, potřebujeme jenom Entity Framework říct naši novou databázi a její tabulky. Klikněte na tlačítko vedle uložit připojení k naší databázi v konfiguraci naši webovou aplikaci. Teď zkontrolujte tabulky a filmové zaškrtávací políčko a klikněte na tlačítko Dokončit.

[![Průvodce datovým modelem entity](getting-started-with-mvc-part4/_static/image21.png)](getting-started-with-mvc-part4/_static/image20.png)

Nyní jsme najdete v naší nové tabulky Movie v Entity Framework Designer a k němu přístup z kódu.

[![Videa – Microsoft Visual Web Developer 2010 Express](getting-started-with-mvc-part4/_static/image23.png)](getting-started-with-mvc-part4/_static/image22.png)

Na návrhové ploše vidíte třídou "Video". Tato třída se mapuje na tabulku "Video" v naší databázi a každou vlastnost v něm se mapuje na sloupec v tabulce. Každá instance třídy "Video" bude odpovídat řádek v tabulce "Video".

Pokud se vám výchozí názvy a mapování konvencemi použitými rozhraním Entity Framework, můžete změnit nebo si je přizpůsobit, Entity Framework designer. Pro tuto aplikaci použijeme výchozí hodnoty a stačí uložit soubor jako-je.

Nyní se budeme pracovat s daty skutečné!

> [!div class="step-by-step"]
> [Předchozí](getting-started-with-mvc-part3.md)
> [další](getting-started-with-mvc-part5.md)
