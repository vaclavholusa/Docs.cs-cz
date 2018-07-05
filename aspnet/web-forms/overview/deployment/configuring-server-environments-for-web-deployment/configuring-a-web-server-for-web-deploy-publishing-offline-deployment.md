---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: Konfigurace webového serveru pro webové nasazení publikování (Offline nasazení) | Dokumentace Microsoftu
author: jrjlee
description: Toto téma popisuje, jak nakonfigurovat webový server služby IIS pro podporu nasazení a publikování na webu v režimu offline. Při práci s Internetová informační služba (I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: 555b4b3c79d8efc641b1c179482993371735dbe2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397574"
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Konfigurace webového serveru pro publikování (Offline nasazení) pro nasazení webu
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nakonfigurovat webový server služby IIS pro podporu nasazení a publikování na webu v režimu offline.
> 
> Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (nasazení webu) 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání aplikací nebo webů na webovém serveru. Můžeš:
> 
> - Použití *Webdeploy služba vzdáleného agenta*. Tento přístup vyžaduje méně konfiguraci webového serveru, ale je potřeba zadat přihlašovací údaje správce místním serveru, aby se nenasazujte nic k serveru.
> - Použití *obslužná rutina nasazení webu*. Tento přístup je mnohem složitější a vyžaduje další úsilí počáteční nastavení webového serveru. Ale při použití tohoto přístupu můžete nakonfigurovat služby IIS umožňuje uživatelům bez oprávnění správce k provedení nasazení. Obslužné rutiny nasazení webu je pouze k dispozici ve službě IIS verze 7 nebo novější.
> - Použití *offline nasazení*. Tento přístup vyžaduje nejmenší míru konfigurace webového serveru, ale správce serveru musíte ručně zkopírovat webový balíček na server a importujte ho pomocí Správce služby IIS.
> 
> Další informace o klíčové funkce, výhody a nevýhody těchto přístupů, naleznete v tématu [výběr právo přístupu k nasazení webu](choosing-the-right-approach-to-web-deployment.md).


Ano, pokud vaše síťové infrastruktury nebo zabezpečení omezení brání vzdálené nasazení. Příčinou je pravděpodobně v případě v produkčních prostředích přístupem k Internetu, ve kterém jsou webové servery izolované&#x2014;fyzicky nebo pomocí brány firewall a podsítě&#x2014;od zbytku serverové infrastruktury.

Samozřejmě tento přístup se změní na méně žádoucí, pokud vaše webové aplikace se aktualizují v pravidelných intervalech. Pokud vaše infrastruktura umožňuje, můžete zvážit povolení vzdáleného nasazení pomocí obslužné rutiny nasazení webu nebo webové nasazení vzdáleného agenta služby.

## <a name="task-overview"></a>Přehled úloh

Pokud chcete nakonfigurovat webový server pro podporu offline importu a nasazení webových balíčků, bude nutné:

- Nainstalujte službu IIS 7.5 a IIS 7 doporučená konfigurace.
- Nainstalujte nástroj nasazení webu 2.1 nebo novější.
- Můžete vytvořte web služby IIS pro hostování nasazeným obsahem.
- Zakážete agenta nasazení webové služby.

Pro hostování ukázkové řešení konkrétně, bude také potřeba:

- Instalace rozhraní .NET Framework 4.0.
- Instalace technologie ASP.NET MVC 3.

Postup pro každý z těchto postupů se zobrazí v tomto tématu. Úlohy a názorné postupy v tomto tématu se předpokládá, že začínáte s čistou server sestavení s Windows serverem 2008 R2. Než budete pokračovat, ujistěte se, že:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace jsou nainstalovány.
- Server není připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně najdete v tématu [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurujte statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Nainstalovat produkty a komponent

Tato část vás provede s instalací požadovaných produktů a komponenty na webovém serveru. Než začnete, je vhodné spuštěním služby Windows Update tak, aby byl váš server plně aktuálním stavu.

V takovém případě musíte nainstalovat tyto věci:

- **Doporučená konfigurace služby IIS 7**. Díky tomu **webového serveru (IIS)** role na webovém serveru a instalaci sadu modulů IIS a komponenty, které potřebujete k hostování aplikace ASP.NET.
- **Rozhraní .NET framework 4.0**. To je potřeba ke spouštění aplikací, které byly vytvořeny v této verzi rozhraní .NET Framework.
- **Webové nástroje pro nasazení 2.1 nebo novější**. Tím se nainstaluje Web Deploy (a její podkladové spustitelný soubor MSDeploy.exe) na serveru. Nástroj nasazení webu se službou IIS a umožňuje importovat a exportovat webových balíčků.
- **ASP.NET MVC 3**. Tím se nainstaluje sestavení, je potřeba spouštět aplikace MVC 3.

> [!NOTE]
> Tento návod popisuje použití instalačního programu webové platformy nainstalovat a nakonfigurovat různé součásti. I když není nutné používat instalačního programu webové platformy, zjednodušuje proces instalace automaticky zjišťuje závislosti a zajištění vždycky toho nejnovější verze produktu. Další informace najdete v tématu [Microsoft webové platformy verze 3.0](https://go.microsoft.com/?linkid=9805118).


**Chcete-li nainstalovat požadované produkty a komponenty**

1. Stáhněte a nainstalujte [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).
2. Po dokončení instalace, automaticky se spustí instalace webové platformy.

    > [!NOTE]
    > Instalace webové platformy teď můžete spustit kdykoli **Start** nabídky. K tomu, na **Start** nabídky, klikněte na tlačítko **všechny programy**a potom klikněte na tlačítko **instalačního programu webové platformy Microsoft**.
3. V horní části **3.0 Instalační služby webové platformy** okna, klikněte na tlačítko **produkty**.
4. Na levé straně okna, v navigačním podokně klikněte na tlačítko **architektury**.
5. V **rozhraní Microsoft .NET Framework 4** řádek, pokud rozhraní .NET Framework není nainstalovaná, klikněte na tlačítko **přidat**.

    > [!NOTE]
    > Možná jste již nainstalovali .NET Framework 4.0 prostřednictvím služby Windows Update. Pokud už je nainstalován produkt nebo komponenty, instalačního programu webové platformy se označit to tak, že nahradíte **přidat** tlačítko s textem **nainstalováno**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. V **ASP.NET MVC 3 (Visual Studio 2010)** řádku, klikněte na tlačítko **přidat**.
7. V navigačním podokně klikněte na tlačítko **Server**.
8. V **IIS 7 doporučená konfigurace** řádku, klikněte na tlačítko **přidat**.
9. V **2.1 nástroj Web Deployment** řádku, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů&#x2014;spolu s případnými přidružené závislosti&#x2014;k instalaci a zobrazí výzvu k potvrzení licenčních podmínek.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Přečtěte si licenční podmínky a pokud souhlasíte s podmínkami, klikněte na tlačítko **souhlasím**.
12. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **3.0 Instalační služby webové platformy** okna.

Pokud jste nainstalovali .NET Framework 4.0, před instalací služby IIS, budete potřebovat ke spuštění [ASP.NET IIS Registration Tool](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) na nejnovější verzi technologie ASP.NET pro službu IIS zaregistrovat. Pokud to neuděláte, zjistíte, že služba IIS bude poskytovat statický obsah (jako jsou soubory HTML) bez problémů, ale vrátí **HTTP Chyba 404.0 – nenalezeno** při pokusu o procházení obsahu ASP.NET. Následující postup slouží k zajištění, že je zaregistrované technologii ASP.NET 4.0.

**Chcete-li službu IIS zaregistrovat technologii ASP.NET 4.0**

1. Klikněte na tlačítko **Start**a pak zadejte **příkazový řádek**.
2. Ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku**a potom klikněte na tlačítko **spustit jako správce**.
3. V okně příkazového řádku, přejděte **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** adresáře.
4. Zadejte následující příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Pokud máte v plánu hostování 64-bit webových aplikací v libovolném bodě, byste měli také zaregistrovat 64bitovou verzi technologie ASP.NET se službou IIS. Chcete-li to provést, v okně příkazového řádku, přejděte na **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** adresáře.
6. Zadejte následující příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Dobrým postupem je Windows Update znovu použijte v tuto chvíli ke stažení a instalaci dostupných aktualizací pro nové produkty a komponenty, které jste nainstalovali.

## <a name="configure-the-iis-website"></a>Konfigurovat web služby IIS

Před nasazením webového obsahu na serveru, budete muset vytvořit a nakonfigurovat web služby IIS pro hostování daného obsahu. Nástroj nasazení webu nasadit jenom webových balíčků na existující web služby IIS; na webu jej nelze vytvořit za vás. Na vysoké úrovni budete potřebovat k dokončení těchto úloh:

- Vytvořte složku v systému souborů pro hostování vašeho obsahu.
- Vytvoření webu služby IIS k poskytování obsahu a přidružit ho k místní složce.
- Udělit oprávnění k identitě fondu aplikací v místním adresáři čtení.

I když není nic zastavení jste od nasazení obsahu na výchozí web ve službě IIS, tento přístup není doporučen pro ostatní formáty vyjma testu nebo ukázkové scénáře. Pokud chcete simulovat produkční prostředí, měli byste vytvořit nový web služby IIS s nastavením, které jsou specifické pro požadavky vaší aplikace.

**Vytvoření a konfigurace webu služby IIS**

1. V místním systému souborů, vytvořte složku pro uložení obsahu (například **C:\DemoSite**).
2. Na **Start** nabídky, přejděte k **nástroje pro správu**a potom klikněte na tlačítko **Správce Internetové informační služby (IIS)**.
3. Ve Správci služby IIS v **připojení** podokně rozbalte uzel serveru (například **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Klikněte pravým tlačítkem myši **lokality** uzel a potom klikněte na **přidat web**.
5. V **název lokality** zadejte název webu služby IIS (například **DemoSite**).
6. V **fyzická cesta** zadejte (nebo vyhledejte) cestu k místní složce (třeba **C:\DemoSite**).
7. V **Port** zadejte číslo portu, na kterém chcete hostovat web (například **85**).

    > [!NOTE]
    > Jsou čísla standardní port 80 pro protokol HTTP a 443 pro protokol HTTPS. Nicméně pokud hostujete tohoto webu na portu 80, musíte zastavit výchozí web pro přístup k webu.
8. Nechte **název hostitele** pole prázdné, pokud chcete nakonfigurovat záznam systému DNS (Domain Name) pro web a potom klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > V produkčním prostředí budete pravděpodobně chtít hostovat svůj web na portu 80 a nakonfigurovat hlavičku hostitele, společně s odpovídající záznamy DNS. Další informace o konfiguraci hlavičky hostitele ve službě IIS 7, najdete v části [konfigurace hlavičku hostitele pro webový server (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Další informace o roli serveru DNS ve Windows serveru 2008 R2 najdete v tématu [přehled serveru DNS](https://technet.microsoft.com/en-gb/library/cc770392.aspx) a [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).
9. V **akce** podokně v části **upravit web**, klikněte na tlačítko **vazby**.
10. V **vazby webu** dialogové okno, klikněte na tlačítko **přidat**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. V **přidat vazbu webu** dialogové okno, nastavte **IP adresu** a **Port** tak, aby odpovídaly vaší existující konfiguraci lokality.
12. V **název hostitele** zadejte název webového serveru (například **PROWEB1**) a potom klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > První vazba webu umožňuje přístup k serveru místně s použitím IP adresy a portu nebo `http://localhost:85`. Druhá vazba webu umožňuje přístup k webu z jiných počítačů v doméně pomocí názvu počítače (například http://proweb1:85).
13. V **vazby webu** dialogové okno, klikněte na tlačítko **Zavřít**.
14. V **připojení** podokně klikněte na tlačítko **fondy aplikací**.
15. V **fondy aplikací** podokně klikněte pravým tlačítkem na název vašeho fondu aplikací a klikněte na **základní nastavení**. Ve výchozím nastavení, bude název fondu aplikací odpovídat názvu vašeho webu (například **DemoSite**).
16. V **verzi rozhraní .NET Framework** seznamu vyberte **rozhraní .NET Framework v4.0.30319**a potom klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Ukázkové řešení vyžaduje rozhraní .NET Framework 4.0. Toto není povinné pro nasazení webu obecně.

V pořadí pro váš web k poskytování obsahu identita fondu aplikací musí mít oprávnění ke čtení na místní složku, která ukládá obsah. Ve službě IIS 7.5 fondy aplikací s identitou fondu jedinečné ve výchozím nastavení spouští (na rozdíl od předchozích verzí služby IIS, kde by fondy aplikací obvykle běží účtem Network Service). Identita fondu aplikací není skutečný účet a není uveden na všechny seznamy uživatelů nebo skupin&#x2014;místo toho se vytvoří v dynamicky při spuštění fondu aplikací. Každá identita fondu aplikací se přidá do místní **IIS\_IUSRS** skupiny zabezpečení jako skrytá položka.

Udělení oprávnění identitě fondu aplikací pro soubor nebo složku, máte dvě možnosti:

- Přiřazení oprávnění pro identitu fondu aplikací přímo, pomocí formátu <strong>fondu aplikací služby IIS\</ strong ><em>[název fondu aplikací]</em>(například <strong>IIS AppPool\DemoSite</strong>).
- Přidělování oprávnění k **IIS\_IUSRS** skupiny.

Nejběžnější přístup je k přiřazení oprávnění pro místní **IIS\_IUSRS** skupiny, protože tento přístup umožňuje změnit fondy aplikací bez nutnosti měnit oprávnění systému souborů. Následující postup používá tento přístup na základě skupin.

> [!NOTE]
> Další informace o identity fondu aplikací ve službě IIS 7.5, naleznete v tématu [identity fondu aplikací součásti](https://go.microsoft.com/?linkid=9805123).


**Jak nakonfigurovat oprávnění složky pro web služby IIS**

1. V Průzkumníku Windows přejděte do umístění vaší místní složky.
2. Klikněte pravým tlačítkem na složku a potom klikněte na tlačítko **vlastnosti**.
3. Na **zabezpečení** klikněte na tlačítko **upravit**a potom klikněte na tlačítko **přidat**.
4. Klikněte na tlačítko **umístění**. V **umístění** dialogové okno, vyberte na místním serveru a potom klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. V **vybrat uživatele nebo skupiny** dialogovém okně **IIS\_IUSRS**, klikněte na tlačítko **Kontrola názvů**a potom klikněte na tlačítko **OK**.
6. V <strong>oprávnění pro</strong><em>[název složky]</em> dialogové okno, Všimněte si, že byla přiřazena nová skupina <strong>čtení &amp; provést</strong>, <strong>vypsat složky obsah</strong>, a <strong>čtení</strong> oprávnění ve výchozím nastavení. Nechte beze změny a klikněte na tlačítko <strong>OK</strong>.
7. Klikněte na tlačítko <strong>OK</strong> zavřete <em>[název složky]</em><strong>vlastnosti</strong> dialogové okno.

## <a name="disable-the-remote-agent-service"></a>Zakázat službu vzdáleného agenta

Při instalaci Web Deploy Agent služba pro nasazení webu je nainstalována a spuštěna automaticky. Tato služba umožňuje nasazení a publikování webových balíčků ze vzdáleného umístění. Nebudete používat funkci vzdálené nasazení v tomto scénáři, měli byste zastavit a zakázat službu.

> [!NOTE]
> Není potřeba zastavit službu vzdáleného agenta k importu a nasazení webového balíčku ručně. Je však vhodné zastavit a zakázat službu, pokud nemáte v úmyslu použít.


Můžete zastavit a zakázat službu v několika ohledech pomocí různých nástrojů příkazového řádku nebo pomocí rutin prostředí Windows PowerShell. Tento postup popisuje jednoduchým řešením by na základě uživatelského rozhraní.

**Zastavení a zakázání služby vzdáleného agenta**

1. Na **Start** nabídky, přejděte k **nástroje pro správu**a potom klikněte na tlačítko **služby**.
2. V konzole služby, vyhledejte **webová služba agenta nasazení** řádek.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Klikněte pravým tlačítkem na **webová služba agenta nasazení**a potom klikněte na tlačítko **vlastnosti**.
4. V **vlastnosti webové nasazení agenta služby** dialogové okno, klikněte na tlačítko **Zastavit**.
5. V **typ spouštění** seznamu vyberte **zakázané**a potom klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Závěr

Webový server je v tomto okamžiku připravené pro offline webového balíčku nasazení. Než se pokusíte importovat webových balíčků na web služby IIS, můžete chtít zkontrolovat tyto klíčové body:

- Jste zaregistrovali ASP.NET 4.0 se službou IIS?
- Má identita fondu aplikací přístup pro čtení ke zdrojové složce pro váš web?
- Je nutné zastavit službu webové nasazení agenta?

> [!div class="step-by-step"]
> [Předchozí](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
> [další](configuring-a-database-server-for-web-deploy-publishing.md)
