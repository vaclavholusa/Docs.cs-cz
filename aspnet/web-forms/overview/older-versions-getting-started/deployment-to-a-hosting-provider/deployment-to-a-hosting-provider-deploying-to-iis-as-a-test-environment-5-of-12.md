---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do služby IIS jako testovacího prostředí – 5 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 46d986aad52b0ab5a235eade2e17b0cf9f8cdb9f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835198"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nasazení do služby IIS jako testovacího prostředí – 5 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nasadit webovou aplikaci ASP.NET do služby IIS v místním počítači.

Když vyvíjíte aplikaci, je obvykle otestovat spuštěním v sadě Visual Studio. Ve výchozím nastavení to znamená, že používáte Visual Studio vývojový Server (nazývaný též Cassini). Vývojový Server sady Visual Studio usnadňuje testování během vývoje v sadě Visual Studio, ale nebude fungovat stejně jako služby IIS. V důsledku toho je možné, že aplikace bude správně spustit při testování v sadě Visual Studio, ale selhat při nasazení do služby IIS v hostitelském prostředí.

Aplikaci můžete otestovat spolehlivěji následujícími způsoby:

1. Pomocí služby IIS Express nebo úplný IIS místo vývojový Server sady Visual Studio při testování v sadě Visual Studio během vývoje. Tato metoda obvykle emuluje přesněji spouštění svůj web v rámci služby IIS. Tato metoda však není testovací proces nasazení nebo ověřit, že výsledek procesu nasazení bude pracovat správně.
2. Nasaďte aplikaci do služby IIS na vašem vývojovém počítači s použitím stejného procesu, které použijete později k nasazení do produkčního prostředí. Tato metoda ověří proces nasazení kromě ověřuje, že vaše aplikace poběží správně v rámci služby IIS.
3. Nasazení aplikace do testovacího prostředí, který je co nejblíže k produkčnímu prostředí. Protože je produkčním prostředí pro tyto kurzy poskytovatele hostitelských služeb třetích stran, ideální testovacího prostředí by druhého účtu u poskytovatele hostingu. Tento druhý účet byste použili pouze pro testování, ale byste nastavili stejným způsobem jako produkčního účtu.

Tento kurz ukazuje kroky 2. Poskytujeme pokyny pro možnost 3, na konci [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzu, a na konci tohoto kurzu jsou odkazy na zdroje informací pro možnost 1.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurace aplikace pro spuštění v úrovni Medium Trust

Před instalací služby IIS a nasazení do ní, změníte nastavení souboru Web.config aby webu spuštění více jako způsobí v typické sdílené hostitelského prostředí.

Poskytovatelé hostingu obvykle běží vaše webová stránka *úrovni medium trust*, což znamená, že není povoleno provést na několik věcí. Například kód aplikace nemůže získat přístup k registru Windows a nejde přečíst nebo zapisovat soubory, které se nachází mimo hierarchii složek vaší aplikace. Ve výchozím nastavení vaše aplikace spuštěná *vysokou důvěryhodností* v místním počítači, což znamená, že aplikace může být schopen provádět operace, které selže při nasazování do produkčního prostředí. Proto aby testovací prostředí, ve kterém více přesně vystihují produkčního prostředí, nakonfigurujete aplikaci, aby běžela v úrovni medium trust.

V souboru Web.config aplikace přidejte **důvěryhodnosti** element v **system.web** elementu, jak je znázorněno v tomto příkladu.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Aplikace se teď spustí v úrovni medium trust ve službě IIS i na svém místním počítači. Toto nastavení umožňuje zachytit co nejdříve všechny pokusy kód aplikace něco udělat, aby zabránil nezdařené v produkčním prostředí.

> [!NOTE]
> Pokud používáte migrace Entity Framework Code First, ujistěte se, že máte verze 5.0 nebo novější. V rozhraní Entity Framework verze 4.3 Migrace vyžaduje úplný vztah důvěryhodnosti za účelem aktualizace schématu databáze.


## <a name="installing-iis-and-web-deploy"></a>Instalace služby IIS a webové nasazení

K nasazení do služby IIS na vašem vývojovém počítači, musíte mít službu IIS a nasazení webu nainstalován. Ty nejsou zahrnuté ve výchozí konfiguraci Windows 7. Pokud jste již nainstalovali IIS a nasazení webu, přejděte k další části.

Použití [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) je preferovaný způsob, jak nainstalovat službu IIS a nasazení webu, protože instalačního programu webové platformy nainstaluje doporučenou konfiguraci pro službu IIS a automaticky instaluje nezbytné požadavky pro službu IIS a Web Nasazení v případě potřeby.

Ke spuštění instalačního programu webové platformy nainstalovat službu IIS a nasazení webu, použijte následující odkaz. Pokud jste již nainstalovali IIS, Web Deploy nebo některý z jejich požadované součásti, instalačního programu webové platformy nainstaluje jenom toho, co chybí.

- [Instalace IIS a nasazení webu pomocí instalace webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Nastavení výchozí fond aplikací na rozhraní .NET 4

Po instalaci služby IIS, spusťte **Správce služby IIS** abyste měli jistotu, že rozhraní .NET Framework verze 4 je přiřazeno výchozí fond aplikací.

Z Windows **Start** nabídce vyberte možnost **spustit**, zadejte "inetmgr" a potom klikněte na tlačítko **OK**. (Pokud **spustit** příkaz není v vaše **Start** nabídky, můžete stisknout klávesu Windows a jazyka R ho otevřete. Nebo klikněte pravým tlačítkem na hlavním panelu, klikněte na tlačítko **vlastnosti**, vyberte **nabídky Start** klikněte na tlačítko **vlastní**a vyberte **spusťte příkaz**.)

V **připojení** podokně rozbalte uzel serveru a vyberte **fondy aplikací**. V **fondy aplikací** podokně, pokud **DefaultAppPool** je přiřazen k rozhraní .NET framework verze 4 jako na následujícím obrázku, přejděte k další části.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Pokud se zobrazí pouze dva fondy aplikací a jejich jsou nastaveny na rozhraní .NET Framework 2.0, je nutné nainstalovat technologii ASP.NET 4 ve službě IIS:

- Otevřete okno příkazového řádku kliknutím pravým tlačítkem myši **příkazového řádku** v Windows **Start** nabídky a vyberete **spustit jako správce**. Potom spusťte [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) instalace technologie ASP.NET 4 ve službě IIS, pomocí následujících příkazů. (V 64bitových systémech, nahraďte "Rozhraní" s "Framework64".)

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Tento příkaz vytvoří nové fondy aplikací pro rozhraní .NET Framework 4, ale výchozí fond aplikací bude stále nastaven na 2.0. Je budete nasazovat aplikace, který cílí na rozhraní .NET 4 do tohoto fondu aplikací, je nutné změnit fond aplikací na rozhraní .NET 4.

Pokud jste zavřeli **Správce služby IIS**, znovu jej spusťte, rozbalte uzel serveru a klikněte na tlačítko **fondy aplikací** zobrazíte **fondy aplikací** podokně znovu.

V **fondy aplikací** podokně klikněte na tlačítko **DefaultAppPool**a potom v **akce** podokně **základní nastavení**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

V **upravit fond aplikací** dialogovém okně Změnit **verzi rozhraní .NET Framework** k **rozhraní .NET Framework v4.0.30319** a klikněte na tlačítko **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Nyní jste připraveni k publikování do služby IIS.

## <a name="publishing-to-iis"></a>Publikování do služby IIS

Existuje několik způsobů, jak můžete nasadit pomocí sady Visual Studio 2010 a nasazení webu:

- Pomocí sady Visual Studio publikování jedním kliknutím.
- Vytvoření *balíček pro nasazení* a nainstalujte ho pomocí uživatelského rozhraní Správce služby IIS. Balíček pro nasazení se skládá z *ZIP* soubor, který obsahuje všechny soubory a metadata potřebná k instalaci lokality ve službě IIS.
- Vytvořit balíček pro nasazení a nainstalovat pomocí příkazového řádku.

Proces, který jste provedli v předchozích kurzech k nastavení sady Visual Studio k automatizaci úloh nasazení se vztahuje na všechny tyto tři metody. V těchto kurzech použijete první z těchto metod. Informace o používání balíčků pro nasazení najdete v tématu [mapa obsahu nasazení ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

Před publikováním, ujistěte se, že používáte Visual Studio v režimu správce. (Ve Windows 7 **Start** nabídky, klikněte pravým tlačítkem na ikonu pro verzi sady Visual Studio, kterou používáte a vyberte **spustit jako správce**.) Režim správce je vyžadován pro publikování, jen pokud publikujete do služby IIS v místním počítači.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity (nikoli projekt ContosoUniversity.DAL) a vyberte **publikovat**.

**Publikování webu** průvodce se zobrazí.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

V rozevíracím seznamu vyberte  **&lt;nový... &gt;**.

V **nový profil** dialogové okno, zadejte "Test" a potom klikněte na tlačítko **OK**.

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Tento název je že stejný jako prostřední uzel Web.Test.config transformovat soubor, který jste vytvořili dříve. Tuto komunikaci se, co způsobí, že transformace Web.Test.config uplatňovat, když publikujete pomocí tohoto profilu.

Průvodce automaticky přejde **připojení** kartu.

V **adresa URL služby** zadejte *localhost*.

V **web/aplikace** zadejte *výchozí webový server/ContosoUniversity*.

V **cílovou adresu URL** zadejte `http://localhost/ContosoUniversity`.

**Cílovou adresu URL** nastavení není povinné. Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč a tuto adresu URL. Pokud nechcete, aby prohlížeč, aby po nasazení automaticky otevře, ponechte toto pole prázdné.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Klikněte na tlačítko **ověřit připojení** k ověření, že je nastavení správné a může připojit ke službě IIS na místním počítači.

Zelená značka zaškrtnutí ověří, zda je připojení úspěšné.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Klikněte na tlačítko **Další** pro přechod **nastavení** kartu.

**Konfigurace** rozevíracího seznamu určuje konfiguraci sestavení a nasadit. Výchozí hodnota je verze, která požadujete.

Nechte **odebrat další soubory v cílovém umístění** políčko zaškrtnuto. Protože je to vaše první nasazení, nebude existovat všechny soubory v cílové složce ještě.

V **databází** části, zadejte následující hodnotu v poli připojovací řetězec pro **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Proces nasazení se umístit tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** zaškrtnuto.

Také v části **SchoolContext**vyberte **použít migrace Code First**. Tato možnost způsobí, že proces nasazení ke konfiguraci nasazeném souboru Web.config pro zadání `MigrateDatabaseToLatestVersion` inicializátor. Databáze tento inicializátor automaticky aktualizuje na nejnovější verzi, když aplikace poprvé po nasazení získá přístup k databázi.

V poli připojovací řetězec pro **objekt DefaultConnection**, zadejte následující hodnotu:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Ponechte **aktualizace databáze** vymazána. Nasadí se členství v databázi tak, že zkopírujete soubor SDF v aplikaci\_dat a nechcete, aby proces nasazení dělat nic dalšího pomocí této databáze.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Klikněte na tlačítko **Další** k přechodu na **ve verzi Preview** kartu.

V **ve verzi Preview** klikněte na tlačítko **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Klikněte na tlačítko **publikovat**.

Pokud aplikace Visual Studio není v režimu správce, může získat chybovou zprávu, která označuje chybu oprávnění. V takovém případě ukončit sadu Visual Studio, otevřete v režimu správce a zkuste publikování znovu.

Pokud je v režimu správce, Visual Studio **výstup** okno sestavy úspěšné sestavení a publikování.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Prohlížeč se automaticky otevře Contoso University domovskou stránku běží ve službě IIS na místním počítači.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testování v testovacím prostředí

Všimněte si, že prostředí indikátor zobrazuje "(testovací)" místo "(vývoj)", který ukazuje, že *Web.config* transformace pro ukazatel prostředí bylo úspěšné.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Spustit **studenty** stránku a ověřte, zda má nasazené databáze se žádní studenti. Když vyberete tuto stránku může trvat několik minut, než načíst, protože Code First vytvoří databázi a pak spustí `Seed` metody. (To neprovedli, když jste byli na domovskou stránku, protože při pokusu o přístup k databázi ještě nebyl v aplikaci.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Spustit **Instruktoři** stránku a ověřte, že Code First naplnila databázi s instruktorem dat:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Vyberte **přidat studenty** z **studenty** přidat student nabídky a pak zobrazit nového objektu student do **studenty** stránku a ověřte, že můžete úspěšně zapisovat do databáze :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Z **kurzy** nabídce vyberte možnost **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **přihlásit** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, které jste vytvořili dříve ("admin" a "Pa$ w0rd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozím kurzu byla správně nasazena do testovacího prostředí.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Ověřte, že *Elmah* složka existuje s zástupný soubor v ní.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Kontrola změn automatické Web.config pro migrace Code First

Otevřít *Web.config* souboru v nasazení aplikace na *C:\inetpub\wwwroot\ContosoUniversity* a je vidět, kde proces nasazení automaticky nakonfigurované migrace Code First pro Aktualizujte databázi na nejnovější verzi.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Proces nasazení vytvoří taky nový připojovací řetězec pro migrace Code First používat výhradně pro aktualizaci schématu databáze:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Tento další připojovací řetězec můžete zadat jeden uživatelský účet pro aktualizace schématu databáze a jiný uživatelský účet pro přístup k datům aplikace. Například můžete přiřadit databáze\_role vlastníka k migrace Code First a db\_datareader a db\_datawriter nesmí být role pro aplikaci. Toto je běžný vzor v obrany, který brání potenciálně škodlivý kód v aplikaci změny schématu databáze. (Například k tomu může dojít v úspěšném útoku prostřednictvím injektáže SQL.) Tento model není používán těchto kurzů. Nevztahuje se na SQL Server Compact a nevztahuje se při migraci na SQL Server v pozdějších kurzech v této sérii. Web Cytanium nabízí jen jeden uživatelský účet pro přístup k databázi serveru SQL Server, který vytvoříte v Cytanium. Pokud budete moct tento model implementovat ve vašem scénáři, můžete provést podle následujícího postupu:

1. V **nastavení** karty **Publikovat Web** průvodce, zadejte připojovací řetězec, který určuje uživatel s oprávněními aktualizovat schéma celé databáze a zrušte zaškrtnutí **použít tento připojovací řetězec za běhu** zaškrtávací políčko. V nasazeném souboru Web.config, toto řešení `DatabasePublish` připojovací řetězec.
2. Vytvoření transformace souboru Web.config pro připojovací řetězec, který chcete, aby aplikace pro použití v době běhu.

Nyní nasazení aplikace do služby IIS na vašem vývojovém počítači a testovat existuje. Ověří, že proces nasazení zkopíroval obsah aplikace na správné umístění (s výjimkou souborů, které jste nechtěli nasazení) a také nasazení webu služby IIS správně nakonfigurován během nasazení. V dalším kurzu spustíte jeden další test, který vyhledá úlohu nasazení, které nebylo dosud provedeno: nastavení oprávnění pro složky *Elmah* složky.

## <a name="more-information"></a>Další informace

Informace o spuštění služby IIS nebo IIS Express v sadě Visual Studio naleznete na následujících odkazech:

- [Přehled služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) na webu IIS.net.
- [Úvod do služby IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) v blogu Scotta Guthrie.
- [Postupy: určení webového serveru pro webové projekty v sadě Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Hlavní rozdíly mezi služby IIS a serveru ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) na webu ASP.NET.
- [Testování aplikace webových formulářů ve službě IIS 7 nebo technologie ASP.NET MVC za 30 sekund](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) na blogu Ricka Andersona. Tato položka obsahuje příklady, proč není stejně spolehlivá jako testování ve službě IIS Express testování pomocí vývojového serveru Visual Studio (Cassini) a proč není stejně spolehlivá jako testování ve službě IIS testování ve službě IIS Express.

Informace o problémech, které mohou nastat při spuštění aplikace v úrovni medium trust, najdete v části [hostování aplikace ASP.NET ve střední důvěryhodnosti](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys Rolla webu.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [další](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
