---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 8e875a4282df78ec647579e74c3fbeabd2495fc2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu provedete změnu databáze a změny související kódu, testovat změny v sadě Visual Studio a pak nasadit aktualizace do prostředí testovací, přípravné nebo produkční prostředí.

Kurz nejprve ukazuje, jak aktualizovat databázi, která spravuje migrace Code First, a pak později ukazuje, jak aktualizace databáze pomocí zprostředkovatele dbDacFx.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Nasazení aktualizace databáze pomocí migrace Code First

V této části můžete přidat sloupec Datum narození `Person` základní třídu pro `Student` a `Instructor` entity. Potom je aktualizovat stránky, která zobrazuje data lektorem tak, aby se zobrazí nový sloupec. Nakonec můžete nasadit změny pro testovací, přípravné nebo produkční prostředí.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Přidat sloupce do tabulky v databázi aplikace

1. V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` – třída (měla by existovat dvě zavřením složené závorky následující ji):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Potom aktualizujte `Seed` metoda, kterou it přináší hodnotu pro nový sloupec. Otevřete *Migrations\Configuration.cs* a nahraďte bloku kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Sestavte řešení a pak otevřete **Konzola správce balíčků** okno. Ujistěte se, že je stále ContosoUniversity.DAL vybrán jako **výchozí projekt**.
3. V **Konzola správce balíčků** vyberte **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Po dokončení tohoto příkazu Visual Studio otevře soubor třídy, který definuje nový `DbMIgration` třída a v `Up` metoda vidíte kód, který vytvoří nový sloupec. `Up` Metoda vytvoří sloupec při implementaci změny a `Down` metoda odstraní sloupec při vrácení zpět změny.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (musí být vybrána projektu ContosoUniversity.DAL stále):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Rozhraní Entity Framework běží `Up` metoda a poté spustí `Seed` metoda.

### <a name="display-the-new-column-in-the-instructors-page"></a>Zobrazit nového sloupce na stránce vyučující

1. Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidejte nové šablony pole, které chcete zobrazit datum narození. Přidáte mezi těm, které jsou pro přiřazení přijetím datum a office:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Pokud odsazení kód získá synchronizován, můžete stisknout CTRL-K a pak CTRL-D automaticky přeformátuje souboru.)
2. Spusťte aplikaci a klikněte na **vyučující** odkaz.

    Při načtení stránky uvidíte, že má nové pole pro datum narození.

    ![Stránka vyučující s datum narození](deploying-a-database-update/_static/image2.png)
3. Zavřete prohlížeč.

### <a name="deploy-the-database-update"></a>Nasaďte aktualizaci databáze

1. V **Průzkumníku řešení** vyberte ContosoUniversity projekt.
2. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **Test** profil publikování a potom klikněte na **Publikovat Web**. (Pokud je zakázána panelu nástrojů, vyberte projekt ContosoUniversity v **Průzkumníku řešení**.)

    Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku.
3. Spustit **vyučující** a ověřte, že aktualizace byla úspěšně nasazena.

    Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizace schématu databáze a spustí `Seed` metoda. Při zobrazení stránky, uvidíte očekávané **datum narození** sloupce s daty v ní.
4. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **pracovní** profil publikování a potom klikněte na **Publikovat Web**.
5. Spustit **vyučující** stránky v pracovní k ověření, že aktualizace byla úspěšně nasazena.
6. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **produkční** profil publikování a potom klikněte na **Publikovat Web**.
7. Spustit **vyučující** stránky v produkčním prostředí, chcete-li ověřit, že aktualizace byla úspěšně nasazena.

    Pro aktualizaci skutečné produkční aplikace, která zahrnuje změnu databáze by také běžně využít aplikace do režimu offline během nasazení pomocí *aplikace\_offline.htm*, jak jste viděli v předchozí kurzu.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Nasazení aktualizace databáze pomocí poskytovatele dbDacFx

V této části přidáte *komentáře* sloupec, který se *uživatele* tabulky v databázi členství a vytvořte stránku, která vám umožní zobrazit a upravit komentáře pro každého uživatele. Pak můžete nasadit změny pro testovací, přípravné nebo produkční prostředí.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Přidat sloupce do tabulky v databázi členství

1. V sadě Visual Studio otevřete **Průzkumník objektů systému SQL Server**.
2. Rozbalte položku **(localdb) \v11.0**, rozbalte položku **databáze**, rozbalte položku **aspnet ContosoUniversity** (ne **aspnet. ContosoUniversity produkčnímu**) a potom rozbalte **tabulky**.

    Pokud nevidíte **(localdb) \v11.0** v části **systému SQL Server** uzel, klikněte pravým tlačítkem myši **systému SQL Server** uzel a klikněte na tlačítko **přidat SQL Server**. V **připojit k serveru** dialogové okno zadejte *(localdb) \v11.0* jako **název serveru**a potom klikněte na **Connect**.

    Pokud nevidíte **aspnet ContosoUniversity**, spusťte projekt a přihlaste se pomocí *správce* přihlašovací údaje (heslo je *devpwd*) a potom aktualizujte  **Průzkumník objektů systému SQL Server** okno.
3. Klikněte pravým tlačítkem myši **uživatelé** tabulky a potom klikněte na **Návrhář zobrazení**.

    ![Návrhář SSOX zobrazení](deploying-a-database-update/_static/image3.png)
4. V návrháři, přidejte *komentáře* sloupce a nastavit jej jako *nvarchar(128)* a s povolenou hodnotou Null a potom klikněte na **aktualizace**.

    ![Přidáním sloupce komentáře](deploying-a-database-update/_static/image4.png)
5. V **aktualizace databáze Preview** pole, klikněte na tlačítko **aktualizace databáze**.

    ![Aktualizace databáze Preview](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Vytvořte stránku k zobrazení a úpravě nového sloupce

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **účet** složky ContosoUniversity projektu, klikněte na tlačítko **přidat**a potom klikněte na **nová položka** .
2. Vytvořte novou **webové formuláře pomocí stránka předlohy** a pojmenujte ji *UserInfo.aspx*. Přijměte výchozí nastavení *Site.Master* souboru stránky předlohy.
3. Zkopírujte následující kód do `MainContent` `Content` – element (posledních 3 `Content` prvky):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Klikněte pravým tlačítkem myši *UserInfo.aspx* a klikněte na tlačítko **zobrazit v prohlížeči**.
5. Přihlaste se pomocí vaší *správce* přihlašovací údaje uživatele (heslo je *devpwd*) a přidejte některé komentáře pro uživatele k ověření, že stránka funguje správně.

    ![Stránka informací o uživateli](deploying-a-database-update/_static/image6.png)
6. Zavřete prohlížeč.

## <a name="deploy-the-database-update"></a>Nasaďte aktualizaci databáze

Nasazení pomocí poskytovatele dbDacFx, stačí vybrat **aktualizace databáze** možnost v profilu publikování. Však pro počáteční nasazení při použití této možnosti můžete také nakonfigurovat spuštění některých dalších skriptů SQL: těch, které jsou stále v profilu a budete se muset zabránit znovu spustit.

1. Otevřete **Publikovat Web** Průvodce pravým tlačítkem na projekt ContosoUniversity a kliknutím na **publikovat**.
2. Vyberte **Test** profilu.
3. Klikněte **nastavení** kartě.
4. V části **objekt DefaultConnection**, vyberte **aktualizace databáze**.
5. Zakažte další skripty, které jste nakonfigurovali pro počáteční nasazení:

    1. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
    2. V **konfigurovat aktualizace databáze** dialogové okno, zrušte políček vedle *Grant.sql* a *aspnet. data dev.sql*.
    3. Klikněte na tlačítko **Zavřít**.
6. Klikněte **Preview** kartě.
7. V části **databáze** a napravo od **objekt DefaultConnection**, klikněte na tlačítko **Preview databáze** odkaz.

    ![K náhledu databáze](deploying-a-database-update/_static/image7.png)

    Okno náhledu zobrazí skript, který se spustí v cílové databázi, aby schéma této databáze odpovídat schématu zdrojové databáze. Tento skript obsahuje příkaz ALTER TABLE, který přidá nový sloupec.
8. Zavřít **náhledu databáze** dialogové okno a pak klikněte na tlačítko **publikovat**.

    Visual Studio nasadí aktualizovanou aplikaci a prohlížeči se otevře na domovskou stránku.
9. Spuštění stránky informací o uživateli (Přidat *Account/UserInfo.aspx* na adresu URL domovské stránce) Chcete-li ověřit, že aktualizace byla úspěšně nasazena. Budete se muset přihlásit zadáním *správce* a *devpwd*.

    Data v tabulkách není nasazený ve výchozím nastavení a nenakonfigurovali skript nasazení do dat spustit, takže nebude možné najít komentář, který jste přidali v vývoj. Teď můžete přidat komentář, nové v pracovním pro ověření, že tato změna byla nasazena do databáze a stránka funguje správně.
10. Postupujte stejným způsobem, k nasazení pro pracovní a provozní.

    Nezapomeňte si zakázat další skripty. Ve srovnání s profilem testovací jediným rozdílem je, že zakážete pouze jeden skript staging a produkční profily, protože byly nakonfigurované ke spuštění pouze *aspnet. produkčnímu data.sql*.

    Správce a prodpwd jsou pověření pro pracovní a provozní.

    Pro aktualizaci skutečné produkční aplikace, která zahrnuje změnu databáze by také běžně trvat aplikace do režimu offline během nasazení tím, že nahrajete *aplikace\_offline.htm* před publikováním a odstranění následně, jako jste viděli v [předchozí kurzu](deploying-a-code-update.md).

## <a name="summary"></a>Souhrn

Nyní jste nasadili aktualizaci aplikace, která zahrnuté změnu databáze pomocí migrace Code First a poskytovatele dbDacFx.

![Stránka vyučující s datum narození](deploying-a-database-update/_static/image8.png)

![Stránka informací o uživateli](deploying-a-database-update/_static/image9.png)

V dalším kurzu se dozvíte, jak provést nasazení pomocí příkazového řádku.

>[!div class="step-by-step"]
[Předchozí](deploying-a-code-update.md)
[další](command-line-deployment.md)
