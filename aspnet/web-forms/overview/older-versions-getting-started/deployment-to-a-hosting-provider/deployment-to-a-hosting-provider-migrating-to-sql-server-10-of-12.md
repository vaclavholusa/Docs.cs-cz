---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: migrace na SQL Server – 10 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 31d83a11488212ab0ff83494d5e896ffcbeaa8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: migrace na SQL Server – 10 12
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak migrovat ze systému SQL Server Compact do systému SQL Server. Jedním z důvodů, které že můžete k tomu je využít výhod funkcí systému SQL Server, které systém SQL Server Compact nepodporuje, jako jsou uložené procedury, triggery, zobrazení nebo replikace. Další informace o rozdílech mezi systém SQL Server Compact a SQL Server najdete v tématu [nasazení systému SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) kurzu.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express a plnou instalaci systému SQL Server pro vývoj

Jakmile jste se rozhodli upgradujte na SQL Server, můžete chtít používat SQL Server nebo SQL Server Express v vývojová a testovací prostředí. Kromě rozdíly v nástroj pro podporu a databázové stroje funkce jsou rozdíly v implementacích zprostředkovatele mezi systém SQL Server Compact a jiné verze systému SQL Server. Tyto rozdíly může způsobit stejný kód ke generování odlišné výsledky. Proto pokud se rozhodnete zachovat SQL Server Compact jako vývoj databáze, měli byste důkladně otestovat váš web v systému SQL Server nebo SQL Server Express v testovacím prostředí před každé nasazení do produkčního prostředí.

Na rozdíl od systém SQL Server Compact je v podstatě stejný databázový stroj SQL Server Express a používá stejný zprostředkovatel .NET jako plnou instalaci systému SQL Server. Při testování pomocí serveru SQL Server Express, může být jisti, získání stejné výsledky jako budete s SQL serverem. Většina nástroje stejné databáze můžete použít s SQL Server Express, který můžete použít s SQL serverem (významné výjimky se [SQL Server Profiler](https://msdn.microsoft.com/en-us/library/ms181091.aspx)), a podporuje další funkce systému SQL Server jako uložené procedury, triggery, a zobrazení a replikace. (Obvykle musíte použít úplnou systému SQL Server v provozním webu, ale. Ve sdíleném hostování prostředí, můžete spustit SQL Server Express, ale nebyl navržen, pro který a mnoho poskytovatelů hostingu nepodporují.)

Pokud používáte Visual Studio 2012, obvykle zvolíte SQL serveru Express LocalDB pro vývojové prostředí protože se jedná o tom, co je nainstalované ve výchozím nastavení pomocí sady Visual Studio. Ale LocalDB nefunguje ve službě IIS, takže pro testovací prostředí, budete muset používat SQL Server nebo SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Kombinování databáze a zachování jejich samostatné

Aplikace Contoso univerzity má dvě databáze systému SQL Server Compact: databáze členství (*aspnet.sdf*) a také databázi aplikace (*School.sdf*). Když provádíte migraci, můžete migrovat tyto databáze do dvou samostatných databází nebo do jedné databáze. Můžete kombinovat pro usnadnění databáze spojení mezi aplikační databáze a databáze členství. Plán hostování může také zadat příslušný důvod kombinovat. Například poskytovatel hostingu může účtují informace pro více databází nebo nemusí povolit i více než jednu databázi. Je to tento případ se Cytanium Lite hostování účet, který se používá pro tento kurz, který umožňuje pouze jednu databázi systému SQL Server.

V tomto kurzu budete migrovat dvě databáze tímto způsobem:

- Migrujte na dvě databáze LocalDB ve vývojovém prostředí.
- Migrace do obou databází systému SQL Server Express v testovacím prostředí.
- Migrujte jeden kombinované úplnou databázi serveru SQL Server v provozním prostředí.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalace SQL Server Express.

SQL Server Express je automaticky nainstalována ve výchozím nastavení s Visual Studio 2010, ale ve výchozím nastavení nenainstaluje se sadou Visual Studio 2012. Pokud chcete nainstalovat SQL Server 2012 Express, klikněte na následující odkaz

- [Systém SQL Server Express 2012](https://www.microsoft.com/en-us/download/details.aspx?id=29062)

Zvolte *CSY/x64/SQLEXPR\_x64\_ENU.exe* nebo *csy/x86/SQLEXPR\_x86\_ENU.exe*a v Průvodci instalací přijmete výchozí hodnoty nastavení. Další informace o možnostech instalace najdete v tématu [instalace systému SQL Server 2012 z Průvodce instalací (Instalační program)](https://msdn.microsoft.com/en-us/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Vytváření databází systému SQL Server Express pro testovací prostředí

Dalším krokem je vytvoření členství technologie ASP.NET a školní databáze.

Z **zobrazení** nabídky vyberte možnost **Průzkumníka serveru** (**Průzkumník databáze** v aplikaci Visual Web Developer) a potom klikněte pravým tlačítkem na **připojení dat**a vyberte **vytvořit novou databázi SQL serveru**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

V **vytvořit novou databázi SQL serveru** dialogovém okně zadejte ". \SQLExpress" v **název serveru** pole a "aspnet-Test" v **nový název databáze** pole a pak klikněte na **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Postupujte stejným způsobem, chcete-li vytvořit novou databázi SQL serveru Express školní s názvem "Školní-Test".

(Přidáváte "Test" tyto názvy databází protože později vytvoříte další instanci každou databázi pro vývojové prostředí a musíte být schopni rozlišit dvě sady databází.)

**V Průzkumníku serveru** nyní zobrazuje dvě nové databáze.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Vytváření skriptu udělení nové databáze

Když aplikace běží ve službě IIS na vývojovém počítači, aplikace přistupovat k databázi pomocí přihlašovacích údajů výchozí fond aplikací. Nicméně ve výchozím nastavení, identita fondu aplikací nemá oprávnění k otevření databáze. Proto je třeba spustit skript k udělení tohoto oprávnění. V této části můžete vytvořit skript, že je potřeba spustit později a ujistěte se, že aplikace bude moci otevřít databáze při spuštění v IIS.

V rámci řešení *SolutionFiles* složku, kterou jste vytvořili v [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu, vytvořte nový soubor SQL s názvem *Grant.sql*. Zkopírujte následující příkazy SQL do souboru a uložte a zavřete soubor:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Tento skript je navržen pro práci s SQL Server 2008 a nastavení služby IIS v systému Windows 7, jako jsou uvedené v tomto kurzu. Pokud používáte jinou verzi systému SQL Server nebo Windows, nebo pokud jste nastavili služby IIS v počítači odlišně, může být potřeba změny tohoto skriptu. Další informace o skripty SQL serveru najdete v tématu [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Poznámka k zabezpečení** tento skript poskytuje db\_oprávnění vlastníka na uživatele, který přistupuje k databázi v době běhu, což je budete mít v provozním prostředí. V některých případech můžete chtít zadat uživatele, který má úplné databáze schéma aktualizovat oprávnění pouze pro nasazení a zadáte jiný uživatel, který má oprávnění pouze ke čtení a zápis dat pro běhu. Další informace najdete v tématu **Kontrola automatické změny souboru Web.config pro migrace Code First** v [nasazení do IIS jako testovacím prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurace nasazení databáze pro testovací prostředí

Visual Studio v dalším kroku nakonfigurujete tak, že provede následující úlohy pro jednotlivé databáze:

- Generovat skript SQL, který se vytvoří struktura zdrojové databáze (tabulky, sloupce, omezení atd.) v cílové databázi.
- Generovat skript SQL, který vkládá data zdrojové databáze do tabulek v cílové databázi.
- Spusťte generovaných skriptů a udělte skript, který jste vytvořili v cílové databázi.

Otevřete **vlastnosti projektu** okno a vybrat **balení/publikování kódu SQL** kartě.

Ujistěte se, že **aktivní (verze)** nebo **verze** vybrán **konfigurace** rozevíracího seznamu.

Klikněte na tlačítko **povolit tuto stránku**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**Balení/publikování kódu SQL** karta je obvykle zakázána, protože určuje způsob starší verze nasazení. Pro většinu scénářů byste měli nakonfigurovat nasazení databáze v **Publikovat Web** průvodce. Migrace ze systému SQL Server Compact do systému SQL Server nebo SQL Server Express je zvláštní případ, pro který tato metoda je vhodná.

Klikněte na tlačítko **Import ze souboru Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Vyhledá připojovací řetězce v sadě Visual Studio *Web.config* soubor, vyhledá jeden pro databázi členství a jeden pro databázi školní a přidá řádek odpovídající každé připojovací řetězec **položky databáze**  tabulky. Připojovací řetězce, najde se pro existující databáze systému SQL Server Compact a dalším krokem bude nakonfigurovat, jak a kde nasadit těchto databází.

Zadejte nastavení nasazení databáze **podrobnosti položky databáze** části **položky databáze** tabulky. Nastavení zobrazené v **podrobnosti položky databáze** podle toho, co řádek v části se týkají **položky databáze** vybrané tabulky, jak je znázorněno na následujícím obrázku.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurace nastavení nasazení pro databázi členství

Vyberte **objekt DefaultConnection nasazení** řádek v **položky databáze** tabulky, chcete-li nakonfigurovat nastavení vztahující se k databázi členství.

V **připojovací řetězec pro cílovou databázi**, zadejte připojovací řetězec, který odkazuje na novou databázi SQL Server Express členství. Můžete získat připojovací řetězec, je nutné z **Průzkumníka serveru**. V **Průzkumníka serveru**, rozbalte položku **připojení dat** a vyberte **aspnetTest** databáze, pak z **vlastnosti** okno kopie **Připojovací řetězec** hodnotu.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Stejný připojovací řetězec je uveden zde:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Zkopírujte a vložte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartě.

Ujistěte se, že **stáhnout data nebo schéma z existující databáze** je vybrána. Je to, co způsobí, že skripty SQL, které automaticky generuje a spusťte v cílové databázi.

**Připojovací řetězec pro zdrojové databáze** hodnota je rozbalena z *Web.config* soubor a odkazuje na databázi systému SQL Server Compact vývoj. Toto je zdrojové databáze, který se použije ke generování skriptů, které se spustí později v cílové databázi. Vzhledem k tomu, že chcete nasadit produkční verzi databáze, změňte "aspnet-Dev.sdf" na "aspnet-Prod.sdf".

Změna **databáze možnosti skriptování** z **pouze schéma** k **schéma a data**, vzhledem k tomu, že chcete zkopírovat data (uživatelských účtů a rolí) a také strukturu databáze.

Chcete-li konfigurovat nasazení ke spouštění skriptů udělení, které jste předtím vytvořili, budete muset přidat je do **databázové skripty** části. Klikněte na tlačítko **přidat skript**a v **přidat skripty SQL** dialogové okno pole, přejděte do složky, kam jste uložili skript grant (to je složka, která obsahuje váš soubor řešení). Vyberte soubor s názvem *Grant.sql*a klikněte na tlačítko **otevřete**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Nastavení **objekt DefaultConnection nasazení** řádek v **položky databáze** teď měl vypadat jako na následujícím obrázku:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurace nastavení nasazení pro databázi školy

Potom vyberte **SchoolContext nasazení** řádek v **položky databáze** tabulky, chcete-li nakonfigurovat nastavení nasazení pro databázi školy.

Můžete použít stejnou metodu, kterou jste použili dříve k získání připojovacího řetězce pro novou databázi SQL Server Express. Zkopírujte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartě.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Ujistěte se, že **stáhnout data nebo schéma z existující databáze** je vybrána.

**Připojovací řetězec pro zdrojové databáze** hodnota je rozbalena z *Web.config* soubor a odkazuje na databázi systému SQL Server Compact vývoj. Změňte "Školní-Dev.sdf" na "Školní-Prod.sdf" nasazení produkční verzi databáze. (Vůbec vytvořen soubor školní Prod.sdf v aplikaci\_složky dat, takže budete zkopírovat daný soubor z testovacího prostředí do aplikace\_složky dat ve složce projektu ContosoUniversity později.)

Změna **databáze možnosti skriptování** k **schéma a data**.

Chcete spustit skript, který chcete udělit pro čtení a zápisu oprávnění pro tuto databázi k identitě fondu aplikací, takže přidat *Grant.sql* soubor skriptu, jako jste to udělali pro databázi členství.

Když jste hotovi, nastavení **SchoolContext nasazení** řádek v **položky databáze** vypadat podobně jako na následujícím obrázku:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Uložit změny **balení/publikování kódu SQL** kartě.

Kopírování *školní-Prod.sdf* souboru z *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* složku pro *aplikace\_Data* složky v ContosoUniversity projektu.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Určení transakčního režimu pro udělení skriptu

Proces nasazení generuje skripty, které nasazení schématu databáze a data. Ve výchozím nastavení jsou tyto skripty spustit v transakci. Vlastní skripty (např. skripty grant) ve výchozím nastavení se ale nespustí v transakci. Pokud proces nasazení zkombinuje režimy transakce, můžete získat vypršení časového limitu při spuštění skriptů při nasazení. V této části upravte soubor projektu chcete-li nakonfigurovat vlastní skripty se spustí v transakci.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projektu a vyberte **uvolnit projekt**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Potom znovu klikněte pravým tlačítkem na projekt a vyberte **upravit ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Editoru Visual Studio zobrazí obsah XML souboru projektu. Všimněte si, že existuje několik `PropertyGroup` elementy. (V bitové kopii, obsah `PropertyGroup` elementy byly vynechány.)

![Okno editoru souboru projektu](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

První z nich, který neobsahuje žádné `Condition` atributu, je pro konfiguraci nastavení, které platí bez ohledu na to sestavení. Jeden `PropertyGroup` element platí pouze pro konfiguraci ladění sestavení (Poznámka: `Condition` atribut), jeden platí pouze pro konfiguraci sestavení verze a jeden platí pouze pro testovací konfigurace sestavení. V rámci `PropertyGroup` element pro konfiguraci sestavení verze, zobrazí se `PublishDatabaseSettings` element, který obsahuje nastavení, které jste zadali **balení/publikování kódu SQL** kartě. Došlo `Object` element, který odpovídá všechny skripty grant zadané (Všimněte si dvě instance objektu "Grant.sql"). Ve výchozím nastavení `Transacted` atribut `Source` element pro každý skript grant je `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Změňte hodnotu `Transacted` atribut `Source` element `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Uložte a zavřete soubor projektu, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **znovu načíst projekt**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Nastavení služby transformace Web.Config pro připojovací řetězce

Řetězce připojení pro nové SQL Express databáze, kterou jste zadali **balení/publikování kódu SQL** kartě používají Web Deploy pouze pro aktualizaci v cílové databázi během nasazení. Stále nutné nastavit *Web.config* transformace tak, aby připojení řetězců v nasazené *Web.config* souboru, přejděte na nové databáze SQL Server Express. (Při použití **balení/publikování kódu SQL** kartě připojovací řetězce nelze konfigurovat v profilu publikování.)

Otevřete *Web.Test.config* a nahraďte `connectionStrings` element s `connectionStrings` element v následujícím příkladu. (Ujistěte se, že zkopírujete elementu connectionStrings není okolního kód, který se zobrazí tady zadejte kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Tento kód způsobí, že `connectionString` a `providerName` atributy jednotlivých `add` elementu, který chcete nahradit v nasazené *Web.config* souboru. Tyto řetězce připojení nejsou identické na ty, které jste zadali v **balení/publikování kódu SQL** kartě. Nastavení "MultipleActiveResultSets = True" je přidaný do nich, protože je to požadováno rozhraní Entity Framework a Universal Providers.

## <a name="installing-sql-server-compact"></a>Instalace serveru SQL Server Compact

Balíček SqlServerCompact NuGet poskytuje modul sestavení pro aplikaci Contoso univerzity databázi systému SQL Server Compact. Ale nyní není aplikace ale nasazení webu, který musí být možné číst databáze systému SQL Server Compact, aby bylo možné vytvořit skripty se spustí v databáze systému SQL Server. Chcete-li Web Deploy ke čtení databáze systému SQL Server Compact, nainstalujte SQL Server Compact na vývojovém počítači pomocí následujícího odkazu: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Nasazení do testovacího prostředí

Chcete-li publikovat do testovacího prostředí, je nutné vytvořit profil publikování, který je nakonfigurován pro použití **balení/publikování kódu SQL** kartu pro publikování místo databáze nastavení profilu publikování databáze.

Nejprve odstraňte stávající profil testu.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profil** kartě.

Klikněte na tlačítko **spravovat profily**.

Vyberte **Test**, klikněte na tlačítko **odebrat**a potom klikněte na **Zavřít**.

Zavřít **Publikovat Web** průvodce uložte tuto změnu.

Dále vytvořte nový profil Test a ji použít k publikování tohoto projektu.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profil** kartě.

Vyberte  **&lt;nové... &gt;**  z rozevíracího seznamu a zadejte "Test" jako název profilu.

V **adresa URL služby** zadejte *localhost*.

V **web nebo aplikaci** zadejte *Default Web Site/ContosoUniversity*.

V **cílová adresa URL** zadejte `http://localhost/ContosoUniversity/`.

Klikněte na tlačítko **Další**.

**Nastavení** vás varuje, karta, **balení/publikování kódu SQL** kartě nebyl nakonfigurován, a dává příležitost k přepsání je kliknutím na Povolit publikování vylepšení nové databáze. Pro toto nasazení nechcete přepsat **balení/publikování kódu SQL** nastavení, takže stačí kliknout na **Další**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Zprávy při **Preview** kartě znamená, že **nejsou vybrány žádné databáze k publikování**, ale to jenom znamená, že publikování databáze není nastaveno v profilu publikování.

Klikněte na tlačítko **publikování**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio nasadí aplikaci a otevře prohlížeč na domovské stránce webu v testovacím prostředí. Spuštění stránky vyučující zobrazíte zobrazuje stejná data, která jste viděli dříve. Spustit **přidat studenty** přidat nový student a následně zobrazit nové student v **studenty** stránky. Tím ověříte, že můžete aktualizovat databázi. Vyberte **aktualizace kredity** stránky (budete potřebovat k přihlášení) Chcete-li ověřit, zda byla nasazená databáze členství a zda máte přístup k němu.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Vytvoření databáze SQL serveru pro produkční prostředí

Teď, když máte ukázku nasazenou do testovacího prostředí, jste připravení nastavit nasazení do produkčního prostředí. Můžete začít, jako jste to udělali pro testovací prostředí, tak, že vytvoříte databázi k nasazení. Jak si Vzpomínáte z přehledu, Cytanium Lite hostingový plán umožňuje použití jenom jedné databáze systému SQL Server, tak nastavíte pouze jedna databáze není dva. Všechny tabulky a data z členství a školní SQL Server Compact databází se nasadí do jedné databáze systému SQL Server v provozním prostředí.

Přejděte do ovládacích panelů Cytanium v [http://panel.cytanium.com](http://panel.cytanium.com). Ukazatelem myši na **databáze** a pak klikněte na **systému SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

V **systému SQL Server 2008** klikněte na tlačítko **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Název databáze "Škole" a klikněte na tlačítko **Uložit**. (Stránce automaticky přidá předponu "contosou", takže bude efektivní název "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Na stránce, klikněte na tlačítko **vytvořit uživatele**. Na Cytanium na serverech, nikoli pomocí integrované zabezpečení systému Windows, takže identita fondu aplikací, otevřete databázi vytvoříte uživatele, který má oprávnění pro vaši databázi otevřít. Přihlašovací údaje uživatele přidáte do připojovací řetězce, které přejděte v produkční *Web.config* souboru. V tomto kroku vytvoříte tyto přihlašovací údaje.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Vyplňte požadovaná pole v **vlastnosti uživatele SQL** stránky:

- Jako název zadejte "ContosoUniversityUser".
- Zadejte heslo.
- Vyberte **contosouSchool** jako výchozí databáze.
- Vyberte **contosouSchool** zaškrtávací políčko.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurace nasazení databáze pro produkční prostředí

Teď jste připravení nastavit nastavení nasazení databáze **balení/publikování kódu SQL** kartě, jako jste to udělali dříve pro testovací prostředí.

Otevřete **vlastnosti projektu** vyberte **balení/publikování kódu SQL** kartě a ujistěte se, že **aktivní (verze)** nebo **verze** je vybrané v **konfigurace** rozevíracího seznamu.

Při konfiguraci nastavení nasazení pro každou databázi, klíčovým rozdílem mezi co můžete udělat pro produkční a testovací prostředí je v tom, jak nakonfigurovat připojovací řetězce. Pro testovací prostředí jste zadali jiné cílové databázové připojovací řetězce, ale pro produkční prostředí cílový připojovací řetězec bude mít stejný pro obě databáze. Je to proto, že nasazujete obou databází pro jednu databázi v produkčním prostředí.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurace nastavení nasazení pro databázi členství

Chcete-li nakonfigurovat nastavení vztahující se k databázi členství, vyberte **objekt DefaultConnection nasazení** řádek v **položky databáze** tabulky.

V **připojovací řetězec pro cílovou databázi**, zadejte připojovací řetězec, který odkazuje na novou provozní databáze systému SQL Server, kterou jste právě vytvořili. Uvítacího e-mailu můžete získat připojovací řetězec. Příslušné části e-mailu obsahuje následující ukázka připojovací řetězec:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Po nahradíte tří proměnných, vypadá připojovací řetězec, který je nutné v tomto příkladu:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Zkopírujte a vložte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartě.

Ujistěte se, že **stáhnout data nebo schéma z existující databáze** je stále vybraná a že **databáze možnosti skriptování** stále **schéma a Data**.

V **databázové skripty** pole, zrušte zaškrtnutí políčka vedle Grant.sql skriptu.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurace nastavení nasazení pro databázi školy

Potom vyberte **SchoolContext nasazení** řádek v **položky databáze** tabulky, chcete-li nakonfigurovat nastavení databáze školní.

Zkopírujte stejný připojovací řetězec do **připojovací řetězec pro cílovou databázi** který jste zkopírovali do tohoto pole pro databázi členství.

Ujistěte se, že **stáhnout data nebo schéma z existující databáze** je stále vybraná a že **databáze možnosti skriptování** stále **schéma a Data**.

V **databázové skripty** pole, zrušte zaškrtnutí políčka vedle Grant.sql skriptu.

Uložit změny **balení/publikování kódu SQL** kartě.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Nastavení Web.Config transformuje pro připojovací řetězce pro provozní databáze

Dále budete nastavíte *Web.config* transformace tak, aby připojení řetězců v nasazené *Web.config* souboru tak, aby odkazoval na nový produkční databázi. Připojovací řetězec, který jste zadali na **balení/publikování kódu SQL** karta pro nasazení webu použít je stejná jako ta, aplikace musí používat, s výjimkou přidání `MultipleResultSets` možnost.

Otevřete *Web.Production.config* a nahraďte `connectionStrings` element s `connectionStrings` element, který vypadá jako v následujícím příkladu. (Kopírovat pouze `connectionStrings` elementu, nikoli okolního značky, které jsou k dispozici k zobrazení kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Někdy vidět poradenství, které informuje vždy šifrování připojovací řetězce v *Web.config* souboru. To může být vhodné, pokud byly nasazení serverů na vaše vlastní podnikové síti. Při nasazování do sdílené hostitelského prostředí, ale důvěřujete postupy zabezpečení poskytovatele hostingu, a není nutné nebo praktické k šifrování připojovací řetězce.

## <a name="deploying-to-the-production-environment"></a>Nasazení do produkčního prostředí

Teď jste připravení nasadit do produkčního prostředí. Nasazení webu, bude číst databáze systému SQL Server Compact ve vašem projektu *aplikace\_Data* složky a znovu vytvořit všechny svoje tabulky a data v produkční databázi SQL serveru. Chcete-li publikovat pomocí **balení/publikování webu** nastavení na kartě, je nutné vytvořit nový profil publikování pro produkční prostředí.

Nejprve odstraňte stávající profil produkční, jako jste to udělali profilem testovací dříve.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profil** kartě.

Klikněte na tlačítko **spravovat profily**.

Vyberte **produkční**, klikněte na tlačítko **odebrat**a potom klikněte na **Zavřít**.

Zavřít **Publikovat Web** průvodce uložte tuto změnu.

Dále vytvořte nový profil produkční a ji použít k publikování tohoto projektu.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profil** kartě.

Klikněte na tlačítko **Import**a vyberte soubor .publishsettings, který jste předtím stáhli.

Na **připojení** změňte **cílová adresa URL** správné dočasné adresy URL, který v tomto příkladu je http://contosouniversity.com.vserver01.cytanium.com.

Přejmenujte jej na produkční. (Vyberte **profil** a klikněte na **spravovat profily** k tomu).

Zavřít **Publikovat Web** průvodce a uložit provedené změny.

V reálné aplikaci ve kterém při aktualizaci databáze v produkčním prostředí by dva další kroky proveďte nyní před publikováním:

1. Nahrát *aplikace\_offline.htm*, jak je znázorněno [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu.
2. Použití **správce souborového** funkce ovládacího panelu Cytanium zkopírovat *aspnet Prod.sdf* a *školní-Prod.sdf* soubory z pracoviště *Aplikace\_Data* složky ContosoUniversity projektu. Tím se zajistí, že data, která nasazujete k nové databázi systému SQL Server obsahuje nejnovější aktualizace provedené provozního webu.

V **publikování webu jedním kliknutím** nástrojů, ujistěte se, že **produkční** profilu je vybrána a potom klikněte na **publikovat**.

Pokud jste nahráli *aplikace\_offline.htm* před publikováním, budete muset použít **Správce souborů** nástroj v Ovládacích panelech Cytanium odstranit *aplikace\_offline.* htm před zahájením testovacího. Můžete také ve stejnou dobu odstranit *SDF* souborů z *aplikace\_Data* složky.

Teď můžete otevřít prohlížeč a přejděte na adresu URL na veřejném webu k testování aplikace stejným způsobem jako jste to udělali po nasazení do testovacího prostředí.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Přepnutí na SQL serveru Express LocalDB vývojem

Jak je vysvětleno v přehledu, je obecně nejvhodnější použít stejné databázového stroje v vývoj, které budete používat v testovací a produkční. (Nezapomeňte, že použití systému SQL Server Express v vývoj výhodou je, že databáze bude fungovat stejně ve vašich prostředích vývoj, testování a provozním.) V této části budete nastavení projektu ContosoUniversity používat SQL Server Express LocalDB při spuštění aplikace ze sady Visual Studio.

Nejjednodušší způsob, jak tuto migraci provést je umožnit Code First a systém členství vytvářet i nové databáze vývoj za vás. Pomocí této metody pro migraci vyžaduje tři kroky:

1. Změňte připojovací řetězce k určení nové databáze SQL Express LocalDB.
2. Spusťte nástroj Správa webu k vytvoření uživatel s oprávněním správce. Tím se vytvoří databáze členství.
3. Vytvořit a naplnit databázi aplikace pomocí příkazu update-database migrace Code First.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualizace připojovací řetězce v souboru Web.config

Otevřete *Web.config* souboru a nahraďte `connectionStrings` element s následujícím kódem:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Vytvoření databáze členství

V **Průzkumníku řešení**, vyberte ContosoUniversity projekt a pak klikněte na tlačítko **konfigurace ASP.NET** v **projektu** nabídky.

Vyberte kartu zabezpečení.

Klikněte na tlačítko **vytvořit nebo spravovat role**a poté vytvořit **správce** role.

Vraťte se na kartu zabezpečení.

Klikněte na tlačítko **vytvořit uživateli**a pak vyberte **správce** zaškrtněte políčko a vytvořit uživatele s názvem správce.

Zavřít **nástroj Správa webu**.

### <a name="creating-the-school-database"></a>Vytvoření databáze školy

Otevřete okno konzoly Správce balíčků.

V **výchozí projekt** rozevíracího seznamu vyberte ContosoUniversity.DAL projektu.

Zadejte následující příkaz:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrace Code First platí počáteční migrace, která vytvoří databázi a pak použije AddBirthDate migrace a potom je spuštěna metoda počáteční hodnoty.

Stisknutím klávesy F5 ovládací prvek spuštění tohoto webu. Jako jste to udělali pro testovací a produkční prostředí, spusťte **přidat studenty** přidat nový student a následně zobrazit nové student v **studenty** stránky. Tím ověříte, že databázi školy byl vytvořen a inicializován a ke čtení a zápisu přístup k němu.

Vyberte **aktualizace kredity** stránce a přihlaste se k ověření, že byla nasazena databáze členství a zda máte přístup k němu. Pokud migrujete není uživatelské účty, vytvoření účtu správce a pak vyberte **aktualizace kredity** stránku a ověřte, že funguje.

## <a name="cleaning-up-sql-server-compact-files"></a>Čištění soubory serveru SQL Server Compact

Již nepotřebujete, soubory a balíčky NuGet, které byly zahrnuté pro podporu systému SQL Server Compact. Chcete-li (Tento krok není povinný), můžete smazat nepotřebné soubory a odkazy.

V **Průzkumníku řešení**, odstraňte *SDF* souborů z *aplikace\_Data* složky a *amd64* a *x86* složky z *bin* složky.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši řešení (ne z projektů) a pak klikněte na tlačítko **spravovat balíčky NuGet pro řešení**.

V levém podokně **spravovat balíčky NuGet** dialogové okno, vyberte **nainstalované balíky**.

Vyberte **EntityFramework.SqlServerCompact** balíček a klikněte na tlačítko **spravovat**.

V **vyberte projekty** dialogové okno, jsou vybrané obou projektů. Pro odinstalaci balíčku v obou projektů, zrušte zaškrtnutí obou políček a pak klikněte na **OK**.

V dialogovém okně s dotazem, pokud chcete také odinstalovat závislé balíčky, klikněte na tlačítko Ne. Jedním z nich je balíček rozhraní Entity Framework, který budete muset zachovat.

Postupujte stejným způsobem, chcete-li odinstalovat **SqlServerCompact** balíčku. (Balíčky musí být v tomto pořadí odinstalovat, protože **EntityFramework.SqlServerCompact** závislý balíček **SqlServerCompact** balíčku.)

Jste teď úspěšně migrovali do systému SQL Server Express a plnou instalaci systému SQL Server. V dalším kurzu budete provedete jiné změny databáze a zobrazí nasazení změny v databázi, pokud vaše testovací a produkční databáze systému SQL Server Express a plnou instalaci systému SQL Server použít.

>[!div class="step-by-step"]
[Předchozí](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[další](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
