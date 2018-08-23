---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Přidání potvrzení na straně klienta při odstraňování (C#) | Dokumentace Microsoftu
author: rick-anderson
description: V rozhraní, která jsme dosud vytvořili uživatel může omylem odstranit data po kliknutí na tlačítko odstranit, pokud jsou určeny k klikněte na tlačítko Upravit. V tomto t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: b8cca6ece2eb008170192dc1774a6f88c1b37a21
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757129"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Přidání potvrzení na straně klienta při odstraňování (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) nebo [stahovat PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> V rozhraní, která jsme dosud vytvořili uživatel může omylem odstranit data po kliknutí na tlačítko odstranit, pokud jsou určeny k klikněte na tlačítko Upravit. V tomto kurzu přidáme dialogovému oknu potvrzení na straně klienta, který se zobrazí, když dojde ke kliknutí na tlačítko Odstranit.


## <a name="introduction"></a>Úvod

Za posledních několik kurzů jsme ve viděli, jak zajistit vložení, úpravy a odstranění možnosti použít naší aplikace architektury, ObjectDataSource a data webové ovládací prvky ve vzájemné součinnosti. Odstraňování jsme rozhraní ve prověřit, jaký se skládá z odstranění tlačítko, které při kliknutí na, vyvolá zpětné volání a vyvolá ObjectDataSource s `Delete()` metoda. `Delete()` Metoda potom vyvolá metodu nakonfigurované z vrstvy obchodní logiky, která rozšíří volání do vrstvy přístupu k datům, vydávání skutečnou `DELETE` příkaz do databáze.

Toto uživatelské rozhraní umožňuje návštěvníkům odstraňovat záznamy pomocí ovládacího prvku GridView, DetailsView nebo FormView ovládacích prvků, chybí jakýkoli druh potvrzení, když uživatel klikne na tlačítko Odstranit. Pokud uživatel klikne omylem při jejich účelem klikněte na tlačítko Upravit, bude místo toho odstranit záznam, který se má aktualizovat. Chcete-li tomu zabránit, v tomto kurzu přidáme dialogovému oknu potvrzení na straně klienta, který se zobrazí, když dojde ke kliknutí na tlačítko Odstranit.

JavaScript `confirm(string)` funkce zobrazuje svůj parametr vstupního řetězce jako text v modální dialogové okno, které součástí jsou dvě tlačítka – OK a zrušení (viz obrázek 1). `confirm(string)` Funkce vrátí hodnotu typu Boolean v závislosti na tom, jaké je stisknuto tlačítko (`true`, pokud uživatel klikne na tlačítko OK, a `false` Pokud klikněte na tlačítko Storno).


![JavaScript confirm(string) metoda zobrazí modální, Messagebox na straně klienta](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Obrázek 1**: JavaScript `confirm(string)` metoda zobrazí prvek Messagebox modální, na straně klienta


Při odeslání formuláře, pokud hodnota `false` vrátí obslužnou rutinu události na straně klienta a odeslání formuláře se zrušila. Díky této funkci můžete máme odstranění tlačítka s klientské `onclick` obslužná rutina události návratová hodnota volání `confirm("Are you sure you want to delete this product?")`. Pokud uživatel klikne na tlačítko Storno, `confirm(string)` vrátí false, a tím způsobující odesílání formuláře zrušit. S žádné zpětné volání, jehož tlačítko pro odstranění došlo ke kliknutí na produkt vyhráli t odstranit. Pokud však uživatel klikne na tlačítko OK v dialogovém okně potvrzení, zpětné volání bude dál nepokračuje a produkt se odstraní. Poraďte [s využitím jazyka JavaScript `confirm()` metody k odeslání formuláře ovládacího prvku](http://www.webreference.com/programming/javascript/confirm/) Další informace o této techniky.

Přidání nezbytné skriptu na straně klienta při použití šablony než při použití CommandField se mírně liší. Proto se v tomto kurzu se podíváme na příklad FormView i ovládacího prvku GridView.

> [!NOTE]
> Pomocí technik potvrzení na straně klienta, jako jsou popsané v tomto kurzu se předpokládá, že vaši uživatelé navštěvují v prohlížečích, které podporují jazyka JavaScript a, ke kterým mají povolen jazyk JavaScript. Pokud některý z těchto předpokladů neplatí pro konkrétního uživatele, kliknutím na tlačítko odstranit okamžitě vyvolávají zpětné odeslání (nejsou zobrazena pole messagebox potvrzení).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Krok 1: Vytvoření FormView, která podporuje odstranění

Začněte přidáním FormView k `ConfirmationOnDelete.aspx` stránku `EditInsertDelete` složku vytvoříte jejich vazbu na nový prvek ObjectDataSource, který získává informace o produktech přes zpět `ProductsBLL` třída s `GetProducts()` – metoda. Také nakonfigurovat prvku ObjectDataSource tak, aby `ProductsBLL` třída s `DeleteProduct(productID)` metoda je mapován na prvek ObjectDataSource s `Delete()` metody; Ujistěte se, že kartách INSERT, UPDATE a rozevírací seznamy jsou nastaveny na (žádný). A konečně zaškrtněte políčko Povolit stránkování ve třídě FormView s inteligentním.

Po provedení těchto kroků bude nový prvek ObjectDataSource s deklarativní vypadat nějak takto:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Stejně jako v naší poslední příklady, které nepoužili optimistického řízení souběžnosti, věnujte chvíli smažte ObjectDataSource s `OldValuesParameterFormatString` vlastnost.

Protože byla svázána se ovládací prvek ObjectDataSource, který podporuje pouze odstraňuje, FormView s `ItemTemplate` nabízí jenom tlačítko odstranit, ve kterém chybí tlačítka New a Update. FormView s deklarativní, ale obsahuje nadbytečnými `EditItemTemplate` a `InsertItemTemplate`, které je možné odebrat. Využít k přizpůsobení `ItemTemplate` tak, že se zobrazuje pouze podmnožinu produktu datová pole. Můžu uložit nakonfigurovaný dolování, aby se zobrazil název produktu s v `<h3>` záhlaví nad jeho dodavatele a kategorie názvy (spolu s tlačítko Odstranit).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

S těmito změnami máme plně funkční webovou stránku, která umožňuje uživatelům přepínat prostřednictvím produktů jeden najednou, s možností odstranit produkt jednoduše kliknutím na tlačítko Odstranit. Obrázek 2 ukazuje snímek obrazovky náš postup doposud při prohlížení prostřednictvím prohlížeče.


[![FormView s informacemi o jednoho produktu](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Obrázek 2**: The FormView zobrazuje informace o jeden produkt ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Krok 2: Volání z odstranění tlačítka Client-Side onclick události confirm(string) – funkce

S FormView vytvořili, posledním krokem je konfigurace tlačítko odstranit takové, že ho s kliknutí návštěvníkem jazyka JavaScript `confirm(string)` vyvolání funkce. Přidání skriptu na straně klienta tlačítko, odkazem (LinkButton) nebo ImageButton s klientským `onclick` událostí lze provést prostřednictvím `OnClientClick property`, což je nová technologie ASP.NET 2.0. Protože chceme hodnotu `confirm(string)` funkce vrátila, jednoduše nastavte tuto vlastnost na: `return confirm('Are you certain that you want to delete this product?');`

Po této změně deklarativní syntaxe s odkazem (LinkButton) odstranit by měla vypadat podobně jako:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Všechny existuje tento s je to! Obrázek 3 ukazuje snímek obrazovky toto potvrzení v akci. Kliknutím na tlačítko Odstranit zobrazí dialogové okno potvrzení. Pokud uživatel klikne na tlačítko Storno, zpětné volání se zrušila a produktu není odstraněn. Zpětné volání, pokud ale uživatel klikne na tlačítko OK, pokračuje a prvku ObjectDataSource s `Delete()` je vyvolána metoda ukončené odstranění záznamu v databázi.

> [!NOTE]
> Řetězec předaný do `confirm(string)` funkce JavaScript, která jsou odděleny apostrofy (spíše než uvozovky). V jazyce JavaScript může být oddělené řetězce buď znaku. Apostrofy tady používáme tak, aby oddělovače pro řetězec předat do `confirm(string)` nezavádí nejednoznačnost pomocí oddělovače pro `OnClientClick` hodnotu vlastnosti.


[![Potvrzení se teď zobrazují při kliknutím na tlačítko Odstranit](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Obrázek 3**: potvrzení A je teď zobrazují při kliknutím na tlačítko Odstranit ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Krok 3: Konfigurace vlastnost OnClientClick pro tlačítko Odstranit v CommandField

Při práci s tlačítko, odkazem (LinkButton) nebo ImageButton přímo v šabloně, dialogové okno potvrzení lze přidružit ho jednoduše nakonfigurováním jeho `OnClientClick` vlastnost pro vrácení výsledků z jazyka JavaScript `confirm(string)` funkce. Ale CommandField – které přidá pole odstranit tlačítek do ovládacího prvku GridView nebo prvku DetailsView. - nemá `OnClientClick` vlastnost, která lze nastavit deklarací. Místo toho jsme musí prostřednictvím kódu programu odkazovat na tlačítko Odstranit v prvku GridView nebo prvek DetailsView s odpovídající `DataBound` obslužná rutina události a pak nastavte jeho `OnClientClick` existuje vlastnost.

> [!NOTE]
> Při nastavování tlačítko s `OnClientClick` vlastnost v odpovídajícím `DataBound` obslužná rutina události, máme přístup k data byla vázána na aktuální záznam. To znamená, že můžeme rozšířit o potvrzení zprávy zahrnout podrobnosti o konkrétním záznamu, jako jsou například "Jste si jisti, že chcete odstranit produkt Chai?" Tato vlastní nastavení je také možné v šablonách pomocí syntaxe datové vazby.


Postup nastavení `OnClientClick` vlastnost tlačítkem Delete (tlačítky) v CommandField, umožňují s GridView přidat na stránku. Konfigurace tohoto ovládacího prvku GridView používat stejný ovládací prvek ObjectDataSource, který používá FormView. Také omezte GridView s BoundFields zahrnout pouze s názvem produktu, kategorie a dodavateli. A konečně zaškrtněte políčko Povolit odstranění z ovládacího prvku GridView s inteligentním. Tím přidáte CommandField GridView s `Columns` kolekce s jeho `ShowDeleteButton` nastavenou na `true`.

Po provedení těchto změn, vaše GridView s deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField obsahuje jednu instanci odstranit odkazem (LinkButton), který je možné programově přistupovat z ovládacího prvku GridView s `RowDataBound` obslužné rutiny události. Jakmile se odkazuje, jsme nastavili jeho `OnClientClick` vlastnost odpovídajícím způsobem. Vytvořte obslužnou rutinu události pro `RowDataBound` události pomocí následujícího kódu:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Tato obslužná rutina události funguje s řádky dat (ty, které se mají na tlačítko Odstranit) a začne programově odkazováním na tlačítko Odstranit. Obecně používají následující vzor:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*Má vlastnost ButtonType* se typ používá CommandField – tlačítko, odkazem (LinkButton) nebo ImageButton tlačítka. Ve výchozím nastavení, použije CommandField LinkButtons, ale to je možné přizpůsobit prostřednictvím CommandField s `ButtonType property`. *CommandFieldIndex* je bude index pořadových čísel CommandField v rámci ovládacího prvku GridView s `Columns` kolekce, že *controlIndex* je index na tlačítko Odstranit v rámci CommandField s `Controls` kolekce. *ControlIndex* hodnota závisí na pozici tlačítko s relativně k další tlačítka v CommandField. Například pokud pouze tlačítka zobrazí v CommandField je tlačítko pro odstranění, použijte index 0. Pokud však existovat s tlačítko pro úpravy, které předchází tlačítko odstranit, použijte index 2. Důvod se používá index 2 totiž CommandField před tlačítko přidá dva ovládací prvky: tlačítko Upravit a LiteralControl této s použít k přidání některých mezer mezi tlačítky Edit a Delete.

V našem konkrétním příkladu CommandField používá LinkButtons a pole nejvíce vlevo má *commandFieldIndex* 0. Vzhledem k tomu, že neexistují žádné další tlačítka, ale tlačítko Odstranit v CommandField, používáme *controlIndex* 0.

Po odkazující na tlačítko Odstranit v CommandField, jsme dále získejte informace o produktu vázána na aktuální řádek prvku GridView. Nakonec nastavíme na tlačítko Odstranit s `OnClientClick` vlastnost odpovídající jazyka JavaScript, která zahrnuje název produktu s. Protože byl předán řetězec jazyka JavaScript do `confirm(string)` funkce je oddělen apostrofy jsme musíte je přeskočit všechny apostrofy, které se zobrazují v rámci názvu produktu s použitím. Konkrétně, jsou všechny apostrofy v názvu produktu s uvozeny řídicími znaky s "`\'`".

S těmito změnami dokončeno kliknutím na tlačítko Odstranit v zobrazení GridView přizpůsobené potvrzovací dialogové okno pole (viz obrázek 4). Jako s ovládacím prvkem messagebox potvrzení z FormView, pokud uživatel klikne na tlačítko Storno zpětného odeslání dojde ke zrušení, a tím brání odstranění proti.

> [!NOTE]
> Tento postup můžete použít také k programovému přístupu ke tlačítko Odstranit v CommandField v DetailsView. Prvku DetailsView, ale d vytvořit obslužnou rutinu události pro `DataBound` událost, protože nemá žádné ovládacím prvku DetailsView `RowDataBound` událostí.


[![Kliknutím na tlačítko Odstranit prvek GridView s zobrazí dialogové okno Vlastní potvrzení](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Obrázek 4**: kliknutí na prvek GridView s tlačítko pro odstranění zobrazí přizpůsobit potvrzovací dialogové okno ([kliknutím ji zobrazíte obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Použití vlastností TemplateField

Jednu z nevýhod CommandField je, že jeho tlačítka je možný přes indexování a zda výsledný objekt musí být přetypovat na typ odpovídající tlačítka (Button, odkazem (LinkButton) nebo ImageButton). Použití "čísla magic" a pevně zakódované typy vyzývá problémy, které nejde zjistit až do modulu runtime. Například pokud se vám nebo jiným vývojářem, přidat nová tlačítka na CommandField v určitém okamžiku v budoucnu (jako je například tlačítko pro úpravy) nebo změny `ButtonType` vlastnosti existujícího kódu se stále zkompiluje bez chyb, ale na stránce může způsobit výjimku nebo neočekávané chování v závislosti na tom, jak byla zapsána váš kód a jaké změny byly provedeny.

Alternativním přístupem je převést s ovládacími prvky GridView a DetailsView CommandFields vlastností TemplateField. Tím se vygeneruje TemplateField s `ItemTemplate` pro jednotlivá tlačítka CommandField, který má odkazem (LinkButton) (nebo tlačítko nebo ImageButton). Tato tlačítka `OnClientClick` vlastnosti je možné přiřadit deklarativně, jako jsme viděli s FormView, nebo můžete programově přistupovat do příslušné `DataBound` obslužné rutiny události pomocí následujícího vzorce:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Kde *controlID* je hodnota tlačítko s `ID` vlastnost. Přestože tento vzor vyžaduje stále pevně zakódované typ pro přetypování, je už není nutné indexování, umožňující rozložení změnit, aniž by výsledkem je chyba za běhu.

## <a name="summary"></a>Souhrn

JavaScript `confirm(string)` funkce je technika běžně používaných pro řízení pracovního postupu odeslání formuláře. Při spuštění, zobrazí funkce na straně klienta, modální dialogové okno, které obsahuje dvě tlačítka OK a zrušit. Pokud uživatel klikne na tlačítko OK, `confirm(string)` vrací funkce `true`; klepnutím na tlačítko Storno vrátí `false`. Tato funkce doplňuje chování prohlížeče s obslužnou rutinu události během procesu odesílání vrátí-li zrušit odeslání formuláře `false`, lze použít k zobrazení pole messagebox potvrzení při odstraňování záznamu.

`confirm(string)` Funkce můžou být spojené s tlačítko webového ovládacího prvku s klientský `onclick` obslužné rutiny události pomocí ovládacího prvku s `OnClientClick` vlastnost. Při práci s tlačítko Odstranit v šabloně – buď v některé ze šablon s FormView nebo TemplateField v prvku DetailsView nebo GridView - tuto vlastnost lze nastavit pomocí deklarace nebo prostřednictvím kódu programu, jako jsme viděli v tomto kurzu.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](implementing-optimistic-concurrency-cs.md)
> [další](limiting-data-modification-functionality-based-on-the-user-cs.md)
