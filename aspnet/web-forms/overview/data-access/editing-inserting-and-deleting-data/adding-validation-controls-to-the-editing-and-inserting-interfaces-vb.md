---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: "Přidání ovládacích prvků ověření pro úpravu a vkládání rozhraní (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu ukážeme, jak snadné je přidat do EditItemTemplate a InsertItemTemplate dat ovládací prvek webu, zadejte další ovládací prvky pro ověřování..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 53414aa17514d07083fe05b8c2abcba10a01cf98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Přidání ovládacích prvků ověření pro úpravu a vkládání rozhraní (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) nebo [stáhnout PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> V tomto kurzu budete vidíte, jak je snadné k přidávání ovládacích prvků ověření EditItemTemplate a InsertItemTemplate dat ovládací prvek webu, zadejte více spolehlivá uživatelské rozhraní.


## <a name="introduction"></a>Úvod

Rutina GridView a DetailsView ovládací prvky v příkladech jsme jste prozkoumali v posledních tří kurzy mít všechny se skládá z BoundFields a CheckBoxFields (typy polí automaticky přidá Visual Studio, při vytváření vazby GridView nebo DetailsView ke zdroji dat řízení pomocí inteligentních značek). Při úpravě řádek v GridView nebo DetailsView, tyto BoundFields, které nejsou jen pro čtení se převedou do textových polí, ze kterého může koncový uživatel upravit existující data. Podobně když vkládání nový záznam do ovládacího prvku DetailsView těchto BoundFields jehož `InsertVisible` je nastavena na `True` (výchozí) se vykresluje jako prázdná textová pole, do kterého může uživatel zadat hodnoty polí nový záznam. Podobně CheckBoxFields, které jsou zakázány v rozhraní standardní, jen pro čtení, se převedou na povoleno zaškrtávací políčka v úpravy a vkládání rozhraní.

Výchozí hodnota úpravy a vkládání rozhraní pro BoundField a vlastnost CheckBoxField mohou být užitečné, rozhraní nemá žádný typ ověřování. Pokud uživatel provede chybu položka dat – například vynechání `ProductName` pole nebo zadáte neplatnou hodnotu pro `UnitsInStock` (například -50) výjimku, bude vyvolána z v rámci depths architektury aplikace. Když tato výjimka může být řádně zpracována jak je předvedeno v [předchozí kurzu](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), v ideálním případě by mělo prvky ověřování k uživateli zabránit v přechodu do takové neplatná data v zahrnovat úprav nebo vkládání uživatelské rozhraní první místo.

Chcete-li provést vložení rozhraní nebo vlastní úpravy, musíme BoundField nebo vlastnost CheckBoxField nahraďte TemplateField. TemplateFields, které byly v tématu diskuse ve [pomocí TemplateFields v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) a [pomocí TemplateFields v ovládacím prvku DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) kurzy, se může skládat z několika definování šablony oddělení rozhraní pro stavy různých řádků. TemplateField `ItemTemplate` se používá ke při vykreslování pole jen pro čtení nebo řádků v ovládacích prvcích DetailsView nebo GridView, zatímco `EditItemTemplate` a `InsertItemTemplate` znamenat rozhraní pro úpravu a vkládání režimy, v uvedeném pořadí.

V tomto kurzu ukážeme, jak je snadné přidání ovládacích prvků ověření TemplateField `EditItemTemplate` a `InsertItemTemplate` zajistit více spolehlivá uživatelské rozhraní. Konkrétně v tomto kurzu trvá vytvořené v příkladu [zkoumání události spojené s vložení, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurzu a rozšiřuje úpravy a vkládání rozhraní zahrnout příslušné ověření.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Krok 1: Replikace příklad z[zkoumání události související s vložení, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

V [zkoumání události spojené s vložení, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurzu jsme vytvořili stránky, který uvedené názvy a ceny produktů v upravitelné GridView. Kromě toho stránce zahrnuté DetailsView jejichž `DefaultMode` byla nastavena na `Insert`, a tím vždy vykreslení v režimu vkládání. Z této DetailsView může uživatel zadat název a ceny pro nového produktu, klikněte na tlačítko Vložit a jej přidat do systému (viz obrázek 1).


[![Předchozí příklad umožňuje uživatelům přidávat nové produkty a upravit existující](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Obrázek 1**: předchozím příkladu umožňuje uživatelům přidat novými produkty a upravit existující ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Naším cílem pro tento kurz je k posílení DetailsView a GridView, aby se zajistil ovládací prvky pro ověřování. Konkrétně se ověřovací logiku:

- Vyžadovat, aby při vkládání nebo úpravou produktu třeba zadat název
- Vyžadovat, aby cenu byla zadána při vkládání záznam; Při úpravě záznam, jsme se stále vyžadují ceny, ale použije programový logiku v prvku GridView `RowUpdating` obslužné rutiny události z dřívějších kurzu již existuje.
- Zkontrolujte, zda je hodnota zadaná pro cenu formátu platný měny

Podíváme můžete na předchozí příklad zahrnout ověření rozšířit, nejprve musíme replikovat příklad z `DataModificationEvents.aspx` stránky na stránku pro účely tohoto kurzu `UIValidation.aspx`. K tomu je potřeba kopírovat přes obě `DataModificationEvents.aspx` deklarativní stránky a jeho zdrojový kód. Nejdřív zkopírujte přes deklarativní provedením následujících kroků:

1. Otevřete `DataModificationEvents.aspx` stránka v sadě Visual Studio
2. Přejděte na stránky deklarativní (kliknutím na tlačítko zdroj v dolní části stránky)
3. Zkopírujte text v rámci `<asp:Content>` a `</asp:Content>` značky (čáry 3 až 44), jako v zobrazené na obrázku 2.


[![Zkopírujte Text v &lt;asp: obsah&gt; ovládací prvek](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Obrázek 2**: kopírování textu v `<asp:Content>` ovládací prvek ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Otevřete `UIValidation.aspx` stránky
2. Přejděte na stránky deklarativní značky
3. Sem vložte text, v rámci `<asp:Content>` ovládacího prvku.

Pokud chcete zkopírovat přes zdrojový kód, otevřete `DataModificationEvents.aspx.vb` stránky a zkopírujte pouze text *v rámci* `EditInsertDelete_DataModificationEvents` – třída. Zkopírujte tři události obslužné rutiny (`Page_Load`, `GridView1_RowUpdating`, a `ObjectDataSource1_Inserting`), ale provést **není** zkopírujte deklaraci třídy. Vložte zkopírovaný text *v rámci* `EditInsertDelete_UIValidation` třídy v `UIValidation.aspx.vb`.

Po přesunutí nad obsah a kód z `DataModificationEvents.aspx` k `UIValidation.aspx`, za chvíli k otestování průběh v prohlížeči. Měli byste vidět stejný výstup a zkušenosti stejnou funkcionalitu v každé z těchto dvou stránkách (odkazuje zpět na obrázku 1 pro snímek obrazovky `DataModificationEvents.aspx` v akci).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Krok 2: Převodu BoundFields TemplateFields

K přidávání ovládacích prvků ověření na rozhraní úpravy a vkládání, potřebovat BoundFields používá funkci DetailsView a GridView ovládacích prvků mají být převedeny do TemplateFields. Jak toho docílit, klikněte na Upravit sloupce a upravit pole odkazy v GridView a prvku DetailsView inteligentních značek v uvedeném pořadí. Vyberte jednotlivé BoundFields existuje a klikněte na odkaz "Převést toto pole TemplateField".


[![Každý z prvku DetailsView a GridView BoundFields převést TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Obrázek 3**: převést každý z prvku DetailsView a GridView BoundFields do TemplateFields ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Převod BoundField do TemplateField prostřednictvím dialogové okno pole generuje TemplateField, pro jehož stejné rozhraní jen pro čtení, úpravy a vkládání jako BoundField sám sebe. Následující kód ukazuje deklarativní syntaxe `ProductName` pole v ovládacím prvku DetailsView po převedení do TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Všimněte si, že tento TemplateField měl třemi šablonami, automaticky vytvoří `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate`. `ItemTemplate` Zobrazí hodnotu pole jednoho datového (`ProductName`) při použití ovládacího prvku popisek Web, `EditItemTemplate` a `InsertItemTemplate` současná hodnota pole dat v ovládacím prvku TextBox webového, který přidruží datové pole textového pole `Text` Vlastnost pomocí obousměrné vazby dat. Vzhledem k tomu, že DetailsView na této stránce se používá pouze pro vložení, může dojít k odebrání `ItemTemplate` a `EditItemTemplate` ze dvou TemplateFields, i když není škodu v necháte.

Vzhledem k tomu, že rutina GridView nepodporuje integrovanou vkládání funkcí DetailsView, převod prvku GridView `ProductName` pole do TemplateField výsledkem pouze `ItemTemplate` a `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Kliknutím na tlačítko "Převést toto pole do TemplateField" sady Visual Studio se vytvořil TemplateField jejichž šablony napodobovat uživatelského rozhraní převedený BoundField. Můžete to ověřit této stránce prostřednictvím prohlížeče. Zjistíte, že vzhled a chování TemplateFields je stejný jako rozhraní při BoundFields používaly místo.

> [!NOTE]
> Nebojte se, že rozhraní úpravy v šablonách podle potřeby přizpůsobit. Například může Chceme mít textové pole `UnitPrice` TemplateFields se vykresluje jako textové pole menší než `ProductName` textové pole. K tomu můžete nastavit textového pole `Columns` vlastnost na odpovídající hodnotu nebo zadejte absolutní šířku prostřednictvím `Width` vlastnost. V dalším kurzu vidíte úplně přizpůsobení rozhraní úpravy nahrazením textové pole s alternativní datový záznam ovládací prvek webu.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Krok 3: Přidání ovládacích prvků ověření do prvku GridView`EditItemTemplate` s

Při vytváření formuláře pro zadávání dat, je důležité, že uživatelé zadají všechna požadovaná pole a zda jsou všechny zadané vstupy právní, správně naformátován hodnoty. Aby bylo zajištěno, že vstupy uživatele jsou platné, technologie ASP.NET poskytuje pět integrované ověření ovládacích prvků, které jsou navržené tak, který se má použít k ověření hodnoty jednoho vstupního ovládacího prvku:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) zajistí, že byla zadána hodnota
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) ověří hodnotu s jinou hodnotou webové ovládací prvek nebo konstantní hodnotu nebo zajišťuje, že hodnota formátu právní pro zadaný datový typ
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) zajistí, že je hodnota v rozsahu hodnot
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) hodnotu porovnává [regulární výraz](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) hodnotu porovnává vlastní, uživatelsky definované metoda

Další informace o těchto pět ovládacích prvků, podívejte se [část ovládací prvky pro ověřování](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) z [ASP.NET rychlý start kurzy](https://asp.net/QuickStart/aspnet/).

V našem kurzu budeme muset použít RequiredFieldValidator DetailsView i GridView `ProductName` TemplateFields a RequiredFieldValidator v ovládacím prvku DetailsView `UnitPrice` TemplateField. Kromě toho budeme muset přidat CompareValidator do obou ovládacích prvků `UnitPrice` TemplateFields, který zajistí, že zadaná cena na hodnotu větší než nebo rovna 0 a se zobrazí ve formátu platné měny.

> [!NOTE]
> Při ASP.NET 1.x měl tyto stejné pět ověřovací ovládací prvky, technologii ASP.NET 2.0 přidala množství vylepšení, hlavní dva skripty na straně klienta se podpora pro prohlížeče než Internet Explorer a možnost ověření oddílu ovládacích prvků na stránce do ověření skupiny. Další informace o nových funkcí řízení ověření v 2.0, najdete v části [Rozvěrače ověřovacích ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Začněme přidáním potřeby ověření ovládací prvky `EditItemTemplate` s v prvku GridView TemplateFields. K tomu, klikněte na odkaz Upravit šablony z prvku GridView inteligentní značky se zprovoznit rozhraní úpravy šablony. Tady můžete vybrat šablonu upravit v rozevíracím seznamu. Vzhledem k tomu, že chceme posílení rozhraní úprav, je potřeba přidat ověřovací ovládací prvky pro `ProductName` a `UnitPrice`na `EditItemTemplate` s.


[![Je potřeba rozšířit ProductName a na UnitPrice EditItemTemplates](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Obrázek 4**: je potřeba rozšířit `ProductName` a `UnitPrice`na `EditItemTemplate` s ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


V `ProductName` `EditItemTemplate`, přidejte RequiredFieldValidator přetažením z panelu nástrojů do rozhraní úpravy šablony umístění po textové pole.


[![Přidání RequiredFieldValidator do ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Obrázek 5**: přidejte RequiredFieldValidator k `ProductName` `EditItemTemplate` ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Všechny ovládací prvky ověřování práci ověření vstupu jeden ovládací prvek ASP.NET Web. Proto je potřeba znamenat, že by měl RequiredFieldValidator, kterou jsme právě přidali vyhodnotit proti textového pole v `EditItemTemplate`; to provádí nastavení ovládacího prvku ověření [vlastnost ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) k `ID` ovládacího prvku příslušný Web. Do textového pole v současnosti má místo nondescript `ID` z `TextBox1`, ale můžeme ho změnit na něco vhodnější. Klikněte na textové pole v šabloně a poté v okně vlastností změňte `ID` z `TextBox1` k `EditProductName`.


[![Změňte ID textového pole na EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Obrázek 6**: Změna textového pole `ID` k `EditProductName` ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Dále nastavte RequiredFieldValidator `ControlToValidate` vlastnost `EditProductName`. Nakonec nastavte [ErrorMessage vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) k "Je nutné zadat název produktu" a [vlastnost Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) na "\*". `Text` Hodnota vlastnosti, pokud je zadán, je text, který se zobrazí ověření ovládacím prvkem, pokud se ověření nezdaří. `ErrorMessage` Hodnotu vlastnosti, který je vyžadován, je používán ovládacího prvku ValidationSummary; Pokud `Text` hodnota vlastnosti je vynechán, `ErrorMessage` hodnota vlastnosti je také textu zobrazovaného ovládací prvek ověřování v neplatný vstup.

Po nastavení tyto tři vlastnosti RequiredFieldValidator, by měla vypadat podobně jako na obrázku 7 obrazovky.


[![Nastavte RequiredFieldValidator ControlToValidate, ErrorMessage a vlastnosti textu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Obrázek 7**: nastavte RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, a `Text` vlastnosti ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


S RequiredFieldValidator přidán do `ProductName` `EditItemTemplate`, všechny, že zůstanou je přidání potřebné k ověření `UnitPrice` `EditItemTemplate`. Vzhledem k tomu, že jsme jste se rozhodli, že pro tuto stránku `UnitPrice` je volitelný při úprava záznamů, není třeba přidat RequiredFieldValidator. , Ale potřebujete přidat CompareValidator zajistit, aby `UnitPrice`, pokud je zadaný, správně naformátován jako měny a je větší než nebo rovna 0.

Před přidáme CompareValidator k `UnitPrice` `EditItemTemplate`, nejprve změňte ID ovládacího prvku TextBox webového z `TextBox2` k `EditUnitPrice`. Po provedení této změny, přidejte CompareValidator, nastavení jeho `ControlToValidate` vlastnost `EditUnitPrice`, jeho `ErrorMessage` vlastnost "cenu musí být větší než nebo roven nule a nemůže obsahovat symbolu měny" a jeho `Text` vlastnosti "\*".

Označuje, zda `UnitPrice` hodnota musí být větší než nebo rovna 0, nastavte CompareValidator [operátor vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) k `GreaterThanEqual`, jeho [ValueToCompare vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" a jeho [ Type – vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) k `Currency`. Následující deklarativní syntaxe ukazuje `UnitPrice` na TemplateField `EditItemTemplate` po provedly tyto změny:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Po provedení těchto změn, otevřete stránku v prohlížeči. Pokud se pokusíte vynechat název nebo zadejte hodnotu ceny neplatný při úpravě produktu, se zobrazí vedle textového pole hvězdičku. Jak ukazuje obrázek 8, cena hodnotu, která zahrnuje symbol měny, jako je například 19,95 je považovány za neplatné. CompareValidator `Currency` `Type` umožňuje oddělovačů číslice (například čárky a tečky, v závislosti na nastavení jazykové verze) a začátku plus nebo minus, ale nemá *není* povolit symbolu měny. Toto chování může perplex uživatelů, jak rozhraní úpravy aktuálně vykreslí `UnitPrice` formátu měny.

> [!NOTE]
> Odvolat, že v *události spojené s vložení, aktualizace a odstranění* kurzu jsme nastavit BoundField `DataFormatString` vlastnost `{0:c}` k formátování jako měny. Kromě toho jsme nastavit `ApplyFormatInEditMode` způsobuje GridView je rozhraní pro úpravy vlastnost na hodnotu true, `UnitPrice` jako měny. Při převodu BoundField TemplateField, Visual Studio uvedené tato nastavení a formátu textového pole `Text` vlastnost jako měny pomocí syntaxe datové vazby `<%# Bind("UnitPrice", "{0:c}") %>`.


[![U textových polí s neplatný vstup se zobrazuje hvězdička](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Obrázek 8**: hvězdičky se zobrazí další textová pole s neplatný vstup ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Při ověření funguje jako-je, že uživatel musí ručně odebrat symbolu měny při úpravě záznam, který není platný. Chcete-li to opravit, máme tři možnosti:

1. Konfigurace `EditItemTemplate` tak, aby `UnitPrice` hodnota není formátu měny.
2. Povolit uživatele k zadání symbolu měny odebráním CompareValidator a nahraďte ho RegularExpressionValidator, správně zjišťuje přítomnost hodnotu správně formátovaný měny. Problémem je, že regulární výraz k ověření hodnotu měny není velmi a by vyžadovaly psaní kódu, pokud jsme chtěli začlenit nastavení jazykové verze.
3. Úplně odebrat ovládací prvek ověřovací a spoléhá na logiku ověřování na straně serveru v prvku GridView `RowUpdating` obslužné rutiny události.

Vraťme se s možností #1 pro toto cvičení. Aktuálně `UnitPrice` je ve formátu měny z důvodu ve výrazu datové vazby v textové pole `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Změňte příkaz vazby a `Bind("UnitPrice", "{0:n2}")`, který zformátuje výsledek jako číslo s dvě číslice přesnosti. To lze provést přímo pomocí deklarativní syntaxe, nebo kliknutím na odkaz Upravit datové vazby z `EditUnitPrice` textového pole v `UnitPrice` na TemplateField `EditItemTemplate` (viz následující obrázky 9 a 10).


[![Klikněte na odkaz Upravit datové vazby textového pole](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Obrázek 9**: klikněte na odkaz Upravit datové vazby textového pole ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![V příkazu vazby zadejte specifikace formátu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Obrázek 10**: Zadejte specifikace formátu v `Bind` – příkaz ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Díky této změně formátovaný cenu v úpravy rozhraní zahrnuje čárky jako oddělovač skupin a jako oddělovač desetinných míst, ale ponechá vypnout symbolu měny.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Neobsahuje RequiredFieldValidator, což zpětné volání pro ověřte a logice aktualizací a zahájení. Ale `RowUpdating` obslužné rutiny události zkopírovali z *zkoumání události spojené s vložení, aktualizace a odstranění* kurz zahrnuje kontrolu programový, které zajistí, že `UnitPrice` je k dispozici. Nebojte se odebrat tuto logiku, nechte ji jako-, nebo přidejte RequiredFieldValidator k `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Krok 4: Shrnutí problémů položka dat

Kromě pěti ověřovací ovládací prvky, technologie ASP.NET obsahuje [ovládací prvek ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), které zobrazuje `ErrorMessage` s kontrolní mechanismy ověřování, které zjištěna neplatná data. Tato souhrnná data mohou být zobrazeny jako text na webové stránce nebo prostřednictvím messagebox modální, na straně klienta. Umožňuje zvýšit tento kurz a zahrnují klienta messagebox shrnutí problémy ověření.

K tomu, přetáhněte ovládací prvek ValidationSummary z panelu nástrojů na návrháře. Umístění ovládacího prvku ověření není důležité skutečnosti, vzhledem k tomu, že vytvoříme a nakonfigurujte ho na jenom zobrazit souhrn jako messagebox. Po přidání ovládacího prvku, nastavte jeho [ShowSummary vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) k `False` a jeho [ShowMessageBox vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) k `True`. Pomocí tohoto přidání všechny chyby ověřování, jsou shrnuté v messagebox na straně klienta.


[![Chyby ověření jsou shrnuty v Messagebox na straně klienta](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Obrázek 11**: chyby ověření jsou shrnuty v Messagebox na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Krok 5: Přidání ovládacích prvků ověření do ovládacího prvku DetailsView`InsertItemTemplate`

V tomto kurzu jen zbývá k přidávání ovládacích prvků ověření na rozhraní vložení prvku DetailsView. Proces přidávání ovládacích prvků pro ověřování DetailsView šablon je stejná jako v kroku 3; proto jsme budete breeze prostřednictvím úlohy v tomto kroku. Jako jsme to udělali s prvku GridView `EditItemTemplate` s, I doporučujeme, abyste přejmenovat `ID` s textových polí z nondescript `TextBox1` a `TextBox2` k `InsertProductName` a `InsertUnitPrice`.

Přidat RequiredFieldValidator k `ProductName` `InsertItemTemplate`. Nastavte `ControlToValidate` k `ID` textového pole v šabloně, jeho `Text` vlastnost "\*" a jeho `ErrorMessage` vlastnost "Je nutné zadat název produktu".

Vzhledem k tomu, `UnitPrice` je potřeba pro tuto stránku při přidávání nového záznamu, přidejte RequiredFieldValidator k `UnitPrice` `InsertItemTemplate`, nastavení jeho `ControlToValidate`, `Text`, a `ErrorMessage` vlastnosti správně. Nakonec přidejte CompareValidator k `UnitPrice` `InsertItemTemplate` taky konfigurace jeho `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, a `ValueToCompare` vlastnosti stejně, jako jsme to udělali s `UnitPrice`na CompareValidator v prvku GridView `EditItemTemplate`.

Po přidání tyto ovládací prvky pro ověřování, nelze přidat do systému, pokud není zadán její název, nebo pokud svou cenu na záporné číslo nového produktu nebo nelegálního formátu.


[![Logiku ověření byl přidán do rozhraní vložení prvku DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Obrázek 12**: logiku ověření byl přidán do rozhraní vložení prvku DetailsView ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Krok 6: Ověření ovládacích prvků do skupin ověření rozdělení do oddílů

Naši stránku se skládá ze dvou logicky různorodých sad ovládacích prvků ověřování: ty, které odpovídají GridView je úprava rozhraní a ty, které odpovídají DetailsView je vložení rozhraní. Ve výchozím nastavení, když dojde k zpětné volání *všechny* se kontroluje ověřovací ovládací prvky na stránce. Ale při úpravě záznam Neradi bychom prvku DetailsView rozhraní vkládání ovládacích prvků ověřování k ověření. Obrázek 13 znázorňuje naše aktuální dilematem, pokud uživatel upravuje produkt perfektně platné hodnoty, kliknutím na aktualizace způsobí chybu ověření, protože hodnoty název a cena v vkládání rozhraní jsou prázdné.


[![Aktualizace produktu vede vkládání rozhraní ověřovací ovládací prvky pro ještě efektivněji](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Obrázek 13**: aktualizace produktu vede ověření rozhraní vkládání ovládacích prvků pro ještě efektivněji ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Ověřovacích ovládacích prvků v technologii ASP.NET 2.0 lze rozdělit do skupin ověření prostřednictvím jejich `ValidationGroup` vlastnost. Chcete-li přidružit sadu ovládacích prvků ověření ve skupině, jednoduše nastavte jejich `ValidationGroup` vlastnost, která má stejnou hodnotu. V našem kurzu nastavit `ValidationGroup` vlastnosti ovládacích prvků ověřování v prvku GridView TemplateFields k `EditValidationControls` a `ValidationGroup` vlastnosti prvku DetailsView TemplateFields k `InsertValidationControls`. Tyto změny lze provést přímo v deklarativní nebo prostřednictvím okna Vlastnosti při použití nástroje Designer upravit šablonu rozhraní.

Kromě ověření ovládacích prvků, tlačítka a tlačítko Související ovládací prvky v technologii ASP.NET 2.0 také zahrnovat `ValidationGroup` vlastnost. Kontroluje validátory skupinu ověření platnosti pouze, pokud je zpětné volání vyvolané pomocí tlačítka, která má stejný `ValidationGroup` nastavení vlastnosti. Například v pořadí pro tlačítko vložení prvku DetailsView k aktivaci `InsertValidationControls` ověření skupiny je potřeba nastavit CommandField `ValidationGroup` vlastnost `InsertValidationControls` (viz obrázek 14). Kromě toho nastavit prvku GridView na CommandField `ValidationGroup` vlastnost `EditValidationControls`.


[![Sada DetailsView je vlastnost na CommandField ValidationGroup InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Obrázek 14**: nastavte DetailsView CommandField na `ValidationGroup` vlastnost `InsertValidationControls` ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Po provedení těchto změn DetailsView a GridView TemplateFields a CommandFields by měl vypadat takto:

TemplateFields a CommandField prvku DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField a TemplateFields GridView.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

Ovládací prvky specifické pro úpravy ověření v tomto okamžiku protipožární pouze když rutina GridView aktualizace po kliknutí na tlačítko a ještě efektivněji ovládací prvky specifické pro vložení ověření jenom v případě, že DetailsView vložení po kliknutí na tlačítko, vyřešení tohoto problému zvýraznit pomocí obrázek 13. Ale v této změně naše ovládací prvek ValidationSummary už nebude zobrazovat při zadávání neplatná data. Také obsahuje prvek ValidationSummary `ValidationGroup` vlastnost a pouze zobrazuje souhrnné informace o kontrolní mechanismy ověřování ve skupině jeho ověření. Proto je potřeba mít dva ověřovací ovládací prvky na této stránce, jeden pro `InsertValidationControls` skupiny ověření a jeden pro `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Pomocí tohoto přidání našem kurzu je dokončena.

## <a name="summary"></a>Souhrn

Při BoundFields můžete zadat, jak rozhraní vkládání a úpravy, není přizpůsobitelný rozhraní. Běžně chceme Přidání ověření ovládacích prvků pro úpravy a vkládání rozhraní zajistit, že uživatel zadá povinné vstupní ve formátu právní. K tomu nemůžeme převést BoundFields TemplateFields a přidání ovládacích prvků ověření do příslušné šablony. V tomto kurzu jsme příklad z rozšířené *zkoumání události spojené s vložení, aktualizace a odstranění* kurzu přidání ovládacích prvků ověření do obou DetailsView je vkládání rozhraní a GridView. úpravy rozhraní. Kromě toho jsme viděli, jak zobrazit informace o souhrnu ověření pomocí ovládacího prvku ValidationSummary a jak oddílu ověřovací ovládací prvky na stránce do skupin odlišné ověření.

Jak jsme viděli v tomto kurzu, povolí TemplateFields rozhraní úpravy a vkládání do být rozšíření, zahrnující ovládací prvky pro ověřování. TemplateFields lze také rozšířit zahrnout další vstupní ovládací prvky webového, povolení textového pole Nahradit za vhodnější ovládací prvek webu. V našem kurzu další ukážeme, jak nahradit ovládacího prvku textového pole s ovládací prvek rozevírací seznam vázané na data, který je ideální pro úpravy cizí klíč (například `CategoryID` nebo `SupplierID` v `Products` tabulky).

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři v tomto kurzu se Liz Shulok a Zack Petr. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
[další](customizing-the-data-modification-interface-vb.md)
