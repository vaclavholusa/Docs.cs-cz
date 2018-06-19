---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizace dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC (6 10) | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 227a7fed0ced884db591f0375454d6d0a62518f5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875081"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualizace dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC (6 10)
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 4 s použitím Entity Framework 5 Code First a Visual Studio 2012. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Kurz řady můžete začít od začátku nebo [stáhnout počáteční projekt pro tato kapitola](building-the-ef5-mvc4-chapter-downloads.md) a začněte zde.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte, [stáhnout dokončené kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a pokuste se znovu provést váš problém. Řešení problému můžete získat obecně tak, že porovnáte svůj kód Dokončený kód. Pro některé běžné chyby a jak je vyřešit, najdete v části [chyby a řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V tomto kurzu předchozí zobrazených souvisejících dat; v tomto kurzu budete aktualizovat data v relaci. Pro většinu relace to můžete provést aktualizaci na odpovídající pole cizího klíče. Pro relace m: n rozhraní Entity Framework nezveřejňuje tabulku spojení přímo, takže musíte explicitně přidat a odebrat entity do a z odpovídající navigační vlastnosti.

Následující ilustrace znázorňuje stránky, že bude fungovat s.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Přizpůsobení vytvořit a upravit stránky pro kurzy

Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení. K provedení této automaticky generovaný kód obsahuje metody kontroleru a vytvořit a upravit zobrazení, které zahrnují rozevíracího seznamu pro výběr z oddělení. Nastaví rozevíracího seznamu `Course.DepartmentID` vlastností cizího klíče, a to je všechno rozhraní Entity Framework potřebuje, aby zatížení `Department` navigační vlastnost s příslušnou `Department` entity. Budete používat automaticky generovaný kód, ale to změnit mírně na přidání zpracování chyb a řazení rozevíracím seznamu.

V *CourseController.cs*, odstranit čtyři `Edit` a `Create` metody a nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Metoda získá seznam všech oddělení seřazené podle názvu, vytvoří `SelectList` kolekci rozevíracího seznamu a předává kolekce pro zobrazení v `ViewBag` vlastnost. Metodu je možné zadat nepovinný `selectedDepartment` parametr, který umožňuje volací kód, který zadejte položku, která bude vybrána při vykreslení rozevíracího seznamu. Zobrazení bude předat název `DepartmentID` k [ `DropDownList` pomocná](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), a pomocné rutiny pak zná k prohledání `ViewBag` objekt pro `SelectList` s názvem `DepartmentID`.

`HttpGet` `Create` Volání metod `PopulateDepartmentsDropDownList` metoda bez nastavení vybrané položky, protože na nový kurz není oddělení ještě vytvořit:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Metoda nastaví vybrané položky podle ID oddělení, které je již přiřazen ke kurzu upravovaný:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Metody pro oba `Create` a `Edit` také obsahovat kód, který nastaví vybrané položky, když se znovu zobrazit stránku po chybě:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Tento kód zajistí, že když stránky se zobrazí znovu, chcete-li zobrazit chybová zpráva, ať oddělení nebyla vybrána zůstává vybrané.

V *Views\Course\Create.cshtml*, přidejte zvýrazněný kód k vytvoření nové číslo pole během před **název** pole. Jak je popsáno v předchozí kurz, ve výchozím nastavení nejsou vygeneroval pole primárního klíče, ale tento primární klíč má smysl, takže má uživatel moct zadat hodnotu klíče.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

V *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, a *Views\Course\Details.cshtml*, přidání pole číslo kurzu před **název**  pole. Protože je primární klíč, se zobrazí, ale nelze ho změnit.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Spustit **vytvořit** stránky (zobrazení stránky průběhu Index a klikněte na tlačítko **vytvořit nový**) a zadejte data pro nový kurz:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Klikněte na tlačítko **vytvořit**. Zobrazí se nové během přidán do seznamu se během indexovou stránku. Název oddělení v seznamu Index stránky pochází z navigační vlastnost, zobrazující, že byla správně navázat relaci.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Spustit **upravit** stránky (zobrazení stránky průběhu Index a klikněte na tlačítko **upravit** v kurzu).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Data na stránce změny a klikněte na tlačítko **Uložit**. Během indexovou stránku se zobrazí s daty aktualizované kurzu.

## <a name="adding-an-edit-page-for-instructors"></a>Přidání stránku upravit pro vyučující

Při úpravách záznamu lektorem chcete mít možnost aktualizovat lektorem office přiřazení. `Instructor` Entita má vztah jeden pro žádná nebo jedna s `OfficeAssignment` entity, což znamená, je nutné zpracovat těchto situacích:

- Pokud uživatel zruší zaškrtnutí přiřazení office a původně hodnotu, je nutné odstranit a odstranit `OfficeAssignment` entity.
- Pokud uživatel zadá hodnotu přiřazení office a původně byl prázdný, je nutné vytvořit novou `OfficeAssignment` entity.
- Pokud uživatel změní hodnotu přiřazení office, musíte změnit hodnotu v existující `OfficeAssignment` entity.

Otevřete *InstructorController.cs* a podívejte se na `HttpGet` `Edit` metoda:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Automaticky generovaný kód není, co chcete použít. Nastavení dat pro rozevíracího seznamu, ale je nutné je textové pole. Tato metoda nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Tento kód zahodí `ViewBag` příkaz a přidá přes načítání pro přidruženého `OfficeAssignment` entity. Nelze provést přes načítání s `Find` metoda, proto `Where` a `Single` metody se místo toho používají k výběru lektorem.

Nahraďte `HttpPost` `Edit` metoda následujícím kódem. který zpracovává aktualizace přiřazení office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kód provede následující akce:

- Získá aktuální `Instructor` entity z databáze pomocí přes načítání pro `OfficeAssignment` navigační vlastnost. Je to stejné jako v `HttpGet` `Edit` metoda.
- Aktualizace načtený `Instructor` entity hodnotami z vazače modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) vám umožní použít přetížení *povolených* vlastnosti, které chcete zahrnout. To brání přečerpání účtování, jak je popsáno v [druhý kurzu](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Pokud umístění kanceláře je prázdné, nastaví `Instructor.OfficeAssignment` vlastnost na hodnotu null, aby související řádek v `OfficeAssignment` tabulky se odstraní.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Uloží změny do databáze.

V *Views\Instructor\Edit.cshtml*, po `div` prvky pro **datum přijetí** pole, přidat nové pole pro úpravy umístění kanceláře:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Spuštění stránky (vyberte **vyučující** a pak klikněte **upravit** na lektorem). Změna **umístění kanceláře** a klikněte na tlačítko **Uložit**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Stránka upravit přidání kurzu přiřazení lektorem

Vyučující může naučit libovolný počet kurzy. Nyní budete vylepšit stránce lektorem upravit přidáním možnost změnit přiřazení kurzu pomocí skupiny zaškrtávacích políček, jak je znázorněno na následujícím snímku obrazovky:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Vztah mezi `Course` a `Instructor` entity je m: n, to znamená, nemají přímý přístup k tabulce spojení. Místo toho budou přidávat a odebírat entity do a z `Instructor.Courses` navigační vlastnost.

Uživatelské rozhraní, které umožňuje změnit které kurzy lektorem je přiřazen je skupina zaškrtávacích políček. Zaškrtávací políčko pro každý kurz v databázi se zobrazí, a jsou ty, které lektorem je aktuálně přiřazen k vybrané. Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu. Pokud byly mnohem větší počet kurzy, by pravděpodobně chtít použít jinou metodu prezentace dat v zobrazení, ale by použijte stejnou metodu manipulace s navigační vlastnosti, aby bylo možné vytvořit nebo odstranit relace.

Pokud chcete data k zobrazení seznamu zaškrtávacích políček, použijete třídu modelu zobrazení. Vytvoření *AssignedCourseData.cs* v *ViewModels* složky a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

V *InstructorController.cs*, nahraďte `HttpGet` `Edit` metoda následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Kód přidá přes načítání pro `Courses` navigační vlastnost a volá novou `PopulateAssignedCourseData` metoda podat informace o použití pole zaškrtávací políčko `AssignedCourseData` zobrazit třídu modelu.

Kód `PopulateAssignedCourseData` metoda čte prostřednictvím všechny `Course` třída modelu entity, aby bylo možné načíst seznam kurzů pomocí zobrazení. Pro každý kurz kód ověří, zda během existuje v lektorem `Courses` navigační vlastnost. Chcete-li vytvořit efektivní vyhledávání při kontrole, zda je přiřazen kurzu lektorem, jsou vložena kurzy přiřazené lektorem [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) kolekce. `Assigned` Je nastavena na `true` pro kurzy lektorem přiřazen. Zobrazení bude pomocí této vlastnosti k určení, které Kontrola polí musí být zobrazí jako vybraný. Nakonec je předán zobrazení v seznamu `ViewBag` vlastnost.

Dál přidejte kód, který je spuštěn, když uživatel klikne na **Uložit**. Nahraďte `HttpPost` `Edit` metoda následující kód, který volá nová metoda, která aktualizuje `Courses` vlastnost navigace `Instructor` entity. Změny se zvýrazněnou.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Vzhledem k tomu, že zobrazení nemá kolekce `Course` entity, vazač modelu nelze aktualizovat automaticky `Courses` navigační vlastnost. Místo použití vazače modelu k aktualizaci navigační vlastnost kurzy, můžete to udělat v novém `UpdateInstructorCourses` metoda. Proto musíte vyloučit `Courses` vlastnost z vazby modelu. To nevyžaduje všechny změny kód, který volá [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) vzhledem k tomu, že používáte *povolených* přetížení a `Courses` není v seznamu zahrnout.

Pokud žádná kontrola byly zaškrtnutá políčka, kód v `UpdateInstructorCourses` inicializuje `Courses` navigační vlastnost s prázdnou kolekci:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kód pak projde všechny kurzy v databázi a zkontroluje každý kurzu proti těm, které jsou přiřazeny k lektorem a ty, které byly vybrány v zobrazení. Pro usnadnění efektivní hledání, jsou uloženy pozdější dvě kolekce v `HashSet` objekty.

Pokud je zaškrtnuté políčko kurzu ale během není v `Instructor.Courses` navigační vlastnost během je přidat do kolekce ve vlastnosti navigace.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Pokud není zaškrtnuté políčko kurzu, ale probíhá během `Instructor.Courses` navigační vlastnost, během je odebrána z navigační vlastnost.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

V *Views\Instructor\Edit.cshtml*, přidejte **kurzy** pole s pole zaškrtávacích políček přidáním následující zvýrazněnou code ihned po `div` prvky pro `OfficeAssignment` pole:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Tento kód vytvoří tabulky jazyka HTML, který má tři sloupce. V každém sloupci je zaškrtávací políčko, za nímž následuje popisek, který se skládá z kurzu číslo a název. Všechna zaškrtávací políčka mají stejný název ("selectedCourses"), která informuje o vazač modelu, které jsou považovány za skupinu. `value` Atribut každé zaškrtávací políčko je nastaven na hodnotu `CourseID.` při odeslání stránky vazač modelu předá pole na řadič, který se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.

Když zaškrtnutí políček jsou původně vykresleno, ty, které jsou pro kurzy přiřazené lektorem mají `checked` atributy, které vybere je (zobrazí se jejich zaškrtnutí).

Po změně přiřazení kurzu, budete chtít ověřit změny, když se vrátí webu `Index` stránky. Proto je nutné přidat sloupce do tabulky v této stránce. V takovém případě není nutné používat `ViewBag` objekt, protože je již v informace, které chcete zobrazit `Courses` vlastnost navigace `Instructor` entita, která jste předávání na stránku jako model.

V *Views\Instructor\Index.cshtml*, přidejte **kurzy** záhlaví hned **Office** záhlaví, jak je znázorněno v následujícím příkladu:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Pak přidejte nové buňky podrobností hned za buněk podrobností umístění office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Spustit **lektorem Index** stránky zobrazíte kurzy přiřazené každý lektorem:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klikněte na tlačítko **upravit** na lektorem chcete zobrazit stránku upravit.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Změna některých během přiřazení a klikněte na tlačítko **Uložit**. Provedené změny se projeví na indexovou stránku.

 Poznámka: Přístup použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy. Pro kolekce, které jsou mnohem větší by bylo zapotřebí různých uživatelského rozhraní a jinou metodu aktualizace.  
 

## <a name="update-the-delete-method"></a>Aktualizujte metodu Delete

Změňte kód v metodě HttpPost odstranění tak záznam přiřazení office (pokud existuje) se odstraní při odstranění lektorem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Pokud se pokusíte odstranit lektorem, který je přiřazený k oddělení jako správce, získáte chyba referenční integrity. V tématu [aktuální verzi v tomto kurzu](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) pro další kód, který lektorem se automaticky odeberou všechny oddělení, kde lektorem je přiřazen jako správce.

## <a name="summary"></a>Souhrn

Teď jste dokončili tento úvod k práci s související data. Pokud v těchto kurzech jste provádí operace CRUD úplné rozsah, ale nebyly řešit potíže se souběžností. V dalším kurzu bude zavádět tématu souběžnosti, popisují možnosti pro její zpracování a přidejte souběžnosti zpracování CRUD kódu, které jste již vytvořili pro typ jednu entitu.

Odkazy na další zdroje Entity Framework najdete na konci [poslední kurzu této série](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Předchozí](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
