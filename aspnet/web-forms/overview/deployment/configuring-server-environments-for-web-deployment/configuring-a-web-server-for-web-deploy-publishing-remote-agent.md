---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
title: Konfigurace webového serveru pro Web nasazení publikování (vzdáleného agenta) | Microsoft Docs
author: jrjlee
description: Toto téma popisuje postup konfigurace webovém serveru Internetové informační služby (IIS) pro podporu nasazení pomocí nasazení webu služby IIS a publikování na webu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 239c7aa8-d09a-4d02-9c0e-6bd52be5f0d5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent
msc.type: authoredcontent
ms.openlocfilehash: 8cad6ee45a8331513c72c4079f300fbb06c1ed77
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-remote-agent"></a>Konfigurace webového serveru pro nasazení webu publikování (vzdáleného agenta)
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup konfigurace webovém serveru Internetové informační služby (IIS) pro podporu nasazení pomocí služby vzdáleného agenta nástroj nasazení webu služby IIS (Web Deploy) a publikování na webu.
> 
> Při práci s nasazení webu 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání aplikací nebo webů na webovém serveru. Můžeš:
> 
> - Použití *vzdáleného agenta služby nasazení webu*. Tento postup vyžaduje méně konfigurace webového serveru, ale budete muset zadat přihlašovací údaje správce místním serveru za účelem nasazení nic k serveru.
> - Použití *obslužné rutiny pro nasazení webu*. Tento přístup je mnohem složitější a vyžaduje další úsilí počáteční nastavení webového serveru. Pokud použijete tuto metodu, ale můžete nakonfigurovat služby IIS, aby uživatelé bez oprávnění správce k provedení nasazení. Obslužné rutiny nasazení webu je pouze k dispozici ve službě IIS 7 nebo novější verze.
> - Použití *offline nasazení*. Tento přístup, vyžaduje nejmenší míru konfigurace webového serveru, ale správce serveru musíte ručně zkopírovat webového balíčku na server a importujte ji pomocí Správce služby IIS.
> 
> Další informace o klíčové funkce, výhody a nevýhody těchto přístupů, najdete v části [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md).


## <a name="is-the-web-deploy-remote-agent-the-right-approach-for-you"></a>Je webu nasazení vzdáleného agenta správný přístup pro vás?

Ano, pokud uživatel, který bude nasazení obsahu můžete zadat přihlašovací údaje správce na cílovém serveru. Tento přístup je často žádoucí v těchto typech scénáře:

- Vývoj nebo testovací prostředí, kde vývojář má plnou kontrolu nad cílový webový server a databázový server.
- Menší organizace, ve kterých jeden uživatel nebo malá skupina uživatelů má kontrolu nad životním cyklu celou aplikaci.

V mnoha větší organizací a platí to hlavně o pracovním nebo produkčním prostředí je často není realistické uživatelům udělit oprávnění správce na webové servery. U hostované webové servery je to pravděpodobně hlavně v případě. Pokud k automatizaci nasazení ze serveru sestavení, nemusí kromě toho chcete použít přihlašovací údaje správce pro proces nasazení. V těchto scénářích konfigurace webových serverů pro podporu nasazení pomocí [obslužné rutiny nasazení webu](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md) může poskytnout přijatelnější výběru.

## <a name="task-overview"></a>Přehled úloh

Toto téma popisuje postup konfigurace webovém serveru Internetové informační služby (IIS) 7.5 přijmout a nasadit webové balíčky ze vzdáleného počítače pomocí vzdáleného agenta nasadit webový přístup. Budete muset:

- Nainstalujte službu IIS 7.5 a IIS 7 doporučená konfigurace.
- Instalovat nasazení webu 2.1 nebo vyšší.
- Vytvoření webu IIS hostovat nasazený obsah.
- Ověřte, zda je spuštěna Služba agenta pro nasazení webu.

Pro hostování ukázkové řešení konkrétně, bude také nutné:

- Nainstalujte rozhraní .NET Framework 4.0.
- Nainstalujte rozhraní ASP.NET MVC 3.

Toto téma vám ukáže, jak provést všechny tyto postupy. Úlohy a postupy v tomto tématu předpokládat, že začínáte s čistou serveru sestavení systémem Windows Server 2008 R2. Než budete pokračovat, zajistěte, aby:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.
- Server je připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně, najdete v části [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurovat statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Služba vzdáleného agenta podporuje služby IIS 6 a vyšší a není nutné být připojen k doméně. Ale kroky v tomto kurzu se vyvíjí a otestovali na službu IIS 7.5 a postupy pro jiné verze se může lišit.


## <a name="install-products-and-components"></a>Nainstalujte produkty a součásti

Tato část vás provede instalací požadovaných produktů a součásti na webovém serveru. Než začnete, je vhodné spustit službu Windows Update zajistit, že váš server je zcela aktuální.

V takovém případě musíte nainstalovat tyto věci:

- **Doporučená konfigurace služby IIS 7**. Díky tomu **webového serveru (IIS)** role na vašem webovém serveru a nainstaluje sadu moduly služby IIS a součásti, které potřebujete k hostování aplikace ASP.NET.
- **Rozhraní .NET framework 4.0**. To je nutné ke spuštění aplikací, které byly vytvořené v této verzi rozhraní .NET Framework.
- **Webové nástroje pro nasazení 2.1 nebo vyšší**. Tím se nainstaluje Web Deploy (a její základní spustitelný soubor MSDeploy.exe) na serveru. V rámci tohoto procesu se nainstaluje a spustí služba agenta pro nasazení webu. Tato služba umožňuje nasadit webovou balíčky ze vzdáleného počítače.
- **ASP.NET MVC 3**. Tím se nainstaluje sestavení, budete muset spustit aplikace MVC 3.

> [!NOTE]
> Tento návod popisuje použití instalačního programu webové platformy nainstalovat a nakonfigurovat požadované součásti. I když nemáte pomocí služby instalace webové platformy, zjednodušuje proces instalace automaticky zjišťuje závislosti a zajistíte, že vždy dostanete nejnovější verze produktu. Další informace najdete v tématu [Microsoft webové platformy verze 3.0](https://go.microsoft.com/?linkid=9805118).


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

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image1.png)
6. V **ASP.NET MVC 3 (Visual Studio 2010)** řádek, klikněte na tlačítko **přidat**.
7. V navigačním podokně klikněte na tlačítko **Server**.
8. V **IIS 7 doporučená konfigurace** řádek, klikněte na tlačítko **přidat**.
9. V **2.1 nástroj pro nasazení webového** řádek, klikněte na tlačítko **přidat**.
10. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů&#x2014;spolu s případnými přidružené závislosti&#x2014;k instalaci a zobrazí výzvu k potvrzení licenčních podmínek.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image2.png)
11. Přečtěte si licenční podmínky a pokud vyjadřujete svůj souhlas s podmínkami, klikněte na **souhlasím**.
12. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **webové platformy verze 3.0** okno.

Pokud jste nainstalovali rozhraní .NET Framework 4.0, před instalací služby IIS, budete muset spustit [registrační nástroj](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) pro službu IIS zaregistrovat nejnovější verzi technologie ASP.NET. Pokud to neuděláte, zjistíte, že služba IIS bude poskytovat statický obsah (jako jsou soubory HTML) bez problémů, ale vrátí **HTTP Chyba 404.0 – nenalezeno** při pokusu přejděte do obsahu ASP.NET. Tento postup slouží k zajištění, že je zaregistrován technologii ASP.NET 4.0.

**Chcete-li službu IIS zaregistrovat technologii ASP.NET 4.0**

1. Klikněte na tlačítko **spustit**a pak zadejte **příkazového řádku**.
2. Ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku**a potom klikněte na **spustit jako správce**.
3. V okně příkazového řádku přejděte do **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** adresáře.
4. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample1.cmd)]
5. Pokud budete chtít hostitele 64-bit webové aplikace v libovolném bodě, byste měli také zaregistrovat 64bitovou verzi technologie ASP.NET se službou IIS. Chcete-li to provést, v okně příkazového řádku, přejděte na **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** adresáře.
6. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-remote-agent/samples/sample2.cmd)]

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
3. Ve Správci služby IIS v **připojení** podokně rozbalte uzel serveru (například **TESTWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image3.png)
4. Klikněte pravým tlačítkem myši **lokality** uzel a pak klikněte na tlačítko **přidat web**.
5. V **název lokality** zadejte název pro web služby IIS (například **DemoSite**).
6. V **fyzická cesta** zadejte (nebo vyhledejte) cestu do místní složky (například **C:\DemoSite**).
7. V **Port** zadejte číslo portu, na kterém chcete hostovat na webu (například **85**).

    > [!NOTE]
    > Standardní port čísla jsou 80 pro protokol HTTP a 443 pro protokol HTTPS. Ale pokud hostujete tento web na portu 80, musíte zastavit výchozího webu, než se dostanete k vaší lokality.
8. Ponechte **název hostitele** pole prázdné, pokud chcete nakonfigurovat záznam systému DNS (Domain Name) pro web a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image4.png)

    > [!NOTE]
    > V produkčním prostředí budete pravděpodobně chtít hostování vašeho webu na portu 80 a Konfigurace hlavičky hostitele, společně s odpovídající záznamy DNS. Další informace o konfiguraci hlavičky hostitele ve službě IIS 7 najdete v tématu [konfigurace hlavičku hostitele pro webový server (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Další informace o roli serveru DNS v systému Windows Server 2008 R2 najdete v tématu [přehled systému DNS Server](https://technet.microsoft.com/en-gb/library/cc770392.aspx) a [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).
9. V **akce** podokně v části **upravit web**, klikněte na tlačítko **vazby**.
10. V **vazby webu** dialogové okno, klikněte na tlačítko **přidat**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image5.png)
11. V **přidat vazbu webu** dialogové okno, sada **IP adresu** a **Port** tak, aby odpovídaly vaší stávající konfigurace lokality.
12. V **název hostitele** zadejte název webového serveru (například **TESTWEB1**) a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image6.png)

    > [!NOTE]
    > První vazba webu umožňuje přístup k webu místně pomocí IP adresy a portu nebo `http://localhost:85`. Druhé vazby webu umožňuje přístup k webu z jiných počítačů v doméně pomocí názvu počítače (například http://testweb1:85).
13. V **vazby webu** dialogové okno, klikněte na tlačítko **Zavřít**.
14. V **připojení** podokně klikněte na tlačítko **fondy aplikací**.
15. V **fondy aplikací** podokně klikněte pravým tlačítkem na název vašeho fondu aplikací a pak klikněte na tlačítko **základní nastavení**. Ve výchozím nastavení, bude název vašeho fondu aplikací shodovat s názvem vašeho webu (například **DemoSite**).
16. V **verze rozhraní .NET Framework** seznamu, vyberte **rozhraní .NET Framework v4.0.30319**a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image7.png)

    > [!NOTE]
    > Ukázkové řešení vyžaduje rozhraní .NET Framework 4.0. Toto není požadavek pro nasazení webu obecně.

V pořadí pro svůj web pro práci s obsahem identita fondu aplikací musí mít oprávnění ke čtení místní složky, která uloží obsah. Ve službě IIS 7.5 spusťte fondy aplikací s identitou fondu aplikací jedinečný ve výchozím nastavení (na rozdíl od předchozí verze služby IIS, kde by fondy aplikací obvykle běží pod účtem síťové služby). Identita fondu aplikací není skutečné uživatelský účet a nezobrazuje na všechny seznamy uživatelů nebo skupin&#x2014;místo toho se vytváří v dynamicky při spuštění fondu aplikací. Každý identita fondu aplikací se přidá do místní **IIS\_IUSRS** skupiny zabezpečení jako skrytá položka.

Abyste mohli udělit oprávnění k souboru nebo složce identity fondu aplikací, máte dvě možnosti:

- Přiřadit oprávnění k identitě fondu aplikací přímo, formátu <strong>IIS AppPool\</ strong ><em>[název fondu aplikací]</em>(například <strong>IIS AppPool\DemoSite</strong>).
- Přidělování oprávnění k **IIS\_IUSRS** skupiny.

Většina běžný postup je přiřadit oprávnění k místní **IIS\_IUSRS** skupiny, protože tento přístup umožňuje změnit fondy aplikací bez nutnosti měnit oprávnění systému souborů. Následující postup používá tento přístup na základě skupiny.

> [!NOTE]
> Další informace o identity fondu aplikací ve službě IIS 7.5, najdete v části [identity fondu aplikací součásti](https://go.microsoft.com/?linkid=9805123).


**Konfigurace složky oprávnění pro web služby IIS**

1. V Průzkumníku Windows přejděte do umístění místní složky.
2. Klikněte pravým tlačítkem složku a pak klikněte na **vlastnosti**.
3. Na **zabezpečení** , klikněte na **upravit**a potom klikněte na **přidat**.
4. Klikněte na tlačítko **umístění**. V **umístění** dialogové okno, vyberte místního serveru a pak klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image8.png)
5. V **vybrat uživatele nebo skupiny** dialogové okno, typ **IIS\_IUSRS**, klikněte na tlačítko **Kontrola názvů**a potom klikněte na **OK**.
6. V <strong>oprávnění pro</strong><em>[název složky]</em>dialogové okno, Všimněte si, že byl přiřazen do nové skupiny <strong>čtení &amp; provést</strong>, <strong>složku seznamu obsah</strong>, a <strong>čtení</strong> oprávnění ve výchozím nastavení. Nechte beze změny a klikněte na tlačítko <strong>OK</strong>.
7. Klikněte na tlačítko <strong>OK</strong> zavřete <em>[název složky]</em><strong>vlastnosti</strong> dialogové okno.

Jako poslední úlohy před dalším pokusem o všechny balíčky webového nasazení na server, měli byste zajistit, že je spuštěna Služba agenta pro nasazení webu. Při nasazení balíčku ze vzdáleného počítače se Služba agenta pro nasazení webu zodpovídá za extrahování a instalace obsahu balíčku. Služba je spuštěna ve výchozím nastavení po instalaci nástroj pro nasazení webu a bude spuštěna pod síťovou službu.

Můžete zkontrolovat zda je spuštěná v několika různými způsoby, pomocí různých nástrojů příkazového řádku nebo rutiny prostředí Windows PowerShell. Tento postup popisuje Nejjednodušším způsobem na základě uživatelského rozhraní.

**Zkontrolujte, zda je spuštěna Služba agenta pro nasazení webu**

1. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **služby**.
2. Vyhledejte **Služba agenta pro nasazení webu** řádek a ověřte, že **stav** je nastaven na **Začínáme**.

    ![](configuring-a-web-server-for-web-deploy-publishing-remote-agent/_static/image9.png)
3. Pokud je již spuštěna není, klikněte na tlačítko **spustit**.

## <a name="configure-firewall-exceptions"></a>Konfigurace výjimek brány Firewall

Ve výchozím nastavení vzdálené služby Agent naslouchá na portu TCP 80 a na této adrese URL:

http:// [<em>název serveru</em>] / MSDEPLOYAGENTSERVICE

Ve většině případů nebude muset nakonfigurovat pravidla žádné další brány firewall pro službu vzdáleného agenta, protože webové servery obvykle naslouchat požadavkům HTTP na portu 80. Pokud jste si přizpůsobili instalaci tak, aby naslouchala na nestandardním portu, budete muset nakonfigurovat výjimky brány firewall podle potřeby.

## <a name="conclusion"></a>Závěr

Váš webový server je nyní připraven k přijímání a instalaci webové balíčky ze vzdáleného počítače. Před dalším pokusem o nasazení webové aplikace na server, můžete Zkontrolujte tyto klíčové body:

- Jste zaregistrovali technologii ASP.NET 4.0 IIS?
- Má identita fondu aplikací přístup pro čtení ke zdrojové složce pro svůj web?
- Je spuštěna Služba agenta pro nasazení webu?

## <a name="further-reading"></a>Další čtení

Pokyny ke konfiguraci vlastních souborů projektu Microsoft Build Engine (MSBuild) pro nasazení webových balíčků pro službu vzdáleného agenta najdete v tématu [konfigurace vlastnosti nasazení pro cílové prostředí](configuring-deployment-properties-for-a-target-environment.md).

> [!div class="step-by-step"]
> [Předchozí](scenario-configuring-a-production-environment-for-web-deployment.md)
> [další](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md)
