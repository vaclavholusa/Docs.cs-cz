---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884883"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení aktualizace databáze - 9 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu provedete změnu databáze a změny související kódu, testovat změny v sadě Visual Studio a pak nasadit aktualizace do testovací i produkčním prostředí.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Přidá sloupec do tabulky

V této části můžete přidat sloupec Datum narození `Person` základní třídu pro `Student` a `Instructor` entity. Potom je aktualizovat stránky, která zobrazuje data lektorem tak, aby se zobrazí nový sloupec.

V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` – třída (měla by existovat dvě zavřením složené závorky následující ji):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Potom aktualizujte metodu počáteční hodnoty tak, aby poskytuje hodnotu pro nový sloupec. Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidejte nové šablony pole, které chcete zobrazit datum narození. Přidáte mezi těm, které jsou pro přiřazení přijetím datum a office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Pokud odsazení kód získá synchronizován, můžete stisknout CTRL-K a pak CTRL-D automaticky přeformátuje souboru.)

Sestavte řešení a pak otevřete **Konzola správce balíčků** okno. Ujistěte se, že je stále ContosoUniversity.DAL vybrán jako **výchozí projekt**.

V **Konzola správce balíčků** vyberte **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` třída a v `Up` metoda vidíte kód, který vytvoří nový sloupec.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (musí být vybrána projektu ContosoUniversity.DAL stále):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Až se příkaz dokončí, spusťte aplikaci a vyberte stránku vyučující. Při načtení stránky uvidíte, že má nové pole pro datum narození.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Nasazení aktualizace databáze do testovacího prostředí

V **Průzkumníku řešení** vyberte ContosoUniversity projekt.

V **publikování webu jedním kliknutím** nástrojů, vyberte **Test** profil publikování a potom klikněte na **Publikovat Web**. (Pokud je zakázána panelu nástrojů, vyberte projekt ContosoUniversity v **Průzkumníku řešení**.)

Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku. Spusťte stránku vyučující a ověřte, že aktualizace byla úspěšně nasazena. Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizace schématu databáze a spustí `Seed` metoda. Při zobrazení stránky, uvidíte očekávané **datum narození** sloupce s daty v ní.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Nasazení aktualizace databáze do produkčního prostředí

Teď můžete nasadit do produkčního prostředí. Jediným rozdílem je, že budete používat *aplikace\_offline.htm* lze zabránit uživatelům v přístupu na web a proto aktualizace databáze, zatímco nasazujete změny. Pro produkční nasazení proveďte následující kroky:

- Nahrát *aplikace\_offline.htm* soubor pracoviště.
- V sadě Visual Studio, vyberte profil produkční v **publikování webu jedním kliknutím** panelu nástrojů a klikněte na tlačítko **Publikovat Web**.
- Odstranit *aplikace\_offline.htm* soubor z pracoviště.

> [!NOTE]
> Když aplikaci právě používá v produkčním prostředí by měl být implementace plánu zálohování. To znamená, které by měl být pravidelně kopírování *školní-Prod.sdf* a *aspnet Prod.sdf* soubory z produkční lokality do umístění zabezpečeného úložiště a měli udržuje několik generací například zálohování. Při aktualizaci databáze byste měli vytvořit záložní kopii z bezprostředně před provedením změny. Poté Pokud došlo k chybě a nebudete zjistit až po jeho nasazení do produkčního prostředí, bude stále budete moci obnovit databázi do stavu, ve kterém se nacházel před jeho poškozením.


Když Visual Studio otevře adresu URL domovskou stránku v prohlížeči *aplikace\_offline.htm* zobrazí se stránka. Po odstranění *aplikace\_offline.htm* souboru, můžete přejít na domovskou stránku znovu a ověřte, že aktualizace byla úspěšně nasazena.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Nyní jste nasadili aktualizaci aplikace, která zahrnuté ke změně databáze pro testovací a produkční. V dalším kurzu se dozvíte, jak migrovat databázi z systém SQL Server Compact do systému SQL Server Express a SQL Server.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [další](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
