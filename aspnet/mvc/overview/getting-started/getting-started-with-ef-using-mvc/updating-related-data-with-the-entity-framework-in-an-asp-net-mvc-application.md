---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizace dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs
author: tdykstra
description: Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2015
ms.topic: article
ms.assetid: 7ba88418-5d0a-437d-b6dc-7c3816d4ec07
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: cf4a6183e068e8668eb706d9a9e311616649e863
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Aktualizace dat souvisejících s platformou Entity Framework v aplikaci ASP.NET MVC
====================
Podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


V tomto kurzu předchozí zobrazených souvisejících dat; v tomto kurzu budete aktualizovat data v relaci. Pro většinu relace to můžete provést aktualizaci polí cizího klíče nebo navigační vlastnosti. Pro relace m: n rozhraní Entity Framework nezveřejňuje tabulku spojení přímo, tak přidávat a odebírat entity do a z odpovídající navigační vlastnosti.

Následující ilustrace znázorňuje některé stránek, které budete pracovat.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

![Upravit lektorem s kurzy](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Přizpůsobení vytvořit a upravit stránky pro kurzy

Při vytvoření nové entity kurzu, musí mít relaci s existující oddělení. K provedení této automaticky generovaný kód obsahuje metody kontroleru a vytvořit a upravit zobrazení, které zahrnují rozevíracího seznamu pro výběr z oddělení. Nastaví rozevíracího seznamu `Course.DepartmentID` vlastností cizího klíče, a to je všechno rozhraní Entity Framework potřebuje, aby zatížení `Department` navigační vlastnost s příslušnou `Department` entity. Budete používat automaticky generovaný kód, ale to změnit mírně na přidání zpracování chyb a řazení rozevíracím seznamu.

V *CourseController.cs*, odstranit čtyři `Create` a `Edit` metody a nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,11-12,19-25,40,44,46,48-78)]

Přidejte následující `using` příkaz na začátku souboru:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`PopulateDepartmentsDropDownList` Metoda získá seznam všech oddělení seřazené podle názvu, vytvoří `SelectList` kolekci rozevíracího seznamu a předává kolekce pro zobrazení v `ViewBag` vlastnost. Metodu je možné zadat nepovinný `selectedDepartment` parametr, který umožňuje volací kód, který zadejte položku, která bude vybrána při vykreslení rozevíracího seznamu. Zobrazení bude předat název `DepartmentID` k [rozevírací seznam](../../older-versions/working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md) pomocné rutiny a pomocné rutiny pak zná k prohledání `ViewBag` objekt pro `SelectList` s názvem `DepartmentID`.

`HttpGet` `Create` Volání metod `PopulateDepartmentsDropDownList` metoda bez nastavení vybrané položky, protože na nový kurz není oddělení ještě vytvořit:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpGet` `Edit` Metoda nastaví vybrané položky podle ID oddělení, které je již přiřazen ke kurzu upravovaný:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=12)]

`HttpPost` Metody pro oba `Create` a `Edit` také obsahovat kód, který nastaví vybrané položky, když se znovu zobrazit stránku po chybě:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=6)]

Tento kód zajistí, že když stránky se zobrazí znovu, chcete-li zobrazit chybová zpráva, ať oddělení nebyla vybrána zůstává vybrané.

Během zobrazení jsou již vygeneroval s rozevírací seznamy pro pole oddělení, ale nechcete, aby titulek DepartmentID u tohoto pole, zkontrolujte následující zvýrazněnou změnit na *Views\Course\Create.cshtml* do souboru Změna titulku.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml?highlight=43)]

Proveďte požadovanou změnu stejné v *Views\Course\Edit.cshtml*.

Normálně není scaffolder vygenerovat primární klíč, protože hodnota klíče je generován databáze nelze změnit a není hodnotu smysluplný, který se má zobrazit uživatelům. Pro entity kurzu scaffolder nezahrnuje textové pole pro `CourseID` pole vzhledem k tomu, že rozumí, který `DatabaseGeneratedOption.None` atribut znamená, uživatel by měl být schopný zadejte hodnotu primárního klíče. Ale nerozumí, protože číslo má smysl budete chtít najdete v ostatních zobrazeních, proto musíte přidat ručně.

V *Views\Course\Edit.cshtml*, přidání pole číslo kurzu před **název** pole. Protože je primární klíč, se zobrazí, ale nelze ho změnit.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cshtml)]

Již existuje skrytá pole (`Html.HiddenFor` pomocné rutiny) pro číslo kurzu v okně Upravit. Přidání *Html.LabelFor* pomocné rutiny není eliminují nutnost použití skryté pole, protože není způsobil kurzu číslo, které má být součástí odeslaných dat, když uživatel klikne **Uložit** na stránce Upravit.

V *Views\Course\Delete.cshtml* a *Views\Course\Details.cshtml*, změňte titulek název oddělení z "Název" na "Oddělení" a přidání pole číslo kurzu před **název**  pole.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cshtml?highlight=2,9-15)]

Spustit **vytvořit** stránky (zobrazení stránky průběhu Index a klikněte na tlačítko **vytvořit nový**) a zadejte data pro nový kurz:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Klikněte na tlačítko **vytvořit**. Zobrazí se nové během přidán do seznamu se během indexovou stránku. Název oddělení v seznamu Index stránky pochází z navigační vlastnost, zobrazující, že byla správně navázat relaci.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Spustit **upravit** stránky (zobrazení stránky průběhu Index a klikněte na tlačítko **upravit** v kurzu).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Data na stránce změny a klikněte na tlačítko **Uložit**. Během indexovou stránku se zobrazí s daty aktualizované kurzu.

## <a name="adding-an-edit-page-for-instructors"></a>Přidání stránku upravit pro vyučující

Při úpravách záznamu lektorem chcete mít možnost aktualizovat lektorem office přiřazení. `Instructor` Entita má vztah jeden pro žádná nebo jedna s `OfficeAssignment` entity, což znamená, je nutné zpracovat těchto situacích:

- Pokud uživatel zruší zaškrtnutí přiřazení office a původně hodnotu, je nutné odstranit a odstranit `OfficeAssignment` entity.
- Pokud uživatel zadá hodnotu přiřazení office a původně byl prázdný, je nutné vytvořit novou `OfficeAssignment` entity.
- Pokud uživatel změní hodnotu přiřazení office, musíte změnit hodnotu v existující `OfficeAssignment` entity.

Otevřete *InstructorController.cs* a podívejte se na `HttpGet` `Edit` metoda:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Automaticky generovaný kód není, co chcete použít. Nastavení dat pro rozevíracího seznamu, ale je nutné je textové pole. Tato metoda nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=7-10)]

Tento kód zahodí `ViewBag` příkaz a přidá přes načítání pro přidruženého `OfficeAssignment` entity. Nelze provést přes načítání s `Find` metoda, proto `Where` a `Single` metody se místo toho používají k výběru lektorem.

Nahraďte `HttpPost` `Edit` metoda následujícím kódem. který zpracovává aktualizace přiřazení office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Odkaz na `RetryLimitExceededException` vyžaduje `using` příkaz ho přidat, klikněte pravým tlačítkem na `RetryLimitExceededException`a potom klikněte na **vyřešit** - **pomocí System.Data.Entity.Infrastructure**.

![Vyřešit výjimku opakování](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Kód provede následující akce:

- Změní název metody k `EditPost` protože podpis je nyní stejná jako `HttpGet` – metoda ( `ActionName` atribut určuje, jestli se stále používá adresu URL /Edit/).
- Získá aktuální `Instructor` entity z databáze pomocí přes načítání pro `OfficeAssignment` navigační vlastnost. Je to stejné jako v `HttpGet` `Edit` metoda.
- Aktualizace načtený `Instructor` entity hodnotami z vazače modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) vám umožní použít přetížení *povolených* vlastnosti, které chcete zahrnout. To brání přečerpání účtování, jak je popsáno v [druhý kurzu](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]
- Pokud umístění kanceláře je prázdné, nastaví `Instructor.OfficeAssignment` vlastnost na hodnotu null, aby související řádek v `OfficeAssignment` tabulky se odstraní.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]
- Uloží změny do databáze.

V *Views\Instructor\Edit.cshtml*, po `div` prvky pro **datum přijetí** pole, přidat nové pole pro úpravy umístění kanceláře:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml)]

Spuštění stránky (vyberte **vyučující** a pak klikněte **upravit** na lektorem). Změna **umístění kanceláře** a klikněte na tlačítko **Uložit**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Stránka upravit přidání kurzu přiřazení lektorem

Vyučující může naučit libovolný počet kurzy. Nyní budete vylepšit stránce lektorem upravit přidáním možnost změnit přiřazení kurzu pomocí skupiny zaškrtávacích políček, jak je znázorněno na následujícím snímku obrazovky:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Vztah mezi `Course` a `Instructor` entity je m: n, což znamená, nemají přímý přístup k vlastnosti cizího klíče, které jsou v tabulce spojení. Místo toho můžete přidat a odebrat entity do a z `Instructor.Courses` navigační vlastnost.

Uživatelské rozhraní, které umožňuje změnit které kurzy lektorem je přiřazen je skupina zaškrtávacích políček. Zaškrtávací políčko pro každý kurz v databázi se zobrazí, a jsou ty, které lektorem je aktuálně přiřazen k vybrané. Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu. Pokud byly mnohem větší počet kurzy, by pravděpodobně chtít použít jinou metodu prezentace dat v zobrazení, ale by použijte stejnou metodu manipulace s navigační vlastnosti, aby bylo možné vytvořit nebo odstranit relace.

Pokud chcete data k zobrazení seznamu zaškrtávacích políček, použijete třídu modelu zobrazení. Vytvoření *AssignedCourseData.cs* v *ViewModels* složky a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

V *InstructorController.cs*, nahraďte `HttpGet` `Edit` metoda následujícím kódem. Změny se zvýrazněnou.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs?highlight=9,12,20-35)]

Kód přidá přes načítání pro `Courses` navigační vlastnost a volá novou `PopulateAssignedCourseData` metoda podat informace o použití pole zaškrtávací políčko `AssignedCourseData` zobrazit třídu modelu.

Kód `PopulateAssignedCourseData` metoda čte prostřednictvím všechny `Course` třída modelu entity, aby bylo možné načíst seznam kurzů pomocí zobrazení. Pro každý kurz kód ověří, zda během existuje v lektorem `Courses` navigační vlastnost. Chcete-li vytvořit efektivní vyhledávání při kontrole, zda je přiřazen kurzu lektorem, jsou vložena kurzy přiřazené lektorem [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) kolekce. `Assigned` Je nastavena na `true` pro kurzy lektorem přiřazen. Zobrazení bude pomocí této vlastnosti k určení, které Kontrola polí musí být zobrazí jako vybraný. Nakonec je předán zobrazení v seznamu `ViewBag` vlastnost.

Dál přidejte kód, který je spuštěn, když uživatel klikne na **Uložit**. Nahraďte `EditPost` metoda následující kód, který volá nová metoda, která aktualizuje `Courses` vlastnost navigace `Instructor` entity. Změny se zvýrazněnou.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs?highlight=3,11,25,37,40-68)]

Podpis metody se liší od teď `HttpGet` `Edit` proto název metody změní z `EditPost` zpět na `Edit`.

Vzhledem k tomu, že zobrazení nemá kolekce `Course` entity, vazač modelu nelze aktualizovat automaticky `Courses` navigační vlastnost. Místo použití vazače modelu k aktualizaci `Courses` navigační vlastnost, můžete to udělat v novém `UpdateInstructorCourses` metoda. Proto musíte vyloučit `Courses` vlastnost z vazby modelu. To nevyžaduje všechny změny kód, který volá [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) vzhledem k tomu, že používáte *povolených* přetížení a `Courses` není v seznamu zahrnout.

Pokud žádná kontrola byly zaškrtnutá políčka, kód v `UpdateInstructorCourses` inicializuje `Courses` navigační vlastnost s prázdnou kolekci:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

Kód pak projde všechny kurzy v databázi a zkontroluje každý kurzu proti těm, které jsou přiřazeny k lektorem a ty, které byly vybrány v zobrazení. Pro usnadnění efektivní hledání, jsou uloženy pozdější dvě kolekce v `HashSet` objekty.

Pokud je zaškrtnuté políčko kurzu ale během není v `Instructor.Courses` navigační vlastnost během je přidat do kolekce ve vlastnosti navigace.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

Pokud není zaškrtnuté políčko kurzu, ale probíhá během `Instructor.Courses` navigační vlastnost, během je odebrána z navigační vlastnost.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

V *Views\Instructor\Edit.cshtml*, přidat **kurzy** pole s pole zaškrtávacích políček přidáním následující kód bezprostředně po `div` prvky pro `OfficeAssignment` pole a před `div` element pro **Uložit** tlačítko:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml)]

Po vložení kódu, pokud zalomení a odsazení nevypadají jako zde udělají, opravte všechno, co ručně, takže to vypadá to, co vidíte zde. Odsazení nemusí být úplně bez chyby, ale `@</tr><tr>`, `@:<td>`, `@:</td>`, a `@</tr>` řádky musí být na jeden řádek znázorněné nebo získáte Chyba za běhu.

Tento kód vytvoří tabulky jazyka HTML, který má tři sloupce. V každém sloupci je zaškrtávací políčko, za nímž následuje popisek, který se skládá z kurzu číslo a název. Všechna zaškrtávací políčka mají stejný název ("selectedCourses"), která informuje o vazač modelu, které jsou považovány za skupinu. `value` Atribut každé zaškrtávací políčko je nastaven na hodnotu `CourseID.` při odeslání stránky vazač modelu předá pole na řadič, který se skládá z `CourseID` hodnoty pro pouze zaškrtávací políčka, které jsou vybrány.

Když zaškrtnutí políček jsou původně vykresleno, ty, které jsou pro kurzy přiřazené lektorem mají `checked` atributy, které vybere je (zobrazí se jejich zaškrtnutí).

Po změně přiřazení kurzu, budete chtít ověřit změny, když se vrátí webu `Index` stránky. Proto je nutné přidat sloupce do tabulky v této stránce. V takovém případě není nutné používat `ViewBag` objekt, protože je již v informace, které chcete zobrazit `Courses` vlastnost navigace `Instructor` entita, která jste předávání na stránku jako model.

V *Views\Instructor\Index.cshtml*, přidejte **kurzy** záhlaví hned **Office** záhlaví, jak je znázorněno v následujícím příkladu:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml?highlight=6)]

Pak přidejte nové buňky podrobností hned za buněk podrobností umístění office:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml?highlight=7-14)]

Spustit **lektorem Index** stránky zobrazíte kurzy přiřazené každý lektorem:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klikněte na tlačítko **upravit** na lektorem chcete zobrazit stránku upravit.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

Změna některých během přiřazení a klikněte na tlačítko **Uložit**. Provedené změny se projeví na indexovou stránku.

 Poznámka: Přístup zde použitý k jejich úpravě lektorem kurzu funguje dobře, pokud je omezený počet kurzy. Pro kolekce, které jsou mnohem větší by bylo zapotřebí různých uživatelského rozhraní a jinou metodu aktualizace.  
 

## <a name="update-the-deleteconfirmed-method"></a>Update – metoda DeleteConfirmed

V *InstructorController.cs*, odstraňte `DeleteConfirmed` metoda a vložte následující kód na příslušné místo.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=5-8,12-18)]

Tento kód provede následující změny:

- Pokud lektorem je přiřazen jako správce všech oddělení, odebere přiřazení lektorem oddělení. Bez tento kód by dojde k chybě referenční integrity, pokud jste se pokusili odstranit lektorem, který byl přiřazen jako správce pro oddělení.

Tento kód nemůže pracovat s scénáře jeden lektorem přiřadit jako správce pro více oddělení. V poslední kurzu přidáte kód, který zabrání této situaci nedocházelo.

## <a name="add-office-location-and-courses-to-the-create-page"></a>Přidejte na stránku vytvořit umístění kanceláře a kurzy

V *InstructorController.cs*, odstraňte `HttpGet` a `HttpPost` `Create` metody a poté přidejte následující kód v místě jejich:


[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

Tento kód je podobná co jste viděli pro úpravy metody s tím rozdílem, že jsou na začátku vybrané žádné kurzy. `HttpGet` `Create` Volání metod `PopulateAssignedCourseData` metoda není, protože může být kurzy, ale ve vybrané pořadí zajistit pro prázdnou kolekci `foreach` smyčky v zobrazení (jinak kód zobrazení by throw výjimka odkazu s hodnotou null ).

Metoda HttpPost vytvořit přidá každý vybraný kurz k navigační vlastnosti kurzy před šablony kód, který kontroluje chyby ověření a přidá nové lektorem do databáze. Kurzy jsou přidány, i když jsou chyby modelu tak, aby po chyby modelu (například uživatele s klíči na neplatné datum) tak, že když se zobrazí stránku znovu s chybovou zprávou, se automaticky obnoví kterýkoli výběr kurzu, které byly provedeny.

Všimněte si, že aby bylo možné přidat kurzy, které `Courses` navigační vlastnost, je nutné inicializovat vlastnost jako prázdnou kolekci:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

Jako alternativu k to v kódu kontroleru lze nastavit v modelu lektorem změnou metoda getter vlastnosti pro automatické vytvoření kolekce Pokud neexistuje, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample27.cs)]

Pokud změníte `Courses` vlastnost tímto způsobem můžete odstranit kód inicializace explicitní vlastnost v kontroleru.

V *Views\Instructor\Create.cshtml*, přidejte office umístění textového pole a kurzu zaškrtávací políčka po pronájem pole data a před **odeslání** tlačítko.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample28.cshtml)]

Po vložení kódu, opravte konce řádků a odsazení stejně jako dříve pro stránku upravit.

Spuštění stránky vytvořit a přidat lektorem.

![Vytvoření lektorem s kurzy](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

<a id="transactions"></a>
## <a name="handling-transactions"></a>Zpracování transakcí

Jak je popsáno v [základní funkce CRUD kurzu](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md), ve výchozím nastavení rozhraní Entity Framework implicitně implementuje transakce. Scénáře, kde můžete potřebovat více řídit – například pokud chcete takové operace, provést v transakci – mimo Entity Framework najdete v tématu [práce s transakce](https://msdn.microsoft.com/data/dn456843) na webu MSDN.

## <a name="summary"></a>Souhrn

Teď jste dokončili tento úvod k práci s související data. V těchto kurzech, pokud jste pracovali s kód, který nemá synchronní vstup-výstup. Můžete použít více efektivně používat webový server prostředky implementací asynchronní kód aplikace, a to udělat v dalším kurzu.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Předchozí](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
