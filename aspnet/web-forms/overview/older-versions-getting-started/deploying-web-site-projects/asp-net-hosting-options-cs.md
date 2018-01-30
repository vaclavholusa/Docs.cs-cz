---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: "ASP.NET hostování možnosti (C#) | Microsoft Docs"
author: rick-anderson
description: "Webové aplikace ASP.NET obvykle jsou navržené tak, vytvořili a testovány v místní vývojové prostředí a je nutné nasadit až produkčního prostředí o..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 34e1f9c7ee1ae22bceb614eeeaa1ebe286c1ccad
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-hosting-options-c"></a>Hostování prostředí ASP.NET možnosti (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhnout PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Webové aplikace ASP.NET se obvykle navržená tak, vytvořit a otestovat v místní vývojové prostředí a potřeba nasadit do produkčního prostředí, až bude připravený pro verzi. Tento kurz obsahuje podrobný přehled procesu nasazení a slouží jako úvod do tohoto kurzu řady.


## <a name="introduction"></a>Úvod

Webové aplikace jsou obvykle navržená tak, vytvořit a otestovat ve vývojovém prostředí, které je přístupné pouze pro programátory práce na webu. Jakmile aplikace je připravena k uvolnění, se přesune do provozního prostředí kde webu je přístupný každému uživateli na Internetu. Tento proces nasazení zavádí určité problémy:

- Provozním prostředí musí existovat a být správně instalační program, než lze nasadit aplikace ASP.NET; Kromě toho provozním prostředí musí být udržovány aktuálním stavu pomocí nejnovějších oprav zabezpečení.
- Správnou sadu souborů značek, soubory kódu a podpůrné soubory musí být zkopírovány z vývojového prostředí do produkčního prostředí. Pro datové aplikace může vyžadovat kopírování schématu databáze nebo data, také.
- Mohou existovat konfigurace rozdíly mezi těmito dvěma prostředími. Připojovací řetězec nebo e-mailu serverem databáze používá ve vývojovém prostředí budou pravděpodobně lišit od produkčního prostředí. Chování aplikace může navíc závisí na prostředí. Například když dojde k chybě při vývoji na obrazovce lze zobrazit podrobnosti k chybě, ale když dojde k chybě v produkčním prostředí, místo toho by měla být zobrazí uživatelsky přívětivý chybová stránka a podrobnosti o chybě e-mailem pro vývojáře.

Aby se předešlo v prvním kroku – nastavení a údržbu provozním prostředí - mnoho uživatele a organizace externí jejich produkční prostředí *webové poskytovatelé hostingu*. Poskytovatele webových hostitelských služeb je ve firmě, která spravuje produkčního prostředí vaším jménem. Existuje obrovském množství webového hostitele poskytovatelů, každý s různými ceny a úrovně služeb; Tipy k vyhledání poskytovatele služeb v části "Hledání webového hostitele zprostředkovatele".

Toto je první ze série kurzů, které podívejte se na postup nasazení webové aplikace do provozního prostředí spravovaný poskytovatelem hostitele webové služby ASP.NET. V průběhu následujících kurzech jsme zkontrolujte:

- Jaké soubory musí být nasazeny ke zprostředkovateli webového hostitele.
- Nástroje pro zjednodušení procesu nasazení.
- Postup nasazení databáze.
- Tipy pro nasazení databáze, která používá [zprostředkovatele SQL na základě členství a rolí](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), společně s způsoby tak, aby napodoboval nástroj pro správu webu v produkčním prostředí.
- Strategie pro plynule aktualizace databáze v produkčním prostředí se změny provedené během vývoje.
- Techniky pro protokolování chyb, ke kterým došlo na produkční a způsoby, jak vývojáři upozornit, když dojde k chybě.

Tyto kurzy jsou mají za cíl se stručným a poskytnout podrobné pokyny s dostatkem snímky obrazovky vizuálně provedou celým procesem. V tomto kurzu zahajovací obsahuje přehled procesu nasazení technologie ASP.NET a Rady, jak na hledání webové poskytovatele hostitelských služeb. Můžeme začít!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Přehled procesu nasazení technologie ASP.NET

Stručně řečeno nasazení aplikace ASP.NET zahrnuje následující tři kroky:

1. Konfigurovat webovou aplikaci, webový server a databáze v provozním prostředí.
2. Synchronizovat stránek ASP.NET, soubory kódu, sestavení v `Bin` složku a související HTML podpůrné soubory, jako jsou soubory šablon stylů CSS a JavaScript.
3. Synchronizujte schématu databáze nebo data.

Informace o konfiguraci pro webovou aplikaci se obvykle nachází v `Web.config` souboru a zahrnuje databázové připojovací řetězce, kritéria zpracování chyb, pravidla a informace o poštovním serveru přepisování adres URL. Tyto informace se často liší pro aplikaci v vývoj versus stejnou aplikaci v produkčním prostředí. Například při vývoji aplikace je nejvhodnější použít databázi vývoj tak, aby nejsou testování proti produkční databázi. V důsledku toho databázové připojovací řetězce se obvykle liší mezi aplikacemi pro vývoj a provozní. Z důvodu tyto rozdíly součástí nasazení zahrnuje, změny na informace o konfiguraci webové aplikace.

Kromě změny konfigurace webové aplikace krok 1 také může mít za následek konfigurace pro webový server a databáze. Například pokud stránku ASP.NET vytvoří nebo odstraní soubory z adresáře na webovém serveru pak webového serveru musí být nakonfigurována tak, aby tyto úpravy systému souborů. Podobně může být nastavení oprávnění nebo ověřování, které je potřeba provést k databázi.


Krok 2 zahrnuje synchronizaci sadu nezbytné stránek ASP.NET a podpůrné soubory mezi vývoj a produkční prostředí. Konkrétní sada ASP. NET související soubory, které musí být synchronizovány mezi těmito dvěma prostředími závisí na typu projektu jste vytvořili v sadě Visual Studio a je diskuse v dalším kurzu [ *určení co soubory musí být nasazeny*](determining-what-files-need-to-be-deployed-cs.md). Třetí a čtvrtý kurzy - [ *nasazení vaše lokality pomocí protokolu FTP* ](deploying-your-site-using-an-ftp-client-cs.md) a [ *nasazení vaše lokality pomocí sady Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -zkontrolujte různé nástroje a techniky pro synchronizaci těchto souborů.

Při vytváření datové aplikace, které jsou obvykle dvě databáze používá: jeden pro vývoj a druhý na produkční. Během vývoje schéma databáze vývoj může upravit tak, aby zahrnují nové tabulky, sloupce, uložených procedur a aktivačních událostí, nebo může upravit tak, aby neodeberete nebo nepřejmenujete stávající databázové objekty. Mezi ve chvíli, tyto změny jsou vytvářeny a čas, kdy aplikace je nasazená do produkčního prostředí vývoj a provozní databáze nejsou synchronizovány. Tento asynchrony je potřeba opravit během procesu nasazení. Tyto problémy prozkoumá v budoucnu kurzy.

## <a name="finding-a-web-host-provider"></a>Hledání zprostředkovatele webového hostitele

Aplikace ASP.NET můžete nasadit na libovolný webový server, který má rozhraní .NET Framework a Internetové informační služby (IIS). Lokality může hostovat ze svého počítače, za předpokladu, že jste měli širokopásmové připojení k Internetu a si přehled postup konfigurace směrovači povolit příchozích webových požadavků. Lokality z počítače v intranetu, může taky hostovat, stejně jako mnoho společností. Fokus tyto kurzy, ale je hostování svého webu pomocí zprostředkovatele webového hostitele.

> [!NOTE]
> [Služba IIS](https://www.iis.net/) je Microsoftu na podnikové úrovni webovým serverem. Je dodáván s bez domovské edicemi systému Windows, například Windows Server 2008 a určitými edicemi systému Windows Vista. Není nutné instalace služby IIS k obsluze aplikace ASP.NET ve vývojovém prostředí sady Visual Studio obsahuje webový vývojový Server ASP.NET. Však webový vývojový Server ASP.NET přijímá pouze místní připojení a proto jej nelze použít v produkčním prostředí.


Před nasazením webu do webového hostitele zprostředkovatele musíte nejprve rozhodnout, jaké společnosti obchodování s. Existují obrovském množství webových hostitelských společností na webu Marketplace; Vyhledejte "webového hostingu společnosti" vrátí více než 5 milionů výsledky. Jak ten, který je pro vás nejvhodnější můžete najít? Váš oblíbený vyhledávací web je počáteční vhodná, jako jsou weby, jako je [TopHosts](http://www.tophosts.com/) a [HostCritique](http://www.hostcritique.net/), který porovnat a kontrastu různých hostitelských služeb. I taky poradit kolegy a spolupracovníky požadující žádná doporučení; také můžete požádat o doporučení [hostování otevřete fórum](https://forums.asp.net/158.aspx) sem na [ASP.NET fóra](https://forums.asp.net/).

Webových hostitelských společností obvykle nabízejí sdíleného hostingu plány a vyhrazené hostování plány. S sdílené hostování jednom webovém serveru hostitele desítek, pokud není stovky různých webů. S vyhrazenou hostování zapůjčení počítač od společnosti, která obsluhuje váš web a váš web samostatně. Sdílené hostování plán může obsahovat podporu pro stránky ASP.NET umožňuje pracovat s databází aplikace Microsoft Access, 5 GB místa na disku a 100 GB měsíčního výstupního provozu šířky pásma pro 9,95 za měsíc. Jiné sdílené hostování plán může obsahovat podporu pro stránky ASP.NET, přístup k databázi serveru Microsoft SQL Server 2008, 10 GB místa na disku a 250 GB měsíčního výstupního provozu šířky pásma pro 19,95 za měsíc. Vyhrazené hostování plány jsou obvykle výrazně dražší, nákladů několik set dolarů za měsíc, ale nabídka lepší výkon a větší kontrolu než sdílené hostování možnosti. Jaké plán zvolíte, závisí na vaší nároky, kolik provozu přijímá vašeho webu a funkce, které očekáváte, že je budete potřebovat.

Dvě důležité zvážit při výběru poskytovatele hostitele webových jsou oddělení zákaznických služeb a kvalitu služeb. Pokud máte otázky nebo k potížím s konfigurací, jak dlouho trvá odesílání váš problém na technickou podporu webového hostitele, dokud nezískáte odpověď ze? Jak spolehlivé jsou služby společnosti? Se často mají výpadků databáze? Jak často jejich e-mailový server přechodu do offline režimu? Můžete se vždy dotázat společnosti obsahují podrobné informace o jejich provozu a dotazovat se na zásady služby jejich zákazníků, ale více surefire způsob, jak je k získání zpětné aktuální a starší zákazníků, které můžete provést prostřednictvím online fóra, diskusní skupiny a listservs e-mailu .

> [!NOTE]
> Některé webových hostitelských společností zaměřit na konkrétní technologie zásobníku, jako je například .NET své firmy nebo [svítilna](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, a **P** HP), proto se ujistěte, že vyberete společnost hostuje aplikace ASP.NET. Zkontrolujte také, že podporují verzi technologie ASP.NET, který používáte pro sestavení aplikace. A pokud vytváříte aplikaci na základě dat, ujistěte se, že webový hostitel nabízí stejný databázový server a verze, kterou používáte.


## <a name="summary"></a>Souhrn

Webové aplikace ASP.NET jsou obvykle určený, vytvořit a testováno v místním vývojovém prostředí. Po připravená pro vydání verze je přesunuta do produkčního prostředí. I když je možné hostitele webů ASP.NET na svůj počítač nebo na serverech v rámci vaší společnosti, zvolte využít hostování ke zprostředkovateli webového hostitele k mnoha firmám a jednotlivce.

Tento kurz řady prověří kroky k nasazení aplikace technologie ASP.NET pro webové zprostředkovatele hostitele, prohlížení běžné problémy. V tomto kurzu nabízí souhrnné informace o procesu nasazení technologie ASP.NET a Dal tipy pro hledání poskytovatele webových vhodný hostitel. V dalším kurzu zjistí co soubory související s ASP.NET je nutné zkopírovat do produkčního prostředí při nasazování webu.

Radostí programování!

### <a name="special-thanks-to"></a>Zvláštní poděkování...

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Teresy Murphy. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Next](determining-what-files-need-to-be-deployed-cs.md)
