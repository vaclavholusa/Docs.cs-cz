---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: "Tím se maximalizuje výkon s rozhraní Entity Framework 4.0 4 webové aplikace ASP.NET | Microsoft Docs"
author: tdykstra
description: "Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 40a53a110115e5f6342d2a97d21b64470450fd3c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Tím se maximalizuje výkon s rozhraní Entity Framework 4.0 4 webové aplikace technologie ASP.NET
====================
podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady. Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).


V tomto kurzu předchozí jste viděli způsobu řešení konfliktů souběžnosti. Tento kurz ukazuje možnosti pro zvýšení výkonu webové aplikace ASP.NET, který používá rozhraní Entity Framework. Dozvíte několik metod pro maximalizaci výkonu nebo pro diagnostikování problémů s výkonem.

Informace uvedené v následujících částech je mohly být užitečné v mnoha rozličných scénářů:

- Efektivní načtěte související data.
- Správa stavu zobrazení.

Informace uvedené v následujících částech může být užitečné, pokud máte jednotlivé dotazy, že existuje problémy s výkonem:

- Použití `NoTracking` merge – možnost.
- Předem zkompilujte dotazů LINQ.
- Zkontrolujte příkazů dotazů odeslal do databáze.

Informace uvedené v následující části je potenciálně užitečné pro aplikace, které mají velmi velké datové modely:

- Předběžnému generování zobrazení.

> [!NOTE]
> Výkon webové aplikace je ovlivňován mnoha faktory, včetně například velikost požadavku a odpovědi dat, rychlosti dotazů databáze, kolik požadavky, že můžete server fronty a jak rychle může obsluhovat a to i účinnost žádné knihovny klientských skriptů, které používáte. Pokud výkon, je důležité v aplikaci, nebo pokud testování nebo role experience ukazuje, že není dostatečný výkon aplikace, postupujte podle normální protokol pro optimalizaci výkonu. Měření k určení, kde dochází k kritické body a pak Adresujte oblastí, kterým budou mít největší vliv na výkon aplikací.
> 
> Toto téma se zaměřuje především na způsobům, ve kterém můžete potenciálně zlepšit výkon konkrétně rozhraní Entity Framework technologie ASP.NET. Návrhy tady jsou užitečné, pokud zjistíte, že přístup k datům je jednou z kritická místa výkonu ve vaší aplikaci. S výjimkou toho, jak je uvedeno, metody popsané v tomto poli by se neměla považovat &quot;osvědčené postupy&quot; obecně – řada z nich jsou vhodné pouze ve výjimečných situacích nebo na adresu velmi konkrétní typy kritické body.


Pokud chcete spustit tento kurz, spuštění sady Visual Studio a otevřete Contoso univerzity webovou aplikaci, kterou jste pracovali v předchozí kurzu.

## <a name="efficiently-loading-related-data"></a>Efektivní načítání související Data

Aby rozhraní Entity Framework můžete načíst související data do navigační vlastnosti entity několika způsoby:

- *Opožděného načítání*. Když je nejdřív přečíst entity, není načíst související data. Ale při prvním pokusu o přístup k navigační vlastnost, lze data potřebná pro tuto navigační vlastnost je automaticky načte. To vede k více dotazů odeslal do databáze – jednu pro samotné entity a jeden musí načíst pokaždé, když související data pro entitu. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Přes načítání*. Při čtení je entita, související data načtena společně s jeho. To obvykle vede jednoho připojení dotaz, který načte všechna data, která je potřeba. Zadejte přes načítání pomocí `Include` metoda, jak jste jste viděli již v těchto kurzech.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Explicitní načítání*. Toto je podobná opožděného načítání, s tím rozdílem, že je explicitně načíst související data v kódu; není provedena automaticky při přístupu k navigační vlastnosti. Načíst související data ručně pomocí `Load` metoda navigační vlastnost pro kolekce, nebo můžete použít `Load` metoda vlastnost referenčního pro vlastnosti, které mají jednoho objektu. (Například volání `PersonReference.Load` metoda načíst `Person` vlastnost navigace `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Vzhledem k tomu, že nemáte načíst okamžitě hodnoty vlastností, opožděného načítání a explicitní načítání jsou obě známou taky jako *odložené načítání*.

Opožděného načítání je výchozí chování v kontextu objektu, která byla vygenerována v designeru. Pokud otevřete *SchoolModel.Designer.cs* soubor, který definuje třídu objektu kontextu, zjistíte tři metody konstruktoru a každý z nich obsahuje následující příkaz:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Obecně platí Pokud víte, je třeba souvisejících dat pro každou entitu, které načtené, přes načítání nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každé entity načíst jediný dotaz odeslal do databáze. Na druhé straně, pokud budete potřebovat pro přístup k navigační vlastnosti entity jen zřídka, nebo jenom pro malá může být sadu entit, opožděného načítání nebo explicitní načítání efektivnější, protože přes načítání by získat víc dat, než budete potřebovat.

Ve webové aplikaci opožděného načítání může být poměrně málo hodnota přesto provést, protože provést akce uživatele, které ovlivňují potřebu souvisejících dat v prohlížeči, který nemá připojení k kontext objektu, který vykreslí stránku. Na druhé straně když jste databind ovládacího prvku, obvykle víte, jaká data je třeba a proto je obecně osvědčené zvolit přes načítání nebo odložené načítání na základě co je vhodná v každém scénáři.

Kromě toho může u ovládacího prvku pomocí objektu entity, po uvolnění objektu kontextu, má. V takovém případě dojde k selhání pokusu o opožděné zatížení navigační vlastnost. Je zřejmé, které se zobrazí chybová zpráva:&quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Řízení zakáže opožděného načítání ve výchozím nastavení. Pro `ObjectDataSource` řízení, které používáte pro aktuální kurz (nebo pokud máte přístup k objektu kontextu z kódu stránky), existuje několik způsobů, které můžete provést opožděného načítání ve výchozím nastavení zakázaná. Ji můžete vypnout, pokud doložit kontextu objektu. Například přidejte následující řádek na konstruktor metodu `SchoolRepository` třídy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Pro aplikace společnosti Contoso univerzity provedete kontext objektu automaticky zakázat opožděného načítání tak, aby tato vlastnost nemá být nastavit vždy, když je vytvořena instance kontextu.

Otevřete *SchoolModel.edmx* dat model, klikněte na návrhovou plochu a potom v podokně vlastností nastavte **povoleno opožděného načítání** vlastnost `False`. Uložte a zavřete datový model.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Správa stavu zobrazení

Chcete-li provést aktualizaci funkce, musí webová stránka ASP.NET ukládat původní hodnoty vlastností entity při vykreslení stránky. Během zpracování ovládacího prvku zpětné volání můžete znovu vytvořit původního stavu entity a volání entity `Attach` metoda než při použití změn a volání `SaveChanges` metoda. Ve výchozím nastavení použijte ovládací prvky webových formulářů ASP.NET datových zobrazení stavu k uložení původní hodnoty. Zobrazení stavu však může ovlivnit výkon, protože je uložený v skrytá pole, které podstatně zvýšit velikost stránky, která se odešle do a z prohlížeče.

Techniky pro správu zobrazení stavu nebo alternativy, jako je například stav relace nejsou jedinečné pro rozhraní Entity Framework, tak v tomto kurzu není přejděte do tohoto tématu podrobně. Další informace najdete v článku odkazy na konci tohoto kurzu.

Však verze 4 technologie ASP.NET poskytuje nový způsob práce se stav zobrazení, kterou by si měl vědět každý vývojář ASP.NET aplikací webových formulářů: `ViewStateMode` vlastnost. Tato vlastnost můžete nastavit na úrovni stránka nebo ovládací prvek, a umožňuje zakázat stav zobrazení ve výchozím nastavení pro stránku a povolit pouze pro ovládací prvky, které je třeba ji.

Pro aplikace, kde je důležité výkonu je vhodné vždy zakázat stav zobrazení na úrovni stránky a povolit pouze pro ovládací prvky, které je vyžadují. Velikost stav zobrazení na stránkách společnosti Contoso univerzity by být podstatně snížena touto metodou, ale pokud chcete zjistit, jak to funguje, uděláte ho *Instructors.aspx* stránky. Tato stránka obsahuje mnoho ovládacích prvků, včetně `Label` ovládací prvek, který obsahuje zobrazení stavu zakázáno. Žádná z ovládacích prvků na této stránce ve skutečnosti musí být povolen stav zobrazení. ( `DataKeyNames` Vlastnost `GridView` řízení Určuje stav, který musí být zachovaná mezi postback, ale tyto hodnoty jsou uchovány v řízení stavu, který nemá vliv `ViewStateMode` vlastnost.)

`Page` – Direktiva a `Label` značky ovládacího prvku aktuálně podobá následujícímu příkladu:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Proveďte následující změny:

- Přidat `ViewStateMode="Disabled"` k `Page` – direktiva.
- Odebrat `ViewStateMode="Disabled"` z `Label` ovládacího prvku.

Kód nyní podobá následujícímu příkladu:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Stav zobrazení je nyní zakázán pro všechny ovládací prvky. Pokud přidáte později ovládacího prvku, který potřebuje používat stav zobrazení, je potřeba udělat je zahrnout `ViewStateMode="Enabled"` atribut pro ovládací prvek.

## <a name="using-the-notracking-merge-option"></a>Pomocí možnosti sloučení NoTracking

Když kontextu objektu načte řádky databáze, vytvoří objekty entity, které představují je ve výchozím nastavení je také sleduje tyto objekty entity pomocí jeho správce stavu objektu. Tato data sledování funguje jako mezipaměť a používá se při aktualizaci entity. Protože webová aplikace má obvykle instance kontextu krátkodobou objektů, dotazy často vracející data, která nemusí být sledovat, protože kontext objektu, který čte je uvolní před žádné entity, které je používají znovu nebo aktualizovat.

Rozhraní Entity Framework, můžete zadat, zda kontext objektu sleduje objekty entity nastavením *merge – možnost*. Můžete nastavit možnost sloučení pro jednotlivé dotazy nebo sady entit. Pokud jste nastavili pro sadu entit, to znamená, že nastavujete výchozí možnost sloučení pro všechny dotazy, které jsou vytvořené pro tuto sadu entit.

Pro aplikace společnosti Contoso univerzity sledování není vyžadován pro všechny sad entit, které přístup z úložiště, abyste mohli nastavit možnosti sloučení `NoTracking` pro tyto sady entit, když doložit kontext objektu v třídě úložiště. (Všimněte si, že v tomto kurzu možnost sloučení nebude mít nastavení znatelný vliv na výkon aplikace. `NoTracking` Nejspíš aby zlepšení výkonu pozorovatelné pouze v některých scénářích vysokoobjemový data.)

Ve složce DAL, otevřete *SchoolRepository.cs* souboru a přidejte konstruktor metodu, která nastaví možnost sloučení pro entity nastaví, zda má přístup k úložišti:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Předem kompilace dotazů LINQ

Při prvním rozhraní Entity Framework provede dotaz Entity SQL v rámci dobu životnosti danou `ObjectContext` instance, bude trvat delší dobu kompilace dotazu. Výsledek kompilace je uložené v mezipaměti, což znamená, že následné spuštění dotazu jsou mnohem rychlejší. Dotazy LINQ podle podobný vzor, s tím rozdílem, že některé práce potřebné ke kompilaci dotazu se provádí pokaždé, když je proveden dotaz. Jinými slovy pro dotazy LINQ ve výchozím nastavení všechny výsledky kompilace se ukládá do mezipaměti.

Pokud máte dotaz LINQ, který chcete spustit opakovaně existence kontextu objektu, můžete napsat kód, který způsobí, že všechny výsledky kompilace ukládat do mezipaměti při prvním spuštění dotazu LINQ.

Jako ilustraci, Uděláte to pro dvě `Get` metody v `SchoolRepository` třída, z nichž jeden neberou žádné parametry ( `GetInstructorNames` metoda) a ten, který nevyžaduje parametr ( `GetDepartmentsByAdministrator` metoda). Tyto metody jako platném nyní ve skutečnosti není nutné kompilovat, protože nejsou dotazů LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Však tak, aby můžete vyzkoušet kompilované dotazy, můžete budete pokračovat, jako by to měla byla zapsána jako následující dotazy LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Co je uvedené výše a spusťte aplikaci ověřte, že funguje před pokračováním může změnit kód v těchto metod. Ale podle následujících pokynů pustíme přímo do vytváření předem kompilovaném jejich verze.

Vytvořit soubor třídy v *DAL* složky, pojmenujte ji *SchoolEntities.cs*a existujícího kódu nahraďte následujícím kódem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Tento kód vytvoří částečné třídu, která rozšiřuje třídu automaticky generovaný kontext. Třídu zahrnuje dvě kompilované dotazů LINQ pomocí `Compile` metodu `CompiledQuery` třídy. Vytvoří také metody, které můžete použít k volání dotazy. Uložte a zavřete tento soubor.

Vedle *SchoolRepository.cs*, změňte existující `GetInstructorNames` a `GetDepartmentsByAdministrator` metody v úložišti třídy tak, aby volání kompilované dotazy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Spustit *Departments.aspx* a ověřte, že bude fungovat jako předtím. `GetInstructorNames` Metoda je volána k naplnění rozevíracího seznamu správce a `GetDepartmentsByAdministrator` metoda je volána, když kliknete na tlačítko **aktualizace** abyste ověřili, že žádné lektorem správce více než jednoho oddělení.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

V aplikaci Contoso univerzity pouze chcete zjistit, jak to udělat, protože by zlepšení měřitelný výkon výkonu jste předem kompilovaném dotazy. Předem kompilování dotazů LINQ přidat úroveň složitost kódu, proto se ujistěte, že můžete udělat jenom pro dotazy, které ve skutečnosti představují kritické body ve vaší aplikaci.

## <a name="examining-queries-sent-to-the-database"></a>Zkoumání dotazy odeslal do databáze

Pokud jste příčin problémů s výkonem, někdy je užitečné vědět přesný příkazy SQL, které rozhraní Entity Framework odesílá do databáze. Pokud pracujete s `IQueryable` objekt, je použít jeden způsob, jak to udělat `ToTraceString` metoda.

V *SchoolRepository.cs*, změňte kód v `GetDepartmentsByName` metoda tak, aby odpovídaly v následujícím příkladu:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Proměnná musí být převedena `ObjectQuery` typu, protože `Where` metoda na konci předchozího řádku vytvoří `IQueryable` objektu; bez `Where` metody přetypování nebude nutné.

Nastavit zarážky `return` řádek a spusťte *Departments.aspx* stránky v ladicím programu. Pokud jste průchodu zarážkou, podívejte se `commandText` proměnné v **místní hodnoty –** okno a použijte vizualizér text (lupy v **hodnotu** sloupec) k zobrazení jeho hodnoty v **Text vizualizér** okno. Zobrazí celý příkaz SQL, která je výsledkem tento kód:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Jako alternativu funkci IntelliTrace v nástroji Visual Studio Ultimate poskytuje způsob, jak zobrazit příkazy SQL, které jsou generované rozhraní Entity Framework, která nevyžaduje měnit kód nebo dokonce nastavit zarážky.

> [!NOTE]
> Následující postupy můžete provést pouze v případě, že máte Visual Studio Ultimate.


Obnovit původní kód `GetDepartmentsByName` metoda a potom spusťte *Departments.aspx* stránky v ladicím programu.

V sadě Visual Studio, vyberte **ladění** nabídky, pak **IntelliTrace**a potom **události IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

V **IntelliTrace** okně klikněte na tlačítko **přerušení všech**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** okně se zobrazí seznam posledních událostí:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klikněte **ADO.NET** řádku. Rozšiřuje tak, aby zobrazovalo text příkazu:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Celý příkaz textový řetězec můžete zkopírovat do schránky z **místní hodnoty –** okno.

Předpokládejme, že jste pracovali s databází s více tabulek, vztahy a sloupců než jednoduché `School` databáze. Je možné, že dotaz, který shromažďuje všechny informace, je nutné v jediném `Select` příkazu, který obsahuje více `Join` klauzule stane příliš složité pro efektivní práci. V takovém případě můžete přejít z eager načítání explicitní načítání Zjednodušte dotaz.

Zkuste například změna kódu v `GetDepartmentsByName` metoda v *SchoolRepository.cs*. Aktuálně v tom, že metoda máte dotaz objekt, který má `Include` metody `Person` a `Courses` navigační vlastnosti. Nahraďte `return` příkaz s kódem, který provádí explicitní načítání, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Spustit *Departments.aspx* stránky v ladicím programu a kontrola **IntelliTrace** okno znovu jako předtím. Nyní kde byl jediný dotaz před, se zobrazí dlouho pořadí z nich.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klikněte na první **ADO.NET** řádku zobrazíte, co se stalo komplexní dotaz můžete prohlížet dříve.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Dotaz z oddělení se stal jednoduchou `Select` dotaz bez `Join` klauzule, ale je následován samostatné dotazy, které načíst související kurzy a správce, pomocí sady dva dotazy pro každé oddělení vrácený původní dotaz.

> [!NOTE]
> Pokud necháte opožděného načítání povoleno, vzor, který zde, zobrazí se stejný dotaz opakuje tolikrát, kolikrát, může být výsledkem opožděného načítání. Vzor, který obvykle budete chtít vyhnout se související data opožděného načítání pro každý řádek v primární tabulce. Pokud ověříte, že jeden spojení dotazu je příliš složitý účinný, by obvykle možné zlepšit výkon v takových případech změnou primární dotaz, který bude použit přes načítání.


## <a name="pre-generating-views"></a>Předběžnému generování zobrazení

Když `ObjectContext` nejprve vytvořit objekt v nové doméně aplikace, rozhraní Entity Framework vytvoří sadu tříd, které používá pro přístup k databázi. Tyto třídy, se nazývají *zobrazení*, a pokud máte velké datový model, generování tato zobrazení můžou zdržet webu odpověď na první požadavek pro stránku po inicializaci novou doménu aplikace. Vytvořením zobrazení v době kompilace spíše než v době běhu může snížit Tato prodleva prvního požadavku.

> [!NOTE]
> Pokud aplikace nemá velmi velký datový model, nebo pokud ji máte model velkého množství dat, ale nejste zajímá problémy s výkonem, který má vliv pouze úplně první požadavek na stránku po recyklaci služby IIS, můžete tuto část přeskočit. Vytvoření nedojde pokaždé, když instanci můžete vytvořit zobrazení `ObjectContext` objekt, protože zobrazení jsou uložené v mezipaměti v doméně aplikace. Proto pokud se často recyklace aplikace ve službě IIS, velmi málo požadavků stránky by těžit z předem vygenerovaných zobrazení.


Můžete předběžnému generování zobrazení pomocí *EdmGen.exe* nástroj příkazového řádku nebo pomocí *Text šablony transformace Toolkit* šablony (T4). V tomto kurzu budete používat šablony T4.

V *DAL* složky, přidejte do souboru pomocí **textové šablony** šablony (je pod **Obecné** uzel v **nainstalovaných šablonách** seznamu), a pojmenujte ji *SchoolModel.Views.tt*. Existující kód v souboru nahraďte následujícím kódem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Vygeneruje zobrazení pro tento kód *.edmx* souboru, který je umístěný ve stejné složce jako šablona a který má stejný název jako soubor šablony. Například pokud je název souboru šablony *SchoolModel.Views.tt*, bude hledat soubor datového modelu s názvem *SchoolModel.edmx*.

Uložte soubor a potom klikněte pravým tlačítkem na soubor v **Průzkumníku řešení** a vyberte **spustit nástroj pro vlastní**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio vytvoří soubor kód, který vytvoří zobrazení, která se nazývá *SchoolModel.Views.cs* založený na šabloně. (Možná jste si všimli, kód soubor je vytvořen i před výběrem **spustit nástroj pro vlastní**, jakmile bude uložen soubor šablony.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Teď můžete aplikaci spustit a ověřte, že funguje jako předtím.

Další informace o předem vygenerovaných zobrazení najdete v následujících zdrojích informací:

- [Postupy: předběžnému generování zobrazení za účelem zlepšení výkonu dotazů](https://msdn.microsoft.com/library/bb896240.aspx) na webu MSDN. Vysvětluje, jak používat `EdmGen.exe` nástroj příkazového řádku k předběžnému generování zobrazení.
- [Izolace výkonu předkompilované nebo vedlejší-generated zobrazení v rozhraní Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) na Windows Server AppFabric poradní tým blogu.

Tím dokončíte Úvod ke zlepšení výkonu ve webové aplikaci ASP.NET, který používá rozhraní Entity Framework. Další informace naleznete v následujících materiálech:

- [Faktory ovlivňující výkon (rozhraní Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) na webu MSDN.
- [Související s výkonem příspěvcích na blogu týmu Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [EF sloučení možnosti a kompilované dotazy](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Příspěvek blogu, který vysvětluje neočekávané chování kompilované dotazy a merge možnosti, jako `NoTracking`. Pokud budete chtít použít kompilované dotazy nebo upravit nastavení možnosti sloučení v aplikaci, přečtěte si nejprve tuto.
- [Entity Framework související odešle blog in the Data a modelování poradní tým](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Zahrnuje příspěvcích na kompilované dotazy a pomocí Visual Studio 2010 profileru ke zjišťování problémů s výkonem.
- [Entity Framework fórum vlákno s Rady, jak ke zlepšení výkonu vysoce složitých dotazů](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Doporučení pro správu stavu ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Použitím rozhraní Entity Framework a ObjectDataSource: vlastní stránkování](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Příspěvek blogu, který je založený na ContosoUniversity aplikace vytvořené v těchto kurzech vysvětlují, jak implementovat stránkování v *Departments.aspx* stránky.

V dalším kurzu zkontroluje některé důležité vylepšení na rozhraní Entity Framework, které nově jsou v verze 4.

>[!div class="step-by-step"]
[Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
[další](what-s-new-in-the-entity-framework-4.md)
