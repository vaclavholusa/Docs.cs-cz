---
uid: mvc/overview/getting-started/database-first-development/generating-views
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: generování zobrazení | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/29/2014
ms.topic: article
ms.assetid: 669367cf-8e30-4eb6-821d-10a7d9bb906c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/generating-views
msc.type: authoredcontent
ms.openlocfilehash: b60e89a187a879255eb051dc87241714cef6fa63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879748"
---
<a name="ef-database-first-with-aspnet-mvc-generating-views"></a>Databázi EF nejprve s architekturou ASP.NET MVC: generování zobrazení
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na generování kontrolery a zobrazení pomocí generování uživatelského rozhraní ASP.NET.


## <a name="add-scaffold"></a>Přidat vygenerované uživatelské rozhraní

Jste připraveni ke generování kódu, který bude poskytovat standard datové operace pro třídy modelu. Přidejte kód tak, že přidáte položku vygenerované uživatelské rozhraní. Existuje mnoho možností pro typ generování uživatelského rozhraní, které můžete přidat; v tomto kurzu bude obsahovat vygenerované uživatelské rozhraní, řadič a zobrazení, které odpovídají Student a registrace modely, kterou jste vytvořili v předchozí části.

K zajištění konzistence v projektu, přidáte nový řadič ke stávající **řadiče** složky. Klikněte pravým tlačítkem myši **řadiče** složky a vyberte **přidat** – **novou vygenerovanou položku**.

![Přidat vygenerované uživatelské rozhraní](generating-views/_static/image1.png)

Vyberte **kontroler MVC 5 se zobrazeními s využitím nástroje Entity Framework** možnost. Tato možnost způsobí vygenerování řadiče a zobrazení pro aktualizaci, odstranění, vytváření a zobrazování dat v modelu.

![přidat řadič mvc](generating-views/_static/image2.png)

Vyberte **Student** pro třídu modelu, vyberte **ContosoUniversityEntities** pro třídy kontextu. Zachovat názvu řadiče jako **StudentsController**,

![Zadejte řadič](generating-views/_static/image3.png)

Klikněte na tlačítko **přidat**.

Pokud se zobrazí chyba, pravděpodobně není sestavení projektu v předchozí části. Pokud ano, zkuste jej sestavit projekt a poté znovu přidejte vygenerované položky.

Po dokončení procesu generování kódu se zobrazí nový řadič a zobrazení ve vašem projektu.

![Zobrazit zobrazení](generating-views/_static/image4.png)

Stejný postup proveďte znovu, ale přidat vygenerované uživatelské rozhraní pro třídu registrace. Po dokončení byste měli mít **EnrollmentsController.cs** souborů a složku ve složce **zobrazení** s názvem **registrace** vytvořit, odstranit, podrobnosti, úpravy a Index zobrazení.

![Zobrazit zobrazení](generating-views/_static/image5.png)

## <a name="add-links-to-new-views"></a>Přidání odkazů na nové zobrazení

K usnadnění přejděte na nová zobrazení, můžete přidat několik hypertextové odkazy na Index zobrazení pro studenty a registrace. Otevřete soubor v **Views/Home/Index.cshtml**, což je domovská stránka pro svůj web. Přidejte následující kód pod jumbotron.

[!code-cshtml[Main](generating-views/samples/sample1.cshtml)]

Pro metodu ActionLink první parametr je text, který se zobrazí v odkazu. Druhý parametr je akce a třetí parametr je název řadiče. Například odkazuje na první odkaz na akce indexu v StudentsController. Skutečný hypertextový odkaz je sestavit z těchto hodnot. Na první odkaz nakonec trvá uživatelům **Index.cshtml** souboru v rámci **zobrazení/studenty** složky.

## <a name="display-student-views"></a>Zobrazit student zobrazení

Ověříte, že kód přidán do projektu správně zobrazí seznam na studentů a umožňuje uživatelům upravit, vytvořit nebo odstranit záznamy studentů v databázi.

Klikněte pravým tlačítkem myši **Views/Home/Index.cshtml** soubor a vyberte **zobrazit v prohlížeči**. Na této stránce klikněte na odkaz na seznam studenty.

![](generating-views/_static/image6.png)

Na této stránce Všimněte si seznamu studenty a odkazy, které chcete upravit tato data.

![seznam studenty](generating-views/_static/image7.png)

Klikněte **vytvořit nový** propojení a zadejte hodnoty pro nové student.

![Vytvořit nový student](generating-views/_static/image8.png)

Klikněte na tlačítko **vytvořit**a Všimněte si nový student je přidán do seznamu.

![seznam s novou studenty](generating-views/_static/image9.png)

Vyberte **upravit** propojit a změnit některé hodnoty pro student.

![Upravit studenty](generating-views/_static/image10.png)

Klikněte na tlačítko **Uložit**a Všimněte si student záznam byl změněn.

Nakonec vyberte **odstranit** propojit a potvrďte, že chcete odstranit záznam kliknutím **odstranit** tlačítko.

![Odstranit studenty](generating-views/_static/image11.png)

Bez psaní kódu, jste přidali zobrazení, které provádějí běžné operace s daty v tabulce Student.

Jste si všimli, že je vlastnost databáze podle textový popisek pro pole (například **LastName**) který není nutně co chcete zobrazit na webové stránce. Například můžete chtít štítek, který chcete být **příjmení**. Tento problém se zobrazením opraví později v tomto kurzu.

## <a name="display-enrollment-views"></a>Zobrazit registrace zobrazení

Vaše databáze zahrnuje na více relaci mezi tabulkami Student a registrace a jeden mnoho relace mezi tabulkami průběh a zápisu. Zobrazení pro registraci správně zpracovat tyto relace. Přejít na domovskou stránku pro web a vyberte **seznam registrace** odkazu a potom **vytvořit nový** odkaz. Zobrazení zobrazí formulář pro vytvoření nového záznamu registrace. Zejména Všimněte si, že tento formulář obsahuje dvě rozevírací seznamy, které se naplní hodnoty ze souvisejících tabulek.

![Vytvoření registrace](generating-views/_static/image12.png)

Kromě toho ověření zadaných hodnot se automaticky použity na základě datový typ pole. Úrovni vyžaduje číslo, takže pokud se pokusíte zadat hodnotu není kompatibilní, zobrazí se chybová zpráva.

![ověřování zpráv](generating-views/_static/image13.png)

Jste ověřili, že zobrazení automaticky generované umožnit uživatelům pracovat s daty v databázi. V dalším kurzu této série se aktualizovat databázi a provést odpovídající změny ve webové aplikaci.

> [!div class="step-by-step"]
> [Předchozí](creating-the-web-application.md)
> [další](changing-the-database.md)
