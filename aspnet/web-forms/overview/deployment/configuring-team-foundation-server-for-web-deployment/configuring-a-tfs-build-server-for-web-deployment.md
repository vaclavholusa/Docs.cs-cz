---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: "Konfigurace sady TFS sestavení serveru pro nasazení webu | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje postup přípravy serveru Team Foundation Server (TFS) sestavení pro sestavení a nasadit řešení pomocí Team Build a Internetu Informat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: de31a9dffb95b863a4ec38b74fd2c6e03f287a7f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurace serveru TFS sestavení pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup přípravy serveru Team Foundation Server (TFS) sestavení pro sestavení a nasadit řešení pomocí Team Build a nástroj pro nasazení Internetové informační služby (IIS) webu (Web Deploy).


Toto téma je součástí ze série kurzů na základě kolem podnikové požadavky nasazení fiktivní společnost s názvem Fabrikam, Inc. Tento kurz series používá ukázkové řešení & #x 2014; [řešení obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; představující webovou aplikaci s úrovní realistické složitější, včetně aplikace ASP.NET MVC 3 systému Windows Komunikační služby Foundation (WCF) a projekt databáze.

Metoda nasazení jádrem tyto kurzy je založena na popsaný přístup souboru projektu rozdělení [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízené procesu sestavení dva projektu soubory & #x 2014; jeden obsahující sestavení pokyny, které platí pro každé cílové prostředí a jeden, který obsahuje nastavení pro konkrétní prostředí sestavení a nasazení. V okamžiku sestavení souboru projektu konkrétní prostředí sloučeny do souboru projektu bez ohledu na prostředí a vytvořit úplnou sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Příprava serveru sestavení pro sestavení a nasadit řešení, musíte:

- Nainstalujte a nakonfigurujte službu sestavení sady TFS.
- Nainstalujte sadu Visual Studio 2010.
- Nainstalujte všechny produkty nebo součásti, které jsou nutné k vytvoření vlastního řešení, jako je verze rozhraní .NET Framework nebo ASP.NET MVC.
- Instalovat nasazení webu 2.0 nebo novější.

Toto téma vám ukáže, jak provádět tyto postupy nebo přejděte na další zdroje, pokud existují. Úlohy a postupy v tomto tématu předpokládat, že:

- Začínáte s čistou serveru sestavení systémem Windows Server 2008 R2 Service Pack 1.
- Server je připojený k doméně se statickou IP adresou.
- Aplikační vrstvě TFS jste nainstalovali na samostatném serveru, jak je popsáno v [Enterprise nasazení webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Ve většině případů bude zodpovědná za konfiguraci servery sestavení správce sady TFS. V některých případech týmem vývojářů může převzít vlastnictví servery konkrétní sestavení.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalace a konfigurace služby sestavení sady TFS

Při konfiguraci serveru sestavení první úlohou je nainstalovat a nakonfigurovat službu sestavení sady TFS. V rámci tohoto procesu budete potřebovat k:

- Nainstalovat službu sestavení sady TFS a konfigurace účtu služby. Všechny úlohy sestavení, včetně nasazení, se spustí pomocí identity účtu služby sestavení.
- Vytvoření *vytvoření řadiče* a jeden nebo více *agentů sestavení*. Každý řadič sestavení spravuje sadu agenty sestavení. Pokud jste ve frontě sestavení, přiřadí kontroleru buildu sestavovací úlohy agenta k dispozici sestavení. Každou kolekci týmového projektu v sadě TFS je namapována na řadič jednoho sestavení.
- Konfigurace složky drop pro vaše výstupy sestavení. Toto je sdílené síťové složky. Žádné sestavení výstupů, jako jsou balíčky nasazení webu, se odesílají do složky, vyřaďte.

[Správu Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) kapitoly na webu MSDN obsahuje všechny prostředky, které potřebují k vykonávání těchto úkolů:

- Koncepční přehled služby Team Foundation Build, včetně služby sestavení, řadiče sestavení a agenty sestavení, viz [Principy Team Foundation sestavení System](https://msdn.microsoft.com/library/dd793166.aspx).
- Informace o instalaci a konfiguraci služby sestavení najdete v tématu [konfigurace počítače sestavení](https://msdn.microsoft.com/library/ms181712.aspx).
- Informace o vytváření řadiče sestavení najdete v tématu [vytvoření a práci s řadičem sestavení](https://msdn.microsoft.com/library/ee330987.aspx).
- Informace o vytváření agentů sestavení najdete v tématu [vytvoření a práci s agenty, sestavení](https://msdn.microsoft.com/library/bb399135.aspx).
- Informace o vytváření a konfiguraci rozevírací složky najdete v tématu [nastavit až vyřadit složky](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalace požadovaných produktů a součásti

K povolení sestavení serveru k vytvoření řešení, musíte nainstalovat všechny produkty, součásti nebo sestavení, které vyžaduje vaše řešení. Než nainstalujete všechny komponenty webové platformy, musíte nainstalovat Visual Studio 2010 (všechny verze) na serveru sestavení. Tím se zajistí cílové soubory jádra Microsoft Build Engine (MSBuild) a soubory cíl webových publikování kanálu (WPP) k dispozici pro službu sestavení. Instalační program sady Visual Studio také nainstalovat nasazení webu, který budete potřebovat, pokud máte v úmyslu nasadit jako součást procesu sestavení webových balíčků.

Nejlepší způsob, jak nainstalovat běžné komponenty webové platformy má používat [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118). To zajišťuje, že instalujete nejnovější verzi každého produktu a taky automaticky rozpozná a nainstaluje všechny požadavky pro jednotlivé produkty. U [obraťte se na správce](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení, používejte služby instalace webové platformy nainstalovat tyto produkty a součásti:

- **Rozhraní .NET framework 4.0**. To je nutné ke spuštění aplikací, které byly vytvořené v této verzi rozhraní .NET Framework.
- **Webové nástroje pro nasazení 2.1 nebo vyšší**. Tím se nainstaluje Web Deploy (a její základní spustitelný soubor MSDeploy.exe) na serveru. V rámci tohoto procesu se nainstaluje a spustí služba agenta pro nasazení webu. Tato služba umožňuje nasadit webovou balíčky ze vzdáleného počítače.
- **ASP.NET MVC 3**. Tím se nainstaluje sestavení, je nutné spouštět aplikace ASP.NET MVC 3.

**K instalaci požadovaných produktů a součásti**

1. Nainstalujte sadu Visual Studio 2010. Po zobrazení výzvy vyberte funkce určené k instalaci, by měla obsahovat:

    1. Žádné programovací jazyky, které budete muset zkompilovat.
    2. Visual Web Developer. Tím se zajistí, že jsou přidány jako cíle na vašem serveru sestavení.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Po dokončení instalace sady Visual Studio 2010 stáhnout a nainstalovat [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (Pokud je již není součástí instalačního média).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 řeší chyby, které by mohly bránit MSBuild nemůže nalézt MSDeploy spustitelný soubor.
3. Stáhněte a spusťte [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).
4. V horní části **webové platformy verze 3.0** okně klikněte na tlačítko **produkty**.
5. Na levé straně okna, v navigačním podokně klikněte na tlačítko **rozhraní**.
6. V **rozhraní Microsoft .NET Framework 4** řádek, pokud již není nainstalována rozhraní .NET Framework, klikněte na tlačítko **přidat**.

    > [!NOTE]
    > Může po instalaci rozhraní .NET Framework 4.0 prostřednictvím služby Windows Update. Pokud produkt nebo součást je již nainstalován, instalace webové platformy označí to nahrazením **přidat** tlačítka s textem **nainstalovaná**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. V **ASP.NET MVC 3 (Visual Studio 2010)** řádek, klikněte na tlačítko **přidat**.
8. V navigačním podokně klikněte na tlačítko **Server**.
9. V **2.1 nástroj pro nasazení webového** řádek, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů & #x 2014; společně s všechny přidružené závislosti & #x 2014; nainstalována a zobrazí výzvu k potvrzení licenčních podmínek.
11. Přečtěte si licenční podmínky a pokud vyjadřujete svůj souhlas s podmínkami, klikněte na **souhlasím**.
12. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **webové platformy verze 3.0** okno.

> [!NOTE]
> Pokud váš proces nasazení obsahuje nástroje, jako je VSDBCMD.exe nebo SQLCMD.exe, musíte zajistit, aby byly nainstalovány na vašem serveru sestavení. VSDBCMD.exe je nástroj Visual Studio a je obvykle přidána na server, když instalujete Team Foundation Build. SQLCMD.exe je nástroj SQL Server. Samostatnou verzi SQLCMD.exe z můžete stáhnout [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) stránky.


## <a name="conclusion"></a>Závěr

V tomto okamžiku je váš server sestavení chtít začít vytvářet a nasazovat vaše projekty webových aplikací. Dalším tématu [vytváření sestavení definice, podporuje nasazení](creating-a-build-definition-that-supports-deployment.md), popisuje, jak vytvořit a nakonfigurovat definice sestavení, které řídí, kdy a jak jsou projekty vytvořené a nasazené.

## <a name="further-reading"></a>Další čtení

Další obecné pokyny o práci s Team Build najdete v tématu [správu Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

>[!div class="step-by-step"]
[Předchozí](adding-content-to-source-control.md)
[další](creating-a-build-definition-that-supports-deployment.md)
