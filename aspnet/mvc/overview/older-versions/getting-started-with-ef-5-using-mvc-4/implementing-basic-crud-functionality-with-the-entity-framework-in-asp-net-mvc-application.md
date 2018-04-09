---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementace funkce základní CRUD s platformou Entity Framework v aplikaci ASP.NET MVC (2 10) | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: acec5c9641b1de230956478c4396d1d541fcb0eb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementace funkce základní CRUD s platformou Entity Framework v aplikaci ASP.NET MVC (2 10)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozích kurzu jste vytvořili aplikaci MVC, která uchovává a zobrazí data pomocí rozhraní Entity Framework a SQL Server LocalDB. V tomto kurzu budete zkontrolujte a přizpůsobit CRUD (vytvořit, číst, aktualizovat, odstraňovat) kód, který generování uživatelského rozhraní MVC pro vás automaticky vytvoří v kontrolery a zobrazení.

> [!NOTE]
> Je běžnou praxí implementovat vzor úložiště před vytvořením vrstvu abstrakce mezi řadiči a vrstva přístupu k datům. Pro zjednodušení tyto kurzy, nebude implementovat úložiště až později kurzu této série.


V tomto kurzu vytvoříte následující webové stránky:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Stránce s podrobnostmi o vytváření

Automaticky generovaný kód na studentů `Index` vynecháno stránky `Enrollments` vlastnost, protože tato vlastnost obsahuje kolekci. V `Details` stránku budete zobrazení obsahu kolekce do tabulky HTML.

 V *Controllers\StudentController.cs*, metoda akce pro `Details` zobrazení používá `Find` metoda pro načtení jedné `Student` entity. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Hodnota klíče je předaný metodě jako `id` parametr a pocházejí z dat trasy v **podrobnosti** hypertextový odkaz na indexovou stránku. 

1. Open *Views\Student\Details.cshtml*. Každé pole se zobrazí, použití `DisplayFor` pomocné rutiny, jak je znázorněno v následujícím příkladu: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Po `EnrollmentDate` pole a bezprostředně před uzavírací `fieldset` značky, přidejte kód, který zobrazí seznam registrace, jak je znázorněno v následujícím příkladu:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Tento kód projde entity v `Enrollments` navigační vlastnost. Pro každou `Enrollment` entity ve vlastnosti, zobrazí název kurzu a úrovni. Název postupu načten z `Course` entity, která je uložená v `Course` vlastnost navigace `Enrollments` entity. Všechna tato data je načíst z databáze automaticky, když je to potřeba. (Jinými slovy, používáte opožděného načítání zde. Nezadali jste *přes načítání* pro `Courses` navigační vlastnost, aby při prvním pokusu o přístup k této vlastnosti, bude odeslán dotaz do databáze načíst data. Další informace o opožděného načítání a přes načítání v [čtení související Data](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série.)
3. Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **podrobnosti** odkaz pro Alexander Carsonovy. Zobrazí seznam kurzů a tříd pro vybrané student:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Stránka pro vytvoření aktualizace

1. V *Controllers\StudentController.cs*, nahraďte `HttpPost``Create` metodu akce pomocí následujícího kódu přidat `try-catch` bloku a [atribut vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) vygenerované metody: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Tento kód přidá `Student` entity vytvořené vazač modelu ASP.NET MVC do `Students` entitu nastavení a pak uloží změny do databáze. (*Vazač modelu* odkazuje na funkci ASP.NET MVC, díky, což usnadňuje práci s data odeslaná formuláře; vazač modelu převede odeslání formuláře hodnot pro typy CLR a předává je na metodu akce v parametrech. In this Case, vytvoří instanci vazače modelu `Student` entity můžete pomocí vlastnosti hodnoty z `Form` kolekce.)

    `ValidateAntiForgeryToken` Atribut pomáhá zabránit [padělání požadavku posílaného mezi weby](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) útoky.

<a id="overpost"></a>

    > [!WARNING]
    > Security - The `Bind` attribute is added to protect against *over-posting*. For example, suppose the `Student` entity includes a `Secret` property that you don't want this web page to update.
    > 
    > [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample5.cs?highlight=7)]
    > 
    > Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as [fiddler](http://fiddler2.com/home), or write some JavaScript, to post a `Secret` form value. Without the [Bind](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) attribute limiting the fields that the model binder uses when it creates a `Student` instance*,* the model binder would pick up that `Secret` form value and use it to update the `Student` entity instance. Then whatever value the hacker specified for the `Secret` form field would be updated in your database. The following image shows the fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.
    > 
    > ![](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image6.png)  
    > 
    > The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to update that property.
    > 
    > It's a security best practice to use the `Include` parameter with the `Bind` attribute to *whitelist* fields. It's also possible to use the `Exclude` parameter to *blacklist* fields you want to exclude. The reason `Include` is more secure is that when you add a new property to the entity, the new field is not automatically protected by an `Exclude` list.
    > 
    > Another alternative approach, and one preferred by many, is to use only view models with model binding. The view model contains only the properties you want to bind. Once the MVC model binder has finished, you copy the view model properties to the entity instance.

    Other than the `Bind` attribute, the `try-catch` block is the only change you've made to the scaffolded code. If an exception that derives from [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) is caught while the changes are being saved, a generic error message is displayed. [DataException](https://msdn.microsoft.com/library/system.data.dataexception.aspx) exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again. Although not implemented in this sample, a production quality application would log the exception (and non-null inner exceptions ) with a logging mechanism such as [ELMAH](https://code.google.com/p/elmah/).

    The code in *Views\Student\Create.cshtml* is similar to what you saw in *Details.cshtml*, except that `EditorFor` and `ValidationMessageFor` helpers are used for each field instead of `DisplayFor`. The following example shows the relevant code:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample6.cshtml)]

    *Create.chstml* also includes `@Html.AntiForgeryToken()`, which works with the `ValidateAntiForgeryToken` attribute in the controller to help prevent [cross-site request forgery](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) attacks.

    No changes are required in *Create.cshtml*.
2. Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **vytvořit nový**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Některé ověření dat funguje ve výchozím nastavení. Zadejte názvy a neplatné datum a klikněte na tlačítko **vytvořit** zobrazíte chybovou zprávu.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Následující zvýrazněný kód ukazuje kontrolu ověření modelu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Změňte na platnou hodnotu například 9/1/2005 na datum a klikněte na tlačítko **vytvořit** zobrazíte nové student se zobrazí v **Index** stránky.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualizace stránky POST úpravy

V *Controllers\StudentController.cs*, `HttpGet` `Edit` – metoda (, aniž byste `HttpPost` atribut) používá `Find` metoda pro načtení vybraný `Student` entity, jako jste viděli v `Details` metoda. Nemusíte tuto metodu změnit.

Však nahradit `HttpPost` `Edit` metodu akce pomocí následujícího kódu přidat `try-catch` bloku a [atribut vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Tento kód je podobná viděli v `HttpPost` `Create` metoda. Však místo přidávání entity vytvořené vazač modelu pro sadu entit, nastaví tento kód příznak u entity oznamující, že se změnil. Když [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda je volána, [změněné](https://msdn.microsoft.com/library/system.data.entitystate.aspx) příznak způsobí, že rozhraní Entity Framework vytváření příkazů SQL k aktualizaci řádku databáze. Všechny sloupce řádku databáze bude aktualizován, včetně těch, které uživatel nebyl změnit, a je v konfliktu souběžnosti jsou ignorovány. (Určuje způsob zpracování souběžnosti novější kurzu této série získáte informace.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stavy entity a připojit a SaveChanges metody

Uchovává informace o kontextu databáze, zda jsou synchronizována s jejich odpovídající řádky v databázi entity v paměti, a tyto informace Určuje, co se stane, když zavoláte `SaveChanges` metoda. Například když předat do nové entity [přidat](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metoda, která je nastavena do stavu entity `Added`. Potom při volání [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda, vydá kontext databáze SQL `INSERT` příkaz.

Entita může být v jednom z[následující stavy](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Entita ještě neexistuje v databázi. `SaveChanges` Metoda musíte vydat `INSERT` příkaz.
- `Unchanged`. Nic je nutné provést k této entitě pomocí `SaveChanges` metoda. Při čtení entity z databáze, entity začíná se tento stav.
- `Modified`. Některé nebo všechny hodnoty vlastností entity byly upraveny. `SaveChanges` Metoda musíte vydat `UPDATE` příkaz.
- `Deleted`. Entita byla označena pro odstranění. `SaveChanges` Musíte vydat metoda `DELETE` příkaz.
- `Detached`. Entity není sledována aplikací kontext databáze.

V aplikaci pracovní plochy změny stavu se obvykle nastavuje automaticky. V plochy typu aplikace přečtěte si entitou a změnit některé z jeho hodnot vlastností. To způsobí, že stav entity automaticky změnit tak, aby `Modified`. Potom při volání `SaveChanges`, rozhraní Entity Framework generuje SQL `UPDATE` příkaz, který aktualizuje jenom skutečné vlastnosti, které jste změnili.

Pro toto průběžné pořadí neumožňuje odpojené povaha webové aplikace. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , který čte entitu je zrušen po vykreslení stránky. Když `HttpPost` `Edit` akce metoda je volána, nové požadavku a máte novou instanci třídy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), takže budete muset ručně nastavit stav entity na `Modified.` pak při volání `SaveChanges`, rozhraní Entity Framework aktualizuje všechny sloupce řádku databáze, protože kontext nemá žádný způsob, jak zjistit vlastnosti, které jste změnili.

Pokud chcete, aby SQL `Update` příkaz k aktualizaci pouze pole, která uživatel ve skutečnosti změnit, můžete uložit původní hodnoty nějakým způsobem (například skrytá pole), aby byly k dispozici při `HttpPost` `Edit` metoda je volána. Potom můžete vytvořit `Student` entitu pomocí původní hodnoty, volání `Attach` metoda tento původní verzi entity, aktualizujte hodnoty entity na nové hodnoty a pak zavolají `SaveChanges.` Další informace najdete v tématu [ Stavy entity a SaveChanges](https://msdn.microsoft.com/data/jj592676) a [místní Data](https://msdn.microsoft.com/data/jj592872) v Centru pro vývojáře MSDN Data.

Kód v *Views\Student\Edit.cshtml* je podobná viděli v *Create.cshtml*, a je nutné provést žádné změny.

Spuštění stránky tak, že vyberete **studenty** kartu a pak levým na **upravit** hypertextový odkaz.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Některé dat a klikněte na tlačítko Změnit **Uložit**. Zobrazí změněná data na stránce indexu.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizace stránky odstranění

V *Controllers\StudentController.cs*, kód šablony pro `HttpGet` `Delete` metoda používá `Find` metoda pro načtení vybraný `Student` entity, jako jste viděli v `Details` a `Edit` metody. Ale implementovat vlastní chybová zpráva při volání `SaveChanges` selže, přidáte některé funkce pro tuto metodu a jeho odpovídající zobrazení.

Protože jste viděli pro aktualizaci a vytvoření operací, operacemi odstranění vyžadují dvě metody akce. Metoda, která je volána v odpovědi na požadavek GET zobrazí zobrazení, které umožňuje uživateli možnost schválit nebo zrušit operaci odstranění. Pokud uživatel schválí jeho, vytvoří se požadavek POST. Pokud k tomu dojde, `HttpPost` `Delete` metoda je volána, a pak dané metody ve skutečnosti provádí operaci odstranění.

Přidáte `try-catch` zablokujte `HttpPost` `Delete` metodu ke zpracování všechny chyby, které mohou nastat při aktualizaci databáze. Pokud dojde k chybě, `HttpPost` `Delete` volání metod `HttpGet` `Delete` metoda, předání parametru, která určuje, že došlo k chybě. `HttpGet Delete` Metoda pak znovu zobrazí potvrzovací stránku spolu s chybovou zprávu, umožní uživateli možnost zrušit nebo akci opakujte.

1. Nahraďte `HttpGet` `Delete` metody akce, která spravuje zasílání zpráv o chybách následujícím kódem: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Tento kód přijímá [volitelné](https://msdn.microsoft.com/library/dd264739.aspx) logického parametru, která určuje, zda byla volána po selhání uložte změny. Tento parametr je `false` při `HttpGet` `Delete` metoda je volána bez předchozí chybě. Když je volána metodou `HttpPost` `Delete` metoda v odpovědi na chybu aktualizace databáze, je parametr `true` a chybová zpráva je předána do zobrazení.
2. Nahraďte `HttpPost` `Delete` metoda akce (s názvem `DeleteConfirmed`) s následujícím kódem, který provede operaci skutečného odstranění a zachytí všechny chyby aktualizace databáze.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Tento kód načte vybrané entity, pak zavolá [odebrat](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodu a nastavit stav entity `Deleted`. Když `SaveChanges` nazývá SQL `DELETE` se vygeneruje příkaz. Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód s názvem `HttpPost` `Delete` metoda `DeleteConfirmed` umožnit `HttpPost` metoda jedinečný podpis. (Modulu CLR vyžaduje přetížené metody, mít parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete přilepit s konvence MVC a použít pro stejný název `HttpPost` a `HttpGet` odstranit metody.

     Pokud je zvýšení výkonu v aplikaci vysoký počet prioritu, se můžete vyhnout dotazu SQL nepotřebné načíst řádek nahrazením řádků kódu, které volají `Find` a `Remove` metody následujícím kódem, jak je znázorněno v žlutý Zvýrazněte:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Tento kód vytvoří `Student` entitu pomocí pouze hodnotu primárního klíče a poté nastaví stav entity na `Deleted`. To je všechno, která rozhraní Entity Framework potřebuje, aby odstranit entitu.

     Jak jsme uvedli, `HttpGet` `Delete` metoda neodstraní data. Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádět všechny operace upravit vytvořit operaci, nebo jiná operace, která se mění data) vytvoří bezpečnostní riziko. Další informace najdete v tématu [46 Tip # aplikace ASP.NET MVC – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
3. V *Views\Student\Delete.cshtml*, přidejte chybovou zprávu mezi `h2` záhlaví a `h3` záhlaví, jak je znázorněno v následujícím příkladu:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Spuštění stránky tak, že vyberete **studenty** kartě a kliknutím na **odstranit** hypertextový odkaz:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Klikněte na tlačítko **odstranit**. Bez odstraněné student se zobrazí stránka indexu. (Příklady kódu v akci v pro zpracování chyby najdete [zpracování souběžnosti](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) kurz později z této série.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Zajištění, že připojení databáze ponechány není otevřené

Abyste měli jistotu, že správně zavřeny připojení databáze a prostředky, které drží uvolněné nahoru, měli byste vidět k němu uvolnění má instance kontextu. To znamená, proč obsahuje automaticky generovaný kód [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metoda na konci `StudentController` třídy v *StudentController.cs*, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Základní `Controller` třídy již implementuje `IDisposable` rozhraní, takže tento kód jednoduše přidá přepsání pro `Dispose(bool)` metodu dispose explicitně instance kontextu.

## <a name="summary"></a>Souhrn

Nyní máte úplnou sadu stránek, které provádějí jednoduché operace CRUD pro `Student` entity. Pomocné rutiny MVC jste použili k vygenerování prvky uživatelského rozhraní pro datová pole. Další informace o pomocné rutiny MVC najdete v tématu [vykreslování HTML Pomocníci pomocí formuláře](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (stránka je pro MVC 3, ale je stále relevantní pro MVC 4).

V dalším kurzu rozbalíte funkce indexovou stránku přidáním řazení a stránkování.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k dat ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [další](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
