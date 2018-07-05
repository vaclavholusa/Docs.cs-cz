---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Přechodná chyba zpracování (sestavování skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 13ed8c2373c22070d21519bc495161e956b0ac4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398411"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>(Vytváření skutečných cloudových aplikací s Azure) zpracování přechodných chyb
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Při navrhování reálného světa cloudové aplikace, jednou z věcí, které musíte zamyslet se zpracování přerušení dočasné služeb. Tento problém je jednoznačně důležité v cloudových aplikacích, protože jste tak závislá na připojení k síti a externí služby. Často můžete získat trochu potížím s vykreslováním, které jsou obvykle opravy, a pokud nejste připraveni je inteligentně, budete mít ve špatné pro vaše zákazníky.

## <a name="causes-of-transient-failures"></a>Způsobí, že přechodných selhání

V prostředí cloud, ve kterém zjistíte, která se nezdařila a vyřadit připojení k databázi dojít pravidelně. To je částečně proto, že se vám bude prostřednictvím další nástroje pro vyrovnávání zatížení, které jsou ve srovnání s místním prostředím kdy webový server a databázový server mají přímé fyzické připojení. Také někdy po závisí na víceklientské služby zobrazí volání služby získat pomalejší nebo vypršení časového limitu protože někdo jiný, který používá službu je silně narazili. V jiných případech může být uživatel, který příliš často dosahuje služba a služba záměrně omezit aby se zabránilo nepříznivě ovlivňují ostatní tenanty služby můžete – odepře připojení –.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Použijte inteligentní opakování/regresní logiku ke zmírnění efektu přechodných chyb

Namísto vyvolání výjimky a zobrazení stránky není k dispozici nebo chyby pro vaše zákazníky, dokáže rozpoznat chyby, které jsou nejčastěji přechodné a automaticky opakovat operaci, která způsobila chybu, v doufá, který předtím, než dlouho budete mít úspěšné. Ve většině případů operace proběhne úspěšně na druhý pokus a budete moci obnovit z chyby bez zákazníků s byl někdy vědět, že došlo k potížím.

Existuje několik způsobů, jak můžete implementovat logiku inteligentní opakování.

- Microsoft Patterns &amp; má skupina postupy [přechodné Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , který provádí všechno, co pro vás Pokud používáte ADO.NET pro přístup k SQL Database (ne přes rozhraní Entity Framework). Stačí nastavit zásady opakování – jak často má opakování dotazu nebo příkaz a jak dlouho se má čekat mezi pokusy – a zabalte SQL kódu v *pomocí* bloku.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Také podporuje TFH [mezipaměť In-Role Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) a [služby Service Bus](https://azure.microsoft.com/services/service-bus/).
- Při použití rozhraní Entity Framework obvykle nepracujete přímo s připojení SQL, proto nelze použít tento balíček vzory a postupy, ale Entity Framework 6 vytvoří tento druh logiku opakování přímo do rozhraní. Podobným způsobem je zadat strategie opakování, a potom pomocí EF strategie je, že pokaždé, když přistupuje k databázi.

    Tuto funkci lze použít v aplikaci opravit, vše musíme udělat, je přidat třídu, která je odvozena z *DbConfiguration* a zapněte logika opakovaných pokusů.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Pro SQL Database výjimky, které rozhraní identifikuje jako nejčastěji přechodné chyby kód zobrazený dává pokyn EF a zkuste operaci zopakovat, až 3krát s exponenciální regresní prodlevu mezi opakovanými pokusy a Maximální zpoždění 5 sekund. Exponenciální regrese znamená, že po každé neúspěšné opakování se bude čekat delší dobu, než to zkusíte znovu. Pokud selžou i tři pokusy po sobě, vyvolají výjimku. Následující část o jističe vysvětluje, proč chcete exponenciální regresní a omezený počet opakovaných pokusů.

    Obdobným problémům může mít, pokud používáte službu Azure Storage jako aplikace Fix It pro objekty BLOB, a rozhraní .NET API klienta úložiště už implementuje stejný druh logiku. Stačí zadat zásady opakovaných pokusů, nebo ještě nemáte to provést, pokud budete spokojeni s výchozím nastavením.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Jističe

Tady je několik důvodů, proč není nutné opakovat příliš mnohokrát po příliš dlouhou dobu:

- Příliš mnoho uživatelů trvale opakování neúspěšných žádostí může poklesnout zkušeností jiných uživatelů. Pokud jsou miliony lidí, že všechny provedení opakované opakovat pokusy o můžete může obsadit fronty pro odeslání služby IIS a brání obsluhu požadavků, které je jinak může zpracovat úspěšně vaší aplikace.
- Pokud všem uživatelům se opakovaně pokouší o z důvodu selhání služby, existuje může být mnoho požadavků ve frontě, která získá službu zahlcenou po jeho spuštění k obnovení.
- Pokud je chyba z důvodu omezení šířky pásma a je na služba používá pro omezení časové okno, trvalé opakování může přesunout okno navýšení kapacity a způsobit, že omezení pokračujte.
- Můžete mít uživatelem čekajícím webové stránky k vykreslení. Provádění lidé čekání příliš dlouhý může být více obtěžující této poměrně rychle předobjednávky je a zkuste to znovu později.

Exponenciální regrese řeší některé z těchto problém omezením počtu opakovaných pokusů, které služba můžete získat z vaší aplikace. Ale musíte také mít *jističe*: to znamená, že na určitou opakujte prahová hodnota aplikace přestane opakování a přijímá nějaká jiná akce, jako je například jeden z následujících akcí:

- Vlastní použití náhradní lokality. Pokud nelze získat minimální cenu akcie z Reuters, možná můžete ho získat z Bloomberg; nebo pokud nemůže získat data z databáze, možná můžete ho získat z mezipaměti.
- Selhání pasivní. Pokud potřebujete ze služby není rigidní pro vaši aplikaci, stačí vrátíte hodnotu null při nemůže získat data. Pokud chcete zobrazit úlohu Fix It a neodpovídá služby Blob service, můžete zobrazit podrobnosti úlohy bez bitovou kopii.
- Selhání. Chyba uživatele mají předejít zahlcení služby s požadavky, které by mohly způsobit přerušení služeb pro ostatní uživatele nebo rozšířit omezovací okno zkuste to znovu. Můžete zobrazit přátelskou zprávou "zkuste to znovu později.".

Neexistuje žádné zásady opakovaných pokusů univerzální. Můžete opakovat je víckrát a počkat déle v procesu pracovního procesu na pozadí asynchronní než byste to udělali v synchronním webové aplikace, kde uživatel je čekání na odpověď. Můžete počkat déle mezi jednotlivými pokusy o služba relačních databází než byste to udělali pro službu cache service. Tady jsou některé ukázkové doporučené zásady získáte představu o tom, jak se může lišit čísla opakování. ("Rychlé první" znamená, že žádné zpoždění před prvním opakováním.

![Ukázkové zásady opakování](transient-fault-handling/_static/image1.png)

Pokyny zásady opakování SQL Database najdete v tématu [řešení přechodných chyb a chyb připojení ke službě SQL Database](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Souhrn

Opakování/regresní strategie může pomoct skrytí dočasné chyby zákazníkovi ve většině případů a společnost Microsoft poskytuje rozhraní, které vám umožní minimalizovat práci implementace strategii, ať už používáte Azure, ADO.NET nebo Entity Framework Služba úložiště.

V [další kapitolu](distributed-caching.md), se podíváme na to, jak ke zlepšení výkonu a spolehlivosti pomocí distribuované ukládání do mezipaměti.

## <a name="resources"></a>Prostředky

Další informace naleznete v následujících materiálech:

Dokumentace

- [Osvědčené postupy pro navrhování rozsáhlých služeb v Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper Mark Simms a Michael Thomassy. Podobně jako řady bezporuchový ale přejde do další postupy podrobnosti. V části telemetrických dat a diagnostiky.
- [Bezporuchový: Pokyny, odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper Marc Mercuri, Ulrich Homann a Andrew Townhill. Webová stránka verze bezporuchový videu z řady.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Zobrazit opakování vzor, model Plánovač-Agent-správce.
- [Odolnost proti chybám ve službě Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Příspěvek na blogu od Tony Petrossian.
- [Entity Framework – odolnost připojení / Logika opakování](https://msdn.microsoft.com/data/dn456835). Jak používat a přizpůsobit zpracování funkce Entity Framework 6 přechodných chyb.
- [Odolnost připojení a zachycení příkazů s Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Čtvrtý z devět částí série, ukazuje, jak vytvořit funkci odolnost připojení EF 6 pro službu SQL Database.

Videa

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri a Mark Simms devět částí série. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Přečtěte si diskuzi o jističe v epizodě 3 počínaje 40:55.
- [Vytváření Big: Získané od zákazníků Azure – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms hovoří o návrh pro případ selhání přechodné selhání zpracování a všechno, co instrumentace.

Ukázka kódu

- [Cloud Service Fundamentals v Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázková aplikace vytvořené Microsoft zákaznického poradního týmu Azure, který ukazuje, jak používat [Enterprise přechodné selhání zpracování bloku knihovny](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Další informace najdete v tématu [cloudové služby Data vrstva přístupu k Fundamentals – zpracování přechodných chyb](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH se doporučuje pro přístup k databázi pomocí ADO.NET přímo (bez použití rozhraní Entity Framework).

> [!div class="step-by-step"]
> [Předchozí](monitoring-and-telemetry.md)
> [další](distributed-caching.md)
