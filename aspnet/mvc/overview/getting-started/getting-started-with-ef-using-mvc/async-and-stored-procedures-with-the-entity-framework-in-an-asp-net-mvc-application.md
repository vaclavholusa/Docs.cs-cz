---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async a uložené procedury s rozhraní Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 84cf427c7da7905444568ac34534e9ed98a7d8c8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873258"
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async a uložené procedury s rozhraní Entity Framework v aplikaci ASP.NET MVC
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V dřívějších kurzech jste zjistili, jak číst a aktualizovat data pomocí synchronní programovací model. V tomto kurzu uvidíte, jak implementovat asynchronní programovací model. Asynchronní kódu může pomoci aplikace poskytují lepší výkon, protože umožňuje lepší využití prostředků serveru.

V tomto kurzu dozvíte se taky, jak pomocí uložené procedury pro vložení, aktualizace a odstranění operace s entitou.

Nakonec budete znovu nasaďte aplikaci do Azure společně s všechny změny databáze, které jste implementovali od poprvé, které jste nasadili.

Následující ilustrace znázorňuje některé stránek, které budete pracovat.

![Stránka oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Vytvořit oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Proč zabývat asynchronní kódu

Webový server má omezený počet vláken, které jsou k dispozici, a v situacích, vysokého zatížení všech dostupných vláken může být používán. Pokud k tomu dojde, server nemůže zpracovat nové žádosti o, dokud se uvolní vláken. Synchronní kód může být vázáno mnoho vláken při jejich nejsou to skutečně veškerou práci, protože se čeká vstupně-výstupních operací na dokončení. Asynchronní kód když proces čeká na vstupně-výstupních operací na dokončení, je jeho přístup z více vláken uvolněno pro server, aby používal pro zpracování dalších požadavků. V důsledku toho asynchronní kódu umožňuje prostředky serveru více efektivně používat, a server je povolena a zvládli větší provoz bez zpoždění.

V dřívějších verzích rozhraní .NET, byl složitý, psaní a testování asynchronní kódu chyby náchylné k chybám a ladění složité. V rozhraní .NET 4.5 je mnohem jednodušší, pokud nemáte důvod nikoli k musí obecně napsat asynchronní kód psaní, testování a ladění kódu asynchronní. Asynchronní kód zavést malé množství režijní náklady na, ale pro situace nízkým provozem, stiskněte klávesu výkonu je nepatrné při pro intenzivní provoz situace, je výrazně potenciální zvýšení výkonu.

Další informace o asynchronní programování najdete v tématu [použití rozhraní .NET 4.5 asynchronní podpora k zabránění blokování volání](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Vytvoření řadiče oddělení

Vytvořit řadič oddělení vyberte stejně jako jste to udělali starších řadičů, ale s výjimkou **použití asynchronní kontroler** akce zaškrtávací políčko.

![Oddělení řadiče vygenerované uživatelské rozhraní](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Následující označuje zobrazit, co byla přidána do synchronní kód `Index` metoda aby asynchronní:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Chcete-li povolit dotaz databáze Entity Framework pro asynchronní byly použity čtyři změny:

- Metoda je označena `async` – klíčové slovo, které sděluje kompilátoru generování zpětných volání pro části těla metody a automaticky vytvářet `Task<ActionResult>` objekt, který je vrácen.
- Návratový typ byl změněn z `ActionResult` k `Task<ActionResult>`. `Task<T>` Typ reprezentuje probíhající práce s tím výsledkem typu `T`.
- `await` – Klíčové slovo, které bylo použito pro volání webové služby. Když kompilátor uvidí toto klíčové slovo, na pozadí se rozdělí metodu na dvě části. První část končí operace, které se spustí asynchronně. Druhá část je uvést do metody zpětného volání, která je volána po dokončení operace.
- Asynchronní verzi `ToList` byla volána metoda rozšíření.

Proč je `departments.ToList` příkaz změnil, ale ne `departments = db.Departments` příkaz? Důvodem je, že pouze příkazy, které způsobily dotazy nebo příkazy k odeslání do databáze se spustí asynchronně. `departments = db.Departments` Příkaz nastaví dotazu, ale dokud není spustit dotaz `ToList` metoda je volána. Proto pouze `ToList` metoda se spustí asynchronně.

V `Details` metoda a `HttpGet` `Edit` a `Delete` metody, `Find` metoda je ten, který způsobí, že dotaz k odeslání do databáze, tak, aby se metoda, která se provede asynchronně:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

V `Create`, `HttpPost Edit`, a `DeleteConfirmed` metody, je `SaveChanges` volání metody, které způsobí, že příkaz, který má být proveden, není příkazy, jako `db.Departments.Add(department)` který pouze způsobit entity v paměti, která má být změněn.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Otevřete *Views\Department\Index.cshtml*a kód šablony nahraďte následujícím kódem:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Tento kód změny názvu od indexu oddělení, přesune jméno správce napravo a poskytuje úplný název tohoto správce.

V části Vytvoření odstranit, podrobnosti a upravit zobrazení, změňte popisek pro `InstructorID` pole na "Správce" stejně jako jste změnili pole název oddělení "Oddělení" v zobrazení průběhu.

Vytvořit a upravit zobrazení použít následující kód:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

V zobrazení Podrobnosti a odstranění použijte následující kód:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Spusťte aplikaci a klikněte na **oddělení** kartě.

![Stránka oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Všechno funguje stejně jako ostatní řadiče, ale v tomto kontroleru všechny dotazy SQL jsou asynchronně.

Některé věci znát při použití asynchronní programování s platformou Entity Framework:

- Kód asynchronní není bezpečné pro přístup z více vláken. Jinými slovy jinými slovy, nejsou pokusí provést více operací paralelně pomocí ke stejné instanci kontextu.
- Pokud chcete využít výhod výkonu asynchronní kódu, ujistěte se, že všechny knihovny balíčky, které používáte (například konkrétní stránkování), také použít asynchronní, pokud volají žádné metody rozhraní Entity Framework, které způsobí dotazy k odeslání do databáze.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Pomocí uložených procedur pro vložení, aktualizaci a odstraňování

Někteří vývojáři a DBAs přednost pomocí uložených procedur pro přístup k databázi. V dřívějších verzích rozhraní Entity Framework můžete načíst data pomocí uložené procedury pomocí [provádění nezpracovaná dotazu SQL](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), ale nemůže pokyn EF používat uložené procedury pro operace aktualizace. V EF 6 se snadno konfiguruje Code First pomocí uložených procedur.

1. V *DAL\SchoolContext.cs*, přidejte zvýrazněný kód, který `OnModelCreating` metoda.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Tento kód dá pokyn Entity Framework, použijte uložené postupy pro vložení, aktualizace a odstranění operací na `Department` entity.
2. V konzole pro správu balíčku zadejte následující příkaz:

    `add-migration DepartmentSP`

    Otevřete *migrace\&lt; časové razítko&gt;\_DepartmentSP.cs* zobrazíte kód `Up` metodu, která vytvoří Insert, Update a Delete uložené procedury:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. V konzole pro správu balíčku zadejte následující příkaz:

     `update-database`
4. Spuštění aplikace v režimu ladění, klikněte **oddělení** a pak klikněte **vytvořit nový**.
5. Zadejte data pro nové oddělení a pak klikněte na tlačítko **vytvořit**.

     ![Vytvořit oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
6. V sadě Visual Studio, podívejte se na protokoly v **výstup** okna, zobrazí se, že uložené procedury použila vložit nový řádek oddělení.

     ![Vložení SP oddělení](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Kód nejprve vytvoří výchozí uložené procedury názvy. Pokud používáte existující databázi, může být nutné přizpůsobit názvy uložené procedury, chcete-li použít uložené procedury již definována v databázi. Informace o tom, jak to udělat najdete v tématu [Entity Framework Code první vložení, aktualizaci nebo odstranění uložených procedur](https://msdn.microsoft.com/data/dn468673).

Pokud chcete přizpůsobit, co generované uložené procedury proveďte, můžete upravit automaticky generovaný kód pro byla migrace `Up` metodu, která vytvoří uložené procedury. Tímto způsobem vaše změny se projeví kdykoli, migrace se spustí a použijí se na vaši produkční databázi když migrace spustí automaticky v produkčním prostředí po nasazení.

Pokud chcete změnit existující uložené procedury, která byla vytvořena v předchozí migrace, můžete použít příkaz Add-Migration generování prázdné migrace a pak ručně napsat kód, který volá [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) – metoda .

## <a name="deploy-to-azure"></a>Nasazení do Azure

V této části vyžaduje, abyste dokončili nepovinný **nasazení aplikace do Azure** tématu [nasazení a migrace](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurzu této série. Pokud jste měli chyby migrace, které můžete vyřešit odstraněním databázi ve vašem místním projektu, tuto část přeskočte.

1. V sadě Visual Studio, klikněte pravým tlačítkem na projekt v **Průzkumníku řešení** a vyberte **publikovat** v místní nabídce.
2. Klikněte na tlačítko **publikování**.

    Visual Studio nasadí aplikaci do Azure a aplikace otevře výchozí prohlížeč, běží v Azure.
3. Testování aplikace k ověření pracuje.

    Při prvním spuštění stránky, který přistupuje k databázi, rozhraní Entity Framework spustí všechny byla migrace `Up` metody, které jsou nezbytné k přivedení aktuální pomocí aktuálního modelu dat databáze. Teď můžete použít všechny webových stránek, které jste přidali od posledního, které jste nasadili, včetně oddělení stránek, které jste přidali v tomto kurzu.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak ke zlepšení efektivity serveru podle psaní kódu, který se spustí asynchronně a jak pomocí uložených procedur pro vložení, aktualizace a operace odstranění. V dalším kurzu se zobrazí, jak zabránit ztrátě dat při pokusu o několik uživatelů k úpravě stejné záznamu ve stejnou dobu.

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
