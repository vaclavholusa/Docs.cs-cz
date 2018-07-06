---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do testovacího prostředí | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 6bfd1399c9e627839005fa27086c90bc0cc049e5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826365"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nasazení do testovacího prostředí
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

Tento kurz ukazuje, jak nasadit webovou aplikaci ASP.NET do služby IIS v místním počítači.

Když vyvíjíte aplikaci, je obvykle otestovat spuštěním v sadě Visual Studio. Projekty webových aplikací v sadě Visual Studio 2012 používat jako vývojovému webovému serveru ve výchozím nastavení, služby IIS Express. Služba IIS Express chová podobně jako úplnou službu IIS, než Server sady Visual Studio vývoje (nazývaný též Cassini), která ve výchozím nastavení používá Visual Studio 2010. Ale ani vývojovému webovému serveru funguje úplně stejně jako služby IIS. V důsledku toho je možné, že aplikace bude správně spustit, když testování v sadě Visual Studio, ale nezdaří, pokud je nasazený do služby IIS.

Aplikaci můžete otestovat spolehlivěji následujícími způsoby:

1. Nasaďte aplikaci do služby IIS na vašem vývojovém počítači s použitím stejného procesu, které použijete později k nasazení do produkčního prostředí. Visual Studio pro použití služby IIS při spuštění projektu, ale způsobem, který by otestovali procesu nasazení můžete nakonfigurovat. Tato metoda ověří proces nasazení kromě ověřuje, že vaše aplikace poběží správně v rámci služby IIS.
2. Nasazení aplikace do testovacího prostředí, která je téměř shodná do produkčního prostředí. Protože produkčního prostředí pro tyto kurzy jsou Web Apps ve službě Azure App Service, je ideální testovacího prostředí další webové aplikace vytvořené ve službě Azure App Service. Tento druhý webovou aplikaci použijete pouze pro testování, ale byste nastavili stejným způsobem jako produkční webovou aplikaci.

Možnost 2 je nejspolehlivější způsob, jak otestovat, a pokud to uděláte, nemusíte být nutně možnost 1. Nicméně pokud provádíte nasazení do jiného poskytovatele možnost hostování 2 nemusí být možné nebo může být náročné, takže v této sérii kurzů zobrazí obě metody. Pokyny pro možnost 2 je k dispozici v [nasazení do produkčního prostředí](deploying-to-production.md) kurzu.

Další informace o používání webových serverů v sadě Visual Studio najdete v tématu [webové servery v sadě Visual Studio pro webové projekty ASP.NET](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="install-iis"></a>Instalace služby IIS

K nasazení do služby IIS na vašem vývojovém počítači, musíte mít službu IIS a nasazení webu nainstalován. Ve výchozím nastavení se sadou Visual Studio je nainstalován nástroj nasazení webu, ale služba IIS není zahrnutý ve výchozí konfiguraci Windows 8 nebo Windows 7. Pokud jste již nainstalovali IIS a výchozí fond aplikací je již nastaven na rozhraní .NET 4, přejděte k [v další části](#sqlexpress).

1. Použití [instalačního programu webové platformy](https://www.microsoft.com/web/downloads/platform.aspx) je preferovaný způsob, jak nainstalovat službu IIS a nasazení webu, protože instalačního programu webové platformy nainstaluje doporučenou konfiguraci pro službu IIS a automaticky instaluje nezbytné požadavky pro službu IIS a Web Nasazení v případě potřeby.

    Ke spuštění instalačního programu webové platformy nainstalovat službu IIS a nasazení webu, použijte následující odkaz. Pokud jste již nainstalovali IIS, Web Deploy nebo některý z jejich požadované součásti, instalačního programu webové platformy nainstaluje jenom toho, co chybí.

   - [Instalace IIS a nasazení webu pomocí instalace webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Zobrazí zprávy, která udává, že se nainstaluje službu IIS 7. Odkaz funguje pro služby IIS 8 v systému Windows 8, ale pro Windows 8 Ujistěte se, že je nainstalována technologie ASP.NET 4.5 provedením následujících kroků:

   - Otevřít **ovládací panely**, **programy a funkce**, **Windows zapnout nebo vypnout funkce**.
   - Rozbalte **Internetová informační služba**, **webové služby**, a **funkce pro vývoj aplikací**.
   - Ujistěte se, že **technologie ASP.NET 4.5** zaškrtnuto.

      ![Vyberte technologii ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Po instalaci služby IIS, spusťte **Správce služby IIS** abyste měli jistotu, že rozhraní .NET Framework verze 4 je přiřazeno výchozí fond aplikací.

1. Stiskněte WINDOWS + R otevřete **spustit** dialogové okno.

    (Nebo v systému Windows 8 zadejte "spustit" na **Start** stránky, nebo ve Windows 7 vyberte **spustit** z **Start** nabídky. Pokud **spustit** není v **Start** nabídky, klikněte pravým tlačítkem na hlavním panelu, klikněte na tlačítko **vlastnosti**, vyberte **nabídky Start** klikněte na tlačítko **Vlastní**a vyberte **spusťte příkaz**.)
2. Zadejte "inetmgr" a potom klikněte na tlačítko **OK**.
3. V **připojení** podokně rozbalte uzel serveru a vyberte **fondy aplikací**. V **fondy aplikací** podokně, pokud **DefaultAppPool** je přiřazen k rozhraní .NET framework verze 4 jako na následujícím obrázku, přejděte k další části.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Pokud se zobrazí pouze dva fondy aplikací a jejich jsou nastaveny na rozhraní .NET Framework 2.0, budete muset nainstalovat technologii ASP.NET 4 ve službě IIS.

    Windows 8 naleznete v tématu podle pokynů v předchozí části a ujistěte se, že technologie ASP.NET 4.5 je nainstalovaná, nebo v tématu [tohoto článku znalostní BÁZE](https://support.microsoft.com/kb/2736284). Pro Windows 7, otevřete okno příkazového řádku kliknutím pravým tlačítkem myši **příkazového řádku** v Windows **Start** nabídky a vyberete **spustit jako správce**. Potom spusťte [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) instalace technologie ASP.NET 4 ve službě IIS, pomocí následujících příkazů. (V 32bitových systémech, nahraďte "Framework64" s "Rozhraní".)

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Tento příkaz vytvoří nové fondy aplikací pro rozhraní .NET Framework 4, ale výchozí fond aplikací bude stále nastaven na 2.0. Je budete nasazovat aplikace, který cílí na rozhraní .NET 4 do tohoto fondu aplikací, je nutné změnit fond aplikací na rozhraní .NET 4.
5. Pokud jste zavřeli **Správce služby IIS**, znovu jej spusťte, rozbalte uzel serveru a klikněte na tlačítko **fondy aplikací** zobrazíte **fondy aplikací** podokně znovu.
6. V **fondy aplikací** podokně klikněte na tlačítko **DefaultAppPool**a potom v **akce** podokně **základní nastavení**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. V **upravit fond aplikací** dialogovém okně Změnit **verzi rozhraní .NET Framework** k **rozhraní .NET Framework v4.0.30319** a klikněte na tlačítko **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

Služba IIS je teď připravená k publikování webové aplikace k němu, ale než to můžete udělat, je nutné vytvořit databáze, které budete používat v testovacím prostředí.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Nainstalujte SQL Server Express

Nespouští fungují ve službě IIS, takže pro testovací prostředí je potřeba mít nainstalovaný SQL Server Express LocalDB. Pokud používáte Visual Studio 2010 SQL Server Express je nainstalovaná ve výchozím nastavení. Pokud používáte sadu Visual Studio 2012, musíte ji nainstalovat.

Pokud chcete nainstalovat systém SQL Server Express, nainstalujte ji z [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) kliknutím [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) nebo [ ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Pokud se rozhodnete je nesprávný pro váš systém nebude schopen provést instalaci a zkuste druhou.

Na první stránce Centrum instalace SQL serveru, klikněte na tlačítko **samostatná instalace nového serveru SQL Server nebo přidání funkcí do existující instalace**a postupujte podle pokynů, přijímat výchozí volby. V Průvodci instalací přijměte výchozí nastavení. Další informace o možnostech instalace najdete v tématu [instalace systému SQL Server 2012 pomocí Průvodce instalací (nastavení)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Vytvoření databáze SQL Server Express pro testovací prostředí

Aplikace Contoso University má dvě databáze: databáze členství a také databázi aplikace. Tyto databáze můžete nasadit na dvě samostatné databáze nebo do jediné databáze. Můžete kombinovat předpřipravených databáze spojení mezi vaší aplikační databáze a databáze členství. Pokud provádíte nasazení do poskytovatele hostitelských služeb třetích stran, může váš plán hostování také zadejte důvod k jejich zkombinování. Například poskytovatele hostitelských služeb můžou účtovat další možnosti pro více databází nebo nemusí povolit i více než jednu databázi.

V tomto kurzu nasadíte dvě databáze v testovacím prostředí a jednu databázi přípravného a produkčního prostředí.

Z **zobrazení** nabídky v sadě Visual Studio vyberte **Průzkumníka serveru** (**Průzkumník databáze** v aplikaci Visual Web Developer) a potom klikněte pravým tlačítkem na **datová připojení**  a vyberte **vytvořit novou databázi SQL serveru**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

V **vytvořit novou databázi SQL serveru** dialogového okna zadejte ". \SQLExpress" v **název serveru** pole a "aspnet-ContosoUniversity" v **nového názvu databázového** pole, pak Klikněte na tlačítko **OK**.

![Vytvoření aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

Postupujte stejným způsobem vytvoření nové databáze SQL serveru Express školy s názvem "ContosoUniversity".

**Průzkumník serveru** teď zobrazují dvě nové databáze.

![Nové databáze v Průzkumníku serveru](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Vytvoření nové databáze skriptu udělení

Při spuštění aplikace ve službě IIS na vašem vývojovém počítači aplikace přistupuje k databázi s použitím přihlašovacích údajů výchozí fond aplikací. Ale ve výchozím nastavení, identita fondu aplikací nemá oprávnění k otevření databáze. Proto budete muset spustit skript k udělení oprávnění. V této části vytvoříte skript, že je potřeba spustit později, abyste měli jistotu, že aplikace bude moci otevřít databáze při spuštění ve službě IIS.

Klikněte pravým tlačítkem řešení (ne z projektů) a klikněte na tlačítko **přidat novou položku**a pak vytvořte nový **soubor SQL** s názvem *Grant.sql*. Zkopírujte následující příkazy SQL do souboru a pak uložte a zavřete soubor:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Tento skript je navržen pro práci s SQL Server Express 2012 a nastavení služby IIS v systému Windows 8 nebo Windows 7, jak jsou uvedeny v tomto kurzu. Pokud používáte jinou verzi systému SQL Server nebo Windows, nebo pokud jste nastavili IIS počítače odlišně, může být vyžadováno změny tohoto skriptu. Další informace o skripty systému SQL Server najdete v tématu [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Poznámka k zabezpečení** tento skript vám db\_oprávnění vlastníka na uživatele, který přistupuje k databázi v době běhu, což je budete mít v provozním prostředí. V některých případech můžete chtít určit jako uživatel, který má celé databáze schéma aktualizovat oprávnění pouze pro nasazení a zadejte jiný uživatel, který má oprávnění pouze ke čtení a zápisu dat běhu. Další informace najdete v tématu [kontroly automatické změn souboru Web.config pro migrace Code First](#reviewingmigrations) dále v tomto kurzu.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Spusťte skript udělení aplikační databáze

Můžete nakonfigurovat profil publikování pro spuštění skriptu udělení v databázi členství během nasazování vzhledem k tomu používá dbDacFx poskytovatele nasazení této databáze. Během migrace Code First nasazení, která je, jak byste nasazovali aplikační databáze nelze spouštět skripty. Proto budete muset ručně spustit skript před nasazením v databázi aplikace.

1. V sadě Visual Studio, otevřete *Grant.sql* soubor, který jste vytvořili dříve.
2. Klikněte na tlačítko **připojit**. 

    ![Tlačítko pro připojení](deploying-to-iis/_static/image11.png)
3. V **připojit k serveru** dialogového okna zadejte *. \SQLExpress* jako **název serveru**a potom klikněte na tlačítko **připojit**.
4. V rozevíracím seznamu databáze vyberte **ContosoUniversity**a potom klikněte na tlačítko **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Identita fondu aplikací výchozí teď má dostatečná oprávnění v databázi aplikace pro migrace Code First k vytvoření databázových tabulek při spuštění aplikace.

## <a name="publish-to-iis"></a>Publikování do služby IIS

Existuje několik způsobů, jimiž můžete nasadit do služby IIS pomocí sady Visual Studio a nasazení webu:

- Pomocí sady Visual Studio publikování jedním kliknutím.
- Publikování z příkazového řádku.
- Vytvoření *balíček pro nasazení* a nainstalujte ho pomocí uživatelského rozhraní Správce služby IIS. Balíček pro nasazení se skládá z *ZIP* soubor, který obsahuje všechny soubory a metadata potřebná k instalaci lokality ve službě IIS.
- Vytvořit balíček pro nasazení a nainstalovat pomocí příkazového řádku.

Proces, který jste provedli v předchozích kurzech k nastavení sady Visual Studio k automatizaci úloh nasazení se vztahuje na všechny tyto metody. V těchto kurzech použijete první dva z těchto metod. Informace o používání balíčků pro nasazení najdete v tématu [nasazení webové aplikace pomocí vytvoření a instalace balíčku pro nasazení webu](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) v obsahu mapy webové nasazení pro Visual Studio a ASP.NET.

Před publikováním, ujistěte se, že používáte Visual Studio v režimu správce. Pokud nevidíte **(správce)** v záhlaví okna zavřete sadu Visual Studio. V systému Windows 8 **Start** stránky nebo Windows 7 **Start** nabídky, klikněte pravým tlačítkem na ikonu pro verzi sady Visual Studio, kterou používáte a vyberte **spustit jako správce**. Režim správce je vyžadován pro publikování, jen pokud publikujete do služby IIS v místním počítači.

### <a name="create-the-publish-profile"></a>Vytvořit profil publikování

1. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt ContosoUniversity (nikoli projekt ContosoUniversity.DAL) a vyberte **publikovat**.

    **Publikování webu** průvodce se zobrazí.

    ![Karta Web Průvodce profil publikování](deploying-to-iis/_static/image13.png)
2. V rozevíracím seznamu vyberte  **&lt;nový... &gt;**. (S nejnovější aktualizace sady Visual Studio nainstalovaný, není žádný rozevírací seznam a pokud chcete vytvořit nový profil úplně od začátku, klikněte na tlačítko je **vlastní**.)
3. V **nový profil** dialogové okno, zadejte "Test" a potom klikněte na tlačítko **OK**.

    Průvodce automaticky přejde **připojení** kartu.
4. V **adresa URL služby** zadejte *localhost*.
5. V **web/aplikace** zadejte *výchozí webový server/ContosoUniversity*
6. V **cílovou adresu URL** zadejte `http://localhost/ContosoUniversity`

    **Cílovou adresu URL** nastavení není povinné. Po dokončení nasazení aplikace Visual Studio automaticky otevře výchozí prohlížeč a tuto adresu URL. Pokud nechcete, aby prohlížeč, aby po nasazení automaticky otevře, ponechte toto pole prázdné.
7. Klikněte na tlačítko **ověřit připojení** k ověření, že je nastavení správné a může připojit ke službě IIS na místním počítači.

    Zelená značka zaškrtnutí ověří, zda je připojení úspěšné.

    ![Publikování webových kartě připojení Průvodce](deploying-to-iis/_static/image14.png)
8. Klikněte na tlačítko **Další** pro přechod **nastavení** kartu.
9. **Konfigurace** rozevíracího seznamu určuje konfiguraci sestavení a nasadit. Nechte nastavené na výchozí hodnotu verze. Nebude nasazení sestavení ladicí verze v tomto kurzu.
10. Rozbalte **možností publikování souboru**a pak vyberte **vyloučit soubory z aplikace\_složka dat**.

    V testovacím prostředí, aplikace bude přístup k databází, které jste vytvořili v místní instanci systému SQL Server Express, není .mdf soubory *aplikace\_Data* složky.
11. Nechte **Precompile během publikování** a **odebrat další soubory v cílovém umístění** zaškrtávací políčka prázdná.

    ![Na kartě Nastavení možností publikování souboru](deploying-to-iis/_static/image15.png)

    Předkompilace je tato možnost je užitečná hlavně pro velké weby. Doba spuštění stránky při prvním vyžádání stránky po publikování na webu může snížit.

    Není nutné odebrat další soubory, jelikož Toto je první nasazení a nebudou všechny soubory v cílové složce ještě.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Pokud vyberete **odebrat další soubory** pro následné nasazení do stejné lokality, ujistěte se, pozor, abyste použili funkci ve verzi preview, takže uvidíte předem soubory, které se odstraní, před nasazením. Chování je očekávané, nasazení webu se odstraní soubory na cílovém serveru, který jste odstranili ve vašem projektu. Však porovnání strukturu celou složku ve zdrojové a cílové složky a v některých scénářích nasazení webu může odstranit soubory, které nechcete odstranit.
    > 
    > Například pokud máte webovou aplikaci do podsložky na serveru při nasazení projektu do kořenové složky, podsložky se odstraní. Může mít jeden projekt pro hlavní server v doméně contoso.com a jiného projektu pro blog na contoso.com/blog. Blog aplikace je do podsložky. Pokud vyberete odebrat další soubory v cíli při nasazování hlavního webu, blogu aplikace se odstraní.
    > 
    > Další příklad, vaše aplikace\_složka dat může být odstraněný neočekávaně. Některé databáze, jako jsou SQL Server Compact ukládat soubory databáze v aplikaci\_složku Data. Po počátečním nasazení nechcete zachovat kopírování souborů databáze v následné nasazení, takže vybrat vyloučit aplikace\_údaje o kartě Balení/publikování webu. Poté, co můžete udělat, pokud máte odebrat další soubory v cílovém umístění vybrali, soubory databáze a aplikace\_samotné složce Data se odstraní při příštím publikování.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurace nasazení pro databázi členství

Následující postup se vztahuje na **objekt DefaultConnection** databáze v **databází** části dialogového okna.

1. V **vzdálený připojovací řetězec** zadejte následující připojovací řetězec, který odkazuje na novou databázi serveru SQL Server Express členství.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Proces nasazení se umístit tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** zaškrtnuto.

    Můžete také získat připojovací řetězec z **Průzkumníka serveru**. V **Průzkumníka serveru**, rozbalte **datová připojení** a vyberte  **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** databáze, pak z **vlastnosti** okno kopírování **připojovací řetězec** hodnotu. Že připojovací řetězec bude mít jeden další nastavení, které můžete odstranit: `Pooling=False`.
2. Vyberte **aktualizace databáze**.

    To způsobí, že schéma databáze byly vytvořeny v cílové databázi během nasazení. V následujících krocích zadejte další skripty, které je potřeba spustit: z nich se má udělit přístup k databázi na výchozí fond aplikací a nasazení dat. z nich.
3. Klikněte na tlačítko **konfigurovat aktualizace databáze**.
4. V **konfigurace aktualizací databáze** dialogové okno, klikněte na tlačítko **přidat skript SQL** a přejděte na *Grant.sql* skript, který jste předtím uložili ve složce řešení.
5. Opakujte postup pro přidání *aspnet-data-dev.sql* skriptu.

    ![Konfigurace aktualizací databáze pro databázi členství](deploying-to-iis/_static/image16.png)
6. Klikněte na tlačítko **Zavřít**.

### <a name="configure-deployment-for-the-application-database"></a>Ke konfiguraci nasazení pro databázi aplikace

Když Visual Studio zjistí Entity Framework `DbContext` třídy, je vytvořena položka ve **databází** oddíl, který má **spustit migrace Code First** zaškrtávací políčko místo  **Aktualizace databáze** zaškrtávací políčko. Pro účely tohoto kurzu budete používat toto políčko k určení nasazení migrace Code First.

V některých případech je může použít `DbContext` databáze, ale chcete použít poskytovatele dbDacFx místo migrace pro nasazení databáze. V takovém případě naleznete v tématu [jak nasadit bez migrace Code First databázi?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) v nejčastějších Dotazech technologie ASP.NET webové nasazení na webu MSDN.

Následující postup se vztahuje **SchoolContext** databáze v **databází** části dialogového okna.

1. V **vzdálený připojovací řetězec** zadejte následující připojovací řetězec, který odkazuje na nové databáze aplikace SQL Server Express.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Proces nasazení se umístit tento připojovací řetězec v nasazeném souboru Web.config, protože **použít tento připojovací řetězec za běhu** zaškrtnuto.

    Můžete také získat připojovací řetězec databáze aplikace z **Průzkumníka serveru** stejným způsobem, který jste získali členství řetězec připojení k databázi.
2. Vyberte **spustit migrace Code First (spuštěno při spuštění aplikace)**.

    Tato možnost způsobí, že proces nasazení ke konfiguraci nasazeném souboru Web.config pro zadání `MigrateDatabaseToLatestVersion` inicializátor. Databáze tento inicializátor automaticky aktualizuje na nejnovější verzi, když aplikace poprvé po nasazení získá přístup k databázi.

### <a name="configure-publish-profile-transforms"></a>Konfigurace transformace profil publikování

1. Klikněte na tlačítko **Zavřít**a potom klikněte na tlačítko **Ano** při zobrazí se výzva, pokud chcete uložit změny.
2. V **Průzkumníka řešení**, rozbalte **vlastnosti**, rozbalte **PublishProfiles**.
3. Kliknutím Rright *Test.pubxml,* a potom klikněte na tlačítko **přidat konfigurační transformace**.

    ![Přidat konfigurační transformaci nabídky](deploying-to-iis/_static/image17.png)

    Visual Studio vytvoří *Web.Test.config* transformační soubor a otevře jej.
4. V *Web.Test.config* transformovat soubor, vložte následující kód bezprostředně po otevření konfigurační značky.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Pokud používáte testovací profil publikování, tato transformace nastaví prostředí ukazatel na "Test". V nasazeném webu uvidíte "(testovací)" po nadpis H1 "University společnosti Contoso".
5. Soubor uložte a zavřete.
6. Klikněte pravým tlačítkem myši *Web.Test.config* souboru a klikněte na tlačítko **transformace ve verzi Preview** abyste měli jistotu, že transformace kódování vytvoří očekávané změny.

    **Náhled souboru Web.config** okno zobrazuje výsledek použití i *Web.Release.config* transformuje a *Web.Test.config* transformace.

### <a name="preview-the-deployment-updates"></a>Verze Preview aktualizace nasazení

1. Otevřít **Publikovat Web** průvodce znovu (klikněte pravým tlačítkem na projekt ContosoUniversity a klikněte na tlačítko **publikovat**).
2. V **ve verzi Preview** kartu, ujistěte se, že **testovací** profilu je stále vybrán a potom klikněte na **spustit Náhled** zobrazíte seznam souborů, které budou zkopírovány.

    ![Tlačítko náhledu](deploying-to-iis/_static/image18.png)

    ![Náhled publikování](deploying-to-iis/_static/image19.png)

    Můžete také kliknout **databáze ve verzi Preview** odkaz zobrazíte skripty, které se spustí v databázi členství. (Žádné skripty se spouštějí pro nasazení migrace Code First, takže není nutné nic ve verzi preview pro databázi aplikace.)
3. Klikněte na tlačítko **publikovat**.

    Pokud aplikace Visual Studio není v režimu správce, může získat chybovou zprávu, která označuje chybu oprávnění. V takovém případě ukončit sadu Visual Studio, otevřete v režimu správce a zkuste publikování znovu.

    Pokud je v režimu správce, Visual Studio **výstup** okno sestavy úspěšné sestavení a publikování.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Pokud jste zadali adresu URL v **cílovou adresu URL** pole na profil publikování **připojení** kartě prohlížeče se automaticky otevře Contoso University domovskou stránku běží ve službě IIS na místním počítači.

## <a name="test-in-the-test-environment"></a>Testování v testovacím prostředí

Všimněte si, že prostředí indikátor zobrazuje "(testovací)" místo "(vývoj)", který ukazuje, že *Web.config* transformace pro ukazatel prostředí bylo úspěšné.

Spustit **Instruktoři** stránku a ověřte, že Code First naplnila databázi s instruktorem daty. Když vyberete tuto stránku, může trvat několik minut, než načíst, protože Code First vytvoří databázi a pak spustí `Seed` metody. (To neprovedli, když jste byli na domovskou stránku, protože při pokusu o přístup k databázi ještě nebyl v aplikaci.)

Klikněte na tlačítko **studenty** kartu ověřte, že má nasazené databáze se žádní studenti.

Vyberte **přidat studenty** z **studenty** přidat student nabídky a pak zobrazit nového objektu student do **studenty** stránku a ověřte, že můžete úspěšně zapisovat do databáze .

Z **kurzy** nabídce vyberte možnost **aktualizace kredity**. **Aktualizace kredity** stránka vyžaduje oprávnění správce, proto **přihlásit** zobrazí se stránka. Zadejte přihlašovací údaje účtu správce, které jste vytvořili dříve ("admin" a "devpwd"). **Aktualizace kredity** se zobrazí stránka, která ověřuje, že účet správce, který jste vytvořili v předchozím kurzu byla správně nasazena do testovacího prostředí.

Ověřte, že *Elmah* složka existuje v *c:\inetpub\wwwroot\ContosoUniversity* složka se zástupný soubor v ní.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Zkontrolujte automatických změn souboru Web.config pro migrace Code First

Otevřít *Web.config* souboru v nasazení aplikace na *C:\inetpub\wwwroot\ContosoUniversity* a je vidět, kde proces nasazení automaticky nakonfigurované migrace Code First pro Aktualizujte databázi na nejnovější verzi.

![](deploying-to-iis/_static/image21.png)

Proces nasazení vytvoří taky nový připojovací řetězec pro migrace Code First používat výhradně pro aktualizaci schématu databáze:

![Database_Publish připojovací řetězec](deploying-to-iis/_static/image22.png)

Tento další připojovací řetězec můžete zadat jeden uživatelský účet pro aktualizace schématu databáze a jiný uživatelský účet pro přístup k datům aplikace. Například můžete přiřadit **db\_vlastníka** úlohu migrace Code First a **db\_datareader** a **db\_datawriter nesmí být**role pro aplikaci. Toto je běžný vzor v obrany, který brání potenciálně škodlivý kód v aplikaci změny schématu databáze. (Například k tomu může dojít v úspěšném útoku prostřednictvím injektáže SQL.) Tento model není používán těchto kurzů. Pokud chcete tento model implementovat ve vašem scénáři to provedením následujících kroků:

1. V **nastavení** karty **Publikovat Web** průvodce, zadejte připojovací řetězec, který určuje uživatel s oprávněními aktualizovat schéma celé databáze a zrušte zaškrtnutí **použít tento připojovací řetězec za běhu** zaškrtávací políčko. V nasazeném souboru Web.config, toto řešení `DatabasePublish` připojovací řetězec.
2. Vytvoření transformace souboru Web.config pro připojovací řetězec, který chcete, aby aplikace pro použití v době běhu.

## <a name="summary"></a>Souhrn

Nyní nasazení aplikace do služby IIS na vašem vývojovém počítači a testovat existuje.

![Domovské stránky v testu](deploying-to-iis/_static/image23.png)

Ověří, že proces nasazení zkopíroval obsah aplikace na správné umístění (s výjimkou souborů, které jste nechtěli nasazení) a také nasazení webu služby IIS správně nakonfigurován během nasazení. V dalším kurzu spustíte jeden další test, který vyhledá úlohu nasazení, které nebylo dosud provedeno: nastavení oprávnění pro složky *Elmah* složky.

## <a name="more-information"></a>Další informace

Informace o spuštění služby IIS nebo IIS Express v sadě Visual Studio naleznete na následujících odkazech:

- [Přehled služby IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) na webu IIS.net.
- [Úvod do služby IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) v blogu Scotta Guthrie.
- [Webové servery v sadě Visual Studio pro projekty ASP.NET Web](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Hlavní rozdíly mezi služby IIS a serveru ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) na webu ASP.NET.

Informace o problémech, které mohou nastat při spuštění aplikace v úrovni medium trust, najdete v části [hostování aplikace ASP.NET ve střední důvěryhodnosti](http://www.4guysfromrolla.com/articles/100307-1.aspx) na 4 Guys Rolla webu.

> [!div class="step-by-step"]
> [Předchozí](project-properties.md)
> [další](setting-folder-permissions.md)
