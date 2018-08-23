---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: řešení potíží s (12 12) | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2923501289f31243a7341848ed3f7c2142c98e75
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756921"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: řešení potíží s (12 12)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do modelu weby Windows Azure, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Tato stránka popisuje některé běžné problémy, které mohou nastat při nasazení webové aplikace ASP.NET pomocí sady Visual Studio. U každé z nich jeden nebo více možné příčiny a odpovídající řešení jsou k dispozici.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Chyba serveru v aplikaci – "/" aktuální vlastní nastavení chyb zabránit podrobností o chybě zobrazení vzdáleně

### <a name="scenario"></a>Scénář

Po nasazení webu na vzdáleného hostitele, zobrazí chybová zpráva, která uvádí nastavení customErrors v souboru Web.config, ale neukazuje, jak se skutečné příčinu chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve výchozím nastavení technologie ASP.NET zobrazuje podrobné informace o chybě pouze v případě, že vaše webová aplikace běží na místním počítači. Obecně nechcete zobrazit podrobné informace o chybě, když je webová aplikace veřejně dostupné přes Internet, protože hackery možné pomocí těchto informací můžete najít ohrožení zabezpečení v aplikaci. Ale při nasazování webu nebo aktualizace do lokality, někdy něco může pokazit a budete muset získat skutečné chybové zprávě.

Umožňuje aplikaci zobrazovat podrobné chybové zprávy, když je spuštěná na vzdáleného hostitele, upravte soubor Web.config pro nastavení `customErrors` režimu vypnuté, opětovné nasazení aplikace a spusťte aplikaci znovu:

1. Pokud má soubor Web.config aplikace `customErrors` prvek `system.web` prvku, změnit `mode` atribut "Off". Jinak přidejte `customErrors` prvek `system.web` element s `mode` atribut nastaven na "off", jak je znázorněno v následujícím příkladu:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Nasazení aplikace.
3. Spuštění aplikace a opakujte cokoli, co jste provedli dříve, která způsobila k dojít k chybě. Teď vidíte, co je skutečné chybové zprávě.
4. Po vyřešení chyby obnovení původní `customErrors` nastavení a znovu nasadit aplikaci.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Přístup byl odepřen na webové stránce, používá SQL Server Compact

### <a name="scenario"></a>Scénář

Po nasazení webu, který používá SQL Server Compact a spuštění stránky v nasazené lokality, který přistupuje k databázi, se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Účet NETWORK SERVICE na serveru musí být schopni číst SQL Compact služba nativní binární soubory, které jsou v *bin\amd64* nebo *bin\x86* složka, ale oprávnění pro čtení pro tyto složky. Sada oprávnění ke čtení pro SÍŤOVOU službu *bin* složku a zkontrolujte, zda rozšíření oprávnění na podsložky.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Nelze přečíst konfigurační soubor. z důvodu nedostatečných oprávnění

### <a name="scenario"></a>Scénář

Po kliknutí na Visual Studio tlačítko Publikovat k nasazení aplikace do služby IIS na místním počítači, publikování selže a **výstup** okno zobrazuje chybová zpráva podobná této:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chcete-li použít jedním kliknutím publikovat do služby IIS na místním počítači, musíte používat Visual Studio s oprávněními správce. Zavřete sadu Visual Studio a restartujte ji s oprávněními správce.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Nelze se připojit k cílovému počítači... Pomocí zadaného procesu

### <a name="scenario"></a>Scénář

Po kliknutí na Visual Studio tlačítko Publikovat k nasazení aplikace, publikování selže a **výstup** okno zobrazuje chybová zpráva podobná této:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Proxy server je přerušení komunikace s na cílový server. Pomocí ovládacích panelů Windows nebo v aplikaci Internet Explorer, vyberte **Možnosti Internetu** a vyberte **připojení** kartu. V **vlastnosti Internetu** dialogové okno, klikněte na tlačítko **nastavení místní sítě**. V **nastavení místní sítě (LAN)** dialogové okno, zrušte **automaticky zjišťovat nastavení** zaškrtávací políčko. Potom klikněte na tlačítko publikovat znovu.

Pokud se problém nevyřeší, obraťte se na správce systému k určení, co se dá dělat pomocí nastavení brány firewall nebo proxy serveru. Tento problém nastává vzhledem k tomu nestandardního portu nasazení webu používá pro nasazení služby webové správy (8172); pro další připojení nasazení webu používá port 80. Při nasazování do poskytovatele hostitelských služeb třetích stran, obvykle použijete služby webové správy.

## <a name="default-net-40-application-pool-does-not-exist"></a>Výchozí fond 4.0 aplikací rozhraní .NET neexistuje.

### <a name="scenario"></a>Scénář

Když nasadíte aplikaci, která vyžaduje rozhraní .NET Framework 4, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, který nasazujete na vývojovém počítači a na kterém je nainstalována aplikace Visual Studio 2010, technologii ASP.NET 4 je nainstalovaná na počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, na které nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Může také musíte ručně nastavit verzi rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu [nasazení do služby IIS jako testovacího prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Formát inicializačního řetězce neodpovídá specifikaci začínající na indexu 0.

### <a name="scenario"></a>Scénář

Poté, co nasadíte aplikaci jedním kliknutím pomocí publikování, když spustíte stránku, která přistupuje k databázi získáte následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Otevřít *Web.config* souboru v nasazené lokality a zkontrolujte, zda hodnoty připojovacího řetězce začínají `$(ReplacableToken_`, jako v následujícím příkladu:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Pokud připojovací řetězce vypadají jako v tomto příkladu, upravte soubor projektu a přidejte následující vlastnost, která má `PropertyGroup` element, který je pro všechny konfigurace sestavení:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Znovu nasaďte aplikaci.

## <a name="http-500-internal-server-error"></a>HTTP 500 Internal Server Error

### <a name="scenario"></a>Scénář

Při spuštění webu nasazené, zobrazí se následující chybová zpráva bez konkrétních informací určující příčinu chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Existuje mnoho příčiny 500 chyb, ale je jednou z možných příčin pokud postupujete tyto kurzy vložit XML element ve špatné místo v jednom ze souborů transformace XML. Například byste získali tuto chybu, když vložíte transformace, která vloží `<location>` element v rámci `<system.web>` místo přímo pod `<configuration>`. Řešením je v takovém případě opravte soubor transformace XML a znovu nasadit.

## <a name="http-50021-internal-server-error"></a>HTTP 500.21 se interní chyba serveru

### <a name="scenario"></a>Scénář

Při spuštění bude web se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Webu jste nasadili cíle ASP.NET 4, ale ASP.NET 4 není zaregistrovaný ve službě IIS na serveru. Na serveru otevřete příkazový řádek se zvýšenými oprávněními a zaregistrovat technologii ASP.NET 4 spuštěním následujících příkazů:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Může také musíte ručně nastavit verzi rozhraní .NET Framework výchozí fond aplikací. Další informace najdete v tématu [nasazení do služby IIS jako testovacího prostředí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) kurzu.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Přihlášení se nezdařilo otevření databáze SQL Server Express v aplikaci\_dat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na databázi SQL Server Express jako *.mdf* ve vaší *aplikace\_Data* složky a první Doba spuštění aplikace, které se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Název *.mdf* soubor nesmí shodovat s názvem databáze SQL Server Express, který stále existuje ve vašem počítači i v případě, že jste odstranili *.mdf* soubor dříve existující databáze. Změňte název *.mdf* soubor má název, který dosud nebyl použit jako název databáze a změňte *Web.config* soubor se má použít nový název. Jako alternativu můžete použít [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) odstranit dříve existující systém SQL Server Express databáze.

## <a name="model-compatibility-cannot-be-checked"></a>Model kompatibility nelze zaregistrovat

### <a name="scenario"></a>Scénář

Můžete aktualizovat *Web.config* souboru připojovací řetězec tak, aby odkazoval na nové databáze systému SQL Server Express a při prvním spuštění aplikace se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Pokud název databáze, kterou chcete vložit do souboru Web.config byla použita nikdy předtím, než v počítači, může být databáze již existuje v rámci některé tabulky v ní. Zvolte jiný název, který nebyl použit v počítači před a změnit *Web.config* souboru tak, aby odkazoval na tento nový název databáze používat. Jako alternativu můžete použít [Express nástroje SQL Server](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) nebo [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) k odstranění existující databáze.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>Chyba SQL, když se skript pokusí vytvořit uživatele nebo role

### <a name="scenario"></a>Scénář

Nasazení databáze nakonfigurované na použití **balení/publikování kódu SQL** kartu, skripty SQL, které při nasazení zahrnovat příkazy Create User nebo vytvořit roli a selhání spuštění skriptu při spuštění těchto příkazů. Může se zobrazit podrobné zprávy, jako je následující:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Pokud k této chybě dochází při konfiguraci nasazení databáze v **Publikovat Web** Průvodce místo **balení/publikování kódu SQL** kartu, vytvořit posloupnost v: [konfigurace a Nasazení](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) fóra a řešení se přidají do této stránce řešení potíží.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Uživatelský účet, který používáte k nasazení nemá oprávnění k vytváření uživatelů nebo rolí. Například může přiřadit hostovací společnost `db_datareader`, `db_datawriter`, a `db_ddladmin` role pro uživatelský účet, který nastaví kolekci za vás. Toto jsou dostatečná pro vytvoření Většina databázových objektů, ale ne pro vytváření uživatelů nebo rolí. Jedním ze způsobů, aby se zabránilo chybě je vyloučení uživatelé a role z nasazení databáze. Můžete to provést úpravou `PreSource` – element pro databáze automaticky vygenerovaný skript tak, že obsahují následující atributy:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Informace o tom, jak upravit `PreSource` element v souboru projektu naleznete v tématu [postupy: Úprava nastavení nasazení v souboru projektu](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Pokud uživatelé nebo role v databázi vývoj musí být v cílové databázi, obraťte se na svého poskytovatele hostingových služeb.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>Časový limit chyba systému SQL Server při spuštění vlastních skriptů při nasazení

### <a name="scenario"></a>Scénář

Jste zadali vlastní skripty SQL ke spuštění během nasazení a spuštění nasazení webu je, že časový limit.

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Spouštění více skriptů, které mají různé transakční režimy mohou způsobit chyby časového limitu. Ve výchozím nastavení automaticky generovaných skriptů spustit v transakci, ale nepodporují vlastní skripty. Pokud vyberete **o přijetí změn dat a/nebo schéma z existující databáze** možnost **balení/publikování kódu SQL** kartu, a pokud chcete přidat vlastní skripty SQL, je nutné změnit nastavení transakcí na některé skripty tak, aby všechny skripty používají stejné nastavení transakce. Další informace najdete v tématu [postupy: nasazení databáze se projekt webové aplikace](https://msdn.microsoft.com/library/dd465343.aspx).

Pokud jste nakonfigurovali nastavení transakcí tak, aby všechny jsou stejné, ale stále se tato chyba, možná alternativní řešení je na spouštění skriptů samostatně. V **databázové skripty** mřížky **balení/publikování** karta SQL, zrušte **zahrnout** zaškrtávací políčko pro skript, který způsobuje chybu časového limitu, poté projekt publikujte. Pak přejděte zpátky do **databázové skripty** mřížce, vyberte tento skript **zahrnout** zaškrtněte políčko a zrušte zaškrtnutí **zahrnout** zaškrtávací políčka pro jiné skripty. Potom znovu publikujte projekt. Tentokrát při publikování, jen vybrané vlastní skript se spustí.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream Data lokality manifestu ještě není k dispozici

### <a name="scenario"></a>Scénář

Při instalaci balíčku pomocí *deploy.cmd* souboru `t` (testovací) možnost se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Chybová zpráva znamená, že příkaz nelze vytvořit testovací sestavu. Však může spustit příkaz, pokud používáte `y` (vlastní instalace). Zpráva značí pouze, že je nějaký problém se spuštěním příkazu v režimu testu.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Tato aplikace vyžaduje ManagedRuntimeVersion v4.0

### <a name="scenario"></a>Scénář

Při pokusu o nasazení se zobrazí následující chybová zpráva:

 Chyba: Streamování dat z ' umístění sitemanifest/dbFullSql [@path= "C:\TEMP\AdventureWorksGrant.sql']/sqlScript" ještě není k dispozici. Fond aplikací, který se pokoušíte použít má vlastnost 'managedRuntimeVersion' nastavena na "v2.0". Tato aplikace vyžaduje "v4.0". 

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Ve službě IIS není nainstalována technologie ASP.NET 4. Pokud je server, který nasazujete na vývojovém počítači a na kterém je nainstalována aplikace Visual Studio 2010, technologii ASP.NET 4 je nainstalovaná na počítači, ale nemusí být nainstalován ve službě IIS. Na serveru, na které nasazujete otevřete příkazový řádek se zvýšenými oprávněními a spuštěním následujících příkazů nainstalujte technologii ASP.NET 4 ve službě IIS:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Nelze přetypovat Microsoft.Web.Deployment.DeploymentProviderOptions

### <a name="scenario"></a>Scénář

Když nasazujete balíček, zobrazí se následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Pokoušíte se nasazení ze Správce služby IIS pomocí uživatelského rozhraní služby webové nasazení 1.1 na serveru, který má Web Deploy 2.0 nainstalované. Pokud používáte nástroj služby IIS pro vzdálenou správu nasazení na základě importování balíčku, zaškrtněte **dostupné nové funkce** dialogové okno po navázání připojení. (Toto dialogové okno může být zobrazil jenom jednou při prvním vytvoření připojení. Vymazat připojení a začít znovu, zavřete Správce služby IIS a jeho opětovné zahájení tak, že zadáte `inetmgr /reset` příkazového řádku.) Pokud jedna z funkcí uvedených **uživatelského rozhraní webu nasadit**a má číslo verze nižší než 8, zatímco nasazujete na server může mít nasazení webu, nainstalované verze 1.1 a 2.0. K nasazení z klienta, který má 2.0 nainstalované, server musí mít pouze nasazení webu 2.0 nainstalované. Budete muset kontaktovat poskytovatele hostingu k vyřešení tohoto problému.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Nepovedlo se načíst nativní součásti systému SQL Server Compact

### <a name="scenario"></a>Scénář

Při spuštění bude web se zobrazí následující chybová zpráva:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Nasazený web nemá *amd64* a *x86* podsložky s nativní sestavení v nich v rámci aplikace *bin* složky. Na počítači, který má SQL Server Compact nainstalován nativní sestavení jsou umístěny v *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. K instalaci balíčku NuGet SqlServerCompact je nejlepší způsob, jak získat správný soubory do správné složky v projektu sady Visual Studio. Instalace balíčku přidává skriptu po sestavení zkopírovat nativní sestavení do *amd64* a *x86*. Aby tato nasazení je však nutné ručně zahrnout je do projektu. Další informace najdete v tématu [nasazení systému SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) kurzu.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Cesta není platná" Chyba po nasazení aplikace Entity Framework Code First

### <a name="scenario"></a>Scénář

Nasazení aplikace, který používá migrace Entity Framework Code First a DBMS, jako je například SQL Server Compact, která ukládá své databáze v souboru v aplikaci\_složku Data. Máte migrace Code First nakonfigurované k vytvoření databáze po prvním nasazení. Při spuštění aplikace zobrazí chybová zpráva jako v následujícím příkladu:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Kód nejprve se pokouší vytvořit databázi, ale aplikace\_Data složka neexistuje. Buď nebyly žádné soubory *aplikace\_Data* složky, pokud jste nasadili, nebo jste vybrali **vyloučit aplikace\_Data** na **Balení/publikováníwebu** karty **vlastnosti projektu** okna. Proces nasazení nebude vytvořte složku na serveru, pokud nejsou žádné soubory ve složce, které se mají zkopírovat na server. Pokud už máte databázi nastavit na webu, procesu nasazení odstraní soubory a *aplikace\_Data* samotnou Pokud vyberete složku **odebrat další soubory v cílovém umístění** v profil publikování. Chcete-li problém vyřešit, umístěte zástupný soubor například soubor .txt v *aplikace\_Data* složky, ujistěte se, že nemáte **vyloučit aplikace\_Data** vybrali a znovu nasadit. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"Nelze použít objekt COM, který byl oddělen od podkladové obálky RCW."

### <a name="scenario"></a>Scénář

Byli jste úspěšně jedním kliknutím pomocí publikování pro nasazení aplikace a pak spusťte zobrazuje tato chyba:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

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

Ve výchozím nastavení, sady Visual Studio oprávnění ke čtení v kořenové složce serveru a oprávnění k zápisu na aplikaci\_složku Data. Pokud vaše aplikace potřebuje oprávnění k zápisu do podsložky, oprávnění pro tuto složku můžete nastavit, jak je znázorněno [nastavení oprávnění pro složky](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) a [nasazení do produkčního prostředí](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) kurzy. Pokud vaše aplikace potřebuje oprávnění k zápisu do kořenové složky webu, je nutné zabránit v nastavení přístup jen pro čtení pro kořenovou složku tak, že přidáte **&lt;IncludeSetACLProviderOn cílové&gt;False&lt;/ IncludeSetACLProviderOnDestination&gt;** souboru profilu publikování (Chcete-li mít vliv na jeden profil) nebo do souboru wpp.targets (Chcete-li mít vliv na všechny profily). Informace o tom, jak tyto soubory upravit, naleznete v tématu [postupy: Úprava nastavení nasazení profilu (.pubxml) soubory](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Chyba konfigurace – atribut targetFramework odkazuje na verzi, která je novější než nainstalovaná verze rozhraní .NET Framework

### <a name="scenario"></a>Scénář

Byla úspěšně publikována webový projekt, který cílí na technologii ASP.NET 4.5, ale při spuštění aplikace (s `customErrors` režim nastaven na hodnotu "off" v souboru Web.config) zobrazí následující chyba:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Do pole chyby zdroje chybové stránky najdete následující řádek ze souboru Web.config jako Příčina chyby:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Možná příčina a řešení

Server nepodporuje technologii ASP.NET 4.5. Obraťte se na poskytovatele hostingu k určení, kdy a zda lze přidat podporu pro technologii ASP.NET 4.5. Pokud upgradujete server není možné zvolit, budete muset nasadit webový projekt, který cílí na ASP.NET 4 nebo dřívější místo. Pokud provádíte nasazení ASP.NET 4 nebo dřívější webového projektu do stejného cíle, vyberte **odebrat další soubory v cílovém umístění** zaškrtávací políčko na **nastavení** karty **Publikovat Web**průvodce. Pokud nevyberete **odebrat další soubory v cílovém umístění**, budete i nadále stránka Chyba konfigurace.

Projekt **vlastnosti** windows zahrnuje cílového rozhraní framework rozevíracího seznamu, ale tento problém nelze vyřešit stačí, když změníte z **rozhraní .NET Framework 4.5** k **rozhraní .NET Framework 4**. Pokud změníte cílový rámec na starší verzi rozhraní framework projektu budou mít dál odkazy na novější verzi rozhraní framework, sestavení a nespustí. Budete muset ručně tyto odkazy změnit nebo vytvořit nový projekt, který cílí na .NET Framework 4 nebo dřívější. Další informace najdete v tématu [cílení rozhraní .NET pro weby](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
