---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
title: Zpracování výjimek na úrovni knihoven BLL a DAL (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu se podíváme tactfully zpracování výjimky vyvolána během pracovního postupu aktualizace upravitelné DataList.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: f8fd58e2-f932-4f08-ab3d-fbf8ff3295d2
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-cs
msc.type: authoredcontent
ms.openlocfilehash: ebaca5ea34fabe3fcd4979eab2e3f684e8e221be
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754629"
---
<a name="handling-bll--and-dal-level-exceptions-c"></a>Zpracování výjimek na úrovni knihoven BLL a DAL (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_CS.exe) nebo [stahovat PDF](handling-bll-and-dal-level-exceptions-cs/_static/datatutorial38cs1.pdf)

> V tomto kurzu se podíváme tactfully zpracování výjimky vyvolána během pracovního postupu aktualizace upravitelné DataList.


## <a name="introduction"></a>Úvod

V [Přehled úprav a odstraňování dat v ovládacím prvku DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) kurzu jsme vytvořili DataList, která nabízejí jednoduché úpravy a odstranění funkce. Zatímco plně funkční, bylo téměř uživatelsky přívětivé jako všechny chyby, ke které došlo během úpravy nebo odstranění proces vedl k neošetřené výjimce. Například vynechání produkt s názvem nebo při úpravách produktu, zadáním hodnoty cena velmi cenově!, dojde k výjimce. Vzhledem k tomu, že tato výjimka není zachycena v kódu, bubliny až modul runtime ASP.NET, která zobrazí podrobnosti o výjimce s na webové stránce.

Jak jsme viděli v [zpracování knihoven BLL a výjimek úrovni DAL na stránce ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurz, pokud je vyvolána výjimka z hlubin obchodní logikou nebo vrstvy přístupu k datům, podrobnosti výjimky jsou vráceny do ObjectDataSource a potom do prvku GridView. Jsme viděli, jak tyto výjimky elegantně zpracovat tak, že vytvoříte `Updated` nebo `RowUpdated` obslužné rutiny událostí pro prvek ObjectDataSource nebo prvku GridView, kontrola výjimku a pak označující, že výjimka byla zpracována.

Naše DataList kurzy, ale nejsou t pro aktualizace a odstranění dat pomocí ObjectDataSource. Místo toho spolupracujeme přímo proti BLL. Aby bylo možné zjišťování výjimek pocházejících z knihoven BLL a DAL, musíme implementovat kódu v rámci kódu naši stránku ASP.NET pro zpracování výjimek. V tomto kurzu uvidíme více tactfully zpracování výjimky vyvolána během upravitelné DataList s aktualizace pracovního postupu.

> [!NOTE]
> V *přehled o úpravy a odstraňování dat v ovládacím prvku DataList* pomocí ObjectDataSource pro aktualizaci zahrnuty některé techniky kurzu jsme probírali různých postupů pro úpravy a odstraňování dat v ovládacím prvku DataList a Odstraňuje se. Pokud tyto postupy, můžete zpracovávat výjimky z knihoven BLL a DAL pomocí prvku ObjectDataSource s `Updated` nebo `Deleted` obslužných rutin událostí.


## <a name="step-1-creating-an-editable-datalist"></a>Krok 1: Vytvoření upravitelné DataList

Předtím, než jsme starat o zpracování výjimek, ke kterým dochází při aktualizaci pracovního postupu, umožní s nejprve vytvořit upravitelné DataList. Otevřít `ErrorHandling.aspx` stránku `EditDeleteDataList` složky, přidat a v prvku DataList do návrháře, nastavte jeho `ID` vlastnost `Products`, a přidejte nový prvek ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třída s `GetProducts()` zaznamenává metodu pro výběr; nastavte rozevírací seznamy v INSERT, UPDATE a odstranit karty na (žádný).


[![Vrátí informace o produktu pomocí GetProducts() – metoda](handling-bll-and-dal-level-exceptions-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-cs/_static/image1.png)

**Obrázek 1**: vrátit informací pomocí produktu `GetProducts()` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-cs/_static/image3.png))


Po dokončení Průvodce prvek ObjectDataSource, vytvoří Visual Studio automaticky `ItemTemplate` pro prvku DataList. Nahraďte ho názvem `ItemTemplate` , který se zobrazí každý produkt s názvem a ceny a obsahuje tlačítko pro úpravy. Dále vytvořte `EditItemTemplate` s ovládacím prvkem webového textové pole pro název a ceny a aktualizace a zrušit. Nakonec nastavte DataList s `RepeatColumns` vlastnost na 2.

Po provedení těchto změn kódu s deklarativní stránky s by měl vypadat nějak takto. Překontrolujte, ujistěte se, že úprav, zrušení, a aktualizace tlačítka mají jejich `CommandName` vlastnosti nastavení upravit, zrušit a aktualizovat v uvedeném pořadí.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample1.aspx)]

> [!NOTE]
> Pro účely tohoto kurzu prvku DataList musí být povolen stav zobrazení s.


Za chvíli zobrazíte náš postup přes prohlížeč (viz obrázek 2).


[![Každý produkt obsahuje tlačítko pro úpravy](handling-bll-and-dal-level-exceptions-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-cs/_static/image4.png)

**Obrázek 2**: každý produkt obsahuje tlačítko Upravit ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-cs/_static/image6.png))


V současné době na tlačítko Upravit pouze vyvolá zpětné volání je t kódu ještě používání produktu snazší upravovat. Aby se povolily úpravy, potřebujeme vytvořit obslužné rutiny událostí pro DataList s `EditCommand`, `CancelCommand`, a `UpdateCommand` události. `EditCommand` a `CancelCommand` události jednoduše aktualizovat DataList s `EditItemIndex` vlastnost a obnovení vazby dat k ovládacím prvku DataList:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample2.cs)]

`UpdateCommand` Obslužná rutina události se tak zapojí o něco víc. Je potřeba si přečíst také upravených produktu s `ProductID` z `DataKeys` spolu s produkt s názvem a cenu z textových polí v kolekci `EditItemTemplate`a následně zavolat `ProductsBLL` třída s `UpdateProduct` metoda před vrácením prvku DataList do předem úprav stavu.

Prozatím použijte umožňují s právě přesně stejný kód z `UpdateCommand` obslužné rutině událostí ve *Přehled úprav a odstraňování dat v ovládacím prvku DataList* kurzu. Přidáme kód pro pohodlné zpracování výjimek v kroku 2.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample3.cs)]

I v případě neplatné zadání, což může být ve formě nesprávně formátovaná Jednotková cena, hodnotu neplatná jednotka cena takto: $5.00 nebo vynechání s název produktu, který bude vyvolána výjimka. Vzhledem k tomu, `UpdateCommand` obslužná rutina události neobsahuje žádné kód v tomto okamžiku zpracování výjimek, výjimky je předána do modulu runtime ASP.NET, ve kterém se zobrazí koncovému uživateli (viz obrázek 3).


![Když dojde k neošetřené výjimce koncovému uživateli se zobrazí chybová stránka](handling-bll-and-dal-level-exceptions-cs/_static/image7.png)

**Obrázek 3**: když dojde k neošetřené výjimce koncovému uživateli se zobrazí chybová stránka


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Krok 2: Řádně zpracování výjimek v obslužné rutině události UpdateCommand

Během aktualizace pracovního postupu, může dojít k výjimkám v `UpdateCommand` obslužná rutina události, BLL nebo vrstvy DAL. Například, pokud uživatel zadá cenu příliš drahé, `Decimal.Parse` příkaz v `UpdateCommand` vyvolá obslužnou rutinu události `FormatException` výjimky. Pokud uživatel vynechá s názvem produktu nebo cena má zápornou hodnotu, bude DAL vyvolat výjimku.

Když dojde k výjimce, chceme zobrazit informativní zprávy v rámci samotné stránky. Přidat popisek webový ovládací prvek na stránce, jehož `ID` je nastavena na `ExceptionDetails`. Konfigurace zobrazovaný v červené, velmi velká, písmo tučné písmo a kurzíva přiřazením text popisku s jeho `CssClass` vlastnost `Warning` třídu šablony stylů CSS, která je definována v `Styles.css` souboru.

Když dojde k chybě, chceme jenom popisek, který se zobrazí pouze jednou. To znamená by měl popisek s upozornění na následné postbacky zmizet. Můžete to udělat buď vymazání si popisek s `Text` vlastnost nebo nastavení jeho `Visible` vlastnost `False` v `Page_Load` obslužné rutiny události (jako jsme to udělali v [zpracování knihoven BLL a DAL úrovni výjimky v ASP Stránku technologie .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurzu) nebo zakázáním podpory popisek s zobrazení stavu. Umožní s použít druhou možnost.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample4.aspx)]

Když je vyvolána výjimka, přiřadíme podrobnosti výjimka, která má `ExceptionDetails` ovládacímu prvku s popisek `Text` vlastnost. Protože svůj stav zobrazení je zakázané v následných zpětného odeslání `Text` vlastnost s programové změny budou ztraceny, návrat k výchozí text (prázdný řetězec), a tím skrytí upozornění.

Pokud chcete zjistit, kdy bylo vyvoláno chybu mohla zobrazit na stránce užitečné zprávu, potřebujeme přidat `Try ... Catch` bloku `UpdateCommand` obslužné rutiny události. `Try` Část obsahuje kód, který může způsobit výjimku, zatímco `Catch` blok obsahuje kód, který se spustí i v případě výjimku. Podívejte se [Základy zpracování výjimek](https://msdn.microsoft.com/library/2w8f0bss.aspx) tématu v dokumentaci k rozhraní .NET Framework pro další informace o `Try ... Catch` bloku.


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample5.cs)]

Při vyvolání výjimky libovolného typu kódu v rámci `Try` bloku `Catch` kód bloku s začne provádění. Typ výjimky, která je vyvolána `DbException`, `NoNullAllowedException`, `ArgumentException`a tak dále, co přesně, závisí na prvním místě srážejí chybu. Pokud se tam s problém na úrovni databáze `DbException` bude vyvolána výjimka. Pokud je zadána neplatná hodnota pro `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, nebo `ReorderLevel` pole, `ArgumentException` bude vyvolána výjimka, protože jsme přidali kód pro ověření tyto hodnoty polí v `ProductsDataTable` třídy (najdete v článku [ Vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-cs.md) kurzu).

Můžeme poskytnout užitečné vysvětlení pro koncového uživatele tak, že zvolíte text zprávy pro typ zachycena výjimka. Následující kód, který byl použit ve formě skoro stejné zpátky [zpracování knihoven BLL a výjimek úrovni DAL na stránce ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurz obsahuje tato úroveň podrobností:


[!code-csharp[Main](handling-bll-and-dal-level-exceptions-cs/samples/sample6.cs)]

K dokončení tohoto kurzu, jednoduše zavolejte `DisplayExceptionDetails` metodu z `Catch` bloku předávajícího zachycené `Exception` instance (`ex`).

S `Try ... Catch` blokovat na místě, uživatelům se zobrazí chybovou zprávu dál jako hodnoty 4 a 5 zobrazit. Všimněte si, že i v případě výjimky prvku DataList zůstane v režimu úprav. Je to proto, jakmile dojde k výjimce, toku řízení okamžitě přesměrován `Catch` bloku, bez použití kódu, který vrátí do stavu před úpravy prvku DataList.


[![Pokud uživatel vynechá povinné pole, zobrazí se chybová zpráva](handling-bll-and-dal-level-exceptions-cs/_static/image9.png)](handling-bll-and-dal-level-exceptions-cs/_static/image8.png)

**Obrázek 4**: Pokud uživatel vynechá povinné pole, zobrazí se chybová zpráva ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-cs/_static/image10.png))


[![Chybová zpráva se zobrazí při zadání záporné cena](handling-bll-and-dal-level-exceptions-cs/_static/image12.png)](handling-bll-and-dal-level-exceptions-cs/_static/image11.png)

**Obrázek 5**: chybová zpráva se zobrazí při zadání záporné cena ([kliknutím ji zobrazíte obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-cs/_static/image13.png))


## <a name="summary"></a>Souhrn

GridView a ObjectDataSource poskytují po úrovně obslužné rutiny, které obsahují informace o žádné výjimky, které byly vyvolány během pracovního postupu aktualizace a odstranění, jakož i vlastnosti, které lze nastavit k označení, zda byla výjimka zpracovat. Tyto funkce, ale nejsou k dispozici při práci s prvku DataList a použití BLL přímo. Místo toho jsme zodpovídají za implementaci zpracování výjimek.

V tomto kurzu jsme viděli, jak přidat zpracování výjimek pro upravitelné DataList s aktualizace pracovního postupu tak, že přidáte `Try ... Catch` bloku `UpdateCommand` obslužné rutiny události. Pokud se během aktualizace pracovního postupu, je vyvolána výjimka `Catch` spustí kód bloku s, užitečné informace v zobrazení `ExceptionDetails` popisek.

V tomto okamžiku prvku DataList nevyvine žádnou akci zabránit výjimky, pokud chcete zabránit vzniku na prvním místě. I když víme, že negativní cena způsobí výjimku, jsme některé t ještě přidány žádné funkce k proaktivnímu zabránit uživateli v zadávání těchto neplatný vstup. V následujícím kurzem uvidíme, jak snížit výjimky způsobené Neplatný uživatelský vstup ve přidání validačních ovládacích prvků v `EditItemTemplate`.

Všechno nejlepší programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Pokyny k návrhu pro výjimky](https://msdn.microsoft.com/library/ms298399.aspx)
- [Chyba protokolovací moduly a obslužné rutiny (ELMAH)](http://workspaces.gotdotnet.com/elmah) (knihovny open source pro protokolování chyb)
- [Enterprise Library pro .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (včetně bloku výjimky správy aplikace)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Ken Pespisa. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](performing-batch-updates-cs.md)
> [další](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
