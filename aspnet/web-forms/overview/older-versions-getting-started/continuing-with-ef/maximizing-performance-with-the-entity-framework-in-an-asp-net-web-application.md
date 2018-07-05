---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
title: Dosažení maximálního výkonu se sadou Entity Framework 4.0 v ASP.NET 4 webové aplikace | Dokumentace Microsoftu
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 4e43455e-dfa1-42db-83cb-c987703f04b5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: a83c225d563f54885941474998ccb6186e4a0b4f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388555"
---
<a name="maximizing-performance-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Dosažení maximálního výkonu se sadou Entity Framework 4.0 v ASP.NET 4 webové aplikace
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů. Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


V předchozím kurzu jste viděli, jak zpracování konfliktů souběžnosti. Tento kurz ukazuje možnosti pro zlepšení výkonu webové aplikace ASP.NET, která používá Entity Framework. Dozvíte několik metod pro dosažení maximálního výkonu, nebo pro diagnostiku problémů s výkonem.

Informace uvedené v následujících částech by mohla být užitečná v širokou řadu scénářů:

- Efektivní načtěte související data.
- Správa stavu zobrazení.

Informace uvedené v následující části mohou být užitečné, pokud máte jednotlivé dotazy této k dispozici problémy s výkonem:

- Použití `NoTracking` merge – možnost.
- Před kompilací dotazů LINQ.
- Prozkoumejte query příkazy, odeslané do databáze.

Informace uvedené v následující části je potenciálně užitečné pro aplikace, které mají velmi velkých datových modelů:

- Předem vygenerujte zobrazení.

> [!NOTE]
> Je celá řada faktorů, včetně řadí velikosti dat požadavků a odpovědí, rychlost databázových dotazů, kolik požadavků, že server lze zařadit do fronty a jak rychle můžete služby a dokonce efektivitu žádné vliv na výkon webové aplikace skript klienta knihovny, které je možné, že používáte. Pokud výkon je velmi důležité ve vaší aplikaci nebo testování nebo role experience systému ukazuje, že není vyhovující výkon aplikace, měli byste postupovat podle normální protokol pro optimalizaci výkonu. Měření k určení, kde dochází k kritické body výkonu a pak Adresujte oblastí, které budou mít největší vliv na výkon aplikací.
> 
> Toto téma se zaměřuje především na způsoby, ve kterém může potenciálně zvýšit výkon konkrétně Entity Framework v technologii ASP.NET. Návrhy Zde jsou užitečné, pokud zjistíte, že přístup k datům je jedním z kritické body výkonu ve vaší aplikaci. S výjimkou toho, jak je uvedeno, je zde vysvětleno metody by neměly být zahrnuté &quot;osvědčené postupy&quot; obecně – řada z nich jsou vhodné pouze ve výjimečných případech nebo na adresu velmi konkrétní typy kritické body výkonu.


Zahájit kurz, spusťte sadu Visual Studio a otevřete webové aplikace Contoso University, kterým jste pracovali v předchozím kurzu.

## <a name="efficiently-loading-related-data"></a>Efektivní načítání souvisejících dat

Existuje několik způsobů, Entity Framework mohou načíst související data do navigační vlastnosti entity:

- *Opožděné načtení*. Pokud entita je nejdřív přečíst, související data nebude načten. Ale při prvním pokusu o přístup k vlastnosti navigace data požadovaná pro tuto navigační vlastnost je automaticky načte. Výsledkem je více dotazy odeslané do databáze – jeden pro samotné entity a jeden pokaždé, když související data entity musí být načten. 

    [![Image05](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

*Předběžné načítání*. Při čtení entity související data načíst společně. Obvykle v důsledku jednoho spojení dotaz, který zkopíruje všechna data, který je nezbytný. Předběžné načítání určíte pomocí `Include` metodu, jak jste se seznámili s již v těchto kurzech.

[![Image07](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

- *Explicitní načtení*. To se podobá opožděné načtení, s tím rozdílem, že explicitně načíst související data v kódu. je tomu tak není automaticky při přístupu k vlastnosti navigace. Načíst související data ručně pomocí `Load` metoda navigační vlastnost pro kolekce, nebo můžete použít `Load` metoda referenční vlastnosti pro vlastnosti, které obsahují jeden objekt. (Například volání `PersonReference.Load` metodu pro načtení `Person` navigační vlastnost `Department` entity.)

    [![Image06](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Protože jejich není okamžitě hodnoty vlastností, opožděné načtení a explicitní načtení se obě označují jako *odložené načítání*.

Opožděné načtení je výchozí chování pro generovaný návrhářem kontextu objektu. Pokud otevřete *SchoolModel.Designer.cs* soubor, který definuje třídu objektů kontextu, najdete tu tři metody konstruktoru a každý z nich obsahuje následující příkaz:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Obecně platí Pokud víte, musíte souvisejících dat pro každou entitu, které načtené, nemůžou dočkat, až načítání nabízí nejlepší výkon, protože je obvykle efektivnější než samostatné dotazy pro každou entitu načíst jednoho dotazu odeslaného do databáze. Na druhou stranu, pokud potřebujete jenom zřídka přístup k navigační vlastnosti entity, nebo jenom pro malé sadu entit, opožděné načtení nebo explicitní načtení může být mnohem efektivnější, protože nemůžou dočkat, až načítání by byl načten více dat, než potřebujete.

Ve webové aplikaci opožděné načtení může být poměrně málo hodnota i přesto, protože provedou uživatelské akce, které ovlivňují potřebu souvisejících dat v prohlížeči, která nemá připojení ke kontextu objektu, který vykreslí stránku. Na druhé straně když jste databind ovládací prvek, obvykle víte, jaká data potřebujete a proto je obvykle osvědčené zvolit předběžné načítání nebo odložené načítání na základě co je vhodné pro jednotlivé scénáře.

Kromě toho ovládací prvek databound použít objekt entity po vyřazení objektu kontextu. V takovém případě se pokus o opožděné načtení navigační vlastnost selže. Je jasné, které se zobrazí chybová zpráva: &quot;`The ObjectContext instance has been disposed and can no longer be used for operations that require a connection.`&quot;

`EntityDataSource` Ovládacího prvku ve výchozím nastavení zakáže opožděné načtení. Pro `ObjectDataSource` ovládací prvek, že používáte aktuální kurzu (nebo Pokud přistupujete kontextu objektu z kódu stránky), existuje několik způsobů, jak můžete provést opožděné načtení ve výchozím nastavení zakázané. Můžete jej zakázat při vytváření instance kontextu objektu. Například přidejte následující řádek na konstruktor metodu `SchoolRepository` třídy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Aplikace Contoso University provede automaticky vypnout opožděné načtení, tak, aby tato vlastnost nebude muset nastavit vždy, když je vytvořena instance kontextu kontextu objektu.

Otevřít *SchoolModel.edmx* dat model, klikněte na návrhové ploše a potom v podokně vlastností nastavte **opožděné načtení povoleno** vlastnost `False`. Uložte a zavřete datový model.

[![Image04](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

## <a name="managing-view-state"></a>Správa stavu zobrazení

Aby bylo možné poskytovat aktualizace funkce, webové stránky ASP.NET uložit původní hodnoty vlastností entity při vykreslení stránky. Během zpracování ovládací prvek postbacku můžete znovu vytvořit původní stav entity a volat entity `Attach` metody před použitím změn a volání `SaveChanges` metody. Ve výchozím nastavení používají ovládací prvky webových formulářů ASP.NET dat zobrazení stavu k uložení původní hodnoty. Stav zobrazení však může ovlivnit výkon, protože je uložený v skrytá pole, které může významně zvětšit velikost stránky, která se odešle do a z prohlížeče.

Techniky pro správu stavu zobrazení, nebo alternativy, jako je například stav relace nejsou jedinečné Entity Framework, tak v tomto kurzu není přejít na toto téma podrobně. Další informace najdete v odkazech na konci tohoto kurzu.

Nicméně 4 verzi technologie ASP.NET poskytuje nový způsob práce se stav zobrazení, které byste měli vědět každý vývojář technologie ASP.NET webové formuláře aplikací: `ViewStateMode` vlastnost. Tato vlastnost lze nastavit na úrovni stránky nebo ovládací prvek a umožňuje zakázat zobrazení stavu ve výchozím nastavení pro stránku a povolit pouze pro ovládací prvky, které potřebujete.

Vhodné pro aplikace, kde je nejdůležitější výkon, je vždy zakázat stav zobrazení na úrovni stránky a povolit pouze pro ovládací prvky, které je vyžadují. Velikost zobrazení stavu na stránkách společnosti Contoso University nebude možné snížit podstatně touto metodou, ale pokud chcete zobrazit, jak to funguje, budete používat ho *Instructors.aspx* stránky. Tuto stránku obsahuje mnoho ovládacích prvků, včetně `Label` ovládací prvek, který se má zobrazit stav Zakázáno. Žádné ovládací prvky na této stránce ve skutečnosti je potřeba mít povolen stav zobrazení. ( `DataKeyNames` Vlastnost `GridView` ovládací prvek určuje stav, který je třeba udržovat mezi jednotlivými zpětnými odesláními, ale tyto hodnoty jsou uloženy v stav ovládacího prvku, který nemá vliv `ViewStateMode` vlastnosti.)

`Page` Směrnice a `Label` aktuálně značky ovládacího prvku se podobá následujícímu příkladu:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

Proveďte následující změny:

- Přidat `ViewStateMode="Disabled"` k `Page` směrnice.
- Odebrat `ViewStateMode="Disabled"` z `Label` ovládacího prvku.

Značky teď vypadá podobně jako v následujícím příkladu:

[!code-aspx[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Stav zobrazení je teď zakázané pro všechny ovládací prvky. Pokud chcete později přidat ovládací prvek, který se musí použít stav zobrazení, všechno, co potřebujete udělat je zahrnout `ViewStateMode="Enabled"` atribut pro ovládací prvek.

## <a name="using-the-notracking-merge-option"></a>Pomocí možnosti sloučení NoTracking

Když kontextu objektu načte řádky databáze, vytvoří objekty entity, které představují je ve výchozím nastavení také sleduje tyto objekty entity pomocí jeho správce stavu objektu. Tato data sledování funguje jako mezipaměť a používá se při aktualizaci entity. Protože webová aplikace obvykle má instance kontextu krátkodobých objektů, dotazy často vracející data, která není nutné ke sledování, protože objekt kontextu, který čte je uvolní předtím, než se znovu použít některý z entity, které načte nebo aktualizuje.

V rozhraní Entity Framework, můžete určit, zda kontext objektu sleduje nastavením objekty entity *merge – možnost*. Můžete nastavit možnost sloučení pro jednotlivé dotazy nebo sady entit. Pokud ji nastavíte pro sadu entit, to znamená, že nastavujete výchozí možnost sloučení pro všechny dotazy, které jsou vytvořeny pro tuto sadu entit.

Aplikace Contoso University, není potřeba sledování pro všechny sady entit, které lze používat z úložiště, abyste mohli nastavit možnost sloučení `NoTracking` pro tyto sady entit, když vytvoříte instanci objektu kontextu v třídě úložiště. (Všimněte si, že v tomto kurzu se nastavení možnosti sloučení nemá znatelný vliv na výkon vaší aplikace. Tím `NoTracking` možnosti se budou pravděpodobně uskutečňovat zlepšení výkonu pozorovatelných pouze v určitých scénářích dat velkého.)

Ve složce DAL otevřete *SchoolRepository.cs* a přidejte metodu konstruktor, který nastaví možnost sloučení pro sady entit, nemá přístup k úložišti:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

## <a name="pre-compiling-linq-queries"></a>Předem kompilaci dotazů LINQ

Při prvním Entity Framework provede dotaz Entity SQL v rámci životnosti daný `ObjectContext` instance, bude trvat nějakou dobu kompilace dotazu. Výsledek kompilace je uložené v mezipaměti, což znamená, že jsou mnohem rychlejší, protože další spuštění dotazu. Dotazy LINQ podle podobný vzorec, s tím rozdílem, že určitou část práce potřebné ke kompilaci dotazu se provádí při každém spuštění dotazu. Jinými slovy pro LINQ dotazy, ve výchozím nastavení všechny výsledky kompilace jsou uložené v mezipaměti.

Pokud máte dotaz LINQ, který plánujete spouštět opakovaně běžný kontextu objektu, můžete napsat kód, který způsobí, že všechny výsledky kompilace do mezipaměti při prvním spuštění dotazu LINQ.

Jako ukázku, provedete to pro dva `Get` metody v `SchoolRepository` tříd, z nichž jeden nepřijímá žádné parametry ( `GetInstructorNames` metoda) a ten, který vyžaduje parametr ( `GetDepartmentsByAdministrator` – metoda). Tyto metody jak se stát nyní ve skutečnosti není potřeba kompilovat, protože nejsou dotazů LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

Ale tak, aby si můžete vyzkoušet kompilované dotazy, které budete pokračovat, jako by tyto napsán jako následující dotazy LINQ:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Můžete změnit kód v těchto metodách na co má uvedené výše a spuštění aplikace pro ověření, zda funguje než budete pokračovat. Ale v následujících pokynech rovnou do vytváření Předkompilovaná verze.

Vytvořit soubor třídy v *DAL* složky, pojmenujte ho *SchoolEntities.cs*a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Tento kód vytvoří částečnou třídu, která rozšiřuje třídu automaticky generovaný kontext. Částečné třídy obsahuje dvě kompilované dotazy LINQ pomocí `Compile` metodu `CompiledQuery` třídy. Vytvoří také metody, které můžete použít k volání dotazy. Uložte a zavřete tento soubor.

Dále v *SchoolRepository.cs*, změňte existující `GetInstructorNames` a `GetDepartmentsByAdministrator` metody v úložišti třídy tak, aby volání kompilované dotazy:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

Spustit *Departments.aspx* stránku a zkontrolujte, jestli funguje stejně jako dříve. `GetInstructorNames` Metoda je volána k naplnění rozevíracího seznamu správce a `GetDepartmentsByAdministrator` metoda je volána při kliknutí na **aktualizace** ověří, že žádné kurzů vedených je správcem více než jednoho oddělení.

[![Image03](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

Jste předem kompilovaných dotazů v aplikaci Contoso University pouze se dozvíte, jak to provést, není, protože by mohly zlepšit zlepšení života na zemi výkonu. Předkompilace dotazů LINQ přidat úroveň složitosti kódu, proto se ujistěte, že můžete provést pouze pro dotazy, které ve skutečnosti představují problémových míst výkonu v aplikaci.

## <a name="examining-queries-sent-to-the-database"></a>Zkoumání odeslání dotazů do databáze

Pokud zkoumáte problémy s výkonem, někdy je užitečné vědět přesné příkazy SQL, které odesílá Entity Framework do databáze. Pokud pracujete s `IQueryable` objektu, provedete to jedním ze způsobů je použít `ToTraceString` metody.

V *SchoolRepository.cs*, změňte kód v `GetDepartmentsByName` metoda odpovídat následujícímu příkladu:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.cs)]

`departments` Proměnná musí být přetypovat na `ObjectQuery` zadejte pouze, protože `Where` metoda na konci na každém řádku vždy vytvoří `IQueryable` objektu; bez `Where` metody přetypování by být nutné.

Nastavit zarážku na `return` řádek a spusťte *Departments.aspx* stránky v ladicím programu. Pokud dostanete k zarážce, podívejte se `commandText` proměnné v **lokální** okno a použijte vizualizátor textu (na lupu v **hodnotu** sloupce) zobrazíte její hodnotu **Vizualizátor textu** okna. Můžete zobrazit celý příkaz SQL, která je výsledkem tento kód:

[![Image08](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Jako alternativu funkcí technologie IntelliTrace v sadě Visual Studio Ultimate poskytuje způsob, jak zobrazit příkazy SQL vygenerovaným rozhraním Entity Framework, která nevyžaduje, můžete změnit váš kód nebo dokonce nastavit zarážku.

> [!NOTE]
> Následující postupy můžete provést pouze v případě, že máte Visual Studio Ultimate.


Obnovit původní kód v `GetDepartmentsByName` metodu a poté spusťte *Departments.aspx* stránky v ladicím programu.

V sadě Visual Studio, vyberte **ladění** nabídce a potom **IntelliTrace**a potom **události IntelliTrace**.

[![Image11](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

V **IntelliTrace** okna, klikněte na tlačítko **příkaz Pozastavit vše**.

[![Image12](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

**IntelliTrace** okně se zobrazí seznam posledních událostí:

[![Image09](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klikněte na tlačítko **ADO.NET** řádku. Rozšiřuje zobrazit text příkazu:

[![Image10](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Celý příkaz textový řetězec můžete zkopírovat do schránky z **lokální** okna.

Předpokládejme, že jste pracovali s databází s další tabulky, vztahy a sloupců, než jednoduché `School` databáze. Můžete se setkat dotaz, který shromažďuje všechny informace potřebné v jediném `Select` příkazu, který obsahuje více `Join` klauzule stane příliš složitý k zajištění efektivní práce. V takovém případě můžete přepnout z eager načítání pro explicitní načtení pro zjednodušení dotazu.

Například, zkuste změnit kód v `GetDepartmentsByName` metoda *SchoolRepository.cs*. Aktuálně v tom, že metoda máte dotaz objektu, který má `Include` metody `Person` a `Courses` navigační vlastnosti. Nahradit `return` příkaz s kódem, který provádí explicitní načtení, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Spustit *Departments.aspx* stránce v ladicím programu a zkontrolujte **IntelliTrace** okno znovu jako jste to udělali dříve. Nyní tam, kde byla jednoho dotazu, uvidíte dlouhá pořadí z nich.

[![Image13](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klikněte na první **ADO.NET** řádku naleznete v tématu co se stalo s komplexní dotaz můžete prohlížet dříve.

[![Image14](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

Dotaz z oddělení stal jednoduchý `Select` dotazu bez `Join` klauzule, ale je následována samostatné dotazy, které načítají související kurzy a správce, používáte sadu dva dotazy pro každé oddělení vrácený původní dotaz.

> [!NOTE]
> Pokud opožděné načtení povoleno, vzor, který se zobrazí tady pomocí stejného dotazu opakuje tolikrát, kolikrát, může dojít z opožděné načtení. Vzor, který obvykle chcete se vyhnout se opožděné načtení souvisejících dat pro každý řádek v primární tabulce. Pokud jste ověřili, že jeden spojení dotazu je příliš složitý pro je efektivní, by obvykle mohli zlepšit výkon v takových případech změnou primární dotaz, který bude použit předběžné načítání.


## <a name="pre-generating-views"></a>Předem generování zobrazení

Když `ObjectContext` prvním vytvoření objektu v nové doméně aplikace, Entity Framework generuje sadu tříd, které používá pro přístup k databázi. Tyto třídy se nazývají *zobrazení*, a pokud máte velké datový model, generuje tato zobrazení zpozdit na webu odpověď na první žádost o stránku po inicializaci nové aplikační doméně. Toto zpoždění první žádosti můžete snížit vytváření zobrazení v době kompilace místo v době běhu.

> [!NOTE]
> Pokud aplikace nemá modelu velmi velkých objemů dat, nebo pokud ho máte modelu velkých objemů dat, ale nejste obavy o problém s výkonem, který má vliv pouze úplně první požadavek na stránku po recyklaci služby IIS, můžete tuto část přeskočit. Zobrazení vytváření nestane pokaždé, když vytvoříte instanci `ObjectContext` objektu, protože zobrazení jsou ukládány do mezipaměti v doméně aplikace. Proto pokud se často recyklace aplikace ve službě IIS, velmi málo žádostí stránek je výhodná předem vygenerovaných zobrazení.


Můžete předem vygenerovat zobrazení pomocí *EdmGen.exe* nástroj příkazového řádku nebo pomocí *Toolkit transformace šablony textu* šablony (T4). V tomto kurzu použijete šablonu T4.

V *DAL* složky, přidejte do souboru pomocí **textové šablony** šablony (je v části **Obecné** uzlu v **nainstalované šablony** seznamu), a pojmenujte ho *SchoolModel.Views.tt*. Nahraďte stávající kód v souboru následujícím kódem:

[!code-csharp[Main](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

Tento kód vytvoří zobrazení pro *edmx* soubor, který je umístěný ve stejné složce jako šablona a který má stejný název jako soubor šablony. Například, pokud je název souboru šablony *SchoolModel.Views.tt*, bude hledat soubor modelu dat s názvem *SchoolModel.edmx*.

Uložte soubor a pak klikněte pravým tlačítkem na soubor v **Průzkumníka řešení** a vyberte **spustit vlastní nástroj**.

[![Image02](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Visual Studio generuje soubor kódu, který vytvoří zobrazení, který se nazývá *SchoolModel.Views.cs* založený na šabloně. (Možná jste si všimli, že soubor kódu se generuje ještě dřív, než vyberete **spustit vlastní nástroj**, co nejdříve uložte soubor šablony.)

[![Image01](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Teď můžete spustit aplikaci a zkontrolujte, jestli funguje stejně jako dříve.

Další informace o předem vygenerovaných zobrazení naleznete na následujících odkazech:

- [Postupy: předem vygenerovat zobrazení ke zlepšení výkonu dotazů](https://msdn.microsoft.com/library/bb896240.aspx) na webové stránce MSDN. Vysvětluje způsob používání `EdmGen.exe` předem vygenerovat zobrazení nástroje příkazového řádku.
- [Izolaci výkonu pomocí Předkompilovaná/Předprodukčním-generated zobrazení v rozhraní Entity Framework 4](https://blogs.msdn.com/b/appfabriccat/archive/2010/08/06/isolating-performance-with-precompiled-pre-generated-views-in-the-entity-framework-4.aspx) na blogu Windows Server AppFabric zákaznického poradního týmu.

Dokončení tohoto postupu Úvod ke zlepšení výkonu ve webové aplikaci ASP.NET, která používá Entity Framework. Další informace naleznete v následujících materiálech:

- [Důležité informace o výkonu (Entity Framework)](https://msdn.microsoft.com/library/cc853327.aspx) na webové stránce MSDN.
- [Související s výkonem příspěvky na blogu týmu Entity Framework](https://blogs.msdn.com/b/adonet/archive/tags/performance/).
- [Možnosti a kompilované dotazy sloučit EF](https://blogs.msdn.com/b/dsimmons/archive/2010/01/12/ef-merge-options-and-compiled-queries.aspx). Blogový příspěvek, který vysvětluje neočekávané chování kompilované dotazy a sloučení možnosti, jako `NoTracking`. Pokud máte v plánu použít kompilované dotazy nebo s nastavením možností sloučení v aplikaci, přečtěte si nejprve tuto.
- [Entity Framework související příspěvky v blogu Data a modelování zákaznického poradního týmu](https://blogs.msdn.com/b/dmcat/archive/tags/entity+framework/). Zahrnuje příspěvky na kompilované dotazy a použít Visual Studio 2010 Profiler ke zjištění problémů s výkonem.
- [Vlákna ve fóru Entity Framework s Rady, jak ke zlepšení výkonu velice složitých dotazů](https://social.msdn.microsoft.com/Forums/adodotnetentityframework/thread/ffe8b2ab-c5b5-4331-8988-33a872d0b5f6).
- [Doporučení pro řízení stavu ASP.NET](https://msdn.microsoft.com/library/z1hkazw7.aspx).
- [Použití rozhraní Entity Framework a ObjectDataSource: vlastní stránkování](http://geekswithblogs.net/Frez/articles/using-the-entity-framework-and-the-objectdatasource-custom-paging.aspx). Blogový příspěvek, který staví na ContosoUniversity aplikace vytvořené v těchto kurzech vysvětlují, jak implementovat stránkování v *Departments.aspx* stránky.

V dalším kurzu kontroly některá důležitá vylepšení rozhraní Entity Framework, které jsou nové ve verzi 4.

> [!div class="step-by-step"]
> [Předchozí](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
> [další](what-s-new-in-the-entity-framework-4.md)
