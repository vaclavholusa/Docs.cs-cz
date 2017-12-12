---
title: "Nahrávání souborů na stránku Razor v ASP.NET Core"
author: guardrex
description: "Zjistěte, jak k nahrání souborů do stránky Razor."
keywords: "Jádro ASP.NET Razor, stránky Razor, IFormFile, nahrávání souborů, odesílání souborů při odpovědích"
ms.author: riande
manager: wpickett
ms.date: 09/12/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/uploading-files
ms.openlocfilehash: 3b54bf0b40c396c8c141966219f65231fb362ca4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="uploading-files-to-a-razor-page-in-aspnet-core"></a>Nahrávání souborů na stránku Razor v ASP.NET Core

Podle [Luke Latham](https://github.com/guardrex)

V této části je znázorněn nahrávání souborů stránky Razor.

[Film stránky Razor ukázkovou aplikaci](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) v tento kurz používá jednoduchý model vazby k nahrání souborů, které funguje dobře pro nahrávání souborů malé. Informace o streamování velkých souborů, v tématu [nahrávání velkých souborů s streamování](xref:mvc/models/file-uploads#uploading-large-files-with-streaming).

V následujících krocích přidáte funkci nahrávání souboru plán film ukázkovou aplikaci. Je reprezentována plán film `Schedule` třídy. Třída zahrnuje dvě verze plánu. Jedna verze je poskytováno zákazníkům, `PublicSchedule`. Jiné verze se má použít pro zaměstnance společnosti `PrivateSchedule`. Každá verze je uloženo jako samostatný soubor. Tento kurz ukazuje, jak provést dvě nahrávání souborů ze stránky s jediným POŠTOVNÍM k serveru.

## <a name="add-a-fileupload-class"></a>Přidání třídy odesílání souborů při odpovědích

Níže vytvoříte stránky Razor pro zpracování pár nahrávání souborů. Přidat `FileUpload` třídy, která je vázána na stránku získat data plánu. Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **odesílání souborů při odpovědích** a přidejte následující vlastnosti:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/FileUpload.cs)]

Třída nemá vlastnost pro nadpis podle plánu a vlastnost pro každou z dvou verzích plán. Všechny tři vlastnosti jsou požadovány a název musí mít 3 až 60 znaků.

## <a name="add-a-helper-method-to-upload-files"></a>Přidejte pomocnou metodu k nahrání souborů

Nechcete duplicity kód pro zpracování souborů nahrané plán, přidejte nejprve statickou pomocnou metodu. Vytvoření *nástroje* složky v aplikaci a přidejte *FileHelpers.cs* soubor s následujícím obsahem. Pomocná metoda `ProcessFormFile`, trvá [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile) a [ModelStateDictionary](/api/microsoft.aspnetcore.mvc.modelbinding.modelstatedictionary) a vrátí řetězec obsahující velikost souboru a jeho obsah. Se kontroluje, typu obsahu a délka. Pokud soubor neprojde ověřování pravosti, chyba je přidán do `ModelState`.

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Utilities/FileHelpers.cs)]

## <a name="add-the-schedule-class"></a>Přidání třídy plán

Klikněte pravým tlačítkem *modely* složky. Vyberte **přidat** > **třída**. Název třídy **plán** a přidejte následující vlastnosti:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/Schedule.cs)]

Používá třída `Display` a `DisplayFormat` atributy, které vytvářejí popisné názvy a formátování při vykreslení data plánu.

## <a name="update-the-moviecontext"></a>Aktualizace MovieContext

Zadejte `DbSet` v `MovieContext` (*Models/MovieContext.cs*) pro plány:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieContext.cs?highlight=13)]

## <a name="add-the-schedule-table-to-the-database"></a>Přidat do databáze v tabulce plán

Otevřete konzolu Správce balíčků (pomocí PMC): **nástroje** > **Správce balíčků NuGet** > **Konzola správce balíčků**.

![Pomocí PMC nabídky](../first-mvc-app/adding-model/_static/pmc.png)

V pomocí PMC spuštěním následujících příkazů. Přidat tyto příkazy `Schedule` tabulky k databázi:

```powershell
Add-Migration AddScheduleTable
Update-Database
```

## <a name="add-a-file-upload-razor-page"></a>Přidat nahrání souboru stránky Razor

V *stránky* složky, vytvoření *plány* složky. V *plány* složku vytvořit stránku s názvem *Index.cshtml* pro nahrávání plán s následujícím obsahem:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml)]

Každá skupina formuláře zahrnuje  **\<popisek >** který zobrazí název každé vlastnosti třídy. `Display` Atributy v `FileUpload` model poskytují zobrazovaných hodnot pro popisky. Například `UploadPublicSchedule` zobrazovaný název vlastnosti je nastaven s `[Display(Name="Public Schedule")]` a proto zobrazí "Veřejné plán" v popisku, když se vykreslí formulář.

Každá skupina formuláře zahrnuje ověřování  **\<span >**. Uživatelský vstup nesplňuje-li vlastnost atributy nastavit v `FileUpload` třídy nebo pokud platí jedna z `ProcessFormFile` metoda souboru ověřování selže, model se nepodaří ověřit. Pokud selže ověření modelu, je generován zprávu užitečné ověření uživatele. Například `Title` vlastnost je opatřen poznámkou `[Required]` a `[StringLength(60, MinimumLength = 3)]`. Pokud se uživateli nepodaří zadat název, obdrží zprávu s upozorněním, že je vyžadována hodnota. Pokud uživatel zadá hodnotu menší než 3 znaky nebo víc než 60 znaků, obdrží zprávu s upozorněním, že hodnota má nesprávnou délku. Soubor je zadaný, který nemá žádný obsah, zobrazí se zpráva označující, že soubor je prázdný.

## <a name="add-the-code-behind-file"></a>Přidejte soubor kódu

Přidejte soubor kódu (*Index.cshtml.cs*) k *plány* složky:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs)]

Model stránky (`IndexModel` v *Index.cshtml.cs*) váže `FileUpload` třídy:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet1)]

Model také používá seznam plány (`IList<Schedule>`) k zobrazení plány, které jsou uloženy v databázi na stránce:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet2)]

Při načtení stránky s `OnGetAsync`, `Schedules` je naplněny z databáze a používat ke generování tabulky HTML z načíst plánů:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet3)]

Při odeslání formuláře na server, `ModelState` je zaškrtnuté. Pokud je neplatná, `Schedule` znovu sestaven, a vykreslí stránku s jeden nebo více ověřovacích zpráv s informacemi o tom, proč stránky ověření se nezdařilo. Pokud je platná, `FileUpload` vlastnosti se používají v *OnPostAsync* k dokončení nahrávání souborů pro dvě verze plánu a vytvořit nový `Schedule` objekt, který chcete uložit data. Plán pak je uložena do databáze:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Index.cshtml.cs?name=snippet4)]

## <a name="link-the-file-upload-razor-page"></a>Odkaz nahrávání souborů stránky Razor

Otevřete *_Layout.cshtml* a přidat odkaz na navigačním panelu k dosažení stránka nahrávání souboru:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/_Layout.cshtml?range=31-38&highlight=4)]

## <a name="add-a-page-to-confirm-schedule-deletion"></a>Přidat stránku potvrďte odstranění plánu

Když uživatel klikne na Odstranit plán, kterým chcete šance, že na tlačítko Storno. Přidat stránku potvrzení odstranění (*Delete.cshtml*) k *plány* složky:

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml)]

Souboru kódu na pozadí (*Delete.cshtml.cs*) načte jeden plán identifikovaný `id` v datech trasy žádosti. Přidat *Delete.cshtml.cs* do souboru *plány* složky:

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs)]

`OnPostAsync` Metoda zpracovává odstranění plán, podle jeho `id`:

[!code-csharp[Main](razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Schedules/Delete.cshtml.cs?name=snippet1&highlight=8,12-13)]

Po úspěšném odstranění plán, `RedirectToPage` odešle uživateli zpět na plány *Index.cshtml* stránky.

## <a name="the-working-schedules-razor-page"></a>Funkční stránky Razor plány

Při načtení stránky, popisky a vstupy pro plán název, plán veřejné a privátní plán se vykreslují tlačítka Odeslat:

![Plány stránky Razor, jak je vidět na počáteční zatížení bez chyby ověření a prázdné pole](uploading-files/_static/browser1.png)

Výběr **nahrát** tlačítko bez naplnění žádná pole je v rozporu `[Required]` atributů v modelu. `ModelState` Je neplatný. Uživateli se zobrazí chybové zprávy ověření:

![Ověření chybové zprávy zobrazují vedle každého vstupního ovládacího prvku](uploading-files/_static/browser2.png)

Zadejte dvě písmena do **název** pole. Ověření změny zpráva indikující, že název musí být 3 až 60 znaků:

![Změnit název ověřovací zprávy](uploading-files/_static/browser3.png)

Pokud jeden nebo více plánů se odešlou, **načíst plány** načíst plány vykreslí část:

![Tabulka načíst plány, zobrazuje název každý plán nahrán datum UTC, veřejné verze velikost souboru a velikost souboru privátní verze](uploading-files/_static/browser4.png)

Můžete kliknout na uživatele **odstranit** odkaz z ní k dosažení zobrazení potvrzení odstranění, které mají příležitost potvrdit nebo zrušit operaci odstranění.

## <a name="troubleshooting"></a>Poradce při potížích

Pro řešení potíží s informací o s `IFormFile` nahrát, najdete v článku [nahrávání souborů v ASP.NET Core: řešení potíží s](xref:mvc/models/file-uploads#troubleshooting).

Děkujeme, že dokončení tohoto úvodu do stránky Razor. Děkujeme za jakékoli komentáře, která zůstanou. [Začínáme s MVC a EF základní](xref:data/ef-mvc/intro) je vynikající postupujte podle kroků až tento kurz.

## <a name="additional-resources"></a>Další zdroje

* [Nahrávání souborů v ASP.NET Core](xref:mvc/models/file-uploads)
* [IFormFile](/dotnet/api/microsoft.aspnetcore.http.iformfile)

>[!div class="step-by-step"]
[Předchozí: ověření](xref:tutorials/razor-pages/validation)
