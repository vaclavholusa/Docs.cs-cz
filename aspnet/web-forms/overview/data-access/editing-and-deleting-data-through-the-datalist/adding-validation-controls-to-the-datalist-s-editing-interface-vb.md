---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Přidání validačních ovládacích prvků DataList uživatele úpravy rozhraní (VB) | Dokumentace Microsoftu
author: rick-anderson
description: V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků do prvku DataList EditItemTemplate negace spolehlivá víc úpravy int. uživatele...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 6fe5fcba322f3d3a37b862f0a85810d8b4dda5f4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752208"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>Přidání validačních ovládacích prvků do rozhraní úpravy prvku DataList (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) nebo [stahovat PDF](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků do prvku DataList EditItemTemplate negace spolehlivá víc úpravy uživatelského rozhraní.


## <a name="introduction"></a>Úvod

V ovládacím prvku DataList úpravy kurzy doposud DataLists úpravy rozhraní nezahrnuli žádné ověření vstupu uživatele proaktivní i v případě, že Neplatný uživatelský vstup například chybějící název produktu nebo záporná cena za následek výjimku. V [předchozím kurzu](handling-bll-and-dal-level-exceptions-vb.md) jsme se zaměřili na tom, jak přidat kód do prvku DataList s zpracování výjimek `UpdateCommand` obslužná rutina události, aby bylo možné zachytit a řádně zobrazení informací o žádné výjimky, které byly vyvolány. V ideálním případě by však úpravy rozhraní bude zahrnovat validačních ovládacích prvků, chcete-li zabránit uživateli v zadávání těchto dat na prvním místě.

V tomto kurzu uvidíme, jak snadné je přidání validačních ovládacích prvků DataList s `EditItemTemplate` negace spolehlivá víc úpravy uživatelského rozhraní. Konkrétně v tomto kurzu používá příklad vytvořili v předchozím kurzu a argumentech rozhraní úprav zahrnout příslušné ověření.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>Krok 1: Replikace z příkladu[zpracování výjimek na úrovni knihoven BLL a DAL](handling-bll-and-dal-level-exceptions-vb.md)

V [zpracování knihoven BLL a DAL úrovni výjimky](handling-bll-and-dal-level-exceptions-vb.md) kurzu jsme vytvořili stránku, která uvedené názvy a ceny produktů v dvěma sloupci, upravitelné DataList. Naším cílem pro účely tohoto kurzu je k posílení DataList s úpravy rozhraní zahrnout validačních ovládacích prvků. Konkrétně se naše logiku ověřování:

- Vyžadovat, aby být zadaný název produktu s
- Ujistěte se, že hodnota zadaná pro cenu formátu platné měny
- Ujistěte se, že hodnota zadaná pro cena je větší než nebo rovna hodnotě nula, od záporné `UnitPrice` hodnota je neplatná

Předtím, než abychom se mohli podívat na rozšíření předchozího příkladu na zahrnout ověřování, musíme nejprve příklad z replikace `ErrorHandling.aspx` stránku `EditDeleteDataList` složky na stránku pro účely tohoto kurzu `UIValidation.aspx`. Dosáhnout musíme zkopírovat i `ErrorHandling.aspx` stránka s deklarativní a jeho zdrojový kód. Nejprve zkopírovat deklarativní provedením následujících kroků:

1. Otevřít `ErrorHandling.aspx` stránky v sadě Visual Studio
2. Přejděte na stránku s deklarativní (kliknutím na tlačítko zdroj v dolní části stránky)
3. Zkopírujte text v rámci `<asp:Content>` a `</asp:Content>` značky (čáry 3 až 32), jako podle obrázku 1.


[![Zkopírujte Text v &lt;asp: Content&gt; ovládacího prvku](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Obrázek 2**: Zkopírujte Text v `<asp:Content>` ovládacího prvku ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Otevřít `UIValidation.aspx` stránky
2. Přejděte na stránku s deklarativní
3. Vložit text v rámci `<asp:Content>` ovládacího prvku.

Chcete-li zkopírovat zdrojový kód, otevřete `ErrorHandling.aspx.vb` stránce a zkopírujte pouze text *v rámci* `EditDeleteDataList_ErrorHandling` třídy. Zkopírujte tři obslužné (`Products_EditCommand`, `Products_CancelCommand`, a `Products_UpdateCommand`) spolu s `DisplayExceptionDetails` metody, ale nechcete **není** zkopírujte deklaraci třídy nebo `using` příkazy. Vložit zkopírovaný text *v rámci* `EditDeleteDataList_UIValidation` třídy v `UIValidation.aspx.vb`.

Po přesunutí nad obsah a kód z `ErrorHandling.aspx` k `UIValidation.aspx`, využít k otestování stránky v prohlížeči. By měl zobrazit stejný výstup a vyzkoušet stejně v každém z těchto dvou stránkách (viz obrázek 2).


[![Na stránce UIValidation.aspx napodobuje funkce v ErrorHandling.aspx](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Obrázek 2**: `UIValidation.aspx` stránky napodobuje funkce v `ErrorHandling.aspx` ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Krok 2: Přidání validačních ovládacích prvků do DataList s EditItemTemplate

Při vytváření formuláře pro zadávání dat, je důležité, aby uživatelé zadali všechna povinná pole a že jejich zadané vstupy jsou hodnoty právní, správně naformátovaná. K zajištění, že jsou platné uživatelské vstupy s technologie ASP.NET poskytuje pět ověření integrované ovládací prvky, které slouží k ověření hodnoty jednoho vstupního ovládacího prvku webové:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) zajistí, že byl poskytnut hodnotu
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) ověřuje hodnoty s jinou hodnotou ovládací prvek webové nebo konstantní hodnotu, nebo zajistí, že hodnota s formát není platný pro zadaný datový typ
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) zajistí, že je hodnota v rozsahu hodnot
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) hodnotu porovnává [regulárního výrazu](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) hodnotu porovnává vlastní, uživatelem definované metody

Další informace o těchto pět ovládacích prvků vrátit zpět k [přidání validačních ovládacích prvků pro úpravy a vložení rozhraní](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) kurzu nebo si [validačních ovládacích prvků části](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) z [Kurzy rychlý start ASP.NET](https://quickstarts.asp.net).

V našem kurzu jsme budete muset použít RequiredFieldValidator zajistit, že byla zadána hodnota pro název produktu a CompareValidator zajistíte, že zadaná cena má hodnotu větší než nebo rovna 0 a je uvedené ve formátu platné měny.

> [!NOTE]
> Zatímco ASP.NET 1.x měla tyto stejné pět validačních ovládacích prvků, technologii ASP.NET 2.0 bylo přidáno několik vylepšení, hlavní dvou skriptů na straně klienta se podporují pro prohlížeče kromě aplikace Internet Explorer a schopnost oddílu validačních ovládacích prvků na stránce do ověření skupiny. Další informace o nových funkcích ovládacího prvku ověření ve verzi 2.0, přečtěte si [rozbor validačních ovládacích prvků v technologii ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


Umožní s spustit tak, že přidáte nezbytné validačních ovládacích prvků DataList s `EditItemTemplate`. Tuto úlohu lze provést prostřednictvím návrháře kliknutím na odkaz Upravit šablony v prvku DataList s inteligentním nebo využijte deklarativní syntaxi. Umožní krok s procesem, pomocí možnosti Upravit šablony ze zobrazení návrhu. Po výběru pro úpravy prvku DataList s `EditItemTemplate`, přidejte RequiredFieldValidator jeho přetažením z panelu nástrojů do rozhraní pro úpravy šablony, ho po uvedení `ProductName` textového pole.


[![Přidat RequiredFieldValidator do EditItemTemplate po ProductName textového pole](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Obrázek 3**: Přidat RequiredFieldValidator k `EditItemTemplate After` `ProductName` textového pole ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Všechny ovládací prvky ověřování pracujte ověření vstupu jeden ovládací prvek technologie ASP.NET. Proto potřebujeme k označení, že RequiredFieldValidator jsme právě přidali, měli ověřit proti `ProductName` textové pole; to provádí nastavením ověřovací ovládací prvek s [ `ControlToValidate` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) k `ID` z odpovídající ovládací prvek webového (`ProductName`, v tomto případě). Dále nastavte [ `ErrorMessage` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) k je nutné zadat název produktu s a [ `Text` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) k \*. `Text` Hodnota vlastnosti, pokud je zadán, je text, který se zobrazí v ovládacím prvku ověření, pokud se ověření nezdaří. `ErrorMessage` Hodnotu vlastnosti, které je potřeba, se používá prvek; Pokud `Text` hodnota vlastnosti je vynechán, `ErrorMessage` hodnota vlastnosti se zobrazí při ověřování ovládacího prvku na neplatný vstup.

Po nastavení tyto tři vlastnosti RequiredFieldValidator, vaše obrazovka by měla vypadat podobně jako na obrázku 4.


[![Nastavení RequiredFieldValidator s ControlToValidate, chybová zpráva a vlastnosti textu](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Obrázek 4**: Set RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`, a `Text` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


S RequiredFieldValidator přidán do `EditItemTemplate`, all, zůstane je přidání nezbytné ověření za cenu produktů s textového pole. Vzhledem k tomu, `UnitPrice` je volitelný při úpravě záznamu, budeme zadávat t nutné přidat RequiredFieldValidator. , Ale potřebujeme přidat CompareValidator zajistit, aby `UnitPrice`, je-li zadán, je ve správném formátu jako měnu a je větší než nebo rovna 0.

Přidat CompareValidator do `EditItemTemplate` a nastavte jeho `ControlToValidate` vlastnost `UnitPrice`, jeho `ErrorMessage` vlastnost s cenou při musí být větší než nebo rovna nule a nemůže obsahovat symbol měny a jeho `Text` vlastnost \*. Označuje, že `UnitPrice` hodnota musí být větší než nebo rovno 0, nastavte CompareValidator s [ `Operator` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) k `GreaterThanEqual`, jeho [ `ValueToCompare` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) na hodnotu 0, a jeho [ `Type` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) k `Currency`.

Po přidání těchto dvou validačních ovládacích prvků, DataList s `EditItemTemplate` s deklarativní syntaxe by měl vypadat nějak takto:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Po provedení těchto změn, otevřete stránku v prohlížeči. Při pokusu vynechat název nebo zadejte hodnotu Neplatná cena při úpravách produktu, se zobrazí hvězdičku vedle textového pole. Jak je vidět na obrázku 5, hodnotu cena, která obsahuje symbol měny, jako je například 19,95 je považovány za neplatné. CompareValidator s `Currency` `Type` umožňuje oddělovače číslic (například mezery nebo období, v závislosti na nastavení jazykové verze) a předních plus nebo mínus, ale nemá *není* povolit symbol měny. Toto chování může perplex uživatelů, jak rozhraní úprav aktuálně vykreslí `UnitPrice` formátu měny.


[![Hvězdička se zobrazí vedle textových polí s neplatný vstup](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Obrázek 5**: hvězdička se zobrazí další textová pole s neplatný vstup ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


Při ověřování funguje jako-se, uživatel musí při úpravách záznam, který není přijatelné, ručně odeberte symbol měny. Kromě toho pokud jsou neplatné vstupy úpravy rozhraní ani aktualizace ani zrušit tlačítka, po kliknutí na, vyvolá zpětné volání. Tlačítko Storno v ideálním případě by vrátil prvku DataList do stavu před úprav bez ohledu na jeho platnost uživatelské vstupy s. Potřebujeme také, ujistěte se, že data na stránce s platnou před aktualizací informace o produktech v ovládacím prvku DataList s `UpdateCommand` obslužná rutina události, jako ovládací prvky ověřování uživatelů, jejichž prohlížeče zadávat t podporu jazyka JavaScript nebo mít lze vynechat logiku na straně klienta svou podporu zakázán.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Odebrání Symbol měny z textového pole UnitPrice EditItemTemplate s

Při použití CompareValidator s `Currency``Type`, se ověřují vstupní nesmí obsahovat žádné symboly měny. Přítomnost tyto symboly způsobí, že CompareValidator pro označení vstupu jako neplatný. Ale naše úpravy rozhraní aktuálně obsahuje symbol měny v `UnitPrice` textové pole, což znamená, uživatel musí explicitně odstranit symbol měny před uložením změn. Chcete-li to napravit máme tři možnosti:

1. Konfigurace `EditItemTemplate` tak, aby `UnitPrice` hodnotu pole TextBox není ve formátu jako měnu.
2. Povolit, aby uživatel zadal symbol měny odebráním CompareValidator a jeho nahrazení atributem RegularExpressionValidator, který kontroluje hodnotu správně formátovaná měny. Před obrovskou výzvou – tady je, že regulární výraz k ověření hodnotu měny není tak přímočaré jako CompareValidator a bude vyžadovat psaní kódu, pokud jsme chtěli začlenit nastavení jazykové verze.
3. Úplně odebrat ovládací prvek ověření a Spolehněte se na logiku vlastního ověřování na straně serveru v prvku GridView s `RowUpdating` obslužné rutiny události.

Umožní s využít možnost 1 pro účely tohoto kurzu. Aktuálně `UnitPrice` formátována jako hodnota měny z důvodu výraz datové vazby pro textové pole v `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Změnit `Eval` příkazu `Eval("UnitPrice", "{0:n2}")`, který zformátuje výsledek jako číslo s dvěma číslicemi přesnosti. To můžete udělat přímo pomocí deklarativní syntaxe nebo kliknutím na odkaz upravit vlastnosti DataBindings z `UnitPrice` textového pole v ovládacím prvku DataList s `EditItemTemplate`.

Díky této změně formátovaný cena úpravy rozhraní zahrnuje čárky jako oddělovače skupin a tečku jako oddělovač desetinných míst, ale ponechá vypnout symbol měny.

> [!NOTE]
> Při odebírání formát měny z rozhraní upravovat, můžu být užitečné pro umístění symbolu měny jako text mimo textového pole. Tato stránka slouží jako Nápověda pro uživatele, který není potřeba poskytovat symbol měny.


## <a name="fixing-the-cancel-button"></a>Oprava tlačítka Storno

Ve výchozím nastavení ovládací prvky webového ověření generování JavaScript, který chcete provést ověření na straně klienta. Po kliknutí na tlačítko, odkazem (LinkButton) nebo ImageButton je validačních ovládacích prvků na stránce jsou kontrolovány na straně klienta, než dojde k zpětné volání. Pokud je neplatná data, dojde ke zrušení zpětného odeslání. U některých tlačítek ale platnost dat může být nepodstatné; v takovém případě je mít postback zrušena v důsledku neplatných dat jaké části.

Tlačítko Zrušit je takových příkladů. Představte si, že uživatel zadá neplatná data, jako je například vynechání názvu produktu s rozhodne she kódu t chcete po uložení produktu a narazí na tlačítko Storno. V současné době se aktivuje tlačítko Storno validačních ovládacích prvků na stránce, která zprávu, že chybí název produktu a zabránit zpětné volání. Zadejte nějaký text do našich uživatel musí `ProductName` TextBox jen pro zrušení úprav mimoprocesové.

Naštěstí mít tlačítko, odkazem (LinkButton) a ImageButton [ `CausesValidation` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , který můžete určit, zda kliknutím na tlačítko by mělo zahájit logiku ověřování (výchozí hodnota je `True`). Nastavení s tlačítkem Storno `CausesValidation` vlastnost `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Zajištění vstupy jsou platné v obslužné rutině události UpdateCommand

Z důvodu skript na straně klienta, které validačních ovládacích prvků, pokud uživatel zadá platný vstup validačních ovládacích prvků zrušit všechny postbacků iniciovaných tlačítko, odkazem (LinkButton), nebo ImageButton ovládací prvky, jejichž `CausesValidation` vlastnosti jsou `True` () Výchozí nastavení). Pokud je uživatel navštívit pomocí antiquated prohlížeče, nebo jejichž podpora JavaScriptu se zakázalo, nebude spuštěno kontroly ověřování na straně klienta.

Všechny ovládací prvky ověřování ASP.NET zopakujte jejich logiku ověřování ihned po zpětného odeslání a tvorba sestav celkového platnosti vstupy stránky s prostřednictvím [ `Page.IsValid` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Však není stránka flow přerušení nebo zastavení v libovolném způsobem na základě hodnoty z `Page.IsValid`. Jako vývojáři, je našich povinností ujistit se, `Page.IsValid` vlastnost má hodnotu `True` předtím, než budete pokračovat s kódem, který předpokládá platný vstupní data.

Pokud má uživatel zakázaný JavaScript, naši stránku navštíví, upraví produktu, zadá hodnotu ceny příliš drahé a klikne na tlačítko Aktualizovat ověřování na straně klienta se přeskočí a bude následovat zpětného odeslání. Při zpětném odeslání stránky s ASP.NET `UpdateCommand` spustí obslužnou rutinu události a je vyvolána výjimka při pokusu o analýzu příliš drahé `Decimal`. Protože máme zpracování výjimek, budou bez výpadku zpracovávat takové výjimky, ale jsme může zabránit neplatná data na prvním místě průniku prostřednictvím podle pouze pokračuje se `UpdateCommand` obslužná rutina události Pokud `Page.IsValid` má hodnotu `True`.

Přidejte následující kód do začátku `UpdateCommand` obslužná rutina události, bezprostředně před `Try` blok:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Uveďte se pokusí produktu aktualizovat pouze v případě, že odeslaná data je platný. Většina uživatelů vyhráli t moct odeslat zpět neplatných dat z důvodu skripty na straně klienta pro ovládací prvky ověření, ale uživatelé, jejichž prohlížeče zadávat t podporu jazyka JavaScript nebo obsahujících jazyka JavaScript podporují zakázaná, můžete vynechat kontroly na straně klienta a odeslat neplatná data.

> [!NOTE]
> Bystří čtenáři budou Vzpomeňte si, že při aktualizaci dat s použitím prvku GridView, jsme kód nefungoval nemusíte explicitně zkontrolovala `Page.IsValid` vlastnost ve třídě použití modelu code-behind naší stránce s. Důvodem je, že prvku GridView consults `Page.IsValid` vlastnost pro nás a pouze pokračovat aktualizace pouze v případě, že vrací hodnotu `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>Krok 3: Shrnutí problémů položka dat

Kromě pěti ověřovacích ovládacích prvků technologie ASP.NET obsahuje [ovládacího prvku ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), které ukazuje `ErrorMessage` s těmito validačních ovládacích prvků, které se zjistila neplatná data. Tato souhrnná data lze zobrazit jako text na webové stránce nebo přes pole messagebox modální, na straně klienta. Umožní s vylepšit tento kurz a zahrnout pole messagebox na straně klienta sumarizace jakékoli zaznamenané problémy s ověřením.

K tomu, přetáhněte ovládací prvek souhrnu ověření z panelu nástrojů do návrháře. Umístění t kódu ovládacího prvku ValidationSummary opravdu důležité, protože jsme re chcete nakonfigurovat jenom zobrazit souhrn jako pole messagebox. Po přidání ovládacího prvku, nastavte jeho [ `ShowSummary` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) k `False` a jeho [ `ShowMessageBox` vlastnost](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) k `True`. Uveďte, všechny chyby ověření jsou shrnuté v messagebox na straně klienta (viz obrázek 6).


[![Chyby ověření jsou shrnuté v Messagebox na straně klienta](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Obrázek 6**: The chyby ověření jsou shrnuté v Messagebox na straně klienta ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Souhrn

V tomto kurzu jsme viděli, jak snížit pravděpodobnost, že výjimky prostřednictvím validačních ovládacích prvků, které proaktivně zajistí, že naše vstupů uživatele jsou platné před pokusem o jejich použití v aktualizaci pracovního postupu. Technologie ASP.NET poskytuje pět ověření webové ovládací prvky, které jsou určeny ke kontrole konkrétní webové ovládací prvek s vstup a zpětně hlásit platnosti vstupní s. V tomto kurzu jsme použili dvě z těchto pět ovládacích prvků RequiredFieldValidator a CompareValidator zajistit, že byl zadán název produktu s a že cena měl formát měny s hodnotou větší než nebo rovna hodnotě nula.

Přidání validačních ovládacích prvků DataList s úpravy rozhraní je snadné – stačí na jejich přetažením `EditItemTemplate` z panelu nástrojů a nastavení na několik vlastností. Ve výchozím nastavení ovládací prvky ověřování automaticky vygenerovat skript pro ověřování na straně klienta obsahují taky ověřování na straně serveru na zpětné volání, ukládání kumulativní vést `Page.IsValid` vlastnost. Chcete-li obejít ověřování na straně klienta při kliknutí na tlačítko, odkazem (LinkButton) nebo obrázkové tlačítko, nastavte tlačítko s `CausesValidation` vlastnost `False`. Také, před provedením jakékoli úlohy s daty odeslaného na zpětné volání, ujistěte se, že `Page.IsValid` vrátí vlastnost `True`.

Všechny kurzy pro úpravy prvku DataList jsme ve prověřit, pokud jste využili velmi jednoduché úpravy rozhraní textové pole pro název produktu s a druhý pro cenu. Úpravy rozhraní, ale může obsahovat kombinaci různých ovládacích prvků, jako je například DropDownLists, kalendáře, přepínačů, zaškrtávacích políček a tak dále. V následujícím kurzem podíváme na vytváření, který používá celou řadu ovládacích prvků webového rozhraní.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí revidující pro účely tohoto kurzu byly Dennis Patterson, Ken Pespisa a Liz Shulok. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](handling-bll-and-dal-level-exceptions-vb.md)
> [další](customizing-the-datalist-s-editing-interface-vb.md)
