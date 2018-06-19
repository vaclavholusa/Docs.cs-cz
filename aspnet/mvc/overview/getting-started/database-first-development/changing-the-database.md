---
uid: mvc/overview/getting-started/database-first-development/changing-the-database
title: 'Databázi EF nejprve s architekturou ASP.NET MVC: Změna databáze | Microsoft Docs'
author: tfitzmac
description: Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: cfd5c083-a319-482e-8f25-5b38caa93954
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/changing-the-database
msc.type: authoredcontent
ms.openlocfilehash: 63ee8768a43dbdac80922e3adbedd3378c10da73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879319"
---
<a name="ef-database-first-with-aspnet-mvc-changing-the-database"></a>Databázi EF nejprve s architekturou ASP.NET MVC: Změna databáze
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze.
> 
> Tato část řady se zaměřuje na provedení aktualizace na strukturu databáze a šíření této změny v rámci webové aplikace.


## <a name="add-a-column"></a>Přidá sloupec

Pokud aktualizujete struktura tabulky v databázi, je třeba zajistit, že změny rozšířena do datový model, zobrazení a kontroler.

V tomto kurzu přidáte nový sloupec do tabulky Student k zaznamenání křestní jméno student. Pokud chcete přidat tento sloupec, otevřete projekt databáze a otevřete soubor Student.sql. Prostřednictvím návrháře nebo kód T-SQL, přidat sloupec s názvem **MiddleName** , je NVARCHAR(50) a umožňuje hodnoty NULL.

![Přidat křestní jméno](changing-the-database/_static/image1.png)

Nasaďte tuto změnu do místní databáze spuštěním projekt databáze (nebo F5). Nové pole se přidá do tabulky. Pokud se nezobrazí se v Průzkumníku objektů SQL serveru, klikněte na tlačítko Aktualizovat v podokně.

![zobrazení nového sloupce](changing-the-database/_static/image2.png)

Nový sloupec v tabulce databáze existuje, ale neexistuje aktuálně v třídy datového modelu. Je třeba aktualizovat model, který má obsahovat nový sloupec. V **modely** složku, otevřete **ContosoModel.edmx** soubor k zobrazení diagramu modelu. Všimněte si, že Student model neobsahuje vlastnost MiddleName. Klepněte pravým tlačítkem myši na návrhovou plochu a vyberte **aktualizovat Model z databáze**.

![Aktualizace modelu](changing-the-database/_static/image3.png)

V Průvodci aktualizací, vyberte **aktualizovat** kartě a **Student** tabulky.

![Průvodce aktualizací](changing-the-database/_static/image4.png)

Klikněte na tlačítko **Dokončit**.

Po dokončení procesu aktualizace zahrnuje nový diagram databáze **MiddleName** vlastnost. Uložit **ContosoModel.edmx** souboru. Je nutné uložit tento soubor nové vlastnosti mají být předány **Student.cs** třídy. Nyní jste aktualizovali databáze a modelu.

Sestavte řešení.

Bohužel zobrazení stále neobsahuje novou vlastnost. K aktualizaci zobrazení máte dvě možnosti – můžete znovu vygenerovat zobrazení generování uživatelského rozhraní pro třídu Student přidáním znovu, nebo můžete ručně přidat nové vlastnosti pro stávající zobrazení. V tomto kurzu přidáte generování uživatelského rozhraní znovu vzhledem k tomu, že jste neprovedli změny přizpůsobené zobrazení automaticky generovány. Můžete uvažovat o ručně přidejte vlastnost, pokud byly provedeny změny zobrazení a nechcete tyto změny budou ztraceny.

Aby se znovu vytvořit zobrazení, odstranit **studenty** ve složce **zobrazení**a odstranit **StudentsController**. Klikněte pravým tlačítkem **řadiče** složky a přidat generování uživatelského rozhraní pro **Student** modelu. Znovu, názvu kontroleru **StudentsController**. Vyberte **OK**.

Zobrazení nyní obsahují vlastnost MiddleName.

![Zobrazit křestní jméno](changing-the-database/_static/image5.png)

V další části přidáte kód pro přizpůsobení zobrazení pro zobrazení podrobností o student záznamu.

> [!div class="step-by-step"]
> [Předchozí](generating-views.md)
> [další](customizing-a-view.md)
