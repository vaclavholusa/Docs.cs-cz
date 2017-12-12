---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: "Zpracování výjimek úrovni BLL a DAL (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu vidíte způsob tactfully zpracování výjimek vyvolaných během pracovního postupu aktualizace upravitelné DataList."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 245e381eca2aca61be5f860d1ec9994b482a9863
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="handling-bll--and-dal-level-exceptions-vb"></a>Zpracování výjimek úrovni BLL a DAL (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) nebo [stáhnout PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> V tomto kurzu vidíte způsob tactfully zpracování výjimek vyvolaných během pracovního postupu aktualizace upravitelné DataList.


## <a name="introduction"></a>Úvod

V [přehled úpravy a odstraňování dat v prvku DataList](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) kurzu jsme vytvořili DataList, který nabízí jednoduché úpravy a odstraňování možnosti. Během plně funkční, se špatně uživatelsky přívětivý systém, jako chyby, které došlo během úprav nebo odstraňování proces vedlo k neošetřené výjimce. Například vynechání název produktu s nebo při úpravě produktu, zadáním hodnoty ceny velmi dostupnou!, vyvolá výjimku. Vzhledem k tomu, že tato výjimka není zachycena v kódu, bubliny až modul runtime ASP.NET, který pak zobrazí podrobnosti o výjimce s na webové stránce.

Jak jsme viděli v [zpracování BLL - a výjimky na úrovni DAL na stránku ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) kurzu, pokud dojde k výjimce z depths obchodní logiky nebo datové vrstvy přístup, podrobnosti této výjimky se vrátíte na ObjectDataSource a potom do GridView. Jsme viděli, jak pro pohodlné zpracování tyto výjimky tak, že vytvoříte `Updated` nebo `RowUpdated` obslužných rutin událostí pro ObjectDataSource nebo GridView, kontrolu pro výjimku a pak indikující, že byla zpracována výjimka.

Naše DataList kurzy, ale nejsou t pomocí ObjectDataSource pro aktualizaci a odstraňování data. Místo toho pracujeme přímo na BLL. Chcete-li zjistit výjimky pocházející z BLL nebo DAL, musíme implementovat kódu v rámci kódu naší stránce s ASP.NET pro zpracování výjimek. V tomto kurzu vidíte postup více tactfully zpracování výjimek vyvolaných během upravitelné DataList s aktualizace pracovního postupu.

> [!NOTE]
> V *přehled o úpravy a odstraňování dat v prvku DataList* kurzu jsme probrali různé techniky pro úpravy a odstranění dat z prvku DataList některé techniky podílejí pomocí ObjectDataSource pro aktualizaci a odstranění. Pokud budete využívat tyto postupy, můžete zpracovat výjimky z BLL nebo DAL prostřednictvím ObjectDataSource s `Updated` nebo `Deleted` obslužné rutiny událostí.


## <a name="step-1-creating-an-editable-datalist"></a>Krok 1: Vytvoření upravitelné DataList

Před jsme starat o zpracování výjimek, ke kterým došlo během aktualizace pracovního postupu, umožní s nejprve vytvořit upravitelné DataList. Otevřete `ErrorHandling.aspx` stránky v `EditDeleteDataList` složky, přidejte DataList do návrháře, nastavte jeho `ID` vlastnost `Products`, a přidejte nový ObjectDataSource s názvem `ProductsDataSource`. Konfigurace ObjectDataSource používat `ProductsBLL` třídu s `GetProducts()` metoda pro výběr zaznamenává; nastavte rozevírací seznamy v příkaz INSERT, UPDATE a odstranit karty na (žádný).


[![Vrátí informace o produktu pomocí metody GetProducts()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Obrázek 1**: vrátit na informace o produktu pomocí `GetProducts()` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))


Po dokončení Průvodce ObjectDataSource vytvoří sada Visual Studio automaticky `ItemTemplate` pro prvku DataList. Nahraďte ho s `ItemTemplate` , zobrazí každý název produktu s a ceny a obsahuje tlačítko pro úpravy. Dále vytvořte `EditItemTemplate` s ovládacím prvkem webové textové pole pro název a ceny a tlačítek aktualizace a zrušit. Nakonec nastavte DataList s `RepeatColumns` vlastnost na hodnotu 2.

Po provedení těchto změn značky s deklarativní stránky s by měl vypadat takto. Překontrolujte, ujistěte se, že úpravy, zrušit, a mít aktualizace tlačítka jejich `CommandName` vlastnosti nastavena na upravit, zrušit a aktualizovat, v uvedeném pořadí.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> Pro účely tohoto kurzu DataList musí být povolena s zobrazení stavu.


Za chvíli zobrazit naše průběh prostřednictvím prohlížeče (viz obrázek 2).


[![Každý produkt obsahuje tlačítko pro úpravy](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Obrázek 2**: každý produkt obsahuje tlačítko Upravit ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))


V současné době tlačítko Upravit pouze způsobí postback ho nemá t ještě usnadňují používání produktu upravovat. Pokud chcete povolit, úpravy, musíme vytváření obslužných rutin událostí pro DataList s `EditCommand`, `CancelCommand`, a `UpdateCommand` události. `EditCommand` a `CancelCommand` události jednoduše aktualizovat DataList s `EditItemIndex` vlastnost a obnovení vazby dat prvku DataList:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

`UpdateCommand` Obslužné rutiny události je trochu složitější. Je potřeba si přečíst upravená produktu s `ProductID` z `DataKeys` kolekce spolu s názvem produktu s a cena z textových polí v `EditItemTemplate`a pak zavolají `ProductsBLL` třídu s `UpdateProduct` před vrácením prvku DataList – metoda předem úpravy stav.

Teď, umožňují s stačí použít přesný stejný kód z `UpdateCommand` obslužné rutiny událostí v *přehled úpravy a odstraňování dat v prvku DataList* kurzu. Přidáme kód pro pohodlné zpracování výjimek v kroku 2.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

Při krátkodobém neplatný vstup může být ve tvaru nesprávně naformátovaný jednotkové ceny, hodnotu ceny neplatný jednotky jako-$5.00 nebo opomenutí s název produktu, který bude vyvolána výjimka. Vzhledem k tomu `UpdateCommand` obslužné rutiny události neobsahuje žádné zpracování kód v tomto okamžiku výjimek, výjimka je předána až modulem runtime ASP.NET, kde se zobrazí koncovému uživateli (viz obrázek 3).


![Když dojde k neošetřené výjimce koncovému uživateli se zobrazí chybová stránka](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Obrázek 3**: když dojde k neošetřené výjimce koncovému uživateli se zobrazí chybová stránka


## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Krok 2: Pohodlné zpracování výjimek v obslužné rutiny události UpdateCommand

Během aktualizace pracovního postupu, může dojít výjimky v `UpdateCommand` obslužné rutiny události, BLL nebo DAL. Například, pokud uživatel zadá cena příliš nákladnější `Decimal.Parse` příkaz v `UpdateCommand` obslužné rutiny události vyvolá výjimku `FormatException` výjimka. Pokud uživatel vynechá s název produktu nebo cenu má zápornou hodnotu, DAL vyvolá výjimku.

Když dojde k výjimce, chceme zobrazit informativní zpráva v rámci vlastní stránky. Přidání webové popisek řízení na stránku, jehož `ID` je nastaven na `ExceptionDetails`. Konfigurace na text popisku s zobrazíte v red velmi velká tučné písmo a kurzíva písma přiřazením jeho `CssClass` vlastnost, která má `Warning` třídy CSS, která je definována v `Styles.css` souboru.

Když dojde k chybě, chceme jenom štítek, který chcete se zobrazují jen jednou. To znamená by měl popisek s upozornění na následné postback zmizí. To lze provést tak, že smažete na štítek s `Text` vlastnost nebo nastavení jeho `Visible` vlastnost `False` v `Page_Load` obslužné rutiny události (jako jsme to udělali zpět v [zpracování BLL - a úrovni DAL výjimky v ASP Stránku technologie .NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) kurzu) nebo zakázáním podporu popisek s zobrazení stavu. Umožní s, použijte druhou možnost.


[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Pokud je vyvolána výjimka, jsme budete přiřadit podrobnosti výjimky k `ExceptionDetails` popisku ovládacího prvku s `Text` vlastnost. Protože je zakázán svůj stav zobrazení na následné postback `Text` vlastnosti s programové změny budou ztraceny, vrácení zpět na výchozí text (prázdný řetězec), a tím skrytí upozornění.

Pokud chcete zjistit, kdy bylo vyvoláno chybu mohla zobrazit užitečné zpráva na stránce, je potřeba přidat `Try ... Catch` zablokujte `UpdateCommand` obslužné rutiny události. `Try` Část obsahuje kód, který může způsobit výjimku, zatímco `Catch` blok obsahuje kód, který se spustí při krátkodobém výjimku. Podívejte se [Základy zpracování výjimek](https://msdn.microsoft.com/en-us/library/2w8f0bss.aspx) části v dokumentaci k rozhraní .NET Framework pro další informace na `Try ... Catch` bloku.


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Pokud je vyvolána výjimka libovolného typu kódem v rámci `Try` bloku, `Catch` kód bloku s zahájí provádění. Typ výjimky, která je vyvolána `DbException`, `NoNullAllowedException`, `ArgumentException`a tak dále, závisí na co, přesně, nejprve vysráží chyba. Pokud zde s problém na úrovni databáze `DbException` bude vyvolána. Pokud je zadaná neplatná hodnota pro `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, nebo `ReorderLevel` pole, `ArgumentException` bude vyvolána, protože jsme přidali kód pro ověření těchto polí v aplikaci `ProductsDataTable` – třída (najdete v článku [ Vytvoření vrstvy obchodní logiky](../introduction/creating-a-business-logic-layer-vb.md) kurzu).

Můžeme poskytnout další užitečné vysvětlení pro koncového uživatele tak, že zvolíte text zprávy na typu byla zjištěna výjimka. Následující kód, který byl použit ve formuláři skoro stejné zpět v [zpracování BLL - a výjimky na úrovni DAL na stránku ASP.NET](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) kurzu poskytuje tato úroveň podrobností:


[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

K dokončení tohoto kurzu, stačí zavolat `DisplayExceptionDetails` metoda z `Catch` bloku předávání v zachycení `Exception` instance (`ex`).

Pomocí `Try ... Catch` blokovat na místě, uživatelé si můžou informativnější chybová zpráva, jako obrázků 4 a 5 zobrazit. Všimněte si, že při krátkodobém výjimku prvku DataList zůstane v režimu úprav. Důvodem je, že jakmile dojde k výjimku, tok řízení se okamžitě přesměruje na `Catch` bloku obcházení kód, který vrátí prvku DataList do stavu před úpravy.


[![Chybová zpráva se zobrazí, pokud uživatel vynechá požadované pole](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Obrázek 4**: chybová zpráva se zobrazí, pokud uživatel vynechá požadované pole ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))


[![Chybová zpráva se zobrazí při zadávání záporné cena](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Obrázek 5**: chybová zpráva se zobrazí při zadávání záporné ceny ([Kliknutím zobrazit obrázek v plné velikosti](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))


## <a name="summary"></a>Souhrn

Rutina GridView a ObjectDataSource poskytují po úrovně obslužné rutiny, které obsahují informace o všechny výjimky, které byly vyvolány během pracovního postupu aktualizaci a odstraňování, jakož i vlastnosti, které lze nastavit indikující, zda byla výjimka zpracovává. Tyto funkce, ale jsou k dispozici při práci s prvku DataList a pomocí BLL přímo. Místo toho jsme zodpovídají za implementace zpracování výjimek.

V tomto kurzu jsme viděli, jak přidat upravitelné DataList s přidáním aktualizace pracovního postupu zpracování výjimek `Try ... Catch` zablokujte `UpdateCommand` obslužné rutiny události. Pokud dojde k výjimce během aktualizace pracovního postupu, `Catch` kód bloku s provede, užitečné informace najdete v zobrazení `ExceptionDetails` popisek.

V tomto okamžiku DataList nesnaží zabránit výjimky z děje na prvním místě. I když víme, že záporné cena způsobí výjimku, nebyla některá jsme t ještě přidány žádné funkce proaktivně zabránit uživatelům ve vstupu takové neplatný vstup. V našem kurzu další ukážeme, jak snížit výjimky způsobené Neplatný uživatelský vstup přidáním ověřovacích ovládacích prvků v `EditItemTemplate`.

Radostí programování!

## <a name="further-reading"></a>Další čtení

Další informace o tématech popsané v tomto kurzu najdete v následujících zdrojích informací:

- [Pokyny pro návrh pro výjimky](https://msdn.microsoft.com/en-us/library/ms298399.aspx)
- [Moduly protokolování chyb a obslužné rutiny (ELMAH)](http://workspaces.gotdotnet.com/elmah) (knihovny open source pro protokolování chyb)
- [Knihovna Enterprise pro rozhraní .NET Framework 2.0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (zahrnuje bloku výjimky správy aplikace)

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Ken Pespisa. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](performing-batch-updates-vb.md)
[další](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
