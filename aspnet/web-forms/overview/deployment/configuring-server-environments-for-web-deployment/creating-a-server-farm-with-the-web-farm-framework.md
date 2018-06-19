---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Vytvoření serverové farmy pomocí rozhraní Web Farm Framework | Microsoft Docs
author: jrjlee
description: Toto téma popisuje postup použijte k vytvoření a konfigurace webové farmy serveru z kolekce serverů webové farmy Framework (WFF) 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 53a91660953795f2c55edcd795b053641d308dfe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882462"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Vytvoření serverové farmy pomocí rozhraní Web Farm Framework
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup použijte k vytvoření a konfigurace webové farmy serveru z kolekce serverů webové farmy Framework (WFF) 2.0.


WFF umožňuje synchronizovat produkty webové platformy a součásti webové aplikace, weby a nastavení konfigurace více Vyrovnávání zatížení sítě webových serverů. Ve scénářích, kde je nutné více než jeden webového serveru, jako pracovní a provozní prostředí to může významně zjednodušit proces nasazení a konfigurace. Můžete nasadit webovou aplikaci na jednom serveru&#x2014; *primární server*&#x2014;a WFF bude automaticky replikovat této webové aplikace na všechny další webové servery v serverové farmě.

## <a name="understanding-the-web-farm-framework"></a>Vysvětlení rozhraní Web Farm Framework

WFF 2.0 můžete zřizovat a spravovat nasazení obsahu ke skupině webových serverů. WFF nasazení se skládá ze tří role klíče serveru:

- *Serveru controller*. Tento server se používá k vytvoření a konfigurace WFF serverové farmy. Serveru controller spravuje synchronizace komponenty webové platformy, nastavení konfigurace a aplikací mezi webovými servery v serverové farmě. Nainstalujte WFF 2.0 na serveru řadiče a serveru controller zase nainstalujte agenta WFF na všech serverech ve farmě serverů. Serveru controller nepatří koncepčně do farmy serverů všechny WFF a jedním řadičem server můžete spravovat více serverové farmy. V tomto scénáři použijete jeden server řadiče WFF k vytváření a správě pracovní serverovou farmu a produkční serverové farmy.
- *Primární server*. Každý server farmy WFF obsahuje jeden primární server. Při instalaci komponenty webové platformy nebo nasazení aplikace na primární server, WFF synchronizuje změny na všechny ostatní servery v serverové farmě.
- *Sekundární server*. Každý server farmy WFF obsahuje jeden nebo více sekundárních serverů. Všechny změny provedené na primární server se replikují na každý sekundární server v serverové farmě.

Ukazuje to, jak tyto role serveru vztahují k společnost Fabrikam A.s. pracovní a provozní prostředí:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

V tomto scénáři je pracovní prostředí a provozním prostředí jsou obě nakonfigurovány jako WFF serverové farmy. Jeden server řadiče WFF spravuje obou farmy. V rámci každé serverové farmě všechny změny na primární server replikovány na každý sekundární server.

Než začnete konfigurovat vaše pracovní a provozní prostředí, doporučujeme, abyste si přečetli tyto články Seznamte se s klíčové koncepty WFF 2.0:

- [Přehled rozhraní Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Nastavení serverové farmy s Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Systém a požadavky na platformu pro rozhraní Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Přehled úloh

Pokud chcete provést postupy v tomto tématu a úlohy, budete potřebovat aspoň tři servery&#x2014;jeden řadič WFF, jeden primární webový server pro serverovou farmu a jeden nebo více sekundárních webových serverů pro serverové farmy. Kdykoli můžete přidat další sekundární servery do farmy serverů WFF. Na vysoké úrovni, vytvoření a konfigurace serverové farmy WFF ve vašem pracovním nebo produkčním prostředí, které budete muset:

- Vytvoření řadiče serveru Internetové informační služby (IIS) 7.5 a WFF 2.0.
- Připravte primární a sekundární servery vytvořením společného účtu správce a konfigurace výjimek brány firewall.
- Konfigurace serverové farmy pomocí Správce služby IIS na serveru řadiče.
- Konfigurace služby Vyrovnávání zatížení pomocí služby IIS směrování (žádostí na aplikace) nebo alternativní technologie Vyrovnávání zatížení.

Úlohy a postupy v tomto tématu předpokládat, že začínáte s sestavení čistou serveru se systémem Windows Server 2008 R2. Než začnete, pro každý server, zajistěte, aby:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.
- Server je připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně, najdete v části [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurovat statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Vytvoření serveru WFF Controller

Vytvoření serveru řadiče WFF, budete muset nainstalovat službu IIS 7 nebo novější a WFF 2.0 nebo novější. V pozadí, WFF používá nástroj nasazení webu služby IIS (Web Deploy) 2.x synchronizovat serverech ve farmě. Pokud instalujete WFF pomocí služby instalace webové platformy, instalační program automaticky stáhnout a nainstalovat nasazení webu pro vás.

**Vytvoření serveru řadiče WFF**

1. Stáhněte a nainstalujte [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9739157).
2. V horní části **webové platformy verze 3.0** okně klikněte na tlačítko **produkty**.
3. Na levé straně okna, v navigačním podokně klikněte na tlačítko **Server**.
4. V **IIS 7 doporučená konfigurace** řádek, klikněte na tlačítko **přidat**.
5. V <strong>webové farmy Framework 2.</strong> <em>x</em> řádek, klikněte na tlačítko <strong>přidat</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Klikněte na tlačítko **nainstalovat**. Všimněte si, že instalace webové platformy přidala nástroj pro nasazení webu, společně s různé další závislosti, do seznamu instalace.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Přečtěte si licenční podmínky a pokud vyjadřujete svůj souhlas s podmínkami, klikněte na **souhlasím**.
8. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **webové platformy verze 3.0** okno.

## <a name="configure-the-primary-and-secondary-servers"></a>Nakonfigurujte primární a sekundární servery

Než vytvoříte WFF serverové farmy, můžete provést některé přípravy úlohy na webových serverech, které budou součástí farmy:

- Přidat výjimky brány firewall umožňující **základní sítě**, **vzdálené správy**, a **sdílení souborů a tiskáren** funkce pro komunikaci se serverem řadiče WFF .
- Vytvoření účtu domény (například **FABRIKAM\stagingfarm**) ve službě Active Directory a přidejte ji do místní skupiny administrators na každém serveru. Tento účet jako účet správce serverové farmy budete používat při vytváření serverové farmy.

Další informace o tom, jak konfigurovat tyto výjimky brány firewall v bráně Windows Firewall najdete v tématu [systému a požadavky na platformu pro Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805128). Pro jiné systémy brány firewall vyhledejte v dokumentaci produktu.

Další postup slouží k přidání účtu domény do místní skupiny administrators v systému Windows Server 2008 R2. Tento postup proveďte na každém serveru, který chcete přidat do farmy serverů&#x2014;jinými slovy, stejný účet domény přidat do místní skupiny administrators na primárním serveru a na každém sekundárním serveru.

**Přidat účet domény do místní skupiny administrators**

1. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **správce serveru**.
2. V **správce serveru** rozbalte okno, v podokně zobrazení stromu **konfigurace**, rozbalte položku **místní uživatelé a skupiny**a pak klikněte na tlačítko **skupiny**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. V **skupiny** podokně dvakrát klikněte na **správci**.
4. V **správci vlastnosti** dialogové okno, klikněte na tlačítko **přidat**.
5. V **vyberte uživatele, počítače, účty služby nebo skupiny** dialogové okno, zadejte (nebo vyhledejte) ke svému účtu domény (například **FABRIKAM\stagingfarm**) a pak klikněte na tlačítko **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. V **správci vlastnosti** dialogové okno, klikněte na tlačítko **OK**.

Vaše servery jsou nyní připraveny pro přidání do serverové farmy. V případě primární server, můžete nastavit server podle požadavků vaší aplikace před nebo po vytvoření serverové farmy&#x2014;v obou případech bude WFF synchronizovat servery nasazením stejné produkty, součásti nebo konfigurace na sekundární servery. Z důvodu zjednodušení tento kurz předpokládá, že po dokončení vytvoření serverové farmy nakonfigurujete primární server.

## <a name="create-the-wff-server-farm"></a>Vytvoření serverové farmy WFF

V tomto okamžiku jsou připravené k přidání do farmy serverů WFF všechny vaše servery:

- WFF jste nainstalovali na server řadiče.
- Výjimky brány firewall jste nakonfigurovali na primární a sekundární webové servery.
- Účet domény jste přidali do místní skupiny administrators na primární a sekundární webové servery.

Dalším krokem je vytvoření serverové farmy v WFF. To provedete ze Správce služby IIS na serveru řadiče WFF.

**Chcete-li vytvořit WFF serverové farmy**

1. Na serveru řadiče WFF na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **Správce Internetové informační služby (IIS)**.
2. V **připojení** podokně rozbalte uzel místní server, klikněte pravým tlačítkem na **serverové farmy**a potom klikněte na **vytvořit serverovou farmu**.
3. V **vytvořit serverovou farmu** dialogovém okně zadejte smysluplný název serverové farmy (například **pracovní farmy**) a potom vyberte **zřídit serverové farmy**.
4. Zadejte uživatelské jméno a heslo účtu domény, který jste přidali do místní skupiny administrators na každém serveru.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Klikněte na tlačítko **Další**.
6. Na **přidat servery** stránky, zadejte plně kvalifikovaný název domény (FQDN) primárního serveru, vyberte **primární Server**a pak klikněte na **přidat**.
7. V tomto okamžiku WFF pokusí kontaktovat primární server, pomocí přihlašovacích údajů, které jste zadali. Pokud je připojení úspěšné, bude primární server přidat do tabulky na **přidat servery** stránky.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Možná jste si všimli, **Server je k dispozici pro vyrovnávání zatížení** je standardně vybraná. WFF modul směrování žádostí na aplikace služby IIS používá k implementaci Vyrovnávání zatížení a tím distribuci požadavků webových serverů v serverové farmě. Ve většině scénářů byste pouze vymazat **Server je k dispozici pro službu Vyrovnávání zatížení** možnost, pokud jste chtěli použít místo toho řešení vyrovnávání zatížení třetích stran.
8. Na **přidat servery** zadejte plně kvalifikovaný název domény vaší první sekundární server a pak klikněte na tlačítko **přidat**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Opakujte krok 7 pro všechny další sekundární servery ve farmě a pak klikněte na tlačítko **Dokončit**.

Serverové farmy WFF je teď spuštěná. Všechny produkty webové platformy nebo součásti, které můžete nainstalovat na primární server a všechny webové aplikace nebo obsah, který nasazujete na primární server, se automaticky zřídí na všechny sekundární servery.

WFF je široký a komplexní téma a další informace o ho na [Microsoft Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805129) webu. Prozatím, ale existují dvě funkce oblasti, které budete muset mít na paměti:

- *Zřizování aplikace* je proces, který replikuje obsah z primárního serveru, jako jsou webové aplikace a nastavení konfigurace, přes všechny sekundární servery v serverové farmě. Například pokud primární server pracovní nasazení ukázkové řešení obraťte se na správce, WFF procesu zřizování aplikace nasadí řešení na všechny sekundární servery pracovní. Proces zřizování aplikací ve výchozím nastavení spouští každých 30 sekund.
- *Zřizování platformy* je proces, který synchronizuje produkty webové platformy a součásti z primárního serveru na všechny sekundární servery v serverové farmě. Například pokud nainstalujete primární pracovní server ASP.NET MVC 3, použije proces zřizování platformy služby instalace webové platformy nainstalovat ASP.NET MVC 3 na všech serverech sekundární pracovní. Ve výchozím nastavení spouští každých pět minut platformy procesu zřizování.

Můžete spravovat základní aplikace a nastavení zřizování platformy ze Správce služby IIS na serveru řadiče WFF.

**Prozkoumejte platformu zřizování nastavení a aplikace**

1. Ve Správci služby IIS v **připojení** podokně vyberte serverovou farmu.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. V **serverové farmy** podokně dvakrát klikněte na **zřizování aplikace**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Jak vidíte, serverové farmy je nyní nakonfigurován, aby webového obsahu a konfigurace nastavení mezi primárním serverem a sekundárních serverech synchronizovat každých 30 sekund.
4. Klikněte na tlačítko **zpět**a potom dvakrát klikněte na **platformy zřizování**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Jak vidíte, serverové farmy je nyní nakonfigurován, aby synchronizovat produkty webové platformy a součásti mezi primárním serverem a sekundárních serverech každých pět minut.
6. Klikněte na tlačítko **zpět**.
7. Chcete-li vynutit serverové farmy synchronizovat produkty webové platformy okamžitě, v **akce** podokně klikněte na tlačítko **zřídit platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Zřizování platformy může chvíli trvat. Instalační program proces běží na pozadí na sekundárních serverech ve farmě serveru.
8. Jakmile jste povolili dostatek času na dokončení procesu zřizování, můžete ověřit, že produkty a součásti, které jste přidali na primární server nyní replikovaly na sekundárních serverech. Například může přihlásit k sekundárnímu serveru a používat **správce serveru** okna ověřit, že byla nainstalována role webového serveru.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Můžete také zkontrolovat seznamu nainstalovaných programů a ověřte, že byly přidány různé komponenty webové platformy.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurace služby Vyrovnávání zatížení

Když vytvoříte webové farmy, budete muset nastavit určitou formu k distribuci požadavků HTTP mezi webových serverů Vyrovnávání zatížení. To může být Windows Server 2008 služby Vyrovnávání zatížení sítě, směrování žádostí na aplikace služby IIS, nebo jiných výrobců založené na hardwaru nebo softwaru na základě řešení vyrovnávání zatížení.

WFF slouží k integraci úzce s směrování žádostí na aplikace služby IIS Chcete-li využít této integrace, nainstalujte modul směrování žádostí na aplikace na server WFF řadiče. Pak nasměrujete veškerého webového provozu k serveru řadiče obvykle nakonfigurováním záznamy systému DNS (Domain Name). Server řadiče se pak distribuovat příchozí požadavky mezi servery ve farmě, na základě dostupnosti serveru a různých dalších kritérií.

> [!NOTE]
> Nemusíte používat směrování žádostí na aplikace s WFF; můžete nakonfigurovat WFF pro práci s řešení vyrovnávání zatížení třetích stran. Další informace najdete v tématu [přehled Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805126).


Vyrovnávání zatížení pomocí směrování žádostí na aplikace je komplexní téma, většina z nich je nad rámec tohoto kurzu. Můžete však následující postup k instalaci modulu Směrování žádostí na aplikace a Začínáme se službou Vyrovnávání zatížení.

**Chcete-li nastavit na serveru řadiče WFF Vyrovnávání zatížení**

1. Na serveru řadiče WFF spuštění instalačního programu webové platformy.
2. V horní části **webové platformy verze 3.0** okně klikněte na tlačítko **produkty**.
3. Na levé straně okna, v navigačním podokně klikněte na tlačítko **Server**.
4. V **směrování 2.5 žádosti o aplikace** řádek, klikněte na tlačítko **přidat**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Klikněte na tlačítko **nainstalovat**a pak postupujte podle pokynů v **instalace webové platformy** okno.
6. Po dokončení instalace spusťte Správce služby IIS a **připojení** podokně klikněte na uzel serveru farmy. Všimněte si, že přidané několik nových ikon **serverové farmy** podokně.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. V **serverové farmy** podokně dvakrát klikněte na **nástroj pro vyrovnávání zatížení**.
8. V **Vyrovnávání zatížení** podokně, vyberte zatížení vyvážit algoritmus (například **alespoň aktuální požadavek**).

    > [!NOTE]
    > Další informace o vyrovnávání zatížení algoritmy a dalších nastavení konfigurace, najdete v části [aplikace požadavků modulu Směrování](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. V **akce** podokně klikněte na tlačítko **použít**.

Nyní jste nakonfigurovali základní Vyrovnávání zatížení pro servery v serverové farmě. Pokud jste směrovat všechen provoz farmy vaší web na serveru řadiče, požadavky budou distribuována mezi servery ve farmě podle dostupnosti a algoritmus Vyrovnávání zatížení, které jste vybrali.

Další informace o tom, jak nakonfigurovat Vyrovnávání zatížení se směrováním žádostí na aplikace najdete v tématu [aplikace požadavků modulu Směrování](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorování serverové farmy

Kdykoli pomocí Správce služby IIS na serveru řadiče můžete monitorovat stav serverové farmy. V **připojení** podokně rozbalte serverovou farmu a pak klikněte na tlačítko **servery**. V prostředním podokně se zobrazí souhrn každého serveru ve farmě společně s poslední aktivity protokolu trasování.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Závěr

Serverové farmy WFF by teď měly být spuštěná. Můžete nastavit primární server pro podporu v obou případech nasazení dáváte přednost&#x2014;najdete v části Další čtení podrobnosti&#x2014;a konfiguraci bude replikován na každém sekundárním serveru v serverové farmě.

## <a name="further-reading"></a>Další čtení

Další informace o všech aspektech konfiguraci a použití WFF najdete v části [Microsoft Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805129) webu.

> [!div class="step-by-step"]
> [Předchozí](configuring-a-database-server-for-web-deploy-publishing.md)
> [další](configuring-deployment-properties-for-a-target-environment.md)
