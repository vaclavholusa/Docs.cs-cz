---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: migrace na SQL Server – 10 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: e47f9cb7df6cc119c69f0332df904183cef86405
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753764"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: migrace na SQL Server – 10 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu se dozvíte, jak provést migraci ze systému SQL Server Compact do systému SQL Server. Jedním z důvodů, že kdy je vhodné, který je využít výhod funkcí systému SQL Server, které SQL Server Compact není podporován, jako je například uložených procedur, aktivačních událostí, zobrazení nebo replikace. Další informace o rozdílech mezi SQL Server Compact a SQL Server, najdete v článku [nasazení systému SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) kurzu.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express a plnou instalaci systému SQL Server pro vývoj

Jakmile jste se rozhodli upgrade na SQL Server, můžete chtít použít SQL Server nebo SQL Server Express ve vašich vývojových a testovacích prostředích. Kromě rozdíly v podpora nástroje a funkce databázového stroje existují rozdíly v implementacích zprostředkovatele SQL Server Compact a jiné verze systému SQL Server. Tyto rozdíly může způsobit, že stejný kód ke generování odlišné výsledky. Proto pokud se rozhodnete zachovat SQL Server Compact jako vývoj databáze, měli byste důkladně otestovat svůj web v systému SQL Server nebo SQL Server Express v testovacím prostředí před každé nasazení do produkčního prostředí.

Na rozdíl od systému SQL Server Compact SQL Server Express je v podstatě stejný databázový stroj a používá stejný zprostředkovatel .NET jako plnou instalaci systému SQL Server. Při testování pomocí serveru SQL Server Express, máte jistotu, že z vrátí stejné výsledky, jak se s SQL serverem. Většina nástrojů stejné databáze můžete použít s SQL Server Express, který vám pomůže s SQL serverem (významné výjimky se [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), a podporuje další funkce systému SQL Server jako uložené procedury, triggery, a zobrazení a replikace. (Budete obvykle muset použít plnou instalaci systému SQL Server v provozním webu, ale. SQL Server Express můžete spustit ve sdíleném hostování prostředí, ale nebyl navržen, pro který, a mnoho poskytovatelů hostingu je nepodporují.)

Pokud používáte sadu Visual Studio 2012, obvykle zvolíte SQL Server Express LocalDB pro vaše vývojové prostředí vzhledem k tomu, který je, co se instaluje standardně pomocí sady Visual Studio. Ale LocalDB nefunguje ve službě IIS, tak pro testovací prostředí, budete muset použít SQL Server nebo SQL Server Express.

### <a name="combining-databases-versus-keeping-them-separate"></a>Kombinování databáze a zachování jejich samostatným dokumentem

Aplikace Contoso University má dvě databáze systému SQL Server Compact: databáze členství (*aspnet.sdf*) a aplikační databáze (*School.sdf*). Při migraci, můžete migrovat tyto databáze k dvě samostatné databáze a izolované databáze. Můžete kombinovat předpřipravených databáze spojení mezi vaší aplikační databáze a databáze členství. Váš plán hostování může také zadejte důvod k jejich zkombinování. Například poskytovatele hostitelských služeb můžou účtovat další možnosti pro více databází nebo nemusí povolit i více než jednu databázi. To je v případě, Cytanium Lite hostitelský účet, který se používá pro účely tohoto kurzu, který umožňuje pouze jednu databázi SQL serveru.

V tomto kurzu provedeme migraci vašeho dvě databáze tímto způsobem:

- Migrace na dvě databáze LocalDB ve vývojovém prostředí.
- Migrace na dvě databáze systému SQL Server Express v testovacím prostředí.
- Migrace do jedné kombinované úplné databáze serveru SQL Server v provozním prostředí.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Instalace SQL serveru Express

SQL Server Express se automaticky nainstaluje ve výchozím nastavení se sadou Visual Studio 2010, ale ve výchozím nastavení není nainstalována pomocí sady Visual Studio 2012. Pokud chcete nainstalovat SQL Server 2012 Express, klikněte na následující odkaz

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Zvolte *ENU/x64/SQLEXPR\_x64\_ENU.exe* nebo *ENU/x86/SQLEXPR\_x86\_ENU.exe*a v Průvodci instalací přijmete výchozí hodnoty nastavení. Další informace o možnostech instalace najdete v tématu [instalace systému SQL Server 2012 pomocí Průvodce instalací (nastavení)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Vytvoření databáze systému SQL Server Express pro testovací prostředí

Dalším krokem je vytvoření členství technologie ASP.NET a databáze školy.

Z **zobrazení** nabídky vyberte možnost **Průzkumníka serveru** (**Průzkumník databáze** v aplikaci Visual Web Developer) a potom klikněte pravým tlačítkem na **datová připojení**a vyberte **vytvořit novou databázi SQL serveru**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

V **vytvořit novou databázi SQL serveru** dialogového okna zadejte ". \SQLExpress" v **název serveru** pole a "aspnet testování" v **nového názvu databázového** pole a potom klikněte na **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Postupujte stejným způsobem vytvoření nové databáze SQL serveru Express školy s názvem "Školní Test".

(Přidáváte "Test" tyto názvy databází vzhledem k tomu budete později vytvořit další instance jednotlivých databází pro vývojové prostředí a musíte být schopni odlišení dvě sady databází.)

**Průzkumník serveru** teď zobrazují dvě nové databáze.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Vytvoření nové databáze skriptu udělení

Při spuštění aplikace ve službě IIS na vašem vývojovém počítači aplikace přistupuje k databázi s použitím přihlašovacích údajů výchozí fond aplikací. Ale ve výchozím nastavení, identita fondu aplikací nemá oprávnění k otevření databáze. Proto budete muset spustit skript k udělení oprávnění. V této části vytvoříte skript, že je potřeba spustit později, abyste měli jistotu, že aplikace bude moci otevřít databáze při spuštění ve službě IIS.

V rámci řešení *SolutionFiles* složky, které jste vytvořili [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu, vytvořte nový soubor SQL *Grant.sql*. Zkopírujte následující příkazy SQL do souboru a pak uložte a zavřete soubor:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Tento skript je navržen pro práci s SQL Server 2008 a nastavení služby IIS ve Windows 7, jako jsou uvedeny v tomto kurzu. Pokud používáte jinou verzi systému SQL Server nebo Windows, nebo pokud jste nastavili IIS počítače odlišně, může být vyžadováno změny tohoto skriptu. Další informace o skripty systému SQL Server najdete v tématu [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Poznámka k zabezpečení** tento skript vám db\_oprávnění vlastníka na uživatele, který přistupuje k databázi v době běhu, což je budete mít v provozním prostředí. V některých případech můžete chtít určit jako uživatel, který má celé databáze schéma aktualizovat oprávnění pouze pro nasazení a zadejte jiný uživatel, který má oprávnění pouze ke čtení a zápisu dat běhu. Další informace najdete v tématu **kontroly automatické změn souboru Web.config pro migrace Code First** v [nasazení do služby IIS jako testovacího prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurace nasazení databáze pro testovací prostředí

Visual Studio v dalším kroku nakonfigurujete tak, že provedete následující úlohy pro každou databázi:

- Generovat skript SQL, který vytvoří strukturu zdrojové databáze (tabulky, sloupce, omezení, atd.) v cílové databázi.
- Generovat skript SQL, který vkládá data zdrojové databáze do tabulek v cílové databázi.
- Spusťte vygenerované skripty a udělení skript, který jste vytvořili v cílové databázi.

Otevřít **vlastnosti projektu** okna a vyberte **balení/publikování kódu SQL** kartu.

Ujistěte se, že **aktivní (verze)** nebo **vydání** výběru v **konfigurace** rozevíracího seznamu.

Klikněte na tlačítko **povolit tuto stránku**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**Balení/publikování kódu SQL** karta je zakázaná obvykle, protože určuje způsob starší verze nasazení. Pro většinu scénářů, měli byste nakonfigurovat nasazení databáze v **publikování webu** průvodce. Migrace ze systému SQL Server Compact do systému SQL Server nebo SQL Server Express je zvláštní případ, pro kterou tato metoda je dobrou volbou.

Klikněte na tlačítko **importovat ze souboru Web.config**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio hledá připojovací řetězce v *Web.config* souboru, vyhledá jeden pro databáze členství a jeden pro databáze školy a přidá řádek odpovídající každé připojovací řetězec **položky databáze**  tabulky. Připojovací řetězce, které nalezne jsou pro existující databáze systému SQL Server Compact a další krok budete moct nakonfigurovat jak a kde k nasazení těchto databází.

Zadejte nastavení nasazení databáze **podrobnosti o databázové položce** níže v části **položky databáze** tabulky. Nastavení uvedená v **podrobnosti o databázové položce** podle toho, co řádek v části se týkají **položky databáze** vybrané tabulky, jak je znázorněno na následujícím obrázku.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurace nastavení nasazení pro databázi členství

Vyberte **objekt DefaultConnection nasazení** řádku v **položky databáze** tabulky, aby bylo možné konfigurovat nastavení, které platí pro databáze členství.

V **připojovací řetězec pro cílovou databázi**, zadejte připojovací řetězec, který odkazuje na novou databázi serveru SQL Server Express členství. Můžete získat připojovací řetězec budete potřebovat z **Průzkumníka serveru**. V **Průzkumníka serveru**, rozbalte **datová připojení** a vyberte **aspnetTest** databáze, pak z **vlastnosti** okno Kopírovat **Připojovací řetězec** hodnotu.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Stejný připojovací řetězec je reprodukován zde:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Zkopírujte a vložte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartu.

Ujistěte se, že **o přijetí změn dat a/nebo schéma z existující databáze** zaškrtnuto. Je to, co způsobí, že skripty SQL pro automaticky generované a spustit v cílové databázi.

**Připojovací řetězec pro zdrojovou databázi** extrakci hodnot z *Web.config* souboru a odkazuje na databázi systému SQL Server Compact vývoje. Toto je zdrojové databáze, který se použije ke generování skriptů, které se spustí později v cílové databázi. Protože ale chcete nasazovat produkční verzi databáze, změňte "aspnet Dev.sdf" na "aspnet Prod.sdf".

Změna **databáze možnosti skriptování** z **pouze schéma** k **schéma a data**, protože ale chcete si zkopírujte svá data (uživatelské účty a role) a také struktura databáze.

Konfigurace nasazení tak, aby spouštění skriptů udělení, které jste vytvořili dříve, budete muset přidat je do **databázové skripty** oddílu. Klikněte na tlačítko **přidat skript**a **přidat skripty SQL** dialogové okno, přejděte do složky, kam jste uložili skriptu udělení (je to složka, která obsahuje váš soubor řešení). Vyberte soubor s názvem *Grant.sql*a klikněte na tlačítko **otevřít**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Nastavení **objekt DefaultConnection nasazení** řádku v **položky databáze** teď měl vypadat jako na následujícím obrázku:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurace nastavení nasazení pro databáze školy

V dalším kroku vyberte **SchoolContext nasazení** řádku v **položky databáze** tabulky, aby bylo možné konfigurovat nastavení nasazení pro databázi School.

Můžete použít stejnou metodu, kterou jste předtím použili ke získání připojovacího řetězce pro novou databázi serveru SQL Server Express. Zkopírujte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartu.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Ujistěte se, že **o přijetí změn dat a/nebo schéma z existující databáze** zaškrtnuto.

**Připojovací řetězec pro zdrojovou databázi** extrakci hodnot z *Web.config* souboru a odkazuje na databázi systému SQL Server Compact vývoje. Změňte "School Dev.sdf" na "Školní-Prod.sdf" nasazení produkční verzi databáze. (Nikdy nevytvářejí School Prod.sdf souboru v aplikaci\_složku Data, takže budete zkopírovat daný soubor z testovacího prostředí do aplikace\_složku Data ve složce projektu ContosoUniversity později.)

Změna **databáze možnosti skriptování** k **schéma a data**.

Můžete také spustit skript, který chcete udělit pro čtení a zápis oprávnění pro tuto databázi k identitě fondu aplikací, proto přidat *Grant.sql* soubor skriptu, který jste použili pro databáze členství.

Až budete mít, nastavení **SchoolContext nasazení** řádku v **položky databáze** vypadat jako na následujícím obrázku:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Uložit změny **balení/publikování kódu SQL** kartu.

Kopírování *školní-Prod.sdf* souboru z *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* složku *aplikace\_Data* složky v ContosoUniversity projektu.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Určení transakčního režimu pro udělení skriptu

Proces nasazení generuje skripty, které se nasadit schéma databáze a data. Ve výchozím nastavení spusťte tyto skripty v transakci. Vlastní skripty (např. skripty grant) ve výchozím nastavení však nelze spustit v transakci. Pokud se proces nasazení používá režimy transakce, může se zobrazit vypršení časového limitu při spuštění skriptů při nasazení. V této části upravte soubor projektu konfigurujete vlastní skripty pro spuštění v rámci transakce.

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši **ContosoUniversity** projektu a vyberte **uvolnit projekt**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Klikněte pravým tlačítkem na projekt a vyberte **upravit ContosoUniversity.csproj**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Editor sady Visual Studio zobrazí obsah XML souboru projektu. Všimněte si, že existuje několik `PropertyGroup` elementy. (Na obrázku, obsah `PropertyGroup` prvky byly vynechány.)

![Okno editoru souboru projektu](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

První z nich, který nemá žádné `Condition` atribut, je pro konfiguraci nastavení, která platí bez ohledu na to sestavení. Jeden `PropertyGroup` element platí pouze pro konfiguraci sestavení ladění (Poznámka: `Condition` atribut), jeden platí pouze pro konfigurace sestavení pro vydání a jeden platí pouze pro konfiguraci testovacího sestavení. V rámci `PropertyGroup` – element pro konfigurace sestavení pro vydání, zobrazí se vám `PublishDatabaseSettings` element, který obsahuje nastavení, které jste zadali na **balení/publikování kódu SQL** kartu. Je `Object` element, který odpovídá každému udělení skriptů, které jste zadali (Všimněte si, že dva výskyty "Grant.sql"). Ve výchozím nastavení `Transacted` atribut `Source` – element pro každý skript udělení je `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Změňte hodnotu `Transacted` atribut `Source` elementu `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Uložte a zavřete soubor projektu, klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **znovu načíst projekt**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Nastavení transformace Web.Config pro připojovací řetězce

Řetězce připojení pro nový systém SQL Express databáze, které jste zadali na **balení/publikování kódu SQL** kartu pomocí nasazení webu slouží pouze k aktualizaci v cílové databázi během nasazení. Stále nutné nastavit *Web.config* transformace tak, aby připojení řetězce v nasazených *Web.config* souboru, přejděte na nové databáze systému SQL Server Express. (Při použití **balení/publikování kódu SQL** kartu, nelze nakonfigurovat připojovací řetězce v profilu publikování.)

Otevřít *Web.Test.config* a nahraďte `connectionStrings` element s `connectionStrings` element v následujícím příkladu. (Ujistěte se, že zkopírujete elementu connectionStrings není okolním kódem, který je znázorněna zde k zajištění kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Tento kód způsobí, že `connectionString` a `providerName` atributy každého `add` elementu, které mají být nahrazeny v nasazených *Web.config* souboru. Tyto řetězce připojení nejsou stejné jako ty, které jste zadali **balení/publikování kódu SQL** kartu. Nastavení "MultipleActiveResultSets = True" je přidaný do jejich, protože je to požadováno rozhraní Entity Framework a Universal Providers.

## <a name="installing-sql-server-compact"></a>Instalace serveru SQL Server Compact

Balíček SqlServerCompact NuGet obsahuje databázi systému SQL Server Compact modul sestavení pro aplikaci Contoso University. Ale nyní není aplikace, ale nasazení webu, který musí být schopni číst databáze systému SQL Server Compact, chcete-li vytvořit skripty pro spuštění v databázích systému SQL Server. Chcete-li nasazení webu ke čtení databází systému SQL Server Compact, nainstalujte SQL Server Compact na vývojovém počítači pomocí následujícího odkazu: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Nasazení do testovacího prostředí

Chcete-li publikovat do testovacího prostředí, budete muset vytvořit profil publikování, který je nakonfigurován pro použití **balení/publikování kódu SQL** kartu pro databázi místo nastavení databáze publikování profilu publikování.

Nejprve odstraňte existující profil testu.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profilu** kartu.

Klikněte na tlačítko **spravovat profily**.

Vyberte **testovací**, klikněte na tlačítko **odebrat**a potom klikněte na tlačítko **Zavřít**.

Zavřít **publikování webu** průvodce a uložte tuto změnu.

Dále vytvořte nový profil testu a použít k publikování projektu.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profilu** kartu.

Vyberte **&lt;nové... &gt;** z rozevíracího seznamu a zadejte "Test" jako název profilu.

V **adresa URL služby** zadejte *localhost*.

V **web/aplikace** zadejte *výchozí webový server/ContosoUniversity*.

V **cílovou adresu URL** zadejte `http://localhost/ContosoUniversity/`.

Klikněte na tlačítko **Další**.

**Nastavení** kartu upozorní, který **balení/publikování kódu SQL** karta není nakonfigurovaná, a dává příležitost k jejich přepsání kliknutím na Povolit nová vylepšení publikování databáze. Pro toto nasazení nechcete přepsat **balení/publikování kódu SQL** kartě nastavení, takže stačí kliknout na **Další**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Zprávu na **ve verzi Preview** kartu znamená, že **nejsou vybrány žádné databáze k publikování**, ale to pouze znamená, že publikování databáze není nakonfigurované v profilu publikování.

Klikněte na tlačítko **publikovat**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio nasadí aplikaci a otevře prohlížeč na domovské stránce webu v testovacím prostředí. Spustíte školitelů stránku, abyste viděli, zobrazuje stejná data, která jste viděli již dříve. Spustit **přidat studenty** stránce, přidání nového studenta a potom zobrazit nového objektu student do **studenty** stránky. Ověří, že budete aktualizovat databázi. Vyberte **aktualizace kredity** stránka (bude potřeba přihlásit) Chcete-li ověřit, že byla nasazena databáze členství a je k ní máte přístup.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Vytvoření databáze SQL serveru pro produkční prostředí

Teď, když jste nasadili do testovacího prostředí, budete připraveni k nasazení do produkčního prostředí. Začnete, jako jste to udělali pro testovací prostředí, tak, že vytvoříte databázi nasadit. Jak si Vzpomínáte přehled, Cytanium Lite plán hostování pouze umožňuje izolované databáze SQL serveru, tak nastavíte pouze jedna databáze není dvě. Veškerá data z členství a školy SQL Server Compact databází a tabulek se nasadí do jedné databáze SQL serveru v produkčním prostředí.

Přejděte do ovládacích panelů Cytanium na [ http://panel.cytanium.com ](http://panel.cytanium.com). Podržte ukazatel myši nad **databází** a potom klikněte na tlačítko **systému SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

V **systému SQL Server 2008** klikněte na **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Pojmenujte databázi "Školy" a klikněte na tlačítko **Uložit**. (Na stránce automaticky přidá předponu "contosou", takže efektivní název bude "contosouSchool".)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Klikněte na stejné stránce **Create User**. Na servery společnosti Cytanium, spíše než použití integrovaného zabezpečení Windows, takže identita fondu aplikací, otevřete databázi vytvoříte uživatele, který má oprávnění k otevření vaší databáze. Přidejte přihlašovací údaje uživatele na připojovací řetězce, které najdete v provozní *Web.config* souboru. V tomto kroku vytvoříte tyto přihlašovací údaje.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Vyplňte požadovaná pole v **vlastnosti uživatele SQL** stránky:

- Jako název zadejte "ContosoUniversityUser".
- Zadejte heslo.
- Vyberte **contosouSchool** jako výchozí databáze.
- Vyberte **contosouSchool** zaškrtávací políčko.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurace nasazení databáze pro produkční prostředí

Nyní jste připraveni nastavit nastavení nasazení databáze **balení/publikování kódu SQL** kartě, jako jste to udělali dříve pro testovací prostředí.

Otevřít **vlastnosti projektu** okna, vyberte **balení/publikování kódu SQL** kartu a ujistěte se, že **aktivní (verze)** nebo **vydání** je vybrané v **konfigurace** rozevíracího seznamu.

Při konfiguraci nastavení nasazení pro každou databázi, je klíčovým rozdílem mezi udělat pro produkční a testovací prostředí v konfiguraci připojovací řetězce. Pro testovací prostředí jste zadali jiné cílové databázové připojovací řetězce, ale pro produkční prostředí cílový připojovací řetězec bude mít stejná pro databázi i databázi. Je to proto, že obě databáze jsou nasazení do jedné databáze v produkčním prostředí.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurace nastavení nasazení pro databázi členství

Pokud chcete nakonfigurovat nastavení vztahující se k databázi členství, vyberte **objekt DefaultConnection nasazení** řádku v **položky databáze** tabulky.

V **připojovací řetězec pro cílovou databázi**, zadejte připojovací řetězec, který odkazuje na novou produkční databázi SQL serveru, který jste právě vytvořili. Získání připojovacího řetězce z Uvítacího e-mailu. Příslušné části e-mailu obsahuje následující připojovací řetězec vzorku:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Po nahradíte tří proměnných, potřebujete připojovací řetězec vypadá jako v tomto příkladu:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Zkopírujte a vložte tento připojovací řetězec do **připojovací řetězec pro cílovou databázi** v **balení/publikování kódu SQL** kartu.

Ujistěte se, že **o přijetí změn dat a/nebo schéma z existující databáze** stále vybraná a že **databáze možnosti skriptování** je stále **schéma a Data**.

V **databázové skripty** pole, zrušte zaškrtnutí políčka vedle Grant.sql skriptu.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurace nastavení nasazení pro databáze školy

V dalším kroku vyberte **SchoolContext nasazení** řádku v **položky databáze** tabulky, aby bylo možné konfigurovat nastavení databáze školy.

Zkopírujte stejný připojovací řetězec do **připojovací řetězec pro cílovou databázi** , který jste zkopírovali do pole pro databáze členství.

Ujistěte se, že **o přijetí změn dat a/nebo schéma z existující databáze** stále vybraná a že **databáze možnosti skriptování** je stále **schéma a Data**.

V **databázové skripty** pole, zrušte zaškrtnutí políčka vedle Grant.sql skriptu.

Uložit změny **balení/publikování kódu SQL** kartu.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Nastavení Web.Config transformuje pro připojovací řetězce provozních databází

V dalším kroku nastavíte *Web.config* transformace tak, aby připojení řetězce v nasazených *Web.config* souboru tak, aby odkazoval na nové produkční databázi. Připojovací řetězec, který jste zadali na **balení/publikování kódu SQL** karta pro nasazení webu použít je stejná jako ta, aplikace musí používat, s výjimkou přidání `MultipleResultSets` možnost.

Otevřít *Web.Production.config* a nahraďte `connectionStrings` element s `connectionStrings` element, který bude vypadat jako v následujícím příkladu. (Pouze zkopírovat `connectionStrings` elementu, nikoli okolního značky, které jsou k dispozici k zobrazení kontextu.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

V některých případech uvidíte Rady, že musíte vždy šifrují připojovací řetězce v *Web.config* souboru. To může být vhodné, pokud se nasazení pro servery v síti vaší společnosti. Při nasazování na sdíleném hostování prostředí, ale důvěřujete bezpečnostní postupy poskytovatele hostingu a není nutné nebo praktické šifrování připojovací řetězce.

## <a name="deploying-to-the-production-environment"></a>Nasazení do produkčního prostředí

Nyní jste připraveni nasadit do produkčního prostředí. Nástroj nasazení webu bude číst databázím SQL Server Compact do projektu *aplikace\_Data* složky a znovu vytvořit všechny jejich tabulek a dat v produkční databázi SQL serveru. Chcete-li publikovat pomocí **balení/publikování webu** nastavení karet, je nutné vytvořit nový profil publikování pro produkční prostředí.

Nejprve odstraňte existující profil produkčního prostředí, jako se to dělá testovacího profilu.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profilu** kartu.

Klikněte na tlačítko **spravovat profily**.

Vyberte **produkční**, klikněte na tlačítko **odebrat**a potom klikněte na tlačítko **Zavřít**.

Zavřít **publikování webu** průvodce a uložte tuto změnu.

Dále vytvořte nový profil produkčního prostředí a použít k publikování projektu.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**.

Vyberte **profilu** kartu.

Klikněte na tlačítko **Import**a vyberte soubor .publishsettings, který jste předtím stáhli.

Na **připojení** kartu, změnit **cílovou adresu URL** správné dočasné adresy URL, které se v tomto příkladu je http://contosouniversity.com.vserver01.cytanium.com.

Přejmenujte profil do produkčního prostředí. (Vyberte **profilu** kartě a klikněte na tlačítko **spravovat profily** k tomu).

Zavřít **publikování webu** průvodce uložte provedené změny.

V reálné aplikaci, ve kterém se aktualizuje databázi v produkčním prostředí provedli byste další dva kroky nyní před publikováním:

1. Nahrát *aplikace\_offline.htm*, jak je znázorněno [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu.
2. Použití **Správce souborů** funkce ovládacího panelu Cytanium zkopírovat *aspnet Prod.sdf* a *školní-Prod.sdf* soubory z pracoviště *Aplikace\_Data* složky ContosoUniversity projektu. Tím se zajistí, že data, které nasazení provádíte do nové databáze systému SQL Server zahrnuje nejnovější aktualizace od produkčního webu.

V **publikování webu jedním kliknutím** nástrojů, ujistěte se, že **produkční** profilu je vybrána a potom klikněte na **publikovat**.

Pokud jste nahráli <em>aplikace\_offline.htm</em> před publikováním, je nutné použít <strong>Správce souborů</strong> nástroj v Ovládacích panelech Cytanium odstranit <em>aplikace\_offline.</em> htm před testováním. Můžete najednou odstranit také <em>SDF</em> souborů z doručené pošty <em>aplikace\_Data</em> složky.

Můžete teď otevřete prohlížeč a přejděte na adresu URL na veřejném webu k otestování aplikace stejným způsobem, které jste provedli po nasazení do testovacího prostředí.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Přepnutí na SQL Server Express LocalDB ve vývoji

Jak bylo vysvětleno v přehledu, je obecně nejvhodnější použít stejný databázový stroj při vývoji, který používáte v provozním i testovacím prostředí. (Mějte na paměti, že výhoda používání SQL Server Express ve vývoji je, že databáze bude fungovat stejně v vývoj, testování a produkční prostředí.) V této části budete nastavení projektu ContosoUniversity používat SQL Server Express LocalDB při spuštění aplikace ze sady Visual Studio.

Nejjednodušší způsob, jak tuto migraci provést je nechat Code First a systém členství vytvářet i nové vývoje databáze za vás. Pomocí této metody migrace vyžaduje tři kroky:

1. Změňte připojovací řetězce k určení nové databáze SQL Express LocalDB.
2. Spusťte nástroj Správa webu vytvořit uživatel s oprávněním správce. Tím se vytvoří databáze členství.
3. Pomocí migrace Code First aktualizace databáze příkazu k vytvoření a přidání dat do databáze aplikace.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualizují se připojovací řetězce v souboru Web.config

Otevřít *Web.config* soubor a nahradit `connectionStrings` element následujícím kódem:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Vytváří se databáze členství

V **Průzkumníka řešení**, vyberte projekt ContosoUniversity a potom klikněte na tlačítko **konfigurace ASP.NET** v **projektu** nabídky.

Vyberte kartu zabezpečení.

Klikněte na tlačítko **vytvořit nebo spravovat role**a pak vytvořte **správce** role.

Vraťte se na kartu zabezpečení.

Klikněte na tlačítko **vytvořit uživatele**a pak vyberte **správce** zaškrtněte políčko a vytvoří uživatele správce.

Zavřít **nástroje pro správu webu**.

### <a name="creating-the-school-database"></a>Vytvoření databáze školy

Otevřete okno konzoly Správce balíčků.

V **výchozí projekt** rozevíracího seznamu vyberte ContosoUniversity.DAL projektu.

Zadejte následující příkaz:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Migrace Code First platí počáteční migrace, který vytvoří databázi a poté použije migrace AddBirthDate potom spustí metodu počáteční hodnoty.

Spuštění tohoto webu stisknutím klávesy F5 ovládacího prvku. Stejně jako u testovacím a produkčním prostředí, spusťte **přidat studenty** stránce, přidání nového studenta a potom zobrazit nového objektu student do **studenty** stránky. Ověří, že byl vytvořen a inicializován databáze školy a přečtení a oprávnění k zápisu do něj.

Vyberte **aktualizace kredity** stránky a přihlaste se k ověření, že byla nasazena databáze členství a, abyste měli přístup k němu. Pokud migrujete není uživatelské účty, vytvořit účet správce a pak vyberte **aktualizace kredity** stránku a zkontrolujte, jestli funguje.

## <a name="cleaning-up-sql-server-compact-files"></a>Čištění SQL Server Compact souborů

Už nepotřebujete, soubory a balíčky NuGet, které byly obsaženy pro podporu systému SQL Server Compact. Chcete-li (Tento krok není povinný), můžete smazat nepotřebné soubory a odkazy.

V **Průzkumníku řešení**, odstranit *SDF* souborů z doručené pošty *aplikace\_Data* složky a *amd64* a *x86* složky *bin* složky.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení (nikoli do jednoho z projektů) a potom klikněte na tlačítko **spravovat balíčky NuGet pro řešení**.

V levém podokně **spravovat balíčky NuGet** dialogu **nainstalované balíčky**.

Vyberte **EntityFramework.SqlServerCompact** balíček a klikněte na tlačítko **spravovat**.

V **vyberte projekty** dialogové okno, jsou vybrány obou projektů. Pro odinstalaci balíčku v projektu, zrušte zaškrtnutí obou políček a pak klikněte na **OK**.

V dialogovém okně s dotazem, jestli chcete také odinstalovat závislé balíčky, klikněte na tlačítko Ne. Jednou z nich je balíčku Entity Framework, které chcete zachovat.

Postupujte stejným způsobem odinstalovat **SqlServerCompact** balíčku. (Balíčky musí být v tomto pořadí odinstalovat, protože **EntityFramework.SqlServerCompact** balíček závisí **SqlServerCompact** balíčku.)

Teď úspěšně jste migrovali na SQL Server Express a plnou instalaci systému SQL Server. V dalším kurzu provede další změna databáze a ukážeme, jak nasazení změn databází při použití testovacích a provozních databází systému SQL Server Express a plnou instalaci systému SQL Server.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
