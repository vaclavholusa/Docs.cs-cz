---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async a uložené procedury s Entity Framework v aplikaci ASP.NET MVC | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 5 pomocí sady Visual Studio a Entity Framework 6 Code First...
ms.author: riande
ms.date: 11/07/2014
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84be966c1e1a4357125c1a53b8065676c8f073f6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910730"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async a uložené procedury s Entity Framework v aplikaci ASP.NET MVC
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

> Ukázková webová aplikace Contoso University ukazuje, jak vytvářet aplikace ASP.NET MVC 5 pomocí Entity Framework 6 kód první a Visual Studio. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V předchozích kurzech jste zjistili, jak číst a aktualizovat data pomocí synchronní programovací model. V tomto kurzu můžete zjistit, jak implementovat asynchronní programovací model. Asynchronní kód může pomoct lépe provést, protože umožňuje lepší využití serverových prostředků aplikace.

V tomto kurzu uvidíte také použití uložené procedury pro vložení, aktualizace nebo odstranění operace s entitou.

Nakonec budete znovu nasadit aplikaci do Azure, spolu s všechny změny databáze, které jsme implementovali od první chvíle, kdy jste nasadili.

Následující ilustrace znázorňují některé stránky, které budete pracovat.

![Oddělení stránky](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Vytvoření oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Proč zabývat asynchronní kód

Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán. Pokud k tomu dojde, server nemůže zpracovat nové žádosti, dokud se uvolnit vlákna. Přestože se nejedná skutečně každé dílo vzhledem k tomu, že čekání na vstupně-výstupních operací na dokončení, může kódem synchronní svázané několika vlákny. Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho vlákno uvolněn pro server určený pro zpracováním jiných požadavků. V důsledku toho asynchronního kódu umožňuje serveru prostředků efektivněji používat, a abyste zvládli větší provoz bez zpoždění je povoleno na serveru.

V dřívějších verzích rozhraní .NET byl složitý, psaní a testování asynchronní kód chyby náchylné k chybám a těžko ladění. V rozhraní .NET 4.5 je mnohem jednodušší, pokud nemáte důvod, proč nebylo by měly obecně psaní asynchronního kódu psaní, testování a ladění asynchronního kódu. Asynchronní kód zavést malé množství režie, ale v situacích s nízkým provozem výkonu přístupů je zanedbatelný, při vysoké návštěvnosti situacích, je možné zlepšení výkonu podstatné.

Další informace o asynchronním programování naleznete v tématu [podpory asynchronních operací pomocí rozhraní .NET 4.5 k zabránění blokování volání](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Vytvoří kontroler oddělení

Vytvoření kontroleru oddělení, stejně jako jste to udělali starších řadičů, tentokrát vyberte **použití asynchronní kontroler** akce zaškrtávací políčko.

![Oddělení řadič vygenerované uživatelské rozhraní](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Následujícím hlavním funkcím zobrazit, co byl přidán do synchronní kód `Index` provést asynchronní metody:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Čtyři změny byly použity k povolení databázového dotazu Entity Framework spustit asynchronně:

- Metoda je označena třídou `async` – klíčové slovo, které instruuje kompilátor generovat zpětná volání pro části tělo metody a automaticky vytvářet `Task<ActionResult>` vráceného objektu.
- Návratový typ se změnil z `ActionResult` k `Task<ActionResult>`. `Task<T>` Typ představuje probíhající práci s výsledkem typu `T`.
- `await` – Klíčové slovo byla použita k volání webové služby. Když kompilátor narazí toto klíčové slovo, na pozadí rozdělí metodu na dva oddíly. První část končí operace, která se spustí asynchronně. Druhá část je nepoužili metodu zpětného volání, která je volána po dokončení operace.
- Asynchronní verze `ToList` byla volána metoda rozšíření.

Proč je `departments.ToList` příkaz upravit, ale ne `departments = db.Departments` příkaz? Důvodem je, že pouze příkazy, které způsobují dotazy nebo příkazy, které se odesílají do databáze se provedl asynchronně. `departments = db.Departments` Příkaz nastaví dotazu, ale není dotaz provést, dokud `ToList` metoda je volána. Proto pouze `ToList` metoda provádí asynchronně.

V `Details` metoda a `HttpGet` `Edit` a `Delete` metody, `Find` metoda je ten, který způsobí, že dotaz k odeslání do databáze, tak, aby se metoda, která se provede asynchronně:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

V `Create`, `HttpPost Edit`, a `DeleteConfirmed` metody, je `SaveChanges` volání metody, která způsobí, že příkaz má být proveden, nikoli příkazy, jako `db.Departments.Add(department)` entity což vedlo jenom v paměti, která má být upraven.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Otevřít *Views\Department\Index.cshtml*a nahraďte kód šablony následujícím kódem:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Tento kód změní název z indexu na oddělení, přesune jméno správce napravo a poskytuje úplné jméno správce.

V části Vytvoření, odstranění, podrobností a upravte zobrazení, změnit titulek `InstructorID` pole "Administrator" stejným způsobem jako v zobrazeních kurzu změníte pole název oddělení "Oddělení".

Zobrazení v vytvořit a upravit pomocí následujícího kódu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

V zobrazení Delete a podrobnosti pomocí následujícího kódu:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Spusťte aplikaci a klikněte na tlačítko **oddělení** kartu.

![Oddělení stránky](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Všechno, co funguje stejně jako v ostatních řadičů, ale v tomto kontroleru všechny dotazy SQL jsou asynchronně.

Je potřeba vědět při použití asynchronní programování s rozhraním Entity Framework pár věcí:

- Asynchronní kód není bezpečné pro vlákna. Jinými slovy jinými slovy, nedoporučujeme provádět více operací paralelně pomocí stejné instance kontextu.
- Pokud chcete využít výhod výkony těží z asynchronní kód, ujistěte se, že všechny knihovny balíčky, které používáte (například stránkování), asynchronní použijte i v případě volají všechny Entity Framework metody, které způsobují dotazů k odeslání do databáze.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Pomocí uložené procedury pro vkládání, aktualizaci a odstraňování

Některé vývojáře a specializující raději pomocí uložených procedur pro přístup k databázi. V dřívějších verzích sady Entity Framework můžete načíst data pomocí uložené procedury pomocí [spuštění neupraveného dotazu SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nemůže dát pokyn EF pouze pomocí uložených procedur pro operace update. V EF 6 je snadno konfigurovatelné Code First pomocí uložených procedur.

1. V *DAL\SchoolContext.cs*, přidejte zvýrazněný kód, který `OnModelCreating` metody.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Tento kód nastaví Entity Frameworku na použití uložené procedury pro vložení, aktualizovat a odstraňovat operace `Department` entity.
2. V konzole pro správu balíčků zadejte následující příkaz:

    `add-migration DepartmentSP`

    Otevřít *migrace\&lt; časové razítko&gt;\_DepartmentSP.cs* zobrazíte kód `Up` metodu, která vytvoří Insert, Update a Delete uložené procedury:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. V konzole pro správu balíčků zadejte následující příkaz:

     `update-database`
4. Spusťte aplikaci v režimu ladění, klikněte na tlačítko **oddělení** kartu a potom klikněte na tlačítko **vytvořit nový**.
5. Zadejte data pro jiného oddělení a klikněte na **vytvořit**.

     ![Vytvoření oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. V sadě Visual Studio, podívejte se na protokoly v **výstup** okno a zobrazit, uložené procedury byla použita k vložit nový řádek oddělení.

     ![Oddělení vložit SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kód nejprve vytvoří výchozí uložené procedury názvy. Pokud používáte existující databázi, můžete potřebovat k přizpůsobení názvy uložených postupů Chcete-li použít uložené procedury v databázi již definována. Informace o tom, jak to udělat, najdete v části [Entity Framework Code první Insert/Update/Delete uložené procedury](https://msdn.microsoft.com/data/dn468673).

Pokud chcete přizpůsobit, co vygeneruje uložených procedur, můžete upravit automaticky generovaný kód pro přenesení `Up` metodu, která vytvoří uloženou proceduru. Díky tomu vaše změny se projeví vždy, když, migrace je spuštěný a uplatní se na vaši produkční databázi když migrace spustí automaticky v produkčním prostředí po nasazení.

Pokud chcete změnit stávající úložnou proceduru, který byl vytvořen v předchozí migrace, můžete pomocí příkazu Add-migrace generovat prázdné migrace a ručně napsat kód, který volá [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) – metoda .

## <a name="deploy-to-azure"></a>Nasazení do Azure

Tato část vyžaduje, abyste dokončili nepovinný **nasazení aplikace do Azure** tématu [Migration a nasazení](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) této série kurzů. Pokud jste měli migrace chyby, které jste vyřešili tak, že odstraníte databáze v projektu pro místní, tuto část přeskočte.

1. Ve Visual Studiu klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **publikovat** v místní nabídce.
2. Klikněte na tlačítko **publikovat**.

    Visual Studio nasadí aplikaci do Azure a aplikace se otevře ve výchozím prohlížeči, běží v Azure.
3. Otestování aplikace ověří, že funguje.

    Při prvním spuštění stránky, který přistupuje k databázi, Entity Framework provozovat všechny migrace `Up` metod požadovaných k aktuální s aktuálním datovým modelem databáze. Teď můžete použít všechny webové stránky, které jste přidali od posledního, které jste nasadili, včetně oddělení stránek, které jste přidali v tomto kurzu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak zvýšit efektivitu server napsáním kódu, který se spustí asynchronně a jak pomocí uložené procedury pro vložení, aktualizace a odstranění operace. V dalším kurzu uvidíte, jak zabránit ztrátě dat při pokusu upravit stejný záznam ve stejnou dobu více uživatelů.

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET – doporučené zdroje informací](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
