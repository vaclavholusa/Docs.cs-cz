---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: "Přidání ovládacích prvků ověření do prvku DataList je úpravy rozhraní (VB) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu ukážeme, jak snadné je přidat ovládací prvky pro ověřování DataList EditItemTemplate aby poskytují více spolehlivá úpravy int. uživatele..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: b720c7704a9c44e60ed8a9ad1479558376fb5402
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Přidání ovládacích prvků ověření na rozhraní úpravy DataList (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) nebo [stáhnout PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> V tomto kurzu budete vidíte, jak snadné je přidat ovládací prvky pro ověřování DataList EditItemTemplate aby nabízí více spolehlivá úpravy uživatelské rozhraní.


## <a name="introduction"></a>Úvod

V DataList doposud úpravy kurzy nebyly DataLists úpravy rozhraní zahrnuta žádné ověření proaktivní uživatelského vstupu, i když neplatný uživatelského vstupu, například chybějící název produktu nebo záporné cena za následek výjimku. V [předchozí kurzu](handling-bll-and-dal-level-exceptions-vb.md) jsme se zaměřili na tom, jak přidat kód pro DataList s zpracování výjimek `UpdateCommand` obslužné rutiny události tak, aby bylo možné zachytit a řádně zobrazení informací o všechny výjimky, které byly vyvolány. V ideálním případě by však úpravy rozhraní bude zahrnovat prvky ověřování k uživateli zabránit v přechodu do takové neplatná data na prvním místě.

V tomto kurzu ukážeme, jak je snadné přidání ovládacích prvků ověření DataList s `EditItemTemplate` Chcete-li zadat více spolehlivá úpravy uživatelské rozhraní. Konkrétně tento kurz využívá příklad vytvořili v předchozím kurzu a rozšiřuje úpravy rozhraní, které chcete zahrnout odpovídající ověřování.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Krok 1: Replikace příklad z[zpracování výjimek BLL a DAL úroveň](handling-bll-and-dal-level-exceptions-vb.md)

V [zpracování BLL - a výjimky na úrovni DAL](handling-bll-and-dal-level-exceptions-vb.md) kurzu jsme vytvořili stránky, který uvedené názvy a ceny produktů v DataList dva sloupce, upravovat. Naším cílem pro tento kurz je k posílení DataList s úpravy rozhraní zahrnout ovládací prvky pro ověřování. Konkrétně se ověřovací logiku:

- Vyžadovat, že je třeba zadat název produktu s
- Zkontrolujte, zda je hodnota zadaná pro cenu formátu platný měny
- Ujistěte se, že hodnota zadaná pro cenu je větší než nebo rovna hodnotě nula, protože jako záporné `UnitPrice` hodnota je neplatná

Podíváme můžete na předchozí příklad zahrnout ověření rozšířit, nejprve musíme replikovat příklad z `ErrorHandling.aspx` stránku `EditDeleteDataList` složky na stránku pro účely tohoto kurzu `UIValidation.aspx`. Dosáhnout budeme muset zkopírovat přes i `ErrorHandling.aspx` stránky s deklarativní a jeho zdrojový kód. Nejdřív zkopírujte přes deklarativní provedením následujících kroků:

1. Otevřete `ErrorHandling.aspx` stránka v sadě Visual Studio
2. Přejděte na stránku s deklarativní (kliknutím na tlačítko zdroj v dolní části stránky)
3. Zkopírujte text v rámci `<asp:Content>` a `</asp:Content>` značky (čáry 3 až 32), jako podle obrázku 1.


[![Zkopírujte Text v &lt;asp: obsah&gt; ovládací prvek](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Obrázek 2**: kopírování textu v `<asp:Content>` ovládací prvek ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Otevřete `UIValidation.aspx` stránky
2. Přejděte na stránku s deklarativní
3. Sem vložte text, v rámci `<asp:Content>` ovládacího prvku.

Pokud chcete zkopírovat přes zdrojový kód, otevřete `ErrorHandling.aspx.vb` stránky a zkopírujte pouze text *v rámci* `EditDeleteDataList_ErrorHandling` – třída. Zkopírujte tři události obslužné rutiny (`Products_EditCommand`, `Products_CancelCommand`, a `Products_UpdateCommand`) spolu s `DisplayExceptionDetails` metoda, ale nezadávejte **není** zkopírujte deklaraci třídy nebo `using` příkazy. Vložte zkopírovaný text *v rámci* `EditDeleteDataList_UIValidation` třídy v `UIValidation.aspx.vb`.

Po přesunutí nad obsah a kód z `ErrorHandling.aspx` k `UIValidation.aspx`, za chvíli k otestování stránky v prohlížeči. Měli byste se stejný výstup a mít stejnou funkcionalitu v každé z těchto dvou stránkách (viz obrázek 2).


[![Stránka UIValidation.aspx napodobuje funkci v ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Obrázek 2**: `UIValidation.aspx` stránky napodobuje funkci v `ErrorHandling.aspx` ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Krok 2: Přidání ovládacích prvků ověření do DataList s EditItemTemplate

Při vytváření formuláře pro zadávání dat, je důležité, že uživatelé zadají všechna povinná pole a zda jejich zadané vstupy jsou hodnoty právní, správně naformátován. Chcete-li vám mohou pomoci zajistit, že vstupy uživatele s platnou, technologie ASP.NET poskytuje pět integrované ověření ovládacích prvků, které slouží k ověření hodnoty jeden vstupní ovládací prvek webu:

- [RequiredFieldValidator](https://msdn.microsoft.com/en-us/library/5hbw267h(VS.80).aspx) zajistí, že byla zadána hodnota
- [CompareValidator](https://msdn.microsoft.com/en-us/library/db330ayw(VS.80).aspx) ověří hodnotu s jinou hodnotou webové ovládací prvek nebo konstantní hodnotu nebo zajišťuje, že formátu hodnot s právní pro zadaný datový typ
- [RangeValidator](https://msdn.microsoft.com/en-us/library/f70d09xt.aspx) zajistí, že je hodnota v rozsahu hodnot
- [RegularExpressionValidator](https://msdn.microsoft.com/en-US/library/eahwtc9e.aspx) hodnotu porovnává [regulární výraz](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/en-us/library/9eee01cx(VS.80).aspx) hodnotu porovnává vlastní, uživatelsky definované metoda

Další informace o těchto pět ovládacích prvků odkazuje zpět na [přidání ověřovací ovládací prvky pro úpravy a vkládání rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) kurzu nebo rezervaci [část ovládací prvky pro ověřování](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) z [ASP.NET Quickstart kurzy](https://quickstarts.asp.net).

V našem kurzu budeme potřebovat použít RequiredFieldValidator zajistit, aby byla zadána hodnota pro název produktu a CompareValidator zajistit, že zadaná cena má hodnotu větší než nebo rovna 0 a se zobrazí ve formátu měny platný.

> [!NOTE]
> Při ASP.NET 1.x měl tyto stejné pět ověřovací ovládací prvky, technologii ASP.NET 2.0 přidala množství vylepšení, hlavní dva skripty na straně klienta se podpora pro prohlížeče kromě aplikace Internet Explorer a možnost ověření oddílu ovládacích prvků na stránce do ověření skupiny. Další informace o nových funkcí řízení ověření v 2.0, najdete v části [Rozvěrače ověřovacích ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Umožní s začněte tím, že přidání ovládacích prvků potřeby ověření do DataList s `EditItemTemplate`. Tuto úlohu lze provést pomocí návrháře kliknutím na odkaz Upravit šablony z DataList s inteligentním nebo využijte deklarativní syntaxi. Umožní krok s procesem, pomocí možnosti Upravit šablony ze zobrazení návrhu. Po výběru upravit DataList s `EditItemTemplate`, jeho po přidání RequiredFieldValidator přetažením z panelu nástrojů do rozhraní úpravy šablony umístění `ProductName` textové pole.


[![Přidání RequiredFieldValidator do EditItemTemplate po ProductName TextBox](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Obrázek 3**: přidejte RequiredFieldValidator k `EditItemTemplate After` `ProductName` textové pole ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Všechny ovládací prvky ověřování práci ověření vstupu jeden ovládací prvek ASP.NET Web. Proto potřebujeme k označení, že by měl RequiredFieldValidator, kterou jsme právě přidali vyhodnotit proti `ProductName` textové pole; to se provádí nastavením prvku ověřování s [ `ControlToValidate` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) k `ID` z vhodný ovládací prvek webové (`ProductName`, u této instance). Dále nastavte [ `ErrorMessage` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) chcete je nutné zadat název produktu s a [ `Text` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) k \*. `Text` Hodnota vlastnosti, pokud je zadán, je text, který se zobrazí ověření ovládacím prvkem, pokud se ověření nezdaří. `ErrorMessage` Hodnotu vlastnosti, který je vyžadován, je používán ovládacího prvku ValidationSummary; Pokud `Text` hodnota vlastnosti je vynechán, `ErrorMessage` hodnotu vlastnosti se zobrazí ovládací prvek ověřování v neplatný vstup.

Po nastavení tyto tři vlastnosti RequiredFieldValidator, by měla vypadat podobně jako na obrázku 4 obrazovky.


[![Nastavení RequiredFieldValidator s ControlToValidate, ErrorMessage a vlastnosti textu](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Obrázek 4**: nastavte RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`, a `Text` vlastnosti ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


S RequiredFieldValidator přidán do `EditItemTemplate`, všechny, že zůstanou je přidání potřeby ověření za cenu produktu s textové pole. Vzhledem k tomu `UnitPrice` je volitelný při úpravě záznam, jsme nejsou zobrazeny t potřeba přidat RequiredFieldValidator. , Ale potřebujete přidat CompareValidator zajistit, aby `UnitPrice`, pokud je zadaný, správně naformátován jako měny a je větší než nebo rovna 0.

Přidat CompareValidator do `EditItemTemplate` a nastavit jeho `ControlToValidate` vlastnost `UnitPrice`, jeho `ErrorMessage` vlastnost, která má za cenu musí být větší než nebo roven nule a nesmí obsahovat symbolu měny a jeho `Text` vlastnost \*. Označuje, zda `UnitPrice` hodnota musí být větší než nebo rovna 0, nastavte CompareValidator s [ `Operator` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) k `GreaterThanEqual`, jeho [ `ValueToCompare` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) na hodnotu 0, a jeho [ `Type` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) k `Currency`.

Po přidání ovládacích prvků tyto dvě ověření, DataList s `EditItemTemplate` s deklarativní syntaxe by měl vypadat podobně jako následující:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Po provedení těchto změn, otevřete stránku v prohlížeči. Pokud se pokusíte vynechat název nebo zadejte hodnotu ceny neplatný při úpravě produktu, se zobrazí vedle textového pole hvězdičku. Jak je vidět na obrázku 5, cena hodnotu, která zahrnuje symbol měny, jako je například 19,95 je považovány za neplatné. CompareValidator s `Currency` `Type` umožňuje oddělovačů číslice (například čárky a tečky, v závislosti na nastavení jazykové verze) a začátku plus nebo minus, ale nemá *není* povolit symbolu měny. Toto chování může perplex uživatelů, jak rozhraní úpravy aktuálně vykreslí `UnitPrice` formátu měny.


[![U textových polí s neplatný vstup se zobrazuje hvězdička](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Obrázek 5**: hvězdičky se zobrazí další textová pole s neplatný vstup ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Při ověření funguje jako-je, že uživatel musí ručně odebrat symbolu měny při úpravě záznam, který není platný. Kromě toho pokud jsou neplatné vstupy v úpravu rozhraní ani aktualizace ani Storno tlačítka, po kliknutí na, vyvolá zpětné volání. V ideálním případě by na tlačítko Storno vrátit DataList stavu předem úpravy bez ohledu na jeho platnost vstupů uživatele s. Navíc potřebujeme zajistit, že stránka s data jsou platná před aktualizací informace o produktu v DataList s `UpdateCommand` obslužné rutiny události, jako ověřovací logiku na straně klienta lze obejít, uživatelé, jejichž prohlížeče nejsou zobrazeny nepodporuje JavaScript nebo mít ovládací prvky podporuje zakázána.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Textové pole UnitPrice EditItemTemplate s odebráním symbolu měny

Při použití CompareValidator s `Currency``Type`, vstup ověřován nesmí obsahovat symboly měny. Uvedení těchto symbolů způsobí, že CompareValidator označit vstupu jako neplatné. Ale naše úpravy rozhraní aktuálně obsahuje symbolu měny v `UnitPrice` textovému poli, což znamená, uživatel musí explicitně odstranit symbolu měny před uložením jejich změny. Chcete-li to opravit máme tři možnosti:

1. Konfigurace `EditItemTemplate` tak, aby `UnitPrice` hodnotu pole TextBox není formátu měny.
2. Povolit, aby uživatel zadal symbolu měny odebráním CompareValidator a nahraďte ji RegularExpressionValidator, která vyhledává hodnotu měny správně formátovaný. Výzvy v tomto poli je, že regulární výraz k ověření hodnotu měny není stejně jednoduché jako CompareValidator a by vyžadovaly psaní kódu, pokud jsme chtěli začlenit nastavení jazykové verze.
3. Úplně odebrat ovládací prvek ověřovací a spoléhá na vlastní ověřování na straně serveru logiku GridView s `RowUpdating` obslužné rutiny události.

Umožní s pomocí možnost 1 pro účely tohoto kurzu. Aktuálně `UnitPrice` je formátováno jako hodnotu měny z důvodu ve výrazu datové vazby v textové pole `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Změna `Eval` příkaz, který má `Eval("UnitPrice", "{0:n2}")`, který zformátuje výsledek jako číslo s dvě číslice přesnosti. To lze provést přímo pomocí deklarativní syntaxe, nebo kliknutím na odkaz Upravit datové vazby z `UnitPrice` textového pole v DataList s `EditItemTemplate`.

Díky této změně formátovaný cenu v úpravy rozhraní zahrnuje čárky jako oddělovač skupin a jako oddělovač desetinných míst, ale ponechá vypnout symbolu měny.

> [!NOTE]
> Při odebírání formátu měny z rozhraní upravovat, I užitečné pro umístění symbolu měny jako text mimo textové pole. Tato stránka slouží jako nápovědu pro uživatele, který není potřeba zadat symbolu měny.


## <a name="fixing-the-cancel-button"></a>Oprava tlačítko Zrušit

Ve výchozím nastavení ovládací prvky webového ověření emitování JavaScript k provedení ověření na straně klienta. Když po kliknutí na tlačítko, LinkButton nebo ImageButton, se kontroluje ověřovací ovládací prvky na stránce na straně klienta, předtím, než dojde k zpětné volání. Pokud je neplatná data, je zrušená zpětné volání. Pro určité tlačítka ale platnost dat může být nepodstatné; v takovém případě je s zpětné volání je zrušená z důvodu neplatných dat nepříjemnosti.

Tlačítko Zrušit je takový příklad. Představte si, že uživatel zadá neplatná data, jako je například vynechání název produktu s rozhodne she nemá t chcete uložit produktu po všech a přístupy na tlačítko Storno. V současné době se aktivuje tlačítko Zrušit ověření ovládací prvky na stránce, které ohlásit že chybí název produktu a zabránit zpětné volání. Zadejte text do našich uživatel musí `ProductName` TextBox jenom k zrušit mimo proces úpravy.

Naštěstí tlačítko, LinkButton a ImageButton mají [ `CausesValidation` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , můžete určit, zda kliknutím na tlačítko by měla iniciovat logiku ověření (výchozí nastavení `True`). Nastavit s tlačítkem Storno `CausesValidation` vlastnost `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Zajištění vstupní hodnoty jsou platné v UpdateCommand obslužnou rutinu události

Z důvodu klientský skript vygenerované v ovládacích prvcích ověření, pokud uživatel zadá neplatný vstup ovládací prvky ověřování zrušte všechny postback iniciovaná tlačítko LinkButton, nebo ImageButton ovládací prvky, jejichž `CausesValidation` vlastnosti jsou `True` (na Výchozí nastavení). Pokud návštěvy s antiquated prohlížeč nebo některá jejichž JavaScript podpora byla zakázána, nebude provést kontroly ověřování na straně klienta.

Všechny ovládací prvky ASP.NET ověření zopakujte jejich logiku ověření okamžitě po zpětné volání a sestav celkové platnost vstupy stránky s prostřednictvím [ `Page.IsValid` vlastnost](https://msdn.microsoft.com/en-us/library/system.web.ui.page.isvalid.aspx). Však není tok stránky přeruší nebo byla zastavena v žádném způsobem na základě hodnoty z `Page.IsValid`. Jako vývojáři, je zajistit, aby naše odpovědnost `Page.IsValid` vlastnost má hodnotu `True` před pokračováním kód, který se předpokládá, platné vstupní data.

Pokud má uživatel zakázán JavaScript, navštíví naši stránku, upraví produktu, zadá hodnotu ceny příliš nákladné a kliknutím na tlačítko Aktualizovat, bude možné obejít ověřování straně klienta a výsledkem by měla být zpětné volání. Na zpětné volání, ASP.NET stránky s `UpdateCommand` spustí obslužnou rutinu události a dojde k výjimce při pokusu o analýzu příliš nákladné `Decimal`. Vzhledem k tomu, že máme výjimek, budou řádně zpracovávat takové výjimky, ale jsme může zabránit neplatná data uklouznutí prostřednictvím na prvním místě ve pouze budete pokračovat s `UpdateCommand` obslužné rutiny události Pokud `Page.IsValid` má hodnotu `True`.

Přidejte následující kód do začátku `UpdateCommand` obslužné rutiny události, bezprostředně před `Try` bloku:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Pomocí tohoto přidání produktu se pokusí aktualizovat pouze v případě, že odeslaná data jsou platná. Většina uživatelů won t moct odeslat zpět neplatná data z důvodu ověření ovládací prvky klientské skripty, ale uživatelé, jejichž prohlížeče nejsou zobrazeny nepodporuje JavaScript nebo které mají podporu jazyka JavaScript zakázaná, můžete vynechat kontroly na straně klienta a odeslat neplatná data.

> [!NOTE]
> Astute čtečky připomíná, která při aktualizaci dat s GridView, jsme dodán t potřeba explicitně zkontrolovat `Page.IsValid` vlastnost v třídě naše stránky s kódem v pozadí. Důvodem je, že zajímají GridView `Page.IsValid` vlastnost pro nás a pouze bude pokračovat v aktualizaci pouze v případě, že ji vrací hodnotu `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Krok 3: Shrnutí problémů položka dat

Kromě pěti ověřovací ovládací prvky, technologie ASP.NET obsahuje [ovládací prvek ValidationSummary](https://msdn.microsoft.com/en-US/library/f9h59855(VS.80).aspx), které zobrazuje `ErrorMessage` s kontrolní mechanismy ověřování, které zjištěna neplatná data. Tato souhrnná data mohou být zobrazeny jako text na webové stránce nebo prostřednictvím messagebox modální, na straně klienta. Umožní s vylepšit tento kurz a zahrnují klienta messagebox shrnutí problémy ověření.

K tomu, přetáhněte ovládací prvek ValidationSummary z panelu nástrojů na návrháře. Umístění t ValidationSummary ovládací prvek nemá opravdu důležité, protože jsme níž chcete konfigurovat jenom zobrazit souhrn jako messagebox. Po přidání ovládacího prvku, nastavte jeho [ `ShowSummary` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) k `False` a jeho [ `ShowMessageBox` vlastnost](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) k `True`. Pomocí tohoto přidání všechny chyby ověřování, jsou shrnuty v messagebox klienta (viz obrázek 6).


[![Chyby ověření jsou shrnuty v Messagebox na straně klienta](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Obrázek 6**: chyby ověření jsou shrnuty v Messagebox na straně klienta ([Kliknutím zobrazit obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak snížit pravděpodobnost výjimky proaktivně zajistěte, aby naše vstupy uživatele platný před pokusem o jejich použití v pracovním postupu aktualizace pomocí ovládacích prvků pro ověřování. Technologie ASP.NET poskytuje pět ověření webové ovládací prvky navržené tak, aby kontrolovat konkrétní webové řízení s vstup a zpětně hlásit vstupní s platnosti. V tomto kurzu jsme použili dva z těchto pět ovládacích prvků RequiredFieldValidator a CompareValidator zajistit, aby byl zadán název produktu s a že ceny měl formátu měny s hodnotou větší než nebo rovna hodnotě nula.

Přidání ovládacích prvků ověření do s DataList úpravy rozhraní je jednoduché, přetáhněte je na `EditItemTemplate` ze sady nástrojů a nastavení několik vlastností. Ve výchozím nastavení ovládací prvky ověřování automaticky emitování skriptu ověřování na straně klienta; obsahují taky ověřování na straně serveru na zpětné volání, ukládání kumulativní výsledek v `Page.IsValid` vlastnost. Chcete-li při kliknutí na tlačítko, LinkButton nebo ImageButton, obejít ověřování straně klienta, nastavte tlačítko s `CausesValidation` vlastnost `False`. Navíc před provedením jakékoli úlohy s daty odeslaného na zpětné volání, zkontrolujte, zda `Page.IsValid` vlastnost vrátí `True`.

Všechny DataList úpravy kurzy jsme zkontrolován, pokud jste předtím velmi jednoduché úpravy rozhraní textové pole pro název produktu s a druhý pro cenu. Úpravy rozhraní, ale může obsahovat kombinaci různých ovládací prvky webového, jako je například DropDownLists, kalendáře, přepínací tlačítka, zaškrtávací políčka a tak dále. V našem kurzu další podíváme vytváření rozhraní, které používá celou řadu ovládacích prvků.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontroloři pro účely tohoto kurzu byly společnosti Dennis Patterson, Ken Pespisa a Liz Shulok. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](handling-bll-and-dal-level-exceptions-vb.md)
[další](customizing-the-datalist-s-editing-interface-vb.md)
