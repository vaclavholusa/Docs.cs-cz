---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Zpracování souběžnosti s platformou Entity Framework 6 v aplikaci ASP.NET MVC 5 (10 12) | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b92aded80ad6b435a2409a137bb96fe4d0a726f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Zpracování souběžnosti s platformou Entity Framework 6 v aplikaci ASP.NET MVC 5 (10 12)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V dřívějších kurzech jste zjistili, jak aktualizovat data. Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu.

Změníte webové stránky, které pracují se `Department` entity tak, aby se budou zpracovávat chyby souběžnosti. Na následujících obrázcích je Index a odstranění stránky, včetně některé zprávy, které se zobrazí, pokud dojde ke konfliktu souběžnosti.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Konflikty souběžnosti

Concurrency dojde ke konfliktu jeden uživatel zobrazí entity data ji Pokud chcete upravit, a pak jiný uživatel aktualizuje stejné entity data před první uživatel změnu je zapsána do databáze. Pokud nepovolíte detekce takové konflikty, kdo aktualizuje databázi přepíše poslední změny jiného uživatele. V mnoha aplikacích, je přijatelné toto riziko: Pokud existuje několik uživatelů nebo několik aktualizací, nebo pokud není opravdu důležité, pokud jsou některé změny přepsány, náklady na programování pro concurrency vyváží výhody. V takovém případě nemáte konfigurace aplikace pro zpracování konfliktů souběžnosti.

### <a name="pessimistic-concurrency-locking"></a>Pesimistické souběžnosti (uzamčení)

Pokud aplikace třeba zabránit, aby před náhodnou ztrátou dat v concurrency scénáře, je jeden ze způsobů použití uzamčení databáze. To se označuje jako *pesimistické souběžnosti*. Například předtím, než se pustíte do čtení řádek z databáze, můžete požádat o zámku pro jen pro čtení nebo pro přístup k aktualizaci. Pokud zamknete řádek pro přístup k aktualizaci, je povoleno uzamčení řádek buď pro žádní jiní uživatelé jen pro čtení nebo aktualizace přístup, protože by získají kopii dat, která je právě probíhá změnit. Pokud zamknete řádek pro oprávnění jen pro čtení, ostatní také možné zamknout ho pro přístup jen pro čtení, ale není pro aktualizaci.

Správa zámky má nevýhody. Může být složité programu. Vyžaduje významné databáze správy prostředků, a jako počet uživatelů, aplikace, může to způsobit problémy s výkonem zvyšuje. Z těchto důvodů ne všechny databázové systémy podporovat pesimistické souběžnosti. Rozhraní Entity Framework poskytuje žádné integrovanou podporu pro něj, a v tomto kurzu není ukazují, jak ho implementovat.

### <a name="optimistic-concurrency"></a>Optimistickou metodu souběžného zpracování

Je alternativa k pesimistické souběžnosti *optimistickou metodu souběžného*. Optimistickou metodu souběžného znamená povolení konfliktů souběžnosti provést a reaktivní správně, pokud tomu tak je. Například Jan spustí oddělení stránce Upravit změny **nároky** velikost pro angličtinu oddělení z $350,000.00 0,00 Kč.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Před kliknutím na Jan **Uložit**, Jana spustí na stránku a změny **počáteční datum** pole z 9/1/2007 do 8/8 nebo 2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Jan klikne **Uložit** první a klikne na jeho změna po návratu prohlížeč na indexovou stránku, pak Jana vidí **Uložit**. Co se stane dále je určen jak zpracování konfliktů souběžnosti. Mezi možnosti patří následující:

- Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi. V ukázkovém scénáři by byl žádná data ztratí, protože různé vlastnosti aktualizoval dva uživatele. Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jan a Jana změny – počáteční datum 8/8/2013 a nároky nulové dolarů.

    Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat, ale nemůže vyhnuli ztrátě dat, pokud konkurenční změn stejnou vlastnost entity. Jestli Entity Framework funguje takto závisí na tom, jak implementovat aktualizace kódu. Je často není praktické ve webové aplikaci, protože může vyžadovat, aby udržení přehledu o všechny původní hodnoty vlastností pro entitu a také nové hodnoty udržovat velkých objemů stavu. Zachování velkých objemů stavu může ovlivnit výkon aplikace, protože buď vyžaduje prostředky serveru nebo musí být součástí webové stránky (například v skrytá pole) nebo do souboru cookie.
- Můžete je nechat Jana na změnu Jan pro změna přepsána. Při příštím někdo umožňuje anglické oddělení, se zobrazí 8/8/2013 a obnovený $350,000.00 hodnotu. Tento postup se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář. (Všechny hodnoty z klienta přednost co je v úložišti.) Jak jsme uvedli v Úvod do této části, pokud tak učiníte jakéhokoli kódování pro zpracování souběžnosti, to se stane automaticky.
- Jana na změnu může zabránit aktualizaci v databázi. By obvykle, zobrazí se chybová zpráva, mu zobrazit aktuální stav dat a mu jeho změny znovu použijte, pokud chce je provést. Tento postup se nazývá *Wins úložiště* scénář. (Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu budete implementovat scénář úložiště služby Wins. Tato metoda zajišťuje, že jsou bez uživatele se zobrazí upozornění, na co se děje přepsat žádné změny.

### <a name="detecting-concurrency-conflicts"></a>Zjišťování konfliktů souběžnosti

Můžete vyřešit konflikty zpracování [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) výjimky, které vyvolá rozhraní Entity Framework. Chcete-li vědět, kdy má být vyvolána tyto výjimky, musí být schopna zjistit konflikty rozhraní Entity Framework. Proto musíte nakonfigurovat databázi a datový model správně. Některé možnosti aktivace zjišťování konfliktů, patří:

- V tabulce databáze patří sledování sloupec, který slouží k určení, kdy se změnil na řádek. Potom můžete nakonfigurovat rozhraní Entity Framework zahrnovat tento sloupec v `Where` klauzuli SQL `Update` nebo `Delete` příkazy.

    Datový typ sloupce sledování je obvykle [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) hodnota je pořadové číslo, které se zvýší pokaždé, když se aktualizuje na řádek. V `Update` nebo `Delete` příkaz, `Where` klauzule obsahuje původní hodnota sloupce sledování (původní verze řádku). Došlo ke změně řádek aktualizován jiným uživatelem, hodnota v `rowversion` sloupec je jiná než původní hodnota, proto `Update` nebo `Delete` příkaz nelze najít řádek, abyste aktualizovat z důvodu `Where` klauzule. Při rozhraní Entity Framework zjistí, že žádné řádky bylo aktualizováno správcem `Update` nebo `Delete` příkazů (to znamená, když počet ovlivněných řádků je nulová), který interpretuje jako konflikt souběžnosti.
- Nakonfigurujte rozhraní Entity Framework zahrnují původní hodnoty každý sloupec v tabulce v `Where` klauzuli `Update` a `Delete` příkazy.

    Jako první možnost, pokud nic v řádku od řádek byl nejdřív přečíst, změnila `Where` klauzule nevrátí řádek aktualizace, které rozhraní Entity Framework interpretuje jako konflikt souběžnosti. Pro tabulky databáze, které mají mnoho sloupců, tento přístup může mít za následek velké `Where` klauzule a může vyžadovat udržovat velkých objemů stavu. Jak již bylo uvedeno dříve, údržbu velkých objemů stavu může ovlivnit výkon aplikace. Proto se obecně nedoporučuje tento přístup, a není to metoda použitá v tomto kurzu.

    Pokud chcete implementovat tento přístup k concurrency, je nutné označit všechny vlastnosti primárního klíče entity, kterou chcete sledovat souběžnosti pro přidáním [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) je atribut. Aby změna umožňuje rozhraní Entity Framework zahrňte všechny sloupce v SQL `WHERE` klauzuli `UPDATE` příkazy.

Ve zbývající části tohoto kurzu přidáte [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) sledování vlastnost, která má `Department` entity, vytvořte řadič a zobrazení a otestovat a ověřit, že všechno funguje správně.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Přidat do Entity oddělení ve vlastnosti optimistickou metodu souběžného zpracování

V *Models\Department.cs*, přidejte sledování vlastnost s názvem `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[Časové razítko](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) atribut určuje, že tento sloupec bude součástí `Where` klauzuli `Update` a `Delete` příkazy, odeslané do databáze. Atribut se nazývá [časové razítko](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) protože předchozí verze systému SQL Server používá SQL [časové razítko](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) datového typu než SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) jej nahradit. Typ formátu .net pro [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) je bajtové pole.

Pokud dáváte přednost použijte rozhraní fluent API, můžete použít [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) metoda k určení vlastnosti, sledování, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Přidáním vlastnosti jste změnili model databáze, takže je třeba provést další migraci. V balíček správce konzoly (pomocí PMC), zadejte následující příkazy:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Upravit řadičem oddělení

V *Controllers\DepartmentController.cs*, přidejte `using` příkaz:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

V *DepartmentController.cs* souboru, změňte všechny čtyři výskyty "LastName" na "FullName" tak, aby oddělení správce rozevírací seznamy bude obsahovat celý název lektorem a nikoli pouze poslední název.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Nahraďte existující kód pro `HttpPost` `Edit` metoda následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Pokud `FindAsync` metoda vrátí hodnotu null, z oddělení byla odstraněna jiným uživatelem. Kód zobrazí používá hodnoty odeslaného formuláře k vytvoření entity oddělení tak, aby stránce Upravit můžete zobrazí znovu, zobrazí se chybová zpráva. Jako alternativu nebude muset znovu vytvořit entitu oddělení, pokud se zobrazí pouze se chybová zpráva bez opakované zobrazování pole oddělení.

Zobrazení ukládá původní `RowVersion` hodnotu ve skrytém poli a metodu, obdrží ho `rowVersion` parametr. Před voláním `SaveChanges`, budete muset uvést, původní `RowVersion` hodnoty vlastností v `OriginalValues` kolekce pro entitu. Pak když rozhraní Entity Framework vytvoří SQL `UPDATE` příkaz, že bude obsahovat příkaz `WHERE` klauzuli, která hledá řádek, který má původní `RowVersion` hodnota.

Pokud žádné řádky jsou ve `UPDATE` příkazu (žádné řádky mají původní `RowVersion` hodnotu), vyvolá rozhraní Entity Framework `DbUpdateConcurrencyException` výjimku a kód v `catch` bloku získá ovlivněný `Department` entita z výjimka objekt.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Tento objekt obsahuje nové hodnoty zadané uživatelem v jeho `Entity` vlastnost a můžete získat hodnoty čtení z databáze voláním `GetDatabaseValues` metoda.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues` Metoda vrátí hodnotu null, pokud někdo z databáze odstranila řádek; jinak, budete muset přetypovat vrácený objekt, který má `Department` třídy, aby měli přístup `Department` vlastnosti. (Vzhledem k tomu, že jste již zaškrtnuté pro odstranění, `databaseEntry` by mít hodnotu null jenom v případě, že byla odstraněna z oddělení po `FindAsync` spustí a před `SaveChanges` provede.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

V dalším kroku kód přidá vlastní chybovou zprávu pro každý sloupec, který má databáze hodnoty liší od uživatele zadaný na stránce Upravit:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Delší chybová zpráva vysvětluje, co se stalo a co dělat informace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Nakonec kód nastaví `RowVersion` hodnotu `Department` objekt na novou hodnotu načíst z databáze. Tento nový `RowVersion` hodnota bude uložena ve skrytém poli, když úpravy stránka se zobrazí znovu a další čas uživatel klikne na **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, redisplay upravit stránky.

V *Views\Department\Edit.cshtml*, přidání skrytá pole Uložit `RowVersion` hodnotu vlastnosti, hned za skryté pole pro `DepartmentID` vlastnost:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Testování optimistickou metodu souběžného zpracování

Spuštění tohoto webu a klikněte na tlačítko **oddělení**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klikněte pravým tlačítkem **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě** klikněte **upravit** hypertextový odkaz pro angličtinu oddělení. Dvě karty zobrazí stejné informace.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Změňte pole na první kartě prohlížeče a klikněte na tlačítko **Uložit**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Prohlížeč zobrazí stránku Index s změněné hodnoty.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Změňte pole v druhé záložce prohlížeče a klikněte na tlačítko **Uložit**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klikněte na tlačítko **Uložit** druhý záložce prohlížeče. Zobrazí chybovou zprávu:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klikněte na tlačítko **Uložit** znovu. Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit spolu s původní hodnotu data, která jste změnili v první prohlížeči. Zobrazí uložené hodnoty, když se zobrazí stránka indexu.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizace stránky odstranění

Rozhraní Entity Framework pro stránku odstranit zjistí souběžnosti konflikty způsobené někdo jinak úpravy oddělení podobným způsobem. Když `HttpGet` `Delete` metoda zobrazí potvrzení zobrazení, zobrazení zahrnuje původní `RowVersion` hodnota ve skrytém poli. Hodnota je pak možné `HttpPost` `Delete` metoda, která je volána, když uživatel potvrdí odstranění. Když Entity Framework vytvoří SQL `DELETE` příkaz, obsahuje `WHERE` klauzuli with původní `RowVersion` hodnota. Pokud výsledky příkazu v nulový počet řádků (tj. řádek byl změněn, jakmile se zobrazí stránka potvrzení odstranění), je vyvolána výjimka souběžnosti a `HttpGet Delete` metoda je volána příznakem chyba nastavena na `true` Chcete-li znovu zobrazit potvrzovací stránku s chybovou zprávou. Je také možné, že byly ovlivňuje nulový počet řádků, protože řádek byl odstraněn jiným uživatelem, tak v takovém případě se zobrazí různé chybová zpráva.

V *DepartmentController.cs*, nahraďte `HttpGet` `Delete` metoda následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Metodu je možné zadat volitelný parametr, který označuje, zda stránky se se zobrazí znovu po chybě souběžnosti. Pokud je tento příznak `true`, je odeslána chybová zpráva pro zobrazení s využitím `ViewBag` vlastnost.

Nahraďte kód v `HttpPost` `Delete` – metoda (s názvem `DeleteConfirmed`) s následujícím kódem:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

V automaticky generovaný kód, který právě nahrazen tato metoda povoleny pouze ID záznamu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Změnili jste tento parametr `Department` instance entity vytvořené vazač modelu. To vám umožní přístup k `RowVersion` hodnotu vlastnosti kromě klíč záznamu.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód s názvem `HttpPost` `Delete` metoda `DeleteConfirmed` umožnit `HttpPost` metoda jedinečný podpis. (Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro stejný název `HttpPost` a `HttpGet` odstranit metody.

Pokud je chyba souběžnosti zachycena, kód znovu zobrazí stránka potvrzení odstranění a poskytuje příznak, které označují, že by měl zobrazit chybovou zprávu souběžnosti.

V *Views\Department\Delete.cshtml*, nahraďte následující kód, který přidá na pole zpráva Chyba a skrytá pole vlastností DepartmentID a RowVersion automaticky generovaný kód. Změny se zvýrazněnou.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Tento kód přidá chybovou zprávu mezi `h2` a `h3` záhlaví:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Nahradí `LastName` s `FullName` v `Administrator` pole:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Nakonec přidá skrytá pole pro `DepartmentID` a `RowVersion` vlastnosti po `Html.BeginForm` příkaz:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Spusťte oddělení indexovou stránku. Klikněte pravým tlačítkem **odstranit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě** na první kartě klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.

V prvním okně, změňte jednu z hodnot a klikněte na tlačítko **Uložit** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Indexovou stránku potvrdí změny.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Na kartě druhý klikněte na tlačítko **odstranit**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Zobrazí se zpráva Chyba souběžnosti a hodnoty oddělení aktualizovaly s co je aktuálně v databázi.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Pokud kliknete na tlačítko **odstranit** znovu, budete přesměrováni na indexovou stránku, která ukazuje, že byla odstraněna z oddělení.

## <a name="summary"></a>Souhrn

Tím dokončíte Úvod pro zpracování konfliktů souběžnosti. Informace o jiných způsobech zpracovávat různé scénáře souběžného zpracování najdete v tématu [optimistickou metodu souběžného zpracování vzory](https://msdn.microsoft.com/data/jj592904) a [práce s hodnotami vlastností](https://msdn.microsoft.com/data/jj592677) na webu MSDN. Další kurz ukazuje, jak implementovat tabulky za hierarchie dědičnosti pro `Instructor` a `Student` entity.

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
