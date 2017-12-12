---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: "Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nastavení oprávnění složky - 6 12 | Microsoft Docs"
author: tdykstra
description: "Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 42085fff5f1aed1440f49e1e2ceee0cf0e751e2c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Nasazení webové aplikace ASP.NET SQL Server Compact pomocí sady Visual Studio nebo Visual Web Developer: nastavení oprávnění složky - 6 12
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET projektu webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Visual Studio 2010 můžete také použít při instalaci aktualizace Publikovat Web. Úvod do řady, najdete v části [z prvního kurzu řady](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz, který ukazuje nasazení funkce zavedená po vydání sady Visual Studio 2012 RC, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, naleznete v části [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavíte oprávnění ke složce pro *Elmah* složky v nasazené webové lokality, aby aplikace můžete vytvářet soubory protokolu v této složce.

Při testování webové aplikace v sadě Visual Studio pomocí vývojového serveru Visual Studio (Cassini), aplikace bude spuštěna pod svou identitu. Jsou s největší pravděpodobností správce ve svém vývojovém počítači a mají úplná oprávnění udělat nic, aby všechny soubory v libovolné složky. Ale pokud je aplikace spuštěna v rámci služby IIS, běží pod identitou definované pro fond aplikací, který je přiřazen lokalitě. Toto je obvykle účet definovaná systémem, který má omezená oprávnění. Ve výchozím nastavení má ke čtení a oprávnění ke spouštění vaší webové aplikace souborů a složek, ale nemá oprávnění k zápisu.

Pokud vaše aplikace vytvoří nebo soubory aktualizací, které je společného potřebujete ve webových aplikacích, to všechno bude problém. V aplikaci Contoso univerzity Elmah vytvoří soubory XML ve *Elmah* složky a uložte informace o chybách. I když nepoužijete něco podobného jako Elmah, může váš web umožní uživatelům nahrání souborů nebo provádět další úlohy, které zapisovat data do složky ve vaší lokalitě.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testování Chyba protokolování a vytváření sestav

Pokud chcete zobrazit, jak aplikace nebude fungovat správně ve službě IIS (i když ho nebyla při jeho testování v sadě Visual Studio), může způsobit chybu, která by za normálních okolností protokolování Elmah a pak otevřete v protokolu chyb Elmah a zobrazit podrobnosti. Pokud Elmah se nepodařilo vytvořit soubor XML a ukládání podrobnosti o chybě, zobrazí zprávu o chybách prázdný.

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, a potom si vyžádají neplatná adresa URL jako *Studentsxxx.aspx*. Zobrazí stránka generována chyba místo *GenericErrorPage.aspx* stránky, protože `customErrors` nastavení v souboru Web.config je "RemoteOnly" a se službou IIS místně:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Teď spustit *Elmah.axd* zobrazíte zprávy o chybách. Zobrazí prázdný chybovou stránku protokolu, protože Elmah se nepodařilo vytvořit soubor XML v *Elmah* složky:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Nastavení oprávnění k zápisu do složky Elmah

Oprávnění složky můžete nastavit ručně nebo jej lze vytvořit automatické součást procesu nasazení. Díky tomu automatické vyžaduje komplexní MSBuild kódu a vzhledem k tomu, že máte jenom k tomu při prvním nasazení, v tomto kurzu pouze ukazuje, jak provést ručně. (Informace o tom, aby tuto část procesu nasazení najdete v tématu [nastavení oprávnění k publikování webu](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

V **Průzkumníka Windows**, přejděte na *C:\inetpub\wwwroot\ContosoUniversity*. Klikněte pravým tlačítkem myši *Elmah* složky, vyberte **vlastnosti**a pak vyberte **zabezpečení** kartě.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Pokud nevidíte **DefaultAppPool** v **skupiny nebo jméno uživatele** seznamu, pravděpodobně použijete jiné metody než verze zadaná v tomto kurzu nastavení služby IIS a ASP.NET 4 ve vašem počítači. V takovém případě zjistí, jaké identity používá fond aplikací, které jsou přiřazené k aplikaci univerzity Contoso a udělení oprávnění k zápisu pro danou identitu. V tématech o identity fondu aplikací na konci tohoto kurzu.)

Klikněte na tlačítko **upravit**. V **oprávnění pro Elmah** dialogové okno, vyberte **DefaultAppPool**a potom vyberte **zápisu** zaškrtávací políčko **povolit** sloupec.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Klikněte na tlačítko **OK** v obou dialogových oknech.

## <a name="retesting-error-logging-and-reporting"></a>Opětovném testování Chyba protokolování a vytváření sestav

Test způsobuje chybu znovu stejným způsobem (žádost o špatné adresy URL) a spusťte **protokolu chyb** stránky. Tentokrát chyba se zobrazí na stránce.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Můžete také potřebovat oprávnění k zápisu *aplikace\_Data* složky vzhledem k tomu, že máte systém SQL Server Compact soubory databáze v této složce a chcete mít možnost aktualizovat data v těchto databází. V takovém případě však můžete nemusíte provádět žádné další vzhledem k tomu, že proces nasazení automaticky nastaví oprávnění k zápisu na *aplikace\_Data* složky.

Teď jste dokončili všechny úlohy, které jsou potřebné k získání univerzity společnosti Contoso ve službě IIS správně funguje v místním počítači. V další kurz bude mít webu, které jsou veřejně dostupné po nasazení do hostujícího zprostředkovatele.

## <a name="more-information"></a>Další informace

V tomto příkladu se poměrně zřejmé důvod, proč nebylo možné uložit soubory protokolu Elmah. Služba IIS trasování můžete použít v případech, kdy je příčinou problém není tak zřejmé; v tématu [řešení potíží s požadavky pomocí trasování chybných ve službě IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) na webu IIS.net.

Další informace o tom, jak udělit oprávnění k identity fondu aplikací najdete v tématu [identity fondu aplikací součásti](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) a [zabezpečený obsah ve službě IIS prostřednictvím systému seznamy ACL v souborech](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) na webu IIS.net.

>[!div class="step-by-step"]
[Předchozí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
[další](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
