---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: e94281e36192c91a04392735af318bbc517b0521
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382456"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu provedete změnu databáze a změny související kód, otestovat změny v sadě Visual Studio a pak Nasaďte aktualizaci do testovacích i produkčních prostředí.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Přidá sloupec do tabulky

V této části přidáte sloupec Datum narození `Person` základní třída pro `Student` a `Instructor` entity. Potom aktualizujte stránku, která zobrazuje data kurzů vedených tak, aby zobrazil nový sloupec.

V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` třídy (měl by existovat dva pravou složenou závorkou následující):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

V dalším kroku aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnoty pro nový sloupec. Otevřít *Migrations\Configuration.cs* a nahraďte blok kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové pole šablony se zobrazí datum narození. Přidáte mezi pro zařazení data a office přiřazení:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Pokud odsazení kódu získá synchronizovaný, můžete stisknutím CTRL-K a pak na kombinaci kláves CTRL + D automaticky přeformátovat soubor.)

Sestavte řešení a pak otevřete **Konzola správce balíčků** okna. Ujistěte se, že ContosoUniversity.DAL je stále vybrán jako **výchozí projekt**.

V **Konzola správce balíčků** okně **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` třídy a `Up` metoda se zobrazí kód, který vytvoří nový sloupec.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (ujistěte se, že je stále vybrán projekt ContosoUniversity.DAL):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Až se příkaz dokončí, spusťte aplikaci a vyberte stránku Instruktoři. Když se stránka načte, uvidíte, že na něm nové pole pro datum narození.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Nasazení aktualizace databáze do testovacího prostředí

V **Průzkumníka řešení** vyberte ContosoUniversity projekt.

V **publikování webu jedním kliknutím** nástrojů, vyberte **testovací** profil publikování a potom klikněte na tlačítko **Publikovat Web**. (Pokud panelu nástrojů je zakázaná, vyberte projekt ContosoUniversity v **Průzkumníka řešení**.)

Visual Studio nasadí aktualizované aplikace a prohlížeč otevře domovskou stránku. Spustíte školitelů stránky a ověřte, že se aktualizace úspěšně nasadilo. Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizuje schéma databáze a spustí `Seed` metody. Jakmile se zobrazí na stránce se zobrazí očekávané **datum narození** sloupce s datem, v ní.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Nasazení aktualizace databáze do produkčního prostředí

Teď můžete nasadit do produkčního prostředí. Jediným rozdílem je, že budete používat *aplikace\_offline.htm* lze zabránit uživatelům v přístupu k webu a proto aktualizace databáze, zatímco nasazujete změny. Pro produkční nasazení proveďte následující kroky:

- Nahrát *aplikace\_offline.htm* souboru na pracoviště.
- V sadě Visual Studio, zvolte profilu do produkčního prostředí **publikování webu jedním kliknutím** nástrojů a klikněte na tlačítko **Publikovat Web**.
- Odstranit *aplikace\_offline.htm* soubor z pracoviště.

> [!NOTE]
> Když vaše aplikace je používána v provozním prostředí by měl být implementace plánu zálohování. To znamená, které by měl být pravidelně kopírování *školní-Prod.sdf* a *aspnet Prod.sdf* soubory z produkční lokality do zabezpečeného úložiště, a měli udržování několika generací těchto zálohování. Při aktualizaci databáze, měli byste zajistit záložní kopie z bezprostředně před provedením změny. Potom Pokud došlo k chybě a není objevit až po jeho nasazení do produkčního prostředí, stále budete moci obnovit databázi do stavu, ve kterém byl předtím, než začal být poškozen.


Když Visual Studio otevře adresu URL domovské stránky v prohlížeči *aplikace\_offline.htm* zobrazí se stránka. Po odstranění *aplikace\_offline.htm* soubor, můžete přejít na domovskou stránku znovu a ověřte, že se aktualizace úspěšně nasadilo.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Nyní jste nasadili aktualizaci aplikace, která zahrnuté změny databáze do testovacího a produkčního prostředí. V dalším kurzu se dozvíte, jak migrovat databázi ze systému SQL Server Compact do systému SQL Server Express a SQL Server.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [další](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
