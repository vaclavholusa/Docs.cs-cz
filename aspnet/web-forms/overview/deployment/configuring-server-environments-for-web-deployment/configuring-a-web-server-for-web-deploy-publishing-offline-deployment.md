---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
title: "Konfigurace webového serveru pro Web nasazení publikování (Offline nasazení) | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje, jak nakonfigurovat webový server služby IIS pro podporu nasazení a publikování na webu do režimu offline. Při práci s Internetová informační služba (I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: ba92788f-9f03-44b1-b6b2-af8413e6a35d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-offline-deployment
msc.type: authoredcontent
ms.openlocfilehash: cd3343f58cbb9bb868d15a91152f07444c2bd68e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="configuring-a-web-server-for-web-deploy-publishing-offline-deployment"></a>Konfigurace webového serveru pro nasazení webu publikování (Offline nasazení)
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje, jak nakonfigurovat webový server služby IIS pro podporu nasazení a publikování na webu do režimu offline.
> 
> Při práci s Internetové informační služby (IIS) nástroj pro nasazení webu (Web Deploy) 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání aplikací nebo webů na webovém serveru. Můžeš:
> 
> - Použití *vzdáleného agenta služby nasazení webu*. Tento postup vyžaduje méně konfigurace webového serveru, ale budete muset zadat přihlašovací údaje správce místním serveru za účelem nasazení nic k serveru.
> - Použití *obslužné rutiny pro nasazení webu*. Tento přístup je mnohem složitější a vyžaduje další úsilí počáteční nastavení webového serveru. Pokud použijete tuto metodu, ale můžete nakonfigurovat služby IIS, aby uživatelé bez oprávnění správce k provedení nasazení. Obslužné rutiny nasazení webu je pouze k dispozici ve službě IIS 7 nebo novější verze.
> - Použití *offline nasazení*. Tento přístup, vyžaduje nejmenší míru konfigurace webového serveru, ale správce serveru musíte ručně zkopírovat webového balíčku na server a importujte ji pomocí Správce služby IIS.
> 
> Další informace o klíčové funkce, výhody a nevýhody těchto přístupů, najdete v části [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md).


Ano, pokud vaše síťové infrastruktury nebo zabezpečení omezení brání vzdálené nasazení. Příčinou je pravděpodobně být velká písmena v internetového provozní prostředí, kde jsou webové servery izolované & #x 2014; buď fyzicky nebo pomocí brány firewall a podsítě & #x 2014; zbytek serverové infrastruktury.

Tento přístup stane samozřejmě méně vhodné, pokud vaše webové aplikace se aktualizují v pravidelných intervalech. Pokud vaše infrastruktura dovoluje, můžete zvážit povolení vzdáleného nasazení pomocí obslužné rutiny nasazení webu nebo webové nasazení vzdáleného agenta služby.

## <a name="task-overview"></a>Přehled úloh

Konfigurace webového serveru pro podporu offline importu a nasazení webových balíčků, musíte:

- Nainstalujte službu IIS 7.5 a IIS 7 doporučená konfigurace.
- Instalovat nasazení webu 2.1 nebo vyšší.
- Vytvoření webu IIS hostovat nasazený obsah.
- Zakažte agenta služba pro nasazení webu.

Pro hostování ukázkové řešení konkrétně, bude také nutné:

- Nainstalujte rozhraní .NET Framework 4.0.
- Nainstalujte rozhraní ASP.NET MVC 3.

Toto téma vám ukáže, jak provést všechny tyto postupy. Úlohy a postupy v tomto tématu předpokládat, že začínáte s čistou serveru sestavení systémem Windows Server 2008 R2. Než budete pokračovat, zajistěte, aby:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.
- Server je připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně, najdete v části [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/en-us/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurovat statickou IP adresu](https://technet.microsoft.com/en-us/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Nainstalujte produkty a součásti

Tato část vás provede instalací požadovaných produktů a součásti na webovém serveru. Než začnete, je vhodné spustit službu Windows Update zajistit, že váš server je zcela aktuální.

V takovém případě musíte nainstalovat tyto věci:

- **Doporučená konfigurace služby IIS 7**. Díky tomu **webového serveru (IIS)** role na vašem webovém serveru a nainstaluje sadu moduly služby IIS a součásti, které potřebujete k hostování aplikace ASP.NET.
- **Rozhraní .NET framework 4.0**. To je nutné ke spuštění aplikací, které byly vytvořené v této verzi rozhraní .NET Framework.
- **Webové nástroje pro nasazení 2.1 nebo vyšší**. Tím se nainstaluje Web Deploy (a její základní spustitelný soubor MSDeploy.exe) na serveru. Nasazení webu se službou IIS a umožňuje importovat a exportovat webových balíčků.
- **ASP.NET MVC 3**. Tím se nainstaluje sestavení, budete muset spustit aplikace MVC 3.

> [!NOTE]
> Tento návod popisuje použití instalačního programu webové platformy nainstalovat a nakonfigurovat různé součásti. I když nemáte pomocí služby instalace webové platformy, zjednodušuje proces instalace automaticky zjišťuje závislosti a zajistíte, že vždy dostanete nejnovější verze produktu. Další informace najdete v tématu [Microsoft webové platformy verze 3.0](https://go.microsoft.com/?linkid=9805118).


**K instalaci požadovaných produktů a součásti**

1. Stáhněte a nainstalujte [instalačního programu webové platformy](https://go.microsoft.com/?linkid=9805118).
2. Po dokončení instalace webové platformy se spustí automaticky.

    > [!NOTE]
    > Instalace webové platformy teď můžete spustit kdykoli **spustit** nabídky. To uděláte, na **spustit** nabídky, klikněte na tlačítko **všechny programy**a potom klikněte na **instalačního programu webové platformy Microsoft**.
3. V horní části **webové platformy verze 3.0** okně klikněte na tlačítko **produkty**.
4. Na levé straně okna, v navigačním podokně klikněte na tlačítko **rozhraní**.
5. V **rozhraní Microsoft .NET Framework 4** řádek, pokud již není nainstalována rozhraní .NET Framework, klikněte na tlačítko **přidat**.

    > [!NOTE]
    > Může po instalaci rozhraní .NET Framework 4.0 prostřednictvím služby Windows Update. Pokud produkt nebo součást je již nainstalován, instalace webové platformy označí to nahrazením **přidat** tlačítka s textem **nainstalovaná**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image1.png)
6. V **ASP.NET MVC 3 (Visual Studio 2010)** řádek, klikněte na tlačítko **přidat**.
7. V navigačním podokně klikněte na tlačítko **Server**.
8. V **IIS 7 doporučená konfigurace** řádek, klikněte na tlačítko **přidat**.
9. V **2.1 nástroj pro nasazení webového** řádek, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů & #x 2014; společně s všechny přidružené závislosti & #x 2014; nainstalována a zobrazí výzvu k potvrzení licenčních podmínek.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image2.png)
11. Přečtěte si licenční podmínky a pokud vyjadřujete svůj souhlas s podmínkami, klikněte na **souhlasím**.
12. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **webové platformy verze 3.0** okno.

Pokud jste nainstalovali rozhraní .NET Framework 4.0, před instalací služby IIS, budete muset spustit [registrační nástroj](https://msdn.microsoft.com/en-us/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) pro službu IIS zaregistrovat nejnovější verzi technologie ASP.NET. Pokud to neuděláte, zjistíte, že služba IIS bude poskytovat statický obsah (jako jsou soubory HTML) bez problémů, ale vrátí **HTTP Chyba 404.0 – nenalezeno** při pokusu přejděte do obsahu ASP.NET. Následující postup vám pomůže zajistit, aby je zaregistrovány technologii ASP.NET 4.0.

**Chcete-li službu IIS zaregistrovat technologii ASP.NET 4.0**

1. Klikněte na tlačítko **spustit**a pak zadejte **příkazového řádku**.
2. Ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku**a potom klikněte na **spustit jako správce**.
3. V okně příkazového řádku přejděte do **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** adresáře.
4. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample1.cmd)]
5. Pokud budete chtít hostitele 64-bit webové aplikace v libovolném bodě, byste měli také zaregistrovat 64bitovou verzi technologie ASP.NET se službou IIS. Chcete-li to provést, v okně příkazového řádku, přejděte na **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** adresáře.
6. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/samples/sample2.cmd)]

Jako osvědčených postupů pomocí služby Windows Update znovu v tomto okamžiku stáhnout a nainstalovat všechny dostupné aktualizace pro novou produkty a součásti, které jste nainstalovali.

## <a name="configure-the-iis-website"></a>Konfigurovat web služby IIS

Před nasazením webového obsahu na server, budete muset vytvořit a nakonfigurovat web služby IIS pro hostování daného obsahu. Nasazení webu lze nasadit pouze webových balíčků na existující web služby IIS; můžete ji nelze vytvořit web. Na vysoké úrovni budete potřebovat k dokončení těchto úloh:

- Vytvořte složku v systému souborů hostovat obsah.
- Vytvoření webu IIS k obsluze obsah a přidružte ji k místní složce.
- Udělení oprávnění ke čtení pro identitu fondu aplikací do místní složky.

I když není co můžete zastavit z nasazení obsahu na výchozí web ve službě IIS, tento postup se nedoporučuje pro jakoukoli jinou hodnotu než testu nebo ukázkový scénáře. Pro simulaci provozním prostředí, měli byste vytvořit nový web služby IIS s nastavením, které jsou specifické pro požadavky vaší aplikace.

**Chcete-li vytvořit a nakonfigurovat web služby IIS**

1. V místním systému souborů, vytvořte složku pro uložení obsahu (například **C:\DemoSite**).
2. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **Správce Internetové informační služby (IIS)**.
3. Ve Správci služby IIS v **připojení** podokně rozbalte uzel serveru (například **PROWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image3.png)
4. Klikněte pravým tlačítkem myši **lokality** uzel a pak klikněte na tlačítko **přidat web**.
5. V **název lokality** zadejte název pro web služby IIS (například **DemoSite**).
6. V **fyzická cesta** zadejte (nebo vyhledejte) cestu do místní složky (například **C:\DemoSite**).
7. V **Port** zadejte číslo portu, na kterém chcete hostovat na webu (například **85**).

    > [!NOTE]
    > Standardní port čísla jsou 80 pro protokol HTTP a 443 pro protokol HTTPS. Ale pokud hostujete tento web na portu 80, musíte zastavit výchozího webu, než se dostanete k vaší lokality.
8. Ponechte **název hostitele** pole prázdné, pokud chcete nakonfigurovat záznam systému DNS (Domain Name) pro web a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image4.png)

    > [!NOTE]
    > V produkčním prostředí budete pravděpodobně chtít hostování vašeho webu na portu 80 a Konfigurace hlavičky hostitele, společně s odpovídající záznamy DNS. Další informace o konfiguraci hlavičky hostitele ve službě IIS 7 najdete v tématu [konfigurace hlavičku hostitele pro webový server (IIS 7)](https://technet.microsoft.com/en-us/library/cc753195(WS.10).aspx). Další informace o roli serveru DNS v systému Windows Server 2008 R2 najdete v tématu [přehled systému DNS Server](https://technet.microsoft.com/en-gb/library/cc770392.aspx) a [DNS Server](https://technet.microsoft.com/en-us/windowsserver/dd448607).
9. V **akce** podokně v části **upravit web**, klikněte na tlačítko **vazby**.
10. V **vazby webu** dialogové okno, klikněte na tlačítko **přidat**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image5.png)
11. V **přidat vazbu webu** dialogové okno, sada **IP adresu** a **Port** tak, aby odpovídaly vaší stávající konfigurace lokality.
12. V **název hostitele** zadejte název webového serveru (například **PROWEB1**) a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image6.png)

    > [!NOTE]
    > První vazba webu umožňuje přístup k webu místně pomocí IP adresy a portu nebo `http://localhost:85`. Druhé vazby webu umožňuje přístup k webu z jiných počítačů v doméně pomocí názvu počítače (například http://proweb1:85).
13. V **vazby webu** dialogové okno, klikněte na tlačítko **Zavřít**.
14. V **připojení** podokně klikněte na tlačítko **fondy aplikací**.
15. V **fondy aplikací** podokně klikněte pravým tlačítkem na název vašeho fondu aplikací a pak klikněte na tlačítko **základní nastavení**. Ve výchozím nastavení, bude název vašeho fondu aplikací shodovat s názvem vašeho webu (například **DemoSite**).
16. V **verze rozhraní .NET Framework** seznamu, vyberte **rozhraní .NET Framework v4.0.30319**a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image7.png)

    > [!NOTE]
    > Ukázkové řešení vyžaduje rozhraní .NET Framework 4.0. Toto není požadavek pro nasazení webu obecně.

V pořadí pro svůj web pro práci s obsahem identita fondu aplikací musí mít oprávnění ke čtení místní složky, která uloží obsah. Ve službě IIS 7.5 spusťte fondy aplikací s identitou fondu aplikací jedinečný ve výchozím nastavení (na rozdíl od předchozí verze služby IIS, kde by fondy aplikací obvykle běží pod účtem síťové služby). Identita fondu aplikací není skutečné uživatelský účet a nezobrazuje na všechny seznamy uživatelů nebo skupin & #x 2014; místo toho je vytvořen v dynamicky při spuštění fondu aplikací. Každý identita fondu aplikací se přidá do místní **IIS\_IUSRS** skupiny zabezpečení jako skrytá položka.

Abyste mohli udělit oprávnění k souboru nebo složce identity fondu aplikací, máte dvě možnosti:

- Přiřadit oprávnění k identitě fondu aplikací přímo, formátu **IIS AppPool\***[název fondu aplikací] * (například **IIS AppPool\DemoSite**).
- Přidělování oprávnění k **IIS\_IUSRS** skupiny.

Většina běžný postup je přiřadit oprávnění k místní **IIS\_IUSRS** skupiny, protože tento přístup umožňuje změnit fondy aplikací bez nutnosti měnit oprávnění systému souborů. Následující postup používá tento přístup na základě skupiny.

> [!NOTE]
> Další informace o identity fondu aplikací ve službě IIS 7.5, najdete v části [identity fondu aplikací součásti](https://go.microsoft.com/?linkid=9805123).


**Konfigurace složky oprávnění pro web služby IIS**

1. V Průzkumníku Windows přejděte do umístění místní složky.
2. Klikněte pravým tlačítkem složku a pak klikněte na **vlastnosti**.
3. Na **zabezpečení** , klikněte na **upravit**a potom klikněte na **přidat**.
4. Klikněte na tlačítko **umístění**. V **umístění** dialogové okno, vyberte místního serveru a pak klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image8.png)
5. V **vybrat uživatele nebo skupiny** dialogové okno, typ **IIS\_IUSRS**, klikněte na tlačítko **Kontrola názvů**a potom klikněte na **OK**.
6. V **oprávnění pro***[název složky]* dialogové okno, Všimněte si, že byl přiřazen do nové skupiny **čtení &amp; provést**, **složku seznamu obsah**, a **čtení** oprávnění ve výchozím nastavení. Nechte beze změny a klikněte na tlačítko **OK**.
7. Klikněte na tlačítko **OK** zavřete *[název složky]***vlastnosti** dialogové okno.

## <a name="disable-the-remote-agent-service"></a>Zakázat službu vzdáleného agenta

Při instalaci nástroje nasazení webu, Služba agenta pro nasazení webu je nainstalována a spuštěna automaticky. Tato služba umožňuje nasazovat a publikovat web balíčky ze vzdáleného umístění. Nebudete používat funkce vzdálené nasazení v tomto scénáři, abyste měli zastavit a zakázat službu.

> [!NOTE]
> Nemusíte zastavit službu vzdáleného agenta, abyste mohli importovat a nasadit balíček web ručně. Ale je dobrým zvykem zastavení a zakázání služby, pokud nemáte v úmyslu použít.


Můžete zastavit a zakázat službu několika způsoby, pomocí různých nástrojů příkazového řádku nebo rutiny prostředí Windows PowerShell. Tento postup popisuje Nejjednodušším způsobem na základě uživatelského rozhraní.

**Zastavení a zakázání služby vzdáleného agenta**

1. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **služby**.
2. V konzole služby, vyhledejte **Služba agenta pro nasazení webu** řádek.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image9.png)
3. Klikněte pravým tlačítkem na **Služba agenta pro nasazení webu**a potom klikněte na **vlastnosti**.
4. V **vlastnosti webové nasazení agenta služby** dialogové okno, klikněte na tlačítko **Zastavit**.
5. V **typ spuštění** seznamu, vyberte **zakázané**a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-offline-deployment/_static/image10.png)

## <a name="conclusion"></a>Závěr

V tomto okamžiku je připraven pro nasazení balíčku offline webové váš webový server. Před provedením importu webových balíčků na web služby IIS, můžete zkontrolovat tyto klíčové body:

- Jste zaregistrovali technologii ASP.NET 4.0 IIS?
- Má identita fondu aplikací přístup pro čtení ke zdrojové složce pro svůj web?
- Je nutné zastavit službu webové nasazení agenta?

>[!div class="step-by-step"]
[Předchozí](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
[další](configuring-a-database-server-for-web-deploy-publishing.md)
