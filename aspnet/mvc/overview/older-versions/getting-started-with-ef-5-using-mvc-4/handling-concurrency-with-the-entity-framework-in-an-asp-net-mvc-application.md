---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Zpracování souběžnosti s platformou Entity Framework v aplikaci ASP.NET MVC (7 10) | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b072134043ceda809bfeca98447a132ed407b323
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Zpracování souběžnosti s platformou Entity Framework v aplikaci ASP.NET MVC (7 10)
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozích dvou kurzech pracovali s související data. Tento kurz ukazuje, jak zpracovat souběžnosti. Vytvoříte webové stránky, které pracují s `Department` entitu a stránek, které upravovat a odstraňovat `Department` entity budou zpracovávat chyby souběžnosti. Na následujících obrázcích je Index a odstranění stránky, včetně některé zprávy, které se zobrazí, pokud dojde ke konfliktu souběžnosti.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Concurrency dojde ke konfliktu jeden uživatel zobrazí entity data ji Pokud chcete upravit, a pak jiný uživatel aktualizuje stejné entity data před první uživatel změnu je zapsána do databáze. Pokud nepovolíte detekce takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele. V mnoha aplikacích, je přijatelné toto riziko: Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhody. V takovém případě nemáte konfigurace aplikace pro zpracování konfliktů souběžnosti.

### <a name="pessimistic-concurrency-locking"></a>Pesimistické souběžnosti (uzamčení)

Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze. To se označuje jako *pesimistické souběžnosti*. Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci. Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit. Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.

Správa zámky má nevýhody. Může být složité programu. Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje (to znamená, že se nemá uspokojivé škálování). Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti. Rozhraní Entity Framework poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.

### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování

Je alternativa k pesimistické souběžnosti *optimistickou metodu souběžného*. Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je. Například Jan spustí oddělení stránce Upravit změny **nároky** velikost pro angličtinu oddělení z $350,000.00 0,00 Kč.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Před kliknutím na Jan **Uložit**, Jana spustí na stránku a změny **počáteční datum** pole z 9/1/2007 do 8/8 nebo 2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Jan klikne **Uložit** první a klikne na jeho změna po návratu prohlížeč na indexovou stránku, pak Jana vidí **Uložit**. Co se stane dále je určen jak zpracování konfliktů souběžnosti. Mezi možnosti patří následující:

- Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi. V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele. Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jan a Jana změny – počáteční datum 8/8/2013 a nároky nulové dolarů.

    Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat, ale nemůže vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity. Jestli Entity Framework funguje takto závisí na tom, jak implementovat aktualizace kódu. Je často není praktické ve webové aplikaci, protože může vyžadovat, aby udržení přehledu o všechny původní hodnoty vlastností pro entitu a také nové hodnoty udržovat velkých objemů stavu. Správa velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru nebo musí být součástí webové stránky (například v skrytá pole).
- Můžete je nechat Jana na změnu Jan pro změna přepsána. Při příštím někdo umožňuje anglické oddělení, se zobrazí 8/8/2013 a obnovený $350,000.00 hodnotu. Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář. (Hodnoty klienta přednost co je v úložišti.) Jak jsme uvedli v Úvod do této části, pokud tak učiníte jakéhokoli kódování pro zpracování souběžnosti, to se stane automaticky.
- Jana na změnu může zabránit aktualizaci v databázi. By obvykle, zobrazí se chybová zpráva, mu zobrazit aktuální stav dat a mu jeho změny znovu použijte, pokud chce je provést. Tento postup se nazývá *Wins úložiště* scénář. (Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu budete implementovat scénář úložiště služby Wins. Tato metoda zajišťuje, že jsou bez uživatele se zobrazí upozornění, na co se děje přepsat žádné změny.

### <a name="detecting-concurrency-conflicts"></a>Zjišťování konfliktů souběžnosti

Můžete vyřešit konflikty zpracování [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) výjimky, které vyvolá rozhraní Entity Framework. Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework. Proto musíte nakonfigurovat databázi a datový model správně. Některé možnosti aktivace zjišťování konfliktů, patří:

- V tabulce databáze patří sledování sloupec, který slouží k určení, kdy se změnil na řádek. Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v `Where` klauzuli SQL `Update` nebo `Delete` příkazy.

    Datový typ sloupce sledování je obvykle [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek. V `Update` nebo `Delete` příkaz, `Where` klauzule obsahuje původní hodnota sloupce sledování (původní verze řádku). Došlo ke změně řádek aktualizován jiným uživatelem, hodnota v `rowversion` sloupec je jiná než původní hodnota, proto `Update` nebo `Delete` příkaz nelze najít řádek, abyste aktualizovat z důvodu `Where` klauzule. Při rozhraní Entity Framework zjistí, že žádné řádky bylo aktualizováno správcem `Update` nebo `Delete` příkazů (to znamená, když počet ovlivněných řádků je nulová), který interpretuje jako konflikt souběžnosti.
- Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v `Where` klauzuli `Update` a `Delete` příkazy.

    Jako první možnost, pokud nic v řádku od řádek byl nejdřív přečíst, změnila `Where` klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti. Pro tabulky databáze, které mají mnoho sloupců, tento přístup může mít za následek velké `Where` klauzule a může vyžadovat udržovat velkých objemů stavu. Jak již bylo uvedeno dříve, může ovlivnit zachování velkých objemů stav výkonu aplikace protože ji vyžaduje prostředky serveru nebo musí být součástí webové stránky. Proto tento přístup obecně nedoporučuje, a není to metoda použitá v tomto kurzu.

    Pokud chcete implementovat tento přístup k concurrency, je nutné označit všechny vlastnosti primárního klíče entity, kterou chcete sledovat souběžnosti pro přidáním [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) je atribut. Aby změna umožňuje rozhraní Entity Framework zahrňte všechny sloupce v SQL `WHERE` klauzuli `UPDATE` příkazy.

Ve zbývající části tohoto kurzu přidáte [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) sledování vlastnost, která má `Department` entity, vytvořte řadič a zobrazení a otestovat a ověřit, že všechno funguje správně.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Přidat do Entity oddělení ve vlastnosti optimistickou metodu souběžného zpracování

V *Models\Department.cs*, přidejte sledování vlastnost s názvem `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[Časové razítko](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) atribut určuje, že tento sloupec bude součástí `Where` klauzuli `Update` a `Delete` příkazy, odeslané do databáze. Atribut se nazývá [časové razítko](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) protože předchozí verze systému SQL Server používá SQL [časové razítko](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) datového typu než SQL [rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) jej nahradit. Typ formátu .net pro `rowversion` je bajtové pole. Pokud dáváte přednost použijte rozhraní fluent API, můžete použít [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) metoda k určení vlastnosti, sledování, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Přidáním vlastnosti jste změnili model databáze, takže je třeba provést další migraci. V balíček správce konzoly (pomocí PMC), zadejte následující příkazy:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Vytvořit řadič oddělení

Vytvoření `Department` řadiče a zobrazení stejně jako jste to udělali se ostatní řadiče, pomocí následujících nastavení:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

V *Controllers\DepartmentController.cs*, přidejte `using` příkaz:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Změna "LastName" na "FullName" everywhere v tomto souboru (čtyři výskytů) tak, aby se oddělení správce rozevírací seznamy bude obsahovat celý název lektorem a nikoli pouze poslední název.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Nahraďte existující kód pro `HttpPost` `Edit` metoda následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Zobrazení uloží původní `RowVersion` hodnota ve skrytém poli. Když vazač modelu vytvoří `department` instance, tento objekt bude mít původní `RowVersion` hodnota vlastnosti a nové hodnoty pro ostatní vlastnosti, jako zadá uživatel na stránce Upravit. Pak když rozhraní Entity Framework vytvoří SQL `UPDATE` příkaz, že bude obsahovat příkaz `WHERE` klauzuli, která hledá řádek, který má původní `RowVersion` hodnota.

Pokud žádné řádky jsou ve `UPDATE` příkazu (žádné řádky mají původní `RowVersion` hodnotu), vyvolá rozhraní Entity Framework `DbUpdateConcurrencyException` výjimku a kód v `catch` bloku získá ovlivněný `Department` entita z výjimka objekt. Tato entita má hodnoty čtení z databáze a nové hodnoty zadané uživatelem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

V dalším kroku kód přidá vlastní chybovou zprávu pro každý sloupec, který má databáze hodnoty liší od uživatele zadaný na stránce Upravit:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Delší chybová zpráva vysvětluje, co se stalo a co dělat informace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Nakonec kód nastaví `RowVersion` hodnotu `Department` objekt na novou hodnotu načíst z databáze. Tento nový `RowVersion` hodnota bude uložena ve skrytém poli, když úpravy stránka se zobrazí znovu a další čas uživatel klikne na **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, redisplay upravit stránky.

V *Views\Department\Edit.cshtml*, přidání skrytá pole Uložit `RowVersion` hodnotu vlastnosti, hned za skryté pole pro `DepartmentID` vlastnost:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

V *Views\Department\Index.cshtml*, nahraďte existující kód následujícím kódem řádek odkazy budou přesunuty doleva a změnit záhlaví stránky název a ve sloupci zobrazit `FullName` místo `LastName` v **Správce** sloupce:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testování optimistickou metodu souběžného zpracování

Spuštění tohoto webu a klikněte na tlačítko **oddělení**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klikněte pravým tlačítkem **upravit** hypertextový odkaz pro Jan Novak a vyberte **otevřít na nové kartě** klikněte **upravit** hypertextový odkaz pro Jan Novak. Obě okna zobrazí stejné informace.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Změňte pole v první okno prohlížeče a klikněte na tlačítko **Uložit**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Prohlížeč zobrazí stránku Index s změněné hodnoty.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Každé pole v okně prohlížeče druhý a klikněte na tlačítko **Uložit**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klikněte na tlačítko **Uložit** ve druhém okně prohlížeče. Zobrazí chybovou zprávu:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klikněte na tlačítko **Uložit** znovu. Hodnota, kterou jste zadali v druhé prohlížeče je uložit spolu s původní hodnotu data, která můžete změnit v první prohlížeči. Zobrazí uložené hodnoty, když se zobrazí stránka indexu.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizace stránky odstranění

Rozhraní Entity Framework pro stránku odstranit zjistí souběžnosti konflikty způsobené někdo jinak úpravy oddělení podobným způsobem. Když `HttpGet` `Delete` metoda zobrazí potvrzení zobrazení, zobrazení zahrnuje původní `RowVersion` hodnota ve skrytém poli. Hodnota je pak možné `HttpPost` `Delete` metoda, která je volána, když uživatel potvrdí odstranění. Když Entity Framework vytvoří SQL `DELETE` příkaz, obsahuje `WHERE` klauzuli with původní `RowVersion` hodnota. Pokud výsledky příkazu v nulový počet řádků (tj. řádek byl změněn, jakmile se zobrazí stránka potvrzení odstranění), je vyvolána výjimka souběžnosti a `HttpGet Delete` metoda je volána příznakem chyba nastavena na `true` Chcete-li znovu zobrazit potvrzovací stránku s chybovou zprávou. Je také možné, že byly ovlivňuje nulový počet řádků, protože řádek byl odstraněn jiným uživatelem, tak v takovém případě se zobrazí různé chybová zpráva.

V *DepartmentController.cs*, nahraďte `HttpGet` `Delete` metoda následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Metodu je možné zadat volitelný parametr, který označuje, zda stránky se se zobrazí znovu po chybě souběžnosti. Pokud je tento příznak `true`, je odeslána chybová zpráva pro zobrazení s využitím `ViewBag` vlastnost.

Nahraďte kód v `HttpPost` `Delete` – metoda (s názvem `DeleteConfirmed`) s následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

V automaticky generovaný kód, který právě nahrazen tato metoda povoleny pouze ID záznamu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Změnili jste tento parametr `Department` instance entity vytvořené vazač modelu. To vám umožní přístup k `RowVersion` hodnotu vlastnosti kromě klíč záznamu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód s názvem `HttpPost` `Delete` metoda `DeleteConfirmed` umožnit `HttpPost` metoda jedinečný podpis. (Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro stejný název `HttpPost` a `HttpGet` odstranit metody.

Pokud je chyba souběžnosti zachycena, kód znovu zobrazí stránka potvrzení odstranění a poskytuje příznak, které označují, že by měl zobrazit chybovou zprávu souběžnosti.

V *Views\Department\Delete.cshtml*, nahraďte automaticky generovaný kód s následující kód, který umožňuje, některé formátování změny a přidá na pole chybová zpráva. Změny se zvýrazněnou.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Tento kód přidá chybovou zprávu mezi `h2` a `h3` záhlaví:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Nahradí `LastName` s `FullName` v `Administrator` pole:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Nakonec přidá skrytá pole pro `DepartmentID` a `RowVersion` vlastnosti po `Html.BeginForm` příkaz:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Spusťte oddělení indexovou stránku. Klikněte pravým tlačítkem **odstranit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít v novém okně** v prvním okně klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.

V prvním okně, změňte jednu z hodnot a klikněte na tlačítko **Uložit** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Indexovou stránku potvrdí změny.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

V okně druhý klikněte na **odstranit**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Zobrazí se zpráva Chyba souběžnosti a hodnoty oddělení aktualizovaly s co je aktuálně v databázi.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Pokud kliknete na tlačítko **odstranit** znovu, budete přesměrováni na indexovou stránku, která ukazuje, že byla odstraněna z oddělení.

## <a name="summary"></a>Souhrn

Tím dokončíte Úvod pro zpracování konfliktů souběžnosti. Informace o jiných způsobech zpracovávat různé scénáře souběžného zpracování najdete v tématu [optimistickou metodu souběžného zpracování vzory](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) a [práce s hodnotami vlastností](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) na blog týmu rozhraní Entity Framework. Další kurz ukazuje, jak implementovat tabulky za hierarchie dědičnosti pro `Instructor` a `Student` entity.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[další](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
