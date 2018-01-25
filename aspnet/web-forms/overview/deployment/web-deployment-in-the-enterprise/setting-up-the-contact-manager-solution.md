---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: "Nastavení řešení obraťte se na správce | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak stáhnout a nakonfigurovat spustit místně na pracovní stanici vývojáře řešení obraťte se na správce."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: b8176b3b8622e21187a91647323322e55582373c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Nastavení řešení obraťte se na správce
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak stáhnout a nakonfigurovat spustit místně na pracovní stanici vývojáře řešení obraťte se na správce.


## <a name="system-requirements"></a>Požadavky na systém

Chcete-li spustit místně řešení obraťte se na správce a provádět další úlohy popsané v tomto kurzu, budete potřebovat k instalaci tohoto softwaru na pracovní stanici developer:

- Visual Studio 2010 Service Pack 1, Premium nebo Ultimate Edition
- Internetová informační služba (IIS) 7.5 Express
- SQL Server Express 2008 R2
- Služba IIS nástroj pro nasazení webu (Web Deploy) 2.1 nebo novější
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

S výjimkou Visual Studio 2010 můžete stáhnout a nainstalovat nejnovější verze všech těchto produktů a součástí prostřednictvím [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Stažení a extrakci řešení

Obraťte se na správce ukázkovou aplikaci si můžete stáhnout z galerie kódů MSDN [zde](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurace a spuštění řešení

Pro konfiguraci a spuštění řešení obraťte se na správce na místním počítači, budete potřebovat k provedení těchto kroků:

1. Pokud nemáte již, vytvořte místní databáze služby aplikace ASP.NET pomocí funkce správy členství a rolí povolené.
2. Upravování připojovacích řetězců v *web.config* soubory tak, aby odkazoval na místní instanci systému SQL Server Express.
3. Spuštění řešení ze sady Visual Studio 2010.

Zbývající část Tato část obsahuje další pokyny k dokončení těchto postupů.

**K vytvoření databáze aplikace služby**

1. Otevřete příkazový řádek sady Visual Studio 2010. To uděláte, na **spustit** nabídky, přejděte na příkaz **všechny programy**, klikněte na tlačítko **Microsoft Visual Studio 2010**, klikněte na tlačítko **nástroje sady Visual Studio**a potom Klikněte na tlačítko **Visual Studio – příkazový řádek (2010)**.
2. Na příkazovém řádku zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Použití **– C** přepínač tak, aby zadejte připojovací řetězec pro databázový server.
    2. Použití **– A** přepínač tak, aby zadejte aplikaci služby funkce, které chcete přidat do databáze. V takovém případě **m** označuje, že chcete přidat podporu pro zprostředkovatele členství a **r** označuje, že chcete přidat podporu pro roli správce.
    3. Použití **– d** přepínač tak, aby zadejte název pro vaši databázi aplikace služby. Pokud tento přepínač vynecháte, nástroj vytvoří databázi s názvem výchozí **aspnetdb**.
3. Když databáze byla úspěšně vytvořena, příkazovém řádku se zobrazí potvrzení.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Další informace o aspnet\_regsql nástroj, najdete v části [ASP.NET nástroj pro registraci serveru SQL Server (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Dalším krokem je zajistit, že připojovací řetězce v řešení obraťte se na správce, přejděte na vaše místní instance systému SQL Server Express.

**Chcete-li aktualizovat připojovací řetězce**

1. Obraťte se na správce řešení otevřete v sadě Visual Studio 2010.
2. V **Průzkumníku řešení** okno, rozbalte **ContactManager.Mvc** projektu a potom dvakrát klikněte na **Web.config** uzlu.

    > [!NOTE]
    > Tento projekt ContactManager.Mvc zahrnuje dvě *web.config* soubory. Budete muset upravit soubor projektu.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. V **connectionStrings** elementu, ověřte, že připojovací řetězec s názvem **ApplicationServices** odkazuje na vaše místní databáze aplikace služby ASP.NET.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. V **Průzkumníku řešení** okno, rozbalte **ContactManager.Service** projektu a potom dvakrát klikněte na **Web.config** uzlu.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. V **connectionStrings** element v připojovací řetězec s názvem **ContactManagerContext**, ověřte, zda **zdroj dat** je nastavena na vaše místní instanci systému SQL Server Express. Nemusíte nic jiného v připojovacím řetězci změnit.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Všechny otevřené soubory uložte.

Teď byste měli být připravení spustit řešení obraťte se na správce na místním počítači.

> [!NOTE]
> Pokud budete postupovat podle těchto kroků bez první vytvoření databázi aplikačních služeb ASP.NET databáze se vytvoří při prvním pokusu vytvořit uživatele. Ale ruční vytvoření databáze vám dává mnohem větší kontrolu nad sadu funkce služby aplikace, do které chcete podporovat.


**Ke spuštění řešení obraťte se na správce**

1. V sadě Visual Studio 2010 stisknutím klávesy F5.
2. Internet Explorer spuštění a požadavky adresu URL aplikace, obraťte se na správce ASP.NET MVC 3. Ve výchozím nastavení, zobrazí aplikace **všechny kontakty** stránky.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Přidejte několik kontakty a potom ověřte, že aplikace funguje podle očekávání.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Přejděte do `http://localhost:50114/Account/Register` (Upravit adresu URL, pokud máte aplikace na jiný port). Přidejte uživatelské jméno, e-mailovou adresu a heslo a ověřte, že budete moct zaregistrovat účet úspěšně.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Přejděte do `http://localhost:50114/Account/LogOn` (Upravit adresu URL, pokud máte aplikace na jiný port). Ověřte, že budete moci přihlásit pomocí účtu, který jste právě vytvořili.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Zavřete Internet Explorer, aby se ukončilo ladění.

## <a name="conclusion"></a>Závěr

Obraťte se na správce řešení v tomto okamžiku by měl být plně nakonfigurovaný, ke spuštění na místním počítači. Řešení můžete použít jako referenci, při práci prostřednictvím na další témata v tomto kurzu.

Dalším tématu [vysvětlení souboru projektu](understanding-the-project-file.md), vysvětluje, jak můžete použít vlastní soubory projektu Microsoft Build Engine (MSBuild) v rámci řešení obraťte se na správce pro řízení procesu nasazení.

>[!div class="step-by-step"]
[Předchozí](the-contact-manager-solution.md)
[další](understanding-the-project-file.md)
