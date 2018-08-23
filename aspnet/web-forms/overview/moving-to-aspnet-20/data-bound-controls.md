---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Vázané ovládací prvky dat | Dokumentace Microsoftu
author: microsoft
description: Většina aplikací ASP.NET spoléhá na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data, aby byla pivotal součástí komunikující w...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: b115109c7307d05dc9e620378a51a71407204740
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754389"
---
<a name="data-bound-controls"></a>Vázané ovládací prvky dat
====================
podle [Microsoft](https://github.com/microsoft)

> Většina aplikací ASP.NET spoléhá na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data, aby byla pivotal součástí interakci s daty v dynamické webové aplikace. ASP.NET 2.0 přináší například tato vylepšení ovládacích prvků vázaných na data, včetně nové třídy BaseDataBoundControl a deklarativní syntaxe některé podstatné.


Většina aplikací ASP.NET spoléhá na určitý stupeň prezentace dat ze zdroje dat back-end. Ovládací prvky vázané na data, aby byla pivotal součástí interakci s daty v dynamické webové aplikace. ASP.NET 2.0 přináší například tato vylepšení ovládacích prvků vázaných na data, včetně nové třídy BaseDataBoundControl a deklarativní syntaxe některé podstatné.

BaseDataBoundControl slouží jako základní třída pro třídy DataBoundControl a třída HierarchicalDataBoundControl. V tomto modulu probereme následující třídy, které jsou odvozeny z DataBoundControl:

- Funkce AdRotator
- Ovládací prvky seznamu
- GridView
- FormView
- Prvku DetailsView.

Následující třídy, které jsou odvozeny z třídy třída HierarchicalDataBoundControl jsme také možnosti:

- TreeView
- Nabídka
- SiteMapPath

## <a name="databoundcontrol-class"></a>Třída DataBoundControl

DataBoundControl třída je abstraktní třída (označené MustInherit v jazyce Visual Basic) používaných pro interakci s tabulkové nebo datový list-style. Následující ovládací prvky jsou některé ovládací prvky, které jsou odvozeny z DataBoundControl.

## <a name="adrotator"></a>Funkce AdRotator

Ovládací prvek AdRotator umožňuje zobrazit banner grafické na webové stránce, který je propojený s konkrétní adresy URL. Obrázek, který se zobrazí otočen pomocí vlastností pro ovládací prvek. Četnost konkrétní ad zobrazení na stránce lze konfigurovat pomocí **dojmy** vlastnost a služby Active Directory je možné filtrovat pomocí filtrování klíčových slov.

Funkce AdRotator ovládací prvky použít soubor XML nebo tabulky v databázi pro data. Následující atributy se používají v souborech XML ke konfiguraci AdRotator ovládacího prvku.

### <a name="imageurl"></a>imageUrl
Adresa URL obrázku, který má být zobrazen pro služby ad.

### <a name="navigateurl"></a>NavigateUrl
Adresa URL, která uživateli je třeba se při kliknutí na reklamy. To by měla mít kódování URL.

### <a name="alternatetext"></a>AlternateText
Alternativní text, který je zobrazen v popisku a přečtou čtečky obrazovky. Zobrazí také při určeném ImageUrl bitová kopie není k dispozici.

### <a name="keyword"></a>Klíčové slovo
Definuje klíčové slovo, které se dá použít při pomocí filtrování klíčových slov. -Li zadán, zobrazí se pouze reklamy s klíčovým slovem odpovídající filtr klíčových slov.

### <a name="impressions"></a>Dojmy
Číslo váhu, která určuje, jak často se pravděpodobně se zobrazí konkrétní ad. To je relativní vzhledem k dojem další reklamy ve stejném souboru. Maximální hodnota kolektivní dojmy pro všechny služby Active Directory v souboru XML je 2,048,000,000 1.

### <a name="height"></a>Výška
Výška ad v pixelech.

### <a name="width"></a>Šířka
Šířka v pixelech služby ad.


> [!NOTE]
> Atributy výšku a šířku přepsat výšku a šířku prvku AdRotator samotného.


Typický soubor XML může vypadat nějak takto:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

Ve výše uvedeném příkladu se dvakrát ad pro společnost Contoso jako pravděpodobně kvůli hodnota atributu dojmy zobrazeny jako ad pro ASP.NET Web.

Chcete-li zobrazit reklamy ze souboru XML výše, přidejte ovládací prvek AdRotator na stránku a nastavit **soubor AdvertisementFile** vlastnost pro odkazování na soubor XML, jak je znázorněno níže:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Pokud budete chtít použít tabulku databáze jako zdroj dat pro ovládací prvek AdRotator, musíte nejdřív vytvořit databázi pomocí následující schéma:

| **Název sloupce** | **Datový typ** | **Popis** |
| --- | --- | --- |
| ID | int | Primární klíč. V tomto sloupci může mít libovolný název. |
| imageUrl | nvarchar (*délka*) | Relativní nebo absolutní adresa URL obrázku má být zobrazen pro služby ad. |
| NavigateUrl | nvarchar (*délka*) | Cílová adresa URL pro ad. Pokud nezadáte hodnotu, není ad hypertextový odkaz. |
| AlternateText | nvarchar (*délka*) | Text zobrazený v případě, že na obrázku se nenašel. Text je v některé prohlížeče zobrazí jako popisek. Alternativní text se také používá pro usnadnění přístupu tak, aby uživatelé, kteří neuvidí grafika slyšet jeho popis číst nahlas. |
| Klíčové slovo | nvarchar (*délka*) | Kategorie pro ad, na kterém můžete na stránce filtrovat. |
| Dojmy | int(4) | Číslo, které určuje pravděpodobnost toho, jak často se zobrazí ad. Větší číslo, tím častěji ad se zobrazí. Celkový součet všech hodnot dojmy v souboru XML nesmí překročit 2 048 000 000-1. |
| Šířka | int(4) | Šířka obrázku v pixelech. |
| Výška | int(4) | Výška obrázku v pixelech. |

V případech, kdy už máte databázi s jiným schématem, můžete použít **AlternateTextField**, **ImageUrlField**, a **NavigateUrlField** vlastnosti mapovat Funkce AdRotator atributy k existující databázi. Pro zobrazení dat z databáze v ovládacím prvku AdRotator, přidejte ovládací prvek zdroje dat na stránku, nakonfigurovat připojovací řetězec pro ovládací prvek zdroje dat tak, aby odkazoval na databázi a nastavit u tohoto prvku AdRotator **DataSourceID** Vlastnost ID ovládacího prvku zdroje dat V případech, kde jsou potřeba ke konfiguraci funkce AdRotator používání služby Active Directory prostřednictvím kódu programu, události AdCreated. Události AdCreated přebírá dva parametry; jeden objekt a druhá instance AdCreatedEventArgs. AdCreatedEventArgs je odkaz na ad, který se vytváří.

Následující fragment kódu nastaví ImageUrl, NavigateUrl a AlternateText pro reklamu prostřednictvím kódu programu:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Ovládací prvky seznamu

Ovládací prvky seznamu zahrnují ListBox, DropDownList, CheckBoxList, RadioButtonList a BulletedList. Každá z těchto ovládacích prvků může představovat data vázaná ke zdroji dat. Používají jedno pole ve zdroji dat jako text k zobrazení a druhé pole můžete volitelně použít jako hodnotu položky. Položky mohou být také staticky přidány v době návrhu a statické položky a dynamické položky přidány zdrojům dat můžete kombinovat.

K datům vytvoření vazby ovládacího prvku seznamu, přidejte ovládací prvek zdroje dat do stránky. Zadejte příkaz SELECT pro ovládací prvek zdroje dat a potom nastavte vlastnost DataSourceID ovládacího prvku seznam ID ovládacího prvku zdroje dat. Použití **DataTextField** a **DataValueField** vlastnosti zadat text k zobrazení a hodnotou pro ovládací prvek. Kromě toho můžete použít **DataTextFormatString** vlastnost určovat vzhled zobrazení textu následujícím způsobem:

| **Výraz** | **Popis** |
| --- | --- |
| Cena: {0:C} | Pro číselný/desetinná data. Zobrazí literál "cena:" a čísel ve formátu měny. Formát měny závisí na nastavení jazykové verze určený v atributu jazykovou verzi na **stránky** direktiv nebo v souboru Web.config. |
| {0:D4} | Pro data o celé číslo. Nelze použít s desetinná čísla. Celá čísla jsou zobrazeny v poli nulami, tj. čtyři široké znaky. |
| {0:N2}% | Pro číselná data. Zobrazuje čísla 2 desetinné přesnosti, za nímž následuje literál "%". |
| {0:000.0} | Pro číselný/desetinná data. Jsou čísla zaokrouhlena na jedno desetinné místo. Čísla menší než tři číslice jsou nulami. |
| {0:D} | Pro data datum a čas. Zobrazí dlouhého formátu data ("čtvrtek, 06 srpna 1996"). Formát data závisí na nastavení jazykové verze na stránce nebo v souboru Web.config. |
| {0:d} | Pro data datum a čas. Zobrazí krátkého formátu ("12/31/99"). |
| {0:yy-MM-dd} | Pro data datum a čas. Zobrazí datum v číselném formátu rok měsíc dní (96-08-06) |

## <a name="gridview"></a>GridView

Prvek GridView umožňuje tabulkových dat zobrazení a úpravy pomocí deklarativní přístup a je následníkem prvku DataGrid. Následující funkce jsou dostupné v ovládacím prvku GridView.

- Vytvoření vazby na data ovládací prvky zdroje, jako je například SqlDataSource.
- Integrované možnosti řazení.
- Integrované aktualizace nebo odstranění funkce.
- Integrované funkce stránkování.
- Možnosti výběru předdefinovaných řádek.
- Programový přístup k objektovému modelu GridView dynamicky nastavit vlastnosti, zpracování událostí a tak dále.
- Více polí klíčů.
- Více datových polí pro sloupce hypertextový odkaz.
- Vzhled lze přizpůsobit pomocí motivů a stylů.

**Pole sloupců**

Každý sloupec v ovládacím prvku GridView je reprezentován objektem typu DataControlField. Ve výchozím nastavení je vlastnost AutoGenerateColumns nastavena na **true**, která vytvoří objekt AutoGeneratedField pro každé pole ve zdroji dat. Každé pole se pak vykreslí jako sloupec v ovládacím prvku GridView v pořadí, ve kterém každé pole se objeví ve zdroji dat. Můžete také ručně řídit sloupec pole se zobrazí v **GridView** ovládacího prvku tak, že nastavíte **AutoGenerateColumns** vlastnost **false** a pak definovat vlastní kolekce pole sloupců. Typy pole jiný sloupec určují chování sloupců v ovládacím prvku.

Následující tabulka uvádí typy pole různých sloupců, které lze použít.

| **Typ pole sloupce** | **Popis** |
| --- | --- |
| Vlastnost BoundField | Zobrazí hodnoty pole ve zdroji dat. Toto je výchozí typ sloupce ovládacího prvku GridView. |
| ButtonField | Zobrazí příkazové tlačítko pro každou položku v ovládacím prvku GridView. To umožňuje vytvořit sloupec tlačítka vlastní ovládací prvky, jako je například přidat nebo odebrat tlačítka. |
| Třída CheckBoxField | Zobrazuje zaškrtávací políčko pro každou položku v ovládacím prvku GridView. Tento typ pole sloupce běžně slouží k zobrazení polí s logickou hodnotou. |
| CommandField | Zobrazí předdefinovaných příkazů k provedení výběru, úprava nebo odstranění operace. |
| HyperLinkField | Zobrazí hodnoty pole ve zdroji dat jako hypertextový odkaz. Tento typ pole sloupce můžete vytvořit vazbu druhé pole k adrese URL hypertextového odkazu. |
| Třídy ImageField | Zobrazí obrázek pro každou položku v ovládacím prvku GridView. |
| TemplateField | Zobrazí obsah uživatelem definované pro každou položku v ovládacím prvku GridView podle určené šablony. Tento typ pole sloupce můžete vytvořit vlastního sloupce pole. |

Chcete-li definovat deklarativně kolekce pole sloupců, nejprve přidat otevírání a zavírání **&lt;sloupce&gt;** značky mezi počátečními a ukončovacími značkami ovládacího prvku GridView. V dalším kroku seznamu pole sloupce, které chcete zahrnout mezi otevírací a zavírací **&lt;sloupce&gt;** značky. Sloupce zadané se přidají do kolekce sloupců v uvedeném pořadí. **Sloupce** kolekce ukládá všechna pole v ovládacím prvku sloupec a umožňuje programově spravovat pole sloupce v ovládacím prvku GridView.

V kombinaci s automaticky generovanou sloupcová pole může být zobrazena pole sloupce explicitně deklarované. V případě obě používají explicitně deklarované sloupcová pole vykresleny nejprve, za nímž následuje pole automaticky vygenerované sloupce.

## <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládací prvek GridView mohou být vázány na ovládací prvek zdroje dat (například **SqlDataSource**, **ObjectDataSource**, a tak dále), stejně jako libovolný zdroj dat, která implementuje System.Collections.IEnumerable rozhraní (například System.Data.DataView, System.Collections.ArrayList nebo System.Collections.Hashtable). Použijte jednu z následujících metod k vytvoření vazby ovládacího prvku GridView. typ zdroje dat odpovídající:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte na hodnotu ID ovládacího prvku zdroje dat vlastnost DataSourceID ovládacího prvku GridView. Ovládací prvek GridView automaticky váže do správy zdrojového kódu určená data a můžou využít výhod zdroje dat možnosti ovládacího prvku na řazení, aktualizaci, odstranění a funkce stránkování. Toto je upřednostňovanou metodou pro připojení k datům.
- K vytvoření vazby ke zdroji dat, která implementuje rozhraní System.Collections.IEnumerable, prostřednictvím kódu programu nastavit vlastnosti DataSource ovládacího prvku GridView ke zdroji dat a potom voláním metody DataBind. Při použití této metody, ovládací prvek GridView neposkytuje integrované řazení, aktualizaci, odstranění a stránkování funkce. Budete muset poskytují tuto funkci.

## <a name="data-operations"></a>Operace s daty

Ovládací prvek GridView poskytuje mnoho předdefinovaných možností, které umožní uživateli řadit, aktualizovat, odstranit, vyberte a stránkovat položek v ovládacím prvku. Když se ovládací prvek GridView vazba na ovládací prvek zdroje dat, ovládacím prvku GridView můžete využít zdroje dat možnosti ovládacího prvku a poskytují automatické řazení, aktualizace nebo odstranění funkce.

> [!NOTE]
> Ovládací prvek GridView může poskytovat podporu pro řazení, aktualizaci a odstraňování u jiných typů zdrojů dat; je však potřeba poskytnout implementaci pro tyto operace odpovídající obslužnou rutinu.


Řazení umožňuje uživateli řadit položky v ovládacím prvku GridView s ohledem na konkrétní sloupce kliknutím na záhlaví sloupce. Pokud chcete povolit řazení, nastavte vlastnost AllowSorting na **true**.

Pokud tlačítko v jsou povoleny funkce Automatické aktualizace, odstranění a výběr **ButtonField** nebo **TemplateField** sloupcové pole s názvem příkaz "Edit", "Odstranit" a "Vyberte", v uvedeném pořadí dojde ke kliknutí na. Automaticky přidat ovládací prvek GridView **CommandField** sloupcové pole s upravit, odstranit nebo vyberte tlačítko, pokud je vlastnost AutoGenerateEditButton, AutoGenerateDeleteButton nebo AutoGenerateSelectButton vlastnost nastavena na **true**v uvedeném pořadí.

> [!NOTE]
> Vkládání záznamů do zdroje dat není přímo podporován ovládacím prvku GridView. Nicméně je možné pro vkládání záznamů pomocí ovládacího prvku GridView ve spojení s ovládacím prvku DetailsView nebo ovládacího prvku FormView.


Místo zobrazení všech záznamů ve zdroji dat ve stejnou dobu, ovládací prvek GridView můžete automaticky rozdělte záznamy na stránkách. Chcete-li povolit stránkování, nastavte vlastnost AllowPaging na **true**.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Nastavením vlastnosti stylu pro různé části ovládacího prvku můžete přizpůsobit vzhled ovládacího prvku GridView. V následující tabulce jsou uvedeny vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| AlternatingRowStyle | Nastavení stylu pro střídavé řádky dat v ovládacím prvku GridView. Pokud je tato vlastnost nastavena, řádky dat se zobrazují střídavě RowStyle nastavení a **AlternatingRowStyle** nastavení. |
| EditRowStyle | Nastavení stylu řádku, který právě upravujete v ovládacím prvku GridView. |
| EmptyDataRowStyle | Nastavení stylu pro prázdném datovém řádku zobrazí v ovládacím prvku GridView, pokud zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí prvku GridView. |
| HeaderStyle | Nastavení stylu řádku záhlaví ovládacího prvku GridView. |
| PagerStyle | Nastavení stylu řádku stránkování prvku GridView. |
| RowStyle | Nastavení stylů pro řádky dat v ovládacím prvku GridView. Když **AlternatingRowStyle** je také nastavena, řádky dat se zobrazují střídavě **RowStyle** nastavení a **AlternatingRowStyle** nastavení. |
| SelectedRowStyle | Nastavení stylu pro vybraný řádek v ovládacím prvku GridView. |

Můžete také zobrazit nebo skrýt různé části ovládacího prvku. V následující tabulce jsou uvedeny vlastnosti, které řídí, jaké části zobrazený nebo skrytý.

| **Vlastnost** | **Popis** |
| --- | --- |
| ShowFooter | Zobrazí nebo skryje zápatí prvku GridView. |
| ShowHeader | Zobrazí nebo skryje části záhlaví ovládacího prvku GridView. |

### <a name="events"></a>Události

Ovládací prvek GridView poskytuje několik událostí, které můžete programovat proti. To umožňuje spustit vlastní rutiny pokaždé, když dojde k události. Následující tabulka uvádí události podporována ovládacím prvku GridView.

| **Události** | **Popis** |
| --- | --- |
| PageIndexChanged | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale po zpracovává operaci stránkování prvku GridView. Tato událost se běžně používá, když potřebujete provést úlohu, jakmile uživatel přejde na jinou stránku v ovládacím prvku. |
| PageIndexChanging | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale před prvku GridView. ovládací prvek zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |
| RowCancelingEdit | Vyvolá se při kliknutí na tlačítko Storno řádek, ale před ovládacím prvku GridView ukončí režim úprav. Tato událost se často používá pro zastavení zrušení operace. |
| RowCommand | Vyvolá se při kliknutí na tlačítko v ovládacím prvku GridView. Tato událost se často používá k provedení úloh po kliknutí na tlačítko v ovládacím prvku. |
| RowCreated | Nastane, když je vytvořen nový řádek v ovládacím prvku GridView. Tato událost se často používá k úpravě obsahu řádku při vytvoření řádku. |
| RowDataBound | Nastane, pokud data v řádku je vázán na data v ovládacím prvku GridView. Tato událost se často používá k úpravě obsah řádek po řádku je vázán na data. |
| RowDeleted | Vyvolá se při kliknutí na tlačítko pro odstranění řádku, ale poté, co ovládací prvek GridView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| RowDeleting | Nastane, pokud dojde ke kliknutí na tlačítko pro odstranění řádku, ale před prvku GridView prvek ze zdroje dat odstraní záznam. Tato událost se často používá pro zrušení operace odstranění. |
| RowEditing | Nastane, pokud dojde ke kliknutí na tlačítko Upravit řádku, ale před prvku GridView řízení přejde do režimu úprav. Tato událost se často používá ke zrušení operace úprav. |
| RowUpdated metody | Vyvolá se při kliknutí na tlačítko Aktualizovat řádek, ale po dokončení aktualizace řádku prvku GridView. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| RowUpdating | Vyvolá se při kliknutí na tlačítko aktualizace řádku, ale před prvku GridView. ovládací prvek aktualizuje řádek. Tato událost se často používá pro zrušení operace aktualizace. |
| SelectedIndexChanged. | Vyvolá se při kliknutí na tlačítko pro výběr řádek, ale po ovládacím prvku GridView zpracovává operace select. Tato událost se často používá k provedení úloh po výběru řádku v ovládacím prvku. |
| SelectedIndexChanging | Nastane, pokud dojde ke kliknutí na tlačítko pro výběr řádek, ale před prvku GridView. ovládací prvek zpracovává operace select. Tato událost se často používá ke zrušení operace výběru. |
| Řazení | Vyvolá se při kliknutí na hypertextový odkaz na řazení sloupce, ale po ovládacím prvku GridView zpracovává operace řazení. Tato událost se běžně používá k provedení úloh, jakmile uživatel klikne na hypertextový odkaz na řazení sloupce. |
| Řazení | Nastane, pokud dojde ke kliknutí na hypertextový odkaz na řazení sloupce, ale před prvku GridView. ovládací prvek zpracovává operace řazení. Tato událost se často používá pro zrušení operace řazení nebo provádět vlastní rutiny řazení. |

## <a name="formview"></a>FormView

Chcete-li zobrazit jeden záznam ze zdroje dat se používá ovládacího prvku FormView. Je podobný ovládací prvek DetailsView s výjimkou zobrazí uživatelem definovaných šablon místo pole řádku. Vytvořením vlastní šablony získáte větší flexibilitu při řízení, jak se data zobrazí. Ovládacího prvku FormView podporuje následující funkce:

- Vytvoření vazby na data ovládací prvky zdroje, jako je například SqlDataSource a ObjectDataSource.
- Integrované funkce pro vkládání.
- Integrované aktualizace nebo odstranění funkce.
- Integrované funkce stránkování.
- Programový přístup k objektovému modelu FormView dynamicky nastavit vlastnosti, zpracování událostí a tak dále.
- Vzhled lze přizpůsobit pomocí uživatelem definovaných šablon, motivů a stylů.

## <a name="templates"></a>Šablony

Chcete-li zobrazit obsah ovládacího prvku FormView je potřeba vytvořit šablony pro různé části ovládacího prvku. Většina šablon jsou volitelné. Musíte však vytvořit šablonu pro režim, ve kterém ovládací prvek nakonfigurovat. Například FormView ovládací prvek, který podporuje vkládání záznamů musí mít definované šablony vložit položky. V následující tabulce jsou uvedeny různé šablony, které můžete vytvořit.

| **Typ šablony** | **Popis** |
| --- | --- |
| EditItemTemplate | Definuje obsah pro řádek dat, když ovládacího prvku FormView je v režimu úprav. Tato šablona obvykle obsahuje vstupní ovládací prvky a příkazového tlačítka, pomocí kterého uživatel může upravovat existující záznam. |
| Šablonu EmptyDataTemplate | Definuje obsah prázdném datovém řádku zobrazí, když ovládacího prvku FormView je vázán na zdroj dat, který neobsahuje žádné záznamy. Tato šablona obvykle obsahuje obsah tak, aby upozornila uživatele, že zdroj dat neobsahuje žádné záznamy. |
| Šablona FooterTemplate | Definuje obsah pro řádek zápatí. Tato šablona obvykle obsahuje jakékoli další obsah, který se má zobrazit v řádku zápatí. Jako alternativu můžete jednoduše zadat text zobrazený v zápatí řádek tak, že nastavíte vlastnost FooterText. |
| Parametr HeaderTemplate | Definuje obsah pro řádek záhlaví. Tato šablona obvykle obsahuje jakékoli další obsah, který se má zobrazit v řádku záhlaví. Jako alternativu můžete jednoduše zadat text, který se zobrazí v záhlaví tak, že nastavíte vlastnost HeaderText. |
| Vlastnosti ItemTemplate | Definuje obsah pro řádek dat, když ovládacího prvku FormView je v režimu jen pro čtení. Tato šablona obvykle obsahuje obsah k zobrazení hodnot z existujícího záznamu. |
| Šablona InsertItemTemplate | Definuje obsah pro řádek dat, když ovládacího prvku FormView je v režimu vkládání. Tato šablona obvykle obsahuje vstupní ovládací prvky a příkazového tlačítka, pomocí kterého může uživatel přidat nový záznam. |
| PagerTemplate | Definuje obsah pro stránkování řádek zobrazí, když je povolena funkce stránkování (když je vlastnost AllowPaging nastavena na **true**). Tato šablona obvykle obsahuje ovládací prvky, se kterými uživatel přejít na jiný záznam. |

Vstupní ovládací prvky v šabloně položky upravit a Vložit šablonu položky lze vázán na pole zdroje dat s použitím výrazu obousměrné vazby. To umožňuje ovládacímu prvku FormView automaticky extrahovat hodnoty vstupní ovládací prvek pro aktualizaci nebo vložení operace. Výrazy používají obousměrné vazby umožňují vstupní ovládací prvky v šabloně položky Úpravy automaticky zobrazí původní hodnoty pole.

### <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládacího prvku FormView mohou být vázány na ovládací prvek zdroje dat (například **SqlDataSource**, prvku AccessDataSource, **ObjectDataSource** a tak dále), nebo k libovolnému zdroji dat, která implementuje System.Collections.IEnumerable rozhraní (například System.Data.DataView System.Collections.ArrayList a System.Collections.Hashtable). Použijte jednu z následujících metod k vytvoření vazby ovládacího prvku FormView odpovídající datový typ zdroje:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte na hodnotu ID ovládacího prvku zdroje dat vlastnost DataSourceID ovládacího prvku FormView. Ovládacího prvku FormView automaticky váže do správy zdrojového kódu určená data a můžou využít výhod zdroje dat možnosti ovládacího prvku na vkládání, aktualizaci, odstranění a funkce stránkování. Toto je upřednostňovanou metodou pro připojení k datům.
- K vytvoření vazby ke zdroji dat, která implementuje **System.Collections.IEnumerable** rozhraní, prostřednictvím kódu programu nastavit vlastnosti DataSource ovládacího prvku FormView ke zdroji dat a potom voláním metody DataBind. Při použití této metody, ovládacího prvku FormView neposkytuje integrované vkládání, aktualizaci, odstranění a funkce stránkování. Budete muset zadat tuto funkci pomocí příslušné události.

## <a name="data-operations"></a>Operace s daty

Ovládacího prvku FormView poskytuje mnoho předdefinovaných možností, které umožní uživateli aktualizovat, odstranit, Vložit a stránkovat položek v ovládacím prvku. Když ovládacího prvku FormView je vytvořena vazba na ovládací prvek zdroje dat, ovládacího prvku FormView můžete využít zdroje dat možnosti ovládacího prvku a poskytují automatickou aktualizaci, odstranění, vkládání a stránkování funkce. Ovládacího prvku FormView může poskytovat podporu pro update, delete, insert a operace stránkování u jiných typů zdrojů dat; Musíte však poskytnout implementaci pro tyto operace odpovídající obslužnou rutinu.

Vzhledem k tomu ovládacího prvku FormView používá šablony, neposkytuje způsob, jak automaticky vygenerovat příkazová tlačítka provést aktualizaci, odstranění nebo vložení operace. Musíte ručně zahrnout do těchto příkazů v příslušnou šablonu. Ovládacího prvku FormView rozpozná některá tlačítka, které mají jejich **CommandName** vlastnosti nastavené na konkrétní hodnoty. V následující tabulce jsou uvedeny příkazová tlačítka, který rozpoznává ovládacího prvku FormView.

| **Tlačítko** | **Hodnota CommandName** | **Popis** |
| --- | --- | --- |
| Zrušit | Tlačítko Storno. | Při aktualizaci nebo vložení operace chcete operaci zrušit a zahodit hodnoty zadané uživatelem. Ovládacího prvku FormView vrátí do režimu určeném vlastnost DefaultMode. |
| Odstranit | "Odstranit" | Použít při odstraňování operace zobrazené záznam ze zdroje dat odstranit. Vyvolává události ItemDeleting a ItemDeleted. |
| Upravit | "Edit" | Použít při aktualizaci operace uvést do režimu úprav ovládacího prvku FormView. Zadanému v obsahu **EditItemTemplate** pro řádek dat se zobrazí vlastnosti. |
| Insert | "Vložit" | Použít v vkládání operace pokus o vložení nového záznamu ve zdroji dat pomocí hodnot poskytnutých uživatelem. Vyvolává události ItemInserting a ItemInserted. |
| Nový | "Nové" | Použít v operace vložení do ovládacího prvku FormView v režimu vkládání. Zadanému v obsahu **InsertItemTemplate** pro řádek dat se zobrazí vlastnosti. |
| Stránka | "Page" | Používá operace stránkování představuje tlačítko v řádku stránkování, který provádí stránkování. Chcete-li zadat operaci stránkování, nastavte **CommandArgument** vlastnosti tlačítka "Next", "Předchozí", "First", "Last" nebo index stránky, na který chcete přejít. Vyvolává události PageIndexChanging a PageIndexChanged. |
| Aktualizace | "Úpravy" | Umožňuje při aktualizaci operace pokus o aktualizaci zobrazený záznam ve zdroji dat hodnotami, které zadal uživatel. Vyvolává události ItemUpdating a ItemUpdated. |

Na rozdíl od odstranění tlačítka (která odstraňuje zobrazený záznam okamžitě), při kliknutí na tlačítko Upravit nebo nový FormView řízení přejde do úprav nebo režimu vkládání v uvedeném pořadí. V režimu úprav, součástí obsahu **EditItemTemplate** pro aktuální datová položka je zobrazena vlastnost. Obvykle je definována šablona položky upravit tak, aby se tlačítko Upravit nahradí aktualizace a tlačítko Storno. Vstupní ovládací prvky, které jsou vhodné pro datový typ pole (například TextBox nebo ovládacím prvku zaškrtávací políčko) se obvykle zobrazí hodnotu pole pro uživatele, který chcete upravit. Kliknutím na tlačítko Aktualizovat aktualizuje záznam ve zdroji dat, při kliknutí na tlačítko Storno opustí všechny změny.

Obdobně obsah součástí **InsertItemTemplate** vlastnost se zobrazí položky dat, když je ovládací prvek v režimu vkládání. Vložit šablonu položky je obvykle definováno, takže tlačítko je nahrazen Insert a tlačítko Storno a pro uživatele k zadání hodnoty pro nový záznam se zobrazí prázdný vstupní ovládací prvky. Kliknutím na tlačítko pro vložení vloží záznam ve zdroji dat, při kliknutí na tlačítko Storno opustí žádné změny.

Ovládacího prvku FormView poskytuje funkci stránkování, což mu umožní přejít na další záznamy ve zdroji dat. Pokud je povoleno, je v ovládacím prvku FormView, který obsahuje ovládací prvky navigace stránky zobrazují řádek stránkování. Chcete-li povolit stránkování, nastavte **vlastnost AllowPaging** vlastnost **true**. Nastavením vlastnosti objektů obsažených v PagerStyle a vlastnost PagerSettings můžete přizpůsobit řádek stránkování. Místo použití předdefinovaných stránkování řádku uživatelského rozhraní, můžete vytvořit vlastní uživatelské rozhraní pomocí **PagerTemplate** vlastnost.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Přizpůsobení vzhledu ovládacího prvku FormView nastavením vlastnosti stylu pro různé části ovládacího prvku. V následující tabulce jsou uvedeny vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| EditRowStyle | Nastavení stylu pro řádek dat, když je ovládací prvek FormView v režimu úprav. |
| EmptyDataRowStyle | Nastavení stylu pro prázdném datovém řádku zobrazí v ovládacím prvku FormView, pokud zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí ovládacího prvku FormView. |
| HeaderStyle | Nastavení stylu řádku záhlaví ovládacího prvku FormView. |
| InsertRowStyle | Nastavení stylu pro řádek dat, když je ovládací prvek FormView v režimu vkládání. |
| PagerStyle | Nastavení stylu řádku stránkování zobrazí v ovládacím prvku FormView, když je povolena funkce stránkování. |
| RowStyle | Nastavení stylu pro řádek dat, když je ovládací prvek FormView v režimu jen pro čtení. |

## <a name="events"></a>Události

Ovládacího prvku FormView poskytuje několik událostí, které můžete programovat proti. To umožňuje spustit vlastní rutiny pokaždé, když dojde k události. Následující tabulka uvádí události podporované v ovládacím prvku FormView.

| **Události** | **Popis** |
| --- | --- |
| ItemCommand | Vyvolá se při kliknutí na tlačítko v ovládacím prvku FormView. Tato událost se často používá k provedení úloh po kliknutí na tlačítko v ovládacím prvku. |
| ItemCreated | Vyvolá se po vytvoření všech objektů FormViewRow v ovládacím prvku FormView. Tato událost se často používá k úpravě hodnoty záznamu, než se zobrazí. |
| ItemDeleted | Nastane, pokud tlačítko pro odstranění (tlačítko s jeho **CommandName** vlastnost nastavena na "Odstranit") dojde ke kliknutí na, ale po ovládacího prvku FormView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| ItemDeleting | Nastane, pokud dojde ke kliknutí na tlačítko pro odstranění, ale před FormView prvek ze zdroje dat odstraní záznam. Tato událost se často používá pro operaci odstraňování zrušíte. |
| ItemInserted | Nastane, pokud tlačítko pro vložení (tlačítko s jeho **CommandName** vlastnost nastavena na "Vložit") dojde ke kliknutí na, ale po ovládacího prvku FormView vloží záznam do databáze. Tato událost se často používá ke kontrole výsledky operace insert. |
| ItemInserting | Nastane, pokud dojde ke kliknutí na tlačítko pro vložení, ale před FormView ovládací prvek, vloží záznam do databáze. Tato událost se často používá ke zrušení operace insert. |
| ItemUpdated | Vyvolá se v případě na tlačítko Aktualizovat (tlačítko s jeho **CommandName** vlastnost nastavena na "Úpravy") dojde ke kliknutí na, ale po dokončení aktualizace na řádku ovládacím prvku FormView. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| ItemUpdating | Vyvolá se při kliknutí na tlačítko Aktualizovat, ale před FormView ovládací prvek aktualizuje záznam. Tato událost se často používá ke zrušení operace aktualizace. |
| ModeChanged | Vyvolá se po změně režimu ovládacího prvku FormView (pro úpravy, vložení nebo režimu jen pro čtení). Tato událost se často používá k provedení úloh po změně režimu třídy FormView ovládacího prvku. |
| ModeChanging | Vyvolá se před změní režim ovládacího prvku FormView (pro úpravy, vložení nebo režimu jen pro čtení). Tato událost se často používá pro zrušení změn režimu. |
| PageIndexChanged | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale po ovládacího prvku FormView zpracovává operaci stránkování. Tato událost se běžně používá, když potřebujete provést úlohu, jakmile uživatel přejde na jiný záznam v ovládacím prvku. |
| PageIndexChanging | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale před FormView ovládací prvek zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |

## <a name="detailsview"></a>Prvku DetailsView.

Ovládací prvek DetailsView slouží k zobrazení jeden záznam ze zdroje dat v tabulce, kde každé pole záznamu se zobrazí v řádku v tabulce. Můžete použít v kombinaci s ovládacím prvkem GridView pro scénáře hlavní podrobnosti. Ovládací prvek DetailsView podporuje následující funkce:

- Vytvoření vazby na data ovládací prvky zdroje, jako je například SqlDataSource.
- Integrované funkce pro vkládání.
- Integrované aktualizace nebo odstranění funkce.
- Integrované funkce stránkování.
- Programový přístup k prvku DetailsView objektový model pro dynamicky nastavovat vlastnosti, zpracování událostí a tak dále.
- Vzhled lze přizpůsobit pomocí motivů a stylů.

## <a name="row-fields"></a>Řádková pole

Deklarováním ovládací prvek pole se vytvoří každý řádek dat v ovládacím prvku DetailsView. Typy polí odlišný řádek určení chování řádků v ovládacím prvku. Ovládací prvky pole jsou odvozeny od typu DataControlField. Následující tabulka uvádí typy odlišný řádek pole, které lze použít.

| **Typ pole sloupce** | **Popis** |
| --- | --- |
| Vlastnost BoundField | Zobrazí hodnoty pole ve zdroji dat jako text. |
| ButtonField | Příkazové tlačítko se zobrazí v ovládacím prvku DetailsView. To umožňuje zobrazit v řádku vlastního ovládacího prvku, jako je například přidat nebo odebrat tlačítka. |
| Třída CheckBoxField | Zaškrtávací políčko se zobrazí v ovládacím prvku DetailsView. Tento typ pole řádku běžně slouží k zobrazení polí s logickou hodnotou. |
| CommandField | Zobrazí vestavěné příkazového tlačítka provádět úpravy, vložení nebo odstranění operace v ovládacím prvku DetailsView. |
| HyperLinkField | Zobrazí hodnoty pole ve zdroji dat jako hypertextový odkaz. Tento typ pole řádku umožňuje vytvořit vazbu druhé pole k adrese URL hypertextového odkazu. |
| Třídy ImageField | Zobrazí obrázek v ovládacím prvku DetailsView. |
| TemplateField | Zobrazí obsah uživatelem definované pro řádek v ovládacím prvku DetailsView podle určené šablony. Tento typ pole řádku umožňuje vytvořit vlastní řádek pole. |

Ve výchozím nastavení je vlastnost AutoGenerateRows nastavena na **true**, které automaticky generuje objekt vázaný řádek pole pro každé pole s možností vazby typu ve zdroji dat. Platné typy s možností vazby jsou řetězce, data a času, Decimal, Guid a sad primitivních typů. Každé pole se následně zobrazí po sobě jako text v pořadí, ve kterém každé pole se objeví ve zdroji dat.

Automatické generování řádky poskytuje rychlý a snadný způsob, jak zobrazit všechna pole v záznamu. Však nutné používat prvku DetailsView ovládacího prvku rozšířené možnosti, které jsou musíte explicitně deklarovat řádek polí pro zahrnutí v ovládacím prvku DetailsView. Chcete-li deklarovat pole řádků, nejprve nastavte **AutoGenerateRows** vlastnost **false**. V dalším kroku přidejte otevírání a zavírání **&lt;pole&gt;** značky mezi počátečními a ukončovacími značkami ovládacího prvku DetailsView. Nakonec zobrazte seznam pole řádků, které chcete zahrnout mezi otevírací a zavírací **&lt;pole&gt;** značky. Pole řádku zadaný jsou přidána do kolekce polí v uvedeném pořadí. **Pole** kolekce umožňuje programově spravovat pole řádků v ovládacím prvku DetailsView.

> [!NOTE]
> Automaticky generované pole nejsou přidány do kolekce polí po sobě.


## <a name="binding-to-data"></a>Vytvoření vazby na data

Ovládací prvek DetailsView mohou být vázány na ovládací prvek zdroje dat, například **SqlDataSource** nebo prvku AccessDataSource, nebo k libovolnému zdroji dat, která implementuje rozhraní System.Collections.IEnumerable, jako je například System.Data.DataView, System.Collections.ArrayList a System.Collections.Hashtable.

Použijte jednu z následujících metod k vytvoření vazby ovládacího prvku DetailsView. pro typ zdroje dat odpovídající:

- K vytvoření vazby na ovládací prvek zdroje dat, nastavte vlastnost DataSourceID ovládacím prvku DetailsView na hodnotu ID ovládacího prvku zdroje dat. Do správy zdrojového kódu určená data automaticky sváže ovládacím prvku DetailsView. Toto je upřednostňovanou metodou pro připojení k datům.
- K vytvoření vazby ke zdroji dat, která implementuje **System.Collections.IEnumerable** rozhraní, prostřednictvím kódu programu nastavit vlastnosti DataSource ovládacího prvku DetailsView ke zdroji dat a potom voláním metody DataBind.

## <a name="security"></a>Zabezpečení

Tento ovládací prvek slouží k zobrazení vstupu uživatele, která by mohla obsahovat škodlivé klientského skriptu. Zkontrolujte všechny informace odesílané z klienta pro spustitelný soubor skriptu, příkazy SQL nebo jiný kód před zobrazením ve vaší aplikaci. Technologie ASP.NET poskytuje funkci vstupní požadavek ověřování blokovací skript a HTML v uživatelského vstupu.

## <a name="data-operations"></a>Operace s daty

Ovládací prvek DetailsView poskytuje integrované funkce, které umožní uživateli aktualizovat, odstranit, Vložit a stránkovat položek v ovládacím prvku. Když je vytvořena vazba ovládacího prvku DetailsView na ovládací prvek zdroje dat, můžete ovládací prvek DetailsView využít zdroje dat možnosti ovládacího prvku a poskytují automatickou aktualizaci, odstranění, vkládání a stránkování funkce.

Ovládací prvek DetailsView může poskytovat podporu pro update, delete, insert a operace stránkování u jiných typů zdrojů dat; však musí poskytnout implementaci pro tyto operace v obslužné rutině události.

Automaticky přidat ovládací prvek DetailsView **CommandField** řádek pole pomocí tlačítka Upravit, odstranit nebo nový tak, že nastavíte vlastnost AutoGenerateEditButton, AutoGenerateDeleteButton nebo vlastnost AutoGenerateInsertButton vlastnosti, které mají **true**v uvedeném pořadí. Na rozdíl od odstranění tlačítka (která odstraní vybraný záznam okamžitě), při kliknutí na tlačítko Upravit nebo nový ovládacím prvku DetailsView. ovládací prvek obsahuje úpravy nebo vložit režim, v uvedeném pořadí. V režimu úprav na tlačítko Upravit nahradí se aktualizace a tlačítko Storno. Hodnotu pole pro uživatele k úpravě se zobrazí vstupní ovládací prvky, které jsou vhodné pro datový typ pole (například TextBox nebo ovládacím prvku zaškrtávací políčko). Kliknutím na tlačítko Aktualizovat aktualizuje záznam ve zdroji dat, při kliknutí na tlačítko Storno opustí všechny změny. Podobně v režimu vkládání nové tlačítko je nahrazen Insert a tlačítko Storno a pro uživatele k zadání hodnoty pro nový záznam se zobrazí prázdný vstupní ovládací prvky.

Ovládací prvek DetailsView poskytuje stránkování funkci, která umožňuje uživateli přejít na další záznamy ve zdroji dat. Pokud je povoleno, jsou ovládací prvky navigace stránky zobrazují za sebou stránkování. Chcete-li povolit stránkování, nastavte vlastnost AllowPaging na **true**. Řádek stránkování je možné přizpůsobit pomocí vlastnosti PagerStyle a PagerSettings.

## <a name="customizing-the-user-interface"></a>Přizpůsobení uživatelského rozhraní

Nastavením vlastnosti stylu pro různé části ovládacího prvku můžete přizpůsobit vzhled ovládacího prvku DetailsView. V následující tabulce jsou uvedeny vlastnosti jiný styl.

| **Vlastnost stylu** | **Popis** |
| --- | --- |
| AlternatingRowStyle | Nastavení stylu pro střídavé řádky dat v ovládacím prvku DetailsView. Pokud je tato vlastnost nastavena, řádky dat se zobrazují střídavě RowStyle nastavení a **AlternatingRowStyle** nastavení. |
| CommandRowStyle | Nastavení stylu pro řádek, který obsahuje vestavěné příkazového tlačítka v ovládacím prvku DetailsView. |
| EditRowStyle | Nastavení stylů pro řádky dat, když je ovládací prvek DetailsView v režimu úprav. |
| EmptyDataRowStyle | Nastavení stylu pro prázdném datovém řádku zobrazí v ovládacím prvku DetailsView. Pokud zdroj dat neobsahuje žádné záznamy. |
| FooterStyle | Nastavení stylu řádku zápatí ovládacího prvku DetailsView. |
| HeaderStyle | Nastavení stylu řádku záhlaví v ovládacím prvku DetailsView. |
| InsertRowStyle | Nastavení stylů pro řádky dat, když je ovládací prvek DetailsView v režimu vkládání. |
| PagerStyle | Nastavení stylu řádku stránkování prvku DetailsView. |
| RowStyle | Nastavení stylů pro řádky dat v ovládacím prvku DetailsView. Když **AlternatingRowStyle** je také nastavena, řádky dat se zobrazují střídavě **RowStyle** nastavení a **AlternatingRowStyle** nastavení. |
| FieldHeaderStyle | Nastavení stylu pro sloupec záhlaví ovládacího prvku DetailsView. |

## <a name="events"></a>Události

Ovládací prvek DetailsView poskytuje několik událostí, které můžete programovat proti. To umožňuje spustit vlastní rutiny pokaždé, když dojde k události. Následující tabulka uvádí události podporována ovládacím prvku DetailsView. Tyto události ovládacího prvku DetailsView také dědí z jejích základních tříd: vázání dat, datové vazby, odstranit, Init, Load, PreRender a vykreslení.

| **Události** | **Popis** |
| --- | --- |
| ItemCommand | Vyvolá se při kliknutí na tlačítko v ovládacím prvku DetailsView. |
| ItemCreated | Vyvolá se po vytvoření všech objektů DetailsViewRow v ovládacím prvku DetailsView. Tato událost se často používá k úpravě hodnoty záznamu, než se zobrazí. |
| ItemDeleted | Vyvolá se při kliknutí na tlačítko pro odstranění, ale poté, co ovládací prvek DetailsView odstraní záznam ze zdroje dat. Tato událost se často používá ke kontrole výsledky operace odstranění. |
| ItemDeleting | Nastane, pokud dojde ke kliknutí na tlačítko pro odstranění, ale ovládací prvek před ovládacím prvku DetailsView odstraní záznam ze zdroje dat. Tato událost se často používá pro operaci odstraňování zrušíte. |
| ItemInserted | Vyvolá se při kliknutí na tlačítko pro vložení, ale po ovládacím prvku DetailsView vloží záznam do databáze. Tato událost se často používá ke kontrole výsledky operace insert. |
| ItemInserting | Nastane, pokud dojde ke kliknutí na tlačítko pro vložení, ale ovládací prvek před ovládacím prvku DetailsView vloží záznam do databáze. Tato událost se často používá ke zrušení operace insert. |
| ItemUpdated | Vyvolá se při kliknutí na tlačítko Aktualizovat, ale po dokončení aktualizace na řádku ovládacím prvku DetailsView. Tato událost se často používá ke kontrole výsledky operace aktualizace. |
| ItemUpdating | Vyvolá se při kliknutí na tlačítko Aktualizovat, ale ovládací prvek před ovládacím prvku DetailsView aktualizuje záznam. Tato událost se často používá ke zrušení operace aktualizace. |
| ModeChanged | Vyvolá se po změně režimu (úpravy, vložení nebo režimu jen pro čtení) ovládacího prvku DetailsView. Tato událost se často používá k provedení úloh po změně režimu prvku DetailsView. |
| ModeChanging | Vyvolá se před ovládacím prvku DetailsView změní režim (úpravy, vložení nebo režimu jen pro čtení). Tato událost se často používá pro zrušení změn režimu. |
| PageIndexChanged | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale po zpracovává operaci stránkování prvku DetailsView. Tato událost se běžně používá, když potřebujete provést úlohu, jakmile uživatel přejde na jiný záznam v ovládacím prvku. |
| PageIndexChanging | Vyvolá se při kliknutí na jednu z tlačítka stránkování, ale ovládací prvek před ovládacím prvku DetailsView zpracovává operaci stránkování. Tato událost se často používá ke zrušení operace stránkování. |

## <a name="the-menu-control"></a>Ovládací prvek nabídky

Ovládací prvek nabídky v technologii ASP.NET 2.0 je navržena jako plně funkční navigace systému. Může být datové vazby snadno ke zdrojům hierarchických dat jako SiteMapDataSource.

Struktura ovládací prvky nabídky je možné definovat deklarativně nebo dynamicky a obsahuje jeden kořenový uzel a libovolný počet dílčích uzly. Následující kód definuje deklarativně nabídku pro ovládací prvek nabídky.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

Ve výše uvedeném příkladu Home.aspx uzel je kořenový uzel. Všechny ostatní uzly jsou vnořeny do kořenového uzlu na různých úrovních.

Existují dva typy nabídek, které může mít za následek ovládací prvek nabídky; nabídky, statické a dynamické nabídky. Statické nabídky se skládají z položky nabídky, které jsou vždycky viditelná. Dynamické nabídky se skládají z položek nabídky, které jsou viditelné pouze když uživatel najede myší je pomocí myši. Zákazníci mohou často zmást statické nabídek s nabídkami definované pomocí deklarace a dynamické nabídky s nabídkami, které jsou datové vazby v době běhu. Statické a dynamické nabídky ve skutečnosti nesouvisí s metodu základního souboru. Podmínky *statické* a *dynamické* odkazovat pouze na to, jestli v nabídce je staticky zobrazené ve výchozím nastavení, může obsahovat jenom zobrazí, když uživatel provede určitou akci.

**Hodnota StaticDisplayLevels** vlastnost se používá ke konfiguraci, kolik úrovní nabídky jsou statické a proto zobrazené ve výchozím nastavení. V příkladu výše, nastavení **hodnota StaticDisplayLevels** vlastnost na hodnotu 2 by způsobilo nabídku staticky zobrazíte uzel Domovská stránka, Hudba uzlu a uzel videa. Všechny ostatní uzly by dynamicky zobrazí, když uživatel najede myší na nadřazený uzel.

**MaximumDynamicDisplayLevels** vlastnost nakonfiguruje maximální počet úrovní dynamické je schopná zobrazit v nabídce. Všechny dynamické nabídky na vyšší úrovni než hodnoty určené **MaximumDynamicDisplayLevels** vlastnost se zahodí.

> [!NOTE]
> Je téměř jistý, že může dojít situacích, kdy nejsou zobrazeny nabídek k vykreslení z důvodu vlastností MaximumDynamicDisplayLevels. V těchto případech se ujistěte, že vlastnost je nastavena dostatečně umožňující zobrazení nabídky zákazníkům.


## <a name="data-binding-the-menu-control"></a>Datové vazby ovládacího prvku nabídka

Ovládací prvek nabídky může být vázán k libovolnému zdroji hierarchická data jako SiteMapDataSource nebo prvku XMLDataSource. SiteMapDataSource je mohou nejčastěji používané metody pro vytvoření vazby dat k ovládacímu prvku nabídky, protože informační kanály z Web.sitemap souboru a jeho schématu poskytuje známých rozhraní API pro ovládací prvek nabídky. Výpis níže ukazuje jednoduchý soubor Web.sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Všimněte si, že je pouze jeden kořenový siteMapNode prvek, v tomto případě elementu Domovská stránka. Několik atributů lze nakonfigurovat pro každou siteMapNode. Nejčastěji používané atributy jsou:

- **Adresa URL** Určuje adresu URL má zobrazit, když uživatel klikne na položku nabídky. Pokud tento atribut neexistuje, bude uzel účtovat jednoduše zpět při kliknutí na.
- **název** Určuje text, který je zobrazen na položce nabídky.
- **Popis** použitá jako dokumentace pro uzel. Také zobrazí jako popisku tlačítka, když ukazatel myši je myš uzlu.
- **siteMapFile** umožňuje vnořené mapy webu. Tento atribut musí odkazovat na soubor mapy webu ASP.NET ve správném formátu.
- **role** umožňuje vzhled uzel kontrolován oříznutí zabezpečení technologie ASP.NET.

Mějte na paměti, že tyto atributy jsou nepovinné, chování v nabídce nemusí být co se očekává, pokud nejsou zadané. Například pokud *url* je zadán atribut ale *popis* atribut není, uzel nebude viditelná a bude existovat žádná možnost jak přejít na zadanou adresu URL.

## <a name="controlling-a-menus-operation"></a>Řízení operaci nabídky

Existuje několik vlastností, které ovlivňují provoz ovládací prvek ASP.NET nabídky; **orientace** vlastnost, **DisappearAfter** vlastnost, **StaticItemFormatString** vlastnost a **StaticPopoutImageUrl**vlastnosti jsou jen některé z nich.

- **Orientace** může být nastaven na hodnotu *vodorovné* nebo *svislé* a určuje, zda jsou rozloženy vodorovně za sebou nebo svisle a skládaný na statické položky nabídky mezi sebou. Tato vlastnost nemá vliv na dynamické nabídky.
- **DisappearAfter** vlastnost nakonfiguruje, jak dlouho dynamickou nabídku by měla zůstat viditelné po myši přesunut mimo ho. Hodnota je určena v milisekundách a výchozí hodnota je 500. Nastavení této vlastnosti na hodnotu-1 způsobí, že v nabídce nikdy zmizení automaticky. V takovém případě v nabídce pouze zmizí, jakmile uživatel klikne na tlačítko mimo nabídky.
- **StaticItemFormatString** vlastnost umožňuje snadno udržovat konzistentní tento problém v nabídce systému. Při zadávání tuto vlastnost *{0}* místo popis, který se zobrazí ve zdroji dat by měly být zadány. Například pokud chcete mít položka nabídky z Řekněme cvičení 1 navštivte naše stránky produktů, atd., zadali byste navštivte náš {0} stránka StaticItemFormatString. V době běhu nahradí ASP.NET jakýmkoli výskytem {0} s popisem správné pro položku nabídky.
- **StaticPopoutImageUrl** vlastnost určuje obrázek, který se používá k označení, že konkrétní nabídce uzel obsahuje podřízené uzly, které mohou být přístupné podržením ukazatele nad ním. Dynamické nabídky bude nadále používat výchozí image.

## <a name="templated-menu-controls"></a>Ovládací prvky bez vizuálního vzhledu nabídky

Ovládací prvek nabídky je ovládací prvek bez vizuálního vzhledu a umožňuje dvou různých šablon položek; třídu StaticItemTemplate a třídu DynamicItemTemplate Pomocí těchto šablon můžete snadno přidat serverové ovládací prvky nebo uživatelské ovládací prvky do vaší nabídky.

Chcete-li upravit šablony v sadě Visual Studio .NET, klikněte na tlačítko inteligentních značek v nabídce a vybrat upravit šablony. Zvolte mezi úpravy třídu StaticItemTemplate nebo třídu DynamicItemTemplate.

Všechny ovládací prvky přidané do třídu StaticItemTemplate se zobrazí v nabídce statické při načtení stránky. Všechny ovládací prvky přidané do třídu DynamicItemTemplate se zobrazí na všechny místní nabídky.

## <a name="menu-events"></a>Události nabídky

Ovládací prvek nabídky má dvě události, které jsou jedinečné. **MenuItemClicked** a **MenuItemDatabound** událostí.

MenuItemClicked událost je aktivována, když dojde ke kliknutí na položku nabídky. MenuItemDatabound událost se vyvolá, když je položka nabídky datové vazby. **MenuEventArgs** , která je předána obslužné rutiny události poskytuje přístup k položce nabídky prostřednictvím vlastnosti položky.

## <a name="controlling-a-menus-appearance"></a>Řízení vzhledu nabídky

Můžete také vliv na vzhled ovládacího prvku nabídka použití jednoho nebo více dostupných nabídek formátu mnoho styly. Mezi ně patří **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**a **DynamicHoverStyle**. Tyto vlastnosti jsou konfigurovány pomocí standardní řetězec stylu HTML. Například následující bude mít vliv na styl pro dynamické nabídky.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Pokud používáte některou styly při najetí myší, budete muset přidat &lt;head&gt; prvek na stránce s *runat* element nastaven na hodnotu *server*.


Ovládací prvky nabídky také podporují používání motivů ASP.NET 2.0.

## <a name="the-treeview-control"></a>TreeView – ovládací prvek

TreeView – ovládací prvek zobrazuje data ve struktuře stromu. Stejně jako u ovládacího prvku nabídky, může být snadno data vázaná k libovolnému zdroji hierarchických dat, jako je například SiteMapDataSource.

Na první otázku, kterou zákazníci můžou požádat o ovládacím prvku TreeView technologie ASP.NET 2.0 je určuje, jestli je související s WebControl IE stromovém zobrazení, která byla dostupná pro technologii ASP.NET 1.x. Není. Ovládací prvek TreeView technologie ASP.NET 2.0 byla napsaná od základu a nabízí výrazné zlepšení TreeView WebControl aplikace Internet Explorer, který byl dříve k dispozici.

Přejdu nebude podrobnosti o tom, jak navázat TreeView – ovládací prvek do mapy webu, protože se provádí v přesně stejným způsobem jako ovládací prvek nabídky. TreeView – ovládací prvek má však několik různých rozdílů ve způsobu, jakým funguje.

Ve výchozím nastavení zobrazí se plně rozšířené ovládací prvek TreeView. Chcete-li změnit úroveň rozšíření po počátečním načtení, upravte **ExpandDepth** vlastnost ovládacího prvku. To je zvlášť důležité v případech, kdy je datové vazby při rozbalení konkrétní uzlů prvku TreeView.

## <a name="databinding-the-treeview-control"></a>Datová vazba ovládacího prvku TreeView

Na rozdíl od ovládacího prvku nabídky stromovém zobrazení slouží také k zpracování velkého objemu dat. Kromě datová vazba ovládacího prvku SiteMapDataSource nebo prvek XMLDataSource, prvek TreeView je proto často data vázaná na datovou sadu nebo jiná relační data. V případech, kde je TreeView – ovládací prvek vázán na velké objemy dat je nejlepší vytvořit vazbu jenom na data, která jsou skutečně viditelné v ovládacím prvku. Můžete datové připojení k dalším datům, jako jsou rozbaleny uzlů prvku TreeView.

V těchto případech **Vlastnost PopulateOnDemand** vlastnost ovládacího prvku TreeView musí být nastavena na *true*. Pak musíte poskytnout implementaci pro **TreeNodePopulate** metody.

## <a name="data-binding-without-postback"></a>Datové vazby bez zpětného odeslání

Všimněte si, že po rozbalení uzlu v předchozím příkladu první stránka odešle zpět a aktualizuje. Thats není problém v tomto příkladu, ale dokážete představit, že může být v produkčním prostředí s velké množství dat. Lepší scénář bude jeden kdy stromovém zobrazení by stále dynamicky naplnění jeho uzly, ale bez příspěvek zpět na server.

Tím, že nastavíte **PopulateNodesFromClient** a **Vlastnost PopulateOnDemand** vlastnosti na hodnotu true, prvek TreeView technologie ASP.NET se dynamicky naplnění uzlů bez příspěvek zpět. Po rozbalení nadřazený uzel XMLHttp žádosti z klienta a OnTreeNodePopulate událost se aktivuje. Server odpoví datový ostrůvek XML, který potom slouží k datům vazby podřízených uzlů.

ASP.NET vytvoří dynamicky klientský kód, který implementuje tuto funkci. &lt;Skript&gt; značky, které obsahují skriptu jsou generovány odkazující na soubor s příponou AXD. Například seznam níže zobrazuje odkazy skriptu pro kód skriptu, který generuje požadavek XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Pokud soubor AXD nad přejít pomocí prohlížeče a otevřete ji, zobrazí se kód, který implementuje XMLHttp požadavku. Tato metoda předchází tomu, aby v úpravách souboru skriptu.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Řízení provozu ovládacího prvku TreeView

TreeView – ovládací prvek má několik vlastností, které mají vliv ovládací prvek. Nejobvyklejšími vlastnosti jsou **ShowCheckBoxes**, **ShowExpandCollapse**, a **ShowLines**.

**ShowCheckBoxes** vlastnost ovlivňuje, jestli uzly zobrazí zaškrtávací políčko při vykreslení. Platné hodnoty pro tuto vlastnost jsou **žádný**, **kořenové**, **nadřazené**, **listu**, a **všechny**. Tyto mít vliv na ovládacím prvku TreeView následujícím způsobem:

| **Hodnota vlastnosti** | **Efekt** |
| --- | --- |
| Žádné | Zaškrtávací políčka nejsou zobrazeny na všechny uzly. Toto je výchozí nastavení. |
| Kořenové | Zaškrtávací políčko se zobrazí pouze na kořenový uzel. |
| Nadřazené | Zaškrtávací políčko se zobrazí jenom na ty uzly, které mají podřízené uzly. Tyto podřízené uzly mohou být nadřazené uzly nebo listových uzlů. |
| List | Zaškrtávací políčko se zobrazí pouze na ty uzly, které mají podřízené uzly. |
| Všechny | Zaškrtávací políčko se zobrazí na všech uzlech. |

Když zaškrtávací políčka jsou používány, **CheckedNodes** vlastnost vrátí kolekci uzlů prvku TreeView, které jsou kontrolovány po zpětného odeslání.

**ShowExpandCollapse** vlastnost řídí vzhled obrázku rozbalení/sbalení vedle kořenové a nadřazené uzly. Pokud je tato vlastnost nastavená na **false**, prvek TreeView uzly jsou vykresleny jako hypertextové odkazy a jsou Rozbalit/sbalit kliknutím na odkaz.

**ShowLines** vlastnost určuje, zda jsou zobrazeny čáry připojení nadřazené uzly podřízené uzly. Když **false** (výchozí), jsou zobrazeny žádné řádky. Když **true**, ovládací prvek TreeView použije řádky obrázků ve složce určené parametrem **LineImagesFolder** vlastnost.

Visual Studio .NET 2005 pro přizpůsobení vzhledu čar prvku TreeView, obsahuje nástroj Návrhář řádku. Tento nástroj, pomocí tlačítka inteligentních značek v ovládacím prvku TreeView jako níže můžete používat.


![](data-bound-controls/_static/image1.jpg)

**Obrázek 1**


Když vyberete **vlastní obrázky čar** klikněte na možnost nástroj Návrhář řádek se spustí umožňuje nakonfigurovat vzhled čar prvku TreeView.

## <a name="treeview-events"></a>TreeView – události

TreeView – ovládací prvek má jedinečný následující události:

- SelectedNodeChanged nastane, když je vybrán uzel na základě **SelectAction** vlastnost.
- TreeNodeCheckChanged vyvolá se při změně stavu checkboxs uzly.
- Když se uzel rozbalí, dojde k události TreeNodeExpanded stupněm MODERATE **SelectAction** vlastnost.
- TreeNodeCollapsed vyvolá se při sbalení uzlu.
- TreeNodeDataBound nastane, pokud uzel je data vázaná.
- TreeNodePopulate nastane, když se naplní uzlu.

**SelectAction** vlastnost umožňuje nakonfigurovat, které událost se aktivuje, když je vybrán uzel. Vlastnost SelectAction poskytuje následující akce:

- TreeNodeSelectAction.Expand vyvolává události TreeNodeExpanded při výběru uzlu.
- TreeNodeSelectAction.None vyvolá žádná událost v případě, že je vybrán uzel.
- TreeNodeSelectAction.Select vyvolává událost SelectedNodeChanged při výběru uzlu.
- TreeNodeSelectAction.SelectExpand vyvolává událost SelectedNodeChanged a události TreeNodeExpanded událost, když je vybraný uzel.

## <a name="controlling-appearance-with-styles"></a>Řízení vzhledu se styly

TreeView – ovládací prvek obsahuje mnoho vlastností pro řízení vzhledu ovládacího prvku se styly. Následující vlastnosti jsou k dispozici.

| **Název vlastnosti** | **Ovládací prvky** |
| --- | --- |
| HoverNodeStyle | Ovládací prvky stylu uzlů, když je nastavená myš nad nimi. |
| LeafNodeStyle | Určuje styl uzlů bez podřízených položek. |
| NodeStyle | Určuje styl pro všechny uzly. Styly specifické uzlu (například LeafNodeStyle) přepsat tímto stylem. |
| ParentNodeStyle | Určuje styl pro všechny nadřazené uzly. |
| RootNodeStyle | Určuje styl pro kořenový uzel. |
| SelectedNodeStyle | Určuje styl pro vybraný uzel. |

Každá z těchto vlastností je jen pro čtení. Však budou každý vrátit **TreeNodeStyle** objektů a vlastností tohoto objektu může být upraveno pomocí *vlastnost podvlastností* formátu. Chcete-li například nastavit **ForeColor** vlastnost **SelectedNodeStyle**, použijte následující syntaxi:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Všimněte si, že výše uvedené značka není uzavřená. Důvodem je, když pomocí deklarativní syntaxe je znázorněno zde, by uzly zobrazení TreeViews zahrnout kód HTML.

Vlastnosti stylu je taky možné specifikovat v kódu pomocí *vlastnost.DílčíVlastnost* formátu. Chcete-li například nastavit **ForeColor** vlastnost **RootNodeStyle** v kódu, použijte následující syntaxi:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Úplný seznam vlastnosti různých stylů najdete v dokumentaci MSDN TreeNodeStyle objektu.


## <a name="the-sitemappath-control"></a>Ovládací prvky SiteMapPath ovládacího prvku

Ovládací prvek SiteMapPath poskytuje ovládací prvek navigace bread popis cesty pro vývojáře využívající technologii ASP.NET. Stejně jako ostatní navigační ovládací prvky může být jednoduše, že data svázaná s hierarchickými daty zdrojů, jako jsou SiteMapDataSource nebo XmlDataSource.

Ovládací prvky SiteMapPath ovládací prvek se skládá SiteMapNodeItem objektů. Existují tři typy uzlů; Kořenový uzel nadřazené uzly a aktuální uzel. Kořenový uzel je uzel v horní části hierarchickou strukturu. Aktuální uzel představuje aktuální stránku. Všechny ostatní uzly jsou nadřazených uzlů.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Řízení provozu SiteMapPath ovládacího prvku

Vlastnosti, které řídí operaci ovládací prvek SiteMapPath jsou následující:

| **Vlastnost** | **Popis vlastnosti** |
| --- | --- |
| ParentLevelsDisplayed | Určuje, kolik nadřazené uzly se zobrazí. Výchozí hodnota je -1, které pro žádné omezení počtu nadřazených uzlů zobrazena. |
| PathDirection | Určuje směr mapě webu. Platné hodnoty jsou RootToCurrent (výchozí) a CurrentToRoot. |
| PathSeparator | Řetězec, který určuje znak oddělující uzly v ovládacím prvku ovládací prvky SiteMapPath. Výchozí hodnota je:. |
| RenderCurrentNodeAsLink | Logická hodnota, která určuje, zda je aktuální uzel vykreslen jako odkaz. Výchozí hodnota je False. |
| SkipLinkText | Asistence s přístupností při prohlížení stránky pomocí čtečky obrazovky. Tato vlastnost umožňuje čtečky obrazovky, chcete-li přeskočit SiteMapPath ovládacího prvku. Chcete-li tuto funkci zakázat, nastavte vlastnost na String.Empty. |

## <a name="templated-sitemappath-controls"></a>Ovládací prvky SiteMapPath bez vizuálního vzhledu

SiteMapControl je ovládací prvek bez vizuálního vzhledu a v důsledku toho můžete definovat různé šablony služby pro použití v zobrazení ovládacího prvku. Chcete-li upravit šablony v ovládacím prvku ovládací prvky SiteMapPath, klikněte na tlačítko inteligentních značek na ovládací prvek a vyberte z nabídky Upravit šablony. Zobrazí nabídku SiteMapTasks jak je znázorněno níže, kde můžete vybrat mezi různé šablony, které jsou k dispozici.


![](data-bound-controls/_static/image2.jpg)

**Obrázek 2**


**NodeTemplate** šablona odkazuje na všech uzlech v mapě webu. Pokud uzel je kořenový uzel nebo aktuální uzel a **RootNodeTemplate** nebo **CurrentNodeTemplate** je nakonfigurován, je NodeTemplate přepsána.

## <a name="sitemappath-events"></a>Ovládací prvky SiteMapPath události

Ovládací prvek ovládací prvky SiteMapPath má dvě události, které nejsou odvozeny od třídy ovládacího prvku; **ItemCreated** událostí a **ItemDataBound událost** událostí. ItemCreated událost je aktivována, když je vytvořena položka SiteMapPath. ItemDataBound událost je aktivována při volání metody DataBind při vazbě dat ovládací prvky SiteMapPath uzlu. A **SiteMapNodeItemEventArgs** objekt poskytuje přístup ke konkrétní SiteMapNodeItem prostřednictvím vlastnosti položky.

## <a name="controlling-appearance-with-styles"></a>Řízení vzhledu se styly

Tyto styly jsou k dispozici pro formátování SiteMapPath ovládacího prvku.

| **Název vlastnosti** | **Ovládací prvky** |
| --- | --- |
| CurrentNodeStyle | Určuje styl textu pro aktuální uzel. |
| RootNodeStyle | Určuje styl textu pro kořenový uzel. |
| NodeStyle | Určuje styl textu pro všemi uzly za předpokladu, že CurrentNodeStyle nebo RootNodeStyle neplatí. |

Vlastnost NodeStyle přepsán CurrentNodeStyle nebo RootNodeStyle. Každá z těchto vlastností je jen pro čtení a vrátí **styl** objektu. Pokud chcete mít vliv na vzhled uzlu pomocí jedné z těchto vlastností, je potřeba nastavit vlastnosti Style objektu, který je vrácen. Například následující kód změní vlastnosti forecolor pro aktuální uzel.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

Vlastnost lze použít také programově následujícím způsobem:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Pokud se použije šablona, styl se nepoužije.


## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Praktické cvičení 1: Konfigurace ASP.NET Menu – ovládací prvek

1. Vytvořte nový web.
2. Přidejte soubor mapy webu vyberete soubor, nový, soubor a zvolíte Mapa webu ze seznamu souborů šablon.
3. Otevřete mapy webu (Web.sitemap ve výchozím nastavení) a upravte ho tak, aby to vypadá seznam níže. Na stránkách, ke kterým se připojujete v souboru mapy webu ve skutečnosti neexistují, ale nebude, který chybu pro toto cvičení.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. V návrhovém zobrazení otevřete výchozího webového formuláře.
5. Z části navigační panel nástrojů přidáte nový ovládací prvek nabídky na stránku.
6. Z části dat na panelu nástrojů přidejte nové SiteMapDataSource. SiteMapDataSource automaticky použije soubor Web.sitemap ve vaší lokalitě. (Soubor Web.sitemap *musí* v kořenové složce webu.)
7. Klikněte na ovládací prvek nabídky a potom klikněte na tlačítko inteligentních značek k zobrazení dialogového okna Úkoly nabídky.
8. V rozevírací nabídce zvolit zdroj dat vyberte SiteMapDataSource1.
9. Klikněte na odkaz automatického formátování a Volba formátu pro nabídku.
10. V podokně vlastností nastavte **hodnota StaticDisplayLevels** vlastnost na 2. Ovládací prvek nabídky nyní zobrazit uzel Home, produkty a služby v návrháři.
11. Projděte stránku v prohlížeči pomocí nabídky. (Protože stránek, které jste přidali do mapy webu ve skutečnosti neexistují, zobrazí se chyba při akci a přejděte k nim.)

Experimentujte s měnícími hodnota StaticDisplayLevels a vlastností MaximumDynamicDisplayLevels a zobrazit, jak ovlivňují, jak je vykreslen v nabídce.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Praktické cvičení 2: Dynamické vazby prvku TreeView

V tomto cvičení předpokládá, že máte SQL Server je spuštěn místně a v instanci systému SQL Server nachází databáze Northwind. Nejsou-li tyto podmínky splněny, změňte připojovací řetězec v ukázce. Všimněte si, že budete také muset zadejte ověřování SQL serveru, místo důvěryhodné připojení.

1. Vytvořte nový web.
2. Přepnout na zobrazení kódu pro Default.aspx a nahraďte celý kód kód uvedený níže. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Uložte na stránku jako treeview.aspx.
4. Přejděte na stránku.
5. Při prvním zobrazení stránky, zobrazení zdroje stránky v prohlížeči. Všimněte si, že pouze viditelné uzly byly odeslány do klienta.
6. Klikněte na znaménko plus vedle každého uzlu.
7. Zobrazit zdroj na stránce znovu. Všimněte si, že nově zobrazené uzly jsou teď k dispozici.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Praktické cvičení 3: Podrobně popisuje zobrazení a úpravy dat pomocí ovládacího prvku GridView a prvku DetailsView.

1. Vytvořte nový web.
2. Přidáte nový soubor web.config na webovém serveru.
3. Přidáte připojovací řetězec v souboru web.config, jak je znázorněno níže: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Budete muset změnit připojovací řetězec na základě vašeho prostředí.
4. Uložte a zavřete soubor web.config.
5. Otevřete soubor Default.aspx a přidat nový ovládací prvek SqlDataSource.
6. Změna ID ovládacího prvku SqlDataSource **produkty**.
7. V **Úkoly SqlDataSource** nabídky, klikněte na tlačítko **konfigurace zdroje dat**.
8. Vyberte **Northwind** v rozevírací nabídce připojení a klikněte na tlačítko Další.
9. Vyberte **produkty** z **název** rozevírací seznam a zaškrtněte políčko **ProductID**, **ProductName**, **UnitPrice**, a **UnitsInStock** zaškrtávací políčka, jak je znázorněno níže. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Klikněte na tlačítko **Další**.
11. Klikněte na tlačítko **Dokončit**.
12. Přepněte do zobrazení zdroje a prozkoumejte kód, který byl vygenerován. Všimněte si, že **SelectCommand**, **událost DeleteCommand**, **událost InsertCommand**, a **událost UpdateCommand** , které byly přidány do ovládacím prvkem SqlDataSource ovládací prvek. Všimněte si také parametry, které byly přidány.
13. Přepněte do zobrazení návrhu a na stránku přidat nový ovládací prvek GridView.
14. Vyberte **produkty** z **zvolit zdroj dat** rozevíracího seznamu.
15. Zkontrolujte **povolit stránkování** a **povolit výběr** jak je znázorněno níže. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Klikněte na tlačítko **upravit sloupce** propojit a ujistěte se, že **automaticky generovat pole** je zaškrtnuté políčko.
17. Klikněte na tlačítko **OK**.
18. U ovládacího prvku GridView vybraný, klikněte na tlačítko Další položky **DataKeyNames** vlastnosti v podokně vlastností.
19. Vyberte **ProductID** z **dostupná datová pole** seznamu a klikněte na tlačítko **&gt;** tlačítko Přidat.
20. Klikněte na tlačítko OK.
21. Přidáte nový ovládací prvek SqlDataSource na stránku.
22. Změna ID ovládacího prvku SqlDataSource **podrobnosti**.
23. V nabídce úlohy SqlDataSource zvolte **konfigurace zdroje dat**.
24. Zvolte **Northwind** z rozevíracího seznamu a klikněte na tlačítko **Další**.
25. Vyberte <strong>produkty</strong> z <strong>název</strong> rozevírací seznam a zaškrtněte políčko <strong> \</ strong > * zaškrtávací políčko ve <strong>sloupce</strong> listbox.
26. Klikněte na tlačítko **kde** tlačítko.
27. Vyberte **ProductID** z **sloupec** rozevíracího seznamu.
28. Vyberte **=** v rozevírací nabídce operátor.
29. Vyberte **ovládací prvek** z **zdroj** rozevíracího seznamu.
30. Vyberte **GridView1** z **ID ovládacího prvku** rozevíracího seznamu.
31. Klikněte na tlačítko **přidat** tlačítko pro přidání klauzule WHERE.
32. Klikněte na tlačítko **OK**.
33. Klikněte na tlačítko **Upřesnit** tlačítko a zaškrtněte **Generovat příkazy INSERT, UPDATE a DELETE** zaškrtávací políčko.
34. Klikněte na tlačítko **OK**.
35. Klikněte na tlačítko **Další** a klikněte na tlačítko **Dokončit**.
36. Přidání ovládacího prvku DetailsView. na stránce.
37. V **zvolit zdroj dat** rozevíracím seznamu zvolte **podrobnosti**.
38. Zkontrolujte, **povolit úpravy** zaškrtávací políčko, jak je znázorněno níže. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Uložit na stránku a procházet Default.aspx.
40. Klikněte na tlačítko **vyberte** odkaz vedle různým záznamům zobrazíte aktualizace prvku DetailsView.
41. Klikněte na tlačítko **upravit** odkazu v ovládacím prvku DetailsView.
42. Změňte záznam a klikněte na tlačítko **aktualizace**.
