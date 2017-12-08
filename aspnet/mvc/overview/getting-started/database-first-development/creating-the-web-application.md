---
uid: mvc/overview/getting-started/database-first-development/creating-the-web-application
title: "Databázi EF nejprve s architekturou ASP.NET MVC: vytvoření webové aplikace a datové modely | Microsoft Docs"
author: tfitzmac
description: "Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: bc8f2bd5-ff57-4dcd-8418-a5bd517d8953
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/creating-the-web-application
msc.type: authoredcontent
ms.openlocfilehash: f495bfa3aa5332e4ca3e44c2ffbfb760fbbeafc8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="ef-database-first-with-aspnet-mvc-creating-the-web-application-and-data-models"></a>Databázi EF nejprve s architekturou ASP.NET MVC: vytvoření webové aplikace a datové modely
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na vytvoření webové aplikace a generování modelů dat podle databázových tabulek.


## <a name="create-a-new-aspnet-web-application"></a>Vytvoření nové webové aplikace ASP.NET

V nové řešení nebo do stejného řešení jako projekt databáze, vytvořte nový projekt v sadě Visual Studio a vyberte **webové aplikace ASP.NET** šablony. Název projektu **ContosoSite**.

![Vytvoření projektu](creating-the-web-application/_static/image1.png)

Click **OK**.

V okně Nový projekt ASP.NET, vyberte **MVC** šablony. Můžete vymazat **hostitel v cloudu** možnost prozatím, protože se nasazení aplikace do cloudu později. Klikněte na tlačítko **OK** k vytvoření aplikace.

![Vyberte šablonu mvc](creating-the-web-application/_static/image2.png)

Vytvoření projektu s výchozí soubory a složky.

V tomto kurzu použijete Entity Framework 6. Ve vašem projektu prostřednictvím okno Spravovat balíčky NuGet můžete zkontrolujte verzi rozhraní Entity Framework. V případě potřeby aktualizujte svou verzi rozhraní Entity Framework.

![Zobrazit verze](creating-the-web-application/_static/image3.png)

## <a name="generate-the-models"></a>Generování modelů

Rozhraní Entity Framework modely nyní vytvoří z tabulky databáze. Tyto modely jsou třídy, které budete používat pro práci s daty. Každý model zrcadlí tabulky v databázi a obsahuje vlastnosti, které odpovídají sloupců v tabulce.

Klikněte pravým tlačítkem myši **modely** složky a vyberte **přidat** a **novou položku**.

![Přidat novou položku](creating-the-web-application/_static/image4.png)

V okně Přidat novou položku, vyberte **Data** v levém podokně a **ADO.NET Entity Data Model** z možností v prostředním podokně. Název nového souboru modelu **ContosoModel**.

![Vytvoření modelu](creating-the-web-application/_static/image5.png)

Klikněte na tlačítko **přidat**.

V průvodci Entity Data Model vyberte **EF Designer z databáze**.

![generování z databáze](creating-the-web-application/_static/image6.png)

Klikněte na tlačítko **Další**.

Pokud máte připojení databáze, které jsou definované v rámci vývojového prostředí, může najdete v jednom z těchto připojení předem vybraná. Chcete vytvořit nové připojení k databázi, kterou jste vytvořili v první části tohoto kurzu. Klikněte **nové připojení** tlačítko.

![připojení k databázi](creating-the-web-application/_static/image7.png)

V okně Vlastnosti připojení zadat název místního serveru, kde byla vytvořena databáze (v tomto případě **(localdb) \ProjectsV12**). Po zadání názvu serveru, vyberte z dostupných databází ContosoUniversityData.

![Nastavte vlastnosti připojení](creating-the-web-application/_static/image8.png)

Click **OK**.

Nyní se zobrazí vlastnosti správné připojení. Můžete použít výchozí název pro připojení v souboru Web.Config

![nastavení připojení](creating-the-web-application/_static/image9.png)

Klikněte na tlačítko **Další**.

Vyberte **tabulky** ke generování modely pro všechny tři tabulky.

![Vyberte tabulky](creating-the-web-application/_static/image10.png)

Klikněte na tlačítko **Dokončit**.

Pokud se zobrazí upozornění zabezpečení, vyberte **OK** pokračovat v provozu šablony.

Z tabulky databáze, které se generují modely a zobrazená diagram znázorňuje vlastnosti a vztahy mezi tabulkami.

![Diagram modelu](creating-the-web-application/_static/image11.png)

Složku modely nyní zahrnuje mnoho nové soubory související s modely, které byly vygenerovány z databáze.

![Zobrazit nové soubory modelu](creating-the-web-application/_static/image12.png)

**ContosoModel.Context.cs** soubor obsahuje třídu odvozenou od **DbContext** třídy a poskytuje vlastnosti pro každou třídu modelu, která odpovídá do databázové tabulky. **Course.cs**, **Enrollment.cs**, a **Student.cs** soubory obsahují třídy modelu, které představují tabulky databáze. Třídy kontextu a třídy modelu použije při práci s generování uživatelského rozhraní.

Než budete pokračovat v tomto kurzu, sestavte projekt. V další části bude generovat kód podle datové modely, ale tento oddíl nebude fungovat, pokud nebyl sestaven projektu.

>[!div class="step-by-step"]
[Předchozí](setting-up-database.md)
[další](generating-views.md)
