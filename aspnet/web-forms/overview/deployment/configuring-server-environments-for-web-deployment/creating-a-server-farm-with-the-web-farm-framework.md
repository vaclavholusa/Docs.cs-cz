---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Vytvoření serverové farmy pomocí rozhraní Web Farm Framework | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak můžete vytvořit a nakonfigurovat webovou farmu serveru z kolekce serverů webové farmy Framework (WFF) 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: ad557306cb4d3c62932c640b598f82d692bc74cf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388290"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Vytvoření serverové farmy pomocí rozhraní Web Farm Framework
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak můžete vytvořit a nakonfigurovat webovou farmu serveru z kolekce serverů webové farmy Framework (WFF) 2.0.


WFF umožňuje synchronizaci produkty webové platformy a součásti webové aplikace, weby a nastavení konfigurace na několik serverů s vyrovnáváním zatížení web. V situacích, kdy potřebujete víc než jednom webovém serveru, jako přípravného a produkčního prostředí to může výrazně zjednodušit nasazení a konfiguraci procesu. Můžete nasadit webové aplikace k jednomu serveru&#x2014; *primární server*&#x2014;a WFF budou automaticky replikovat této webové aplikace na všech ostatních serverech web v serverové farmě.

## <a name="understanding-the-web-farm-framework"></a>Vysvětlení rozhraní Web Farm Framework

WFF 2.0 můžete zřizovat a spravovat nasazení obsahu ke skupině webových serverů. WFF nasazení se skládá z tři klíčové serverové role:

- *Serveru řadiče*. Tento server můžete vytvořit a nakonfigurovat WFF serverové farmy. Server řadič spravuje synchronizaci komponenty webové platformy, nastavení konfigurace a aplikace mezi webovými servery v serverové farmě. Nainstalujte WFF 2.0 na serveru řadiče a serveru řadiče zase nainstalujte WFF agenta na všech serverech ve farmě serverů. Serveru řadiče nepatří koncepčně do žádné serverové farmy WFF a jediného kontroleru server můžete spravovat více serverové farmy. V tomto scénáři použijete jeden server kontroleru WFF vytvářet a spravovat pracovní serverovou farmu a produkční farmě.
- *Primární server*. Každý server farmy WFF zahrnuje jeden primární server. Při instalaci komponenty webové platformy nebo nasazování aplikací na primárním serveru, WFF synchronizuje změny na všechny ostatní servery v serverové farmě.
- *Sekundární server*. Každý server farmy WFF obsahuje jeden nebo více sekundárních serverů. Všechny změny provedené na primárním serveru se replikují do každé sekundární server v serverové farmě.

To ukazuje, jak tyto role serveru souvisí společnosti Fabrikam, Inc. přípravného a produkčního prostředí:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

V tomto scénáři testovacího a produkčního prostředí jsou nakonfigurovány jako WFF serverové farmy. Jeden server WFF řadič spravuje obou farmy. V rámci každé serverové farmy se replikují všechny změny na primárním serveru na každém sekundárním serveru.

Před zahájením konfigurace přípravného a produkčního prostředí, doporučujeme, abyste si přečetli tyto články a seznamte se s klíčové koncepty WFF 2.0:

- [Přehled Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Nastavení serverová farma s Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Systém a požadavky na platformu pro rozhraní Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Přehled úloh

K dokončení úlohy a názorné postupy v tomto tématu, budete potřebovat alespoň tři servery&#x2014;jeden kontroler WFF, jeden primární webový server pro serverovou farmu a jeden nebo více sekundárních webových serverů pro serverové farmy. Kdykoli můžete přidat více sekundárních serverů do serverové farmy WFF. Na vysoké úrovni, po vytvoření a konfigurace WFF serverové farmy pro váš pracovní nebo produkční prostředí, ve kterém budete muset:

- Vytvoření serveru kontroleru pomocí instalace Internetové informační služby (IIS) 7.5 a WFF 2.0.
- Příprava primární a sekundární server vytvořením společného účtu správce a konfigurace výjimek brány firewall.
- Konfigurace serverové farmy pomocí Správce služby IIS na serveru řadiče.
- Konfigurace služby Vyrovnávání zatížení pomocí služby IIS požádat o směrování žádostí na aplikace nebo alternativní technologie Vyrovnávání zatížení.

Úlohy a názorné postupy v tomto tématu se předpokládá, že začínáte s čistou server sestavení s Windows serverem 2008 R2. Než začnete, pro každý server, ujistěte se, že:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace jsou nainstalovány.
- Server není připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně najdete v tématu [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurujte statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Vytvoření serveru WFF Kontroleru

Vytvoření serveru WFF řadič, budete muset nainstalovat službu IIS 7 nebo novější a WFF 2.0 nebo novější. Pod pokličkou, WFF používá nástroj nasazení webu služby IIS (Web Deploy) 2.x synchronizaci serverů ve farmě. Pokud používáte k instalaci WFF instalačního programu webové platformy, instalační program automaticky stáhněte a nainstalujte nástroj nasazení webu za vás.

**Vytvoření serveru WFF kontroleru**

1. Stáhněte a nainstalujte [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9739157).
2. V horní části **3.0 Instalační služby webové platformy** okna, klikněte na tlačítko **produkty**.
3. Na levé straně okna, v navigačním podokně klikněte na tlačítko **Server**.
4. V **IIS 7 doporučená konfigurace** řádku, klikněte na tlačítko **přidat**.
5. V <strong>Web Farm Framework 2.</strong> <em>x</em> řádku, klikněte na tlačítko <strong>přidat</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Klikněte na tlačítko **nainstalovat**. Všimněte si, že spolu s různými Další závislosti, nástroj pro nasazení webu do seznamu instalace přidal instalačního programu webové platformy.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Přečtěte si licenční podmínky a pokud souhlasíte s podmínkami, klikněte na tlačítko **souhlasím**.
8. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **3.0 Instalační služby webové platformy** okna.

## <a name="configure-the-primary-and-secondary-servers"></a>Nakonfigurujte primární a sekundární server

Před vytvořením serverové farmy WFF by měl provádět některé přípravy úlohy na webových serverech, které budou použity k vytvoření farmy:

- Přidat výjimky brány firewall umožňující **jádro sítě**, **pro vzdálenou správu**, a **sdílení souborů a tiskáren** funkce ke komunikaci se serverem WFF kontroleru .
- Vytvořte účet domény (například **FABRIKAM\stagingfarm**) ve službě Active Directory a přidejte ji do místní skupiny administrators na každém serveru. Použijete tento účet jako účet správce serverové farmy, při vytvoření serverové farmy.

Další informace o tom, jak nakonfigurovat tyto výjimky brány firewall v bráně Windows Firewall najdete v tématu [systému a požadavky na platformu pro Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805128). Pro jiné firewally naleznete v dokumentaci produktu.

Následující postup slouží k přidání doménového účtu do skupiny místních správců v systému Windows Server 2008 R2. Tento postup proveďte na každém serveru, který chcete přidat do farmy serverů&#x2014;jinými slovy, přidat stejný účet domény do místní skupiny administrators na primárním serveru a na každém sekundárním serveru.

**Přidání účtu domény do místní skupiny administrators**

1. Na **Start** nabídky, přejděte k **nástroje pro správu**a potom klikněte na tlačítko **správce serveru**.
2. V **správce serveru** rozbalte okno, v podokně se stromovým zobrazením, **konfigurace**, rozbalte **místní uživatelé a skupiny**a potom klikněte na tlačítko **skupiny**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. V **skupiny** podokně dvakrát klikněte na panel **správci**.
4. V **správci vlastnosti** dialogové okno, klikněte na tlačítko **přidat**.
5. V **vybrat uživatele, počítače, účty služby nebo skupiny** dialogové okno, zadejte (nebo vyhledejte) ke svému účtu domény (například **FABRIKAM\stagingfarm**) a potom klikněte na tlačítko **OK**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. V **správci vlastnosti** dialogové okno, klikněte na tlačítko **OK**.

Vaše servery jsou teď můžete přidat do serverové farmy. V případě primární server, můžete nakonfigurovat server podle požadavků vaší aplikace před nebo po vytvoření serverové farmy&#x2014;v obou případech platí, WFF budou synchronizovat servery nasazením stejné produkty, součásti nebo konfigurace na sekundárních serverech. Z důvodu zjednodušení tento kurz předpokládá, že po dokončení vytvoření serverové farmy nakonfigurujete primární server.

## <a name="create-the-wff-server-farm"></a>Vytvořit serverovou farmu WFF

Všechny servery jsou v tomto okamžiku můžete přidat do farmy serverů WFF:

- WFF nainstalovaný na serveru řadiče.
- Máte nakonfigurované výjimky brány firewall na primární a sekundární webové servery.
- Přidání účtu domény do místní skupiny administrators na primární a sekundární webové servery.

Dalším krokem je vytvoření serverové farmy v WFF. Provedete to ze Správce služby IIS na serveru řadiče WFF.

**Chcete-li vytvořit WFF serverové farmy**

1. Na serveru řadiče WFF na **Start** nabídky, přejděte k **nástroje pro správu**a potom klikněte na tlačítko **Správce Internetové informační služby (IIS)**.
2. V **připojení** podokně rozbalte uzel serveru místní, klikněte pravým tlačítkem na **serverových farem**a potom klikněte na tlačítko **vytvořit serverovou farmu**.
3. V **vytvořit serverovou farmu** dialogového okna zadejte smysluplný název serverové farmy (například **pracovní farmy**) a pak vyberte **zřídit serverovou farmu**.
4. Zadejte uživatelské jméno a heslo účtu domény, který jste přidali do místní skupiny administrators na každém serveru.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Klikněte na tlačítko **Další**.
6. Na **přidat servery** stránky, zadejte plně kvalifikovaný název domény (FQDN) primárního serveru, vyberte **primární Server**a potom klikněte na tlačítko **přidat**.
7. V tomto okamžiku WFF pokusí kontaktovat primární server pomocí přihlašovacích údajů, které jste zadali. Jestliže je připojení úspěšné, primární server se přidá do tabulky na **přidat servery** stránky.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Možná jste si všimli, který **Server je dostupný pro vyrovnávání zatížení** je standardně vybraná. WFF modulu Směrování žádostí na aplikace služby IIS používá k implementaci služby Vyrovnávání zatížení a tím distribuci požadavků mezi webovými servery v serverové farmě. Ve většině případů by pouze zrušte **Server je dostupný pro vyrovnávání zatížení** možnost, pokud chcete používat řešení místo toho Vyrovnávání zatížení třetích stran.
8. Na **přidat servery** stránky, zadejte plně kvalifikovaný název domény první sekundární server a potom klikněte na **přidat**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Krok 7 opakujte pro všechny další sekundární servery ve farmě a pak klikněte na tlačítko **Dokončit**.

Serverové farmy WFF je teď vytvořená a spuštěná. Všechny produkty webové platformy nebo komponenty, které nainstalujete na primární server a všechny webové aplikace nebo obsah, který nasadíte na primární server, se automaticky zřídí ve všech sekundárních serverech.

WFF je široké a složité téma a další informace o něm na [Microsoft Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805129) webu. Prozatím, ale existují dvě oblasti funkcí, které je potřeba mít na paměti:

- *Zřizování aplikací* je proces, který replikuje obsah z primárního serveru, jako jsou webové aplikace a nastavení konfigurace, přes všechny sekundární servery v serverové farmě. Například pokud vaše primární pracovní server nasazení ukázkové řešení Správce kontaktů, nasadí proces zřizování aplikace WFF toto řešení na všechny sekundární servery pracovní. Proces zřizování aplikací ve výchozím nastavení spouští každých 30 sekund.
- *Zřizování platformy* je proces, který synchronizuje produkty webové platformy a součásti z primárního serveru na všechny sekundární servery v serverové farmě. Například pokud instalujete na serveru primární pracovní ASP.NET MVC 3, použije během procesu zřizování platformy instalačního programu webové platformy nainstalovat technologie ASP.NET MVC 3 na všech serverech sekundární pracovní. Ve výchozím nastavení spouští každých 5 minut, během procesu zřizování platformy.

Můžete spravovat základní aplikace a nastavení zřizování platformy ze Správce služby IIS na serveru řadiče WFF.

**Prozkoumejte aplikace a nastavení zřizování platformy**

1. Ve Správci služby IIS v **připojení** podokně, vyberte serverovou farmu.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. V **serverovou farmu** podokně dvakrát klikněte na panel **zřizování aplikací**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Jak je vidět, serverová farma je nakonfigurován pro obsah a konfiguraci nastavení webu mezi primárním serverem a sekundární servery synchronizovat každých 30 sekund.
4. Klikněte na tlačítko **zpět**a potom dvakrát klikněte na panel **zřizování platformy**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Jak je vidět, serverová farma je nakonfigurován pro synchronizaci produkty webové platformy a komponenty mezi primárním serverem a sekundární servery pro každých pět minut.
6. Klikněte na tlačítko **zpět**.
7. Chcete vynutit okamžitou synchronizaci produkty webové platformy serverové farmy v **akce** podokně klikněte na tlačítko **zřídit platformu**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Zřizování platformy může nějakou dobu trvat. Proces instalačního programu běží na pozadí na sekundárních serverů v serverové farmě.
8. Jakmile jste povolili dostatek času na dokončení procesu zřizování, můžete ověřit, že produkty a komponenty, které jste přidali na primární server nyní replikují na sekundárních serverech. Například může přihlásit pomocí sekundárního serveru a používat **správce serveru** okno a ověřte, jestli je nainstalovaná role Webový server.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. Můžete také zkontrolovat seznamu nainstalovaných programů k ověření, že byly přidány různé komponenty webové platformy.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Konfigurace služby Vyrovnávání zatížení

Při vytváření webové farmy, budete muset nastavit nějakou formu k distribuci požadavků HTTP mezi webovými servery Vyrovnávání zatížení. To může být Windows Server 2008 Vyrovnávání zatížení sítě, směrování žádostí na aplikace služby IIS, nebo třetích stran softwarové nebo hardwarové Vyrovnávání zatížení řešení.

WFF slouží k integraci úzce s IIS ARR. Umožní využít této integrace budete muset nainstalovat modul směrování žádostí na aplikace na serveru řadiče WFF. Můžete potom směrovat veškerý provoz web na serveru řadiče obvykle pomocí konfigurace záznamů systému DNS (Domain Name). Kontroler serveru se pak distribuovat příchozí požadavky mezi servery ve farmě, na základě dostupnosti serveru a různých dalších kritérií.

> [!NOTE]
> Není nutné používat WFF; směrování žádostí na aplikace můžete nakonfigurovat WFF pro práci s řešení vyrovnávání zatížení třetích stran. Další informace najdete v tématu [přehled Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805126).


Vyrovnávání zatížení pomocí směrování žádostí na aplikace je složité téma, většina z nich je nad rámec tohoto kurzu. Můžete ale následující postup k instalaci modulu Směrování žádostí na aplikace a začít pracovat se službou Vyrovnávání zatížení.

**Chcete-li nastavit na serveru řadiče WFF Vyrovnávání zatížení**

1. Na serveru řadiče WFF spuštění instalačního programu webové platformy.
2. V horní části **3.0 Instalační služby webové platformy** okna, klikněte na tlačítko **produkty**.
3. Na levé straně okna, v navigačním podokně klikněte na tlačítko **Server**.
4. V **aplikaci požádat o 2.5 směrování** řádku, klikněte na tlačítko **přidat**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Klikněte na tlačítko **nainstalovat**a pak postupujte podle pokynů **instalace webové platformy** okna.
6. Po dokončení instalace spusťte Správce služby IIS a **připojení** podokně klikněte na uzel serveru farmy. Všimněte si, že byla do několika nových ikon **serverovou farmu** podokně.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. V **serverovou farmu** podokně dvakrát klikněte na panel **nástroj pro vyrovnávání zatížení**.
8. V **Vyrovnávání zatížení** podokně, vyberte zatížení vyrovnávat algoritmus (například **nejméně aktuální požadavek**).

    > [!NOTE]
    > Další informace o algoritmech a dalších nastavení konfigurace pro vyrovnávání zatížení najdete v tématu [aplikaci požádat o modulu Směrování](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. V **akce** podokně klikněte na tlačítko **použít**.

Nyní jste nakonfigurovali základní Vyrovnávání zatížení pro servery v serverové farmě. Pokud jste směrovat veškerý provoz webové farmy na serveru řadiče, požadavky distribuovány mezi servery ve farmě podle dostupnosti a algoritmus Vyrovnávání zatížení, které jste vybrali.

Další informace o tom, jak nakonfigurovat Vyrovnávání zatížení se směrováním žádostí na aplikace najdete v tématu [aplikaci požádat o modulu Směrování](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitorování serverové farmy

Monitorování stavu serverové farmě v okamžiku pomocí Správce služby IIS na serveru řadiče. V **připojení** podokně rozbalte serverové farmy a pak klikněte na tlačítko **servery**. V prostředním podokně se zobrazí souhrn každého serveru ve farmě společně s poslední aktivity protokolu trasování.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Závěr

Serverové farmy WFF by teď zprovozněný. Můžete nakonfigurovat primární server pro podporu v obou případech nasazení dáváte přednost&#x2014;naleznete v části Další čtení podrobností&#x2014;a konfiguraci se budou replikovat na každém sekundárním serveru v serverové farmě.

## <a name="further-reading"></a>Další čtení

Další informace o všech aspektech konfiguraci a použití WFF najdete v tématu [Microsoft Web Farm Framework 2.0 pro službu IIS 7](https://go.microsoft.com/?linkid=9805129) webu.

> [!div class="step-by-step"]
> [Předchozí](configuring-a-database-server-for-web-deploy-publishing.md)
> [další](configuring-deployment-properties-for-a-target-environment.md)
