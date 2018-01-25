---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
title: "Konfigurace webového serveru pro Web nasazení publikování (Web Deploy obslužné rutiny) | Microsoft Docs"
author: jrjlee
description: "Toto téma popisuje postup konfigurace webovém serveru Internetové informační služby (IIS) pro podporu nasazení pomocí Hanu nasazení webové služby IIS a publikování na webu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 90ebf911-1c46-4470-b876-1335bd0f590f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler
msc.type: authoredcontent
ms.openlocfilehash: 81848c683fb9ddaa8942f030a520847a3c89fde0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler"></a>Konfigurace webového serveru pro Web nasazení publikování (Web Deploy obslužné rutiny)
====================
podle [Jason Lee](https://github.com/jrjlee)

[Stáhnout PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Toto téma popisuje postup konfigurace webovém serveru Internetové informační služby (IIS) pro podporu nasazení pomocí rutiny nasazení webové služby IIS a publikování na webu.
> 
> Při práci s nasazení webu 2.0 nebo novější, existují tři hlavní přístupy, které můžete použít k získání aplikací nebo webů na webovém serveru. Můžeš:
> 
> - Použití *vzdáleného agenta služby nasazení webu*. Tento postup vyžaduje méně konfigurace webového serveru, ale budete muset zadat přihlašovací údaje správce místním serveru za účelem nasazení nic k serveru.
> - Použití *obslužné rutiny pro nasazení webu*. Tento přístup je mnohem složitější a vyžaduje další úsilí počáteční nastavení webového serveru. Pokud použijete tuto metodu, ale můžete nakonfigurovat služby IIS, aby uživatelé bez oprávnění správce k provedení nasazení. Obslužné rutiny nasazení webu je pouze k dispozici ve službě IIS 7 nebo novější verze.
> - Použití *offline nasazení*. Tento přístup, vyžaduje nejmenší míru konfigurace webového serveru, ale správce serveru musíte ručně zkopírovat webového balíčku na server a importujte ji pomocí Správce služby IIS.
> 
> Další informace o klíčové funkce, výhody a nevýhody těchto přístupů, najdete v části [výběr práva přístup k nasazení webu](choosing-the-right-approach-to-web-deployment.md).


Ano, pokud chcete povolit uživatelům bez oprávnění správce pro nasazení obsahu pro konkrétní weby služby IIS. Tento přístup je často žádoucí v těchto typech scénáře:

- Pracovním nebo produkčním prostředí, kde je účet osoby nebo služby, který aktivuje vzdálené nasazení nemají přístup k pověření správce serveru.
- Hostovaných prostředích, kde chcete udělit vzdáleným uživatelům umožňuje aktualizovat jejich weby, aniž by jim úplné řízení webových serverů (nebo přístup k jiné osoby weby).

Ve scénářích vývoj nebo testování, nebo v menší organizace, pomocí přihlašovacích údajů správce serveru nasazení obsahu je často menší sporné. V těchto scénářích konfigurace webových serverů pro podporu nasazení pomocí [Vzdálená služba nasazení webu agenta](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md) nabízí více Nejjednodušším způsobem.

## <a name="task-overview"></a>Přehled úloh

Konfigurace webového serveru do přijmout a nasazení webových balíčků ze vzdáleného počítače obslužné rutiny nasazení webu přístup, musíte:

- Vytvořit nebo vybrat účet uživatele domény ("bez oprávnění správce uživatele") jejichž přihlašovací údaje, které použijete k nasazení.
- Nainstalujte službu IIS 7.5, včetně služby webové správy a modulu Základní ověřování.
- Instalovat nasazení webu 2.1 nebo vyšší.
- Konfigurace služby webové správy k povolení vzdálených připojení a spuštění služby.
- Vytvoření webu IIS hostovat nasazený obsah.
- Udělení oprávnění uživatele bez oprávnění správce na vašem webu ve Správci služby IIS.
- Zajistěte, aby služba webové správy pravidla delegování povolit službu k přidání a změna webového obsahu pomocí účtu uživatele bez oprávnění správce.
- Nakonfigurujte všechny brány firewall umožňuje příchozí připojení na portu 8172.

Pro hostování ukázkové řešení ContactManager konkrétně, bude také nutné:

- Nainstalujte rozhraní .NET Framework 4.0.
- Nainstalujte rozhraní ASP.NET MVC 3.

Toto téma vám ukáže, jak provést všechny tyto postupy. Úlohy a postupy v tomto tématu předpokládat, že začínáte s čistou serveru sestavení systémem Windows Server 2008 R2. Než budete pokračovat, zajistěte, aby:

- Windows Server 2008 R2 Service Pack 1 a všechny dostupné aktualizace, které jsou nainstalovány.
- Server je připojený k doméně.
- Server má statickou IP adresu.

> [!NOTE]
> Další informace o připojení počítače k doméně, najdete v části [připojení počítače k doméně a protokolování na](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Další informace o konfiguraci statických IP adres najdete v tématu [nakonfigurovat statickou IP adresu](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="install-products-and-components"></a>Nainstalujte produkty a součásti

Tato část vás provede instalací požadovaných produktů a součásti na webovém serveru. Než začnete, je vhodné spustit službu Windows Update zajistit, že váš server je zcela aktuální.

V takovém případě musíte nainstalovat tyto věci:

- **Doporučená konfigurace služby IIS 7**. Díky tomu **webového serveru (IIS)** role na vašem webovém serveru a nainstaluje sadu moduly služby IIS a součásti, které potřebujete k hostování aplikace ASP.NET.
- **Služby IIS: Služba správy**. Tím se nainstaluje služba webové správy (WMSvc) ve službě IIS. Tato služba umožňuje vzdálenou správu služby IIS weby a zpřístupní koncový bod obslužné rutiny nasazení webu na klienty.
- **Služby IIS: Základní ověření**. Tím se nainstaluje modul základní ověření služby IIS. Tato služba webové správy (WMSvc) umožňuje ověřování přihlašovacích údajů, které zadáte.
- **Webové nástroje pro nasazení 2.1 nebo vyšší**. Tím se nainstaluje Web Deploy (a její základní spustitelný soubor MSDeploy.exe) na serveru. V rámci tohoto procesu se nainstaluje obslužné rutiny nasazení webu a integruje ji pomocí služby webové správy.
- **Rozhraní .NET framework 4.0**. To je nutné ke spuštění aplikací, které byly vytvořené v této verzi rozhraní .NET Framework.
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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image1.png)
6. V **ASP.NET MVC 3 (Visual Studio 2010)** řádek, klikněte na tlačítko **přidat**.
7. V navigačním podokně klikněte na tlačítko **Server**.
8. V **IIS 7 doporučená konfigurace** řádek, klikněte na tlačítko **přidat**.
9. V **2.1 nástroj pro nasazení webového** řádek, klikněte na tlačítko **přidat**.
10. V **služby IIS: základní ověřování** řádek, klikněte na tlačítko **přidat**.
11. V **služby IIS: Služba správy** řádek, klikněte na tlačítko **přidat**.
12. Klikněte na tlačítko **nainstalovat**. Instalace webové platformy zobrazí seznam produktů & #x 2014; společně s všechny přidružené závislosti & #x 2014; nainstalována a zobrazí výzvu k potvrzení licenčních podmínek.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image2.png)
13. Přečtěte si licenční podmínky a pokud vyjadřujete svůj souhlas s podmínkami, klikněte na **souhlasím**.
14. Po dokončení instalace klikněte na tlačítko **Dokončit**a pak zavřete **webové platformy verze 3.0** okno.

Pokud jste nainstalovali rozhraní .NET Framework 4.0, před instalací služby IIS, budete muset spustit [registrační nástroj](https://msdn.microsoft.com/library/k6h9cz8h(v=VS.100).aspx) (aspnet\_regiis.exe) pro službu IIS zaregistrovat nejnovější verzi technologie ASP.NET. Pokud to neuděláte, zjistíte, že služba IIS bude poskytovat statický obsah (jako jsou soubory HTML) bez problémů, ale vrátí **HTTP Chyba 404.0 – nenalezeno** při pokusu přejděte do obsahu ASP.NET. Následující postup vám pomůže zajistit, aby je zaregistrovány technologii ASP.NET 4.0.

**Chcete-li službu IIS zaregistrovat technologii ASP.NET 4.0**

1. Klikněte na tlačítko **spustit**a pak zadejte **příkazového řádku**.
2. Ve výsledcích hledání klikněte pravým tlačítkem na **příkazového řádku**a potom klikněte na **spustit jako správce**.
3. V okně příkazového řádku přejděte do **%WINDIR%\Microsoft.NET\Framework\v4.0.30319** adresáře.
4. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample1.cmd)]
5. Pokud budete chtít hostitele 64-bit webové aplikace v libovolném bodě, byste měli také zaregistrovat 64bitovou verzi technologie ASP.NET se službou IIS. Chcete-li to provést, v okně příkazového řádku, přejděte na **%WINDIR%\Microsoft.NET\Framework64\v4.0.30319** adresáře.
6. Zadejte tento příkaz a stiskněte klávesu Enter:

    [!code-console[Main](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/samples/sample2.cmd)]

Jako osvědčených postupů pomocí služby Windows Update znovu v tomto okamžiku stáhnout a nainstalovat všechny dostupné aktualizace pro novou produkty a součásti, které jste nainstalovali.

## <a name="configure-the-web-management-service"></a>Konfigurace služby webové správy

Teď, když jste nainstalovali vše, co potřebujete, dalším krokem je konfigurace služby webové správy ve službě IIS. Na vysoké úrovni budete potřebovat k dokončení těchto úloh:

- Povolte základní ověřování na úrovni serveru.
- Konfigurace služby webové správy tak, aby přijímal vzdálená připojení.
- Spuštění služby webové správy.
- Zkontrolujte, jestli jsou požadovaná pravidla delegování služby webové správy na místě.

**Konfigurace služby webové správy**

1. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **Správce Internetové informační služby (IIS)**.
2. Ve Správci služby IIS v **připojení** podokně klikněte na uzel serveru (například **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image3.png)
3. V prostředním podokně v části **IIS**, dvakrát klikněte na **ověřování**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image4.png)
4. Klikněte pravým tlačítkem na **základní ověřování**a potom klikněte na **povolit**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image5.png)
5. V **připojení** podokně klikněte na uzel serveru vrátíte k nastavení nejvyšší úrovně.
6. V prostředním podokně v části **správy**, dvakrát klikněte na **služba správy**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image6.png)
7. V prostředním podokně vyberte **povolit vzdálená připojení**.

    > [!NOTE]
    > Pokud již je spuštěna služba webové správy, musíte ji nejprve zastavit.
8. V **akce** podokně klikněte na tlačítko **spustit** spuštění služby webové správy.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image7.png)
9. Pokud se zobrazí výzva k nastavení uložte, klikněte na tlačítko **Ano**.

    > [!NOTE]
    > Můžete také konfigurovat službu na automatické spouštění. Chcete-li to provést, otevřete konzolu služby, klikněte pravým tlačítkem na **služby webové správy**a potom klikněte na **vlastnosti**. V **typ spuštění** rozevíracího seznamu vyberte **automatické**a potom klikněte na **OK**.
10. V **připojení** podokně klikněte na uzel serveru vrátíte k nastavení nejvyšší úrovně.
11. V prostředním podokně v části **správy**, dvakrát klikněte na **delegování správy služby**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image8.png)
12. Ověřte, zda se v prostředním podokně obsahuje sadu pravidel.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image9.png)

    Tato pravidla povolit autorizovaným uživatelům služby webové správy používat různé zprostředkovatele nasazení webu. Například nasadit webové aplikace a obsah do služby IIS prostřednictvím obslužné rutiny nasazení webu, musí existovat pravidla delegování, které umožňuje všechny ověřené uživatele služby webové správy k použití **contentPath** a **iisApp**  zprostředkovatelé (poslední pravidlo, které vidíte na snímku obrazovky).

    Pokud jste nainstalovali produktů a součástí v pořadí, které jsou popsané v tomto tématu, měli nejnovější verzi nástroje nasazení webu automaticky přidat všechna pravidla potřebná delegování služby webové správy. Pokud stránku služby delegování správy nezobrazuje všechna pravidla, budete muset vytvořit sami. Pokyny o tom, jak to udělat najdete v tématu [nakonfigurujte obslužné rutiny pro nasazení webu](https://go.microsoft.com/?linkid=9805124).
13. V **připojení** podokně klikněte na uzel serveru vrátíte k nastavení nejvyšší úrovně.

## <a name="create-and-configure-an-iis-website"></a>Vytvořit a nakonfigurovat web služby IIS

Před nasazením webového obsahu na server, budete muset vytvořit a nakonfigurovat web služby IIS pro hostování daného obsahu. Nasazení webu lze nasadit pouze webových balíčků na existující web služby IIS; můžete ji nelze vytvořit web. Také musíte udělat málo další konfiguraci, kterou chcete povolit účtu bez oprávnění správce pro vzdálené nasazení obsahu. Na vysoké úrovni budete potřebovat k dokončení těchto úloh:

- Vytvořte složku v systému souborů hostovat obsah.
- Vytvoření webu IIS k obsluze obsah a přidružte ji k místní složce.
- Udělení oprávnění ke čtení pro identitu fondu aplikací do místní složky.
- Udělte nezbytná oprávnění k účtu domény, který bude nasadit webovou aplikaci služby IIS.

I když není co můžete zastavit z nasazení obsahu na výchozí web ve službě IIS, tento postup se nedoporučuje pro jakoukoli jinou hodnotu než testu nebo ukázkový scénáře. Pro simulaci provozním prostředí, měli byste vytvořit nový web služby IIS s nastavením, které jsou specifické pro požadavky vaší aplikace.

**K vytvoření webu IIS**

1. V místním systému souborů, vytvořte složku pro uložení obsahu (například **C:\DemoSite**).
2. Na **spustit** nabídky, přejděte na příkaz **nástroje pro správu**a potom klikněte na **Správce Internetové informační služby (IIS)**.
3. Ve Správci služby IIS v **připojení** podokně rozbalte uzel serveru (například **STAGEWEB1**).

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image10.png)
4. Klikněte pravým tlačítkem myši **lokality** uzel a pak klikněte na tlačítko **přidat web**.
5. V **název lokality** zadejte název pro web služby IIS (například **DemoSite**).
6. V **fyzická cesta** zadejte (nebo vyhledejte) cestu do místní složky (například **C:\DemoSite**).
7. V **Port** zadejte číslo portu, na kterém chcete hostovat na webu (například **85**).

    > [!NOTE]
    > Standardní port čísla jsou 80 pro protokol HTTP a 443 pro protokol HTTPS. Ale pokud hostujete tento web na portu 80, musíte zastavit výchozího webu, než se dostanete k vaší lokality.
8. Ponechte **název hostitele** pole prázdné, pokud chcete nakonfigurovat záznam systému DNS (Domain Name) pro web a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image11.png)

    > [!NOTE]
    > V produkčním prostředí budete pravděpodobně chtít hostování vašeho webu na portu 80 a Konfigurace hlavičky hostitele, společně s odpovídající záznamy DNS. Další informace o konfiguraci hlavičky hostitele ve službě IIS 7 najdete v tématu [konfigurace hlavičku hostitele pro webový server (IIS 7)](https://technet.microsoft.com/library/cc753195(WS.10).aspx). Další informace o roli serveru DNS v systému Windows Server 2008 R2 najdete v tématu [přehled systému DNS Server](https://technet.microsoft.com/en-gb/library/cc770392.aspx) a [DNS Server](https://technet.microsoft.com/windowsserver/dd448607).
9. V **akce** podokně v části **upravit web**, klikněte na tlačítko **vazby**.
10. V **vazby webu** dialogové okno, klikněte na tlačítko **přidat**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image12.png)
11. V **přidat vazbu webu** dialogové okno, sada **IP adresu** a **Port** tak, aby odpovídaly vaší stávající konfigurace lokality.
12. V **název hostitele** zadejte název webového serveru (například **STAGEWEB1**) a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image13.png)

    > [!NOTE]
    > První vazba webu umožňuje přístup k webu místně pomocí IP adresy a portu nebo `http://localhost:85`. Druhé vazby webu umožňuje přístup k webu z jiných počítačů v doméně pomocí názvu počítače (například http://stageweb1:85).
13. V **vazby webu** dialogové okno, klikněte na tlačítko **Zavřít**.
14. V **připojení** podokně klikněte na tlačítko **fondy aplikací**.
15. V **fondy aplikací** podokně klikněte pravým tlačítkem na název vašeho fondu aplikací a pak klikněte na tlačítko **základní nastavení**. Ve výchozím nastavení, bude název vašeho fondu aplikací shodovat s názvem vašeho webu (například **DemoSite**).
16. V **verze rozhraní .NET Framework** seznamu, vyberte **rozhraní .NET Framework v4.0.30319**a potom klikněte na **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image14.png)

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

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image15.png)
5. V **vybrat uživatele nebo skupiny** dialogové okno, typ **IIS\_IUSRS**, klikněte na tlačítko **Kontrola názvů**a potom klikněte na **OK**.
6. V **oprávnění pro *** [název složky]* dialogové okno, Všimněte si, že byl přiřazen do nové skupiny **čtení &amp; provést**, **zobrazovat obsah složky**, a **Čtení** oprávnění ve výchozím nastavení. Nechte beze změny a klikněte na tlačítko **OK**.
7. Klikněte na tlačítko **OK** zavřete *[název složky] *** vlastnosti** dialogové okno.

Jako poslední úlohy je nutné udělit příslušná oprávnění k uživatel není správcem, jehož pověření, které použijete k nasazení obsahu. Tento uživatel vyžaduje oprávnění pro nasazení obsahu vzdáleně na váš web.

**Konfigurace služby IIS webu oprávnění pro uživatele bez oprávnění správce domény**

1. Ve Správci služby IIS v **připojení** podokně klikněte pravým tlačítkem na váš web uzlu (například **DemoSite**), přejděte na **nasadit**a potom klikněte na **webové konfigurace Nasazení publikování**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image16.png)
2. V **konfigurace nasazení publikování na webu** dialogové okno, napravo **vybrat uživatele a udělit oprávnění publikování** seznamu, klikněte na tlačítko se třemi tečkami.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image17.png)
3. V **povolit uživatele** dialogovém okně zadejte doména a uživatelské jméno účtu, kterou chcete použít k nasazení obsahu a pak klikněte na tlačítko **OK**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image18.png)
4. V **konfigurace nasazení publikování na webu** dialogové okno, klikněte na tlačítko **instalační program**.

    ![](configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler/_static/image19.png)

    > [!NOTE]
    > Tato operace provádí dvě klíčové funkce v jednom kroku. Nejprve uděluje oprávnění uživatele k úpravě webu vzdáleně prostřednictvím služby webové správy podle pravidel delegování, které zkontrolován v předchozí části. Za druhé uděluje uživateli úplné řízení ke zdrojové složce pro web, který umožňuje uživateli přidat, upravit a nastavit oprávnění pro obsah webu.
5. V **konfigurace nasazení publikování na webu** dialogové okno, klikněte na tlačítko **Zavřít**.

## <a name="configure-firewall-exceptions"></a>Konfigurace výjimek brány Firewall

Ve výchozím nastavení služba správy webové služby IIS naslouchá na portu TCP 8172. Pokud na vašem webovém serveru je povolená brána Windows Firewall, budete muset vytvořit nové příchozí pravidlo umožňující přenos TCP na portu 8172 (všechny odchozí přenosy se ve výchozím nastavení v bráně Windows Firewall). Pokud používáte bránu firewall jiného výrobce, budete muset vytvořit pravidla, aby se povolily přenosy.

| Směr | Z portu | K portu | Typ portu |
| --- | --- | --- | --- |
| Příchozí | všechny | 8172 | TCP |
| Odchozí | 8172 | všechny | TCP |
  

Další informace o konfiguraci pravidla v bráně Windows Firewall najdete v tématu [konfigurace pravidel brány Firewall](https://technet.microsoft.com/library/dd448559(WS.10).aspx). Brány firewall třetích stran najdete v dokumentaci k produktu.

## <a name="conclusion"></a>Závěr

Váš webový server by teď měly být připravena přijímat vzdálené nasazení do obslužné rutiny webu nasadit prostřednictvím služby webové správy. Před dalším pokusem o nasazení webové aplikace na server, můžete Zkontrolujte tyto klíčové body:

- Mít povoleno základní ověřování na úrovni serveru ve službě IIS?
- Povolili jste vzdálená připojení k webové službě správy?
- Jste spustili služby webové správy?
- Jsou správu pravidla delegování služby v místě?
- Má identita fondu aplikací přístup pro čtení ke zdrojové složce pro svůj web?
- Má účet uživatele bez oprávnění správce ve službě IIS oprávnění na úrovni webu?
- Brána firewall umožňuje příchozí připojení k serveru na portu TCP 8172?

## <a name="further-reading"></a>Další čtení

Informace o tom, jak nakonfigurovat vlastní soubory projektu Microsoft Build Engine (MSBuild) k nasazení webových balíčků do obslužné rutiny nasazení webu, najdete v části [konfigurace vlastnosti nasazení pro cílové prostředí](configuring-deployment-properties-for-a-target-environment.md).

>[!div class="step-by-step"]
[Předchozí](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)
[další](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
