---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: Další postupy a pokyny (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: 0ea125dbb889df7f253e3aa4bda636b0c4b2bec6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371804"
---
<a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>Další postupy a pokyny (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Když jste teď zobrazené 13 vzory, které poskytují pokyny o tom, jak byli úspěšní ve cloud computingu. Toto jsou jen některé ze vzorů, které se vztahují ke cloudovým aplikacím. Tady jsou některé další témata cloud computingu a prostředky, které pomůžou s nimi:

- Migrace stávajících místních aplikací do cloudu. 

    - [Přesun aplikací do cloudu](https://msdn.microsoft.com/library/ff728592.aspx). E-kniha od Microsoft Patterns and Practices. K dispozici i jako [tištěné tištěné verze](https://www.amazon.com/dp/1621140202).
    - [Migrace Microsoft ASP.NET a IIS.NET](https://go.microsoft.com/fwlink/?LinkId=400656). Případová studie podle Robert McMurray.
    - [Přesunutí 4th &amp; primátor k webům Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/). Blogový příspěvek podle Jeffa Wilcox chronicling své prostředí, přesun webové aplikace z Amazon Web Services do služby Web Apps ve službě Azure App Service.
    - [Přesun aplikací do Azure: jaké změny?](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) Krátká videa podle Stefan Schackow, vysvětluje přístupu k systému souborů ve službě Web Apps ve službě Azure App Service.
    - [Azure Hybrid Cloud](https://www.amazon.com/dp/B00EOP4UQW). Kniha výtisk nebo e kniha od Danny Garber Jamal Malik a Adam Fazio.
- Zabezpečení, ověřování a autorizace problémy, které jsou jedinečné pro cloudové aplikace

    - [Pokyny pro zabezpečení Azure](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Model vrátný viz, model Federovaná identita.
    - [Zabezpečení sítě Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). Dokument White paper o Ashin Palekara.

Viz také další cloud computingu modely a pokyny na [Microsoft Patterns and Practices – pokyny k Azure](https://msdn.microsoft.com/library/dn568099.aspx).

<a id="resources"></a>
## <a name="resources"></a>Prostředky

Další informace o tomto konkrétním tématem jednotlivých kapitol v této e kniha obsahuje odkazy na prostředky. Následující seznam obsahuje odkazy na přehledy o osvědčené postupy a vzory doporučené úspěšné pro vývoj pro cloud s Azure.

Dokumentace

- [Osvědčené postupy pro navrhování rozsáhlých služeb v Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper Mark Simms a Michael Thomassy.
- [Bezporuchový: Pokyny, odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper Marc Mercuri, Ulrich Homann a Andrew Townhill. Webová stránka verze bezporuchový videu z řady.
- [Doprovodné materiály k Azure](https://azure.microsoft.com/develop/net/guidance/) stránku portálu oficiální dokumentaci související s vývojem aplikací pro Azure.

Videa

- [Sestavování skutečných cloudových aplikací s Azure – část 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) a [2. část](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325). Video na prezentaci Scotta guthrie, založenou na tuto elektronickou příručku. Uvedené v Austrálii Ed Odborný v září 2013. Starší verzi téže prezentaci doručenou v Ndc (Norwegian Developers Conference) v červnu 2013: [1. část Norwegian](http://vimeo.com/68215538), [2. část Norwegian](http://vimeo.com/68215602).
- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Marc Mercuri a Mark Simms. Představuje 400 úroveň zobrazení o tom, jak navrhovat cloudových aplikací. Tato série se zaměřuje na teorií a důvodech doporučené způsoby; Další postupy podrobnosti najdete v tématu Vytváření velký řady podle Mark Simms.
- [Vytváření Big: Získané z Azure zákazníky – část 1](https://channel9.msdn.com/Events/Build/2012/3-029) a [2. část](https://channel9.msdn.com/Events/Build/2012/3-030). Série videí dvojdílného Simon Davies a Mark Simms, podobně jako řady bezporuchový ale orientovaný více směrem k praktické implementace.

Ukázka kódu

- [Oprava aplikace, který doprovází elektronická kniha](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).
- [Cloud Service Fundamentals v Azure v jazyce C# pro sadu Visual Studio 2012](http://aka.ms/csf). Ke stažení projekt na webu Microsoft Code Gallery zahrnuje kód a dokumentace vyvinuté pomocí Microsoft zákazníka poradní tým (kočky). Ukazuje řadu osvědčených postupů v série videí bezporuchový a sestavování velkých objemů a dokument white paper bezporuchový práva v oblasti. Galerie kódu na stránce taky obsahuje odkazy na si rozsáhlou dokumentaci k autory projektu – viz zvlášť [Cloud Service Fundamentals wiki kolekce](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) odkaz modrá pole v horní části Popis projektu. Tento projekt a v dokumentaci pro něj stále aktivně vyvíjen, aby byla informace o mnoha témat vhodnější než podobné, ale starší dokumenty white paper.

Kopírovat pevný knihy

- [Cloud Computing biblické](https://www.amazon.com/dp/0470903562). Podle Barrie Sosinsky.
- [Uvolnění! Návrh a nasazení softwaru připravené pro produkční prostředí](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Zpopularizoval Michael T. Nygard.
- [Vzory cloudové architektury: Použití Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do). Podle Wilder vyúčtování.
- [Windows Azure Platform](https://www.amazon.com/dp/1430235632). Podle Tejaswiho jde.
- [Windows Azure programovací modely pro Start-upy](https://www.amazon.com/dp/1849685606). Podle Riccardo Becker.
- [Microsoft Windows Azure – vývoj kuchařka](https://www.amazon.com/dp/1849682224). Podle Neil Mackenzie.

Nakonec při můžete začít vytvářet skutečné aplikace a běží v Azure, dříve či později ho budete pravděpodobně potřebovat pomoc od odborníků. Můžete klást otázky v komunitním webům, jako [fóra Azure nebo StackOverflow](https://azure.microsoft.com/support/forums/), nebo přímo u podpory Azure můžete kontaktovat Microsoft. Společnost Microsoft nabízí několik úrovní technické podpory Azure: Přehled a porovnání možností najdete v tématu [podpory Azure](https://azure.microsoft.com/support/plans/).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Potvrzení

Tento obsah byl napsán tak, že Tomáš Dykstra, Rick Anderson a Mike Wasson. Většina původní obsah pochází z [Scott Guthrie](https://weblogs.asp.net/scottgu/), a že zase nakreslili na materiál z Mark Simms a Microsoft zákazníka poradní tým (KOČKA).

Mnoho kolegy v Microsoftu zkontroluje a označen jako koncepty a kód:

- TIM Ammann - zkontrolovat, jestli kapitoly automatizace.
- Christopher Bennage - přezkoumány a testovat kód opravit.
- Ryan bobulovinami - zkontrolovat, jestli kapitoly CD/CI.
- Vittorio Bertocci - zkontrolovat, jestli kapitoly jednotného přihlašování.
- Chris Clayton - pomohla řešení technických problémů ve skriptech Powershellu.
- Conor Cunningham - zkontrolovat, jestli kapitoly možnosti datového úložiště.
- Carlos Farre - přezkoumány a testovat kód Fix It pro problémy se zabezpečením.
- Larry Franks - zkontrolovat, jestli telemetrie a monitorování kapitoly.
- Jonathan Gao - MapReduce části kapitoly možnosti úložiště dat a zkontrolovat Hadoop.
- Sidney Higa - zkontrolovat, jestli všechny kapitol.
- Gordon Hogenson - zkontrolovat, jestli kapitoly zdrojového ovládacího prvku.
- Tamra Myers – možnosti úložiště přezkoumání dat, objektů blob a fronty kapitol.
- Pranav Rastogi - zkontrolovat, jestli kapitoly jednotného přihlašování.
- Červen blenderu Rogers – přidání zpracování chyb a nápovědu k automatizační skripty prostředí PowerShell.
- Mani Subramanian - zkontroluje všechny kapitol a vedla revize kódu a testování procesu pro kód opravit.
- Shaun Tinline-Jones - zkontrolovat, jestli dělení kapitoly dat.
- Selcin Tukarslan - přezkoumání kapitol, které se týkají SQL Database a SQL Server.
- EDWARD Wu – k dispozici ukázkový kód pro kapitoly jednotného přihlašování.
- Guang Yang - napsal automatizační skripty prostředí PowerShell.

Členové [Advisory Council pokyny Microsoftu pro vývojáře](http://aka.ms/DGAC) (DGAC) také zkontroluje a označen jako koncepty:

- Boucho Jean Luc
- Catalin Gheorghiu
- Wouter de Kort
- Carlos dos Santos
- Neil Mackenzie
- Dennis Persson
- Simonu Sabat
- [Aleksey Sinyagin](http://www.linkedin.com/in/sinyagin)
- Bill Wagnera
- Michael dřeva

Ostatní členové DGAC přezkoumány a okomentováno předběžné osnovy:

- Damir Arh
- EDWARD Bakker
- Srdjan Bozovic
- Jde Man Chan
- Gianni Rosa Gallina
- Paulo Morgado
- JASON Oliveira
- Alberto Poblacion
- Ryan Rileymu
- Tsisah PEREZ Jones
- Roger Whitehead
- Pawel Wilkosz

> [!div class="step-by-step"]
> [Předchozí](queue-centric-work-pattern.md)
> [další](the-fix-it-sample-application.md)
