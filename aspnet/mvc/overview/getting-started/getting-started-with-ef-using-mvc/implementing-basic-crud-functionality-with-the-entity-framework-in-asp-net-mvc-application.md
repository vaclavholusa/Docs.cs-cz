---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementace funkce základní CRUD s platformou Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/09/2015
ms.topic: article
ms.assetid: a2f70ba4-83d1-4002-9255-24732726c4f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 14f5143bb5086890d4a2f2fb3b98f1be88a549a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application"></a>Implementace funkce základní CRUD s platformou Entity Framework v aplikaci ASP.NET MVC
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V předchozích kurzu jste vytvořili aplikaci MVC, která uchovává a zobrazí data pomocí rozhraní Entity Framework a SQL Server LocalDB. V tomto kurzu budete zkontrolujte a přizpůsobit CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód, který generování uživatelského rozhraní MVC pro vás automaticky vytvoří v kontrolery a zobrazení.

> [!NOTE]
> Je běžnou praxí implementovat vzor úložiště před vytvořením vrstvu abstrakce mezi řadiči a vrstva přístupu k datům. Tyto kurzy zachovat jednoduché a zaměřují se na výuky použití rozhraní Entity Framework, samotné, nepoužívejte úložiště. Informace o tom, jak implementovat úložiště najdete v tématu [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).


V tomto kurzu vytvoříte následující webové stránky:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

## <a name="create-a-details-page"></a>Vytvořte stránku Podrobnosti

Automaticky generovaný kód na studentů `Index` vynecháno stránky `Enrollments` vlastnost, protože tato vlastnost obsahuje kolekci. V `Details` stránku budete zobrazení obsahu kolekce do tabulky HTML.

 V *Controllers\StudentController.cs*, metoda akce pro `Details` zobrazení používá [najít](https://msdn.microsoft.com/library/gg696418(v=VS.103).aspx) metoda pro načtení jedné `Student` entity. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

Hodnota klíče je předaný metodě jako `id` parametr a pocházejí z *směrování dat* v **podrobnosti** hypertextový odkaz na indexovou stránku.

### <a name="tip-route-data"></a>Tip: **směrování dat**

Data trasy, která jsou data, která v segment adresy URL zadané ve směrovací tabulce nalezena vazač modelu. Například výchozí trasa určuje `controller`, `action`, a `id` segmenty:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cs?highlight=3)]

V následující adresu URL, výchozí trasu mapuje `Instructor` jako `controller`, `Index` jako `action` a 1 jako `id`; jsou hodnoty dat trasy.

`http://localhost:1230/Instructor/Index/1?courseID=2021`

"? courseID = 2021" je hodnotu řetězce dotazu. Vazač modelu bude také fungovat, pokud předáte `id` jako hodnotu řetězce dotazu:

`http://localhost:1230/Instructor/Index?id=1&CourseID=2021`

Adresy URL, které jsou vytvořené pomocí `ActionLink` příkazy v zobrazení syntaxe Razor. V následujícím kódu `id` parametr odpovídá výchozí trasu, takže `id` se přidá do data trasy.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml)]

V následujícím kódu `courseID` neodpovídá parametr ve výchozí trasu, přidá se jako řetězec dotazu.

[!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cshtml)]


1. Open *Views\Student\Details.cshtml*. Každé pole se zobrazí, použití `DisplayFor` pomocné rutiny, jak je znázorněno v následujícím příkladu:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cshtml)]
2. Po `EnrollmentDate` pole a bezprostředně před uzavírací `</dl>` značky, přidejte zvýrazněný zobrazíte seznam registrace, jak je znázorněno v následujícím příkladu:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml?highlight=8-29)]

    Pokud kód odsazení nesprávný po vložte kód, stiskněte CTRL-tisíc-D opravte ho.

    Tento kód projde entity v `Enrollments` navigační vlastnost. Pro každou `Enrollment` entity ve vlastnosti, zobrazí název kurzu a úrovni. Název postupu načten z `Course` entity, která je uložená v `Course` vlastnost navigace `Enrollments` entity. Všechna tato data je načíst z databáze automaticky, když je to potřeba. (Jinými slovy, používáte opožděného načítání zde. Nezadali jste *přes načítání* pro `Courses` navigační vlastnost, tak registrace nebyly načteny ve stejném dotazu, který získali na studentů. Místo toho při prvním pokusu o přístup `Enrollments` navigační vlastnost, nové bude odeslán dotaz do databáze načíst data. Další informace o opožděného načítání a přes načítání v [čtení související Data](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série.)
3. Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **podrobnosti** odkaz pro Alexander Carsonovy. (Pokud je otevřen soubor Details.cshtml, stiskněte CTRL + F5, získáte chybu HTTP 400 protože Visual Studio se pokusí spustit stránce s podrobnostmi o, ale nebylo dosaženo pomocí odkazu, který určuje student k zobrazení. In that Case, právě odeberte "Student/podrobnosti" z adresy URL a zkuste to znovu, nebo zavřete prohlížeč, klikněte pravým tlačítkem na projekt a klikněte na tlačítko **zobrazení**a potom klikněte na **zobrazit v prohlížeči**.)

    Zobrazí seznam kurzů a tříd pro vybrané student:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="update-the-create-page"></a>Vytvořit stránku aktualizace

1. V *Controllers\StudentController.cs*, nahraďte `HttpPost``Create` metodu akce pomocí následujícího kódu přidat `try-catch` blokovat a odeberte `ID` z [atribut vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) pro vygenerované metoda:

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=3,5-6,13-18)]

    Tento kód přidá `Student` entity vytvořené vazač modelu ASP.NET MVC do `Students` entitu nastavení a pak uloží změny do databáze. (*Vazač modelu* odkazuje na funkci ASP.NET MVC, díky, což usnadňuje práci s data odeslaná formuláře; vazač modelu převede odeslání formuláře hodnot pro typy CLR a předává je na metodu akce v parametrech. In this Case, vytvoří instanci vazače modelu `Student` entity můžete pomocí vlastnosti hodnoty z `Form` kolekce.)

    Můžete odebrat `ID` z vazeb atributů, protože `ID` je hodnotu primárního klíče, který systému SQL Server bude nastavení automaticky při vložit řádek. Vstup od uživatele nenastaví `ID` hodnotu.

    <a id="overpost"></a>

    ### <a name="security-warning---the-validateantiforgerytoken-attribute-helps-prevent-cross-site-request-forgerysecurityxsrfcsrf-prevention-in-aspnet-mvc-and-web-pagesmd-attacks-it-requires-a-corresponding-htmlantiforgerytoken-statement-in-the-view-which-youll-see-later"></a>Upozornění zabezpečení – `ValidateAntiForgeryToken` atribut pomáhá zabránit [padělání požadavku posílaného mezi weby](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) útoky. Vyžaduje odpovídající `Html.AntiForgeryToken()` příkaz v zobrazení, která se zobrazí později.

    `Bind` Atribut je jedním ze způsobů pro ochranu proti *typu overpost* v vytvářet scénáře. Předpokládejme například, že `Student` zahrnuje entity `Secret` vlastnost, která nechcete, aby tato webová stránka nastavení.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs?highlight=7)]

    I v případě, že nemáte `Secret` pole na webové stránce, se hacker může například použít nástroj [fiddler](http://fiddler2.com/home), nebo zápisu určitého kódu JavaScript ke zveřejnění `Secret` formuláři hodnotu. Bez [vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atribut omezení pole, která při vytváření používá vazač modelu `Student` instance<em>,</em> vazač modelu se vyzvedávat který `Secret` tvoří hodnotu a použijte ho k Vytvořte `Student` entity instance. Pak ať hacker zadaný pro hodnotu `Secret` pole formuláře by aktualizována v databázi. Následující obrázek ukazuje aplikaci fiddler přidání nástroj `Secret` pole (s hodnotou "OverPost") na hodnoty odeslaného formuláře.

    ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)  

    Hodnota "OverPost" by poté byl úspěšně přidán do `Secret` vlastnost vloženého řádku, i když nikdy určený, že webová stránka mohli nastavit tuto vlastnost.

    Je doporučeno používat `Include` parametr s `Bind` atribut *povolených* pole. Je také možné použít `Exclude` parametru *blacklist* pole, které chcete vyloučit. Z důvodu `Include` je bezpečnější je, že když přidáte novou vlastnost entity, nové pole není chráněn automaticky `Exclude` seznamu.

    Můžete zabránit overposting ve scénářích upravit, je nejprve čtení entity z databáze a pak volání `TryUpdateModel`, předejte v seznamu povolených explicitní vlastností. To je metoda použitá v těchto kurzech.

    Alternativní způsob zabránit overposting, je mnoho vývojáři preferované je použití Zobrazit modely spíše než tříd entit s vazby modelu. Zahrnout pouze vlastnosti, které chcete aktualizovat v zobrazení modelu. Po dokončení vazač modelu MVC, zkopírovat vlastnosti modelu zobrazení do instance entity, volitelně pomocí nástroje, jako třeba [AutoMapper](http://automapper.org/). Použijte databázi. Položka na svůj stav nastavit na Unchanged, a poté nastavte Property("PropertyName") instance entity. IsModified na hodnotu true v každé vlastnosti entity, která je součástí modelu zobrazení. Tato metoda funguje v obou upravovat a vytvářet scénáře.

    Jiné než `Bind` atribut, `try-catch` blok je pouze změny, které jste udělali automaticky generovaný kód. Pokud se výjimka, která je odvozena z [dataexception –](https://msdn.microsoft.com/library/system.data.dataexception.aspx) je zachycena, zatímco se ukládají změny, se zobrazí obecnou chybovou zprávu. [Dataexception –](https://msdn.microsoft.com/library/system.data.dataexception.aspx) výjimky jsou někdy způsobeny něco externí do aplikací, nikoli chybě programování, takže uživatel se doporučuje a zkuste to znovu. I když není implementována v této ukázce, produkční kvality aplikace by zaprotokolování výjimky. Další informace najdete v tématu **protokolu přehledy** kapitoly [monitorování a Telemetrie (vytváření reálných cloudových aplikací s Azure)](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log).

    Kód v *Views\Student\Create.cshtml* je podobná viděli v *Details.cshtml*kromě toho, že `EditorFor` a `ValidationMessageFor` pomocné rutiny, které se používají pro každé pole místo `DisplayFor`. Tady je relevantní kód:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cshtml)]

    *Create.chstml* také zahrnuje `@Html.AntiForgeryToken()`, který pracuje s `ValidateAntiForgeryToken` atribut v kontroleru, aby se zabránilo [padělání požadavku posílaného mezi weby](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) útoky.

    Nevyžaduje žádné změny v *Create.cshtml*.
2. Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **vytvořit nový**.
3. Zadejte názvy a neplatné datum a klikněte na tlačítko **vytvořit** zobrazíte chybovou zprávu.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)

    Toto je ověřování na straně serveru, kterou můžete získat ve výchozím nastavení; novější kurzu uvidíte, jak přidat atributy, které bude také generovat kód pro ověřování na straně klienta. Následující zvýrazněný kód ukazuje kontrolu ověření modelu v **vytvořit** metoda.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs?highlight=1)]
4. Změňte na platnou hodnotu data a klikněte na tlačítko **vytvořit** zobrazíte nové student se zobrazí v **Index** stránky.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

## <a name="update-the-edit-httppost-method"></a>Update – metoda HttpPost úpravy

V *Controllers\StudentController.cs*, `HttpGet` `Edit` – metoda (, aniž byste `HttpPost` atribut) používá `Find` metoda pro načtení vybraný `Student` entity, jako jste viděli v `Details` metoda. Nemusíte tuto metodu změnit.

Nahraďte však `HttpPost` `Edit` metoda akce s následujícím kódem:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

Tyto změny implementovat osvědčeným postupem zabezpečení zabránit [overposting](#overpost), scaffolder generované `Bind` atribut a přidat entity vytvořené vazač modelu pro sadu s příznakem změněné entit. Zda kód je už doporučená, protože `Bind` atribut vymaže všechny existující data v polích není uvedený v `Include` parametr. V budoucnu, bude scaffolder řadiče MVC aktualizovat tak, aby to není generovat `Bind` atributy pro úpravy metody.

Nový kód načte existující entity a volání [TryUpdateModel](https://msdn.microsoft.com/library/system.web.mvc.controller.tryupdatemodel(v=vs.118).aspx) aktualizace polí ze vstupu uživatele v datech odeslaného formuláře. Rozhraní Entity Framework Automatická změna sledování nastaví [změněné](https://msdn.microsoft.com/library/system.data.entitystate.aspx) příznak u entity. Když [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda je volána, `Modified` příznak způsobí, že rozhraní Entity Framework vytváření příkazů SQL k aktualizaci řádku databáze. [Konflikty souběžnosti](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) jsou ignorovány, a jsou aktualizovány všechny sloupce řádku databáze, včetně těch, které uživatel nezměnil. (Novější kurz ukazuje, jak pro zpracování konfliktů souběžnosti, a pokud chcete pouze jednotlivých polí, která mají být aktualizována v databázi, můžete nastavit entity na Unchanged a nastavit jednotlivých polí, která mají Modified.)

Jako osvědčený postup, aby se zabránilo overposting, jsou tato pole, které mají být v aktualizovatelné stránka upravit seznam povolených adres v `TryUpdateModel` parametry. Aktuálně neexistují žádná další pole, které chcete chránit, ale seznam polí, které chcete vazač modelu pro vazbu zajistí, že pokud přidáte pole do datového modelu v budoucnu, že jsou automaticky chráněné až je přidáte sem.

V důsledku těchto změn metoda podpis metoda HttpPost úprav je stejná jako metoda upravit třídy MetadataExchangeClientMode; proto jsme přejmenovat metodu EditPost.

> [!TIP] 
> 
> **Stavy entity a připojit a SaveChanges metody**
> 
> Uchovává informace o kontextu databáze, zda jsou synchronizována s jejich odpovídající řádky v databázi entity v paměti, a tyto informace Určuje, co se stane, když zavoláte `SaveChanges` metoda. Například když předat do nové entity [přidat](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metoda, která je nastavena do stavu entity `Added`. Potom při volání [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda, vydá kontext databáze SQL `INSERT` příkaz.
> 
> Entita může být v jednom z[následující stavy](https://msdn.microsoft.com/library/system.data.entitystate.aspx):
> 
> - `Added`. Entita ještě neexistuje v databázi. `SaveChanges` Metoda musíte vydat `INSERT` příkaz.
> - `Unchanged`. Nic je nutné provést k této entitě pomocí `SaveChanges` metoda. Při čtení entity z databáze, entity začíná se tento stav.
> - `Modified`. Některé nebo všechny hodnoty vlastností entity byly upraveny. `SaveChanges` Metoda musíte vydat `UPDATE` příkaz.
> - `Deleted`. Entita byla označena pro odstranění. `SaveChanges` Musíte vydat metoda `DELETE` příkaz.
> - `Detached`. Entity není sledována aplikací kontext databáze.
> 
> V aplikaci pracovní plochy změny stavu se obvykle nastavuje automaticky. V plochy typu aplikace přečtěte si entitou a změnit některé z jeho hodnot vlastností. To způsobí, že stav entity automaticky změnit tak, aby `Modified`. Potom při volání `SaveChanges`, rozhraní Entity Framework generuje SQL `UPDATE` příkaz, který aktualizuje jenom skutečné vlastnosti, které jste změnili.
> 
> Pro toto průběžné pořadí neumožňuje odpojené povaha webové aplikace. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , který čte entitu je zrušen po vykreslení stránky. Když `HttpPost` `Edit` akce metoda je volána, nové požadavku a máte novou instanci třídy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), takže budete muset ručně nastavit stav entity na `Modified.` pak při volání `SaveChanges`, rozhraní Entity Framework aktualizuje všechny sloupce řádku databáze, protože kontext nemá žádný způsob, jak zjistit vlastnosti, které jste změnili.
> 
> Pokud chcete, aby SQL `Update` příkaz k aktualizaci pouze pole, která uživatel ve skutečnosti změnit, můžete uložit původní hodnoty nějakým způsobem (například skrytá pole), aby byly k dispozici při `HttpPost` `Edit` metoda je volána. Potom můžete vytvořit `Student` entitu pomocí původní hodnoty, volání `Attach` metoda tento původní verzi entity, aktualizujte hodnoty entity na nové hodnoty a pak zavolají `SaveChanges.` Další informace najdete v tématu [ Stavy entity a SaveChanges](https://msdn.microsoft.com/data/jj592676) a [místní Data](https://msdn.microsoft.com/data/jj592872) v Centru pro vývojáře MSDN Data.


Kód HTML a Razor v *Views\Student\Edit.cshtml* je podobná viděli v *Create.cshtml*, a je nutné provést žádné změny.

Spuštění stránky tak, že vyberete **studenty** kartu a pak levým na **upravit** hypertextový odkaz.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

Některé dat a klikněte na tlačítko Změnit **Uložit**. Zobrazí změněná data na stránce indexu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-delete-page"></a>Aktualizace stránky odstranění

V *Controllers\StudentController.cs*, kód šablony pro `HttpGet` `Delete` metoda používá `Find` metoda pro načtení vybraný `Student` entity, jako jste viděli v `Details` a `Edit` metody. Ale implementovat vlastní chybová zpráva při volání `SaveChanges` selže, přidáte některé funkce pro tuto metodu a jeho odpovídající zobrazení.

Protože jste viděli pro aktualizaci a vytvoření operací, operacemi odstranění vyžadují dvě metody akce. Metoda, která je volána v odpovědi na požadavek GET zobrazí zobrazení, které umožňuje uživateli možnost schválit nebo zrušit operaci odstranění. Pokud uživatel schválí jeho, vytvoří se požadavek POST. Pokud k tomu dojde, `HttpPost` `Delete` metoda je volána, a pak dané metody ve skutečnosti provádí operaci odstranění.

Přidáte `try-catch` zablokujte `HttpPost` `Delete` metodu ke zpracování všechny chyby, které mohou nastat při aktualizaci databáze. Pokud dojde k chybě, `HttpPost` `Delete` volání metod `HttpGet` `Delete` metoda, předání parametru, která určuje, že došlo k chybě. `HttpGet Delete` Metoda pak znovu zobrazí potvrzovací stránku spolu s chybovou zprávu, umožní uživateli možnost zrušit nebo akci opakujte.

1. Nahraďte `HttpGet` `Delete` metody akce, která spravuje zasílání zpráv o chybách následujícím kódem: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cs?highlight=1,7-10)]

    Tento kód přijímá [volitelný parametr](https://msdn.microsoft.com/library/dd264739.aspx) určující, zda byla volána metoda po selhání uložte změny. Tento parametr je `false` při `HttpGet` `Delete` metoda je volána bez předchozí chybě. Když je volána metodou `HttpPost` `Delete` metoda v odpovědi na chybu aktualizace databáze, je parametr `true` a chybová zpráva je předána do zobrazení.
2. Nahraďte `HttpPost` `Delete` metoda akce (s názvem `DeleteConfirmed`) s následujícím kódem, který provede operaci skutečného odstranění a zachytí všechny chyby aktualizace databáze.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

     Tento kód načte vybrané entity, pak zavolá [odebrat](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodu a nastavit stav entity `Deleted`. Když `SaveChanges` nazývá SQL `DELETE` se vygeneruje příkaz. Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód s názvem `HttpPost` `Delete` metoda `DeleteConfirmed` umožnit `HttpPost` metoda jedinečný podpis. (Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro stejný název `HttpPost` a `HttpGet` odstranit metody.

     Pokud je zvýšení výkonu v aplikaci vysoký počet prioritu, se můžete vyhnout dotazu SQL nepotřebné načíst řádek nahrazením řádků kódu, které volají `Find` a `Remove` metody s následujícím kódem:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample14.cs)]

     Tento kód vytvoří `Student` entitu pomocí pouze hodnotu primárního klíče a poté nastaví stav entity na `Deleted`. To je všechno, která rozhraní Entity Framework potřebuje, aby odstranit entitu.

     Jak jsme uvedli, `HttpGet` `Delete` metoda neodstraní data. Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádět všechny operace upravit vytvořit operaci, nebo jiná operace, která se mění data) vytvoří bezpečnostní riziko. Další informace najdete v tématu [46 Tip # aplikace ASP.NET MVC – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
3. V *Views\Student\Delete.cshtml*, přidejte chybovou zprávu mezi `h2` záhlaví a `h3` záhlaví, jak je znázorněno v následujícím příkladu:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample15.cshtml?highlight=2)]

     Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **odstranit** hypertextový odkaz:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)
4. Klikněte na tlačítko **odstranit**. Bez odstraněné student se zobrazí stránka indexu. (Příklady kódu v akci v pro zpracování chyby najdete [souběžnosti kurzu](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md).)

## <a name="closing-database-connections"></a>Uzavření připojení databáze

Pokud chcete ukončit připojení databáze a uvolní prostředky, které drží co nejdříve, dispose instance kontextu až skončíte s ním. To znamená, proč obsahuje automaticky generovaný kód [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metoda na konci `StudentController` třídy v *StudentController.cs*, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample16.cs)]

Základní `Controller` třídy již implementuje `IDisposable` rozhraní, takže tento kód jednoduše přidá přepsání pro `Dispose(bool)` metodu dispose explicitně instance kontextu.

<a id="transactions"></a>
## <a name="handling-transactions"></a>Zpracování transakcí

Ve výchozím nastavení implementuje rozhraní Entity Framework implicitně transakce. Ve scénářích, kde více řádků nebo tabulky provést změny a pak zavolají `SaveChanges`, rozhraní Entity Framework automaticky zajišťuje, že buď všechny změny úspěšné, nebo všechny selže. Pokud jsou nejprve provést některé změny a pak se stane chyba, tyto změny jsou automaticky vrácena zpět. Scénáře, kde můžete potřebovat více řídit – například pokud chcete takové operace, provést v transakci – mimo Entity Framework najdete v tématu [práce s transakce](https://msdn.microsoft.com/data/dn456843) na webu MSDN.

## <a name="summary"></a>Souhrn

Nyní máte úplnou sadu stránek, které provádějí jednoduché operace CRUD pro `Student` entity. Pomocné rutiny MVC jste použili k vygenerování prvky uživatelského rozhraní pro datová pole. Další informace o pomocné rutiny MVC najdete v tématu [vykreslování HTML Pomocníci pomocí formuláře](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (stránka je pro MVC 3, ale je stále relevantní pro MVC 5).

V dalším kurzu rozbalíte funkce indexovou stránku přidáním řazení a stránkování.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [další](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
