---
uid: mvc/overview/getting-started/database-first-development/setting-up-database
title: "Začínáme s Entity Framework 6 Database First pomocí MVC 5 | Microsoft Docs"
author: tfitzmac
description: "Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tento kurz seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 095abad4-3bfe-4f06-b092-ae6a735b7e49
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/setting-up-database
msc.type: authoredcontent
ms.openlocfilehash: cb979333131cc6ac87fd640bf7c96931054a1814
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-database-first-using-mvc-5"></a>Začínáme s Entity Framework 6 Database First pomocí MVC 5
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Pomocí generování uživatelského rozhraní ASP.NET, MVC a Entity Framework, můžete vytvořit webovou aplikaci, která poskytuje rozhraní k existující databázi. Tato řada kurzu se dozvíte, jak automaticky vygenerovat kód, který umožňuje uživatelům zobrazit, upravit, vytvořte a odstraňovat data, která se nachází v tabulce databáze. Generovaný kód odpovídá sloupců v tabulce databáze. V poslední části řady nasadíte lokalitu a databázi do Azure.
> 
> Tato část řady se zaměřuje na vytvoření databáze a vyplnění daty.
> 
> Tato řada byla zapsána s příspěvky z tní Dykstra a Rick Anderson. Byl vylepšen na základě na zpětnou vazbu od uživatelů v sekci komentáře.


## <a name="introduction"></a>Úvod

Toto téma ukazuje, jak spustit s existující databáze a rychle vytvořit webovou aplikaci, která umožňuje uživatelům interakci s daty. Entity Framework 6 a MVC 5 používá k vytvoření webové aplikace. Funkce ASP.NET generování uživatelského rozhraní umožňuje automaticky generovat kód pro zobrazení, aktualizaci, vytváření a odstraňování dat. Pomocí nástroje pro publikování v sadě Visual Studio, můžete snadno nasadit lokalitu a databázi do Azure.

Toto téma nabízí situaci, kdy máte databázi a chcete pro generování kódu pro webovou aplikaci na základě polí této databáze. Tento postup se nazývá Database First vývoj. Pokud již nemáte existující databázi, můžete místo toho volat Code First vývoj, který zahrnuje definování datových tříd a generování databáze z vlastnosti třídy přístup.

Příklad úvodní Code First vývoj naleznete v části [Začínáme s ASP.NET MVC 5](../introduction/getting-started.md). Pokročilejší příklad najdete v tématu [vytváření datového modelu Entity Framework pro aplikace ASP.NET MVC 4](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Informace o výběru přístupem Entity Framework, najdete v části [Entity Framework vývoj přístupy](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).

## <a name="prerequisites"></a>Požadavky

Visual Studio 2013 nebo Visual Studio Express 2013 pro Web

## <a name="set-up-the-database"></a>Nastavení databáze

Aby napodobovaly prostředí, že máte existující databázi, nejprve vytvořit databázi s některá předem vyplněné data a poté vytvořit webové aplikace, která se připojuje k databázi.

V tomto kurzu byla vyvinuta pomocí LocalDB Visual Studio 2013 nebo Visual Studio Express 2013 pro Web. Můžete použít existující databázový server místo LocalDB, ale v závislosti na vaší verzi sady Visual Studio a váš typ databáze, všechny nástroje data v sadě Visual Studio nemusí být podporován. Pokud nejsou k dispozici pro vaši databázi nástroje, musíte provést některé z kroků specifické pro databáze v sadě management pro vaši databázi.

Pokud máte potíže s databázové nástroje ve vaší verzi sady Visual Studio, ujistěte se, jestli že je nainstalovaná nejnovější verze nástroje databáze. Informace o aktualizaci nebo instalaci databáze nástroje najdete v tématu [Microsoft SQL Server Data Tools](https://msdn.microsoft.com/data/hh297027).

Spusťte sadu Visual Studio a vytvořte **projekt databáze serveru SQL**. Název projektu **ContosoUniversityData**.

![Vytvoření databáze projektu](setting-up-database/_static/image1.png)

Nyní máte projekt prázdnou databázi. Tato databáze do Azure nasadí později v tomto kurzu, takže budete muset nastavit Azure SQL Database jako cílová platforma pro projekt. Nastavení cílové platformy není nasazen ve skutečnosti databázi; pouze znamená, že projekt databáze bude ověřte, zda návrhu databáze je kompatibilní s cílovou platformu. Chcete-li nastavit cílovou platformu, otevřete **vlastnosti** pro projekt a vyberte **Microsoft Azure SQL Database** pro cílovou platformu.

![Sada cílové platformy](setting-up-database/_static/image2.png)

Můžete vytvořit tabulky pro tento kurz potřeba přidáním skripty SQL, které určují tabulky. Klikněte pravým tlačítkem na projekt a přidejte novou položku.

![Přidat novou položku](setting-up-database/_static/image3.png)

Přidá novou tabulku s názvem Student.

![Přidat tabulka student](setting-up-database/_static/image4.png)

V tabulce souboru nahraďte příkaz T-SQL následující kód k vytvoření tabulky.

[!code-sql[Main](setting-up-database/samples/sample1.sql)]

Všimněte si, že okno návrh automaticky synchronizuje s kódem. Můžete pracovat s kódem nebo designer.

![Zobrazit kód a návrh](setting-up-database/_static/image5.png)

Přidejte jinou tabulkou. Tento čas název postupu a použijte následující příkaz T-SQL.

[!code-sql[Main](setting-up-database/samples/sample2.sql)]

A opakujte ještě jednou a vytvořte tabulku s názvem registrace.

[!code-sql[Main](setting-up-database/samples/sample3.sql)]

Jej můžete naplnit databázi daty pomocí skriptu, který se spouští po nasazení databáze. Po nasazení skriptu přidáte do projektu. Můžete použít výchozí název.

![Přidejte skript po nasazení](setting-up-database/_static/image6.png)

Přidejte následující kód T-SQL skriptu po nasazení. Tento skript jednoduše přidá data do databáze, pokud se nenajde žádný odpovídající záznam. Nemá přepsat nebo odstranit data, která jste zadali do databáze.

[!code-sql[Main](setting-up-database/samples/sample4.sql)]

Je důležité si uvědomit, že po nasazení skript je spuštěn pokaždé, když nasazujete projekt databáze. Proto musíte pečlivě zvažte své požadavky na při zápisu tohoto skriptu. V některých případech můžete chtít začít znovu od známých sadu dat pokaždé, když je projekt nasazen. V ostatních případech nemusíte chtít změnit stávající data žádným způsobem. Podle potřeb, můžete rozhodnout, zda je nutné spuštění skriptu po nasazení nebo co je potřeba zahrnout do skriptu. Další informace o naplnění databáze pomocí skriptu po nasazení najdete v tématu [včetně dat v projektu databáze serveru SQL](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx).

Nyní máte 4 soubory skriptu SQL, ale žádné skutečné tabulky. Jste připravení nasadit projekt databáze na instanci localdb. V sadě Visual Studio klikněte na tlačítko Start (F5) pro sestavení a nasadit projekt databáze. Zkontrolujte kartu výstup a ověřte, zda sestavení a nasazení proběhla úspěšně.

![Zobrazit výstup](setting-up-database/_static/image7.png)

Pokud chcete zobrazit, že byla vytvořena nová databáze, otevřete **Průzkumník objektů systému SQL Server** a podívejte se na název projektu na správné místní databázi serveru (v tomto případě **(localdb) \ProjectsV12**).

![Zobrazit novou databázi](setting-up-database/_static/image8.png)

Pokud chcete zobrazit, v tabulkách jsou naplněný daty, klikněte pravým tlačítkem na tabulku a vyberte **Data zobrazení**.

![Zobrazit data tabulky](setting-up-database/_static/image9.png)

Zobrazí se upravitelné zobrazení dat v tabulce.

![Zobrazit výsledky tabulky dat](setting-up-database/_static/image10.png)

Vaše databáze je nyní nastavení a naplněný daty. V dalším kurzu vytvoříte webovou aplikaci pro databázi.

>[!div class="step-by-step"]
[Next](creating-the-web-application.md)
