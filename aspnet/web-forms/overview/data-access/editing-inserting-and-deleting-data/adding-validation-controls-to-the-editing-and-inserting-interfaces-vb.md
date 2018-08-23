---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Přidání validačních ovládacích prvků pro úpravy a vložení rozhraní (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků do EditItemTemplate a šablona InsertItemTemplate dat webový ovládací prvek, poskytovat...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: d06408717bdf5e7446597ae4330ffb32cf943e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757120"
---
<a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Přidání validačních ovládacích prvků pro úpravy a vložení rozhraní (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) nebo [stahovat PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků do EditItemTemplate a šablona InsertItemTemplate dat webový ovládací prvek, a poskytuje tak více spolehlivá uživatelské rozhraní.


## <a name="introduction"></a>Úvod

Ovládací prvky GridView a DetailsView v příkladech jste Prozkoumali jsme v posledních tří kurzů mají všechny se skládá z BoundFields a CheckBoxFields (typy polí automaticky přidá sada Visual Studio při vytváření vazby ke zdroji dat GridView nebo prvku DetailsView. řízení prostřednictvím inteligentních značek). Při úpravě řádku v prvku GridView nebo prvku DetailsView. Tyto BoundFields, které nejsou jen pro čtení se převedou do textových polí, ze kterého může koncový uživatel upravit existující data. Podobně při vložení nového záznamu do ovládacího prvku DetailsView. Tyto BoundFields jehož `InsertVisible` je nastavena na `True` (výchozí) jsou vykresleny jako prázdná textová pole, do kterého může uživatel zadat hodnoty polí nového záznamu. Obdobně CheckBoxFields, které jsou zakázány v rozhraní standardní, jen pro čtení, se převedou na povoleno zaškrtávací políčka v úpravy a vložení rozhraní.

Výchozí úpravy a vložení pro vlastnost BoundField a třídě CheckBoxField rozhraní mohou být užitečné, nemá rozhraní jakýkoli druh ověření. Pokud uživatel provede chybu položka dat – například vynechání `ProductName` pole nebo zadáte neplatnou hodnotu pro `UnitsInStock` (například -50) bude výjimka vyvolána v rámci hlubin architekturu aplikace. Když tuto výjimku můžete elegantně zpracovat jak je ukázáno v [předchozí kurz o službě](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), v ideálním případě by validačních ovládacích prvků, chcete-li zabránit uživateli v zadávání těchto neplatná data v zahrnout úpravy nebo vložení uživatelského rozhraní první místo.

Aby bylo možné zajistit přizpůsobený úpravy nebo vložení rozhraní, musíme Vlastnost BoundField nebo třídě CheckBoxField nahraďte TemplateField. Vlastností TemplateField, které byly tématu diskuze v [použití vlastností TemplateField v ovládacím prvku GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) a [použití vlastností TemplateField v ovládacím prvku DetailsView](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) kurzy, se může skládat z více definování šablony oddělení rozhraní pro stavy odlišný řádek. Pole TemplateField `ItemTemplate` se používá k při vykreslování pole jen pro čtení nebo řádky v ovládacích prvcích DetailsView nebo ovládacího prvku GridView, že `EditItemTemplate` a `InsertItemTemplate` označení rozhraní pro úpravy a vložení režimy, v uvedeném pořadí.

V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků pole TemplateField `EditItemTemplate` a `InsertItemTemplate` poskytnout více spolehlivá uživatelské rozhraní. Konkrétně tohoto kurzu trvá vytvořený v příkladu [zkoumání události spojené s vložení, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurzu a argumentech úpravy a vložení rozhraní zahrnout příslušné ověření.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deletingexamining-the-events-associated-with-inserting-updating-and-deleting-vbmd"></a>Krok 1: Replikace z příkladu[zkoumání událostí spojených s vložením, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

V [zkoumání události spojené s vložení, aktualizace a odstranění](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) kurzu jsme vytvořili stránku, která uvedené názvy a ceny produktů v upravitelné prvku GridView. Kromě toho na stránce zahrnuté DetailsView jehož `DefaultMode` nastavenou na `Insert`, a tím vždy vykreslení v režimu vkládání. Z tohoto prvku DetailsView. může uživatel zadat název a cena nového produktu, klikněte na tlačítko Vložit a přidat ho do systému (viz obrázek 1).


[![Předchozí příklad umožňuje uživatelům přidávat nové produkty a upravovat existující aplikace](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Obrázek 1**: The předchozí příklad umožňuje uživatelům přidávat nové produkty a upravit existující ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))


Naším cílem pro účely tohoto kurzu je k posílení DetailsView a GridView poskytovat validačních ovládacích prvků. Konkrétně se naše logiku ověřování:

- Vyžadovat, že je možné zadat název při vložení nebo úpravě produktu
- Požadovat, že cena byly poskytnuty při vložení záznamu; Při úpravě záznamu, budeme se stále vyžadují ceny, ale bude použít programovou logiku v prvku GridView `RowUpdating` z předchozích kurzu již existuje obslužná rutina události
- Ujistěte se, že hodnota zadaná pro cenu formátu platné měny

Předtím, než abychom se mohli podívat na rozšíření předchozího příkladu na zahrnout ověřování, musíme nejprve příklad z replikace `DataModificationEvents.aspx` stránky na stránku pro účely tohoto kurzu `UIValidation.aspx`. K tomu potřeba zkopírovat i `DataModificationEvents.aspx` deklarativním označení stránky a jeho zdrojový kód. Nejprve zkopírovat deklarativní provedením následujících kroků:

1. Otevřít `DataModificationEvents.aspx` stránky v sadě Visual Studio
2. Přejděte na stránku deklarativní (kliknutím na tlačítko zdroj v dolní části stránky)
3. Zkopírujte text v rámci `<asp:Content>` a `</asp:Content>` značky (čáry 3 až 44), jako znázorněno na obrázku 2.


[![Zkopírujte Text v &lt;asp: Content&gt; ovládacího prvku](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Obrázek 2**: Zkopírujte Text v `<asp:Content>` ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))


1. Otevřít `UIValidation.aspx` stránky
2. Přejděte na stránku deklarativní
3. Vložit text v rámci `<asp:Content>` ovládacího prvku.

Chcete-li zkopírovat zdrojový kód, otevřete `DataModificationEvents.aspx.vb` stránce a zkopírujte pouze text *v rámci* `EditInsertDelete_DataModificationEvents` třídy. Zkopírujte tři události obslužné rutiny (`Page_Load`, `GridView1_RowUpdating`, a `ObjectDataSource1_Inserting`), na rozdíl od **není** zkopírujte deklaraci třídy. Vložit zkopírovaný text *v rámci* `EditInsertDelete_UIValidation` třídy v `UIValidation.aspx.vb`.

Po přesunutí nad obsah a kód z `DataModificationEvents.aspx` k `UIValidation.aspx`, věnujte chvíli a otestujte svůj postup v prohlížeči. Měli byste vidět stejný výstup a prostředí stejně v každém z těchto dvou stránkách (vrátit zpět na obrázku 1 pro snímek obrazovky `DataModificationEvents.aspx` v akci).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Krok 2: Převod BoundFields do vlastností TemplateField

Přidání validačních ovládacích prvků do rozhraní pro úpravy a vložení, nutné převést do vlastností TemplateField BoundFields používané ovládací prvky prvku DetailsView a ovládacího prvku GridView. K dosažení tohoto cíle, klikněte na Upravit sloupce a upravit pole odkazů v prvku GridView a ovládacího prvku DetailsView inteligentní značky, v uvedeném pořadí. Vyberte každou BoundFields a klikněte na odkaz "Převést toto pole TemplateField".


[![Každá z ovládacího prvku DetailsView a prvku GridView BoundFields převést vlastností TemplateField](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Obrázek 3**: převést každý z ovládacího prvku DetailsView a prvku GridView BoundFields do vlastností TemplateField ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))


Převod na pole TemplateField přes dialogové okno pole Vlastnost BoundField generuje TemplateField vykazuje stejné jen pro čtení, úpravy a vložení rozhraní jako vlastnost BoundField samotný. Následující kód ukazuje deklarativní syntaxe `ProductName` pole v ovládacím prvku DetailsView. poté, co bude převeden na pole TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Všimněte si, že tento TemplateField měli tři šablony vytvoří automaticky `ItemTemplate`, `EditItemTemplate`, a `InsertItemTemplate`. `ItemTemplate` Zobrazí hodnotu jednoho datového pole (`ProductName`) použití popisku webový ovládací prvek, zatímco `EditItemTemplate` a `InsertItemTemplate` současná hodnota pole dat v ovládacím prvku webového textové pole, která přidružuje pole data textového pole `Text` Vlastnost s použitím dvousměrnou datovou vazbou. Protože ovládacím prvku DetailsView. na této stránce se používá pouze pro vkládání, můžete odebrat `ItemTemplate` a `EditItemTemplate` ze dvou vlastností TemplateField, i když není na jejich opuštění škodu.

Protože prvku GridView nepodporuje integrované vkládání funkcí prvku DetailsView, převod prvku GridView `ProductName` pole na pole TemplateField vede pouze `ItemTemplate` a `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Kliknutím na tlačítko "Převést toto pole na pole TemplateField" Visual Studio vytvořila TemplateField jehož šablony napodobují uživatelské rozhraní převedený Vlastnost BoundField. Můžete to ověřit tak navštívit tuto stránku prostřednictvím prohlížeče. Zjistíte, že vzhled a chování vlastností TemplateField je stejný jako zkušenosti při BoundFields používaly místo.

> [!NOTE]
> Nebojte se, že přizpůsobení rozhraní pro úpravy v šablonách podle potřeby. Například může Chceme mít textovém poli `UnitPrice` vlastností TemplateField vykreslen jako textové pole s menší než `ProductName` textového pole. K tomu můžete nastavit textové pole `Columns` vlastnost na odpovídající hodnotu nebo zadejte absolutní šířky prostřednictvím `Width` vlastnost. V dalším kurzu uvidíme zcela přizpůsobení rozhraní pro úpravy nahrazením alternativní datový záznam webový ovládací prvek textového pole.


## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Krok 3: Přidání validačních ovládacích prvků do prvku GridView`EditItemTemplate` s

Při vytváření formuláře pro zadávání dat, je důležité, aby uživatelé zadali všechna povinná pole a zda všechny zadané vstupy jsou hodnoty právní, správně naformátovaná. K zajištění, že jsou platné uživatelské vstupy, technologie ASP.NET poskytuje pět ověření integrované ovládací prvky, které jsou navržené tak, který se má použít k ověření hodnoty jednoho vstupního ovládacího prvku:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) zajistí, že byl poskytnut hodnotu
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) ověřuje hodnoty s jinou hodnotou ovládací prvek webové nebo konstantní hodnotu, nebo se zajistí, že formát hodnoty je platný pro zadaný datový typ
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) zajistí, že je hodnota v rozsahu hodnot
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) hodnotu porovnává [regulárního výrazu](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) hodnotu porovnává vlastní, uživatelem definované metody

Další informace o těchto pět ovládacích prvků, podívejte se [validačních ovládacích prvků části](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) z [kurzy rychlý start ASP.NET](https://asp.net/QuickStart/aspnet/).

V našem kurzu budeme muset použít RequiredFieldValidator v prvku DetailsView i v prvku GridView `ProductName` vlastností TemplateField a RequiredFieldValidator v ovládacím prvku DetailsView `UnitPrice` TemplateField. Kromě toho budeme muset přidat CompareValidator oba ovládací prvky `UnitPrice` vlastností TemplateField, který zajistí, že zadaná cena má hodnotu větší než nebo rovna 0 a je uvedené ve formátu platné měny.

> [!NOTE]
> Zatímco ASP.NET 1.x měla tyto stejné pět validačních ovládacích prvků, technologii ASP.NET 2.0 bylo přidáno několik vylepšení, hlavní dvou skriptů na straně klienta se podpora prohlížečích než Internet Explorer a schopnost oddílu validačních ovládacích prvků na stránce do ověření skupiny. Další informace o nových funkcích ovládacího prvku ověření ve verzi 2.0, přečtěte si [rozbor validačních ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Začněme přidáním nezbytné validačních ovládacích prvků na `EditItemTemplate` s vlastností TemplateField v prvku GridView. K tomu, klikněte na odkaz Upravit šablony z prvku GridView inteligentních značek zobrazíte rozhraní úprav šablony. Tady můžete vybrat šablonu upravit v rozevíracím seznamu. Protože chceme rozšířit rozhraní úprav, musíme přidání validačních ovládacích prvků na `ProductName` a `UnitPrice`společnosti `EditItemTemplate` s.


[![Musíme rozšířit ProductName a EditItemTemplates UnitPrice.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Obrázek 4**: musíme rozšířit `ProductName` a `UnitPrice`společnosti `EditItemTemplate` s ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))


V `ProductName` `EditItemTemplate`, přidejte RequiredFieldValidator jeho přetažením z panelu nástrojů do rozhraní pro úpravy šablony, umístit po textového pole.


[![Přidat RequiredFieldValidator ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Obrázek 5**: Přidat RequiredFieldValidator k `ProductName` `EditItemTemplate` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))


Všechny ovládací prvky ověřování pracujte ověření vstupu jeden ovládací prvek technologie ASP.NET. Proto potřebujeme k označení, že RequiredFieldValidator jsme právě přidali, měli ověřit proti textového pole ve `EditItemTemplate`; to lze provést nastavením ovládacího prvku ověření [vlastnost ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) k `ID` odpovídající webového ovládacího prvku. Textové pole má v současné době spíše nondescript `ID` z `TextBox1`, ale můžeme ho změnit na něco vhodnějšího. Klikněte na textové pole v šabloně a potom v okně Vlastnosti změňte `ID` z `TextBox1` k `EditProductName`.


[![Změnit textovém poli ID EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Obrázek 6**: změnit textovém poli `ID` k `EditProductName` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))


Dále nastavte RequiredFieldValidator `ControlToValidate` vlastnost `EditProductName`. Nastavte [ErrorMessage vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) na "Je nutné zadat název produktu" a [vlastnost Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) na "\*". `Text` Hodnota vlastnosti, pokud je zadán, je text, který se zobrazí v ovládacím prvku ověření, pokud se ověření nezdaří. `ErrorMessage` Hodnotu vlastnosti, které je potřeba, se používá prvek; Pokud `Text` hodnota vlastnosti je vynechán, `ErrorMessage` hodnota vlastnosti je také text zobrazený v ovládacím prvku ověření na neplatný vstup.

Po nastavení tyto tři vlastnosti RequiredFieldValidator, vaše obrazovka by měla vypadat podobně jako na obrázku 7.


[![Nastavte RequiredFieldValidator ControlToValidate, chybová zpráva a vlastnosti textu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Obrázek 7**: nastavte RequiredFieldValidator `ControlToValidate`, `ErrorMessage`, a `Text` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))


S RequiredFieldValidator přidán do `ProductName` `EditItemTemplate`, all, zůstane je přidat nezbytné k ověření `UnitPrice` `EditItemTemplate`. Protože jsme jste se rozhodli, že pro tuto stránku `UnitPrice` volitelně při upravuje záznam, není potřeba přidat RequiredFieldValidator. , Ale potřebujeme přidat CompareValidator zajistit, aby `UnitPrice`, je-li zadán, je ve správném formátu jako měnu a je větší než nebo rovna 0.

Než přidáme CompareValidator k `UnitPrice` `EditItemTemplate`, Změníme nejprve ID ovládacího prvku textového pole webového z `TextBox2` k `EditUnitPrice`. Po provedení této změny, přidejte CompareValidator, nastavení jeho `ControlToValidate` vlastnost `EditUnitPrice`, jeho `ErrorMessage` vlastnost "cena musí být větší než nebo rovna nule a nemůže obsahovat symbol měny" a jeho `Text` vlastnost "\*".

Označuje, že `UnitPrice` hodnota musí být větší než nebo rovno 0, nastavte CompareValidator [operátor vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) k `GreaterThanEqual`, jeho [ValueToCompare vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) "0" a jeho [ Zadejte vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) k `Currency`. Zobrazí se následující deklarativní syntaxe `UnitPrice` společnosti TemplateField `EditItemTemplate` po provedení těchto změn:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Po provedení těchto změn, otevřete stránku v prohlížeči. Při pokusu vynechat název nebo zadejte hodnotu Neplatná cena při úpravách produktu, se zobrazí hvězdičku vedle textového pole. Jak ukazuje obrázek 8, cena hodnotu, která obsahuje symbol měny, jako je například 19,95 je považovány za neplatné. CompareValidator `Currency` `Type` umožňuje oddělovače číslic (například mezery nebo období, v závislosti na nastavení jazykové verze) a předních plus nebo mínus, ale nemá *není* povolit symbol měny. Toto chování může perplex uživatelů, jak rozhraní úprav aktuálně vykreslí `UnitPrice` formátu měny.

> [!NOTE]
> Připomínáme, že v *události přidružené k vložení, aktualizace a odstranění* kurzu nastavíme Vlastnost BoundField `DataFormatString` vlastnost `{0:c}` provedu formátování jako měnu. Kromě toho jsme nastavili `ApplyFormatInEditMode` vlastnost na hodnotu true, způsobí prvku GridView uživatele úpravy rozhraní k formátování `UnitPrice` jako měnu. Při převodu Vlastnost BoundField na pole TemplateField, Visual Studio jste si poznamenali tato nastavení a textového pole ve formátu `Text` vlastnost jako měnu pomocí syntaxe databinding `<%# Bind("UnitPrice", "{0:c}") %>`.


[![Hvězdička se zobrazí vedle textových polí s neplatný vstup](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Obrázek 8**: hvězdička se zobrazí další textová pole s neplatný vstup ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))


Při ověřování funguje jako-se, uživatel musí při úpravách záznam, který není přijatelné, ručně odeberte symbol měny. Chcete-li to napravit, máme tři možnosti:

1. Konfigurace `EditItemTemplate` tak, aby `UnitPrice` hodnota není naformátována jako měnu.
2. Povolit, aby uživatel zadal symbol měny odebráním CompareValidator a jeho nahrazení atributem RegularExpressionValidator, vyhledávající správně hodnotu správně formátovaná měny. Problémem je, že regulární výraz k ověření hodnotu měny není přehlednou a bude vyžadovat psaní kódu, pokud jsme chtěli začlenit nastavení jazykové verze.
3. Úplně odebrat ovládací prvek ověření a Spolehněte se na logiku ověřování na straně serveru v prvku GridView `RowUpdating` obslužné rutiny události.

Pojďme se možnost #1 pro účely tohoto cvičení. Aktuálně `UnitPrice` je ve formátu měny z důvodu výraz datové vazby pro textové pole v `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Změnit vazby příkazu k `Bind("UnitPrice", "{0:n2}")`, který zformátuje výsledek jako číslo s dvěma číslicemi přesnosti. To můžete udělat přímo pomocí deklarativní syntaxe nebo kliknutím na odkaz upravit vlastnosti DataBindings z `EditUnitPrice` textového pole v `UnitPrice` společnosti TemplateField `EditItemTemplate` (viz obrázky 9 a 10).


[![Klikněte na odkaz Upravit datové vazby textové pole](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Obrázek 9**: klikněte na odkaz Upravit datové vazby textové pole ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))


[![V příkazu vazby zadat specifikátor formátu](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Obrázek 10**: určit formát specifikátoru v `Bind` – příkaz ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))


Díky této změně formátovaný cena úpravy rozhraní zahrnuje čárky jako oddělovače skupin a tečku jako oddělovač desetinných míst, ale ponechá vypnout symbol měny.

> [!NOTE]
> `UnitPrice` `EditItemTemplate` Neobsahuje RequiredFieldValidator, povolení zpětné volání, aby a aktualizace logiky a zahájení. Ale `RowUpdating` zkopírovaná z obslužné rutiny události *zkoumání události spojené s vložení, aktualizace a odstranění* kurz zahrnuje kontrolu prostřednictvím kódu programu, který zajistí, že `UnitPrice` je k dispozici. Nebojte se odebrat tuto logiku, necháváme ji jak-, nebo přidat RequiredFieldValidator k `UnitPrice` `EditItemTemplate`.


## <a name="step-4-summarizing-data-entry-problems"></a>Krok 4: Shrnutí problémů položka dat

Kromě pěti ověřovacích ovládacích prvků technologie ASP.NET obsahuje [ovládacího prvku ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), které ukazuje `ErrorMessage` s těmito validačních ovládacích prvků, které se zjistila neplatná data. Tato souhrnná data lze zobrazit jako text na webové stránce nebo přes pole messagebox modální, na straně klienta. Můžeme vylepšit tento kurz a zahrnout pole messagebox na straně klienta sumarizace jakékoli zaznamenané problémy s ověřením.

K tomu, přetáhněte ovládací prvek souhrnu ověření z panelu nástrojů do návrháře. Umístění ovládacího prvku ověření nezáleží, protože chceme ji můžete zobrazit souhrn jenom jako pole messagebox nakonfigurovat. Po přidání ovládacího prvku, nastavte jeho [ShowSummary vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) k `False` a jeho [ShowMessageBox vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) k `True`. Uveďte všechny chyby ověření jsou shrnuté v messagebox na straně klienta.


[![Chyby ověření jsou shrnuté v Messagebox na straně klienta](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Obrázek 11**: The chyby ověření jsou shrnuté v Messagebox na straně klienta ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))


## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Krok 5: Přidání validačních ovládacích prvků do ovládacího prvku DetailsView`InsertItemTemplate`

Pro účely tohoto kurzu už jen zbývá přidání validačních ovládacích prvků do rozhraní vložení prvku DetailsView. Přidání validačních ovládacích prvků na ovládacím prvku DetailsView šablony procesu je stejný jako, který vyšetřovány v kroku 3; proto jsme budete breeze prostřednictvím úkolu v tomto kroku. Jako jsme to udělali s prvku GridView `EditItemTemplate` s, neváhejte se přejmenovat `ID` s textových polí z nondescript `TextBox1` a `TextBox2` k `InsertProductName` a `InsertUnitPrice`.

Přidat RequiredFieldValidator k `ProductName` `InsertItemTemplate`. Nastavte `ControlToValidate` k `ID` textového pole v šabloně, jeho `Text` vlastnost "\*" a jeho `ErrorMessage` vlastnost "Je nutné zadat název produktu".

Protože `UnitPrice` je vyžaduje tuto stránku při přidání nového záznamu, přidejte RequiredFieldValidator k `UnitPrice` `InsertItemTemplate`a nastavte jeho `ControlToValidate`, `Text`, a `ErrorMessage` vlastnosti odpovídajícím způsobem. Nakonec přidejte CompareValidator k `UnitPrice` `InsertItemTemplate` také konfigurace jeho `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`, a `ValueToCompare` vlastnosti stejně, jako jsme to udělali s `UnitPrice`společnosti CompareValidator v prvku GridView `EditItemTemplate`.

Po přidání těchto validačních ovládacích prvků, nelze přidat do systému, pokud jeho název není zadán, nebo pokud cena je záporné číslo nového produktu nebo neoprávněně ve formátu.


[![Logiku ověřování je přidaný do rozhraní vložení ovládacího prvku DetailsView.](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Obrázek 12**: logiku ověřování je přidaný do rozhraní vložení prvku DetailsView ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))


## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Krok 6: Dělení validačních ovládacích prvků do ověření skupiny

Naši stránku se skládá ze dvou logicky různorodé sady validačních ovládacích prvků: ty, které odpovídají prvku GridView je úprava rozhraní a ty, které odpovídají na ovládacím prvku DetailsView je vložení rozhraní. Ve výchozím nastavení, když dojde k postbacku *všechny* validačních ovládacích prvků na stránce nebyly zvoleny. Ale při úpravě záznamu nechceme ovládacím prvku DetailsView vkládání rozhraní validačních ovládacích prvků pro ověření. Obrázek 13 ukazuje naše aktuální dilema, když uživatel upravuje produkt s naprosto platné hodnoty, kliknutím na aktualizace způsobí chybu ověřování, protože hodnoty název a cena vkládání rozhraní jsou prázdné.


[![Aktualizuje se produkt způsobí, že vkládání rozhraní validačních ovládacích prvků k vyvolání](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Obrázek 13**: aktualizace produktu způsobí, že rozhraní vložení validačních ovládacích prvků vyvolání ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))


Validačních ovládacích prvků v technologii ASP.NET 2.0 lze rozdělit do skupin ověřování prostřednictvím jejich `ValidationGroup` vlastnost. Chcete-li přiřadit sadu ověřování ovládacích prvků ve skupině, jednoduše nastavte jejich `ValidationGroup` vlastnost na stejnou hodnotu. V našem kurzu nastavte `ValidationGroup` vlastnosti validačních ovládacích prvků v prvku GridView vlastností TemplateField k `EditValidationControls` a `ValidationGroup` vlastnosti prvku DetailsView vlastností TemplateField k `InsertValidationControls`. Tyto změny můžete udělat přímo v deklarativním označení nebo prostřednictvím okna Vlastnosti při použití návrháři upravit šablonu rozhraní.

Kromě ověřování ovládacích prvků, tlačítka a tlačítka související ovládací prvky v technologii ASP.NET 2.0 také zahrnout `ValidationGroup` vlastnost. Kontroluje validátory skupiny ověření platnosti pouze při zpětném odeslání vyvolané vrácením hodnoty tlačítko, které má stejnou `ValidationGroup` nastavení vlastnosti. Například v pořadí pro tlačítko pro vložení prvku DetailsView k aktivaci `InsertValidationControls` skupiny ověření, musíme nastavit CommandField `ValidationGroup` vlastnost `InsertValidationControls` (viz obrázek 14). Kromě toho nastavení prvku GridView společnosti CommandField `ValidationGroup` vlastnost `EditValidationControls`.


[![Sada ovládacím prvku DetailsView. vaší společnosti CommandField ValidationGroup vlastnost InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Obrázek 14**: nastavte ovládacím prvku DetailsView CommandField `ValidationGroup` vlastnost `InsertValidationControls` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))


Po provedení těchto změn by měl vypadat nějak takto DetailsView a prvku GridView vlastností TemplateField a CommandFields:

Vlastností TemplateField a CommandField ovládacího prvku DetailsView.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField a vlastností TemplateField prvku GridView.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

Specifické pro úpravy validačních ovládacích prvků v tomto okamžiku vyvolat pouze při prvku GridView aktualizace po kliknutí na tlačítko a ovládací prvky typu ověřování specifických pro vložení, pouze v případě, že ovládacím prvku DetailsView vložit po kliknutí na tlačítko, vyřešení problému zvýrazněná obrázek 13. Však s touto změnou naše prvek se už nebude zobrazovat při zadávání neplatná data. Také obsahuje prvek `ValidationGroup` vlastnost a pouze zobrazuje souhrnné informace o těchto validačních ovládacích prvků ve skupině pro její ověření. Proto musíme mít dva validačních ovládacích prvků na této stránce, jeden pro `InsertValidationControls` skupiny ověření a jeden pro `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Tato uveďte v našem kurzu byla dokončena.

## <a name="summary"></a>Souhrn

Přestože BoundFields může poskytovat i vkládání a úpravy rozhraní, není rozhraní přizpůsobitelné. Chceme, aby běžně, přidání validačních ovládacích prvků pro úpravy a vložení rozhraní zajistit, aby uživatel zadal vstupy. platný formát. K tomu můžeme převést BoundFields vlastností TemplateField a přidání validačních ovládacích prvků do příslušné šablony. V tomto kurzu jsme příklad z rozšířené *zkoumání události spojené s vložení, aktualizace a odstranění* kurz, přidání validačních ovládacích prvků na ovládacím prvku DetailsView uživatele vkládání rozhraní a prvku GridView. úpravy rozhraní. Kromě toho jsme viděli, jak zobrazit informace o souhrnu ověření pomocí ovládacího prvku ValidationSummary a jak dělit validačních ovládacích prvků na stránce do různých ověřovacích skupin.

Jak jsme viděli v tomto kurzu, vlastností TemplateField povolit rozhraní úpravy a vložení potřeba rozšířit zahrnout validačních ovládacích prvků. Vlastností TemplateField je také možné rozšířit na další vstupní webové ovládací prvky, povolení textového pole bude nahrazen vhodnější webový ovládací prvek. V následujícím kurzem uvidíme, jak nahradit ovládacího prvku TextBox s ovládacím prvkem DropDownList vázané na data, která je ideální pro úpravy cizí klíč (například `CategoryID` nebo `SupplierID` v `Products` tabulky).

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Liz Shulok a Zack Jones. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [další](customizing-the-data-modification-interface-vb.md)
