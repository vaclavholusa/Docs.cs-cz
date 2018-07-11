---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: Technologie ASP.NET (VB) možnosti hostování | Dokumentace Microsoftu
author: rick-anderson
description: Webové aplikace ASP.NET jsou obvykle navrženy, vytvořili a otestovat v místním vývojovém prostředí a musí být nasazeny produkčního prostředí o...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 68f12de0b459c01ecf766e09144364a64e2d69d3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817707"
---
<a name="aspnet-hosting-options-vb"></a>Možnosti hostování v technologii ASP.NET (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Webové aplikace ASP.NET jsou obvykle navržené, vytvořit a otestovat v místním vývojovém prostředí a potřebujete k nasazení do produkčního prostředí, až bude připravená pro vydanou verzi. Tento kurz poskytuje základní přehled o procesu nasazení a slouží jako úvod k této sérii kurzů.


## <a name="introduction"></a>Úvod

Webové aplikace jsou obvykle navrženy, vytvořili a testovat v prostředí pro vývoj, který je přístupný pouze pro programátory práce na webu. Když aplikace je připravená uvolnit, se přesune do produkčního prostředí kde webu je přístupný všem uživatelům Internetu. Tento proces nasazení představuje určité problémy:

- Musí existovat a být správně nastaveny, předtím, než je možné nasadit aplikaci ASP.NET; produkčním prostředí Kromě toho produkčního prostředí se musí zajistit aktuální s nejnovějšími opravami zabezpečení.
- Správnou sadu souborů značek, soubory kódu a podpůrné soubory musí být zkopírován z vývojového prostředí do produkčního prostředí. Pro datově řízené aplikace to může vyžadovat kopírování schéma databáze nebo data, při selhání.
- Mohou existovat rozdíly mezi těmito dvěma prostředími konfigurací. Připojovací řetězec nebo e-mailu serveru databáze používá ve vývojovém prostředí budou pravděpodobně lišit od produkční prostředí. A co víc chování aplikace může záviset na prostředí. Například při výskytu chyby ve vývoji podrobnosti o chybě lze zobrazit na obrazovce, ale v případě chyby v produkčním prostředí by měl být místo toho zobrazen uživatelsky přívětivou chybovou stránku a podrobnosti o chybě e-mailem pro vývojáře.

Aby se předešlo první před obrovskou výzvou – nastavení a udržování produkčního prostředí – mnoho jednotlivců a podniků externí pomocí svých produkčních prostředích k *webové poskytovatelé hostingu*. Poskytovatele webových hostitelských služeb je společnost, která spravuje v provozním prostředí vaším jménem. Existují aplikací webového hostitele zprostředkovatele, každý s různou cen a úrovní služeb; v části "Hledání hostiteli poskytovatele webových" tipy týkající se vyhledání poskytovatele služeb.

Toto je první ze série kurzů, které podívejte se na postup nasazení webové aplikace ASP.NET do produkčního prostředí spravované poskytovatelem webového hostitele. V průběhu těchto kurzů prozkoumáme:

- Jaké soubory je potřeba nasadit ke zprostředkovateli webového hostitele.
- Nástroje pro zjednodušení procesu nasazení.
- Postup nasazení databáze.
- Tipy pro nasazení, která používá databázi [zprostředkovatele SQL na základě členství a rolí](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), stejně jako se způsoby tak, aby napodoboval nástroje pro správu webu v produkčním prostředí.
- Strategie pro plynule aktualizace databáze v produkčním prostředí se změnami provedenými během vývoje.
- Techniky pro protokolování chyb, ke kterým dochází v produkčním prostředí a způsoby, jak upozornit vývojáře, dojde k chybě.

Tyto kurzy jsou zaměřené stručné a poskytují podrobné pokyny s dostatečným snímky obrazovky, který vás provede procesem vizuálně. V tomto kurzu zahajovací poskytuje přehled o procesu nasazení technologie ASP.NET a Rady, jak na hledání na webu poskytovatele hostitelských služeb. Pusťme se do práce!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Přehled procesu nasazení technologie ASP.NET

Řečeno v kostce nasazení aplikace ASP.NET zahrnuje následující tři kroky:

1. Konfigurace webové aplikace, webový server a databáze v produkčním prostředí.
2. Synchronizovat stránek ASP.NET, soubory kódu, sestavení v `Bin` složky a související s HTML podpůrné soubory, jako jsou soubory šablon stylů CSS a JavaScriptu.
3. Synchronizujte schéma databáze nebo data.

Informace o konfiguraci pro webovou aplikaci se obvykle nachází v `Web.config` souboru a obsahuje databázové připojovací řetězce, zpracování chyb kritéria přepsání pravidel a informace o serveru e-mailovou adresu URL. Tyto informace se často liší pro aplikace ve vývoji a stejné aplikace v produkčním prostředí. Například při vývoji aplikace je nejvhodnější použít databázi vývoje tak, že nejsou testy na produkční databázi. V důsledku toho připojovací řetězce databáze obvykle lišit mezi vývojovou a provozní aplikace. Kvůli těmto rozdílům zahrnuje nasazování změn informace o konfiguraci webové aplikace.

Kromě změny konfigurace webové aplikace krok 1 také může mít za následek konfigurace pro webový server a databáze. Například pokud stránky ASP.NET vytvoří nebo odstraní soubory z adresáře na webovém serveru pak webový server je potřeba nakonfigurovat tak, aby povolovala tyto úpravy systému souborů. Podobně mohou být oprávnění nebo nastavení ověření, které je potřeba provést k databázi.


Krok 2 zahrnuje synchronizaci sady základní stránky technologie ASP.NET a podpůrné soubory mezi vývojovou a provozní prostředí. Konkrétní sada ASP. NET související soubory, které je třeba se dá provést synchronizace mezi těmito dvěma prostředími závisí na typu projektu vytvořené v sadě Visual Studio a je diskuze v dalším kurzu  <em>[určující, co soubory musí být nasazeny](determining-what-files-need-to-be-deployed-vb.md)</em>. Třetí a čtvrtá kurzy -  <em>[nasazení vašeho webu pomocí protokolu FTP](deploying-your-site-using-an-ftp-client-vb.md)</em>a <em>[nasazení webu pomocí sady Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> – prozkoumejte různé nástroje a techniky pro synchronizaci těchto souborů.

Při sestavování aplikací řízených daty jsou obvykle dvě databáze používá: jeden pro vývoj a jeden v produkčním prostředí. Během vývoje schéma databáze vývoje může upravit tak, aby zahrnout nové tabulky, sloupce, uložené procedury a triggery nebo mohou být upraveny odebrat nebo přejmenovat stávající databázové objekty. Mezi časem, které budou provedeny tyto změny a čas, kdy aplikace je nasazená do produkčního prostředí nejsou synchronizované, vývoje a provozu databáze. Tato asynchronie je potřeba opravit během procesu nasazení. Tyto problémy se dají prozkoumat v budoucích kurzech.

## <a name="finding-a-web-host-provider"></a>Hledání zprostředkovateli webového hostitele

Aplikace ASP.NET je nasadit na jakékoli webové servery, který má rozhraní .NET Framework a Internetové informační služby (IIS). Může hostovat lokalitu z počítače, za předpokladu, že jste měli širokopásmové připojení k Internetu a Přehled konfigurace směrovače pro povolení příchozích webových požadavků. Může také hostujete lokality z počítače v intranetu, stejně jako mnoho společností. Zaměření pro tyto kurzy, ale je hostitelem vašeho webu pomocí zprostředkovatele webového hostitele.

> [!NOTE]
> [Služba IIS](https://www.iis.net/) je webový server od Microsoftu na podnikové úrovni. Je dodáván s – Domovská stránka edice systému Windows, jako je například Windows Server 2008 a určitými edicemi systému Windows Vista. Instalace služby IIS k obsluze aplikace ASP.NET ve vývojovém prostředí sady Visual Studio obsahuje webový Server ASP.NET Development nepotřebujete. Webový Server ASP.NET Development však přijímá pouze místní připojení a proto jej nelze použít v produkčním prostředí.


Před nasazením webu poskytovatele webového hostitele je nutné rozhodnout údaje, které společnost uzavírat obchody se. Existuje bezpočet webhosting společností ve službě na webu marketplace. Vyhledejte "webhosting společnosti" vrátí více než 5 milionů výsledky. Jak najít ten, který je pro vás nejvhodnější? Váš oblíbený vyhledávací web není dobrý výchozí bod, protože jsou z webových, jako jsou [TopHosts](http://www.tophosts.com/) a [HostCritique](http://www.hostcritique.net/), který porovnání a kontrast různých hostitelských služeb. Jsem také seznámit s kolegy a spolupracovníky žádostí o všechna doporučení; Můžete také požádat o doporučení v [hostování otevřené fórum](https://forums.asp.net/158.aspx) tady na [fóra ASP.NET](https://forums.asp.net/).

Webové hostingové společnosti obvykle nabízejí sdílené plány hostování a vyhrazené plány hostování. Pomocí sdílené hostování jediném webovém serveru hostitele desítky, pokud není stovky různých webů. S vyhrazený hosting zapůjčení počítač ze společnosti, která slouží pouze váš web a Web. Sdílený plán hostování může zahrnují podporu pro stránky ASP.NET umožňuje pracovat s databází Microsoft Access, 5 GB místa na disku a 100 GB měsíčního výstupního provozu šířky pásma pro 9.95 $ za měsíc. Jiný sdílený plán hostování může zahrnují podporu pro stránky ASP.NET, přístup k databázi serveru Microsoft SQL Server 2008, 10 GB místa na disku a 250 GB měsíčního výstupního provozu šířky pásma pro 19,95 USD za měsíc. Vyhrazené plány hostování jsou obvykle mnohem dražší, ocenění několik stovek dolarů za měsíc, ale nabízejí lepší výkon a větší kontrolu, než sdílený možnosti hostování. Jaký plán zvolíte, závisí na rozpočtu, kolik provozu webu přijímá a funkce, které očekáváte, že je budete potřebovat.

Dva důležité aspekty při výběru poskytovatele webových hostitele jsou zákaznických služeb a kvalitu služeb. Pokud máte dotazy nebo potíže s konfigurací, jak dlouho trvá v odesílání váš problém pro pracovníka webového hostitele, dokud získat odpověď? Jak spolehlivé jsou služby vaší společnosti? Často mají výpadky databáze? Jak často jejich e-mailový server přejdou do režimu offline? Můžete se vždy dotázat společnosti obsahují podrobné informace o jejich dostupnost a dotazovat se na svoje zásady služby zákazníka, ale více surefire způsob, jak se k vyžádání zpětné vazby z aktuálního i staršího zákazníků, které vám pomůžou prostřednictvím online fóra, diskusní skupiny a listservs e-mailu .

> [!NOTE]
> Některé webové hostingové společnosti soustředit svoje podnikání na konkrétní technologiích, jako je .NET nebo [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, a **P** HP), proto se ujistěte, že společnost vyberete hostuje aplikace ASP.NET. Také zkontrolujte, že podporují verzi technologie ASP.NET, který používáte k sestavení aplikace. A pokud vytváříte aplikace řízené daty, ujistěte se, že nabízí webového hostitele na stejný server databáze a verze, kterou používáte.


## <a name="summary"></a>Souhrn

Webové aplikace ASP.NET jsou obvykle navržené, vytvořit a otestovat v místním vývojovém prostředí. Jakmile je připraven k vydání verze, se přesune do produkčního prostředí. I když je možné k hostování webů ASP.NET na váš osobní počítač nebo na serverech v rámci vaší společnosti, zvolte využívající hostování webového hostitele poskytovatele mnoho firmy i jednotlivce.

V této sérii kurzů prozkoumá postup nasazení aplikace ASP.NET u poskytovatele webových hostitele, zkoumání běžných problémů. Tento kurz nabízí přehled procesu nasazení technologie ASP.NET a dala tipy pro vyhledání vhodné webový poskytovatel hostitele. V dalším kurzu se prohledá co soubory související s ASP.NET je nutné zkopírovat do produkčního prostředí při nasazování webu.

Všechno nejlepší programování!

### <a name="special-thanks-to"></a>Speciální k...

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Teresy Murphy. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Předchozí](users-and-roles-on-the-production-website-cs.md)
> [další](determining-what-files-need-to-be-deployed-vb.md)
