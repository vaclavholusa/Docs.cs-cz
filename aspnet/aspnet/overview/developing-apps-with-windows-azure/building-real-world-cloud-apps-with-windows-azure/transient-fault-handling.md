---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Přechodná chyba zpracování (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs
author: MikeWasson
description: Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 86bd67b04931ae2452f6e063e6475a434a0125bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Přechodná chyba zpracování (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Při návrhu cloudové aplikace skutečných, jednou z věcí, které je třeba myslet je způsob zpracování přerušení dočasné služeb. Tento problém je jednoznačně důležité pro cloudové aplikace, protože jste tak závislá na připojení k síti a externích služeb. Často můžete získat moc chyb, které jsou obvykle samoopravení, a pokud nejste připravené k jejich zpracování inteligentně, budete vést chybný přihlašování pro vaše zákazníky.

## <a name="causes-of-transient-failures"></a>Příčiny přechodných chyb

V prostředí cloudu, které zjistíte, se nezdařilo a přerušení připojení databáze dojít pravidelně. Který je částečně, protože jste průchodu přes další nástroje pro vyrovnávání zatížení, které jsou ve srovnání s místním prostředí, kde webový server a databázový server obsahovat přímé připojení k fyzické. Také v případech, kdy jste závisí na víceklientské služby se zobrazí volání get service pomalejší nebo vypršení časového limitu protože někdo jiný, který používá službu je výraznou stiskne. V jiných případech může být uživatel, který je příliš často stiskne službu a službu úmyslně omezí generovaný můžete – odmítne připojení – aby se zabránilo nepříznivě ovlivňuje ostatních klientů služby.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Použití inteligentního logiku opakování nebo back vypnout zmírnit účinku přechodných chyb

Místo vyvolání výjimky a zobrazení stránky není k dispozici nebo Chyba zákazníkovi, poznáte chyb, které jsou nejčastěji přechodné a automaticky opakujte operaci, který způsobil chybu, v doufá, který před dlouho budete úspěšné. Ve většině případů operace proběhne úspěšně na druhý pokus a budete zotavit z chyby bez zákazníka, které byly někdy vědět, že došlo k potížím.

Můžete implementovat logiku opakovaných pokusů inteligentní několika způsoby.

- Microsoft Patterns &amp; má skupina postupy [přechodné chyby zpracování bloku aplikace](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) který provádí všechno, co pro vás Pokud používáte pro přístup k databázi SQL (ne přes rozhraní Entity Framework) ADO.NET. Stačí nastavit zásady pro opakování – jak často má opakování dotazu nebo příkaz a jak dlouho chcete čekat mezi pokusů – a wrap vaše SQL code v *pomocí* bloku.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    Také podporuje TFH [mezipaměť hostovaná v instanci Role Azure](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) a [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Při použití rozhraní Entity Framework obvykle přímo s nepracujete připojení SQL, proto nemůžete použít tento balíček Patterns and Practices, ale Entity Framework 6 vytvoří tento druh logika opakovaných pokusů přímo do rozhraní. Podobným způsobem můžete zadat strategie opakování, a potom EF pomocí této strategie vždy, když přistupuje k databázi.

    Chcete-li použít tuto funkci v aplikaci opravit, všechny budeme muset udělat je přidat třídu, která je odvozena z *DbConfiguration* a zapněte logika opakovaných pokusů.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Výjimky databáze SQL, které rozhraní identifikuje jako obvykle přechodné chyby uvedeném kódu dá pokyn EF operaci opakujte až 3 x, s exponenciální back vypnout prodlevu mezi opakování a Maximální zpoždění 5 sekund. Exponenciální back vypnout znamená, že po každém opakování se nezdařila se bude čekat delší dobu, než to zkusíte znovu. Pokud jsou selhání tři pokusů v řádku, vyvolá výjimku. V následující části o moduly okruh dělení vysvětluje, proč chcete exponenciální back vypnout a omezený počet opakování.

    Pokud používáte službu Azure Storage, opravte ji aplikace nepodporuje pro objekty BLOB a rozhraní API klienta úložiště .NET již implementuje stejný druh logiku, může mít podobné problémy. Stačí zadat zásady opakovaných pokusů, nebo i nemáte k tomu, pokud budete spokojeni s výchozím nastavením.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Moduly okruh dělení

Tady je několik důvodů, proč nechcete opakujte příliš mnohokrát za příliš dlouho období:

- Příliš mnoho uživatelů trvale opakováním neúspěšných požadavků může poklesnout prostředí jiných uživatelů. Pokud miliony lidí, že všechny provedení opakované opakujte požadavků můžete může třeba zadávat fronty odesílání služby IIS a brání obsluhy požadavků, které je jinak může zpracovat úspěšně vaší aplikace.
- Pokud všichni opakuje z důvodu chyby služby, že může být mnoho požadavků zařazen do fronty, získá službu přenášeny po jeho spuštění obnovení.
- Pokud chyba z důvodu omezení a je okno čas služba používá pro omezení, může nepřetržitý opakování přesunutí tohoto okna a způsobit, že omezení pokračujte.
- Můžete mít uživatel čekání na webové stránky k vykreslení. Počkejte osoby provádění příliš dlouho může být více obtěžování této poměrně rychle radí je a zkuste to znovu později.

Exponenciální back vypnout řeší některé z těchto problém omezením frekvenci opakování, které služby můžete získat z vaší aplikace. Ale je také potřeba mít *moduly okruh dělení*: to znamená, že na určitou opakujte prahová hodnota aplikace přestane opakování a trvá některých jiných akcí, jako je například jeden z následujících:

- Vlastní záložní. Pokud uložených cena nelze získat z Reuters, možná můžete ho získat z Bloomberg; nebo pokud nelze získat data z databáze, možná můžete ho získat z mezipaměti.
- Selhání tichou. Pokud potřebujete ze služby není vše nebo nic pro vaši aplikaci, právě vraťte hodnotu null. nelze získat data. Pokud zobrazujete úlohu opravte ji a neodpovídá služby objektů Blob, můžete zobrazit podrobnosti úlohy bez bitovou kopii.
- Rychle se nezdaří. Chybu uživatele, aby nedošlo k zaplavení službu s opakujte požadavků, které by mohly způsobit přerušení služby pro ostatní uživatele nebo rozšířit omezovací okno. Můžete zobrazit přátelskou zprávou "opakujte akci později".

Neexistuje žádné zásady opakovaných pokusů univerzálně. Můžete opakovat vícekrát a ve pracovní proces pozadí asynchronní čekat déle, než byste v synchronní webové aplikace, kde je uživatel čekání na odpověď. Můžete počkat nebudou mezi jednotlivými pokusy o služba relační databáze než byste pro službu mezipaměti. Tady jsou některé ukázkové doporučená opakujte zásady, které můžete získat představu o tom, jak mohou lišit čísla. ("Rychlé první" znamená žádné zpoždění před první opakování.

![Ukázkové zásady opakování](transient-fault-handling/_static/image1.png)

Databáze SQL opakování zásad pokyny najdete v tématu [řešení přechodných potíží a chyb připojení k databázi SQL](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Souhrn

Strategie opakování nebo back vypnout může pomoci zajistit dočasné chyby neviditelná zákazník ve většině případů a společnost Microsoft poskytuje rozhraní, které vám pomůže minimalizovat práci implementace strategie, zda používáte ADO.NET Entity Framework a Azure Služba úložiště.

V [další kapitoly](distributed-caching.md), podíváme jak zvýšit výkon a spolehlivost pomocí distribuované ukládání do mezipaměti.

## <a name="resources"></a>Prostředky

Další informace naleznete v následujících materiálech:

Dokumentace

- [Osvědčené postupy pro návrh rozsáhlých služeb v cloudu Azure služeb](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Dokument White paper moduly SIMM značky a Michael Thomassy. Podobně jako u řady bezporuchový ale přejde do další postupy podrobnosti. Najdete v části Telemetrie a Diagnostika.
- [Bezporuchový: Pokyny pro odolné cloudové architektury](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Dokument White paper matolin Mercuri, Ulrich Homann a Andrew Townhill. Webová stránka verze série videí bezporuchový.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/library/dn568099.aspx). V tématu opakování vzor, vzor Scheduler agenta nadřízeného.
- [Odolnost proti chybám v Azure SQL Database](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Příspěvek blogu podle Petrossian ADAM.
- [Rozhraní Entity Framework - odolnost připojení / logika opakovaných pokusů](https://msdn.microsoft.com/data/dn456835). Jak používat a přizpůsobit přechodná chyba zpracování funkce Entity Framework 6.
- [Odolnost připojení a zachycením příkaz s použitím Entity Framework v aplikaci ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Čtvrtý v kurzu řadu devět část ukazuje, jak nastavit funkci odolnost připojení EF 6 pro databázi SQL.

Videa

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Řada devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Přečtěte si diskuzi o moduly dělení na okruh v díl 3 počínaje 40:55.
- [Vytváření Big: Poučení vyplývající z Azure zákazníků – část II](https://channel9.msdn.com/Events/Build/2012/3-030). Moduly SIMM označit hovoří o návrhu pro selhání přechodný poruch zpracování a instrumentace vše.

Ukázka kódu

- [Základy služby ve službě Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Ukázková aplikace, které jsou vytvořené Azure poradní tým Microsoftu které ukazuje, jak používat [Enterprise knihovny přechodné chyby zpracování bloku](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Další informace najdete v tématu [cloudové služby základy Data Access Layer – přechodných chyb](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH se doporučuje pro přístup k databázi pomocí ADO.NET přímo (bez použití rozhraní Entity Framework).

> [!div class="step-by-step"]
> [Předchozí](monitoring-and-telemetry.md)
> [další](distributed-caching.md)
