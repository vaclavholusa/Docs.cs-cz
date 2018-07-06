---
uid: whitepapers/aspnet-and-iis6
title: Spuštění ASP.NET 1.1 se službou IIS 6.0 | Dokumentace Microsoftu
author: rick-anderson
description: Sice systému Windows Server 2003, zahrnuje službu IIS 6.0 a ASP.NET 1.1, jsou ve výchozím nastavení zakázané tyto komponenty. Tento dokument White Paper popisuje, jak povolit služby IIS 6.0...
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 338059104f46a6c5517212db6e1a54c12677e133
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839291"
---
<a name="running-aspnet-11-with-iis-60"></a>Spuštění ASP.NET 1.1 se službou IIS 6.0
====================
> Sice systému Windows Server 2003, zahrnuje službu IIS 6.0 a ASP.NET 1.1, jsou ve výchozím nastavení zakázané tyto komponenty. Tento dokument White Paper popisuje postup při povolování služby IIS 6.0 a ASP.NET 1.1 a doporučuje několik nastavení konfigurace, abyste získali optimální výkon služby IIS a ASP.NET.
> 
> Platí pro technologii ASP.NET 1.1 a IIS 6.0.


ASP.NET 1.1 se dodává se systémem Windows Server 2003, který taky obsahuje nejnovější verzi nástroje Internet Information Server (IIS) verze 6.0. Služba IIS 6.0 a ASP.NET 1.1 jsou navržené tak bez problémů integrovat a ASP.NET výchozí nastavení pro nový model procesu pracovního procesu služby IIS 6.0.

## <a name="aspnet-11-is-not-installed-by-default"></a>Ve výchozím nastavení není nainstalována technologie ASP.NET 1.1

Na rozdíl od předchozích verzí operačních systémů od Microsoftu server Internet Information Server (IIS) není povolená ve výchozím nastavení; ani je ASP.NET 1.1. Existují dvě možnosti pro povolení služby IIS:

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a>Povolení služby IIS, možnost #1 - Průvodce konfigurací serveru

Windows Server 2003 se dodává novou "Průvodce konfigurací serveru" vám pomohou správně nakonfigurovat váš server do požadovaného režimu.

Spustí se Průvodce – Poznámka: ke spuštění průvodce, musíte být přihlášeni jako správce – přejít na: Start | Programy | Nástroje pro správu a vyberte možnost "Konfigurace serveru".

Po výběru byste měli vidět "Průvodce konfigurací serveru" úvodní obrazovce:

![](aspnet-and-iis6/_static/image1.jpg)

Klikněte na tlačítko ' Další &gt;":

![](aspnet-and-iis6/_static/image2.jpg)

Klikněte na tlačítko ' Další &gt;.

![](aspnet-and-iis6/_static/image3.jpg)

Na této obrazovce je potřeba vybrat "možnosti konfigurace aplikačního serveru (IIS, ASP.NET).

Klikněte na tlačítko ' Další &gt;".

![](aspnet-and-iis6/_static/image4.jpg)

Po výběru konfigurace serveru jako aplikační Server, se zobrazí tato obrazovka s výzvou, jaké další možnosti, které se má nainstalovat. Ve výchozím nastavení je vybrána ani jedna možnost. Umožňuje automaticky technologie ASP.NET, je nutné vybrat "Povolit ASP. NET ".

Klikněte na tlačítko ' Další &gt;".

![](aspnet-and-iis6/_static/image5.jpg)

Tato obrazovka zobrazí, jaké možnosti jsou k instalaci.

Klikněte na tlačítko ' Další &gt;".

![](aspnet-and-iis6/_static/image6.jpg)

Během instalace možností, které jste vybrali, se zobrazí tato obrazovka. Je normální jiné dialogové okno, které zobrazí pole, protože probíhá instalace služeb. Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.

Klikněte na tlačítko ' Další &gt;' po dokončení.

![](aspnet-and-iis6/_static/image7.jpg)

Klikněte na tlačítko 'Dokončit' - Windows Server 2003 je nyní nakonfigurováno pro podporu služby IIS 6.0 a ASP.NET 1.1.

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a>Povolení služby IIS, možnost #2 – Ruční konfigurace služby IIS a ASP.NET

Pokud nechcete použít 'Server Průvodce konfigurací' Volitelně můžete nainstalovat IIS 6.0 a ASP.NET 1.1 pomocí 'Přidat nebo odebrat programy' v Ovládacích panelech.

Nejprve otevřete ovládací Panel:

![](aspnet-and-iis6/_static/image8.jpg)

Pak klikněte na ' Přidat nebo odebrat Windows součásti' tím se otevře Průvodce součásti Windows:

![](aspnet-and-iis6/_static/image9.jpg)

Zvýrazněte a "Aplikační Server" a potom klikněte na "Podrobnosti?" Tlačítko:

![](aspnet-and-iis6/_static/image10.jpg)

Instalace technologie ASP.NET, zkontrolujte ' ASP. NET ".

Klikněte na tlačítko "OK" se vraťte do Průvodce komponentami Windows. Klikněte na tlačítko ' Další &gt;"z Průvodce komponentami Windows k zahájení instalace:

![](aspnet-and-iis6/_static/image11.jpg)

Je normální jiné dialogové okno, které zobrazí pole, protože probíhá instalace služeb. Kromě toho můžete být vyzváni k zadání umístění instalačního disku CD Windows serveru 2003.

Po dokončení instalace se zobrazí poslední obrazovce Průvodce komponentami Windows:

![](aspnet-and-iis6/_static/image12.jpg)

Služba IIS 6.0 a ASP.NET 1.1 jsou teď nakonfigurované a dostupné.

## <a name="recommended-settings"></a>Doporučená nastavení

Při spuštění ASP.NET 1.1 se službou IIS 6.0, existuje několik nastavení konfigurace, které se doporučuje, abyste získali optimální výkon z technologie ASP.NET:

- Konfigurace omezení paměti pracovního procesu
- Konfigurace pracovní proces recykluje

### <a name="configuring-worker-process-memory-limits"></a>Konfigurace omezení paměti pracovního procesu

Ve výchozím nastavení služby IIS 6.0 omezený objem paměti, které může IIS používat nenastaví. ASP. NET vaší mezipaměti funkce závisí na omezení paměti, proto do mezipaměti proaktivně odebrat nepoužité položky z paměti.

Doporučuje se, že nakonfigurujete funkce služby IIS 6.0 recyklace paměti. Ke konfiguraci této otevřete Správce Internetové informační služby (spuštění | Programy | Nástroje pro správu | Internetová informační služba). Jakmile otevřené, rozbalte složku "Fondy aplikací":

Pro každý fond aplikací:

![](aspnet-and-iis6/_static/image13.jpg)

1. Klikněte pravým tlačítkem na fond aplikací, například "DefaultAppPool" a vyberte 'vlastnosti':

![](aspnet-and-iis6/_static/image14.jpg)

2. Dál povolte recyklace paměti po kliknutí na buď "maximální využité paměti (v megabajtech):". Hodnota by neměla být delší než množství fyzické paměti (ne virtual) na serveru, je dobré aproximace 60 % fyzické paměti, například pro server s 512 MB fyzické paměti, vyberte 310. Doporučuje se také, že maximální nepřekročí 800MB při použití adresní prostor 2GB. Pokud se adresní prostor paměť serveru je 3GB, maximální limit paměti pro pracovní proces může být až 1 800MB:

![](aspnet-and-iis6/_static/image15.jpg)

Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogové okno Vlastnosti. Tento postup opakujte pro všechny fondy aplikací k dispozici.

### <a name="configuring-worker-recycling"></a>Konfigurace recyklování pracovního procesu

Ve výchozím nastavení služby IIS 6.0 konfigurován recyklaci jeho pracovní proces každých 29 hodin. Toto je o něco agresivní pro aplikace používající technologii ASP.NET a je doporučeno, recyklace automatické pracovních procesů je zakázané.

Chcete-li zakázat recyklace automatického pracovního procesu, nejprve otevřete Správce Internetové informační služby (spuštění | Programy | Nástroje pro správu | Internetová informační služba). Jakmile otevřené, rozbalte složku "Fondy aplikací":

![](aspnet-and-iis6/_static/image16.jpg)

Pro každý fond aplikací:

1. Klikněte pravým tlačítkem na fond aplikací, například "DefaultAppPool" a vyberte 'vlastnosti':

![](aspnet-and-iis6/_static/image17.jpg)

2. Zrušte zaškrtnutí políčka ' recyklovat pracovní proces (v minutách): ":

![](aspnet-and-iis6/_static/image18.jpg)

Klikněte na tlačítko 'Použít' a 'OK' ukončíte dialogové okno Vlastnosti. Tento postup opakujte pro všechny fondy aplikací k dispozici.

## <a name="granting-write-access-to-the-file-system"></a>Udělení oprávnění k zápisu do systému souborů

Pokud vaše aplikace vyžaduje oprávnění k zápisu do systému souborů a používáte je potřeba upravit řízení přístupu seznamu (ACL) na složku nebo soubor udělit technologii ASP.NET přístup do systému souborů NTFS.

Například udělit ASP.NET oprávnění k zápisu do c:\inetpub\wwwroot nejprve otevřete Průzkumníka a přejděte do adresáře:

![](aspnet-and-iis6/_static/image19.jpg)

Pak klikněte pravým tlačítkem na adresář, např "wwwroot" a vyberte možnost Vlastnosti. Po otevření dialogového okna Vlastnosti vyberte kartu "Zabezpečení":

![](aspnet-and-iis6/_static/image20.jpg)

Adresář c:\inetpub\wwwroot\ je zvláštní v dané speciální skupinu služby IIS 6.0 "IIS\_WPG' je již oprávnění ke čtení &amp; oprávnění spouštět, zobrazovat obsah složky a číst. Ale pokud chcete udělit oprávnění k zápisu, potřebujete klikněte na zaškrtávací políčko Povolit pro zápis:

![](aspnet-and-iis6/_static/image21.jpg)

Služba IIS 6.0 teď má oprávnění k zápisu pro tuto složku. Udělení oprávnění k zápisu v jiných složkách, postupujte podle těchto kroků – mějte na paměti, budete muset přidat IIS\_WPG skupina, pokud ještě neexistuje.

> [!CAUTION]
> Udělení oprávnění zapisovat do služby IIS\_WPG vám umožní libovolné aplikaci ASP.NET k zápisu do tohoto adresáře.

## <a name="supporting-integrated-authentication-with-sql-server"></a>Podpora integrované ověřování s využitím SQL serveru

Integrované ověřování umožňuje serveru SQL Server využívat ověřování systému Windows NT pro ověření serveru SQL Server při účty přihlášení. To umožňuje uživateli obejít standardní proces přihlášení serveru SQL Server. S tímto přístupem sítě uživatel může přistupovat k databázi serveru SQL Server bez poskytnutí samostatné přihlašovací identifikátor nebo heslo, protože systém SQL Server získá uživatele a informace o hesle z procesu zabezpečení sítě systému Windows NT.

Výběr integrované ověřování pro aplikace ASP.NET je dobrou volbou, protože žádné přihlašovací údaje se nikdy neuloží do připojovacího řetězce pro vaši aplikaci. Připojovací řetězec použitý pro připojení k SQL místo toho bude vypadat takto:

`"server=localhost; database=Northwind;Trusted_Connection=true"`

Tento připojovací řetězec říká systému SQL Server pomocí přihlašovacích údajů Windows aplikace pokusu o přístup k systému SQL Server. V případě ASP.NET/IIS 6, to může být účet ve službě IIS\_WPG skupiny.

Pokud chcete povolit integrované ověřování mezi SQL serverem a technologie ASP.NET, budete muset nejprve zkontrolujte, že systém SQL Server je nakonfigurován pro integrované ověřování nebo smíšený režim ověřování – obraťte se na správce databáze ke zjištění. Pokud je SQL Server v jednom z těchto dvou režimů, můžete použít integrované ověřování.

Otevřete správce systému SQL Server Enterprise (Start | Programy | Microsoft SQL Server | Správce rozlehlé sítě), vyberte příslušný server a rozbalte složku zabezpečení:

![](aspnet-and-iis6/_static/image22.jpg)

Pokud se BUILTINT\IIS\_WPG "skupina není uvedena, klikněte pravým tlačítkem na přihlášení a výběr nového přihlašovacího jména:

![](aspnet-and-iis6/_static/image23.jpg)

V "název:" textového pole zadejte "[název serveru/domény] \IIS\_WPG" nebo kliknutím na tlačítko se třemi tečkami otevřete nástroj pro výběr uživatele nebo skupiny systému Windows NT:

![](aspnet-and-iis6/_static/image24.jpg)

Vyberte aktuálního počítače IIS\_WPG skupiny a klikněte na tlačítko "Přidat" a OK zavřete Nástroj pro výběr.

Pak musíte také nastavit výchozí databáze a oprávnění pro přístup k databázi. Nastavit výchozí databázi vyberte z rozevíracího seznamu, například Northwind níže je vybrána:

![](aspnet-and-iis6/_static/image25.jpg)

Pak klikněte na kartě přístup k databázi:

![](aspnet-and-iis6/_static/image26.jpg)

Klikněte na zaškrtávací políčko Povolit pro každou databázi, kterou chcete povolit přístup k. Budete také muset vyberte databázové role, kontrola db\_vlastníka zajistí vaše přihlašovací údaje má všechna potřebná oprávnění ke správě a použít vybraná databáze.

Klikněte na tlačítko OK zavřete dialogové okno vlastností. Vaše aplikace ASP.NET je nyní nakonfigurováno pro podporu integrované ověřování systému SQL Server.

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a>Nelze spustit ASP.NET 1.0 v nativním režimu služby IIS 6.0

ASP.NET 1.0 ve službě IIS 6.0 je podporována pouze v režimu kompatibility služby IIS 5.

Ke konfiguraci ASP.NET 1.0 ke spuštění v režimu kompatibility služby IIS 5.0, otevřete Správce internetové služby a klikněte pravým tlačítkem myši klikněte na tlačítko weby a vyberte vlastnosti:

![](aspnet-and-iis6/_static/image27.jpg)

Přepněte na kartu služby a zkontrolujte? Spustit webovou službu v režimu izolace 5.0 IIS?

![](aspnet-and-iis6/_static/image28.jpg)
