---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
title: 'Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nastavení oprávnění složky – 6 12 | Dokumentace Microsoftu'
author: tdykstra
description: Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET, která obsahuje databázi systému SQL Server Compact pomocí Visual samostatného projektu webové aplikace...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: cd03a188-e947-4f55-9bda-b8bce201d8c6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12
msc.type: authoredcontent
ms.openlocfilehash: 17910952e78418ef9e5b80efb04b5fe1e70c11bf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385308"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-setting-folder-permissions---6-of-12"></a>Nasazení webové aplikace ASP.NET s SQL serverem Compact pomocí sady Visual Studio nebo Visual Web Developer: nastavení oprávnění složky – 6 12
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Tato série kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace, která obsahuje databázi systému SQL Server Compact pomocí sady Visual Studio 2012 RC nebo Visual Studio Express 2012 RC pro Web. Můžete také použít Visual Studio 2010 při instalaci aktualizace Publikovat Web. Úvod do řady, naleznete v tématu [z prvního kurzu této série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Kurz ukazuje nasazení funkce zavedená po verzi RC sady Visual Studio 2012, ukazuje, jak nasadit edicích systému SQL Server než SQL Server Compact a ukazuje, jak nasadit do Azure App Service Web Apps, najdete v tématu [nasazení webu ASP.NET pomocí sady Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavíte oprávnění ke složce pro *Elmah* složky v nasazené webové lokality tak, aby aplikace můžete vytvořit soubory protokolu v této složce.

Při testování webové aplikace v sadě Visual Studio pomocí vývojového serveru Visual Studio (Cassini), aplikace bude spuštěna pod svou identitu. Jsou pravděpodobně správce na počítači pro vývoj a máte úplná oprávnění k ničemu k jakémukoli souboru v jakékoli složce. Ale pokud je aplikace spuštěna v rámci služby IIS, spuštění pod identitou definované pro fond aplikací, který je přiřazen lokalitě. Toto je obvykle účet definovaná systémem, který má omezená oprávnění. Ve výchozím nastavení má ke čtení a spouštěcích oprávnění k souborům a složkám webové aplikace, ale nemá oprávnění k zápisu.

To stává problémem, pokud se vytváří vaše aplikace nebo aktualizace souborů, což je běžný potřebujete ve webových aplikacích. V aplikaci Contoso University Elmah vytvoří soubory XML *Elmah* složky, aby bylo možné uložit podrobnosti o chybách. I v případě, že nepoužíváte něco jako Elmah, může váš web umožnit uživatelům nahrávat soubory nebo provádět další úlohy, které budou zapisovat data do složky na vašem webu.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="testing-error-logging-and-reporting"></a>Testování protokolování a vykazováním chyb

Pokud chcete zobrazit, jak aplikace nebude fungovat správně ve službě IIS (i když udělal při testování v sadě Visual Studio), může způsobit chybu, která by obvykle protokoluje pomocí knihovny Elmah a pak otevřete v protokolu chyb Elmah zobrazíte podrobnosti. Pokud Elmah se nepodařilo vytvořit soubor XML a ukládat podrobnosti o chybě, zobrazí zprávu o chybách prázdný.

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, a potom si vyžádají neplatnou adresu URL jako *Studentsxxx.aspx*. Zobrazí se stránka generována chyba místo *GenericErrorPage.aspx* stránce, protože `customErrors` nastavení v souboru Web.config je "RemoteOnly" a se službou IIS místně:

[![Error_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image2.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image1.png)

Nyní spusťte *Elmah.axd* zobrazíte zprávy o chybách. Vidíte prázdný chybovou stránku protokolu, protože se nepodařilo vytvořit soubor XML v Elmah *Elmah* složky:

[![Error_log_page_empty](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image4.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image3.png)

## <a name="setting-write-permission-on-the-elmah-folder"></a>Nastavení oprávnění pro zápis ve složce Elmah

Můžete nastavit oprávnění pro složky ručně nebo můžete si je automatickou součástí procesu nasazení. Díky tomu je automatické vyžaduje složitější kód MSBuild a vzhledem k tomu, že budete muset udělat při prvním nasazení, v tomto kurzu pouze ukazuje, jak to provést ručně. (Informace o tom, jak provést tuto část procesu nasazení najdete v tématu [nastavení oprávnění k publikování webu](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

V **Windows Explorer**, přejděte na *C:\inetpub\wwwroot\ContosoUniversity*. Klikněte pravým tlačítkem myši *Elmah* složky, vyberte **vlastnosti**a pak vyberte **zabezpečení** kartu.

[![Elmah_folder_Properties_Security_tab](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image6.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image5.png)

(Pokud se nezobrazí **DefaultAppPool** v **skupiny nebo jméno uživatele** seznamu, pravděpodobně používáte nějaké jiné metody než verze zadaná v tomto kurzu k nastavení služby IIS a ASP.NET 4 ve vašem počítači. V takovém případě zjistěte, jaké identita se používá fond aplikací přiřazené k aplikaci Contoso University a udělení oprávnění k zápisu do této identity. V tématech o identity fondu aplikací součásti na konci tohoto kurzu.)

Klikněte na tlačítko **upravit**. V **oprávnění pro Elmah** dialogu **DefaultAppPool**a pak vyberte **zápisu** zaškrtávací políčko **povolit** sloupce.

[![Permissions_for_Elmah_dialog_box](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image8.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image7.png)

Klikněte na tlačítko **OK** v obou polích dialogového okna.

## <a name="retesting-error-logging-and-reporting"></a>Opakované chyby protokolování a vytváření sestav

Otestujte způsobí chybu znovu stejným způsobem (žádost o špatné adresy URL) a spusťte **v protokolu chyb** stránky. Tentokrát na stránce se zobrazí chyba.

[![Elmah_Error_Log_page_Test](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image10.png)](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12/_static/image9.png)

Budete také potřebovat oprávnění k zápisu na *aplikace\_Data* složky vzhledem k tomu, že máte SQL Server Compact soubory databáze v této složce a chcete aktualizovat data v těchto databázích. V takovém případě však není nutné provádět žádné další vzhledem k tomu, že proces nasazení automaticky nastaví oprávnění k zápisu na *aplikace\_Data* složky.

Teď jste dokončili všechny úkoly, které jsou nezbytné pro uvedení University společnosti Contoso ve službě IIS správně funguje na místním počítači. V dalším kurzu kterou zpřístupníte webu veřejně nasazením k poskytovateli hostingu.

## <a name="more-information"></a>Další informace

V tomto příkladu se důvod, proč nebylo možné uložit soubory protokolu Elmah poměrně jasné. V případech, kde není to zřejmé; příčinu problému můžete použít trasování služby IIS Zobrazit [řešení potíží s požadavky pomocí trasování neúspěšných ve službě IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) na webu IIS.net.

Další informace o tom, jak udělit oprávnění k identity fondu aplikací najdete v tématu [identity fondu aplikací součásti](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) a [zabezpečený obsah ve službě IIS prostřednictvím systému seznamy ACL v souborech](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) na webu IIS.net.

> [!div class="step-by-step"]
> [Předchozí](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
> [další](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
