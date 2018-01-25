---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: řešení potíží (12 12) | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: d8c4931a1d26af49ee61c896897fa6ddf12fccea
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: řešení potíží (12 12)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit na weby systému Windows Azure, najdete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Tato stránka popisuje některé běžné problémy, které mohou nastat při nasazení webové aplikace ASP.NET pomocí sady Visual Studio. U každé z nich jsou uvedené možné příčiny a odpovídající řešení.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Chyba serveru v aplikaci - '/' aktuální vlastní nastavení chyb zabránit informace o chybě zobrazení vzdáleně

### <a name="scenario"></a>Scénář

Po nasazení se vzdáleným hostitelem lokality, obdržíte chybovou zprávu, která uvádí customErrors nastavení v souboru Web.config, ale neurčují, co byl skutečné příčinu chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení ASP.NET zobrazuje podrobné informace o chybě jenom v případě, že webová aplikace běží v místním počítači. Obecně nechcete zobrazit podrobné informace o chybě, pokud webová aplikace je veřejně dostupné přes Internet, protože hackery, pravděpodobně bude moci tyto informace slouží k vyhledání ohrožení zabezpečení v aplikaci. Ale při nasazování webu nebo aktualizací k lokalitě, někdy něco přejde nesprávný a potřebujete získat skutečné chybové zprávě.

Chcete-li aplikaci zobrazovat podrobné chybové zprávy, když je spuštěna u vzdáleného hostitele, upravte soubor Web.config nastavit `customErrors` režimu vypnuto, nasazení aplikace a spusťte aplikaci znovu:

1. Pokud má soubor Web.config aplikace `customErrors` element v `system.web` elementu, změny `mode` atribut na "off". V opačném případě přidat `customErrors` element v `system.web` element s `mode` atribut nastaven na "off", jak je znázorněno v následujícím příkladu:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Nasazení aplikace.
3. Spusťte aplikaci a opakujte, ať jste dříve způsobující vzniku problému. Nyní se zobrazí, co je skutečné chybové zprávě.
4. Až vyřešíte chybu, obnovit původní `customErrors` nastavení a znovu nasaďte aplikaci.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Přístup byl odepřen na webové stránce, používá SQL Server Compact

### <a name="scenario"></a>Scénář

Při nasazení webu, který používá systém SQL Server Compact a spuštění stránky v nasazené lokality, který přistupuje k databázi, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Účet NETWORK SERVICE na serveru musí být možné číst SQL Compact služba nativní binární soubory, které jsou v *bin\amd64* nebo *bin\x86* složka, ale nemá čtení oprávnění pro příslušné složky. Sada oprávnění ke čtení pro SÍŤOVOU službu na *bin* složku, a zkontrolujte, zda rozšíření oprávnění k podsložky.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nelze načíst konfigurační soubor z důvodu nedostatečných oprávnění.

### <a name="scenario"></a>Scénář

Po kliknutí na tlačítko sady Visual Studio tlačítko Publikovat k nasazení aplikace do služby IIS na místním počítači, publikování nezdaří a **výstup** okně se zobrazí chybová zpráva podobná této:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chcete-li použít jedním kliknutím publikování do služby IIS na místním počítači, musí být spuštěna Visual Studio s oprávněními správce. Zavřete Visual Studio a restartujte ji s oprávněními správce.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Může se připojit k cílovému počítači... Pomocí zadaného procesu

### <a name="scenario"></a>Scénář

Po kliknutí na tlačítko sady Visual Studio tlačítko Publikovat k nasazení aplikace, publikování nezdaří a **výstup** okně se zobrazí chybová zpráva podobná této:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Proxy server je přerušení komunikace s na cílový server. Z ovládacích panelů Windows nebo v aplikaci Internet Explorer, vyberte **Možnosti Internetu** a vyberte **připojení** kartě. V **vlastnosti Internetu** dialogové okno, klikněte na tlačítko **nastavení místní sítě**. V **nastavení místní sítě (LAN)** dialogové okno, zrušte **automaticky zjišťovat nastavení** zaškrtávací políčko. Klikněte na tlačítko publikovat znovu.

Pokud potíže potrvají, obraťte se na správce systému a zjistit, co můžete udělat pomocí nastavení proxy nebo brány firewall. Problém se stane, protože nasazení webu používá nestandardního portu pro nasazení služby webové správy (8172); pro další připojení Web Deploy používá port 80. Při nasazení do hostujícího zprostředkovatele třetí strany, obvykle použijete služby webové správy.

## <a name="default-net-40-application-pool-does-not-exist"></a>Výchozí fond 4.0 aplikací rozhraní .NET neexistuje.

### <a name="scenario"></a>Scénář

Když nasadíte aplikaci, která vyžaduje rozhraní .NET Framework 4, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, které nasazujete na vývojovém počítači a má na sobě nainstalovanou sadu Visual Studio 2010, technologii ASP.NET 4 je nainstalován v počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, který nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Může také musíte ručně nastavit verze rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu [nasazení do IIS jako testovacím prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formát inicializačního řetězce nevyhovuje specifikaci začínající na pozici 0.

### <a name="scenario"></a>Scénář

Poté, co nasadíte aplikaci pomocí jedním kliknutím publikujte, když spustíte stránky, který přistupuje k databázi získáte následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Otevřete *Web.config* souboru v nasazené lokality a zkontrolujte, zda hodnoty připojovacího řetězce začínat `$(ReplacableToken_`, jako v následujícím příkladu:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Pokud připojovací řetězce vypadat jako tento ukázkový, upravte soubor projektu a přidejte následující vlastnosti, která má `PropertyGroup` element, který je pro všechny konfigurace sestavení:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Pak znovu nasaďte aplikaci.

## <a name="http-500-internal-server-error"></a>HTTP 500 Internal Server Error

### <a name="scenario"></a>Scénář

Při spuštění bude web, zobrazí se následující chybová zpráva bez konkrétní informace o tom, příčinu chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Existuje mnoho příčiny 500 chyb, avšak možnou příčinou použijete-li tyto kurzy je uvést XML element do nesprávné místo v jednom transformace souborů XML. Například by se tato chyba, když vložíte transformace, která vloží `<location>` prvek v rámci `<system.web>` místo přímo pod `<configuration>`. Řešení je v takovém případě opravte transformaci souboru XML a znovu nasadit.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 Internal Server Error

### <a name="scenario"></a>Scénář

Při spuštění bude web, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Webu jste nasadili cíle ASP.NET 4, ale technologie ASP.NET 4 není registrovaný ve službě IIS na serveru. Na serveru otevřete příkazový řádek se zvýšenými oprávněními a zaregistrovat technologii ASP.NET 4 spuštěním následujících příkazů:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Může také musíte ručně nastavit verze rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu [nasazení do IIS jako testovacím prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Přihlášení se nezdařilo otevírání databáze SQL Server Express v aplikaci\_dat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na databázi SQL Server Express jako *.mdf* souboru v vaše *aplikace\_Data* složku a první čas spuštění aplikace, které se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Název *.mdf* souboru nesmí shodovat s názvem databáze SQL Server Express, které se někdy existovalo v počítači, i v případě, že jste odstranili *.mdf* soubor dříve existující databáze. Změňte název *.mdf* souboru na název, který dosud nebyl použit jako název databáze a změňte *Web.config* soubor k použití nového názvu. Jako alternativu, můžete použít [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) odstranit dříve existující systém SQL Server Express databáze.

## <a name="model-compatibility-cannot-be-checked"></a>Model kompatibility nelze zkontrolovat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na novou databázi SQL Server Express, a při prvním spuštění aplikace se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Pokud název databáze, které jste zadali v souboru Web.config byla použita nikdy předtím, než v počítači, možná již existuje databáze s některé tabulky v ní. Vyberte nový název, který nebyl použit v počítači před a změny *Web.config* souboru tak, aby odkazoval používat tento nový název databáze. Jako alternativu, můžete použít [SQL Server Express Utility](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) nebo [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) odstranit stávající databázi.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Chyba SQL, když se skript pokusí o vytvoření uživatelů nebo rolí

### <a name="scenario"></a>Scénář

Používáte-li nasazení databáze nakonfigurované na **balení/publikování kódu SQL** , skripty SQL, které se během nasazení spouští zahrnují vytvořit uživatele nebo vytvořit roli příkazy a selhání spuštění skriptu po provedení těchto příkazů. Zobrazí podrobnější zprávy, jako jsou následující:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Pokud k této chybě dojde, když jste nakonfigurovali nasazení databáze v **Publikovat Web** Průvodce místo **balení/publikování kódu SQL** kartě, vytvořit vlákno v [konfigurace a Nasazení](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) fórum a řešení přidá k této stránce řešení potíží.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Uživatelský účet, který používáte k provedení nasazení nemá oprávnění k vytvoření uživatele nebo role. Například by mohl přiřazovat hostingové společnosti `db_datareader`, `db_datawriter`, a `db_ddladmin` role pro uživatelský účet, který nastavuje za vás. Ty jsou dostatečné pro vytváření Většina databázových objektů, ale ne pro vytváření uživatelů nebo rolí. Jedním ze způsobů, aby se zabránilo chyba je vyloučení uživatelé a role z nasazení databáze. Můžete to provést úpravou `PreSource` element pro databázi automaticky generovaný skript tak, že obsahují následující atributy:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Informace o tom, jak upravit `PreSource` element v souboru projektu, najdete v části [postupy: Úprava nastavení nasazení v souboru projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Pokud uživatelé nebo role ve vaší databázi vývoj musí být v cílové databázi, obraťte se na svého poskytovatele hostingu.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Chyba časového limitu serveru SQL při spouštění vlastních skriptů při nasazení

### <a name="scenario"></a>Scénář

Jste zadali vlastní skripty SQL, pokud chcete spustit během nasazení a spuštění nástroje nasazení webu je, že vypršení časového limitu.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Spuštění více skriptů, které mají režimy jinou transakci může způsobit chyby vypršení časového limitu. Ve výchozím nastavení automaticky generovaných skriptů spustit v transakci, ale nepodporují vlastní skripty. Pokud jste vybrali **stáhnout data nebo schéma z existující databáze** možnost **balení/publikování kódu SQL** kartě, a pokud přidáte vlastní skript SQL, musíte změnit nastavení transakcí na některé skripty tak, aby všechny skripty používat stejné nastavení transakce. Další informace najdete v tématu [postupy: nasazení databáze s projekt webové aplikace](https://msdn.microsoft.com/library/dd465343.aspx).

Pokud jste nakonfigurovali nastavení transakce, tak, aby všechny byly stejné, ale stále se tato chyba zobrazí, možných řešení je spustit skripty odděleně. V **databázové skripty** mřížky ve **nasadit** kartě SQL, zrušte **zahrnout** zaškrtnutí políčka pro skript, který způsobuje chybu vypršení časového limitu, pak publikování tohoto projektu. Přejděte zpět do **databázové skripty** mřížky, vyberte tento skript **zahrnout** zaškrtněte políčko a zrušte výběr **zahrnout** zaškrtnutí políčka pro jiné skripty. Potom projekt znovu publikujte. Tentokrát při publikování, pouze vybrané vlastní skript se spustí.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Datový proud dat lokality manifestu dosud nejsou k dispozici

### <a name="scenario"></a>Scénář

Při instalaci balíčku pomocí *deploy.cmd* soubor s `t` (testovací) možnost, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chybová zpráva znamená, že příkaz nemůže vytvořit sestava testu. Však může spustit příkaz, pokud použijete `y` (vlastní instalace). Zpráva pouze znamená, že existuje problém se spuštěním příkazu v testovacím režimu.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Tato aplikace vyžaduje ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scénář

Při pokusu o nasazení, zobrazí se následující chybová zpráva:

 Chyba: Datový proud dat z ' umístění sitemanifest/dbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' ještě není k dispozici. Fond aplikací, který se pokoušíte použít má vlastnost 'managedRuntimeVersion' na hodnotu "v2.0". Tato aplikace vyžaduje 'v4.0'. 

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, které nasazujete na vývojovém počítači a má na sobě nainstalovanou sadu Visual Studio 2010, technologii ASP.NET 4 je nainstalován v počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, který nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nelze přetypovat Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scénář

Při nasazení balíčku, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Se pokoušíte nasadit ze Správce služby IIS pomocí webového nasazení 1.1 uživatelského rozhraní na server, který má Web Deploy 2.0 nainstalované. Pokud používáte nástroj pro vzdálenou správu služby IIS k nasazení importem balíčku, zkontrolujte **dostupné nové funkce** dialogové okno při vytvoření připojení. (Toto dialogové okno může být zobrazují pouze jednou při prvním vytváření připojení. Zrušte připojení a začít od začátku, zavřete Správce služby IIS a znovu jej spusťte tak, že zadáte `inetmgr /reset` na příkazovém řádku.) Pokud některou z funkcí uvedené **nasazení webového uživatelského rozhraní**a má číslo verze nižší než 8, na server, který nasazujete může mít nasazení webu nainstalovaná verze 1.1 a 2.0. K nasazení z klienta, který má nainstalovanou 2.0, server musí mít jenom nasazení webu 2.0 nainstalované. Budete muset obraťte se na svého poskytovatele hostingu k vyřešení problému.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nelze načíst nativní součásti systému SQL Server Compact

### <a name="scenario"></a>Scénář

Při spuštění bude web, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Bude web nemá *amd64* a *x86* podsložky nativní sestavení v nich v rámci aplikace *bin* složky. Na počítači, který má SQL Server Compact nainstalovaná, nativní sestavení jsou umístěné v *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Chcete-li nainstalovat balíček NuGet SqlServerCompact je nejlepší způsob, jak získat správné soubory do správné složky v projektu sady Visual Studio. Instalace balíčku Přidá skript po sestavení zkopírovat nativní sestavení do *amd64* a *x86*. V pořadí pro tato nasazení ale musíte ručně zahrnout je do projektu. Další informace najdete v tématu [nasazení systému SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) kurzu.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Cesta není platná" Chyba po nasazení aplikace Entity Framework Code First

### <a name="scenario"></a>Scénář

Nasazení aplikace, která používá migrace Entity Framework Code First a databázového systému, jako je například SQL Server Compact, která ukládá svou databázi v souboru v aplikaci\_složku Data. Máte migrace Code First nakonfigurované k vytvoření databáze po prvním nasazení. Při spuštění aplikace zobrazí chybová zpráva jako v následujícím příkladu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Kód nejprve se pokouší vytvořit databázi, ale aplikace\_dat složka neexistuje. Buď nebyly žádné soubory *aplikace\_Data* složky, pokud jste nasadili, nebo jste vybrali **vyloučit aplikace\_Data** na **balení/publikování webu** kartě **vlastnosti projektu** okno. Proces nasazení nebude vytvořte složku na serveru, pokud nejsou žádné soubory ve složce, který se má zkopírovat na server. Pokud jste již měli databázi nastavit v lokalitě, proces nasazení odstraní soubory a *aplikace\_Data* samotné Pokud jste vybrali složce **odebrat další soubory v cíli** v profil publikování. Problém vyřešit, umístit soubor zástupný text jako soubor .txt na *aplikace\_Data* složky, ujistěte se, že nemáte **vyloučit aplikace\_Data** vybrána a znovu nasadit. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nelze použít objekt COM, který má rozdělené z jeho základní RCW."

### <a name="scenario"></a>Scénář

Byly úspěšně jedním kliknutím publikování pro nasazení aplikace a pak spusťte získávání této chybě:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Zavření a spustit sadu Visual Studio je obvykle všechno, co je potřeba tuto chybu vyřešit.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Nasazení selže protože uživatele pověření použít pro nemají publikování setACL autority

### <a name="scenario"></a>Scénář

Publikování selže s chybou, že jste označuje nemáte oprávnění k nastavení oprávnění složky (uživatelský účet, který používáte nemá setACL autorita).

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení, sady Visual Studio oprávnění ke čtení v kořenové složce serveru a oprávnění k zápisu na aplikaci\_složku Data. Pokud víte, že výchozí oprávnění na webu složky jsou správné a není potřeba nastavit, že zakážete toto chování přidáním  **&lt;IncludeSetACLProviderOn cílové&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  do souboru profilu publikování (Chcete-li mít vliv na jeden profil), nebo na soubor wpp.targets (Chcete-li mít vliv na všechny profily). Informace o tom, jak upravit tyto soubory najdete v tématu [postupy: Úprava nastavení nasazení profilu (.pubxml) soubory](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Když se aplikace pokusí o zápis do složky aplikace chyby odepření přístupu

### <a name="scenario"></a>Scénář

Vaše aplikace chyby při pokusu o vytvoření nebo úprava soubor v jednom ze složky aplikací, protože nemá autoritu zápisu pro tuto složku.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení, sady Visual Studio oprávnění ke čtení v kořenové složce serveru a oprávnění k zápisu na aplikaci\_složku Data. Pokud aplikace potřebuje přístup k zápisu do podsložky, můžete nastavit oprávnění pro tuto složku, jak je znázorněno [nastavení oprávnění ke složkám](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) a [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzy. Pokud aplikace potřebuje přístup k zápisu do kořenové složky webu, budete muset zabránit jeho nastavení jen pro čtení v kořenové složce přidáním  **&lt;IncludeSetACLProviderOn cílové&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;**  do souboru profilu publikování (Chcete-li mít vliv na jeden profil), nebo na soubor wpp.targets (Chcete-li mít vliv na všechny profily). Informace o tom, jak upravit tyto soubory najdete v tématu [postupy: Úprava nastavení nasazení profilu (.pubxml) soubory](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Chyba konfigurace - atribut targetFramework odkazuje na verzi, která je novější než nainstalovaná verze rozhraní .NET Framework

### <a name="scenario"></a>Scénář

Úspěšně jste publikovali webový projekt, který cílí technologie ASP.NET 4.5, ale při spuštění aplikace (s `customErrors` režim nastaven na hodnotu "off" v souboru Web.config) zobrazí následující chyba:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Pole Chyba zdroje chybové stránky jsou následující řádek ze souboru Web.config jako příčinu chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Server nepodporuje technologii ASP.NET 4.5. Obraťte se na poskytovatele hostingu k určení, kdy a podporu pro technologii ASP.NET 4.5 mohou být přidány. Pokud se upgrade serveru není možné, je nutné nasadit webový projekt, který cílí ASP.NET 4 nebo dřívější místo. Pokud nasadíte na technologii ASP.NET 4 nebo dřívější webový projekt se stejným cílem, vyberte **odebrat další soubory v cílovém umístění** v zaškrtávací políčko **nastavení** kartě **Publikovat Web**průvodce. Pokud nevyberete **odebrat další soubory v cílovém umístění**, nadále bude mít konfigurace chybovou stránku.

Projekt **vlastnosti** systém windows obsahuje cílový framework rozevírací seznam, ale tento problém nelze vyřešit změnou právě, z **rozhraní .NET Framework 4.5** k **rozhraní .NET Framework 4**. Pokud je na starší verzi framework změnit cílový framework, budou mít dál odkazy na sestavení novější verze framework projektu a nespustí. Budete muset ručně změnit tyto odkazy nebo vytvořte nový projekt, který cílí rozhraní .NET Framework 4 nebo dřívější. Další informace najdete v tématu [rozhraní .NET Framework cílení pro weby](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

>[!div class="step-by-step"]
[Předchozí](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
