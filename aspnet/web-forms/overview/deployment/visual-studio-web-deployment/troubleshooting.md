---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: řešení potíží | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: aspnetcontent
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: b2be5d2f5aa0b7628f7b9338c860593660ef5893
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830889"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: řešení potíží
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


Tato stránka popisuje některé běžné problémy, které mohou nastat při nasazení webové aplikace ASP.NET pomocí sady Visual Studio. U každé z nich jeden nebo více možné příčiny a odpovídající řešení jsou k dispozici.

Scénáře uvedené platí do Azure a externích poskytovatelů hostingu. Další informace o řešení potíží s webovými aplikacemi ve službě Azure App Service naleznete na následujících odkazech:

- [Řešení potíží s webovou aplikací ve službě Azure App Service pomocí sady Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Monitorovat můžete webové aplikace ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Oznamujeme vydání sady Windows Azure SDK 2.0 pro .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (blog ScottGu's, ukazuje, jak získat diagnostické protokoly v sadě Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Chyba serveru v aplikaci – "/" aktuální vlastní nastavení chyb zabránit podrobností o chybě zobrazení vzdáleně

### <a name="scenario"></a>Scénář

Po nasazení webu na vzdáleného hostitele, zobrazí chybová zpráva, která uvádí nastavení customErrors v souboru Web.config, ale neukazuje, jak se skutečné příčinu chyby:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení technologie ASP.NET zobrazuje podrobné informace o chybě pouze v případě, že vaše webová aplikace běží na místním počítači. Obecně nechcete zobrazit podrobné informace o chybě, když je webová aplikace veřejně dostupné přes Internet, protože hackery možné pomocí těchto informací můžete najít ohrožení zabezpečení v aplikaci. Ale při nasazování webu nebo aktualizace do lokality, někdy něco může pokazit a budete muset získat skutečné chybové zprávě.

Umožňuje aplikaci zobrazovat podrobné chybové zprávy, když je spuštěná na vzdáleného hostitele, upravte soubor Web.config pro nastavení režimu customErrors, opětovné nasazení aplikace a spusťte aplikaci znovu:

1. Pokud soubor Web.config aplikace má acustomErrors element v elementu thesystem.web, změňte atribut themode "Off". Jinak přidejte acustomErrors element v elementu thesystem.web s themode atribut nastaven na "off", jak je znázorněno v následujícím příkladu: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Nasazení aplikace.
3. Spuštění aplikace a opakujte cokoli, co jste provedli dříve, která způsobila k dojít k chybě. Teď vidíte, co je skutečné chybové zprávě.
4. Po vyřešení chyby obnovte původní nastavení customErrors a opětovné nasazení aplikace.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Nelze vytvořit nebo stínovou kopii "ContosoUniversity", který již existuje.

### <a name="scenario"></a>Scénář

Při pokusu o spuštění projektu v sadě Visual Studio získáte chybovou stránku a zobrazí se zpráva jako v následujícím příkladu:

Chyba serveru v aplikaci '/'. Nelze vytvořit nebo stínovou kopii "ContosoUniversity", který již existuje.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Počkejte minutu a aktualizujte prohlížeč, nebo znovu zkompilovat web a zkuste spustit znovu.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Přístup byl odepřen na webové stránce, používá SQL Server Compact

### <a name="scenario"></a>Scénář

Po nasazení webu, který používá SQL Server Compact a spuštění stránky v nasazené lokality, který přistupuje k databázi, se zobrazí následující chybová zpráva:

Přístup byl zamítnut. (Výjimka z HRESULT: 0x80070005 (elektronická\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Účet NETWORK SERVICE na serveru musí být schopni číst SQL Compact služba nativní binární soubory, které jsou v *bin\amd64* nebo *bin\x86* složka, ale oprávnění pro čtení pro tyto složky. Sada oprávnění ke čtení pro SÍŤOVOU službu *bin* složku a zkontrolujte, zda rozšíření oprávnění na podsložky.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nelze přečíst konfigurační soubor. z důvodu nedostatečných oprávnění

### <a name="scenario"></a>Scénář

Po kliknutí na Visual Studio tlačítko Publikovat k nasazení aplikace do služby IIS na místním počítači, publikování selže a **výstup** okno zobrazuje chybová zpráva podobná této:

Došlo k chybě při čtení konfiguračního souboru služby IIS "Počítač/PŘESMĚROVÁNÍ". Identita provedením této operace byla... Chyba: Nelze přečíst konfigurační soubor z důvodu nedostatečných oprávnění.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chcete-li použít jedním kliknutím publikovat do služby IIS na místním počítači, musíte používat Visual Studio s oprávněními správce. Zavřete sadu Visual Studio a restartujte ji s oprávněními správce.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nelze se připojit k cílovému počítači... Pomocí zadaného procesu

### <a name="scenario"></a>Scénář

Po kliknutí na Visual Studio tlačítko Publikovat k nasazení aplikace, publikování selže a **výstup** okno zobrazuje chybová zpráva podobná této:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Proxy server je přerušení komunikace s na cílový server. Pomocí ovládacích panelů Windows nebo v aplikaci Internet Explorer, vyberte **Možnosti Internetu** a vyberte **připojení** kartu. V **vlastnosti Internetu** dialogové okno, klikněte na tlačítko **nastavení místní sítě**. V **nastavení místní sítě (LAN)** dialogové okno, zrušte **automaticky zjišťovat nastavení** zaškrtávací políčko. Potom klikněte na tlačítko publikovat znovu.

Pokud se problém nevyřeší, obraťte se na správce systému k určení, co se dá dělat pomocí nastavení brány firewall nebo proxy serveru. Tento problém nastává vzhledem k tomu nestandardního portu nasazení webu používá pro nasazení služby webové správy (8172); pro další připojení nasazení webu používá port 80. Při nasazování do poskytovatele hostitelských služeb třetích stran, obvykle použijete služby webové správy.

## <a name="default-net-40-application-pool-does-not-exist"></a>Výchozí fond 4.0 aplikací rozhraní .NET neexistuje.

### <a name="scenario"></a>Scénář

Když nasadíte aplikaci, která vyžaduje rozhraní .NET Framework 4, zobrazí se následující chybová zpráva:

Výchozí fond aplikací rozhraní .NET 4.0 neexistuje nebo nebylo možné přidat aplikaci. Ověřte prosím, že na tomto počítači je nainstalována technologie ASP.NET 4.0.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, který nasazujete na vývojovém počítači a na kterém je nainstalována aplikace Visual Studio 2010, technologii ASP.NET 4 je nainstalovaná na počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, na které nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Může také musíte ručně nastavit verzi rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu nasazení do služby IIS jako testovacího prostředí kurzu této série.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formát inicializačního řetězce neodpovídá specifikaci začínající na indexu 0.

### <a name="scenario"></a>Scénář

Poté, co nasadíte aplikaci jedním kliknutím pomocí publikování, když spustíte stránku, která přistupuje k databázi získáte následující chybová zpráva:

Formát inicializačního řetězce neodpovídá specifikaci začínající na indexu 0.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Otevřít *Web.config* souboru v nasazené lokality a zkontrolujte, zda hodnoty připojovacího řetězce začínají znakem $(ReplacableToken\_, jako v následujícím příkladu:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Pokud připojovací řetězce vypadají jako v tomto příkladu, upravte soubor projektu a přidejte následující vlastnost PropertyGroup – element, který je u všech konfigurací sestavení:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Znovu nasaďte aplikaci.

## <a name="http-500-internal-server-error"></a>HTTP 500 Internal Server Error

### <a name="scenario"></a>Scénář

Při spuštění webu nasazené, zobrazí se následující chybová zpráva bez konkrétních informací určující příčinu chyby:

Chyba protokolu HTTP 500 - Vnitřní chyba serveru.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Existuje mnoho příčiny 500 chyb, ale je jednou z možných příčin pokud postupujete tyto kurzy vložit XML element ve špatné místo v jednom ze souborů transformace souboru Web.config. Například byste získali tuto chybu, když vložíte transformace, která vloží &lt;umístění&gt; element v rámci &lt;system.web&gt; místo přímo pod &lt;konfigurace&gt;. Funkce ve verzi preview transformace Web.config můžete použít k ověření, že transformace funguje očekávaným způsobem. Řešení, je-li najít transformaci, která byla nesprávně programového je opravte soubor transformace a znovu nasadit. Pokud chyba není zřejmé, zkuste okomentováním odpovídajícího transformací a opětovného nasazení chcete zobrazit, který z nich je příčinou chyby 500.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 se interní chyba serveru

### <a name="scenario"></a>Scénář

Při spuštění bude web se zobrazí následující chybová zpráva:

Chyba protokolu HTTP 500.21 – interní chyba serveru. Obslužná rutina "PageHandlerFactory-Integrated" má chybný modul "ManagedPipelineHandler" ve svém seznamu modulů.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Webu jste nasadili cíle ASP.NET 4, ale ASP.NET 4 není zaregistrovaný ve službě IIS na serveru. Na serveru otevřete příkazový řádek se zvýšenými oprávněními a zaregistrovat technologii ASP.NET 4 spuštěním následujících příkazů:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Může také musíte ručně nastavit verzi rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu nasazení do služby IIS jako testovacího prostředí kurzu této série.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Přihlášení se nezdařilo otevření databáze SQL Server Express v aplikaci\_dat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na databázi SQL Server Express jako *.mdf* ve vaší *aplikace\_Data* složky a první Doba spuštění aplikace, které se zobrazí následující chybová zpráva:

System.Data.SqlClient.SqlException: Databázi "DatabaseName" požadovaný v přihlášení nelze otevřít. Přihlášení se nezdařilo.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Název *.mdf* soubor nesmí shodovat s názvem databáze SQL Server Express, který stále existuje ve vašem počítači i v případě, že jste odstranili *.mdf* soubor dříve existující databáze. Změňte název *.mdf* soubor má název, který dosud nebyl použit jako název databáze a změňte *Web.config* soubor se má použít nový název. Jako alternativu můžete použít [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) odstranit dříve existující systém SQL Server Express databáze.

## <a name="model-compatibility-cannot-be-checked"></a>Model kompatibility nelze zaregistrovat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na nové databáze systému SQL Server Express a při prvním spuštění aplikace se zobrazí následující chybová zpráva:

Model kompatibility nelze zkontrolovat, protože databáze neobsahuje metadata modelu. Ujistěte se, že byl přidán IncludeMetadataConvention konvencím DbModelBuilder.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Pokud název databáze, kterou chcete vložit do souboru Web.config byla použita nikdy předtím, než v počítači, může být databáze již existuje v rámci některé tabulky v ní. Zvolte jiný název, který nebyl použit v počítači před a změnit *Web.config* souboru tak, aby odkazoval na tento nový název databáze používat. Jako alternativu můžete použít [Express nástroje SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) nebo [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) k odstranění existující databáze.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Chyba SQL, když se skript pokusí vytvořit uživatele nebo role

### <a name="scenario"></a>Scénář

Nasazení databáze nakonfigurované na použití **balení/publikování kódu SQL** kartu, skripty SQL, které při nasazení zahrnovat příkazy Create User nebo vytvořit roli a selhání spuštění skriptu při spuštění těchto příkazů. Může se zobrazit podrobné zprávy, jako je následující:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Pokud k této chybě dochází při konfiguraci nasazení databáze v **Publikovat Web** Průvodce místo **balení/publikování kódu SQL** kartu, vytvořit posloupnost v: [konfigurace a Nasazení](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) fóra a řešení se přidají do této stránce řešení potíží.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Uživatelský účet, který používáte k nasazení nemá oprávnění k vytváření uživatelů nebo rolí. Hostovací společnost například může přiřadit databáze\_datareader, db\_datawriter nesmí být a db\_ddladmin role pro uživatelský účet, který nastaví kolekci za vás. Toto jsou dostatečná pro vytvoření Většina databázových objektů, ale ne pro vytváření uživatelů nebo rolí. Jedním ze způsobů, aby se zabránilo chybě je vyloučení uživatelé a role z nasazení databáze. To můžete provést úpravy elementu PreSource pro automaticky generované skript databázi tak, že obsahují následující atributy:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Informace o tom, jak upravit PreSource element v souboru projektu naleznete v tématu [postupy: Úprava nastavení nasazení v souboru projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Pokud uživatelé nebo role v databázi vývoj musí být v cílové databázi, obraťte se na svého poskytovatele hostingových služeb.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Časový limit chyba systému SQL Server při spuštění vlastních skriptů při nasazení

### <a name="scenario"></a>Scénář

Jste zadali vlastní skripty SQL ke spuštění během nasazení a spuštění nasazení webu je, že časový limit.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Spouštění více skriptů, které mají různé transakční režimy mohou způsobit chyby časového limitu. Ve výchozím nastavení automaticky generovaných skriptů spustit v transakci, ale nepodporují vlastní skripty. Pokud vyberete **o přijetí změn dat a/nebo schéma z existující databáze** možnost **balení/publikování kódu SQL** kartu, a pokud chcete přidat vlastní skripty SQL, je nutné změnit nastavení transakcí na některé skripty tak, aby všechny skripty používají stejné nastavení transakce. Další informace najdete v tématu [postupy: nasazení databáze se projekt webové aplikace](https://msdn.microsoft.com/library/dd465343.aspx).

Pokud jste nakonfigurovali nastavení transakcí tak, aby všechny jsou stejné, ale stále se tato chyba, možná alternativní řešení je na spouštění skriptů samostatně. V **databázové skripty** mřížky **balení/publikování** karta SQL, zrušte **zahrnout** zaškrtávací políčko pro skript, který způsobuje chybu časového limitu, poté projekt publikujte. Pak přejděte zpátky do **databázové skripty** mřížce, vyberte tento skript **zahrnout** zaškrtněte políčko a zrušte zaškrtnutí **zahrnout** zaškrtávací políčka pro jiné skripty. Potom znovu publikujte projekt. Tentokrát při publikování, jen vybrané vlastní skript se spustí.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream Data lokality manifestu ještě není k dispozici

### <a name="scenario"></a>Scénář

Při instalaci balíčku pomocí *deploy.cmd* souboru s možností t (testovací) se zobrazí následující chybová zpráva:

Chyba: Streamování dat z ' umístění sitemanifest/dbFullSql [@path= "C:\TEMP\AdventureWorksGrant.sql']/sqlScript" ještě není k dispozici.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chybová zpráva znamená, že příkaz nelze vytvořit testovací sestavu. Příkaz může však spustit, pokud použijete možnost y (vlastní instalace). Zpráva značí pouze, že je nějaký problém se spuštěním příkazu v režimu testu.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Tato aplikace vyžaduje ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scénář

Při pokusu o nasazení se zobrazí následující chybová zpráva:

Fond aplikací, který se pokoušíte použít má vlastnost 'managedRuntimeVersion' nastavena na "v2.0". Tato aplikace vyžaduje "v4.0".

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, který nasazujete na vývojovém počítači a na kterém je nainstalována aplikace Visual Studio 2010, technologii ASP.NET 4 je nainstalovaná na počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, na které nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nelze přetypovat Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scénář

Když nasazujete balíček, zobrazí se následující chybová zpráva:

Nelze přetypovat objekt typu "Microsoft.Web.Deployment.DeploymentProviderOptions' k"Microsoft.Web.Deployment.DeploymentProviderOptions".

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Pokoušíte se nasazení ze Správce služby IIS pomocí uživatelského rozhraní služby webové nasazení 1.1 na serveru, který má Web Deploy 2.0 nainstalované. Pokud používáte nástroj služby IIS pro vzdálenou správu nasazení na základě importování balíčku, zaškrtněte **dostupné nové funkce** dialogové okno po navázání připojení. (Toto dialogové okno může být zobrazil jenom jednou při prvním vytvoření připojení. Vymazat připojení a začít znovu, zavřete Správce služby IIS a jeho opětovné zahájení tak, že zadáte inetmgr/reset na příkazovém řádku.) Pokud jedna z funkcí uvedených **uživatelského rozhraní webu nasadit**a má číslo verze nižší než 8, zatímco nasazujete na server může mít nasazení webu, nainstalované verze 1.1 a 2.0. K nasazení z klienta, který má 2.0 nainstalované, server musí mít pouze nasazení webu 2.0 nainstalované. Budete muset kontaktovat poskytovatele hostingu k vyřešení tohoto problému.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nepovedlo se načíst nativní součásti systému SQL Server Compact

### <a name="scenario"></a>Scénář

Při spuštění bude web se zobrazí následující chybová zpráva:

Nepovedlo se načíst nativní součásti SQL Server Compact odpovídající verze 8482 zprostředkovatele ADO.NET. Nainstalujte správnou verzi systému SQL Server Compact. Přečtěte si článek znalostní BÁZE 974247 další podrobnosti.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Nasazený web nemá *amd64* a *x86* podsložky s nativní sestavení v nich v rámci aplikace *bin* složky. Na počítači, který má SQL Server Compact nainstalován nativní sestavení jsou umístěny v *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. K instalaci balíčku NuGet SqlServerCompact je nejlepší způsob, jak získat správný soubory do správné složky v projektu sady Visual Studio. Instalace balíčku přidává skriptu po sestavení zkopírovat nativní sestavení do *amd64* a *x86*. Aby tato nasazení je však nutné ručně zahrnout je do projektu. Další informace najdete v tématu [nasazení systému SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) kurzu.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Cesta není platná" Chyba po nasazení aplikace Entity Framework Code First

### <a name="scenario"></a>Scénář

Nasazení aplikace, který používá migrace Entity Framework Code First a DBMS, jako je například SQL Server Compact, která ukládá své databáze v souboru v aplikaci\_složku Data. Máte migrace Code First nakonfigurované k vytvoření databáze po prvním nasazení. Při spuštění aplikace zobrazí chybová zpráva jako v následujícím příkladu:

Cesta není platná. Zkontrolujte adresář pro databázi. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Kód nejprve se pokouší vytvořit databázi, ale aplikace\_Data složka neexistuje. Buď nebyly žádné soubory *aplikace\_Data* složky, pokud jste nasadili, nebo jste vybrali **vyloučit aplikace\_Data** na **Balení/publikováníwebu** karta v okně Vlastnosti projektu. Proces nasazení nebude vytvořte složku na serveru, pokud nejsou žádné soubory ve složce, které se mají zkopírovat na server. Pokud už máte databázi nastavit na webu, procesu nasazení odstraní soubory a *aplikace\_Data* samotnou Pokud vyberete složku **odebrat další soubory v cílovém umístění** v profil publikování. Chcete-li problém vyřešit, umístěte zástupný soubor například soubor .txt v *aplikace\_Data* složky, ujistěte se, že nemáte **vyloučit aplikace\_Data** vybrali a znovu nasadit.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nelze použít objekt COM, který byl oddělen od podkladové obálky RCW."

### <a name="scenario"></a>Scénář

Byli jste úspěšně jedním kliknutím pomocí publikování pro nasazení aplikace a pak spusťte zobrazuje tato chyba:

Úloha nasazení webu se nezdařila. (Nelze dokončit žádost o adresu URL vzdáleného agenta '<https://serverurl.com/msdeploy.axd?site=sitename>".)  
 Nelze dokončit žádost o adresu URL vzdáleného agenta '<https://url/msdeploy.axd?site=sitename>".  
Požadavek byl přerušen: požadavek byl zrušen.  
Objekt COM, který byl oddělen od podkladové obálky RCW, nelze použít.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Zavřít a znovu spustit sadu Visual Studio je obvykle všechno potřebné k vyřešení této chyby.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Nasazení selže protože uživatele k použít přihlašovací údaje nemají publikování setACL autority

### <a name="scenario"></a>Scénář

Publikování selže a zobrazí se chyba, která znamená, že nemáte oprávnění k nastavení oprávnění pro složky (uživatelský účet, který používáte nemá setACL autorita).

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení, sady Visual Studio oprávnění ke čtení v kořenové složce serveru a oprávnění k zápisu na aplikaci\_složku Data. Pokud víte, že výchozí oprávnění u složky webu jsou správné a není potřeba nastavit, je toto chování zakázat tak, že přidáte **&lt;IncludeSetACLProviderOn cílové&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** souboru profilu publikování (Chcete-li mít vliv na jeden profil) nebo do souboru wpp.targets (Chcete-li mít vliv na všechny profily). Informace o tom, jak tyto soubory upravit, naleznete v tématu [postupy: Úprava nastavení nasazení profilu (.pubxml) soubory](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Přístup byl odepřen chyby, když se aplikace snaží k zápisu do složky, do aplikace

### <a name="scenario"></a>Scénář

Vaše aplikace chyby při pokusu vytvořit nebo upravit soubor v jednom ze složek aplikace, protože nemá autoritu zápisu pro tuto složku.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení, sady Visual Studio oprávnění ke čtení v kořenové složce serveru a oprávnění k zápisu na aplikaci\_složku Data. Pokud vaše aplikace potřebuje oprávnění k zápisu do podsložky, jak je uvedené v nastavení oprávnění pro složky a zavedení do produkčního prostředí kurzy v této sérii můžete nastavit oprávnění pro tuto složku. Pokud vaše aplikace potřebuje oprávnění k zápisu do kořenové složky webu, je nutné zabránit v nastavení přístup jen pro čtení pro kořenovou složku tak, že přidáte **&lt;IncludeSetACLProviderOn cílové&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** souboru profilu publikování (Chcete-li mít vliv na jeden profil) nebo do souboru wpp.targets (Chcete-li mít vliv na všechny profily). Informace o tom, jak tyto soubory upravit, naleznete v tématu [postupy: Úprava nastavení nasazení profilu (.pubxml) soubory](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Chyba konfigurace – atribut targetFramework odkazuje na verzi, která je novější než nainstalovaná verze rozhraní .NET Framework

### <a name="scenario"></a>Scénář

Byla úspěšně publikována webový projekt, který cílí na technologii ASP.NET 4.5, ale při spuštění aplikace (s režimem customErrors nastavena na "off" v konfiguračním souboru Web.config) se zobrazí následující chyba:

Atribut "targetFramework" &lt;kompilace&gt; element v souboru Web.config se používá pouze pro cílovou verzi 4.0 a novější rozhraní .NET Framework (například "&lt;kompilace targetFramework ="4.0"&gt;'). Atribut 'targetFramework' momentálně odkazuje na verzi, která je novější než nainstalovaná verze rozhraní .NET Framework. Zadejte platnou cílovou verzi rozhraní .NET Framework nebo nainstalujte požadovanou verzi rozhraní .NET Framework.

Do pole chyby zdroje chybové stránky najdete následující řádek ze souboru Web.config jako Příčina chyby:

&lt;kompilace targetFramework = "4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Server nepodporuje technologii ASP.NET 4.5. Obraťte se na poskytovatele hostingu k určení, kdy a zda lze přidat podporu pro technologii ASP.NET 4.5. Pokud upgradujete server není možné zvolit, budete muset nasadit webový projekt, který cílí na ASP.NET 4 nebo dřívější místo.

Pokud provádíte nasazení ASP.NET 4 nebo dřívější webového projektu do stejného cíle, vyberte **odebrat další soubory v cílovém umístění** zaškrtávací políčko na **nastavení** karty **Publikovat Web**průvodce. Pokud nevyberete **odebrat další soubory v cílovém umístění**, budete i nadále stránka Chyba konfigurace.

Projekt **vlastnosti** windows zahrnuje cílového rozhraní framework rozevíracího seznamu, ale tento problém nelze vyřešit stačí, když změníte z **rozhraní .NET Framework 4.5** k **rozhraní .NET Framework 4**. Pokud změníte cílový rámec na starší verzi rozhraní framework projektu budou mít dál odkazy na novější verzi rozhraní framework, sestavení a nespustí. Budete muset ručně tyto odkazy změnit nebo vytvořit nový projekt, který cílí na .NET Framework 4 nebo dřívější. Další informace najdete v tématu [cílení rozhraní .NET pro weby](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Chyby na úrovni Medium Trust

### <a name="scenario"></a>Scénář

Při spuštění aplikace v produkčním prostředí získá chybu související na úrovni medium trust.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Mnoho poskytovatelé hostitelských služeb třetích stran spusťte svůj web v úrovni medium trust, což znamená, že existuje několik věcí, které není povoleno provádět. Například kód aplikace nemůže získat přístup k registru Windows a nejde přečíst nebo zapisovat soubory, které se nachází mimo hierarchii složek vaší aplikace. Ve výchozím nastavení vaše aplikace spuštěná *plně důvěryhodná* v místním počítači, což znamená, že aplikace může být schopen provádět operace, které selže při nasazování do produkčního prostředí.

Můžete nakonfigurovat aplikaci, aby běžela v úrovni medium trust v místním prostředí služby IIS k řešení potíží. Chcete-li to mohli udělat, otevřete aplikaci *Web.config* a přidejte **vztahu důvěryhodnosti** element v **system.web** elementu, jak je znázorněno v tomto příkladu.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Aplikace se teď spustí v úrovni medium trust ve službě IIS i na svém místním počítači.

Neprovádějte Pokud provádíte nasazení do služby Azure App Service, protože Azure nevyžaduje úrovni medium trust. V době, kdy probíhá zápis v tomto kurzu v únoru, 2012 aby vaše aplikace běžela v úrovni medium trust pomocí této metody způsobí chybu v Azure.

Pokud používáte migrace Entity Framework Code First a nasazujete do poskytovatele hostitelských služeb, na kterém běží aplikace v úrovni medium trust, ujistěte se, že máte verze 5.0 nebo novější. V rozhraní Entity Framework verze 4.3 Migrace vyžaduje úplný vztah důvěryhodnosti za účelem aktualizace schématu databáze.

## <a name="http-40417-not-found-error"></a>Nebyla nalezena chyba protokolu HTTP 404.17

### <a name="scenario"></a>Scénář

Při spuštění webu nasazené na vašem vývojovém počítači, ve službě IIS, se zobrazí následující chybová zpráva, že server nemůže zpracovat Default.aspx reporting:

Chyba protokolu HTTP 404.17 – Nenalezeno

Požadovaný obsah se zdá být skript a nebude obsluhuje statický soubor obslužnou rutinou.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

ASP.NET 4.5 nemusí být nainstalována v počítači. Podívejte se na postup v nasazení do služby IIS jako testovacího prostředí kurzu této série, která vysvětlují, jak nainstalovat technologii ASP.NET 4.5.

> [!div class="step-by-step"]
> [Předchozí](deploying-extra-files.md)
