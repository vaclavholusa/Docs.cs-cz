---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: 'Nasazení webu ASP.NET pomocí sady Visual Studio: nastavení oprávnění pro složky | Dokumentace Microsoftu'
author: tdykstra
description: V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, podle usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 930f46c0ddb0b77525098291393e526107a542d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752417"
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nastavení oprávnění ke složkám
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout počáteční projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> V této sérii kurzů se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, s použitím sady Visual Studio 2012 nebo Visual Studio 2010. Informace o této sérii, naleznete v tématu [z prvního kurzu této série](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavíte oprávnění ke složce pro *Elmah* složky v nasazené webové lokality tak, aby aplikace můžete vytvořit soubory protokolu v této složce.

Při testování webové aplikace v sadě Visual Studio pomocí služby IIS Express nebo vývojový Server sady Visual Studio (Cassini), aplikace bude spuštěna pod svou identitu. Jsou pravděpodobně správce na počítači pro vývoj a máte úplná oprávnění k ničemu k jakémukoli souboru v jakékoli složce. Ale pokud je aplikace spuštěna v rámci služby IIS, spuštění pod identitou definované pro fond aplikací, který je přiřazen lokalitě. Toto je obvykle účet definovaná systémem, který má omezená oprávnění. Ve výchozím nastavení má ke čtení a spouštěcích oprávnění k souborům a složkám webové aplikace, ale nemá oprávnění k zápisu.

To stává problémem, pokud se vytváří vaše aplikace nebo aktualizace souborů, což je běžný potřebujete ve webových aplikacích. V aplikaci Contoso University Elmah vytvoří soubory XML *Elmah* složky, aby bylo možné uložit podrobnosti o chybách. I v případě, že nepoužíváte něco jako Elmah, může váš web umožnit uživatelům nahrávat soubory nebo provádět další úlohy, které budou zapisovat data do složky na vašem webu.

Připomenutí: Pokud se zobrazí chybová zpráva nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [stránka o řešení problémů](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test protokolování chyb a vytváření sestav

Pokud chcete zobrazit, jak aplikace nebude fungovat správně ve službě IIS (i když udělal při testování v sadě Visual Studio), může způsobit chybu, která by obvykle protokoluje pomocí knihovny Elmah a pak otevřete v protokolu chyb Elmah zobrazíte podrobnosti. Pokud Elmah se nepodařilo vytvořit soubor XML a ukládat podrobnosti o chybě, zobrazí zprávu o chybách prázdný.

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, a potom si vyžádají neplatnou adresu URL jako *Studentsxxx.aspx*. Zobrazí se stránka generována chyba místo *GenericErrorPage.aspx* stránce, protože `customErrors` nastavení v souboru Web.config je "RemoteOnly" a se službou IIS místně:

![HTTP 404 chybovou stránku](setting-folder-permissions/_static/image1.png)

Nyní spusťte *Elmah.axd* zobrazíte zprávy o chybách. Po přihlášení pomocí přihlašovacích údajů správce účtu (&quot;správce&quot; a &quot;devpwd&quot;), uvidíte prázdné chybovou stránku protokolu, protože Elmah se nepodařilo vytvořit soubor XML v *Elmah*složky:

![Chyba protokolu prázdný](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Nastavit oprávnění k zápisu pro složku Elmah

Můžete nastavit oprávnění pro složky ručně nebo můžete si je automatickou součástí procesu nasazení. Díky tomu je automatické vyžaduje složitější kód MSBuild a vzhledem k tomu stačí k tomu při prvním nasazení, následující kroky tom, jak to provést ručně. (Informace o tom, jak provést tuto část procesu nasazení najdete v tématu [nastavení oprávnění k publikování webu](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

1. V **Průzkumníka souborů**, přejděte na *C:\inetpub\wwwroot\ContosoUniversity*. Klikněte pravým tlačítkem myši *Elmah* složky, vyberte **vlastnosti**a pak vyberte **zabezpečení** kartu.
2. Klikněte na tlačítko **upravit**.
3. V **oprávnění pro Elmah** dialogu **DefaultAppPool**a pak vyberte **zápisu** zaškrtávací políčko **povolit** sloupce.

    ![Oprávnění pro složku ELMAH](setting-folder-permissions/_static/image3.png)

    (Pokud se nezobrazí **DefaultAppPool** v **skupiny nebo jméno uživatele** seznamu, pravděpodobně používáte nějaké jiné metody než verze zadaná v tomto kurzu k nastavení služby IIS a ASP.NET 4 ve vašem počítači. V takovém případě zjistěte, jaké identita se používá fond aplikací přiřazené k aplikaci Contoso University a udělení oprávnění k zápisu do této identity. V tématech o identity fondu aplikací součásti na konci tohoto kurzu.) Klikněte na tlačítko **OK** v obou polích dialogového okna.

## <a name="retest-error-logging-and-reporting"></a>Opětovné testování protokolování chyb a vytváření sestav

Otestujte způsobí chybu znovu stejným způsobem (žádost o špatné adresy URL) a spusťte **v protokolu chyb** stránky. Tentokrát na stránce se zobrazí chyba.

![ELMAH, chybovou stránku protokolu](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Souhrn

Teď jste dokončili všechny úkoly, které jsou nezbytné pro uvedení University společnosti Contoso ve službě IIS správně funguje na místním počítači. V dalším kurzu vytvoříte web veřejně dostupné po nasazení do Azure.

## <a name="more-information"></a>Další informace

V tomto příkladu se důvod, proč nebylo možné uložit soubory protokolu Elmah poměrně jasné. V případech, kde není to zřejmé; příčinu problému můžete použít trasování služby IIS Zobrazit [řešení potíží s požadavky pomocí trasování neúspěšných ve službě IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) na webu IIS.net.

Další informace o tom, jak udělit oprávnění k identity fondu aplikací najdete v tématu [identity fondu aplikací součásti](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) a [zabezpečený obsah ve službě IIS prostřednictvím systému seznamy ACL v souborech](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) na webu IIS.net.

> [!div class="step-by-step"]
> [Předchozí](deploying-to-iis.md)
> [další](deploying-to-production.md)
