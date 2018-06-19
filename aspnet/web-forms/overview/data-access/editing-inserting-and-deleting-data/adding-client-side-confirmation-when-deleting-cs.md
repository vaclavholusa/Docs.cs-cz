---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Přidání potvrzení na straně klienta při odstraňování (C#) | Microsoft Docs
author: rick-anderson
description: V rozhraní, které vytvořili jsme dosavadní práce můžete uživatele omylem odstranit data kliknutím na tlačítko Odstranit při jejich určená klikněte na tlačítko Upravit. V této t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 72b15d498e45cc519a14ecfe39111b224db88c30
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880268"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>Přidání potvrzení na straně klienta při odstraňování (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) nebo [stáhnout PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> V rozhraní, které vytvořili jsme dosavadní práce můžete uživatele omylem odstranit data kliknutím na tlačítko Odstranit při jejich určená klikněte na tlačítko Upravit. V tomto kurzu přidáme dialogové okno potvrzení na straně klienta, které se zobrazí po kliknutí na tlačítko Odstranit.


## <a name="introduction"></a>Úvod

V posledních několika kurzy jsme jste viděli, jak používat naše Architektura aplikace, ObjectDataSource a data ovládací prvky webového ve vzájemné součinnosti zajistit vkládání, úpravy a odstraňování možnosti. Odstranění jsme rozhraní sunout zkontrolován doposud byly složený z odstranění tlačítko, při kliknutí na, způsobí, že zpětné volání a vyvolá ObjectDataSource s `Delete()` metoda. `Delete()` Metoda poté vyvolá metodu nakonfigurované z vrstvy obchodní logiky, které šíří volání dolů Data Access Layer vydání skutečnou `DELETE` příkaz k databázi.

Při této uživatelské rozhraní umožňuje návštěvníkům odstranit záznamy prostřednictvím GridView, DetailsView nebo FormView ovládacích prvků, nemá žádné řazení potvrzení, když uživatel klikne na tlačítko Odstranit. Pokud uživatel omylem klikne na tlačítko Odstranit při jejich určená klikněte na tlačítko Upravit, záznam, který se má aktualizovat místo toho budou odstraněna. Chcete-li tomu zabránit, v tomto kurzu přidáme dialogové okno potvrzení na straně klienta, které se zobrazí po kliknutí na tlačítko Odstranit.

Jazyk JavaScript `confirm(string)` funkce zobrazí její vstupní parametr řetězec jako text v rámci modální dialogové okno, který je vybaven dvě tlačítka - OK a zrušení (viz obrázek 1). `confirm(string)` Funkce vrátí logickou hodnotu, v závislosti na tom, jaké je stisknuto tlačítko (`true`, pokud uživatel klikne na tlačítko OK, a `false` Pokud kliknutím na tlačítko Storno).


![JavaScript confirm(string) metoda zobrazí modální, Messagebox na straně klienta](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Obrázek 1**: JavaScript `confirm(string)` metoda zobrazí Messagebox modální, na straně klienta


Při odeslání formuláře, pokud hodnota `false` vrácená obslužnou rutinu událost na straně klienta a odeslání formuláře byla zrušena. Pomocí této funkce, budeme mít odstranění tlačítko s na straně klienta `onclick` obslužné rutiny události vrátí hodnotu volání `confirm("Are you sure you want to delete this product?")`. Pokud uživatel klikne na tlačítko Storno, `confirm(string)` vrátí false, a tím způsobuje odeslání formuláře zrušit. S žádné zpětné volání, jehož tlačítko Odstranit označeného produktu won t odstranit. Pokud však uživatel klikne na tlačítko OK v dialogovém okně potvrzení, zpětné volání bude nadále nepokračuje a produktu se odstraní. Poraďte se [pomocí jazyka JavaScript s `confirm()` metodu za účelem odeslání formuláře ovládacího prvku](http://www.webreference.com/programming/javascript/confirm/) Další informace o tento postup.

Přidání nezbytné klientský skript se mírně liší při použití šablony než při použití CommandField. Proto v tomto kurzu se podíváme na FormView i GridView příklad.

> [!NOTE]
> Pomocí technik potvrzení na straně klienta, jako ty, které jsou popsané v tomto kurzu se předpokládá, že uživatelé navštěvují s prohlížečů, které podporují JavaScript a zda mají povolen jazyk JavaScript. Pokud platí některá z těchto předpokladů není pro určitého uživatele, kliknutím na tlačítko odstranit okamžitě způsobit zpětné volání (nejsou zobrazena messagebox potvrdit).


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Krok 1: Vytvoření FormView, který podporuje odstranění

Začněte přidáním FormView k `ConfirmationOnDelete.aspx` stránky v `EditInsertDelete` složky, vazba na nové ObjectDataSource, který vrátí zpět informace o produktu prostřednictvím `ProductsBLL` třídu s `GetProducts()` metoda. Také nakonfigurovat ObjectDataSource tak, aby `ProductsBLL` třídu s `DeleteProduct(productID)` metoda je namapována na ObjectDataSource s `Delete()` metoda; zajistěte, aby INSERT a UPDATE karty, rozevírací seznamy jsou nastaveny na (žádný). Nakonec zaškrtněte políčko Povolit stránkování v FormView s inteligentním.

Po provedení těchto kroků nové ObjectDataSource s deklarativní bude vypadat takto:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

V našem posledních příklady, které nepoužili optimistickou metodu souběžného, pozorně vyčistit ObjectDataSource s `OldValuesParameterFormatString` vlastnost.

Protože má vazbu do ovládacího prvku ObjectDataSource, který podporuje pouze odstraňování FormView s `ItemTemplate` nabízí pouze tlačítko odstranit, postrádá tlačítka nové a aktualizace. FormView s deklarativní, ale obsahuje nadbytečné `EditItemTemplate` a `InsertItemTemplate`, který lze odebrat. Za chvíli přizpůsobit `ItemTemplate` tak, že se zobrazuje jenom podmnožinu produktu datová pole. I sunout nakonfigurované dolování zobrazíte název produktu s v `<h3>` záhlaví nad jeho dodavatele a kategorie názvy (spolu s tlačítko Odstranit).


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Tyto změny máme plně funkční webové stránky, která umožňuje uživatelům přepíná mezi produkty jeden současně, pomocí možnosti Odstranit produkt jednoduše kliknutím na tlačítko Odstranit. Obrázek 2 ukazuje snímek obrazovky naše průběh doposud při zobrazení prostřednictvím prohlížeče.


[![FormView se zobrazují informace o jednom produktu](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Obrázek 2**: The FormView zobrazuje informace o jednoho produktu ([Kliknutím zobrazit obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Krok 2: Volání funkce confirm(string) z onclick odstranit tlačítka Client-Side událostí

S FormView vytvořen, posledním krokem je konfigurace tlačítko odstranit takové který po jeho s klikli návštěvníkem JavaScript `confirm(string)` je volána funkce. Přidávání do tlačítko, LinkButton nebo ImageButton s klientský skript na straně klienta `onclick` událostí lze provést prostřednictvím `OnClientClick property`, což je pro technologii ASP.NET 2.0 nové. Vzhledem k tomu, že chceme mají hodnotu `confirm(string)` funkce vrátila, stačí tuto vlastnost nastavit na: `return confirm('Are you certain that you want to delete this product?');`

Po této změně deklarativní syntaxi s odstranit LinkButton by měl vypadat podobně jako:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Že všechny existuje s je k němu! Obrázek 3 ukazuje snímek obrazovky toto potvrzení v akci. Klepnutím na tlačítko Odstranit vyvolá dialogové okno potvrzení. Pokud uživatel klikne na tlačítko Storno, zpětné volání bylo zrušeno a produktu se neodstraní. Pokud však uživatel kliknutím na OK, pokračuje zpětné volání a ObjectDataSource s `Delete()` metoda volána, ukončené v záznamu databáze probíhá odstraňování.

> [!NOTE]
> Řetězec předaný do `confirm(string)` funkce JavaScript, která je oddělená s apostrofy (nikoli uvozovky). V jazyce JavaScript může být oddělený řetězce, pomocí buď znak. Apostrofy tady používáme tak, aby oddělovače pro řetězec předat do `confirm(string)` nevyplývají to nejednoznačnost s oddělovače používá pro `OnClientClick` hodnotu vlastnosti.


[![Potvrzení se nyní zobrazí po kliknutí na tlačítko Odstranit](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Obrázek 3**: A potvrzení se nyní zobrazí po kliknutí na tlačítko Odstranit ([Kliknutím zobrazit obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Krok 3: Konfigurace OnClientClick vlastnost pro tlačítko Odstranit v CommandField

Při práci s tlačítko, LinkButton nebo ImageButton přímo v šabloně, dialogové okno potvrzení lze k ní přidružena jednoduše konfigurací jeho `OnClientClick` vlastnost vracet výsledky jazyka JavaScript `confirm(string)` funkce. Ale CommandField - která přidá pole odstranění tlačítka GridView nebo DetailsView - nemá `OnClientClick` vlastnost, která můžete nastavit deklarativně. Místo toho jsme musí prostřednictvím kódu programu odkazovat na tlačítko Odstranit v GridView nebo DetailsView s příslušnou `DataBound` obslužné rutiny události a potom nastavte její `OnClientClick` vlastnost existuje.

> [!NOTE]
> Při nastavování tlačítko Odstranit s `OnClientClick` vlastnost v příslušné `DataBound` obslužná rutina události, budeme mít přístup k data byla vázána na aktuální záznam. To znamená, že jsme můžete rozšířit o potvrzení zahrnout podrobnosti o konkrétním záznamu, jako je třeba "Opravdu že chcete odstranit produktu Chai?" Takové přizpůsobení je také možné v rámci šablon pomocí syntaxe datové vazby.


Postup nastavení `OnClientClick` vlastnost tlačítkem Delete (tlačítky) v CommandField, umožňují s přidat GridView na stránku. Nakonfigurujte Tato rutina GridView používat stejné ObjectDataSource ovládací prvek, který FormView používá. Také omezte GridView s BoundFields zahrnout pouze s název produktu, kategorie a dodavatele. A konečně zaškrtněte políčko Povolit odstranění z GridView s inteligentním. Tím se přidá CommandField GridView s `Columns` kolekce s jeho `ShowDeleteButton` vlastnost nastavena na hodnotu `true`.

Po provedení těchto změn, vaše GridView s deklarativní by měl vypadat následovně:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField obsahuje jednu instanci odstranit LinkButton, který je možné programově přistupovat z GridView s `RowDataBound` obslužné rutiny události. Jakmile odkazováno, můžete nastavit jsme jeho `OnClientClick` vlastnost odpovídajícím způsobem. Vytvoření obslužné rutiny událostí `RowDataBound` událostí pomocí následujícího kódu:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Tuto obslužnou rutinu události funguje s řádky dat (ty, které budou mít tlačítko Odstranit) a začne prostřednictvím kódu programu odkazem na tlačítko Odstranit. V obecné používají následující vzorec:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* je typ tlačítka používá CommandField – tlačítko, LinkButton nebo ImageButton. Ve výchozím nastavení používá CommandField LinkButtons, ale to lze přizpůsobit prostřednictvím CommandField s `ButtonType property`. *CommandFieldIndex* je index pořadových čísel CommandField v rámci GridView s `Columns` kolekce, zatímco *controlIndex* je index tlačítko Odstranit v rámci CommandField s `Controls` kolekce. *ControlIndex* hodnota závisí na tlačítko s pozice relativně k jiné tlačítka v CommandField. Například pokud tlačítko pouze v CommandField zobrazí tlačítko odstranit, použijte index 0. Pokud však tam s tlačítko pro úpravy předcházejícího tlačítko Odstranit použijte index 2. Z důvodu indexu 2 slouží totiž dvou ovládacích prvků jsou přidávány CommandField před tlačítko Odstranit: tlačítko Upravit a LiteralControl této s použít k přidání některé mezery mezi tlačítka Upravit a odstranit.

Pro náš příklad konkrétní CommandField používá LinkButtons a má se pole nejvíce vlevo *commandFieldIndex* 0. Vzhledem k tomu, že neexistují žádná jiná tlačítka, ale v CommandField tlačítko odstranit, používáme *controlIndex* 0.

Po odkazující na tlačítko Odstranit v CommandField, jsme získat další informace o produktu vázána na aktuální řádek GridView. Nakonec nastaví tlačítko Odstranit s `OnClientClick` vlastnost odpovídající jazyka JavaScript, která zahrnuje název produktu s. Vzhledem k tomu, že řetězec JavaScript předaný do `confirm(string)` funkce je oddělený pomocí apostrofy jsme musí vyhnuli žádné apostrofy, které se zobrazují v názvu produktu s. Konkrétně, jsou všechny apostrofy v názvu produktu s uvozeny zpětným "`\'`".

Tyto změny provést, kliknutím na tlačítko Odstranit v GridView zobrazí, zobrazí se dialogové okno Vlastní potvrzení pole (viz obrázek 4). Jako s messagebox potvrzení z FormView, pokud uživatel klikne na tlačítko Storno zpětné volání je zrušená, a zabrání odstranění výskytu.

> [!NOTE]
> Tento postup můžete použít také k programovému přístupu v CommandField v DetailsView tlačítko Odstranit. Pro DetailsView, ale d vytvořit obslužnou rutinu události pro `DataBound` událost, protože nemá DetailsView `RowDataBound` událostí.


[![Kliknutím na tlačítko Odstranit GridView s zobrazí dialogové okno Vlastní potvrzení](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Obrázek 4**: Kliknutím na tlačítko s GridView tlačítko Odstranit zobrazí přizpůsobit potvrzovací dialogové okno ([Kliknutím zobrazit obrázek v plné velikosti](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>Pomocí TemplateFields

Jedním z nevýhody CommandField je, že jeho tlačítka je nutno přistupovat prostřednictvím indexování a že výsledný objekt musí být přetypovat na typ příslušné tlačítko (tlačítko, LinkButton nebo ImageButton). Použití "čísla magic" a pevně typy žádostí problémy, které nelze zjistit, dokud modulu runtime. Například pokud jste nebo jiné vývojáře přidat nová tlačítka na CommandField v určitém okamžiku v budoucnu (například tlačítko pro úpravy) nebo změny `ButtonType` vlastnost existující kód bude stále kompilovat bez chyby, ale na stránce může způsobit výjimku nebo neočekávanému chování, v závislosti na tom, jak byla zapsána kódu a jaké změny byly provedeny.

Alternativní způsob je převést GridView a DetailsView s CommandFields TemplateFields. Tím se vygeneruje TemplateField s `ItemTemplate` pro každé tlačítko v CommandField s LinkButton (nebo tlačítka nebo ImageButton). Tato tlačítka `OnClientClick` vlastnosti lze přiřadit deklarativně, jako jsme viděli s FormView, nebo můžete programově přistupovat v příslušné `DataBound` obslužné rutiny události pomocí následujícího vzorce:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Kde *controlID* je hodnota tlačítko s `ID` vlastnost. Při tomto vzoru stále vyžaduje typ pevně pro přetypování, se odeberou potřebu indexování, povolení pro rozložení změnit, aniž by vést k chybě za běhu.

## <a name="summary"></a>Souhrn

Jazyk JavaScript `confirm(string)` funkce je technika běžně používané pro řízení pracovního postupu odeslání formuláře. Po provedení funkce zobrazí modální, klienta dialogové okno obsahuje dvě tlačítka OK a Storno. Pokud uživatel klikne na tlačítko OK, `confirm(string)` funkce vrátí `true`; kliknutím na tlačítko Storno vrátí `false`. Tato funkce kombinaci s prohlížeči s chování obslužné rutiny události během procesu odesílání vrátí-li zrušit odeslání formuláře `false`, lze použít k zobrazení messagebox potvrzení při odstraňování záznamu.

`confirm(string)` Funkce může být přidružen tlačítko webové ovládací prvek s klientský `onclick` obslužné rutiny události prostřednictvím ovládacího prvku s `OnClientClick` vlastnost. Při práci s tlačítko pro odstranění v šabloně – buď v jedné z šablon s FormView nebo v TemplateField v DetailsView nebo GridView - tuto vlastnost lze nastavit deklarativně nebo programově, jak jsme viděli v tomto kurzu.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](implementing-optimistic-concurrency-cs.md)
> [další](limiting-data-modification-functionality-based-on-the-user-cs.md)
