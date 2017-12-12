---
uid: whitepapers/aspnet-and-iis6
title: "Používající technologii ASP.NET 1.1 pomocí služby IIS 6.0 | Microsoft Docs"
author: rick-anderson
description: "Zatímco systém Windows Server 2003 zahrnuje službu IIS 6.0 a ASP.NET 1.1, tyto součásti jsou zakázané ve výchozím nastavení. Tento dokument White Paper popisuje, jak povolit služby IIS 6.0..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a>Používající technologii ASP.NET 1.1 pomocí služby IIS 6.0
====================
> Zatímco systém Windows Server 2003 zahrnuje službu IIS 6.0 a ASP.NET 1.1, tyto součásti jsou zakázané ve výchozím nastavení. Tento dokument White Paper popisuje postup při povolování služby IIS 6.0 a ASP.NET 1.1 a doporučuje několik nastavení konfigurace, abyste získali optimální výkon ze služby IIS a ASP.NET.
> 
> Platí pro technologii ASP.NET 1.1 a služby IIS 6.0.


ASP.NET 1.1 se dodává s Windows Server 2003, která také zahrnuje nejnovější verzi služby Internet Information Server (IIS) verze 6.0. Služby IIS 6.0 a ASP.NET 1.1 slouží k integraci bez problémů a ASP.NET výchozí nastavení do nového modelu pracovní proces služby IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>Ve výchozím nastavení není nainstalována technologie ASP.NET 1.1

Na rozdíl od předchozích verzí operačních systémů společnosti Microsoft server Internet Information Server (IIS) není povoleno ve výchozím nastavení; ani ASP.NET 1.1. Existují dvě možnosti pro povolení služby IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Povolení služby IIS, možnost #1 - Průvodce konfigurací serveru

Windows Server 2003 se dodává nové, Průvodce konfigurací serveru, se správně konfigurace serveru v režimu požadované.

Pokud chcete spustit Průvodce – Poznámka: Chcete-li spustit průvodce, musíte být přihlášení jako správce – přejděte na: spuštění | Programy | Nástroje pro správu a vyberte možnost 'konfigurace serveru'.

Po výběru, měli byste vidět, Průvodce konfigurací serveru' úvodní obrazovce:

![](aspnet-and-iis6/_static/image1.jpg)

Klikněte na tlačítko ' Další &gt;':

![](aspnet-and-iis6/_static/image2.jpg)

Klikněte na tlačítko ' Další &gt;.

![](aspnet-and-iis6/_static/image3.jpg)

Na této obrazovce budete muset vybrat, aplikační server (IIS, ASP.NET) jako možnosti pro konfiguraci.

Klikněte na tlačítko ' Další &gt;'.

![](aspnet-and-iis6/_static/image4.jpg)

Po výběru konfigurace serveru jako Server aplikace, tato obrazovka se zobrazí výzvy, jaké další funkce by měly být nainstalovány. Žádná možnost je vybrána ve výchozím nastavení. Automaticky povolit technologii ASP.NET, budete muset vybrat možnost "Povolit ASP. NET'.

Klikněte na tlačítko ' Další &gt;'.

![](aspnet-and-iis6/_static/image5.jpg)

Tato obrazovka se zobrazí, jaké možnosti jsou k instalaci.

Klikněte na tlačítko ' Další &gt;'.

![](aspnet-and-iis6/_static/image6.jpg)

Tato obrazovka se zobrazí během instalace možností, které jste vybrali. Je normální, že se ostatní dialogové okno, které polí se zobrazí, jak probíhá instalace služeb zobrazí. Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.

Klikněte na tlačítko ' Další &gt;se při dokončení.

![](aspnet-and-iis6/_static/image7.jpg)

Klikněte na tlačítko 'Dokončit' – Windows Server 2003 je nyní nakonfigurováno pro podporu služby IIS 6.0 a ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Povolení služby IIS, možnost #2 - ruční konfiguraci služby IIS a ASP.NET

Pokud nechcete používat, Server Průvodce konfigurací' Volitelně můžete nainstalovat službu IIS 6.0 a 1.1 ASP.NET pomocí "Přidat nebo odebrat programy" z ovládacích panelů.

Nejprve otevřete ovládací Panel:

![](aspnet-and-iis6/_static/image8.jpg)

Potom klikněte na 'přidat nebo odebrat součásti systému Windows, které se otevře Průvodce součástmi systému Windows:

![](aspnet-and-iis6/_static/image9.jpg)

Zvýrazněte a 'aplikační Server a pak klikněte na 'Podrobnosti'? tlačítko:

![](aspnet-and-iis6/_static/image10.jpg)

Instalace technologie ASP.NET, zkontrolujte ' ASP. NET'.

Klikněte na tlačítko "OK" se vraťte do Průvodce součástmi systému Windows. Klikněte na tlačítko ' Další &gt;' z Průvodce součástmi systému Windows při zahájení instalace:

![](aspnet-and-iis6/_static/image11.jpg)

Je normální, že se ostatní dialogové okno, které polí se zobrazí, jak probíhá instalace služeb zobrazí. Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.

Po dokončení instalace se zobrazí poslední obrazovce Průvodce součástmi systému Windows:

![](aspnet-and-iis6/_static/image12.jpg)

Služby IIS 6.0 a ASP.NET 1.1 teď nakonfigurované a dostupné.

## <a name="recommended-settings"></a>Doporučená nastavení

Při spuštění ASP.NET 1.1 se službou IIS 6.0 existuje několik nastavení konfigurace, které se doporučuje, aby získali optimální výkon z ASP.NET:

- Konfigurace omezení paměti pracovní proces
- Konfigurace recyklace pracovních procesů

### <a name="configuring-worker-process-memory-limits"></a>Konfigurace omezení paměti pracovní proces

Ve výchozím nastavení služby IIS 6.0 není nastaven limit množství paměti, která může služba IIS použít. SOUBOR ASP. Funkce mezipaměti na NET závisí na omezení paměti, tedy do mezipaměti proaktivně odebrat nepoužité položky z paměti.

Doporučujeme nakonfigurovat paměti recyklace funkce služby IIS 6.0. Ke konfiguraci této otevřete Správce Internetové informační služby (Start | Programy | Nástroje pro správu | Internetová informační služba). Jakmile otevřete, rozbalte složku 'fondů aplikací.:

Pro každý fond aplikací:

![](aspnet-and-iis6/_static/image13.jpg)

1. Klikněte pravým tlačítkem na fond aplikací, například 'DefaultAppPool' a vyberte možnost 'vlastnosti':

![](aspnet-and-iis6/_static/image14.jpg)

2. Dál povolte kliknutím na buď recyklace paměti ' maximální použité paměti (v megabajtech): ". Hodnota nesmí být větší než velikost fyzické paměti (ne virtuální) na serveru, je dobré aproximace 60 % fyzické paměti, tj. pro server s 512 MB fyzické paměti vyberte 310. Doporučujeme taky, že maximální není delší než 800MB při použití adresní prostor 2GB. Pokud je adresní prostor adresy paměti serveru 3GB, maximální limit paměti pro pracovní proces může být až 1 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogovém okně Vlastnosti. Tento postup opakujte pro všechny fondy aplikací k dispozici.

### <a name="configuring-worker-recycling"></a>Konfigurace pracovního procesu recyklace

Ve výchozím nastavení nastaven recyklovat každých 29 hodin jeho pracovní proces služby IIS 6.0. Toto je trochu agresivní pro aplikace používající technologii ASP.NET a se doporučuje, aby bylo zakázané recyklace automatické pracovního procesu.

Chcete-li zakázat automatické pracovní proces recykluje, nejprve otevřete Správce Internetové informační služby (Start | Programy | Nástroje pro správu | Internetová informační služba). Jakmile otevřete, rozbalte složku 'fondů aplikací.:

![](aspnet-and-iis6/_static/image16.jpg)

Pro každý fond aplikací:

1. Klikněte pravým tlačítkem na fond aplikací, například 'DefaultAppPool' a vyberte možnost 'vlastnosti':

![](aspnet-and-iis6/_static/image17.jpg)

2. Zrušte zaškrtnutí políčka ' recyklovat pracovní proces (v minutách): ":

![](aspnet-and-iis6/_static/image18.jpg)

Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogovém okně Vlastnosti. Tento postup opakujte pro všechny fondy aplikací k dispozici.

## <a name="granting-write-access-to-the-file-system"></a>Udělení oprávnění k zápisu do systému souborů

Pokud vaše aplikace vyžaduje přístup k zápisu do systému souborů a používáte budete muset upravit řízení přístupu seznamu (ACL) na soubor nebo složku udělit ASP.NET přístup k systému souborů NTFS.

Například udělit ASP.NET oprávnění k zápisu do c:\inetpub\wwwroot nejdřív otevřete Průzkumníka a přejděte do adresáře:

![](aspnet-and-iis6/_static/image19.jpg)

Pak klikněte pravým tlačítkem na adresář, např 'wwwroot' a vyberte možnost Vlastnosti. Po otevření dialogu Vlastnosti, vyberte kartu 'Zabezpečení':

![](aspnet-and-iis6/_static/image20.jpg)

C:\inetpub\wwwroot\ adresář je speciální adresář, ve které speciální skupinu služby IIS 6.0, IIS\_WPG' je již oprávnění ke čtení &amp; oprávnění spouštět, zobrazovat obsah složky a číst. Ale abyste mohli udělit oprávnění k zápisu, musíte klikněte na zaškrtávací políčko Povolit pro zápis:

![](aspnet-and-iis6/_static/image21.jpg)

Služby IIS 6.0 teď má oprávnění k zápisu v této složce. Udělit oprávnění k zápisu v jiných složkách, postupujte podle těchto kroků - Všimněte si, že budete muset přidat IIS\_WPG skupiny, pokud ještě neexistuje.

> [!CAUTION]
> Udělení oprávnění zapisovat do služby IIS\_WPG vám umožní všechny aplikace technologie ASP.NET pro zápis do tohoto adresáře.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Podporuje integrované ověřování s SQL serverem

Integrované ověřování systému SQL Server tak, aby využívala ověřování systému Windows NT umožňuje ověření účty přihlášení systému SQL Server. To umožňuje uživatelům obejít standardní proces přihlášení serveru SQL Server. Tento přístup síťového uživatele získat přístup k databázi systému SQL Server bez zadávat samostatné přihlašovací identifikátor nebo heslo, protože systém SQL Server získá uživatele a informace o hesle z procesu zabezpečení sítě systému Windows NT.

Výběr integrované ověřování pro aplikace ASP.NET je vhodné použít, protože žádné přihlašovací údaje jsou někdy uloženy v rámci připojovací řetězec pro vaši aplikaci. Připojovací řetězec použitý pro připojení k SQL místo bude vypadat takto:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Tento připojovací řetězec oznamuje serveru SQL pro použití přihlašovacích údajů Windows aplikace pokusu o přístup k systému SQL Server. V případě ASP.NET/IIS 6 bude účet ve službě IIS\_WPG skupiny.

Pokud chcete povolit integrované ověřování mezi SQL serverem a ASP.NET, je potřeba nejdříve se ujistěte, že systém SQL Server je nakonfigurován pro integrované ověřování nebo ověřování v režimu Mixed - obraťte se na správce databáze to bylo možné určit. Pokud je SQL Server v jedné z těchto dvou režimů, můžete použít integrované ověřování.

Otevřete správce systému SQL Server Enterprise (spustit | Programy | Microsoft SQL Server | Správce organizace), vyberte příslušný server a rozbalte složku zabezpečení:

![](aspnet-and-iis6/_static/image22.jpg)

Pokud se BUILTINT\IIS\_WPG "skupina není uvedena, klikněte pravým tlačítkem na přihlášení a vyberte nové přihlašovací údaje:

![](aspnet-and-iis6/_static/image23.jpg)

V, název:' textového pole zadejte buď ' \IIS [název serveru nebo domény]\_WPG, nebo klikněte na tlačítko se třemi tečkami otevřete výběr uživatele nebo skupiny systému Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Vyberte aktuálního počítače IIS\_WPG skupiny a klikněte na tlačítko "Přidat" a OK zavřete nástroje pro výběr.

Pak musíte taky nastavit oprávnění pro přístup k databázi a výchozí databáze. Nastavit výchozí databáze vyberte v rozevíracím seznamu je vybrána například níže Northwind:

![](aspnet-and-iis6/_static/image25.jpg)

Potom klikněte na kartě přístup k databázi:

![](aspnet-and-iis6/_static/image26.jpg)

Klikněte na zaškrtávací políčko povolení pro každou databázi, kterou chcete povolit přístup k. Budete také muset vyberte role databáze, kontrola db\_vlastníka zajistí vaše přihlašovací údaje zahrnují všechna potřebná oprávnění ke správě a použít vybrané databáze.

Klikněte na tlačítko OK zavřete dialogové okno vlastností. Vaše aplikace ASP.NET je nyní nakonfigurován pro podporu integrované ověřování systému SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Nespouštět ASP.NET 1.0 v nativním režimu služby IIS 6.0

ASP.NET 1.0 ve službě IIS 6.0 je podporována pouze v režimu kompatibility IIS 5.

Konfigurace ASP.NET 1.0 ke spuštění v režimu kompatibility služby IIS 5.0, otevřete Správce internetové služby a klikněte pravým tlačítkem na webové servery a vyberte vlastnosti:

![](aspnet-and-iis6/_static/image27.jpg)

Přepněte na kartu služby a zkontrolujte? Spustit webovou službu v režimu služby IIS 5.0 izolace?:

![](aspnet-and-iis6/_static/image28.jpg)
