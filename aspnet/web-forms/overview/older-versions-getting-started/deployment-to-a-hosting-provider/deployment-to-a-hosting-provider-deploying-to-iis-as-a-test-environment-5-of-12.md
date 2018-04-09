---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení pro službu IIS v testovacím prostředí - 5 12 | Microsoft Docs'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 16050455c161c8ced1f954bfce9c2d9a44c522b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení pro službu IIS v testovacím prostředí - 5 12
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nasadit webovou aplikaci ASP.NET do služby IIS v místním počítači.

Pokud vyvíjíte aplikaci, byste obecně otestovat spuštěním v sadě Visual Studio. Ve výchozím nastavení to znamená, že používáte Visual Studio Development Server (nazývaný též Cassini). Vývojový Server sady Visual Studio lze snadno testovat během vývoje v sadě Visual Studio, ale nebude fungovat úplně stejně jako služby IIS. V důsledku toho je možné, že aplikace bude správně spustit při testování v sadě Visual Studio, ale úspěšná, když je nasazena do služby IIS v hostitelském prostředí.

Aplikaci můžete otestovat spolehlivěji těmito způsoby:

1. Při testování v sadě Visual Studio během vývoje pomocí služby IIS Express nebo úplnou službu IIS místo vývojový Server sady Visual Studio. Tato metoda obecně emuluje přesněji způsob spuštění vaší lokality v rámci služby IIS. Tato metoda však není otestovat váš proces nasazení nebo ověřit, že výsledek procesu nasazení, bude fungovat správně.
2. Nasaďte aplikaci do služby IIS na vývojovém počítači pomocí stejného postupu, který budete používat později ji nasadit do provozního prostředí. Tato metoda ověří váš proces nasazení kromě ověřování, které aplikace poběží správně v rámci služby IIS.
3. Nasaďte aplikaci do testovacího prostředí, který je co nejblíže do provozního prostředí. Protože produkčního prostředí pro tyto kurzy poskytovatele hostitelských služeb třetích stran, ideální testovacího prostředí bude druhého účtu s poskytovateli hostitelských služeb. Tento druhý účet by používat pouze pro testování, ale je by ji nastavit stejným způsobem jako účet produkční.

Tento kurz popisuje kroky pro možnost 2. Poskytuje pokyny pro možnost 3 na konci [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu, a na konci tohoto kurzu jsou odkazy na zdroje informací pro možnost 1.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurace aplikace pro spuštění ve středním vztahem důvěryhodnosti

Před instalací služby IIS a nasazení do ní, změníte nastavení souboru Web.config aby webu spuštění více jako proběhne v typické sdíleném hostitelském prostředí.

Poskytovatelé hostingu obvykle běží váš web z umístění *středním vztahem důvěryhodnosti*, což znamená, že některé kroky není možné provést. Například kód aplikace nelze získat přístup k registru systému Windows a nelze číst nebo zapisovat soubory, které jsou mimo hierarchii složek vaší aplikace. Ve výchozím nastavení se vaše aplikace běží *vysokým vztahem důvěryhodnosti* v místním počítači, což znamená, že aplikace je pravděpodobně moci provádět akce, které způsobí selhání při jeho nasazení do produkčního prostředí. Proto aby testovací prostředí, ve kterém více přesně odrážel provozním prostředí, nakonfigurujete aplikace na spouštění v úrovni medium trust.

V souboru Web.config aplikace, přidejte **důvěryhodnosti** element v **system.web** elementu, jak je znázorněno v tomto příkladu.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Aplikace se teď spustí střední úroveň důvěryhodnosti ve službě IIS i v místním počítači. Toto nastavení umožňuje catch co nejdříve pokusy kód aplikace něco udělat, která by v provozu selhal.

> [!NOTE]
> Pokud používáte migrace Code First Entity Framework, ujistěte se, že máte verze 5.0 nebo novější. Rozhraní Entity Framework verze 4.3 Migrace vyžaduje úplný vztah důvěryhodnosti za účelem aktualizace schématu databáze.


## <a name="installing-iis-and-web-deploy"></a>Instalace služby IIS a Web nasazení

K nasazení do služby IIS na vývojovém počítači, musí mít služby IIS a nainstalovat nasazení webu. Tyto nejsou zahrnuté ve výchozím nastavení systém Windows 7. Pokud jste již nainstalovali službu IIS a Web Deploy, přejděte k další části.

Pomocí [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) je upřednostňovaný způsob, jak nainstalovat IIS a Web Deploy, protože instalace webové platformy nainstaluje doporučenou konfiguraci pro službu IIS a automaticky nainstaluje předpoklady pro službu IIS a Web Nasazení v případě potřeby.

Ke spuštění instalačního programu webové platformy nainstalovat službu IIS a Web Deploy, pomocí následujícího odkazu. Pokud jste již nainstalovali službu IIS, nástroje nasazení webu ani v žádné z jejich požadované součásti, instalace webové platformy nainstaluje pouze toho, co chybí.

- [Instalace IIS a Web Deploy pomocí WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Nastavení výchozí fond aplikací na rozhraní .NET 4

Po instalaci služby IIS, spusťte **Správce služby IIS** a ujistěte se, že rozhraní .NET Framework verze 4 je přiřazený výchozí fond aplikací.

Z Windows **spustit** nabídce vyberte možnost **spustit**, zadejte "inetmgr" a pak klikněte na tlačítko **OK**. (Pokud **spustit** příkaz se nenachází ve vaší **spustit** nabídky, můžete stisknout klávesu Windows a R, ho otevřete. Nebo klikněte pravým tlačítkem na hlavním panelu, klikněte na **vlastnosti**, vyberte **nabídce Start** , klikněte na **přizpůsobit**a vyberte **spusťte příkaz**.)

V **připojení** podokně rozbalte uzel serveru a vyberte **fondy aplikací**. V **fondy aplikací** podokně, pokud **DefaultAppPool** je přiřazen k rozhraní .NET framework verze 4 jako na následujícím obrázku, přejděte k další části.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Pokud se zobrazí pouze dva fondů aplikací a jejich současné jsou nastaveny na rozhraní .NET Framework 2.0, je nutné nainstalovat technologii ASP.NET 4 ve službě IIS:

- Otevřete okno příkazového řádku kliknutím pravým tlačítkem na **příkazového řádku** v systému Windows **spustit** nabídky a výběrem **spustit jako správce**. Spusťte [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) nainstalovat technologii ASP.NET 4 ve službě IIS, použijte následující příkazy. (V 64bitových systémech, nahraďte "Framework" s "Framework64".)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Tento příkaz vytvoří novou fondy aplikací pro rozhraní .NET Framework 4, ale výchozí fond aplikací bude stále nastaveno 2.0. Můžete budete nasazovat aplikace s cílem .NET 4 do tohoto fondu aplikací, takže budete muset změnit fond aplikací na rozhraní .NET 4.

Pokud jste zavřeli **Správce služby IIS**, spusťte ji znovu, rozbalte uzel serveru a klikněte na tlačítko **fondy aplikací** zobrazíte **fondy aplikací** podokně znovu.

V **fondy aplikací** podokně klikněte na tlačítko **DefaultAppPool**a potom v **akce** podokně klikněte na tlačítko **základní nastavení**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

V **upravit fond aplikací** dialogové okno, změna **verze rozhraní .NET Framework** k **rozhraní .NET Framework v4.0.30319** a klikněte na tlačítko **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Nyní jste připraveni k publikování do služby IIS.

## <a name="publishing-to-iis"></a>Publikování do služby IIS

Existuje několik způsobů, které můžete nasadit pomocí sady Visual Studio 2010 a nasazení webu:

- Pomocí sady Visual Studio publikování jedním kliknutím.
- Vytvoření *balíček pro nasazení* a nainstalujte ji pomocí uživatelského rozhraní Správce služby IIS. Balíček pro nasazení se skládá z *.zip* soubor, který obsahuje všechny soubory a metadata potřebná k instalaci lokality ve službě IIS.
- Vytvořit balíček pro nasazení a nainstalujte ji pomocí příkazového řádku.

Proces, který jste se v předchozí kurzů a sady Visual Studio k automatizaci úloh nasazení se vztahují na všechny tyto tři metody. V těchto kurzech použijete první z těchto metod. Informace o používání balíčky pro nasazení najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Před publikováním, ujistěte se, že používáte Visual Studio v režimu správce. (V systému Windows 7 **spustit** nabídky, klikněte pravým tlačítkem myši na ikonu pro verzi Visual Studia, kterou používáte a vyberte **spustit jako správce**.) Režim správce je vyžadována pro publikování, pouze když publikujete aplikaci do služby IIS v místním počítači.

V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity (nikoli projekt ContosoUniversity.DAL) a vyberte **publikovat**.

**Publikovat Web** zobrazí se průvodce.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

V rozevíracím seznamu vyberte  **&lt;nový... &gt;**.

V **nový profil** dialogové okno, zadejte "Test" a pak klikněte na tlačítko **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Tento název je že stejný jako střední uzlu Web.Test.config transformace soubor, který jste vytvořili dříve. Tato korespondence je co způsobí, že transformace Web.Test.config má být použita při publikování pomocí tohoto profilu.

Průvodce automaticky přejde **připojení** kartě.

V **adresa URL služby** zadejte *localhost*.

V **web nebo aplikaci** zadejte *Default Web Site/ContosoUniversity*.

V **cílová adresa URL** zadejte `http://localhost/ContosoUniversity`.

**Cílová adresa URL** nastavení není povinné. Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč na tuto adresu URL. Pokud nechcete, aby prohlížeče otevřete automaticky po nasazení, nechte toto pole prázdné.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Klikněte na tlačítko **ověřit připojení** k ověřte, zda jsou nastavení správná, a můžete připojit ke službě IIS v místním počítači.

Zelená značka zaškrtnutí ověřuje, že je připojení úspěšné.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Klikněte na tlačítko **Další** pro přechod **nastavení** kartě.

**Konfigurace** rozevíracího seznamu určuje konfiguraci sestavení a nasazení. Výchozí hodnota je verze, který je co chcete použít.

Ponechte **odebrat další soubory v cílovém umístění** políčko nezaškrtnuté. Vzhledem k tomu, že je to první nasazení, nebude existovat všechny soubory v cílové složce ještě.

V **databáze** zadejte následující hodnotu v poli připojovací řetězec pro **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Proces nasazení bude put tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** je vybrána.

Také v části **SchoolContext**, vyberte **použít migrace Code First**. Tato možnost způsobí, že proces nasazení nakonfigurovat v nasazeném souboru Web.config zadejte `MigrateDatabaseToLatestVersion` inicializátor. Tato inicializátoru automaticky aktualizuje databázi na nejnovější verzi, když aplikace po nasazení poprvé získá přístup k databázi.

V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte následující hodnotu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Nechte **aktualizace databáze** vymazán. Databáze členství nasadí tak, že zkopírujete soubor SDF v aplikaci\_dat a nechcete proces nasazení bezprostředně s tuto databázi.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Klikněte na tlačítko **Další** pro přechod **Preview** kartě.

V **Preview** , klikněte na **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Klikněte na tlačítko **publikování**.

Pokud Visual Studio není v režimu správce, může získat chybovou zprávu, která značí chybu oprávnění. V takovém případě zavřete Visual Studio, otevřete v režimu správce a zkuste to znovu publikovat.

Pokud Visual Studio je v režimu správce, **výstup** okno sestavy úspěšné sestavení a publikování.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Prohlížeči se otevře automaticky Contoso univerzity domovské stránce spuštěná ve službě IIS v místním počítači.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testování v testovacím prostředí

Všimněte si, že zobrazuje indikátor prostředí "(testovací)" místo "(vývoj)", který ukazuje, že *Web.config* transformaci pro prostředí indikátoru byla úspěšná.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Spustit **studenty** a ověřte, že nasazené databáze nemá žádné studenty. Po výběru této stránky může trvat několik minut, načíst, protože Code First vytvoří databázi a poté spustí `Seed` metoda. (Ho nebylo provádět, když jste byli na domovské stránce, protože při pokusu o přístup k databázi ještě nebylo v aplikaci.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Spustit **vyučující** a ověřte, že Code First nasadí databáze s lektorem dat:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Vyberte **přidat studenty** z **studenty** nabídce Přidat student a zobrazte nový student v **studenty** a ověřte, že můžete úspěšně zapisovat do databáze :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Z **kurzy** nabídce vyberte možnost **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **protokolu v** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, který jste vytvořili starší ("admin" a "Pa$ w0rd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozí kurzu byla správně nasazena do testovacího prostředí.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Ověřte, že *Elmah* složka existuje souborem pouze zástupný symbol v ní.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Kontrola změn automatické Web.config pro migrace Code First

Otevřete *Web.config* souboru v nasazení aplikace na *C:\inetpub\wwwroot\ContosoUniversity* a uvidíte, kde proces nasazení nakonfigurované migrace Code First na automaticky aktualizace databáze na nejnovější verzi.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Proces nasazení vytvoří taky nový připojovací řetězec pro migrace Code First používat výhradně pro aktualizace schématu databáze:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Tento další připojovací řetězec můžete zadat jeden uživatelský účet pro aktualizace schématu databáze a jiného uživatelského účtu pro přístup k datům aplikací. Je třeba přiřadit databáze\_vlastníka role migrace Code First a db\_DataReader – a db\_datawriter rolí k aplikaci. Toto je běžný vzor obrany zabezpečení, který brání potenciálně škodlivého kódu v aplikaci ve změně schématu databáze. (Například k tomu může dojít v úspěšné útok prostřednictvím injektáže SQL.) Tyto kurzy nepoužívá tohoto vzoru. Nevztahuje na systém SQL Server Compact a nevztahuje se při migraci do systému SQL Server v novější kurzu této série. Web Cytanium nabízí právě jeden uživatelský účet pro přístup k databázi systému SQL Server, který vytvoříte v Cytanium. Pokud jste tento vzor implementovat ve vašem scénáři, můžete to provést pomocí následujících kroků:

1. V **nastavení** kartě **Publikovat Web** průvodce, zadejte připojovací řetězec, který určuje uživatel s oprávněními aktualizovat schéma úplné databáze a vymazat **použít tento připojovací řetězec v době běhu** zaškrtávací políčko. V nasazeném souboru Web.config, to všechno bude `DatabasePublish` připojovací řetězec.
2. Vytvoření souboru transformace Web.config pro připojovací řetězec, který má aplikace použít za běhu.

Teď můžete nasadit aplikace do služby IIS na vývojovém počítači a otestovat ho existuje. Tím ověříte, že procesu nasazení zkopírovat obsah aplikace do správného umístění (s výjimkou souborů, které nemají mít nasazení) a také nasazení webu služby IIS správně nakonfigurovaný během nasazení. V dalším kurzu, je potřeba spustit jeden další test, který vyhledá nasazení úlohu, která dosud nebylo provedeno: nastavení oprávnění ke složkám na *Elmah* složky.

## <a name="more-information"></a>Další informace

Informace o spuštění služby IIS nebo IIS Express v sadě Visual Studio najdete v následujících zdrojích informací:

- [Přehled služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) na webu IIS.net.
- [Představení služby IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Postupy: určení webového serveru pro webové projekty v sadě Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Hlavní rozdíly mezi IIS a ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) na webu technologie ASP.NET.
- [Testování ASP.NET MVC nebo aplikaci Web Forms ve službě IIS 7 30 sekund](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) na blogu Ricka Andersona. Tato položka obsahuje příklady Proč není stejně spolehlivá jako testování v IIS Express testování pomocí vývojového serveru Visual Studio (Cassini) a proč není stejně spolehlivá jako testování ve službě IIS testování v IIS Express.

Informace o problémech, které mohou se vyskytnout při spuštění aplikace v úrovni medium trust, najdete v části [hostování aplikací ASP.NET ve střední důvěryhodnosti](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 nepřetržitého z Rolla lokality.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [další](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
