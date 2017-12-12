---
uid: web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
title: "Nasazení webu ASP.NET pomocí sady Visual Studio: nastavení oprávnění ke složkám | Microsoft Docs"
author: tdykstra
description: "Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo do hostujícího zprostředkovatele třetí strany podle usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9715a121-fa55-4f1b-a5d2-fb3f6cd8be8f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/setting-folder-permissions
msc.type: authoredcontent
ms.openlocfilehash: 19bef5ff97fd5b79135df8ca9bd6bd316594cc5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-setting-folder-permissions"></a>Nasazení webu ASP.NET pomocí sady Visual Studio: nastavení oprávnění ke složkám
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si úvodní projekt](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Tato řada kurzu se dozvíte, jak nasadit (publikovat) technologie ASP.NET webové aplikace do Azure App Service Web Apps nebo k poskytovateli hostingu třetích stran, pomocí sady Visual Studio 2012 nebo Visual Studio 2010. Informace o řadě najdete v tématu [z prvního kurzu řady](introduction.md).


## <a name="overview"></a>Přehled

V tomto kurzu nastavíte oprávnění ke složce pro *Elmah* složky v nasazené webové lokality, aby aplikace můžete vytvářet soubory protokolu v této složce.

Při testování webové aplikace v sadě Visual Studio pomocí vývojového serveru Visual Studio (Cassini) nebo IIS Express, aplikace bude spuštěna pod svou identitu. Jsou s největší pravděpodobností správce ve svém vývojovém počítači a mají úplná oprávnění udělat nic, aby všechny soubory v libovolné složky. Ale pokud je aplikace spuštěna v rámci služby IIS, běží pod identitou definované pro fond aplikací, který je přiřazen lokalitě. Toto je obvykle účet definovaná systémem, který má omezená oprávnění. Ve výchozím nastavení má ke čtení a oprávnění ke spouštění vaší webové aplikace souborů a složek, ale nemá oprávnění k zápisu.

Pokud vaše aplikace vytvoří nebo soubory aktualizací, které je společného potřebujete ve webových aplikacích, to všechno bude problém. V aplikaci Contoso univerzity Elmah vytvoří soubory XML ve *Elmah* složky a uložte informace o chybách. I když nepoužijete něco podobného jako Elmah, může váš web umožní uživatelům nahrání souborů nebo provádět další úlohy, které zapisovat data do složky ve vaší lokalitě.

Upozornění: Pokud se zobrazí chybové hlášení, nebo něco nefunguje tak, jak absolvovat kurz, nezapomeňte se podívat [řešení potíží s stránky](troubleshooting.md).

## <a name="test-error-logging-and-reporting"></a>Test protokolování chyb a vytváření sestav

Pokud chcete zobrazit, jak aplikace nebude fungovat správně ve službě IIS (i když ho nebyla při jeho testování v sadě Visual Studio), může způsobit chybu, která by za normálních okolností protokolování Elmah a pak otevřete v protokolu chyb Elmah a zobrazit podrobnosti. Pokud Elmah se nepodařilo vytvořit soubor XML a ukládání podrobnosti o chybě, zobrazí zprávu o chybách prázdný.

Otevřete prohlížeč a přejděte na `http://localhost/ContosoUniversity`, a potom si vyžádají neplatná adresa URL jako *Studentsxxx.aspx*. Zobrazí stránka generována chyba místo *GenericErrorPage.aspx* stránky, protože `customErrors` nastavení v souboru Web.config je "RemoteOnly" a se službou IIS místně:

![HTTP 404 chybová stránka](setting-folder-permissions/_static/image1.png)

Teď spustit *Elmah.axd* zobrazíte zprávy o chybách. Po přihlášení pomocí přihlašovacích údajů účtu správce (&quot;správce&quot; a &quot;devpwd&quot;), zobrazí prázdný chybovou stránku protokolu, protože Elmah se nepodařilo vytvořit soubor XML v *Elmah*složky:

![Chyba protokolu prázdný](setting-folder-permissions/_static/image2.png)

## <a name="set-write-permission-on-the-elmah-folder"></a>Nastavit oprávnění k zápisu ve složce Elmah

Oprávnění složky můžete nastavit ručně nebo jej lze vytvořit automatické součást procesu nasazení. Díky tomu automatické vyžaduje složitý kód MSBuild a vzhledem k tomu, že máte pouze k tomu poprvé, který nasazujete, následující kroky postup provést ručně. (Informace o tom, aby tuto část procesu nasazení najdete v tématu [nastavení oprávnění k publikování webu](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) na blogu Sayed Hashimi.)

1. V **Průzkumníka souborů**, přejděte na *C:\inetpub\wwwroot\ContosoUniversity*. Klikněte pravým tlačítkem myši *Elmah* složky, vyberte **vlastnosti**a pak vyberte **zabezpečení** kartě.
2. Klikněte na tlačítko **upravit**.
3. V **oprávnění pro Elmah** dialogové okno, vyberte **DefaultAppPool**a potom vyberte **zápisu** zaškrtávací políčko **povolit** sloupec.

    ![Oprávnění pro složku ELMAH](setting-folder-permissions/_static/image3.png)

    (Pokud nevidíte **DefaultAppPool** v **skupiny nebo jméno uživatele** seznamu, pravděpodobně použijete jiné metody než verze zadaná v tomto kurzu nastavení služby IIS a ASP.NET 4 ve vašem počítači. V takovém případě zjistí, jaké identity používá fond aplikací, které jsou přiřazené k aplikaci univerzity Contoso a udělení oprávnění k zápisu pro danou identitu. V tématech o identity fondu aplikací na konci tohoto kurzu.) Klikněte na tlačítko **OK** v obou dialogových oknech.

## <a name="retest-error-logging-and-reporting"></a>Testování protokolování chyb a vytváření sestav

Test způsobuje chybu znovu stejným způsobem (žádost o špatné adresy URL) a spusťte **protokolu chyb** stránky. Tentokrát chyba se zobrazí na stránce.

![ELMAH Chyba protokolu stránky](setting-folder-permissions/_static/image4.png)

## <a name="summary"></a>Souhrn

Teď jste dokončili všechny úlohy, které jsou potřebné k získání univerzity společnosti Contoso ve službě IIS správně funguje v místním počítači. V dalším kurzu bude zkontrolujte lokality, které jsou veřejně dostupné po nasazení do Azure.

## <a name="more-information"></a>Další informace

V tomto příkladu se poměrně zřejmé důvod, proč nebylo možné uložit soubory protokolu Elmah. Služba IIS trasování můžete použít v případech, kdy je příčinou problém není tak zřejmé; v tématu [řešení potíží s požadavky pomocí trasování chybných ve službě IIS 7](https://www.iis.net/learn/troubleshoot/using-failed-request-tracing/troubleshooting-failed-requests-using-tracing-in-iis) na webu IIS.net.

Další informace o tom, jak udělit oprávnění k identity fondu aplikací najdete v tématu [identity fondu aplikací součásti](https://www.iis.net/learn/manage/configuring-security/application-pool-identities) a [zabezpečený obsah ve službě IIS prostřednictvím systému seznamy ACL v souborech](https://www.iis.net/learn/get-started/planning-for-security/secure-content-in-iis-through-file-system-acls) na webu IIS.net.

>[!div class="step-by-step"]
[Předchozí](deploying-to-iis.md)
[další](deploying-to-production.md)
