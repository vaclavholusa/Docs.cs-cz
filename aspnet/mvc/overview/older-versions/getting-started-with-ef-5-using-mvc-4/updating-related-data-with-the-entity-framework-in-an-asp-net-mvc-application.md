---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: Aktualizace souvisejících dat s Entity Framework v aplikaci ASP.NET MVC (6 10) | Dokumentace Microsoftu
author: tdykstra
description: Contoso University ukázkovou webovou aplikaci ukazuje postup vytvoření aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a sady Visual Studio...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 7871dc05-2750-470f-8b4c-3a52511949bc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: e85162f58ed9826132db8bd854914a14709f709d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37809430"
---
<a name="updating-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-6-of-10"></a>Aktualizace souvisejících dat s Entity Framework v aplikaci ASP.NET MVC (6 10)
====================
podle [Petr Dykstra](https://github.com/tdykstra)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso University ukázkovou webovou aplikaci ukazuje, jak vytvářet aplikace ASP.NET MVC 4 pomocí Entity Framework 5 Code First a Visual Studio 2012. Informace o této sérii kurzů, naleznete v tématu [z prvního kurzu této série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Série kurzů můžete začít od začátku nebo [stáhnout počáteční projekt pro tuto kapitolu](building-the-ef5-mvc4-chapter-downloads.md) a začněte tady.
> 
> > [!NOTE] 
> > 
> > Pokud narazíte na problém nevyřešíte sami, [stáhnout dokončený kapitoly](building-the-ef5-mvc4-chapter-downloads.md) a zkuste problém reprodukovat. Porovnáním kód Dokončený kód v obecně najdete řešení problému. Některé běžné chyby a jejich řešení najdete v tématu [chyby a náhradní řešení.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


V předchozím kurzu zobrazí související data; v tomto kurzu budete aktualizovat související data. U většiny relací to můžete provést aktualizace příslušné pole cizích klíčů. U relací m: n Entity Framework nezveřejňuje tabulky spojení přímo, proto musíte explicitně přidat a odebrat entity do a z odpovídající navigační vlastnosti.

Na následujících obrázcích stránky, kterou budete pracovat.

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="customize-the-create-and-edit-pages-for-courses"></a>Přizpůsobení vytvoření a úprava stránky pro kurzy

Při vytvoření nové entity kurzu musí mít relaci k existující oddělení. K provedení této obsahuje automaticky generovaný kód metody kontroleru a vytvořit a upravit zobrazení, které zahrnují rozevíracího seznamu pro výběr oddělení. Sady rozevíracího seznamu `Course.DepartmentID` vlastnost cizího klíče, a to je všechny Entity Framework, které potřebujete-li načíst `Department` navigační vlastnost s příslušnou `Department` entity. Budete používat automaticky generovaný kód, ale mírně se přidání zpracování chyb a rozevírací seznam seřadit změnit.

V *CourseController.cs*, odstraňte čtyři `Edit` a `Create` metody a nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=3,10,13-14,21-27,34,41,44-45,52-58,62-68)]

`PopulateDepartmentsDropDownList` Metoda načte seznam všech oddělení seřazené podle názvu, vytvoří `SelectList` kolekce rozevírací seznam a předá zobrazení v kolekci `ViewBag` vlastnost. Metoda přijímá volitelný `selectedDepartment` parametr, který umožňuje volajícímu kódu k určení, které se při vykreslení rozevíracího seznamu vybrat položku. Zobrazení předá název `DepartmentID` k [ `DropDownList` pomocné rutiny](../working-with-the-dropdownlist-box-and-jquery/using-the-dropdownlist-helper-with-aspnet-mvc.md), a pomocné rutiny pak mohl podívat `ViewBag` objekt pro `SelectList` s názvem `DepartmentID`.

`HttpGet` `Create` Volání metod `PopulateDepartmentsDropDownList` metody bez nastavení vybrané položky, protože pro nový kurz oddělení nedojde k ještě:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

`HttpGet` `Edit` Metoda nastaví vybrané položky na základě ID oddělení, které je již přiřazen ke kurzu, který právě upravujete:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

`HttpPost` Metody pro oba `Create` a `Edit` také obsahovat kód, který nastaví vybrané položky při jejich opětovné zobrazení na stránce po chybě:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Tento kód se zajistí, že při stránce se zobrazí znovu, chcete-li zobrazit chybová zpráva, ať oddělení byla vybrána pořád vybraný.

V *Views\Course\Create.cshtml*, přidejte zvýrazněný kód k vytvoření nové pole kurz čísla před **název** pole. Jak je popsáno v předchozí kurzu, nejsou ve výchozím nastavení automaticky pole primárního klíče, ale tento primární klíč má smysl, takže má uživatel moct zadat hodnotu klíče.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=17-23)]

V *Views\Course\Edit.cshtml*, *Views\Course\Delete.cshtml*, a *Views\Course\Details.cshtml*, přidejte pole číslo kurzu před **nadpisu**  pole. Protože je primární klíč, se zobrazí, ale nelze změnit.

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Spustit **vytvořit** stránky (Zobrazit kurz indexovou stránku a klikněte na tlačítko **vytvořit nový**) a zadejte data pro nový kurzu:

![Course_create_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Klikněte na tlačítko **vytvořit**. Kurz indexovou stránku se zobrazí nové kurzu přidat do seznamu. Název oddělení v seznamu Index stránky pochází z navigační vlastnosti zobrazující, že byla správně vytvoří vztah.

![Course_Index_page_showing_new_course](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Spustit **upravit** stránky (Zobrazit kurz indexovou stránku a klikněte na tlačítko **upravit** v kurzu).

![Course_edit_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Data na stránce a klikněte na tlačítko **Uložit**. S daty aktualizovaný kurz se zobrazí stránka kurzu indexu.

## <a name="adding-an-edit-page-for-instructors"></a>Přidání stránky pro úpravu pro vyučující

Při úpravě záznamu instruktorem, budete chtít být schopen aktualizovat přiřazení kanceláře instruktorem. `Instructor` Entita má vztah k nule nebo jednom s `OfficeAssignment` entity, což znamená, že je nutné zpracovat v následujících případech:

- Pokud uživatel vymaže přiřazení kanceláře a původně hodnotu, je třeba odebrat a odstranit `OfficeAssignment` entity.
- Pokud uživatel zadá hodnotu přiřazení office a ho původně byla prázdná, je třeba vytvořit novou `OfficeAssignment` entity.
- Pokud uživatel změní hodnotu přiřazení kanceláře, musíte změnit hodnotu v existujícím `OfficeAssignment` entity.

Otevřít *InstructorController.cs* a podívejte se na `HttpGet` `Edit` metody:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Automaticky generovaný kód není, co chcete. Nastavení dat pro rozevíracího seznamu, ale je nutné je textové pole. Tato metoda nahraďte následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Tento kód klesne `ViewBag` příkazu a přidá předběžné načítání pro přidružený `OfficeAssignment` entity. Předběžné načítání s nelze provést `Find` metoda, proto `Where` a `Single` metody se používají místo toho vybrat instruktorem.

Nahradit `HttpPost` `Edit` metodu s následujícím kódem. která zpracovává aktualizace přiřazení sady office:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Kód provede následující akce:

- Získá aktuální `Instructor` entit z databáze pomocí předběžné načítání pro `OfficeAssignment` navigační vlastnost. To je stejný jako co jste se naučili `HttpGet` `Edit` metody.
- Aktualizuje načtený `Instructor` entity s hodnotami z vazače modelu. [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.108).aspx) vám umožní použít přetížení *seznamu povolených IP adres* vlastnosti, které chcete zahrnout. To zabraňuje typu over-pass-the účtování, jak je vysvětleno v [druhé části kurzu](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md).

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]
- Pokud office umístění je prázdné, nastaví `Instructor.OfficeAssignment` vlastnost na hodnotu null tak, aby příslušného řádku v `OfficeAssignment` tabulce se odstraní.

    [!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]
- Uloží změny do databáze.

V *Views\Instructor\Edit.cshtml*, poté, co `div` prvky pro **datum přijetí** pole, přidat nové pole pro úpravy pobočce:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml)]

Spuštění stránky (vyberte **Instruktoři** kartu a potom klikněte na tlačítko **upravit** na instruktorem). Změnit **pobočce** a klikněte na tlačítko **Uložit**.

![Changing_the_office_location](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

## <a name="adding-course-assignments-to-the-instructor-edit-page"></a>Stránka upravit přidáním kurzu přiřazení kurzů vedených

Instruktoři může představuje libovolný počet kurzů. Teď budete vylepšit kurzů vedených upravit stránku tak, že přidáte změnit přiřazení kurz pomocí skupiny zaškrtávacích políček, jak je znázorněno na následujícím snímku obrazovky:

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Vztah mezi `Course` a `Instructor` entity je many-to-many, což znamená, že nemáte přímý přístup k tabulce spojení. Místo toho budou přidávat a odebírat entity do a z `Instructor.Courses` navigační vlastnost.

Rozhraní, které vám umožní měnit které kurzy instruktorem je přiřazená je skupina zaškrtávacích políček. Zaškrtávací políčko pro každý kurz v databázi se zobrazí a jsou ty, které se kurzů vedených je aktuálně přiřazená k vybrané. Uživatel může vyberte nebo zrušte zaškrtnutí políček, chcete-li změnit přiřazení kurzu. Kdyby mnohem větší počet kurzů, by pravděpodobně chtít použít jinou metodu prezentace dat v zobrazení, ale můžete využít stejnou metodu manipulace s navigační vlastnosti, aby bylo možné vytvořit nebo odstranit relace.

Pro zadání dat pro zobrazení seznamu zaškrtávacích políček, použijete třídu modelu zobrazení. Vytvoření *AssignedCourseData.cs* v *modely ViewModels* složky a nahraďte existující kód následujícím kódem:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

V *InstructorController.cs*, nahraďte `HttpGet` `Edit` metodu s následujícím kódem. Změny jsou zvýrazněné.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs?highlight=5,8,12-27)]

Kód přidá předběžné načítání pro `Courses` navigační vlastnost a volá novou `PopulateAssignedCourseData` metodu k dispozici informaci pro použití pole zaškrtávací políčko `AssignedCourseData` zobrazení třídy modelu.

Kód v `PopulateAssignedCourseData` metoda přečte všechny `Course` třída entity-li načíst seznam kurzů pomocí zobrazení modelu. Jednotlivých kurzů, kód kontroluje, zda existuje kurz v kurzů vedených `Courses` navigační vlastnost. Chcete-li vytvořit efektivní vyhledávání při kontrole, zda je přiřazen kurzů vedených kurz, kurzy přiřazená kurzů vedených jsou vloženy do [HashSet](https://msdn.microsoft.com/library/bb359438.aspx) kolekce. `Assigned` Je nastavena na `true` kurzy kurzů vedených přiřazena. Zobrazení bude tuto vlastnost použít k určení, které Kontrola polí musí být zobrazen jako vybraný. Nakonec je předán zobrazení v seznamu `ViewBag` vlastnost.

Dál přidejte kód, který se spouští, když uživatel klikne **Uložit**. Nahradit `HttpPost` `Edit` metodu s následujícím kódem, který volá novou metodu, která aktualizuje `Courses` vlastnost navigace `Instructor` entity. Změny jsou zvýrazněné.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs?highlight=3,7,20,33,37-65)]

Protože zobrazení se nemusí kolekce `Course` entity, vazače modelu se nedají automaticky aktualizovat `Courses` navigační vlastnost. Namísto použití vazače modelu k aktualizaci navigační vlastnost kurzů, můžete to udělat na novém `UpdateInstructorCourses` metody. Proto je třeba vyloučit `Courses` vlastnost z vazby modelu. To nevyžaduje změny kódu, který volá [TryUpdateModel](https://msdn.microsoft.com/library/dd470908(v=vs.98).aspx) vzhledem k tomu, že používáte *přidávání na seznam povolených* přetížení a `Courses` není v seznamu pro zahrnutí.

Pokud není žádná kontrola se zaškrtnuta, kód v `UpdateInstructorCourses` inicializuje `Courses` navigační vlastnost s prázdnou kolekci:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Kód pak projde všechny kurzy v databázi a zkontroluje každý kurz oproti těm, které jsou přiřazeny k instruktorem a ty, které byly vybrány v zobrazení. K usnadnění efektivnější vyhledávání, druhé dvě kolekce jsou uloženy v `HashSet` objekty.

Pokud zaškrtli políčko pro kurz ale kurzu není v `Instructor.Courses` navigační vlastnost, kurz je přidán do kolekce ve vlastnosti navigace.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

Pokud není zaškrtnuté políčko kurzům, ale celé probíhá `Instructor.Courses` navigační vlastnost, kurz je odebrána z navigační vlastnost.

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

V *Views\Instructor\Edit.cshtml*, přidat **kurzy** pole s polem zaškrtávacích políček tak, že přidáte následující zvýrazněný kód ihned po `div` prvky pro `OfficeAssignment` pole:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml?highlight=51-73)]

Tento kód vytvoří tabulku HTML, který obsahuje tři sloupce. V každém sloupci, představuje zaškrtávací políčko, za nímž následuje, který se skládá z kurzu počet a názvy popisků. Všechna zaškrtávací políčka mají stejný název ("selectedCourses"), která informuje o tom, které mají být považovány za skupinu vazače modelu. `value` Atribut každé zaškrtávací políčko je nastaven na hodnotu `CourseID.` při odeslání stránky vazače modelu předá kontroler, který se skládá z pole `CourseID` pouze zaškrtávací políčka, které jsou vybrané hodnoty.

Když zaškrtávací políčka jsou zpočátku vykresleno, mít ty, které jsou pro kurzy, které jsou přiřazeny kurzů vedených `checked` atributy, které vybere je (zobrazí se jim zaškrtnutí).

Po změně přiřazení kurzu, budete chtít mít možnost ověřit změny návratu na web `Index` stránky. Proto budete muset přidat sloupec do tabulky na této stránce. V tomto případě není nutné použít `ViewBag` objektu, protože je již v informace, které chcete zobrazit `Courses` vlastnost navigace `Instructor` entita, která se předá do stránky jako model.

V *Views\Instructor\Index.cshtml*, přidejte **kurzy** záhlaví hned za **Office** záhlaví, jak je znázorněno v následujícím příkladu:

[!code-html[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html?highlight=7)]

Pak přidejte novou buňku podrobností hned za buňku office umístění podrobností:

[!code-cshtml[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=19,50-57)]

Spustit **kurzů vedených Index** stránku, abyste zobrazili kurzy přiřazené jednotlivých kurzů vedených:

![Instructor_index_page](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Klikněte na tlačítko **upravit** na instruktorem zobrazíte stránky pro úpravu.

![Instructor_edit_page_with_courses](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Některá přiřazení kurzu a klikněte na tlačítko **Uložit**. Provedené změny se projeví na indexovou stránku.

 Poznámka: Přístupem upravovat data kurzu kurzů vedených funguje dobře, když je omezený počet kurzů. Pro kolekce, které jsou mnohem větší různé uživatelské rozhraní a jinou metodu aktualizace by vyžaduje.  
 

## <a name="update-the-delete-method"></a>Aktualizujte metodu Delete

Změňte kód v metoda HttpPost Delete, takže záznam přiřazení sady office (pokud existuje) se odstraní při odstranění kurzů vedených:

[!code-csharp[Main](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs?highlight=6,10)]


Pokud se pokusíte odstranit instruktorem, který je přiřazený k oddělení jako správce, získáte chybu referenční integrity. Zobrazit [aktuální verzi tohoto kurzu](../../getting-started/getting-started-with-ef-using-mvc/updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) pro další kód, který se automaticky odeberou kurzů vedených z jakékoli oddělení, ve kterém je přiřazen kurzů vedených jako správce.

## <a name="summary"></a>Souhrn

Teď jste dokončili tento úvod k práci s související data. Pokud v těchto kurzech jste provedli operace CRUD úplný rozsah, ale nebyly řešit potíže se souběžností. V dalším kurzu se zavést tématu souběžnosti, popisují možnosti pro její zpracování a přidejte souběžnosti CRUD kódu, který jste už zadali jednu entitu typu.

Odkazy na další zdroje Entity Framework najdete na konci [poslední kurz v této sérii](advanced-entity-framework-scenarios-for-an-mvc-web-application.md).

> [!div class="step-by-step"]
> [Předchozí](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [další](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
