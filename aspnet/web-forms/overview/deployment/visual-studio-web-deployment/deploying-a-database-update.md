---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817620"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení aktualizace databáze
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu provedete změnu databáze a změny související kód, otestovat změny v sadě Visual Studio a pak Nasaďte aktualizaci do prostředí test, přípravném nebo produkčním prostředí.

Tento kurz nejprve ukazuje, jak aktualizovat databázi, kterou spravuje migrace Code First a pak později ukazuje, jak pomocí poskytovatele dbDacFx aktualizace databáze.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>Nasazení aktualizace databáze pomocí migrace Code First

V této části přidáte sloupec Datum narození `Person` základní třída pro `Student` a `Instructor` entity. Potom aktualizujte stránku, která zobrazuje data kurzů vedených tak, aby zobrazil nový sloupec. Nakonec nasadíte změny do testu, přípravném nebo produkčním prostředí.

### <a name="add-a-column-to-a-table-in-the-application-database"></a>Přidání sloupce do tabulky v databázi aplikace

1. V *ContosoUniversity.DAL* projekt, otevřete *Person.cs* a přidejte následující vlastnost na konci `Person` třídy (měl by existovat dva pravou složenou závorkou následující):

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    Dále, aktualizujte `Seed` metodu tak, že poskytuje hodnoty pro nový sloupec. Otevřít *Migrations\Configuration.cs* a nahraďte blok kódu, který začíná `var instructors = new List<Instructor>` s následující blok kódu, který obsahuje informace o datu narození:

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. Sestavte řešení a pak otevřete **Konzola správce balíčků** okna. Ujistěte se, že ContosoUniversity.DAL je stále vybrán jako **výchozí projekt**.
3. V **Konzola správce balíčků** okně **ContosoUniversity.DAL** jako **výchozí projekt**a potom zadejte následující příkaz:

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    Po dokončení tohoto příkazu, Visual Studio otevře soubor třídy, která definuje nový `DbMIgration` třídy a `Up` metoda se zobrazí kód, který vytvoří nový sloupec. `Up` Metoda vytvoří sloupec při implementaci této změny a `Down` metoda odstraní sloupec při vrácení zpět změnu.

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. Sestavte řešení a potom zadejte následující příkaz v **Konzola správce balíčků** okno (ujistěte se, že je stále vybrán projekt ContosoUniversity.DAL):

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework běží `Up` metody a poté ji spustí `Seed` metoda.

### <a name="display-the-new-column-in-the-instructors-page"></a>Na stránce Instruktoři zobrazit nový sloupec

1. Otevřete v projektu ContosoUniversity *Instructors.aspx* a přidat nové pole šablony se zobrazí datum narození. Přidáte mezi pro zařazení data a office přiřazení:

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    (Pokud odsazení kódu získá synchronizovaný, můžete stisknutím CTRL-K a pak na kombinaci kláves CTRL + D automaticky přeformátovat soubor.)
2. Spusťte aplikaci a klikněte na tlačítko **Instruktoři** odkaz.

    Když se stránka načte, uvidíte, že na něm nové pole pro datum narození.

    ![Stránka Instruktoři s datum narození](deploying-a-database-update/_static/image2.png)
3. Zavřete prohlížeč.

### <a name="deploy-the-database-update"></a>Nasazení aktualizace databáze

1. V **Průzkumníka řešení** vyberte ContosoUniversity projekt.
2. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **testovací** profil publikování a potom klikněte na tlačítko **Publikovat Web**. (Pokud panelu nástrojů je zakázaná, vyberte projekt ContosoUniversity v **Průzkumníka řešení**.)

    Visual Studio nasadí aktualizované aplikace a prohlížeč otevře domovskou stránku.
3. Spustit **Instruktoři** stránku a ověřte, že se aktualizace úspěšně nasadilo.

    Když se aplikace pokusí o přístup k databázi pro tuto stránku, Code First aktualizuje schéma databáze a spustí `Seed` metody. Jakmile se zobrazí na stránce se zobrazí očekávané **datum narození** sloupce s datem, v ní.
4. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **pracovní** profil publikování a potom klikněte na tlačítko **Publikovat Web**.
5. Spustit **Instruktoři** stránky v přípravě, chcete-li ověřit, že se aktualizace úspěšně nasadilo.
6. V **publikování webu jedním kliknutím** nástrojů, klikněte na tlačítko **produkční** profil publikování a potom klikněte na tlačítko **Publikovat Web**.
7. Spustit **Instruktoři** stránky v produkčním prostředí k ověření, že se aktualizace úspěšně nasadilo.

    Pro skutečné produkční aktualizaci aplikace, která zahrnuje změnu databáze by také běžně nastavit aplikaci offline během nasazení s použitím *aplikace\_offline.htm*, jak jste viděli v předchozím kurzu.

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>Nasazení aktualizace databáze s použitím dbDacFx poskytovatele

V této části přidáte *komentáře* sloupec, který se *uživatele* tabulky v databázi členství a vytvořit stránku, která vám umožní zobrazit a upravit komentáře pro každého uživatele. Pak můžete nasadit změny pro test, přípravném nebo produkčním prostředí.

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>Přidání sloupce do tabulky v databázi členství

1. V sadě Visual Studio, otevřete **Průzkumník objektů systému SQL Server**.
2. Rozbalte **(localdb) \v11.0**, rozbalte **databází**, rozbalte **aspnet ContosoUniversity** (ne **aspnet. ContosoUniversity Prod**) a potom rozbalte **tabulky**.

    Pokud nevidíte **(localdb) \v11.0** pod **systému SQL Server** uzel, klikněte pravým tlačítkem na **systému SQL Server** uzel a klikněte na tlačítko **přidat SQL Server**. V **připojit k serveru** dialogového okna zadejte *(localdb) \v11.0* jako **název serveru**a potom klikněte na tlačítko **připojit**.

    Pokud nevidíte **aspnet ContosoUniversity**, spusťte projekt a přihlaste se pomocí *správce* přihlašovací údaje (heslo je *devpwd*) a potom aktualizovat  **Průzkumník objektů systému SQL Server** okna.
3. Klikněte pravým tlačítkem myši **uživatelé** tabulku a pak klikněte na **Návrhář zobrazení**.

    ![Návrhář zobrazení SSOX](deploying-a-database-update/_static/image3.png)
4. V návrháři, přidejte *komentáře* sloupec a nastavte ji *nvarchar(128)* a s povolenou hodnotou Null a potom klikněte na tlačítko **aktualizace**.

    ![Přidání sloupce komentáře](deploying-a-database-update/_static/image4.png)
5. V **náhled aktualizací databáze** klikněte **aktualizace databáze**.

    ![Náhled aktualizací databáze](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>Vytvoření stránky můžete zobrazit a upravit nový sloupec

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na **účet** klikněte na tlačítko složky v projektu ContosoUniversity **přidat**a potom klikněte na tlačítko **novou položku** .
2. Vytvořte nový **webové formuláře pomocí stránka předlohy** a pojmenujte ho *UserInfo.aspx*. Přijměte výchozí nastavení *Site.Master* souboru stránky předlohy.
3. Zkopírujte následující kód do `MainContent` `Content` – element (poslední 3 `Content` prvky):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. Klikněte pravým tlačítkem myši *UserInfo.aspx* stránky a klikněte na tlačítko **zobrazit v prohlížeči**.
5. Přihlášení vaše *správce* přihlašovací údaje uživatele (heslo je *devpwd*) a přidat nějaké komentáře k uživateli a ověřte, že na stránce funguje správně.

    ![Stránka informací o uživateli](deploying-a-database-update/_static/image6.png)
6. Zavřete prohlížeč.

## <a name="deploy-the-database-update"></a>Nasazení aktualizace databáze

Pokud chcete nasadit s použitím poskytovatele dbDacFx, stačí vybrat **aktualizace databáze** možnost v profilu publikování. Ale pro počáteční nasazení při použití této možnosti můžete také nakonfigurovat některé další skripty SQL ke spuštění: to jsou stále v profilu a budete mít tak tomu, aby znovu.

1. Otevřít **Publikovat Web** Průvodce pravým tlačítkem myši projekt ContosoUniversity a kliknutím na **publikovat**.
2. Vyberte **Test** profilu.
3. Klikněte na tlačítko **nastavení** kartu.
4. V části **objekt DefaultConnection**vyberte **aktualizace databáze**.
5. Zakažte další skripty, které jste nakonfigurovali spuštění pro počáteční nasazení:

    1. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
    2. V **konfigurace aktualizací databáze** dialogové okno, zrušte zaškrtnutí políček vedle *Grant.sql* a *aspnet-data-dev.sql*.
    3. Klikněte na tlačítko **Zavřít**.
6. Klikněte na tlačítko **ve verzi Preview** kartu.
7. V části **databází** a napravo od **objekt DefaultConnection**, klikněte na tlačítko **databáze ve verzi Preview** odkaz.

    ![Database ve verzi Preview](deploying-a-database-update/_static/image7.png)

    V okně verze preview se zobrazí skript, který se spustí v cílové databázi, aby toto schéma databáze podle schématu zdrojové databáze. Skript obsahuje pomocí příkazu ALTER TABLE, který přidá nový sloupec.
8. Zavřít **Database ve verzi Preview** dialogové okno a potom klikněte na **publikovat**.

    Visual Studio nasadí aktualizované aplikace a prohlížeč otevře domovskou stránku.
9. Stránka informací o uživateli spustit (Přidat *Account/UserInfo.aspx* na adresu URL domovské stránky) Chcete-li ověřit, že se aktualizace úspěšně nasadilo. Budete se muset přihlásit tak, že zadáte *správce* a *devpwd*.

    Ve výchozím nastavení není nasazený data v tabulkách a nenakonfigurovala skriptu nasazení dat. Pokud chcete spustit, nebude najdou komentář, který jste přidali ve vývoji. Nyní můžete přidat nový komentář v přípravě, chcete-li ověřit, že tato změna byla nasazena do databáze a na stránce funguje správně.
10. Postupujte stejným způsobem pro nasazení do pracovního a produkčního prostředí.

    Nezapomeňte vypnout další skripty. Ve srovnání s testovacího profilu jediným rozdílem je, že zakážete pouze jeden skript do pracovního a produkčního prostředí profily, protože byly nakonfigurované ke spuštění pouze *aspnet-prod-data.sql*.

    Přihlašovací údaje pro pracovního a produkčního prostředí jsou správce a prodpwd.

    Pro skutečné produkční aktualizaci aplikace, která zahrnuje změnu databáze by také běžně nastavit aplikaci offline během nasazení tak, že nahrajete *aplikace\_offline.htm* před publikováním a jeho odstranění následně, protože jste viděli v [předchozí kurz o službě](deploying-a-code-update.md).

## <a name="summary"></a>Souhrn

Nasadili jste nyní zahrnující Změna databáze pomocí migrace Code First a poskytovateli dbDacFx aktualizace aplikace.

![Stránka Instruktoři s datum narození](deploying-a-database-update/_static/image8.png)

![Stránka informací o uživateli](deploying-a-database-update/_static/image9.png)

V dalším kurzu se dozvíte, jak spustit nasazení pomocí příkazového řádku.

> [!div class="step-by-step"]
> [Předchozí](deploying-a-code-update.md)
> [další](command-line-deployment.md)
