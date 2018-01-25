---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do testu | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 01f72e0240e84944f8ffece9a2dbc5802be4646b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do testu
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nasadit webovou aplikaci ASP.NET do služby IIS v místním počítači.

Pokud vyvíjíte aplikaci, byste obecně otestovat spuštěním v sadě Visual Studio. Projekty webových aplikací v sadě Visual Studio 2012 používat jako webový server vývoj ve výchozím nastavení, služba IIS Express. Služba IIS Express chová podobně jako úplnou službu IIS než Visual Studio vývoj Server (nazývaný též Cassini), který používá Visual Studio 2010 ve výchozím nastavení. Ale ani vývoj webový server pracuje úplně stejně jako služby IIS. V důsledku toho je možné, že aplikace bude správně spustit při testování v sadě Visual Studio, ale úspěšná, když je nasazena do služby IIS.

Aplikaci můžete otestovat spolehlivěji těmito způsoby:

1. Nasaďte aplikaci do služby IIS na vývojovém počítači pomocí stejného postupu, který budete používat později ji nasadit do provozního prostředí. Můžete nakonfigurovat pomocí služby IIS, pokud se spuštění projektu, ale učinit nebude otestovat váš proces nasazení sady Visual Studio. Tato metoda ověří váš proces nasazení kromě ověřování, které aplikace poběží správně v rámci služby IIS.
2. Nasaďte aplikaci do testovacího prostředí, který je téměř stejný do provozního prostředí. Vzhledem k tomu, že produkčního prostředí pro tyto kurzy je Web Apps v Azure App Service, je ideální testovací prostředí další webové aplikace vytvořené v Azure App Service. Tento druhý webové aplikace by používat pouze pro testování, ale je by ji nastavit, stejně jako na produkční webové aplikaci.

Možnost 2 je nejspolehlivější způsob, jak otestovat, a pokud tak učiníte, nemusíte nutně možnost 1. Ale pokud nasazujete třetí strany hostování zprostředkovatele možnost 2 nemusí být vhodný nebo může být náročné, takže tento kurz řady zobrazuje obě metody. Pokyny pro možnost 2 je součástí [nasazení do produkčního prostředí](deploying-to-production.md) kurzu.

Další informace o používání webových serverů v sadě Visual Studio najdete v tématu [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="install-iis"></a>Instalace služby IIS

K nasazení do služby IIS na vývojovém počítači, musí mít služby IIS a nainstalovat nasazení webu. Nasazení webu je nainstalovaná ve výchozím nastavení pomocí sady Visual Studio, ale služba IIS není zahrnut ve výchozím nastavení systému Windows 8 nebo Windows 7. Pokud jste již nainstalovali službu IIS a výchozí fond aplikací je již nastaven na rozhraní .NET 4, přejděte k [v další části](#sqlexpress).

1. Pomocí [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) je upřednostňovaný způsob, jak nainstalovat IIS a Web Deploy, protože instalace webové platformy nainstaluje doporučenou konfiguraci pro službu IIS a automaticky nainstaluje předpoklady pro službu IIS a Web Nasazení v případě potřeby.

    Ke spuštění instalačního programu webové platformy nainstalovat službu IIS a Web Deploy, pomocí následujícího odkazu. Pokud jste již nainstalovali službu IIS, nástroje nasazení webu ani v žádné z jejich požadované součásti, instalace webové platformy nainstaluje pouze toho, co chybí.

    - [Instalace IIS a Web Deploy pomocí WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    Zobrazí se zprávy, která udává, že bude nainstalována služba IIS 7. Ujistěte se, funguje odkaz pro IIS 8 v systému Windows 8, ale pro Windows 8 se, že je nainstalována technologie ASP.NET 4.5 provedením následujících kroků:

    1. Otevřete **ovládací panely**, **programy a funkce**, **Windows zapnout nebo vypnout funkce**.
    2. Rozbalte položku **Internetová informační služba**, **webové služby**, a **funkce pro vývoj aplikací**.
    3. Ujistěte se, že **technologie ASP.NET 4.5** je vybrána.

        ![Vyberte položku ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Po instalaci služby IIS, spusťte **Správce služby IIS** a ujistěte se, že rozhraní .NET Framework verze 4 je přiřazený výchozí fond aplikací.

1. Stiskněte klávesu WINDOWS + R otevřete **spustit** dialogové okno.

    (Nebo ve Windows 8 zadejte "spustit" **spustit** stránky, nebo v systému Windows 7 vyberte **spustit** z **spustit** nabídky. Pokud **spustit** není v **spustit** nabídky, klikněte pravým tlačítkem na hlavním panelu, klikněte na tlačítko **vlastnosti**, vyberte **nabídce Start** , klikněte na **Přizpůsobit**a vyberte **spusťte příkaz**.)
2. Zadejte "inetmgr" a pak klikněte na tlačítko **OK**.
3. V **připojení** podokně rozbalte uzel serveru a vyberte **fondy aplikací**. V **fondy aplikací** podokně, pokud **DefaultAppPool** je přiřazen k rozhraní .NET framework verze 4 jako na následujícím obrázku, přejděte k další části.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Pokud se zobrazí pouze dva fondů aplikací a jejich současné jsou nastaveny na rozhraní .NET Framework 2.0, budete muset nainstalovat technologii ASP.NET 4 ve službě IIS.

    Pro systém Windows 8, postupujte podle pokynů v předchozí části pro a ujistěte se, že technologie ASP.NET 4.5 je nainstalován, nebo v tématu [tohoto článku KB](https://support.microsoft.com/kb/2736284). Pro systém Windows 7, otevřete okno příkazového řádku tak, že kliknete pravým tlačítkem na **příkazového řádku** v systému Windows **spustit** nabídky a výběrem **spustit jako správce**. Spusťte [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) nainstalovat technologii ASP.NET 4 ve službě IIS, použijte následující příkazy. (V 32bitové systémy, nahraďte "Framework64" s "Framework".)

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Tento příkaz vytvoří novou fondy aplikací pro rozhraní .NET Framework 4, ale výchozí fond aplikací bude stále nastaveno 2.0. Můžete budete nasazovat aplikace s cílem .NET 4 do tohoto fondu aplikací, takže budete muset změnit fond aplikací na rozhraní .NET 4.
5. Pokud jste zavřeli **Správce služby IIS**, spusťte ji znovu, rozbalte uzel serveru a klikněte na tlačítko **fondy aplikací** zobrazíte **fondy aplikací** podokně znovu.
6. V **fondy aplikací** podokně klikněte na tlačítko **DefaultAppPool**a potom v **akce** podokně klikněte na tlačítko **základní nastavení**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. V **upravit fond aplikací** dialogové okno, změna **verze rozhraní .NET Framework** k **rozhraní .NET Framework v4.0.30319** a klikněte na tlačítko **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

Služba IIS je nyní připraven k publikování webové aplikace k němu, ale předtím, než můžete to udělat, budete muset vytvořit databáze, které budete používat v testovacím prostředí.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Nainstalujte SQL Server Express.

LocalDB nebyla navržena pro práci ve službě IIS, takže pro testovací prostředí je potřeba mít nainstalovaný SQL Server Express. Pokud používáte Visual Studio 2010 SQL Server Express je již nainstalována ve výchozím nastavení. Pokud používáte Visual Studio 2012, musíte ji nainstalovat.

Pokud chcete nainstalovat systém SQL Server Express, nainstalujte ji z [stažení softwaru společnosti Microsoft: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) kliknutím [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) nebo [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Pokud si zvolíte nesprávný pro váš systém se nepodaří nainstalovat, a můžete zkusit jiný.

Na první stránce Centrum instalace SQL serveru, klikněte na tlačítko **samostatná instalace nový Server SQL nebo přidání funkcí do existující instalace**a postupujte podle pokynů, přijímá výchozí volby. V Průvodci instalací přijměte výchozí nastavení. Další informace o možnostech instalace najdete v tématu [instalace systému SQL Server 2012 z Průvodce instalací (Instalační program)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Vytváření databází systému SQL Server Express pro testovací prostředí

Aplikace Contoso univerzity má dvě databáze: databáze členství a databáze aplikace. Tyto databáze můžete nasadit do dvou samostatných databází nebo do jedné databáze. Můžete kombinovat pro usnadnění databáze spojení mezi aplikační databáze a databáze členství. Pokud provádíte nasazení do hostujícího zprostředkovatele třetí strany, plán hostování může také zadat příslušný důvod kombinovat. Například poskytovatel hostingu může účtují informace pro více databází nebo nemusí povolit i více než jednu databázi.

V tomto kurzu nasadíte do dvou databází v testovacím prostředí a jednu databázi v pracovní a provozní prostředí.

Z **zobrazení** nabídky v sadě Visual Studio vyberte **Průzkumníka serveru** (**Průzkumník databáze** v aplikaci Visual Web Developer) a potom klikněte pravým tlačítkem na **datová připojení**  a vyberte **vytvořit novou databázi SQL serveru**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

V **vytvořit novou databázi SQL serveru** dialogovém okně zadejte ". \SQLExpress" v **název serveru** pole a "aspnet-ContosoUniversity" v **nový název databáze** pole, pak Klikněte na tlačítko **OK**.

![Vytvoření aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Postupujte stejným způsobem, chcete-li vytvořit novou databázi SQL serveru Express školní s názvem "ContosoUniversity".

**V Průzkumníku serveru** nyní zobrazuje dvě nové databáze.

![Nové databáze v Průzkumníku serveru](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Vytvoření skriptu udělte nové databáze

Když aplikace běží ve službě IIS na vývojovém počítači, aplikace přistupovat k databázi pomocí přihlašovacích údajů výchozí fond aplikací. Nicméně ve výchozím nastavení, identita fondu aplikací nemá oprávnění k otevření databáze. Proto je třeba spustit skript k udělení tohoto oprávnění. V této části můžete vytvořit skript, že je potřeba spustit později a ujistěte se, že aplikace bude moci otevřít databáze při spuštění v IIS.

Klikněte pravým tlačítkem řešení (ne z projektů) a klikněte na tlačítko **přidat novou položku**a poté vytvořit novou **soubor SQL** s názvem *Grant.sql*. Zkopírujte následující příkazy SQL do souboru a uložte a zavřete soubor:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Tento skript je navržen pro práci s SQL Server Express 2012 a nastavení služby IIS v systému Windows 8 nebo Windows 7, jako jsou uvedené v tomto kurzu. Pokud používáte jinou verzi systému SQL Server nebo Windows, nebo pokud jste nastavili služby IIS v počítači odlišně, může být potřeba změny tohoto skriptu. Další informace o skripty SQL serveru najdete v tématu [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Poznámka k zabezpečení** tento skript poskytuje db\_oprávnění vlastníka na uživatele, který přistupuje k databázi v době běhu, což je budete mít v provozním prostředí. V některých případech můžete chtít zadat uživatele, který má úplné databáze schéma aktualizovat oprávnění pouze pro nasazení a zadáte jiný uživatel, který má oprávnění pouze ke čtení a zápis dat pro běhu. Další informace najdete v tématu [Kontrola automatické změny souboru Web.config pro migrace Code First](#reviewingmigrations) dál v tomto kurzu.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Spusťte skript grant v databázi aplikace

Můžete nakonfigurovat profil publikování pro spuštění skriptu grant v databázi členství během nasazení vzhledem k tomu, že nasazení databáze používá zprostředkovatele dbDacFx. Při nasazení migrace Code First, což je, jak byste nasazovali databázi aplikace nelze spustit skripty. Proto musíte ručně spustit skript před nasazením v databázi aplikace.

1. V sadě Visual Studio, otevřete *Grant.sql* soubor, který jste vytvořili dříve.
2. Klikněte na tlačítko **připojit**. 

    ![Tlačítko pro připojení](deploying-to-iis/_static/image11.png)
3. V **připojit k serveru** dialogovém okně zadejte *. \SQLExpress* jako **název serveru**a potom klikněte na **Connect**.
4. V rozevíracím seznamu databáze vyberte **ContosoUniversity**a potom klikněte na **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Identita fondu aplikací výchozí teď má dostatečná oprávnění v databázi aplikace pro migrace Code First k vytvoření databázových tabulek, když je aplikace spuštěná.

## <a name="publish-to-iis"></a>Publikování do služby IIS

Existuje několik způsobů, který můžete nasadit do služby IIS pomocí sady Visual Studio a nasazení webu:

- Pomocí sady Visual Studio publikování jedním kliknutím.
- Publikování z příkazového řádku.
- Vytvoření *balíček pro nasazení* a nainstalujte ji pomocí uživatelského rozhraní Správce služby IIS. Balíček pro nasazení se skládá z *.zip* soubor, který obsahuje všechny soubory a metadata potřebná k instalaci lokality ve službě IIS.
- Vytvořit balíček pro nasazení a nainstalujte ji pomocí příkazového řádku.

Proces, který jste se v předchozí kurzů a sady Visual Studio k automatizaci úloh nasazení se vztahuje na všechny z těchto metod. V těchto kurzech použijete první dva z těchto metod. Informace o používání balíčky pro nasazení najdete v tématu [nasazení webové aplikace pomocí vytvoření a instalace balíčku pro nasazení webu](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) v nasazení webového obsahu mapy pro Visual Studio a ASP.NET.

Před publikováním, ujistěte se, že používáte Visual Studio v režimu správce. Pokud nevidíte **(správce)** v záhlaví, zavřete Visual Studio. V systému Windows 8 **spustit** stránky nebo Windows 7 **spustit** nabídky, klikněte pravým tlačítkem myši na ikonu pro verzi Visual Studia, kterou používáte a vyberte **spustit jako správce**. Režim správce je vyžadována pro publikování, pouze když publikujete aplikaci do služby IIS v místním počítači.

### <a name="create-the-publish-profile"></a>Vytvoření profilu publikování

1. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity (nikoli projekt ContosoUniversity.DAL) a vyberte **publikovat**.

    **Publikovat Web** zobrazí se průvodce.

    ![Karta Web Průvodce profilu publikování](deploying-to-iis/_static/image13.png)
2. V rozevíracím seznamu vyberte  **&lt;nový... &gt;**. (S nejnovější aktualizaci sady Visual Studio nainstalována, neexistuje žádný rozevíracího seznamu a na možnost pro vytvoření nového profilu od začátku je **vlastní**.)
3. V **nový profil** dialogové okno, zadejte "Test" a pak klikněte na tlačítko **OK**.

    Průvodce automaticky přejde **připojení** kartě.
4. V **adresa URL služby** zadejte *localhost*.
5. V **web nebo aplikaci** zadejte *Default Web Site/ContosoUniversity*
6. V **cílová adresa URL** zadejte`http://localhost/ContosoUniversity`

    **Cílová adresa URL** nastavení není povinné. Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč na tuto adresu URL. Pokud nechcete, aby prohlížeče otevřete automaticky po nasazení, nechte toto pole prázdné.
7. Klikněte na tlačítko **ověřit připojení** k ověřte, zda jsou nastavení správná, a můžete připojit ke službě IIS v místním počítači.

    Zelená značka zaškrtnutí ověřuje, že je připojení úspěšné.

    ![Publikování webové kartě připojení Průvodce](deploying-to-iis/_static/image14.png)
8. Klikněte na tlačítko **Další** pro přechod **nastavení** kartě.
9. **Konfigurace** rozevíracího seznamu určuje konfiguraci sestavení a nasazení. Nechte nastavené na výchozí hodnotu verze. Nebude nasazení sestavení ladicí verze v tomto kurzu.
10. Rozbalte položku **možností publikování souboru**a potom vyberte **vyloučit soubory z aplikace\_složky dat**.

    V testovacím prostředí, aplikace bude přístup k databázím, které jste vytvořili v místní instanci systému SQL Server Express, není .mdf soubory *aplikace\_Data* složky.
11. Ponechte **Precompile během publikování** a **odebrat další soubory v cílovém umístění** zaškrtávací políčka prázdná.

    ![Na kartě Nastavení možností publikování souboru](deploying-to-iis/_static/image15.png)

    Předkompilace je možnost, která je užitečné hlavně pro velké weby; může snížit čas spuštění stránky pro při prvním vyžádání stránky po publikování na webu.

    Není nutné odebrat další soubory, jelikož Toto je první nasazení a nebude všechny soubory v cílové složce ještě.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Pokud vyberete **odebrat další soubory** pro následné nasazení do stejné lokality, ujistěte se, abyste viděli předem soubory, které budou odstraněny, před nasazením použití funkce preview. Očekávané chování je, že nasazení webu se odstraní soubory na cílovém serveru, který jste odstranili ve vašem projektu. Ale se porovná strukturu celou složku ve zdrojové a cílové složky a v některých scénářích nasazení webu může odstranit soubory, které nechcete odstranit.
    > 
    > Například pokud máte webovou aplikaci v podsložce na serveru při nasazení projektu do kořenové složky, podsložky se odstraní. Může mít jeden projekt pro contoso.com na hlavní lokalitu a pro blog na adrese contoso.com/blog jiného projektu. Blog aplikace je v podsložce. Pokud vyberete odebrat další soubory v cíli při nasazení hlavního webu, aplikace blogu se odstraní.
    > 
    > Další příklad, vaše aplikace\_neočekávaně může získat odstranit složku Data. Některé databáze, například SQL Server Compact ukládat soubory databáze v aplikaci\_složku Data. Po počátečním nasazení nechcete zachovat, vyberte aplikaci, vyloučit kopírování souborů databáze následné nasazení\_dat na kartě Balení/publikování webu. Poté, co můžete udělat, pokud máte odebrat další soubory v cílovém umístění vybrali, tyto databázové soubory a aplikace\_samotné složce Data se odstraní při příštím publikování.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurace nasazení pro databázi členství

Následující postup se vztahuje na **objekt DefaultConnection** databáze **databáze** části dialogového okna.

1. V **vzdáleného připojovací řetězec** zadejte následující připojovací řetězec, který odkazuje na novou databázi SQL Server Express členství.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Proces nasazení bude put tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** je vybrána.

    Můžete také získat připojovací řetězec z **Průzkumníka serveru**. V **Průzkumníka serveru**, rozbalte položku **připojení dat** a vyberte  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** databáze, pak z **vlastnosti** okno kopie **připojovací řetězec** hodnotu. Že připojovací řetězec bude mít jeden další nastavení, které můžete odstranit: `Pooling=False`.
2. Vyberte **aktualizace databáze**.

    To způsobí, že schéma databáze byly vytvořeny v cílové databázi během nasazení. V následujících krocích můžete určit další skripty, které budete muset spustit: jednu pro zajistit přístup do výchozí fond aplikací a jeden pro nasazení dat databáze.
3. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
4. V **konfigurovat aktualizace databáze** dialogové okno, klikněte na tlačítko **přidat skript SQL** a potom přejděte na *Grant.sql* skript, který jste předtím uložili ve složce řešení.
5. Opakujte tento postup pro přidání *aspnet. data dev.sql* skriptu.

    ![Konfigurovat aktualizace databáze pro databázi členství](deploying-to-iis/_static/image16.png)
6. Klikněte na tlačítko **Zavřít**.

### <a name="configure-deployment-for-the-application-database"></a>Nakonfigurujte nasazení pro databázi aplikace

Když Visual Studio zjistí Entity Framework `DbContext` třídy, vytvoří položku v **databáze** oddíl, který má **spustit migrace Code First** políčko místo  **Aktualizace databáze** zaškrtávací políčko. Pro účely tohoto kurzu budete pomocí tohoto zaškrtávacího políčka zadejte nasazení migrace Code First.

V některých scénářích možná používáte `DbContext` databáze, ale chcete použít poskytovatele dbDacFx místo migrace pro nasazení databáze. V takovém případě najdete v části [jak nasadit bez migrace Code First databázi?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) v nejčastějších Dotazech webové nasazení ASP.NET na webu MSDN.

Následující postup se vztahuje na **SchoolContext** databáze **databáze** části dialogového okna.

1. V **vzdáleného připojovací řetězec** zadejte následující připojovací řetězec, který odkazuje na novou databázi aplikace SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Proces nasazení bude put tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** je vybrána.

    Můžete také získat připojovací řetězec databáze aplikace z **Průzkumníka serveru** stejným způsobem, který jste získali členství databáze připojovací řetězec.
2. Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.

    Tato možnost způsobí, že proces nasazení nakonfigurovat v nasazeném souboru Web.config zadejte `MigrateDatabaseToLatestVersion` inicializátor. Tato inicializátoru automaticky aktualizuje databázi na nejnovější verzi, když aplikace po nasazení poprvé získá přístup k databázi.

### <a name="configure-publish-profile-transforms"></a>Konfigurace publikování profil transformací

1. Klikněte na tlačítko **Zavřít**a potom klikněte na **Ano** při zobrazí se výzva, pokud chcete uložit změny.
2. V **Průzkumníku řešení**, rozbalte položku **vlastnosti**, rozbalte položku **PublishProfiles**.
3. Kliknutím Rright *Test.pubxml,* a pak klikněte na **přidat transformace konfigurace**.

    ![Přidejte konfigurační transformaci nabídky](deploying-to-iis/_static/image17.png)

    Visual Studio vytvoří *Web.Test.config* transformační soubor a otevře ji.
4. V *Web.Test.config* transformační soubor, vložte následující kód bezprostředně po otevření konfigurace značky.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Pokud používáte testovací profil publikování, tato transformace nastaví indikátoru prostředí při "Test". V nasazeném lokality zobrazí "(testovací)" po nadpis H1 "Univerzity Contoso".
5. Soubor uložte a zavřete.
6. Klikněte pravým tlačítkem myši *Web.Test.config* souboru a klikněte na tlačítko **Preview transformace** a ujistěte se, že transformace můžete programového vytváří očekávané změny.

    **Web.config Preview** v okně se zobrazí výsledek použití i *Web.Release.config* transformuje a *Web.Test.config* transformace.

### <a name="preview-the-deployment-updates"></a>Náhled aktualizací pro nasazení

1. Otevřete **Publikovat Web** průvodce znovu (pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**).
2. V **Preview** kartě, ujistěte se, že **Test** profil je stále vybrána a pak klikněte na tlačítko **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány.

    ![Tlačítko Náhled](deploying-to-iis/_static/image18.png)

    ![Publikování náhledu](deploying-to-iis/_static/image19.png)

    Můžete také kliknutím **Preview databáze** odkaz zobrazíte skripty, které se spustí v databázi členství. (Žádná skripty se spouštějí nasazení migrace Code First, takže žádná data pro náhled pro databázi aplikace.)
3. Klikněte na tlačítko **publikování**.

    Pokud Visual Studio není v režimu správce, může získat chybovou zprávu, která značí chybu oprávnění. V takovém případě zavřete Visual Studio, otevřete v režimu správce a zkuste to znovu publikovat.

    Pokud Visual Studio je v režimu správce, **výstup** okno sestavy úspěšné sestavení a publikování.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Pokud jste zadali adresu URL v **cílová adresa URL** pole na profil publikování **připojení** kartě prohlížeče automaticky otevře na Contoso univerzity domovskou stránku spuštěná ve službě IIS v místním počítači.

## <a name="test-in-the-test-environment"></a>Testování v testovacím prostředí

Všimněte si, že zobrazuje indikátor prostředí "(testovací)" místo "(vývoj)", který ukazuje, že *Web.config* transformaci pro prostředí indikátoru byla úspěšná.

Spustit **vyučující** a ověřte, že Code First nasadí databáze s lektorem data. Když vyberete tuto stránku, může trvat několik minut, načíst, protože Code First vytvoří databázi a poté spustí `Seed` metoda. (Ho nebylo provádět, když jste byli na domovské stránce, protože při pokusu o přístup k databázi ještě nebylo v aplikaci.)

Klikněte **studenty** a ověřte, zda má databáze nasazené žádné studenty.

Vyberte **přidat studenty** z **studenty** nabídce Přidat student a zobrazte nový student v **studenty** a ověřte, že můžete úspěšně zapisovat do databáze .

Z **kurzy** nabídce vyberte možnost **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **protokolu v** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, který jste vytvořili dříve ("admin" a "devpwd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozí kurzu byla správně nasazena do testovacího prostředí.

Ověřte, že *Elmah* složka existuje v *c:\inetpub\wwwroot\ContosoUniversity* složku s pouze soubor zástupný symbol v ní.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Zkontrolujte automatické změny souboru Web.config pro migrace Code First

Otevřete *Web.config* souboru v nasazení aplikace na *C:\inetpub\wwwroot\ContosoUniversity* a uvidíte, kde proces nasazení nakonfigurované migrace Code First na automaticky aktualizace databáze na nejnovější verzi.

![](deploying-to-iis/_static/image21.png)

Proces nasazení vytvoří taky nový připojovací řetězec pro migrace Code First používat výhradně pro aktualizace schématu databáze:

![Database_Publish připojovací řetězec](deploying-to-iis/_static/image22.png)

Tento další připojovací řetězec můžete zadat jeden uživatelský účet pro aktualizace schématu databáze a jiného uživatelského účtu pro přístup k datům aplikací. Je třeba přiřadit **db\_vlastníka** role migrace Code First, a **db\_DataReader –** a **db\_datawriter**rolí k aplikaci. Toto je běžný vzor obrany zabezpečení, který brání potenciálně škodlivého kódu v aplikaci ve změně schématu databáze. (Například k tomu může dojít v úspěšné útok prostřednictvím injektáže SQL.) Tyto kurzy nepoužívá tohoto vzoru. Pokud chcete implementovat tento vzor ve vašem scénáři, můžete to provést pomocí následujících kroků:

1. V **nastavení** kartě **Publikovat Web** průvodce, zadejte připojovací řetězec, který určuje uživatel s oprávněními aktualizovat schéma úplné databáze a vymazat **použít tento připojovací řetězec v době běhu** zaškrtávací políčko. V nasazeném souboru Web.config, to všechno bude `DatabasePublish` připojovací řetězec.
2. Vytvoření souboru transformace Web.config pro připojovací řetězec, který má aplikace použít za běhu.

## <a name="summary"></a>Souhrn

Teď můžete nasadit aplikace do služby IIS na vývojovém počítači a otestovat ho existuje.

![Domovská stránka v testu](deploying-to-iis/_static/image23.png)

Tím ověříte, že procesu nasazení zkopírovat obsah aplikace do správného umístění (s výjimkou souborů, které nemají mít nasazení) a také nasazení webu služby IIS správně nakonfigurovaný během nasazení. V dalším kurzu, je potřeba spustit jeden další test, který vyhledá nasazení úlohu, která dosud nebylo provedeno: nastavení oprávnění ke složkám na *Elmah* složky.

## <a name="more-information"></a>Další informace

Informace o spuštění služby IIS nebo IIS Express v sadě Visual Studio najdete v následujících zdrojích informací:

- [Přehled služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) na webu IIS.net.
- [Představení služby IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) na blogu Scott Guthrie.
- [Webové servery v sadě Visual Studio pro projekty ASP.NET – webové](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Hlavní rozdíly mezi IIS a ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) na webu technologie ASP.NET.

Informace o problémech, které mohou se vyskytnout při spuštění aplikace v úrovni medium trust, najdete v části [hostování aplikací ASP.NET ve střední důvěryhodnosti](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 nepřetržitého z Rolla lokality.

>[!div class="step-by-step"]
[Předchozí](project-properties.md)
[další](setting-folder-permissions.md)
