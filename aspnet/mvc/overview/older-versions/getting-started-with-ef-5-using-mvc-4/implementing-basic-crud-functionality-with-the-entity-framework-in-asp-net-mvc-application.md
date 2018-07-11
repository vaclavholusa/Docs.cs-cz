---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
title: Implementace základních funkcí CRUD s rozhraním Entity Framework v aplikaci ASP.NET MVC (2 z 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: f7bace3f-b85a-47ff-b5fe-49e81441cdf9
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f580ba6d0db07430e991fba2d061bb2632c95353
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817080"
---
<a name="implementing-basic-crud-functionality-with-the-entity-framework-in-aspnet-mvc-application-2-of-10"></a>Implementace základních funkcí CRUD s rozhraním Entity Framework v aplikaci ASP.NET MVC (2 z 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu jste vytvořili aplikaci MVC, která ukládá a zobrazuje data pomocí rozhraní Entity Framework a SQL Server LocalDB. V tomto kurzu zkontrolujete a přizpůsobit CRUD (vytváření, čtení, aktualizace nebo odstranění) kód, který generování uživatelského rozhraní MVC se automaticky vytvoří za vás do kontrolerů a zobrazení.

> [!NOTE]
> Je běžnou praxí pro implementaci vzoru úložiště chcete-li vytvořit vrstvu HAL mezi řadiči a vrstva přístupu k datům. Pro zjednodušení tyto kurzy, nebude implementovat úložiště až do novější kurzu této série.


V tomto kurzu vytvoříte následujících webových stránek:

![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image1.png)

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image2.png)

![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image3.png)

![Student_delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image4.png)

## <a name="creating-a-details-page"></a>Vytváří se stránka s podrobnostmi

Automaticky generovaný kód pro studenty `Index` vynechali stránky `Enrollments` vlastnost, protože tato vlastnost obsahuje kolekci. V `Details` obsah kolekce bude zobrazen jako tabulku HTML stránky.

 V *Controllers\StudentController.cs*, metoda akce pro `Details` zobrazení používá `Find` metodu pro načtení jedné `Student` entity. 

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample1.cs)]

 Hodnota klíče, která se předá metodě jako `id` parametr a pochází z dat trasy v **podrobnosti** hypertextový odkaz na indexovou stránku. 

1. Otevřít *Views\Student\Details.cshtml*. Každé pole je zobrazeno pomocí `DisplayFor` pomocné rutiny, jak je znázorněno v následujícím příkladu: 

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample2.cshtml)]
2. Po `EnrollmentDate` pole a bezprostředně před uzavírací `fieldset` značky, přidejte kód pro zobrazení seznamu registrací, jak je znázorněno v následujícím příkladu:

    [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample3.cshtml?highlight=4-22)]

    Tento kód projde entity v `Enrollments` navigační vlastnost. Pro každou `Enrollment` entity ve vlastnosti, zobrazí název kurzu a na podnikové úrovni. Název kurzu je načten z `Course` entita, která je uložena v `Course` vlastnost navigace `Enrollments` entity. Všechna tato data se načtou z databáze automaticky když ho nepotřebují. (Jinými slovy, že používáte opožděné načtení tady. Nezadali jste *předběžné načítání* pro `Courses` vlastnost navigace, takže při prvním pokusu o přístup k této vlastnosti, bude odeslán dotaz do databáze načíst data. Další informace o opožděné načtení a nemůžou dočkat, až načítání v [čtení souvisejících dat](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) později v této sérii kurzů.)
3. Spuštění stránky tak, že vyberete **studenty** kartu a kliknutím **podrobnosti** odkaz Alexander Carsonovy. Zobrazí seznam kurzů a známek pro vybrané studenta:

    ![Student_Details_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image5.png)

## <a name="updating-the-create-page"></a>Aktualizuje se stránka pro vytvoření

1. V *Controllers\StudentController.cs*, nahraďte `HttpPost``Create` metodu akce pomocí následujícího kódu přidejte `try-catch` bloku a [atribut vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) vygenerované metody: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample4.cs?highlight=4,7-8,14-20)]

    Tento kód přidá `Student` entita vytvořená pomocí vazače modelu ASP.NET MVC do `Students` entity nastavení a pak uloží změny do databáze. (*Modelu pořadače* odkazuje na funkci ASP.NET MVC, která provádí, což usnadňuje práci s data odeslaná formuláře; vazač modelu převádí publikování formuláře hodnoty pro typy CLR a předává je na metodu akce v parametrech. In this Case, vytvoří instanci vazače modelu `Student` entity pomocí vlastnosti hodnoty z `Form` kolekce.)

    `ValidateAntiForgeryToken` Atribut pomáhá zabránit [padělání žádosti více webů](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) útoky.

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
2. Spuštění stránky tak, že vyberete **studenty** kartu a kliknutím na **vytvořit nový**.

    ![Student_Create_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image7.png)

    Některé ověření dat funguje ve výchozím nastavení. Zadejte názvy a neplatné datum a klikněte na tlačítko **vytvořit** zobrazíte chybovou zprávu.

    ![Students_Create_page_error_message](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image8.png)

    Následující zvýrazněný kód ukazuje ověření modelu.

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample7.cs?highlight=5)]

    Změňte na platnou hodnotu například 9/1/2005 datum a klikněte na tlačítko **vytvořit** zobrazíte nového objektu student joinkind **Index** stránky.

    ![Students_Index_page_with_new_student](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image9.png)

## <a name="updating-the-edit-post-page"></a>Aktualizace stránky pro úpravu příspěvku

V *Controllers\StudentController.cs*, `HttpGet` `Edit` – metoda (jeden bez `HttpPost` atribut) používá `Find` metodu pro načtení vybraných `Student` entity, jako jste viděli v `Details` metody. Nemusíte změnit tuto metodu.

Ale nahradit `HttpPost` `Edit` metodu akce pomocí následujícího kódu přidejte `try-catch` bloku a [atribut vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx):

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample8.cs)]

Tento kód se podobá viděli v `HttpPost` `Create` metody. Však místo přidání entity vytvořené vazač modelu pro sadu entit, tento kód nastaví příznak, který u entity oznamující, že se změnil. Když [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda je volána, [změněné](https://msdn.microsoft.com/library/system.data.entitystate.aspx) příznak způsobí, že rozhraní Entity Framework pro vytváření příkazů SQL aktualizujte řádek databáze. Všechny sloupce v řádku databáze se aktualizuje, včetně těch, které uživatel nezměnil, a konfliktů souběžnosti se ignorují. (Dozvíte jak zpracovat souběžnosti v pozdějších kurzech v této sérii.)

### <a name="entity-states-and-the-attach-and-savechanges-methods"></a>Stavy entity a připojit a SaveChanges metody

Uchovává informace o kontextu databáze, jestli jsou synchronizované s jejich odpovídajících řádků v databázi entity v paměti a těchto informací Určuje, co se stane při volání `SaveChanges` metody. Například při předání do nové entity [přidat](https://msdn.microsoft.com/library/system.data.entity.dbset.add(v=vs.103).aspx) metoda, která stav entity je nastavený na `Added`. Potom při volání [SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) metoda, vydá kontext databáze SQL `INSERT` příkazu.

Entity mohou být v jednom z[následující stavy](https://msdn.microsoft.com/library/system.data.entitystate.aspx):

- `Added`. Entita ještě neexistuje v databázi. `SaveChanges` Metody musíte vydat `INSERT` příkazu.
- `Unchanged`. Co je potřeba udělat s touto entitou podle `SaveChanges` metody. Při čtení entity z databáze entity je na začátku s tímto stavem.
- `Modified`. Některé nebo všechny hodnoty vlastností entity byly změněny. `SaveChanges` Metody musíte vydat `UPDATE` příkazu.
- `Deleted`. Entita byla označena k odstranění. `SaveChanges` Metody musíte vydat `DELETE` příkazu.
- `Detached`. Entita není sledován správou kontext databáze.

V desktopové aplikaci změny stavu se obvykle nastaví automaticky. V typu desktopové aplikace čtení entity a provést změny na některé z jeho hodnot vlastností. To způsobí, že jeho stav entity automaticky změnil na `Modified`. Potom při volání `SaveChanges`, Entity Framework generuje SQL `UPDATE` příkaz, který aktualizuje pouze skutečné vlastnosti, které jste změnili.

Toto pořadí průběžné neumožňuje odpojené povaha služby web apps. [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) , která načte entity je uvolněn po vykreslení stránky. Když `HttpPost` `Edit` volání metody, vytvoří se nový požadavek a mít novou instanci třídy [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx), takže je nutné ručně nastavit stavu entity `Modified.` pak při volání `SaveChanges`, Entity Framework aktualizuje všechny sloupce řádek databáze, protože kontext nemá žádný způsob, jak zjistit vlastnosti, které jste změnili.

Pokud chcete, aby SQL `Update` . doje k aktualizaci pouze pole, která uživatel ve skutečnosti změní, můžete uložit původní hodnoty nějakým způsobem (třeba skrytá pole) tak, aby byly k dispozici při `HttpPost` `Edit` metoda je volána. Můžete vytvořit `Student` entity pomocí původní hodnoty, volání `Attach` metodu s původní verzi entitou, aktualizujte hodnoty entity na nové hodnoty a následně zavolat `SaveChanges.` Další informace najdete v tématu [ Stavy entity a SaveChanges](https://msdn.microsoft.com/data/jj592676) a [místních dat](https://msdn.microsoft.com/data/jj592872) v datovém centru pro vývojáře MSDN.

Kód v *Views\Student\Edit.cshtml* je podobný viděli v *Create.cshtml*, a nejsou potřeba žádné změny.

Spuštění stránky tak, že vyberete **studenty** kartu a pak levým na **upravit** hypertextový odkaz.

![Student_Edit_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image10.png)

Některá data a klikněte na tlačítko Změnit **Uložit**. Zobrazí změněných dat v indexovou stránku.

![Students_Index_page_after_edit](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualizace stránky Delete

V *Controllers\StudentController.cs*, kód šablony pro `HttpGet` `Delete` metoda používá `Find` metodu pro načtení vybraných `Student` entity, jako jste viděli v `Details` a `Edit` metody. Však implementovat vlastní chybová zpráva při volání `SaveChanges` selže, přidáte některé funkce pro tuto metodu a jeho odpovídající zobrazení.

Už znáte aktualizace a operace vytvoření, odstranění operace vyžadují dvě metody akce. Metoda, která je volána v reakci na požadavek GET zobrazí zobrazení, které umožňuje uživateli možnost schválit nebo zrušit operaci odstranění zopakovat. Pokud uživatel ho buď schválí, vytvoří se požadavek POST. Pokud k tomu dojde, `HttpPost` `Delete` volání metody a pak tato metoda ve skutečnosti provádí operaci odstranění zopakovat.

Přidáte `try-catch` bloku `HttpPost` `Delete` metodu ke zpracování všechny chyby, které mohou nastat při aktualizaci databáze. Pokud dojde k chybě `HttpPost` `Delete` volání metod `HttpGet` `Delete` metodu předáním parametru, která označuje, že došlo k chybě. `HttpGet Delete` Metoda pak znovu zobrazí na stránce potvrzení spolu s chybovou zprávu, poskytnou tak uživateli možnost zrušit, nebo to zkuste znovu.

1. Nahradit `HttpGet` `Delete` metody akce, která spravuje zpráv o chybách následujícím kódem: 

    [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample9.cs)]

    Tento kód přijímá [volitelné](https://msdn.microsoft.com/library/dd264739.aspx) logický parametr, který označuje, zda byla volána po selhání, uložte změny. Tento parametr je `false` při `HttpGet` `Delete` metoda je volána bez předchozí chyby. Pokud je volán `HttpPost` `Delete` je parametr metody v reakci na chybě aktualizace databáze `true` a chybová zpráva je předána do zobrazení.
2. Nahradit `HttpPost` `Delete` metody akce (s názvem `DeleteConfirmed`) následujícím kódem, který provádí operace odstranění skutečné a zachytí všechny chyby aktualizace databáze.

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample10.cs)]

     Tento kód načte vybranou entitu, zavolá [odebrat](https://msdn.microsoft.com/library/system.data.entity.dbset.remove(v=vs.103).aspx) metodu pro nastavení stavu entity `Deleted`. Když `SaveChanges` nazývá SQL `DELETE` příkaz je generován. Také jste změnili název metody akce z `DeleteConfirmed` k `Delete`. Automaticky generovaný kód s názvem `HttpPost` `Delete` metoda `DeleteConfirmed` poskytnout `HttpPost` metoda jedinečnou signaturu. (CLR vyžaduje přetížené metody s parametry jinou metodu.) Teď, když podpisy jsou jedinečné, můžete zůstat u konvence MVC a použijte stejný název pro `HttpPost` a `HttpGet` metody pro odstranění.

     Pokud je vylepšení výkonu v aplikaci s velkým objemem prioritu, se můžete vyhnout zbytečným jazyka SQL k načtení na řádku tak, že nahradíte řádků kódu, které volají `Find` a `Remove` metody následujícím kódem, jak je znázorněno v žlutá Zvýrazněte:

     [!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample11.cs)]

     Tento kód vytvoří instanci `Student` entity pomocí pouze hodnotu primárního klíče a pak nastaví stav entity `Deleted`. To je vše, Entity Framework potřebuje, aby odstranit entitu.

     Jak je uvedeno, `HttpGet` `Delete` metoda neodstraní data. Operace delete v reakci na příkaz GET žádosti (nebo pro tento účel, provedení libovolné operace úprav vytvořte operace nebo jiné operace, která se mění data) vytvoří ohrožení zabezpečení. Další informace najdete v tématu [46 Tip # aplikace ASP.NET MVC – nepoužívejte odstranit odkazy, protože uživatel vytvořit bezpečnostní díry](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx) na blogu Stephen Walther.
3. V *Views\Student\Delete.cshtml*, přidejte mezi chybová zpráva `h2` záhlaví a `h3` záhlaví, jak je znázorněno v následujícím příkladu:

     [!code-cshtml[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample12.cshtml?highlight=2)]

     Spuštění stránky tak, že vyberete **studenty** kartu a kliknutím **odstranit** hypertextový odkaz:

     ![Student_Delete_page](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/_static/image12.png)
4. Klikněte na tlačítko **odstranit**. Zobrazí se indexovou stránku bez odstraněné studentů. (Zobrazí se vám příklad kód v akci pro zpracování chyb [zpracování souběžnosti](../../getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md) později v této sérii kurzů.)

## <a name="ensuring-that-database-connections-are-not-left-open"></a>Zajištění, že připojení k databázi není otevřená

Abyste měli jistotu, že jsou správně uzavřeny připojení k databázi a prostředky, které drží uvolněné nahoru, byste měli vidět na to, že tato instance kontextu je uvolněn. To znamená, proč obsahuje automaticky generovaný kód [Dispose](https://msdn.microsoft.com/library/system.idisposable.dispose(v=vs.110).aspx) metoda na konci `StudentController` třídy v *StudentController.cs*, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application/samples/sample13.cs)]

Základní `Controller` implementuje třída již `IDisposable` rozhraní, takže tento kód jednoduše přidá přepsání `Dispose(bool)` metoda explicitně uvolnit instance kontextu.

## <a name="summary"></a>Souhrn

Teď máte úplnou sadu stránek, které provádějí jednoduché operace CRUD pro `Student` entity. Pomocné rutiny MVC jste použili k vygenerování prvky uživatelského rozhraní pro datová pole. Další informace o pomocné rutiny MVC najdete v tématu [vykreslení formuláři pomocí pomocných rutin HTML](https://msdn.microsoft.com/library/dd410596(v=VS.98).aspx) (stránka je pro MVC 3, ale je stále relevantní pro MVC 4).

V dalším kurzu rozšiřujete funkce indexovou stránku tak, že přidáte řazení a stránkování.

Odkazy na další zdroje Entity Framework najdete v [mapa obsahu přístupu k Data technologie ASP.NET](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)
> [další](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
