---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: "Vázané ovládací prvky dat | Microsoft Docs"
author: microsoft
description: "Většina aplikací ASP.NET závisí na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data byla hrají součástí vzájemně komunikující w..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 3ebb0f9a7a2f071b7bf7aa3855920f1a5784a61f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="data-bound-controls"></a>Data vázané ovládací prvky
====================
podle [Microsoft](https://github.com/microsoft)

> Většina aplikací ASP.NET závisí na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data byla hrají součástí interakci s daty v dynamické webové aplikace. Technologie ASP.NET 2.0 přináší některé podstatná vylepšení v oblasti pro ovládací prvky vázané na data, včetně novou třídu BaseDataBoundControl a deklarativní syntaxe.


Většina aplikací ASP.NET závisí na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data byla hrají součástí interakci s daty v dynamické webové aplikace. Technologie ASP.NET 2.0 přináší některé podstatná vylepšení v oblasti pro ovládací prvky vázané na data, včetně novou třídu BaseDataBoundControl a deklarativní syntaxe.

BaseDataBoundControl funguje jako základní třída pro třídy prvku DataBoundControl a HierarchicalDataBoundControl. V tomto modulu se budeme zabývat následující třídy, které jsou odvozeny od prvku DataBoundControl:

- AdRotator
- Ovládací prvky seznamu
- GridView
- FormView
- DetailsView

Budeme zabývat také následující třídy, které jsou odvozeny od třídy HierarchicalDataBoundControl:

- TreeView
- Nabídka
- SiteMapPath

## <a name="databoundcontrol-class"></a>Třída prvku DataBoundControl.

Třída prvku DataBoundControl je abstraktní třída (označená MustInherit v jazyce Visual Basic), používají k interakci se tabulkové nebo styl seznamu data. Ovládací prvky jsou některé ovládací prvky, které jsou odvozeny od prvku DataBoundControl.

## <a name="adrotator"></a>AdRotator

Ovládací prvek AdRotator umožňuje zobrazit banner grafické na webové stránce, který je propojený s konkrétní adresy URL. Na obrázku, který se zobrazí otočen pomocí vlastností pro ovládací prvek. Četnost konkrétní ad zobrazení na stránce lze nakonfigurovat pomocí **otisky** vlastnost a služby Active Directory je možné filtrovat pomocí filtrování – klíčové slovo.

Ovládací prvky AdRotator použít soubor XML nebo tabulky v databázi pro data. Následující atributy se používají v souborech XML ke konfiguraci ovládacího prvku AdRotator.

### <a name="imageurl"></a>ImageUrl
Adresa URL obrázku, který má být zobrazen pro služby ad.

### <a name="navigateurl"></a>NavigateUrl
Adresu URL, kterou je třeba věnovat uživatele při kliknutí na reklamy. To by měl být kódovaná adresou URL.

### <a name="alternatetext"></a>AlternateText
Alternativní text, který se zobrazí ve formě popisu tlačítka a číst čtečky obrazovky. Zobrazí také když bitovou kopii určeného ImageUrl není k dispozici.

### <a name="keyword"></a>– Klíčové slovo
Definuje klíčové slovo, které lze použít při používání filtrování – klíčové slovo. -Li zadána, zobrazí se pouze reklam s klíčového slova odpovídající filtru – klíčové slovo.

### <a name="impressions"></a>Otisky
Vyvážení číslo, které určuje, jak často se pravděpodobně objevovat konkrétní ad. Je relativní vzhledem k dojem jiné služby Active Directory ve stejném souboru. Maximální hodnota, která souhrnný otisky pro všechny služby Active Directory v souboru XML je 2,048,000,000 1.

### <a name="height"></a>Výška
Výška ad v pixelech.

### <a name="width"></a>Šířka
Šířka ad v pixelech.


> [!NOTE]
> Výška a šířka atributy přepsat výšky a šířky pro ovládací prvek AdRotator sám sebe.


Typické souboru XML může vypadat následovně:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Ve výše uvedeném příkladu bude dvakrát ad pro společnost Contoso jako pravděpodobně se zobrazí jako ad pro ASP.NET Web kvůli hodnotu pro atribut otisky.

Chcete-li zobrazit reklamy ze souboru XML sady výše, přidání ovládacího prvku AdRotator na stránku a nastavte **soubor AdvertisementFile** vlastnost tak, aby odkazovaly do souboru XML, jak je uvedeno níže:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Pokud chcete použít tabulku databáze jako zdroj dat pro ovládací prvek AdRotator, musíte nejdřív nastavit pomocí následujících schématu databáze:

| **Název sloupce** | **Datový typ** | **Popis** |
| --- | --- | --- |
| ID | int | Primární klíč. Tento sloupec může mít libovolný název. |
| ImageUrl | nvarchar (*délka*) | Relativní nebo absolutní adresa URL obrázku má být zobrazen pro služby ad. |
| NavigateUrl | nvarchar (*délka*) | Cílová adresa URL pro ad. Pokud nezadáte hodnotu, ad není hypertextový odkaz. |
| AlternateText | nvarchar (*délka*) | Text, zobrazí v případě, že obrázek nebyl nalezen. V některých prohlížečích text se zobrazuje jako popisek. Alternativní text se také používá pro usnadnění přístupu tak, aby uživatelé, kteří nelze vidět na obrázku slyšet jeho popis číst nahlas. |
| – Klíčové slovo | nvarchar (*délka*) | Kategorie pro ad, ve kterém můžete filtrovat stránky. |
| Otisky | int(4) | Číslo, které určuje, jak často se zobrazí ad pravděpodobnost. Čím vyšší číslo, tím častěji ad se zobrazí. Celkový počet všech otisky hodnot v souboru XML nesmí překročit 2 048 000 000-1. |
| Šířka | int(4) | Šířku obrázku v pixelech. |
| Výška | int(4) | Výška obrázku v pixelech. |

V případech, kdy již máte databázi s zvolte jiné schéma, můžete použít **AlternateTextField**, **ImageUrlField**, a **NavigateUrlField** vlastnosti pro mapování Atributy AdRotator k existující databázi. Pokud chcete zobrazit data z databáze v ovládacím prvku AdRotator, přidejte na stránku ovládací prvek zdroje dat, nakonfigurovat připojovací řetězec pro ovládací prvek zdroje dat tak, aby odkazoval na vaši databázi a nastavení ovládacího prvku AdRotator **DataSourceID** Vlastnost ID ovládacího prvku datového zdroje. V případech, kdy máte potřeba nakonfigurovat AdRotator reklamy programově použijte události AdCreated. Události AdCreated přebírá dva parametry; jeden objekt a druhá instance AdCreatedEventArgs. AdCreatedEventArgs je odkaz na ad, který je právě vytvářena.

Následující fragment kódu nastaví ImageUrl, NavigateUrl a AlternateText pro ad prostřednictvím kódu programu:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Ovládací prvky seznamu

Ovládací prvky seznamu zahrnují ListBox, rozevírací seznam, CheckBoxList, RadioButtonList a BulletedList. Každý z těchto prvků může představovat data vázaná na zdroj dat. Používají jedno pole ve zdroji dat jako text k zobrazení a druhé pole můžete volitelně použít jako hodnotu položky. Položky lze také přidat staticky v době návrhu a je možné kombinovat statické a dynamické položkami ze zdroje dat přidány.

Data vazby ovládací prvek seznamu, přidejte ovládací prvek zdroje dat na stránku. Určení příkazu, vyberte možnost pro ovládací prvek zdroje dat a nastavte ID ovládacího prvku zdroje dat ovládacího prvku seznam nastavenou vlastnost DataSourceID. Použití **DataTextField** a **DataValueField** vlastnosti, které chcete definovat zobrazovaný text a hodnoty pro ovládací prvek. Kromě toho můžete použít **DataTextFormatString** vlastnost, která má-li řídit vzhled zobrazovaný text následujícím způsobem:

| **Výraz** | **Popis** |
| --- | --- |
| Cena: {0: c} | Pro data číselný/decimal. Zobrazuje skutečné "cena:" následované čísel ve formátu měny. Formátu měny závisí na nastavení jazykové verze zadané v atributu jazykovou verzi na **stránky** direktivy nebo v souboru Web.config. |
| {0:D4} | Pro data, celé číslo. Nelze použít s desetinným číslem. Celá čísla se zobrazí v poli nulami, které je širokém čtyři znaky. |
| {0:N2} % | Pro číselná data. Zobrazuje číslo s 2 desetinné místo přesnost následuje literál "%". |
| {0:000.0} | Pro data číselný/decimal. Čísla jsou zaokrouhleny na jedno desetinné místo. Je menší než nulami tří číslic čísla. |
| {0: D} | Pro data datum a čas. Zobrazí formát dlouhého data ("čtvrtek, srpen 06, 1996"). Formát data závisí na nastavení jazykové verze stránky nebo v souboru Web.config. |
| {0: d} | Pro data datum a čas. Zobrazí krátkého data formátu ("12/31/99"). |
| {0:yy-MM-dd} | Pro data datum a čas. Zobrazí datum v číselném formátu rok měsíc den (96-08-06) |

## <a name="gridview"></a>GridView

GridView umožňuje tabulková data zobrazení a úpravy pomocí deklarativní přístup a je nástupcem DataGrid – ovládací prvek. Následující funkce jsou k dispozici v ovládacím prvku GridView.

- Vytvoření vazby na data ovládací prvky zdroj, například SqlDataSource.
- Integrované funkce pro řazení.
- Předdefinované aktualizace a odstranění možnosti.
- Integrované funkce stránkování.
- Výběr možnosti integrované řádek.
- Programový přístup k modelu objektu GridView dynamicky nastavit vlastnosti, zpracování událostí a tak dále.
- Více polí klíčů.
- Více datových polí pro sloupce hypertextový odkaz.
- Vzhled lze přizpůsobit pomocí motivů a stylů.

**Pole sloupců**

Každý sloupec v ovládacím prvku GridView je reprezentována DataControlField objektu. Ve výchozím nastavení, je AutoGenerateColumns vlastnost nastavena **true**, který vytvoří objekt AutoGeneratedField pro každé pole v datovém zdroji. Každé pole je pak vykreslen jako sloupec v ovládacím prvku GridView v pořadí, ve kterém se zobrazí každé pole v datovém zdroji. Můžete také ručně řídit sloupec pole se zobrazí v **GridView** ovládací prvek pro nastavení **AutoGenerateColumns** vlastnost **false** a pak definování vlastní kolekce pole sloupců. Typy polí různé sloupce určuje chování sloupců v ovládacím prvku.

Následující tabulka uvádí typy polí jiný sloupec, které lze použít.

| **Typ pole sloupců** | **Popis** |
| --- | --- |
| BoundField | Zobrazí hodnotu pole v datovém zdroji. Toto je výchozí typ sloupce ovládacího prvku GridView. |
| ButtonField | Zobrazí příkazového tlačítka pro každou položku v ovládacím prvku GridView. Umožňuje vytvořit sloupec vlastní ovládací prvky, například přidáním nebo na tlačítko Odebrat. |
| Vlastnost CheckBoxField | Zobrazí zaškrtávací políčko pro každou položku v ovládacím prvku GridView. Tento typ pole sloupce se často používá ke zobrazení polí se logická hodnota. |
| CommandField | Zobrazí předdefinované příkazová tlačítka k provedení výběru, úpravy nebo odstranění operace. |
| HyperLinkField | Zobrazí hodnotu pole ve zdroji dat jako hypertextový odkaz. Tento typ pole sloupce umožňuje druhé pole vázat na adresu URL na hypertextový odkaz. |
| ImageField | Zobrazí obrázek pro každou položku v ovládacím prvku GridView. |
| TemplateField | Zobrazí obsah uživatelem definované pro každou položku v ovládacím prvku GridView podle zadané šablony. Tento typ pole sloupce můžete vytvořit vlastní sloupec pole. |

Chcete-li definovat deklarativně kolekci pole sloupec, nejprve přidat otvírání a zavírání  **&lt;sloupce&gt;**  značky mezi počáteční a koncové značky ovládacího prvku GridView. V dalším kroku seznam pole sloupců, které chcete zahrnout mezi počáteční a koncovou  **&lt;sloupce&gt;**  značky. Sloupce zadané se přidají do kolekce sloupců v uvedeném pořadí. **Sloupce** kolekce ukládá všechny sloupce polí v ovládacím prvku a umožňuje vám spravovat prostřednictvím kódu programu pole sloupců v ovládacím prvku GridView.

Pole explicitně deklarované sloupec lze zobrazit v kombinaci s pole automaticky generované sloupců. Pokud obě používají, pole explicitně deklarované sloupců jsou vykreslovány v nejprve, za nímž následuje pole automaticky generovaný sloupec.

## <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládací prvek GridView mohou být vázány na ovládací prvek zdroje dat (například **SqlDataSource**, **ObjectDataSource**a tak dále), stejně jako libovolný zdroj dat, který implementuje System.Collections.IEnumerable rozhraní (například System.Data.DataView, System.Collections.ArrayList nebo System.Collections.Hashtable). K vytvoření vazby ovládacího prvku GridView. typ zdroje dat odpovídající použijte jednu z následujících metod:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte na hodnotu ID ovládacího prvku zdroje dat ovládacího prvku GridView nastavenou vlastnost DataSourceID. Ovládací prvek GridView automaticky váže k řízení zdrojů zadaná data a můžete využít výhod zdroj dat ovládacího prvku schopnosti provádět, řazení, aktualizaci, odstranění a funkce stránkování. Toto je upřednostňovaný způsob vytvoření vazby na data.
- Při vytváření vazby ke zdroji dat, který implementuje rozhraní System.Collections.IEnumerable, programově nastavit vlastnost DataSource ovládacího prvku GridView ke zdroji dat a pak zavolejte metodu DataBind. Při použití této metody ovládacího prvku GridView neposkytuje integrované řazení, aktualizaci, odstranění a stránkování funkce. Budete muset tuto funkčnost zajistit sami.

## <a name="data-operations"></a>Operace s daty

Prvek GridView nabízí mnoho integrované funkce, které uživateli umožňují řazení, aktualizovat, odstranit, vyberte a stránka prostřednictvím položky v ovládacím prvku. Pokud rutina GridView ovládací prvek vázán na ovládací prvek zdroje dat, GridView můžete využít výhod zdroje dat možnosti ovládacího prvku a poskytují automatické řazení, aktualizace a odstranění funkce.

> [!NOTE]
> Ovládací prvek GridView může poskytovat podporu pro řazení, aktualizaci a odstraňování s jinými typy zdrojů dat; však musíte zadat příslušnou obslužnou rutinu s implementací pro tyto operace.


Řazení umožňuje uživatelům položky v ovládacím prvku GridView s ohledem na konkrétní sloupec seřadit kliknutím na záhlaví sloupce. Pokud chcete povolit, řazení, nastavte vlastnost AllowSorting na **true**.

Automatické aktualizace, odstranění a výběr funkce je zapnuté, pokud tlačítka na **ButtonField** nebo **TemplateField** sloupec pole s názvem příkaz "Upravit", "Odstranit" a "Vyberte", v uvedeném pořadí po kliknutí na. Rutina GridView řízení může automaticky přidat **CommandField** sloupec pole s úpravy, odstranění nebo tlačítka vybrat, pokud je vlastnost AutoGenerateEditButton, AutoGenerateDeleteButton nebo AutoGenerateSelectButton vlastnost nastavena na **true**, v uvedeném pořadí.

> [!NOTE]
> Vkládání záznamů do zdroje dat nepodporuje přímo prvek GridView. Je však možné vložení záznamů pomocí prvku GridView ve spojení s DetailsView nebo FormView řízení.


Místo zobrazení všech záznamů ve zdroji dat ve stejnou dobu, rutina GridView řízení může automaticky rozdělit záznamy do stránky. Pokud chcete povolit stránkování, nastavte vlastnost AllowPaging na **true**.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Můžete přizpůsobit vzhled ovládacího prvku GridView nastavením vlastnosti stylu pro různé části ovládacího prvku. Následující tabulka uvádí vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| AlternatingRowStyle | Nastavení stylů střídavých řádků dat v ovládacím prvku GridView. Když je tato vlastnost nastavena, řádky dat se zobrazí střídavých mezi nastavení RowStyle a **AlternatingRowStyle** nastavení. |
| EditRowStyle | Nastavení stylu pro řádek upravovaný v ovládacím prvku GridView. |
| EmptyDataRowStyle | Nastavení stylu řádku prázdný dat v ovládacím prvku GridView zobrazí, když zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí ovládacího prvku GridView. |
| HeaderStyle | Nastavení stylu pro řádek záhlaví ovládacího prvku GridView. |
| PagerStyle | Nastavení stylu pro řádek pager ovládacího prvku GridView. |
| RowStyle | Styl nastavení pro řádky dat v ovládacím prvku GridView. Když **AlternatingRowStyle** také vlastnost nastavena, řádky dat se zobrazují střídavých mezi **RowStyle** nastavení a **AlternatingRowStyle** nastavení. |
| SelectedRowStyle | Nastavení stylu pro vybraný řádek v ovládacím prvku GridView. |

Můžete také zobrazit nebo skrýt různé části ovládacího prvku. Následující tabulka uvádí vlastnosti, které řídí, jaké části jsou zobrazen nebo skryt.

| **Vlastnost** | **Popis** |
| --- | --- |
| ShowFooter | Zobrazí nebo skryje zápatí ovládacího prvku GridView. |
| ShowHeader | Zobrazí nebo skryje v záhlaví části ovládacího prvku GridView. |

### <a name="events"></a>Události

Ovládací prvek GridView poskytuje několik událostí, které můžete naprogramovat oproti. To umožňuje spouštět vlastní rutina vždy, když dojde k události. Následující tabulka uvádí události GridView ovládacím prvkem podporována.

| **Události** | **Popis** |
| --- | --- |
| PageIndexChanged | Nastane při kliknutí na jednu z tlačítka stránkování, ale po prvek GridView zpracovává operaci stránkování. Tato událost se běžně používá, když potřebujete provést úlohu po uživatel přejde na jinou stránku v ovládacím prvku. |
| PageIndexChanging | Nastane, když po kliknutí na jednu z tlačítka stránkování, ale před GridView ovládací prvek zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |
| RowCancelingEdit | Nastane, když po kliknutí na tlačítko Storno řádek, ale před prvek GridView ukončí režim úprav. Tato událost se často používá k zastavení ruší operaci. |
| RowCommand | Nastane při stisknutí tlačítka v ovládacím prvku GridView. Tato událost se často používá k provedení úloh při stisknutí tlačítka v ovládacím prvku. |
| RowCreated | Nastane, když je vytvořen nový řádek v ovládacím prvku GridView. Tato událost se často používá k úpravě obsahu řádku, když je vytvořen řádek. |
| RowDataBound | Nastane, když data v řádku vázané na data v ovládacím prvku GridView. Tato událost se často používá k upravovat obsah řádek po řádku vázané na data. |
| RowDeleted | Nastane při kliknutí na tlačítko Odstranit řádku, ale po prvek GridView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| RowDeleting | Nastane, když po kliknutí na tlačítko Odstranit řádku, ale před GridView řízení odstraní záznam ze zdroje dat. Tato událost se často používá ke zrušení operace odstraňování. |
| RowEditing | Nastane, když po kliknutí na tlačítko Upravit řádek, ale před GridView řízení vstupuje do režimu úprav. Tato událost se často používá ke zrušení operace úpravy. |
| RowUpdated | Nastane při kliknutí na tlačítko Aktualizovat řádek, ale po prvek GridView aktualizací řádek. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| RowUpdating | Nastane, když po kliknutí na tlačítko Aktualizovat řádek, ale před GridView aktualizace ovládacího prvku na řádek. Tato událost se často používá ke zrušení operace aktualizace. |
| SelectedIndexChanged | Nastane při kliknutí na tlačítko Vybrat řádku, ale po prvek GridView zpracovává operace select. Tato událost se často používá k provedení úloh, jakmile je vybrán řádek v ovládacím prvku. |
| SelectedIndexChanging | Nastane, když po kliknutí na tlačítko Vybrat řádku, ale před GridView ovládací prvek zpracovává operace select. Tato událost se často používá ke zrušení operace výběr. |
| Seřadit | Nastane při kliknutí na hypertextový odkaz na sloupec seřadit, ale po prvek GridView zpracovává operace řazení. Tato událost se běžně používá k provedení úloh, jakmile uživatel klikne na hypertextový odkaz na sloupec seřadit. |
| Řazení | Nastane, když po kliknutí na hypertextový odkaz na sloupec seřadit, ale před GridView ovládací prvek zpracovává operace řazení. Tato událost se často používá ke zrušení operace řazení nebo provádět vlastní rutiny řazení. |

## <a name="formview"></a>FormView

Ovládací prvek FormView se používá k zobrazení jeden záznam ze zdroje dat. Je to podobné do ovládacího prvku DetailsView, s výjimkou zobrazuje uživatelem definovaných šablon místo pole řádku. Vytvoření vlastní šablony vám dává větší flexibilitu při řízení, jak se zobrazí data. Ovládací prvek FormView podporuje následující funkce:

- Vytvoření vazby na data ovládací prvky zdroj, například SqlDataSource a ObjectDataSource.
- Integrované funkce vkládání.
- Předdefinované aktualizace a odstranění možnosti.
- Integrované funkce stránkování.
- Programový přístup k modelu objektu FormView dynamicky nastavit vlastnosti, zpracování událostí a tak dále.
- Vzhled lze přizpůsobit pomocí uživatelem definovaných šablon, motivů a stylů.

## <a name="templates"></a>Šablony

Pro ovládací prvek FormView k zobrazení obsahu musíte vytvořit šablony pro různé součásti ovládacího prvku. Většina šablony jsou volitelná. Musíte však vytvořit šablonu pro režim, ve kterém je nakonfigurovaný ovládacího prvku. Například FormView ovládací prvek, který podporuje vložení záznamu musí mít definované šablony položky insert. Následující tabulka uvádí různé šablony, které můžete vytvořit.

| **Typ šablony** | **Popis** |
| --- | --- |
| EditItemTemplate | Definuje obsah pro řádek dat v případě ovládacího prvku FormView je v režimu úprav. Tato šablona obvykle obsahuje vstupní ovládací prvky a příkazová tlačítka, pomocí kterého může uživatel upravit existující záznam. |
| EmptyDataTemplate | Definuje obsah pro prázdný datový řádek zobrazí, když je FormView ovládací prvek vázán ke zdroji dat, který neobsahuje žádné záznamy. Tato šablona obvykle obsahuje obsah k upozornění uživatele, že zdroj dat neobsahuje žádné záznamy. |
| FooterTemplate | Definuje obsah pro řádek zápatí. Tato šablona obsahuje obvykle žádné další obsah, který se má zobrazit v řádku zápatí. Jako alternativu můžete jednoduše zadejte text, který se zobrazí v řádku zápatí nastavením vlastnosti FooterText. |
| HeaderTemplate | Definuje obsah pro řádek záhlaví. Tato šablona obsahuje obvykle žádné další obsah, který se má zobrazit v řádku záhlaví. Jako alternativu můžete jednoduše zadejte text, který se zobrazí v řádku záhlaví nastavením vlastnosti HeaderText. |
| ItemTemplate | Definuje obsah pro řádek dat v případě ovládacího prvku FormView je v režimu jen pro čtení. Tato šablona obvykle obsahuje obsah k zobrazení hodnot z existujícího záznamu. |
| InsertItemTemplate | Definuje obsah pro řádek dat v případě ovládacího prvku FormView je v režimu vkládání. Tato šablona obvykle obsahuje vstupní ovládací prvky a příkazová tlačítka, pomocí kterého uživatel může přidávat nový záznam. |
| PagerTemplate | Definuje obsah pro řádek pager zobrazí, když je povolena funkce stránkování (Pokud je vlastnost AllowPaging nastavena na **true**). Tato šablona obvykle obsahuje ovládací prvky, se kterými uživatel můžete přejít na jiný záznam. |

Vstupní ovládací prvky v šablony položky Úpravy a šablony položky vložení vázat na pole zdroje dat pomocí výrazu obousměrné vazby. To umožňuje řídit FormView automaticky extrahovat hodnoty vstupního ovládacího prvku pro aktualizaci nebo vložení operaci. Vazba obousměrná výrazy také povolit vstupní ovládací prvky v šabloně položky upravit původní hodnoty pole automaticky zobrazit.

### <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládací prvek FormView mohou být vázány na ovládací prvek zdroje dat (například **SqlDataSource**, AccessDataSource, **ObjectDataSource** a tak dále), nebo libovolný zdroj dat, který implementuje Rozhraní System.Collections.IEnumerable (například System.Data.DataView, System.Collections.ArrayList a System.Collections.Hashtable). K vytvoření vazby ovládacího prvku FormView. typ zdroje dat odpovídající použijte jednu z následujících metod:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte na hodnotu ID ovládacího prvku zdroje dat ovládacího prvku FormView nastavenou vlastnost DataSourceID. Ovládací prvek FormView automaticky váže k řízení zdrojů zadaná data a můžete využít výhod zdroje dat možnosti ovládacího prvku provést vložení, aktualizaci, odstranění a funkce stránkování. Toto je upřednostňovaný způsob vytvoření vazby na data.
- Při vytváření vazby ke zdroji dat, který implementuje **System.Collections.IEnumerable** rozhraní, programově nastavit vlastnost DataSource ovládacího prvku FormView ke zdroji dat a pak zavolejte metodu DataBind. Při použití této metody ovládacího prvku FormView neposkytuje integrované vkládání, aktualizaci, odstranění a funkce stránkování. Budete muset tuto funkčnost zajistit pomocí příslušné události.

## <a name="data-operations"></a>Operace s daty

Ovládací prvek FormView poskytuje mnoho integrované funkce, které uživateli umožňují aktualizovat, odstranit, insert a položky v ovládacím prvku procházet po stránkách. Pokud ovládací prvek FormView je vázán na ovládací prvek zdroje dat, ovládacího prvku FormView můžete využít výhod zdroj dat možnosti ovládacího prvku a poskytují automatickou aktualizaci, odstranění, vkládání a stránkování funkce. Ovládací prvek FormView může poskytovat podporu pro aktualizaci, odstranění, insert a stránkovací operace s jinými typy zdrojů dat; Nicméně je nutné zadat příslušnou obslužnou rutinu s implementací pro tyto operace.

Protože ovládacího prvku FormView používá šablony, neposkytuje způsob, jak automaticky vygenerovat příkazová tlačítka provést aktualizaci, odstranění nebo vložení operace. Je nutné ručně zahrnout tyto příkazová tlačítka příslušné šablony. Ovládací prvek FormView rozpozná určité tlačítka, které mají své **CommandName** nastaveny na konkrétní hodnoty vlastnosti. Následující tabulka uvádí příkazová tlačítka, který rozpoznává ovládacího prvku FormView.

| **Tlačítko** | **Hodnota CommandName** | **Popis** |
| --- | --- | --- |
| Zrušit | Tlačítko Storno. | Používá se v aktualizaci nebo vložení operations na tlačítko Storno a zahodit hodnoty zadané uživatelem. Ovládací prvek FormView vrátí do režimu určeného vlastností DefaultMode. |
| Odstranit | "Odstranit. | Používá se v odstranění operations k odstranění zobrazený záznam ze zdroje dat. Vyvolá událost ItemDeleting a ItemDeleted. |
| Upravit | "Upravit" | Použít při aktualizaci operations ovládacího prvku FormView uvést do režimu úprav. Zadanému v obsahu **EditItemTemplate** vlastnost je zobrazena pro řádek data. |
| Insert | "Vložit" | Používá se v vkládání operations k pokusu o vložení nového záznamu ve zdroji dat pomocí hodnot zadané uživatelem. Vyvolá událost ItemInserting a ItemInserted. |
| Nový | "New" | Používá se v operace vložení ovládacího prvku FormView uvést do režimu vkládání. Zadanému v obsahu **InsertItemTemplate** vlastnost je zobrazena pro řádek data. |
| Stránka | "Stránka" | Používá stránkovací operace představuje tlačítka na pager řádek, který provádí stránkování. Chcete-li určit operaci stránkování, nastavte **CommandArgument** vlastnost na tlačítko "Další", "Předchozí", "První", "Posledního" nebo index stránky, ke kterému chcete přejít. Vyvolá událost PageIndexChanging a PageIndexChanged. |
| Aktualizace | "Update" | Umožňuje při aktualizaci operations pokus o aktualizaci zobrazený záznam ve zdroji dat s hodnotami zadané uživatelem. Vyvolá událost ItemUpdating a ItemUpdated. |

Na rozdíl od odstranění tlačítko (která odstraňuje zobrazený záznam okamžitě), při kliknutí na tlačítko Upravit nebo nový FormView řízení přejde do upravit nebo vložit režimu v uvedeném pořadí. V režimu úprav, součástí obsahu **EditItemTemplate** vlastnost je zobrazena pro aktuální datové položky. Obvykle šablony úpravy položky je definována tak, aby tlačítko Upravit je nahrazena aktualizace a tlačítko Zrušit. Vstupní ovládací prvky, které jsou vhodné pro datový typ tohoto pole (například textové pole nebo ovládací prvek zaškrtávací políčko) se také obvykle zobrazují s hodnotu pole pro uživatele, který chcete upravit. Kliknutím na tlačítko Aktualizovat aktualizace záznamu ve zdroji dat, při kliknutí na tlačítko Storno opustí žádné změny.

Podobně obsah součástí **InsertItemTemplate** vlastnost se zobrazí položky dat v případě, že je ovládací prvek v režimu vkládání. Šablony položky insert je obvykle definováno tak, aby tlačítko nové je nahradí typu vložení a tlačítko Zrušit a prázdný vstupní ovládací prvky jsou zobrazeny pro uživatele a zadejte hodnoty pro nový záznam. Kliknutím na tlačítko Vložit Vloží záznamu ve zdroji dat, při kliknutí na tlačítko Storno opustí žádné změny.

Ovládací prvek FormView poskytuje funkci stránkování, která umožňuje uživatelům přejděte na další záznamy v datovém zdroji. Když je povolené, řádek pager se zobrazí v ovládacím prvku FormView, který obsahuje ovládací prvky pro navigaci na stránce. Chcete-li povolit stránkování, nastavte **AllowPaging** vlastnost **true**. Nastavením vlastnosti objektů, které jsou součástí PagerStyle a vlastnost PagerSettings můžete přizpůsobit řádek pager. Místo použití předdefinované pager řádek uživatelského rozhraní, můžete vytvořit vlastní uživatelské rozhraní pomocí **PagerTemplate** vlastnost.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Můžete přizpůsobit vzhled ovládacího prvku FormView nastavením vlastnosti stylu pro různé části ovládacího prvku. Následující tabulka uvádí vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| EditRowStyle | Nastavení stylu pro řádek dat při řízení FormView je v režimu úprav. |
| EmptyDataRowStyle | Nastavení stylu pro prázdný datový řádek zobrazí v ovládacím prvku FormView v případě, že zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí FormView ovládacího prvku. |
| HeaderStyle | Nastavení stylu pro řádek záhlaví FormView ovládacího prvku. |
| InsertRowStyle | Nastavení stylu pro řádek dat při řízení FormView je v režimu vkládání. |
| PagerStyle | Nastavení stylu pro řádek pager v ovládacím prvku FormView zobrazí, když je povolena funkce stránkování. |
| RowStyle | Nastavení stylu pro řádek dat při ovládacího prvku FormView je v režimu jen pro čtení. |

## <a name="events"></a>Události

Ovládací prvek FormView poskytuje několik událostí, které můžete naprogramovat oproti. To umožňuje spouštět vlastní rutina vždy, když dojde k události. Následující tabulka uvádí události FormView ovládacím prvkem podporována.

| **Události** | **Popis** |
| --- | --- |
| ItemCommand | Nastane, když po kliknutí na tlačítko v ovládacím prvku FormView. Tato událost se často používá k provedení úloh při stisknutí tlačítka v ovládacím prvku. |
| ItemCreated | Nastane po vytvoření všechny objekty FormViewRow v ovládacím prvku FormView. Tato událost se často používá k úpravě hodnoty záznamu, než se zobrazí. |
| ItemDeleted | Nastane, když tlačítko Odstranit (tlačítko s jeho **CommandName** vlastnost nastavena na hodnotu "Odstranit") po kliknutí na, ale po ovládacího prvku FormView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| ItemDeleting | Nastane, když po kliknutí na tlačítko odstranit, ale před FormView řízení odstraní záznam ze zdroje dat. Tato událost se často používá pro operaci odstraňování zrušíte. |
| ItemInserted | Nastane, když tlačítko pro vložení (tlačítko s jeho **CommandName** vlastnost nastavena na hodnotu "Insert") po kliknutí na, ale po ovládacího prvku FormView vloží záznamu. Tato událost se často používá ke kontrole výsledky operace insert. |
| ItemInserting | Nastane, když po kliknutí na tlačítko pro vložení, ale před FormView řízení vloží záznamu. Tato událost se často používá ke zrušení operace insert. |
| ItemUpdated | Nastane, když na tlačítko Aktualizovat (tlačítko s jeho **CommandName** vlastnost nastavena na hodnotu "Update") po kliknutí na, ale po ovládacího prvku FormView aktualizací řádek. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| ItemUpdating | Nastane, když po kliknutí na tlačítko Aktualizovat na, ale před FormView aktualizace ovládacího prvku záznamu. Tato událost se často používá ke zrušení operace aktualizace. |
| ModeChanged | Vyskytne se po změně režimu ovládacího prvku FormView (pro úpravy, insert nebo režimu jen pro čtení). Tato událost se často používá k provedení úloh při změně režimu FormView ovládacího prvku. |
| ModeChanging | Nastane před změně režimu ovládacího prvku FormView (pro úpravy, insert nebo režimu jen pro čtení). Tato událost se často používá ke změně režimu zrušit. |
| PageIndexChanged | Nastane při kliknutí na jednu z tlačítka stránkování, ale po ovládacího prvku FormView zpracovává operaci stránkování. Tato událost se běžně používá, když potřebujete provést úlohu po uživatel přejde na jiný záznam v ovládacím prvku. |
| PageIndexChanging | Nastane, když po kliknutí na jednu z tlačítka stránkování, ale před FormView ovládací prvek zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |

## <a name="detailsview"></a>DetailsView

Ovládací prvek DetailsView se používá k zobrazení jeden záznam ze zdroje dat v tabulce, kde každé pole záznamu se zobrazí v řádku tabulky. Můžete použít v kombinaci s ovládacím prvkem GridView pro scénáře podrobností. Ovládací prvek DetailsView podporuje následující funkce:

- Vytvoření vazby na data ovládací prvky zdroj, například SqlDataSource.
- Integrované funkce vkládání.
- Předdefinované aktualizace a odstranění možnosti.
- Integrované funkce stránkování.
- Programový přístup k modelu objektu DetailsView dynamicky nastavit vlastnosti, zpracování událostí a tak dále.
- Vzhled lze přizpůsobit pomocí motivů a stylů.

## <a name="row-fields"></a>Řádková pole

Každý řádek dat v ovládacím prvku DetailsView je vytvořen deklarací ovládací prvek. Typy polí jiný řádek určuje chování řádků v ovládacím prvku. Ovládací prvky pole odvozen od DataControlField. Následující tabulka uvádí typy polí různých řádků, které lze použít.

| **Typ pole sloupců** | **Popis** |
| --- | --- |
| BoundField | Zobrazí hodnotu pole ve zdroji dat jako text. |
| ButtonField | Zobrazí příkazového tlačítka v ovládacím prvku DetailsView. To umožňuje zobrazit řádek s vlastního ovládacího prvku, například Add, nebo tlačítko Odebrat. |
| Vlastnost CheckBoxField | V ovládacím prvku DetailsView zobrazí zaškrtávací políčko. Tento typ pole řádku se často používá ke zobrazení polí se logická hodnota. |
| CommandField | Zobrazí předdefinované příkazového tlačítka provést úpravy, insert nebo delete operace v ovládacím prvku DetailsView. |
| HyperLinkField | Zobrazí hodnotu pole ve zdroji dat jako hypertextový odkaz. Tento typ pole řádku umožňuje druhé pole vázat na adresu URL na hypertextový odkaz. |
| ImageField | Zobrazí obrázek v ovládacím prvku DetailsView. |
| TemplateField | Zobrazí obsah uživatelem definované pro řádek v ovládacím prvku DetailsView podle zadané šablony. Tento typ pole řádku vám umožní vytvořit vlastní řádkového pole. |

Ve výchozím nastavení, je vlastnost AutoGenerateRows nastavena **true**, který automaticky vytvoří objekt vázané řádek pole pro každé pole vazbu typu ve zdroji dat. Platné typy vazbu jsou řetězce, data a času, Decimal, identifikátor Guid a sadu primitivní typy. Každé pole se následně zobrazí v řádku, jako text v pořadí, ve kterém se zobrazí každé pole v datovém zdroji.

Automatické generování řádky poskytuje rychlý a snadný způsob, jak zobrazit každé pole v záznamu. Však nutné používat DetailsView ovládacího prvku rozšířené možnosti, které je potřeba explicitně deklarovat pole řádku, která chcete zahrnout v ovládacím prvku DetailsView. Chcete deklarovat pole řádku, nastavte nejprve **AutoGenerateRows** vlastnost **false**. Dál přidejte otvírání a zavírání  **&lt;pole&gt;**  značky mezi počáteční a koncové značky ovládacího prvku DetailsView. Nakonec seznam pole řádku, které chcete zahrnout mezi počáteční a koncovou  **&lt;pole&gt;**  značky. Zadaná pole řádku jsou přidány do kolekce polí v uvedeném pořadí. **Pole** kolekce umožňuje programovou správu pole řádků v ovládacím prvku DetailsView.

> [!NOTE]
> Automaticky generuje řádek, který pole nejsou přidány do kolekce polí.


## <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládací prvek DetailsView mohou být vázány na ovládací prvek zdroje dat, například **SqlDataSource** nebo AccessDataSource, nebo libovolný zdroj dat, který implementuje rozhraní System.Collections.IEnumerable, jako je například System.Data.DataView, System.Collections.ArrayList a System.Collections.Hashtable.

K vytvoření vazby ovládacího prvku DetailsView. typ zdroje dat odpovídající použijte jednu z následujících metod:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte na hodnotu ID ovládacího prvku zdroje dat ovládacího prvku DetailsView nastavenou vlastnost DataSourceID. Ovládací prvek DetailsView automaticky váže k řízení zdrojů zadaná data. Toto je upřednostňovaný způsob vytvoření vazby na data.
- Při vytváření vazby ke zdroji dat, který implementuje **System.Collections.IEnumerable** rozhraní, prostřednictvím kódu programu nastavit vlastnost DataSource ovládacího prvku DetailsView ke zdroji dat a pak zavolejte metodu DataBind.

## <a name="security"></a>Zabezpečení

Tento ovládací prvek slouží k zobrazení vstupu uživatele, která by mohla obsahovat škodlivý klientský skript. Zkontrolujte všechny informace, které se odesílá z klienta pro spustitelný soubor skriptu příkazů SQL a jiný kód, než se zobrazí v aplikaci. Technologie ASP.NET poskytuje funkce ze vstupní žádost o ověření a bloku skriptu HTML vstup uživatele.

## <a name="data-operations"></a>Operace s daty

Ovládací prvek DetailsView nabízí integrované funkce, které uživateli umožňují aktualizovat, odstranit, insert a položky v ovládacím prvku procházet po stránkách. Pokud ovládací prvek DetailsView je vázán na ovládací prvek zdroje dat, můžete ovládací prvek DetailsView využívat zdroje dat možnosti ovládacího prvku a poskytují automatickou aktualizaci, odstranění, vkládání a stránkování funkce.

Ovládací prvek DetailsView může poskytovat podporu pro aktualizaci, odstranění, insert a stránkovací operace s jinými typy zdrojů dat; Nicméně je nutné zadat implementaci pro tyto operace v příslušné obslužnou rutinu.

Ovládací prvek DetailsView může automaticky přidat **CommandField** řádek pole pomocí tlačítka Upravit, odstranit nebo nový nastavením vlastností vlastnost AutoGenerateEditButton, AutoGenerateDeleteButton nebo vlastnost AutoGenerateInsertButton k **true**, v uvedeném pořadí. Na rozdíl od odstranění tlačítko (který odstraní vybraný záznam okamžitě), při kliknutí na tlačítko Upravit nebo nový DetailsView řízení přejde do upravit nebo vložit režim, v uvedeném pořadí. V režimu úprav tlačítko Upravit se nahradí aktualizace a tlačítko Zrušit. Vstupní ovládací prvky, které jsou vhodné pro datový typ tohoto pole (například textové pole nebo ovládací prvek zaškrtávací políčko) se zobrazí hodnotu pole pro uživatele, který chcete upravit. Kliknutím na tlačítko Aktualizovat aktualizace záznamu ve zdroji dat, při kliknutí na tlačítko Storno opustí žádné změny. Podobně v režimu vkládání tlačítko nové je nahradí typu vložení a tlačítko Zrušit a prázdný vstupní ovládací prvky jsou zobrazeny pro uživatele a zadejte hodnoty pro nový záznam.

Ovládací prvek DetailsView poskytuje funkci stránkování, která umožňuje uživatelům přejděte na další záznamy v datovém zdroji. Když je povolené, ovládací prvky pro navigaci stránky se zobrazí v řádku, pager. Pokud chcete povolit stránkování, nastavte vlastnost AllowPaging na **true**. Pager řádku lze přizpůsobit pomocí vlastnosti PagerStyle a PagerSettings.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Můžete přizpůsobit vzhled ovládacího prvku DetailsView nastavením vlastnosti stylu pro různé části ovládacího prvku. Následující tabulka uvádí vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| AlternatingRowStyle | Nastavení stylů střídavých řádků dat v ovládacím prvku DetailsView. Když je tato vlastnost nastavena, řádky dat se zobrazí střídavých mezi nastavení RowStyle a **AlternatingRowStyle** nastavení. |
| CommandRowStyle | Nastavení stylu pro řádek, který obsahuje integrovaný příkaz tlačítek v ovládacím prvku DetailsView. |
| EditRowStyle | Styl nastavení pro řádky dat při řízení DetailsView je v režimu úprav. |
| EmptyDataRowStyle | Nastavení stylu řádku prázdný dat v ovládacím prvku DetailsView zobrazí, když zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí DetailsView ovládacího prvku. |
| HeaderStyle | Nastavení stylu pro řádek záhlaví DetailsView ovládacího prvku. |
| InsertRowStyle | Styl nastavení pro řádky dat po v ovládacím prvku DetailsView vložit režimu. |
| PagerStyle | Nastavení stylu pro řádek pager DetailsView ovládacího prvku. |
| RowStyle | Styl nastavení pro řádky dat v ovládacím prvku DetailsView. Když **AlternatingRowStyle** také vlastnost nastavena, řádky dat se zobrazují střídavých mezi **RowStyle** nastavení a **AlternatingRowStyle** nastavení. |
| FieldHeaderStyle | Nastavení stylu záhlaví sloupce DetailsView ovládacího prvku. |

## <a name="events"></a>Události

Ovládací prvek DetailsView poskytuje několik událostí, které můžete naprogramovat oproti. To umožňuje spouštět vlastní rutina vždy, když dojde k události. Následující tabulka uvádí události DetailsView ovládacím prvkem podporována. Tyto události ovládacího prvku DetailsView také dědí z jeho základních tříd: datové vazby, vycentrovat, Disposed, Init, zatížení, PreRender a vykreslování.

| **Události** | **Popis** |
| --- | --- |
| ItemCommand | Nastane při stisknutí tlačítka v ovládacím prvku DetailsView. |
| ItemCreated | Nastane po vytvoření všechny objekty DetailsViewRow v ovládacím prvku DetailsView. Tato událost se často používá k úpravě hodnoty záznamu, než se zobrazí. |
| ItemDeleted | Nastane při kliknutí na tlačítko odstranit, ale poté, co ovládací prvek DetailsView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| ItemDeleting | Nastane, když po kliknutí na tlačítko odstranit, ale před DetailsView řízení odstraní záznam ze zdroje dat. Tato událost se často používá pro operaci odstraňování zrušíte. |
| ItemInserted | Nastane při kliknutí na tlačítko pro vložení, ale po ovládací prvek DetailsView vloží záznamu. Tato událost se často používá ke kontrole výsledky operace insert. |
| ItemInserting | Nastane, když po kliknutí na tlačítko pro vložení, ale před DetailsView řízení vloží záznamu. Tato událost se často používá ke zrušení operace insert. |
| ItemUpdated | Nastane při kliknutí na tlačítko Aktualizovat na, ale po ovládací prvek DetailsView aktualizací řádek. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| ItemUpdating | Nastane, když po kliknutí na tlačítko Aktualizovat na, ale před DetailsView aktualizace ovládacího prvku záznamu. Tato událost se často používá ke zrušení operace aktualizace. |
| ModeChanged | Vyskytne se po ovládací prvek DetailsView změní režim (upravit, insert nebo režimu jen pro čtení). Tato událost se často používá k provedení úloh při změně režimu DetailsView ovládacího prvku. |
| ModeChanging | Nastane před ovládací prvek DetailsView změní režim (upravit, insert nebo režimu jen pro čtení). Tato událost se často používá ke změně režimu zrušit. |
| PageIndexChanged | Nastane po kliknutí tlačítka stránkování na, ale po ovládací prvek DetailsView zpracovává operaci stránkování. Tato událost se běžně používá, když potřebujete provést úlohu po uživatel přejde na jiný záznam v ovládacím prvku. |
| PageIndexChanging | Nastane, když po kliknutí na jednu z tlačítka stránkování, ale před DetailsView ovládací prvek zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |

## <a name="the-menu-control"></a>Ovládací prvek nabídky

Ovládací prvek nabídky v technologii ASP.NET 2.0 je navržený jako navigační plnohodnotný systém. Vycentrovat snadno ke zdrojům dat hierarchické například SiteMapDataSource, může být.

Ovládací prvky struktury nabídky lze definovat deklarativně nebo dynamicky a skládá se z jediném kořenovém uzlu libovolný počet podřízených uzlů. Následující kód definuje deklarativně nabídky pro ovládací prvek nabídky.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

V předchozím příkladu je uzlu Home.aspx kořenového uzlu. Všechny ostatní uzly jsou vnořeny do kořenového uzlu na různých úrovních.

Existují dva typy nabídek, které může vykreslit ovládacího prvku nabídka; nabídky statické a dynamické nabídky. Statické nabídky se skládá z položky nabídky, které jsou vždy viditelné. Dynamické nabídky se skládá z položky nabídky, které jsou viditelné pouze tehdy, jestliže uživatel nachází nad nimi pomocí myši. Zákazníci mohou často matou statické nabídky s nabídkami definované deklarativně a dynamické nabídky s nabídkami, které jsou Vycentrovat za běhu. Ve skutečnosti dynamických a statických nabídky se nevztahují ke metodu základního souboru. Podmínky *statické* a *dynamické* odkazovat pouze na to, jestli je v nabídce staticky zobrazené ve výchozím nastavení nebo pouze zobrazí, když uživatel provede některé akci.

**Vlastnost StaticDisplayLevels** vlastnost se používá ke konfiguraci, kolik úrovní v nabídce jsou statické a proto zobrazené ve výchozím nastavení. V předchozím příkladu, nastavení **vlastnost StaticDisplayLevels** vlastnost na hodnotu 2 by způsobilo nabídce staticky zobrazíte uzlu domovské uzlu Hudba a filmy uzlu. Všechny ostatní uzly by dynamicky zobrazena, pokud nastavení ukazatele myši nadřazený uzel.

**Vlastností MaximumDynamicDisplayLevels** vlastnost nakonfiguruje maximální počet dynamické úrovně je schopná zobrazit v nabídce. Žádné dynamické nabídky na vyšší úrovni než hodnotu zadanou pomocí **vlastností MaximumDynamicDisplayLevels** vlastnost se zahodí.

> [!NOTE]
> Je téměř jisté, že můžete narazit situacích, kde nabídky nezobrazí k vykreslení kvůli vlastnost vlastností MaximumDynamicDisplayLevels. V takových případech ověřte, zda vlastnost dostatečně povolit pro zobrazení v nabídkách zákazníků.


## <a name="data-binding-the-menu-control"></a>Datové vazby ovládacího prvku nabídka

Ovládací prvek nabídky mohou být vázány na libovolný zdroj dat hierarchické například SiteMapDataSource nebo prvku. SiteMapDataSource je metoda mohou nejčastěji používané pro datovou vazbu k ovládacímu prvku nabídky, protože ji informační kanály z souboru Web.sitemap a jeho schéma poskytuje známé rozhraní API do ovládacího prvku nabídky. Seznam dole ukazuje jednoduchý soubor Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Všimněte si, že existuje pouze jeden kořenový element siteMapNode, v takovém případě element domovské. Pro každý siteMapNode lze nakonfigurovat několik atributů. Nejčastěji používané atributy jsou:

- **Adresa URL** Určuje adresu URL pro zobrazení, když uživatel klikne na položku nabídky. Pokud tento atribut neexistuje, bude uzel jednoduše odeslána zpět při kliknutí na.
- **název** Určuje text, který se zobrazí na položku nabídky.
- **Popis** použít jako dokumentace pro uzel. Také zobrazuje jako popis tlačítka, při přesunutí myši je při přechodu myší na uzlu.
- **siteMapFile** umožňuje vnořené sitemap. Tento atribut musí odkazovat ve správném formátu mapy webu ASP.NET.
- **role** umožňuje vzhled uzlu kontrolován oříznutí zabezpečení technologie ASP.NET.

Upozorňujeme, že při tyto atributy jsou nepovinné, chování v nabídce nemusí být co se očekává, pokud nejsou zadané. Například pokud *adresa url* zadán atribut ale *popis* atribut není, uzlu není k dispozici, a budou existovat žádný způsob, jak přejděte na zadanou adresu URL.

## <a name="controlling-a-menus-operation"></a>Řízení operace nabídky

Existuje několik vlastností, které ovlivňují činnost ovládací prvek ASP.NET nabídky; **orientaci** vlastnost, **DisappearAfter** vlastnost, **StaticItemFormatString** vlastnost a **StaticPopoutImageUrl**vlastnost je jen několik z nich.

- **Orientaci** může být nastaven na hodnotu *vodorovné* nebo *svislé* a určuje, zda jsou nastíněny vodorovně v řádku, nebo svisle a skládaný při statické položky nabídky jiné. Tato vlastnost nemá vliv na dynamické nabídky.
- **DisappearAfter** vlastnost konfiguruje, jak dlouho dynamická nabídka by měla zůstat viditelné po myši přesunout mimo ni. Hodnota určená v milisekundách a výchozí hodnota je 500. Nastavení této vlastnosti na hodnotu-1 způsobí, že v nabídce nikdy zmizí automaticky. V takovém případě nabídce pouze zmizí, když uživatel klikne mimo v nabídce.
- **StaticItemFormatString** vlastnost usnadňuje udržovat konzistentní tento problém v systému nabídky. Při zadávání tuto vlastnost *{0}* místo popis, který se zobrazí v datovém zdroji, musí být zadán. Například aby bylo možné používat položku nabídky z cvičení 1 indikované navštivte naše stránky produktů, atd., zadáte navštivte náš {0} stránku StaticItemFormatString. Za běhu nahradí ASP.NET kterýkoli z výskytů {0} správný popis pro položku nabídky.
- **StaticPopoutImageUrl** vlastnost určuje bitovou kopii, která slouží k označení, že má uzel konkrétní nabídky podřízené uzly, které jsou přístupné podržením ukazatele nad ním. Dynamické nabídky bude nadále používat výchozí image.

## <a name="templated-menu-controls"></a>Ovládací prvky podle šablony nabídky

Ovládací prvek nabídky je šablonované ovládacího prvku a umožňuje pro dva různé šablon položek; třídu StaticItemTemplate a třídu DynamicItemTemplate. Pomocí těchto šablon, můžete snadno přidat ovládací prvky serveru nebo uživatelských ovládacích prvků do vaší nabídky.

Chcete-li upravit šablony v sadě Visual Studio .NET, klikněte na tlačítko inteligentních značek v nabídce a vybrat upravit šablony. Potom můžete mezi úpravy třídu StaticItemTemplate nebo třídu DynamicItemTemplate.

Všechny ovládací prvky přidány do třídu StaticItemTemplate se zobrazí v nabídce statické při načtení stránky. Všechny ovládací prvky přidány na třídu DynamicItemTemplate se zobrazí na všechny místní nabídky.

## <a name="menu-events"></a>Události nabídky

Ovládací prvek nabídky má dvě události, které jsou jedinečné pro. **MenuItemClicked** a **MenuItemDatabound** událostí.

MenuItemClicked událost se vyvolá, když po kliknutí na položku nabídky. MenuItemDatabound událost se vyvolá, když je Vycentrovat položku nabídky. **MenuEventArgs** předá událost obslužné rutiny poskytuje přístup k položce nabídky přes vlastnost položky.

## <a name="controlling-a-menus-appearance"></a>Řízení vzhledu nabídky

Také může ovlivnit vzhled ovládacího prvku nabídka použití jednoho nebo více mnoho stylů, které jsou k dispozici do formátu nabídek. Mezi ně patří **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**a **DynamicHoverStyle**. Tyto vlastnosti jsou konfigurováni pomocí standardní řetězec stylu HTML. Například následující ovlivní styl pro dynamické nabídky.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Pokud používáte některou Hover stylů, budete muset přidat &lt;head&gt; element v na stránku s *runat* element nastaven na hodnotu *server*.


Ovládací prvky nabídky také podporují použití motivů technologii ASP.NET 2.0.

## <a name="the-treeview-control"></a>TreeView – ovládací prvek

TreeView – ovládací prvek zobrazí data ve struktuře stromu typu. Stejně jako u ovládacího prvku nabídka, může být snadno data vázaná na libovolný zdroj hierarchické dat, jako je například SiteMapDataSource.

První otázku, která je pravděpodobně požádat o TreeView – ovládací prvek v technologii ASP.NET 2.0 zákazníků se, zda je spojený s WebControl TreeView aplikace Internet Explorer, která byla k dispozici pro technologii ASP.NET 1.x. Není. Ovládací prvek ASP.NET 2.0 TreeView byla zapsána od základů a nabízí výrazné zlepšení než TreeView WebControl aplikace Internet Explorer, který byl dříve k dispozici.

Nebude přejít na podrobnosti o tom, jak vytvořit vazbu ovládacího prvku TreeView na mapy webu, jak se provádí v stejným způsobem jako ovládací prvek nabídky. TreeView – ovládací prvek má však několik různých rozdílů takovým způsobem, který funguje.

Ve výchozím nastavení zobrazí se plně rozšířené TreeView – ovládací prvek. Chcete-li změnit úroveň rozšíření při počáteční zatížení, změňte **ExpandDepth** vlastností ovládacího prvku. To je zvlášť důležité v případech, kdy je Vycentrovat při rozšiřování konkrétní uzly v ovládacím prvku TreeView.

## <a name="databinding-the-treeview-control"></a>Datové vazby ovládacím prvku TreeView

Na rozdíl od řízení nabídky ovládacím prvku TreeView různě i pro zpracování velkých objemů dat. Kromě datové vazby k SiteMapDataSource nebo prvku, stromovém zobrazení je proto často data vázaná na datovou sadu nebo jiná relační data. V případech, kde TreeView – ovládací prvek vázán na velké objemy dat je nejvhodnější pro vazbu pouze data, která je ve skutečnosti viditelné v ovládacím prvku. Můžete pak dat vytvořit vazbu k dalším datům, jsou rozbalené uzly TreeView.

V těchto případech **PopulateOnDemand nastavte** vlastnost ovládacího prvku TreeView by měla být nastavená na *true*. Pak musíte poskytnout implementaci pro **TreeNodePopulate** metoda.

## <a name="data-binding-without-postback"></a>Datové vazby bez zpětné volání

Všimněte si, že když rozšiřujete první uzel v předchozím příkladu, stránky odesílá data zpět a aktualizuje. Thats nejedná se o problém v tomto příkladu, ale můžete Představte si, že může být v produkčním prostředí s velké množství dat. Lepší scénář bude jeden ve kterém stromovém zobrazení by stále dynamicky naplnit jeho uzlů, ale bez metodu POST směřující zpět na server.

Nastavením **PopulateNodesFromClient** a **PopulateOnDemand nastavte** vlastnosti na hodnotu true, ovládacím prvku ASP.NET TreeView dynamicky naplníte uzly bez příspěvku na zpět. Pokud není rozbalen nadřazený uzel, z klienta se provádí požadavek XMLHttp a OnTreeNodePopulate událost je aktivována. Server odpoví island data XML, které se pak použije k datům vazby podřízených uzlů.

ASP.NET vytvoří dynamicky kód klienta, který implementuje tuto funkci. &lt;Skriptu&gt; značky, které obsahují skript se generují odkazující na soubor AXD. Například výpis níže ukazuje skript odkazy pro kód skriptu, který generuje XMLHttp žádosti.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Pokud procházet soubor AXD výše v prohlížeči a otevřete jej, zobrazí se kód, který implementuje XMLHttp žádosti. Tato metoda zabraňuje zákazníkům upravování souboru skriptu.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Řízení operaci TreeView – ovládací prvek

TreeView – ovládací prvek má několik vlastností, které ovlivňují činnost ovládacího prvku. Nejobvyklejší vlastnosti jsou **ShowCheckBoxes**, **ShowExpandCollapse**, a **ShowLines**.

**ShowCheckBoxes** vlastnost ovlivňuje, jestli uzly zobrazí zaškrtávací políčko při vykreslení. Platné hodnoty pro tuto vlastnost jsou **žádné**, **kořenové**, **nadřazené**, **listu**, a **všechny**. TreeView – ovládací prvek tato ovlivňují následujícím způsobem:

| **Hodnota vlastnosti** | **Platnost** |
| --- | --- |
| Žádné | Zaškrtávací políčka nejsou zobrazeny na všechny uzly. Toto je výchozí nastavení. |
| kořenové | Zaškrtávací políčko se zobrazí pouze na kořenový uzel. |
| Nadřazené | Zaškrtávací políčko se zobrazí jenom v těchto uzlech, které mají podřízené uzly. Tyto podřízené uzly může být nadřazených uzlů nebo uzlů listu. |
| Listu | Zaškrtávací políčko se zobrazí pouze na ty uzly, které mít žádné podřízené uzly. |
| Všechny | Zaškrtávací políčko se zobrazí na všech uzlech. |

Pokud zaškrtávací políčka se používají, **CheckedNodes** vlastnost vrátí kolekce TreeView uzlů, které se kontroluje na zpětné volání.

**ShowExpandCollapse** vlastnost řídí vzhled obrázku Rozbalit/sbalit vedle kořenové a nadřazené uzly. Pokud je tato vlastnost nastavena na **false**, TreeView uzly se vykresluje jako hypertextové odkazy a jsou rozbalené/sbalené kliknutím na odkaz.

**ShowLines** vlastnost určuje, zda se zobrazí čáry propojíte podřízené uzly nadřazených uzlů. Když **false** (výchozí), zobrazí se žádné řádky. Když **true**, TreeView – ovládací prvek bude používat Image řádků ve složce určeného **LineImagesFolder** vlastnost.

Přizpůsobení vzhledu řádků TreeView, Visual Studio .NET 2005 zahrnuje nástroj Návrhář řádku. Tento nástroj, pomocí tlačítka inteligentních značek v ovládacím prvku TreeView jako níže můžete přistupovat.


![](data-bound-controls/_static/image1.jpg)

**Obrázek 1**


Když vyberte **přizpůsobení bitové kopie řádku** nabídky, nástroj Návrhář řádek se spustí lze konfigurovat vzhled TreeView řádky.

## <a name="treeview-events"></a>TreeView – události

TreeView – ovládací prvek má jedinečný následující události:

- SelectedNodeChanged dojde, když je vybrán uzel na základě **SelectAction** vlastnost.
- TreeNodeCheckChanged nastane při změně stavu checkboxs uzlů.
- Události TreeNodeExpanded nastane, když se uzel rozbalí na základě **SelectAction** vlastnost.
- TreeNodeCollapsed nastane, když je sbalený uzlu.
- TreeNodeDataBound nastane, když se data uzlu vázána.
- TreeNodePopulate nastane, když se naplní uzlu.

**SelectAction** vlastnost umožňuje nakonfigurovat, která událost je aktivována, když je vybrán uzel. Vlastnost SelectAction poskytuje následujících akcí:

- TreeNodeSelectAction.Expand vyvolá události TreeNodeExpanded Pokud je vybrán uzel.
- TreeNodeSelectAction.None vyvolá žádná událost, pokud je vybrán uzel.
- TreeNodeSelectAction.Select vyvolá SelectedNodeChanged událost, pokud je vybrán uzel.
- TreeNodeSelectAction.SelectExpand vyvolá událost SelectedNodeChanged a události TreeNodeExpanded událost, pokud je vybrán uzel.

## <a name="controlling-appearance-with-styles"></a>Řízení vzhledu s styly

TreeView – ovládací prvek poskytuje mnoho vlastností pro řízení vzhledu ovládacího prvku s styly. Následující vlastnosti jsou k dispozici.

| **Název vlastnosti** | **Ovládací prvky** |
| --- | --- |
| HoverNodeStyle | Pokud je při přechodu myší myši nad nimi ovládací prvky stylu uzlů. |
| LeafNodeStyle | Určuje styl uzlů listu. |
| NodeStyle | Určuje styl pro všechny uzly. Styly konkrétním uzlu (například LeafNodeStyle), potlačí tento styl. |
| ParentNodeStyle | Určuje styl pro všechny nadřazené uzly. |
| RootNodeStyle | Určuje styl pro kořenový uzel. |
| SelectedNodeStyle | Určuje styl pro vybraný uzel. |

Každá z těchto vlastností je jen pro čtení. Však se každý vrátit **TreeNodeStyle** objekt a vlastnosti daného objektu můžete změnit pomocí *dílčí vlastnost vlastnosti* formátu. Chcete-li například nastavit **ForeColor** vlastnost **SelectedNodeStyle**, použijte následující syntaxi:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Všimněte si, že není uzavřený značce výše. Je to způsobeno při pomocí deklarativní syntaxe tady uvedené, bude zahrnovat TreeViews uzly v kód HTML.

Vlastnosti stylu lze zadat také v kódu pomocí *vlastnost.DílčíVlastnost* formátu. Chcete-li například nastavit **ForeColor** vlastnost **RootNodeStyle** v kódu, použijte následující syntaxi:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Kompletní seznam vlastností jiný styl najdete v MSDN dokumentaci na objekt TreeNodeStyle.


## <a name="the-sitemappath-control"></a>Ovládací prvek SiteMapPath

Ovládací prvek SiteMapPath poskytuje ovládací prvek navigace mezi aktivní adresní chléb pro vývojáře využívající technologii ASP.NET. Podobně jako další ovládací prvky pro navigaci může se jednat snadno data vázané na data hierarchické zdrojů například SiteMapDataSource nebo prvku.

Ovládací prvek SiteMapPath se skládá z SiteMapNodeItem objekty. Existují tři typy uzlů; Kořenový uzel, nadřazených uzlů a na aktuální uzel. Kořenový uzel je uzel v horní části hierarchická struktura. Aktuální uzel představuje aktuální stránku. Všechny ostatní uzly jsou nadřazených uzlů.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Řízení operaci SiteMapPath ovládacího prvku

Vlastnosti, které řídí operaci ovládací prvek SiteMapPath jsou následující:

| **Vlastnost** | **Popis vlastnosti** |
| --- | --- |
| ParentLevelsDisplayed | Určuje, kolik nadřazené uzly jsou zobrazeny. Výchozí hodnota je -1, který ukládá žádné omezení počtu zobrazí nadřazené uzly. |
| PathDirection | Určuje směr SiteMapPath. Platné hodnoty jsou RootToCurrent (výchozí) a CurrentToRoot. |
| PathSeparator | Řetězec, který řídí znak oddělující uzly v ovládacím prvku SiteMapPath. Výchozí hodnota je:. |
| RenderCurrentNodeAsLink | Logická hodnota, která určuje, zda je aktuální uzel vykresleno jako odkaz. Výchozí hodnota je False. |
| SkipLinkText | Pomáhá s usnadnění při prohlížení stránky pomocí čtečky obrazovky. Tato vlastnost umožňuje čtečky obrazovky tak, aby přeskočil ovládací prvek SiteMapPath. Tuto funkci zakázat, nastavte vlastnost na String.Empty. |

## <a name="templated-sitemappath-controls"></a>Ovládací prvky podle šablony SiteMapPath

SiteMapControl je šablonované ovládacího prvku a jako takový, můžete definovat různé šablony pro použití v zobrazení ovládacího prvku. Chcete-li upravit šablony v ovládacím prvku SiteMapPath, klikněte na tlačítko inteligentních značek na ovládací prvek a vyberte v nabídce Upravit šablony. Zobrazí se v nabídce SiteMapTasks jak je uvedeno níže, kde si můžete vybrat mezi různé šablony, které jsou k dispozici.


![](data-bound-controls/_static/image2.jpg)

**Obrázek 2**


**NodeTemplate** šablony odkazuje na každém uzlu SiteMapPath. Pokud uzel je kořenový uzel nebo aktuálním uzlu a **RootNodeTemplate** nebo **CurrentNodeTemplate** je nakonfigurován, NodeTemplate přepsána.

## <a name="sitemappath-events"></a>SiteMapPath události

Ovládací prvek SiteMapPath má dvě události, které nejsou odvozené z třídy ovládacího prvku. **ItemCreated** událostí a **ItemDataBound událost** událostí. ItemCreated událost se vyvolá, když vytvoření SiteMapPath položky. ItemDataBound událost se vyvolá při volání metody DataBind během vazby dat SiteMapPath uzlu. A **SiteMapNodeItemEventArgs** objekt poskytuje přístup ke konkrétní SiteMapNodeItem přes vlastnost položky.

## <a name="controlling-appearance-with-styles"></a>Řízení vzhledu s styly

Následující styly jsou k dispozici pro formátování ovládacího prvku SiteMapPath.

| **Název vlastnosti** | **Ovládací prvky** |
| --- | --- |
| CurrentNodeStyle | Určuje styl textu pro aktuální uzel. |
| RootNodeStyle | Určuje styl textu pro kořenový uzel. |
| NodeStyle | Určuje styl textu pro všechny uzly za předpokladu, že se netýká CurrentNodeStyle nebo RootNodeStyle. |

Vlastnost NodeStyle přepsána CurrentNodeStyle nebo RootNodeStyle. Každý z těchto vlastností je jen pro čtení a vrátí **styl** objektu. Pokud chcete ovlivnit vzhled uzlu pomocí jedné z těchto vlastností, musíte nastavit vlastnosti stylu objekt, který je vrácen. Například následující kód změní vlastnost forecolor aktuální uzel.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Vlastnost lze použít také prostřednictvím kódu programu následujícím způsobem:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Pokud se použije šablona, styl se nepoužije.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Laboratoř 1: Konfigurace ovládacího prvku Menu technologie ASP.NET

1. Vytvoření nového webu.
2. Přidejte soubor mapy webu vyberete soubor, nový, souboru a zvolíte mapy webu ze seznamu šablon souborů.
3. Otevřete mapy webu (Web.sitemap ve výchozím nastavení) a upravit ho tak, aby vypadal jako seznam níže. Na stránkách, ke kterým se připojujete v souboru mapy webu skutečně neexistují, ale který nebude problém pro toto cvičení.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Otevřete výchozího webového formuláře v zobrazení návrhu.
5. Z části navigačního panelu nástrojů přidáte nový ovládací prvek nabídky na stránku.
6. Z části datové sady nástrojů přidejte nový SiteMapDataSource. SiteMapDataSource budou automaticky používat soubor Web.sitemap ve vaší lokalitě. (Soubor Web.sitemap *musí* v kořenové složce webu.)
7. Klikněte na ovládací prvek nabídky a klikněte na tlačítko inteligentních značek k zobrazení dialogu nabídce úlohy.
8. V rozevírací nabídce vyberte zdroj dat vyberte SiteMapDataSource1.
9. Klikněte na odkaz automatického formátování a zvolte formát pro nabídce.
10. V podokně vlastností nastavit **vlastnost StaticDisplayLevels** vlastnost na hodnotu 2. Ovládací prvek nabídky by měl nyní zobrazovat uzlu Home, produkty a služby v návrháři.
11. Přejděte na stránku v prohlížeči, aby pomocí nabídky. (Vzhledem k tomu, že jste již přidali do mapy webu stránek nejsou ve skutečnosti, zobrazí se chyba při akci a přejděte k nim.)

Vyzkoušejte změna vlastností MaximumDynamicDisplayLevels vlastností a vlastnost StaticDisplayLevels a zobrazit jejich vliv vykreslení v nabídce.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Laboratoř 2: Dynamické vazby ovládacího prvku TreeView

Toto cvičení předpokládá, že máte SQL Server je spuštěn místně a v instanci systému SQL Server nachází databáze Northwind. Pokud nejsou tyto podmínky splněny, změňte připojovací řetězec v ukázce. Všimněte si, že může také musíte zadat ověřování serveru SQL Server namísto připojení důvěryhodná.

1. Vytvoření nového webu.
2. Přepněte do zobrazení kódu pro Default.aspx a všechny kódu nahraďte kód uvedený níže. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Uložte stránky jako treeview.aspx.
4. Přejděte na stránku.
5. Při prvním zobrazení stránky, zobrazte si zdroj stránku v prohlížeči. Všimněte si, že pouze viditelné uzly, které byly odeslány klientovi.
6. Klikněte na symbol plus vedle libovolného uzlu.
7. Zobrazení zdroje na stránce znovu. Všimněte si, že jsou nyní k dispozici nově zobrazených uzlů.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Testovacím 3: Zobrazení a úpravy dat pomocí GridView a DetailsView podrobnosti o

1. Vytvoření nového webu.
2. Přidáte nový soubor web.config na web.
3. Přidáte připojovací řetězec v souboru web.config, jak je uvedeno níže: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Potřebujete změnit připojovací řetězec založený na prostředí.
4. Uložte a zavřete soubor web.config.
5. Otevřete soubor Default.aspx a přidejte nový ovládací prvek SqlDataSource.
6. Změňte ID ovládacího prvku SqlDataSource k **produkty**.
7. V **Úkoly SqlDataSource** nabídky, klikněte na tlačítko **konfigurace zdroje dat**.
8. Vyberte **Northwind** v rozevírací nabídce připojení a klikněte na tlačítko Další.
9. Vyberte **produkty** z **název** rozevírací seznam a zkontrolujte **ProductID**, **ProductName**, **UnitPrice**, a **JednotkyNaSkladě** zaškrtávací políčka, jak je uvedeno níže. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Klikněte na tlačítko **Další**.
11. Klikněte na tlačítko **Dokončit**.
12. Přepnout na zobrazení zdroje a prozkoumat kód, který byl vygenerován. Upozornění **SelectCommand**, **DeleteCommand**, **událost InsertCommand**, a **UpdateCommand** které byly přidány do SqlDataSource ovládací prvek. Všimněte si také parametry, které byly přidány.
13. Umožňuje přepnout do zobrazení návrhu a přidejte nový ovládací prvek GridView na stránku.
14. Vyberte **produkty** z **zvolit zdroj dat** rozevíracího seznamu.
15. Zkontrolujte **povolit stránkování** a **povolení výběru** jak je uvedeno níže. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Klikněte **upravit sloupce** propojit a ujistěte se, že **automaticky generovat pole** je zaškrtnuté.
17. Click **OK**.
18. Pomocí ovládacího prvku GridView vybraný, klikněte na tlačítko Další na **DataKeyNames** vlastnosti v podokně Vlastnosti.
19. Vyberte **ProductID** z **dostupné datová pole** seznamu a klikněte na tlačítko  **&gt;**  tlačítko Přidat.
20. Klikněte na tlačítko OK.
21. Přidání nové SqlDataSource ovládacího prvku na stránku.
22. Změňte ID ovládacího prvku SqlDataSource k **podrobnosti**.
23. V nabídce úlohy SqlDataSource zvolte **konfigurace zdroje dat**.
24. Zvolte **Northwind** z rozevíracího seznamu a klikněte na tlačítko **Další**.
25. Vyberte **produkty** z **název** rozevírací seznam a zkontrolujte  **\***  zaškrtnout políčko **sloupce** listbox.
26. Klikněte **kde** tlačítko.
27. Vyberte **ProductID** z **sloupec** rozevíracího seznamu.
28. Vyberte  **=**  v rozevírací nabídce operátor.
29. Vyberte **řízení** z **zdroj** rozevíracího seznamu.
30. Vyberte **GridView1** z **ID ovládacího prvku** rozevíracího seznamu.
31. Klikněte **přidat** tlačítko přidáte klauzuli WHERE.
32. Click **OK**.
33. Klikněte **Upřesnit** tlačítko a zaškrtněte políčko **Generovat příkazy INSERT, UPDATE a DELETE** zaškrtávací políčko.
34. Click **OK**.
35. Klikněte na tlačítko **Další** a klikněte na tlačítko **Dokončit**.
36. Přidání ovládacího prvku DetailsView na stránku.
37. V **zvolit zdroj dat** rozevíracího seznamu, vyberte **podrobnosti**.
38. Zkontrolujte **povolit úpravy** zaškrtávací políčko, jak je uvedeno níže. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Uložit stránky a procházet Default.aspx.
40. Klikněte **vyberte** odkaz vedle různé záznamy zobrazíte DetailsView aktualizace automaticky.
41. Klikněte **upravit** odkaz v ovládacím prvku DetailsView.
42. Změňte záznam a klikněte na tlačítko **aktualizace**.
