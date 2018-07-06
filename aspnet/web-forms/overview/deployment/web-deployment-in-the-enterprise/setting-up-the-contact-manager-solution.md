---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Nastavení řešení Správce kontaktů | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak stáhnout a nakonfigurovat řešení Správce kontaktů spouštět místně na pracovní stanici vývojáře.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 479dbb8d2edbe9fb953ea9e1312ffb8fdbd3e2fe
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802128"
---
<a name="setting-up-the-contact-manager-solution"></a>Nastavení řešení Správce kontaktů
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak stáhnout a nakonfigurovat řešení Správce kontaktů spouštět místně na pracovní stanici vývojáře.


## <a name="system-requirements"></a>Požadavky na systém

Místní spuštění řešení Správce kontaktů a provádění dalších úloh popsané v tomto kurzu, budete potřebovat k instalaci tohoto softwaru na pracovní stanici pro vývojáře:

- Edici Ultimate, Premium nebo Visual Studio 2010 Service Pack 1
- Internetová informační služba (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Služba IIS nástroj pro nasazení webu (nasazení webu) 2.1 nebo novější
- TECHNOLOGIE ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

S výjimkou Visual Studio 2010 můžete stáhnout a nainstalovat nejnovější verze všech těchto produktů a součástí prostřednictvím [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Stažení a extrakci řešení

Správce kontaktů ukázkovou aplikaci si můžete stáhnout z galerie kódů MSDN [tady](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurace a spuštění řešení

Pro konfiguraci a spuštění řešení Správce kontaktů na místním počítači, bude nutné k provedení těchto kroků:

1. Pokud je nemáte, vytvořte místní databáze aplikačních služeb ASP.NET s funkce správy členství a rolí povolena.
2. Upravit připojovací řetězce v *web.config* soubory tak, aby odkazoval na místní instanci systému SQL Server Express.
3. Spuštění řešení sady Visual Studio 2010.

Zbytek tohoto oddílu poskytuje ještě s něčím poradit o provedení těchto úloh.

**Chcete-li vytvořit databázi služeb aplikací**

1. Otevřete příkazový řádek sady Visual Studio 2010. K tomu, na **Start** nabídky, přejděte k **všechny programy**, klikněte na tlačítko **sadu Microsoft Visual Studio 2010**, klikněte na tlačítko **Visual Studio Tools**a pak Klikněte na tlačítko **příkazový řádek sady Visual Studio (2010)**.
2. Na příkazovém řádku zadejte následující příkaz a stiskněte klávesu Enter:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Použití **– C** přepínač tak, aby zadejte připojovací řetězec pro databázový server.
    2. Použití **–** přepínač k určení funkce, které chcete přidat do databáze služeb aplikací. V takovém případě **m** označuje, že chcete přidat podporu pro zprostředkovatele členství a **r** označuje, že chcete přidat podporu pro správce rolí.
    3. Použití **– d** přepínač tak, aby zadejte název databáze aplikace služby. Vynecháte-li tento přepínač, nástroj vytvoří databázi s výchozí název **aspnetdb**.
3. Když databáze byla úspěšně vytvořena, příkazového řádku se zobrazí potvrzení.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Další informace o aspnet\_regsql nástroj, najdete v článku [nástroj pro registraci serveru SQL technologie ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Abyste měli jistotu, že připojovací řetězce v řešení Správce kontaktů přejděte k vaší místní instanci systému SQL Server Express je dalším krokem.

**Chcete-li aktualizovat připojovací řetězce**

1. Otevřete řešení Správce kontaktů v sadě Visual Studio 2010.
2. V **Průzkumníka řešení** okna, rozbalte **ContactManager.Mvc** projektu a potom dvakrát klikněte **Web.config** uzlu.

    > [!NOTE]
    > Projekt ContactManager.Mvc zahrnuje dva *web.config* soubory. Je potřeba upravit soubor projektu.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. V **connectionStrings** elementu, ověřte, že připojovací řetězec s názvem **ApplicationServices** odkazuje na vaše místní databáze aplikačních služeb technologie ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. V **Průzkumníka řešení** okna, rozbalte **ContactManager.Service** projektu a potom dvakrát klikněte **Web.config** uzlu.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. V **connectionStrings** prvek v řetězci připojení s názvem **ContactManagerContext**, ověřte, že **zdroj dat** je nastavena na vaše místní instanci serveru SQL Server Express. Není potřeba nic jiného v připojovacím řetězci změnit.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Uložte všechny otevřené soubory.

Teď byste měli být připraveni spustit řešení Správce kontaktů na místním počítači.

> [!NOTE]
> Pokud budete postupovat podle těchto kroků bez vytvoření první databáze aplikace služby ASP.NET databáze se vytvoří při prvním pokusu o vytvoření uživatele. Však ručně vytvořit databázi, nabízí mnohem větší kontrolu nad sadě funkcí služby aplikace, které chcete podporovat.


**Chcete-li spustit řešení Správce kontaktů**

1. V sadě Visual Studio 2010 stiskněte klávesu F5.
2. Aplikace Internet Explorer spuštění a vyžaduje adresu URL aplikace, kontaktujte správce ASP.NET MVC 3. Ve výchozím nastavení, zobrazí aplikace **poslat všem kontaktům** stránky.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Přidejte několik kontaktů a potom ověřte, že aplikace funguje podle očekávání.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Přejděte do `http://localhost:50114/Account/Register` (Pokud máte aplikace na jiném portu upravte adresu URL). Přidejte uživatelské jméno, e-mailovou adresu a heslo a ověřte, že budete moct zaregistrovat účet úspěšně.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Přejděte do `http://localhost:50114/Account/LogOn` (Pokud máte aplikace na jiném portu upravte adresu URL). Ověřte, že budete moct přihlásit pomocí účtu, který jste právě vytvořili.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Zavřete aplikaci Internet Explorer chcete zastavit ladění.

## <a name="conclusion"></a>Závěr

V tomto okamžiku řešení Správce kontaktů musí být plně konfigurovaná spustit na místním počítači. Řešení můžete použít jako referenci při práci prostřednictvím dalších témat v tomto kurzu.

Dalším tématu s názvem [vysvětlení souboru projektu](understanding-the-project-file.md), vysvětluje, jak můžete použít vlastní soubory projektu Microsoft Build Engine (MSBuild) v rámci řešení Správce kontaktů k řízení procesu nasazení.

> [!div class="step-by-step"]
> [Předchozí](the-contact-manager-solution.md)
> [další](understanding-the-project-file.md)
