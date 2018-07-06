---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
title: Konfigurace serveru TFS Server sestavení pro nasazení webu | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak připravit server sestavení Team Foundation Server (TFS) k vytvoření a nasazení řešení s využitím Team Build nebo Informat Internetu...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: f8400241-4f4b-4bbd-9994-54fb64909e6e
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/configuring-a-tfs-build-server-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7f57ad0392a068964bb910fbbaafea105fdbb3d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841338"
---
<a name="configuring-a-tfs-build-server-for-web-deployment"></a>Konfigurace serveru TFS Build pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak připravit server sestavení Team Foundation Server (TFS) k vytvoření a nasazení řešení s využitím týmové sestavení a nástroj pro nasazení Internetové informační služby (IIS) webu (nasazení webu).


Toto téma je součástí série kurzů podle požadavků na nasazení enterprise fiktivní společnosti s názvem společnosti Fabrikam, Inc. V této sérii kurzů používá ukázkové řešení&#x2014; [řešení Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;představující webovou aplikaci s realistické úroveň složitosti, včetně aplikace ASP.NET MVC 3, komunikace Windows Služba Foundation (WCF) a databázový projekt.

Metody nasazení v srdci těchto kurzů je založen na rozdělení přístupu soubor projektu je popsáno v [vysvětlení souboru projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), ve které je řízena procesem sestavení dva soubory projektu&#x2014;jeden obsahující pokyny, které platí pro všechny cílové prostředí a jeden obsahuje nastavení pro konkrétní prostředí sestavení a nasazení pro sestavení. V okamžiku sestavení souboru projektu specifických pro prostředí se sloučí do souboru projektu bez ohledu na prostředí a vytvoří kompletní sadu pokynů sestavení.

## <a name="task-overview"></a>Přehled úloh

Pokud chcete připravit server sestavení k sestavení a nasazení vašeho řešení, bude nutné:

- Instalace a konfigurace služby sestavení serveru TFS.
- Instalace sady Visual Studio 2010.
- Nainstalujte všechny produkty nebo komponenty, které jsou nutné k vytvoření řešení, jako je verze rozhraní .NET Framework nebo technologie ASP.NET MVC.
- Nainstalujte nástroj nasazení webu 2.0 nebo novější.

Toto téma se ukazují, jak provést tyto postupy nebo ukazovaly na další zdroje, pokud existují. Úlohy a názorné postupy v tomto tématu se předpokládá, že:

- Začínáte s čisté server sestavení spuštěn Windows Server 2008 R2 Service Pack 1.
- Server není připojený k doméně se statickou IP adresou.
- Jste nainstalovali aplikační vrstvu TFS na samostatném serveru, jak je popsáno v [nasazení podnikového webu: přehled scénářů](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md).

### <a name="who-performs-these-procedures"></a>Kdo provádí tyto postupy?

Ve většině případů bude zodpovědná za konfiguraci serverů sestavení správcem serveru TFS. V některých případech vývojovému týmu může převzít vlastnictví servery konkrétního sestavení.

## <a name="install-and-configure-the-tfs-build-service"></a>Instalace a konfigurace služby sestavení TFS

Při konfiguraci serveru sestavení, je první úkol k instalaci a konfiguraci služby sestavení serveru TFS. Jako součást tohoto procesu bude potřeba:

- Nainstalovat službu sestavení sady TFS a nakonfigurovat účet služby. Všechny úlohy sestavení, včetně nasazení, se spustí pomocí identity účtu služby sestavení.
- Vytvoření *kontrolér sestavení* a jeden nebo více *sestavovací agenty*. Každý kontrolér sestavení spravuje sadu agentů sestavení. Při sestavení do fronty, kontrolér sestavení úlohu sestavení přiřadí agenta sestavení k dispozici. Každou kolekci týmových projektů v TFS je namapován na jeden řadič sestavení.
- Nakonfigurujte odkládací složky pro vaše výstupy sestavení. Toto je sdílené síťové složky. Všechny výstupy, jako jsou balíčky webového nasazení sestavení, jsou odeslány do odkládací složky.

[Správa Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx) kapitoly na webové stránce MSDN obsahuje všechny prostředky, které potřebují k vykonávání těchto úkolů:

- Koncepční přehled služby Team Foundation Build, včetně služby build service, kontrolery sestavení a agenty sestavení, viz [Principy systému sestavení Team Foundation](https://msdn.microsoft.com/library/dd793166.aspx).
- Informace o instalaci a konfiguraci služby sestavení najdete v tématu [konfigurace sestavení počítače](https://msdn.microsoft.com/library/ms181712.aspx).
- Informace o vytváření kontrolery sestavení naleznete v tématu [Create and Work s řadičem sestavení](https://msdn.microsoft.com/library/ee330987.aspx).
- Informace o vytváření sestavovacích agentů najdete v tématu [Create and Work s agenty sestavení](https://msdn.microsoft.com/library/bb399135.aspx).
- Informace o vytváření a nastavení odkládacích složek, najdete v tématu [nastavit až vyřadit složky](https://msdn.microsoft.com/library/bb778394.aspx).

## <a name="install-required-products-and-components"></a>Instalace požadovaných produktů a součásti

Povolit server sestavení pro vytváření řešení, musíte nainstalovat všechny produkty, součásti nebo sestavení, která vyžaduje vaše řešení. Než nainstalujete všechny komponenty webové platformy, měli byste nainstalovat Visual Studio 2010 (libovolná verze) na serveru sestavení. Tím se zajistí, že jsou k dispozici ve službě sestavení cílové soubory jádra Microsoft Build Engine (MSBuild) a cílové soubory webových publikování kanálu (WPP). Instalační program sady Visual Studio by měl také nainstalovat nasazení webu, který budete potřebovat, pokud plánujete nasazení webových balíčků jako součást procesu sestavení.

Nejlepší způsob, jak nainstalovat běžné komponenty webové platformy je použít [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118). Tím se zajistí, že instalujete nejnovější verzi každého produktu, a taky automaticky rozpozná a nainstaluje všechny požadované součásti pro jednotlivé produkty. V případě třídy [Správce kontaktů](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) řešení, používejte instalačního programu webové platformy nainstalovat tyto produkty a komponenty:

- **Rozhraní .NET framework 4.0**. To je potřeba ke spouštění aplikací, které byly vytvořeny v této verzi rozhraní .NET Framework.
- **Webové nástroje pro nasazení 2.1 nebo novější**. Tím se nainstaluje Web Deploy (a její podkladové spustitelný soubor MSDeploy.exe) na serveru. V rámci tohoto procesu se nainstaluje a spustí služba agenta nasazení webu. Tato služba vám umožní nasadit webových balíčků ze vzdáleného počítače.
- **ASP.NET MVC 3**. Tím se nainstaluje sestavení, je potřeba spouštět aplikace ASP.NET MVC 3.

**Chcete-li nainstalovat požadované produkty a komponenty**

1. Instalace sady Visual Studio 2010. Po zobrazení výzvy vyberte funkce určené k instalaci, měli byste zahrnout:

    1. Žádné programovací jazyky, které potřebujete ke kompilaci.
    2. Visual Web Developer. Tím se zajistí, že WPP cíle, které jsou přidány do serveru sestavení.

        ![](configuring-a-tfs-build-server-for-web-deployment/_static/image1.png)
2. Po dokončení instalace sady Visual Studio 2010, stáhněte a nainstalujte [Visual Studio 2010 Service Pack 1](https://go.microsoft.com/?linkid=9805133) (Pokud je již není součástí instalačního média).

    > [!NOTE]
    > Visual Studio 2010 Service Pack 1 řeší chybu, která může zabránit vyhledání MSDeploy spustitelný soubor MSBuild.
3. Stáhnout a spustit [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).
4. V horní části **3.0 Instalační služby webové platformy** okna, klikněte na tlačítko **produkty**.
5. Na levé straně okna, v navigačním podokně klikněte na tlačítko **architektury**.
6. V **rozhraní Microsoft .NET Framework 4** řádek, pokud rozhraní .NET Framework není nainstalovaná, klikněte na tlačítko **přidat**.

    > [!NOTE]
    > Možná jste již nainstalovali .NET Framework 4.0 prostřednictvím služby Windows Update. Pokud už je nainstalován produkt nebo komponenty, instalačního programu webové platformy se označit to tak, že nahradíte **přidat** tlačítko s textem **nainstalováno**.

    ![](configuring-a-tfs-build-server-for-web-deployment/_static/image2.png)
7. V **ASP.NET MVC 3 (Visual Studio 2010)** řádku, klikněte na tlačítko **přidat**.
8. V navigačním podokně klikněte na tlačítko **Server**.
9. V **2.1 nástroj Web Deployment** řádku, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů&#x2014;spolu s případnými přidružené závislosti&#x2014;k instalaci a zobrazí výzvu k potvrzení licenčních podmínek.
11. Přečtěte si licenční podmínky a pokud souhlasíte s podmínkami, klikněte na tlačítko **souhlasím**.
12. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **3.0 Instalační služby webové platformy** okna.

> [!NOTE]
> Pokud váš proces nasazení obsahuje nástroje, jako je VSDBCMD.exe nebo SQLCMD.exe, budete muset zajistit, aby byly nainstalovány na vašem serveru sestavení. VSDBCMD.exe je nástroj sady Visual Studio a je obvykle přidána na server, když instalujete Team Foundation Build. SQLCMD.exe je nástroj SQL Server. Můžete stáhnout samostatné verze SQLCMD.exe z [Microsoft SQL Server 2008 R2 Feature Pack](https://go.microsoft.com/?linkid=9805134) stránky.


## <a name="conclusion"></a>Závěr

V tuto chvíli váš server sestavení připravený začít vytvářet a nasazovat vaše projekty webových aplikací. Dalším tématu s názvem [vytvoření sestavení definice, že podporuje nasazení](creating-a-build-definition-that-supports-deployment.md), popisuje, jak vytvořit a nakonfigurovat definici sestavení, které řídí, kdy a jak jsou vaše projekty sestavíte a nasadíte.

## <a name="further-reading"></a>Další čtení

Další obecné informace o práci s Team Build najdete v tématu [Správa Team Foundation Build](https://msdn.microsoft.com/library/ms252495.aspx).

> [!div class="step-by-step"]
> [Předchozí](adding-content-to-source-control.md)
> [další](creating-a-build-definition-that-supports-deployment.md)
