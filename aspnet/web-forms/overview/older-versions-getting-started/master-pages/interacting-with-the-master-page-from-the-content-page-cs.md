---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
title: Interakce stránky obsahu (C#) se stránkou předlohy | Dokumentace Microsoftu
author: rick-anderson
description: Zkoumá, jak volat metody, nastavte vlastnosti, další stránky předlohy se stránkou z kódu na stránce obsahu.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 32d54638-71b2-491d-81f4-f7417a13a62f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-cs
msc.type: authoredcontent
ms.openlocfilehash: afcec6cb7a6763d068301c7afdd28ff560a3065b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755736"
---
<a name="interacting-with-the-master-page-from-the-content-page-c"></a>Interakce stránky obsahu (C#) se stránkou předlohy
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_CS.zip) nebo [stahovat PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_CS.pdf)

> Zkoumá, jak volat metody, nastavte vlastnosti, další stránky předlohy se stránkou z kódu na stránce obsahu.


## <a name="introduction"></a>Úvod

V průběhu posledních pěti kurzů jsme se podívat na tom, jak vytvořit stránku předlohy, definujte oblasti obsahu, vytvořit vazbu stránky technologie ASP.NET na stránku předlohy a definovat obsah specifický pro stránku. Když návštěvník požaduje konkrétní stránky obsahu, obsah a kód hlavní stránky jsou začleněny za běhu, což vede k vykreslení hierarchie jednotné ovládacího prvku. Proto jsme už viděli jednu způsob, která můžou pracovat na hlavní stránce a jeden z jeho obsahu stránky: stránka obsahu obsahuje značky k transfuse do ovládacích prvků ContentPlaceHolder na hlavní stránce.

Co musíme ještě prozkoumejte je, jak stránky předlohy a stránky obsahu mohou komunikovat prostřednictvím kódu programu. Kromě definování značek pro ovládací prvky ContentPlaceHolder stránky předlohy, stránku obsahu můžete přiřadit hodnoty k hlavní stránce veřejné vlastnosti a volat jeho veřejné metody. Podobně hlavní stránky může komunikovat s jeho obsahem stránky. Programové interakce mezi stránky předlohy a obsahu je méně častý než interakce mezi jejich deklarativního označení, existuje mnoho scénářů, kde je potřeba tyto programové interakce.

V tomto kurzu Zkoumáme, jak můžete stránku obsahu programově interagovat s jeho hlavní stránky; v dalším kurzu se podíváme na hlavní stránce můžete podobně interakci s jeho obsahem stránky.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Příklady programové interakce mezi stránku obsahu a jeho hlavní stránky

Konkrétní oblasti stránky je potřeba nakonfigurovat na základě stránku po stránce, používáme ovládací prvek ContentPlaceHolder. Ale co situacích, kdy se většinou stránky zapotřebí k emitování určité výstup, ale malý počet stránek potřeba ji upravit, aby zobrazit něco jiného? Jedním z takových příkladů, které jsme se zaměřili [ *několik prvků ContentPlaceHolder a výchozí obsah* ](multiple-contentplaceholders-and-default-content-cs.md) kurzu zahrnuje zobrazení přihlášení rozhraní na jednotlivých stránkách. Při přihlášení rozhraní by měla obsahovat většinu stránek, se má výjimka potlačit pro několik stránek, jako například: hlavní přihlašovací stránky (`Login.aspx`); stránce vytvořit účet; a jiných stránek, které jsou přístupné pro ověřené uživatele. [ *Několik prvků ContentPlaceHolder a výchozí obsah* ](multiple-contentplaceholders-and-default-content-cs.md) kurz vám ukázal, jak definovat výchozí obsah pro prvek ContentPlaceHolder na stránce předlohy a stránky jak přepsat v těch, kde výchozí obsah však nebylo záměrem.

Další možností je vytvoření veřejné vlastnosti nebo metody v rámci stránky předlohy, která určuje, jestli se má zobrazit nebo skrýt rozhraní přihlášení. Hlavní stránka může obsahovat třeba veřejnou vlastnost s názvem `ShowLoginUI` jehož hodnota se použil k `Visible` vlastnost ovládacího prvku pro přihlášení na stránce předlohy. Potom programově nastavit tyto obsahu stránky, kde má být potlačeno uživatelského rozhraní pro přihlášení `ShowLoginUI` vlastnost `false`.

Nejběžnějším příkladem interakce stránky předlohy a obsahu pravděpodobně nastane, pokud data zobrazí v hlavní stránka potřebám se aktualizuje po určitou akci vypršel na stránce obsahu. Vezměte v úvahu, že se na stránku předlohy, která zahrnuje GridView zobrazující pět naposledy přidala záznamy z konkrétní databázové tabulky a, že jedna z jeho obsahu stránky obsahuje rozhraní pro přidávání nových záznamů do stejné tabulky.

Když uživatel navštíví stránku přidat nový záznam, zobrazí se mu uživateli, že že pět naposledy přidaný záznamy zobrazené na stránce předlohy. Po vyplnění hodnot pro sloupce nový záznam, uživatel formulář odešle. Za předpokladu, že má GridView na hlavní stránce jeho `EnableViewState` nastavenou na hodnotu true (výchozí), jeho obsah je znovu načten ze zobrazení stavu a v důsledku toho se zobrazí pět záznamů stejné, i v případě, že novější záznam se právě přidané do databáze. To může zmást uživatele.

> [!NOTE]
> I v případě, že zakážete prvku GridView zobrazení stavu tak, aby znovu připojí k jeho základního zdroje dat na každém postbacku, stále nezobrazí just přidá záznam vzhledem k tomu, že data svázaná s dříve v životního cyklu stránky než při přidání nového záznamu datab prvku GridView. služby ase.


Chcete-li to napravit tak, aby právě přidali záznam se zobrazí na stránce předlohy prvku GridView na zpětné dáte pokyn, aby GridView znovu připojit ke zdroji dat potřebujeme *po* byl přidán nový záznam do databáze. To vyžaduje interakci mezi obsahem a stránky předlohy, protože je rozhraní pro přidání nového záznamu (a jeho obslužné rutiny událostí) se v obsahu stránky, ale prvku GridView, kterou je potřeba aktualizovat na stránce předlohy.

Protože aktualizuje zobrazení stránky předlohy z obslužné rutiny události na stránce obsahu je jedním z nejběžnějších potřeby interakce stránky předlohy a obsahu, podíváme se na toto téma podrobněji. Soubor ke stažení pro účely tohoto kurzu obsahuje databázi Microsoft SQL Server 2005 Express Edition s názvem `NORTHWIND.MDF` na webu `App_Data` složky. Databáze Northwind ukládá produktu, zaměstnance a informace o prodeji pro fiktivní společnosti Northwind Traders.

Procházení krok 1 až pět naposledy zobrazení přidat produkty v prvku GridView na hlavní stránce. Krok 2 vytvoří stránku obsahu pro přidání nové produkty. Krok 3 zabývá vytvoření veřejné vlastnosti a metody na hlavní stránce a kroku 4 ukazuje, jak prostřednictvím kódu programu rozhraní s těmito vlastnostmi a metodami ze stránky obsahu.

> [!NOTE]
> V tomto kurzu není delve do konkrétních podrobnostech pracovat s daty v ASP.NET. Kroky pro vytvoření stránky předlohy pro zobrazení dat a obsahu stránky pro vkládání dat jsou kompletní, ještě breezy. Pro podrobnější pohled na zobrazení a vkládání dat a použití ovládacích prvků SqlDataSource a ovládacího prvku GridView najdete na konci tohoto kurzu prostředků v části Další údaje.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Krok 1: Pět naposledy zobrazení přidat produktů na stránce předlohy

Otevřít `Site.master` stránku předlohy a přidejte popisek a do ovládacího prvku GridView `leftContent` `<div>`. Vymazání popisku `Text` vlastnost, nastavte jeho `EnableViewState` vlastnost na hodnotu false a jeho `ID` vlastnost `GridMessage`; nastavit prvku GridView `ID` vlastnost `RecentProducts`. Z návrháře, dále rozbalte inteligentní značky prvku GridView a zvolte možnost pro vytvoření vazby na nový zdroj dat. Otevře se Průvodce konfigurací zdroje dat. Protože v databázi Northwind `App_Data` složka je databáze Microsoft SQL Server, můžete vytvořit SqlDataSource tak, že vyberete (viz obrázek 1); název ve třídě SqlDataSource `RecentProductsDataSource`.


[![Svázat s ovládacím prvkem SqlDataSource s názvem RecentProductsDataSource prvku GridView.](interacting-with-the-master-page-from-the-content-page-cs/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image1.png)

**Obrázek 01**: svázat ovládací prvek SqlDataSource název prvku GridView `RecentProductsDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image3.png))


Dalším krokem výzva k určení, co se připojit k databázi. Zvolte `NORTHWIND.MDF` databázových souborů z rozevíracího seznamu a klikněte na tlačítko Další. Protože to je poprvé, kdy jsme použili této databáze, průvodce bude nabízet k uložení připojovacího řetězce v `Web.config`. Jeho uložení připojovacího řetězce, pomocí názvu `NorthwindConnectionString`.


[![Připojení k databázi Northwind](interacting-with-the-master-page-from-the-content-page-cs/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image4.png)

**Obrázek 02**: připojení k databázi Northwind ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image6.png))


Průvodce konfigurace zdroje dat poskytuje dva prostředky, které lze zadat dotaz použitý k načtení dat:

- Tak, že zadáte vlastní příkaz SQL nebo uloženou proceduru, nebo
- Výběr tabulky nebo zobrazení a zadáním sloupce se mají vrátit

Protože chceme návratové že jen pět naposledy přidaný produkty, potřebujeme zadat vlastní příkaz jazyka SQL. Pomocí následujícího dotazu SELECT:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample1.sql)]

`TOP 5` – Klíčové slovo vrací jenom prvních pět záznamů z dotazu. `Products` Primárního klíče tabulky `ProductID`, je `IDENTITY` sloupec, který nám zajišťuje, že každého nového produktu do tabulky přidat bude mít hodnotu větší než předchozí položka. Proto řazení výsledků podle `ProductID` v sestupném pořadí vrátí produkty počínaje naposledy vytvořený z nich.


[![Vrátí pět naposledy přidané produktů](interacting-with-the-master-page-from-the-content-page-cs/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image7.png)

**Obrázek 03**: vrátit pět nejčastěji nedávno přidali produkty ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image9.png))


Po dokončení průvodce se sada Visual Studio generuje dvě BoundFields pro prvek GridView zobrazíte `ProductName` a `UnitPrice` pole vrácená z databáze. Deklarativní hlavní stránky v tomto okamžiku by měl obsahovat značky podobný následujícímu:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample2.aspx)]

Jak je vidět, obsahuje značky: ovládací prvek popisek webového (`GridMessage`); prvku GridView `RecentProducts`s dvěma BoundFields; a SqlDataSource ovládací prvek, který vrátí pět naposledy přidaný produktů.

Pomocí tohoto ovládacího prvku GridView vytvořen a jeho SqlDataSource ovládací prvek nakonfigurovat přejděte na webovou stránku prostřednictvím prohlížeče. Jak je vidět na obrázku 4, zobrazí se, že se přidala do mřížky v levém dolním rohu, který obsahuje pět naposledy produktů.


[![Pět naposledy přidané produktů zobrazí prvku GridView.](interacting-with-the-master-page-from-the-content-page-cs/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image10.png)

**Obrázek 04**: pět nejčastěji nedávno přidali produktů zobrazí prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image12.png))


> [!NOTE]
> Nebojte se vyčistit vzhled prvku GridView. Některé návrhy zahrnují formátování zobrazených `UnitPrice` hodnotu jako měnu a použití písma a barvy pozadí ke zlepšení vzhledu mřížky.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Krok 2: Vytvoření obsahu stránky pro přidání nové produkty

Naše dalším krokem je vytvoření obsahu stránky, ze kterého může uživatel přidat nový produkt, který má `Products` tabulky. Přidejte novou stránku obsahu, aby `Admin` složku s názvem `AddProduct.aspx`a vytvořte mu vazbu k `Site.master` stránky předlohy. Obrázek 5 ukazuje Průzkumník řešení po přidání této stránce na webu.


[![Přidejte novou stránku ASP.NET ke složce Admin](interacting-with-the-master-page-from-the-content-page-cs/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image13.png)

**Obrázek 05**: přidejte novou stránku ASP.NET `Admin` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image15.png))


Připomínáme, že v [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) kurzu jsme vytvořili vlastní stránku základní třídu s názvem `BasePage` , který v případě, že byla vygenerována název stránky není explicitně nastavena. Přejděte `AddProduct.aspx` kódu stránky třídy a nechat ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` soubor zahrnout položku pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro lekce problémy pojmenování ID ovládacího prvku:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample3.xml)]

Jak je znázorněno na obrázku 6, přidání tohoto `<siteMapNode>` element se projeví v seznamu lekce.

Vraťte se na `AddProduct.aspx`. V ovládacím prvku obsahu pro `MainContent` ContentPlaceHolder, přidejte ovládací prvek DetailsView a pojmenujte ho `NewProduct`. Svázat s ovládacím prvku DetailsView. nový ovládací prvek SqlDataSource s názvem `NewProductDataSource`. Jako s ovládacím prvkem SqlDataSource v kroku 1, nakonfigurovat průvodce tak, aby používal databázi Northwind a pokud se rozhodnete zadat vlastní příkaz jazyka SQL. Vzhledem k tomu, že ovládacím prvku DetailsView se použije k přidání položky do databáze, potřeba zadat současně `SELECT` příkazu a `INSERT` příkazu. Pomocí následujících `SELECT` dotazu:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample4.sql)]

Potom přidejte následující ze záložky vložení `INSERT` – příkaz:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample5.sql)]

Po dokončení Průvodce přejít na ovládacím prvku DetailsView inteligentních značek a zaškrtněte políčko "Povolit vložení". Tento postup přidá CommandField ovládací prvek DetailsView s jeho `ShowInsertButton` vlastností nastavenou na hodnotu true. Protože tento prvek DetailsView se použije pouze pro vkládání dat, nastavte ovládacím prvku DetailsView `DefaultMode` vlastnost `Insert`.

To je všechno je to! Můžeme otestovat tuto stránku. Navštivte `AddProduct.aspx` prostřednictvím prohlížeče, zadejte název a cena (viz obrázek 6).


[![Přidání nového produktu do databáze](interacting-with-the-master-page-from-the-content-page-cs/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image16.png)

**Obrázek 06**: Přidání nového produktu do databáze ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image18.png))


Po zadání názvu a cena za nový produkt, klikněte na tlačítko Vložit. To způsobí, že formulář odeslat zpět. Na zpětné volání, ovládacím prvkem SqlDataSource společnosti `INSERT` je proveden příkaz; jeho dva parametry jsou vyplněna hodnot zadaných uživatelem v ovládacím prvku DetailsView dvou ovládacích prvků textového pole. Bohužel neexistuje žádný vizuální zpětnou vazbu, že došlo k vložení. Bylo by dobré si mají zpráva zobrazená, potvrzení, že byl přidán nový záznam. Můžu ponechte toto cvičení pro čtečku. Navíc po přidání nového záznamu v ovládacím prvku DetailsView. GridView na hlavní stránce stále zobrazuje stejné pět záznamů jako před; neobsahuje záznam právě přidali. Prozkoumáme jak to napravit v dalších krocích.

> [!NOTE]
> Kromě přidání nějakou formu vizuální zpětnou vazbu, která jsou insert proběhla úspěšně, by neváhejte se také aktualizovat zahrnout ověřování v rozhraní vkládání ovládacím prvku DetailsView. V současné době neexistuje žádné ověřování. Pokud uživatel zadá neplatná hodnota pro `UnitPrice` pole, jako například "příliš drahé," na zpětné být vyvolána výjimka, když se systém pokusí převést tento řetězec na desetinné číslo. Další informace o přizpůsobení vkládání rozhraní, přečtěte si [ *přizpůsobení rozhraní pro úpravu dat* kurzu](../../data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) z mé [práce s Data sérii](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Krok 3: Vytvoření veřejné vlastnosti a metody na stránce předlohy

V kroku 1 jsme přidali ovládací prvek popisek Web s názvem `GridMessage` nad GridView na hlavní stránce. Tento popisek má volitelně zobrazit zprávu. Například po přidání nového záznamu do `Products` tabulky, může být má být zobrazena zpráva: "*ProductName* byla přidána do databáze." Namísto pevně zakódovat text pro tento popisek na stránce předlohy nám může být vhodné zprávu lze přizpůsobit pomocí stránky obsahu.

Vzhledem k tomu, že ovládací prvek popisku je implementovaný jako chráněné členské proměnné v rámci stránky předlohy nelze přistupovat přímo z obsahu stránky. Chcete-li pracovat s popiskem v rámci stránky předlohy ze stránky obsahu (nebo k tomuto účelu libovolný ovládací prvek webové stránce předlohy) potřebujeme vytvořit na stránce předlohy, která poskytuje ovládací prvek nebo slouží jako proxy server, podle kterého jednoho z jeho vlastností můžou být veřejná vlastnost  získat přístup. Přidejte následující syntaxi použití modelu code-behind třídy stránky předlohy k vystavení popisku `Text` vlastnost:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample6.cs)]

Při přidání nového záznamu `Products` tabulku z obsahu stránky `RecentProducts` GridView na hlavní stránce potřebuje znovu její podkladový zdroj dat připojit. Znovu připojit volání GridView jeho `DataBind` metoda. Protože GridView na hlavní stránce není přístupný prostřednictvím kódu programu na obsahu stránky, budeme potřeba vytvořit veřejnou metodu na stránce předlohy, která při volání, znovu připojí data k prvku GridView. Přidejte následující metodu do třídy modelu code-behind na hlavní stránce:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample7.cs)]

S `GridMessageText` vlastnost a `RefreshRecentProductsGrid` metoda na místě, stránka obsahu můžete prostřednictvím kódu programu nastavit nebo načíst hodnotu `GridMessage` popisku `Text` vlastnost nebo znovu připojit data, která mají `RecentProducts` ovládacího prvku GridView. Krok 4 zkoumá, jak pro přístup k veřejné vlastnosti a metody na hlavní stránce z obsahu stránky.

> [!NOTE]
> Nezapomeňte k označení hlavní stránky vlastností a metod jako `public`. Pokud jste není označení explicitně těchto vlastností a metod jako `public`, nebudou přístupné z obsahu stránky.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Krok 4: Volání na stránce předlohy veřejné členy z obsahu stránky

Teď, když hlavní stránka má potřebné veřejné vlastnosti a metody, je připraven k vyvolání těchto vlastností a metod od `AddProduct.aspx` stránku obsahu. Konkrétně, musíme nastavit na hlavní stránce `GridMessageText` vlastnosti a volání jeho `RefreshRecentProductsGrid` metoda po přidání nového produktu do databáze. Všechny ovládací prvky webové data technologie ASP.NET aktivovat události bezprostředně před a po dokončení různé úkoly, které usnadní vývojářům stránky určitou akci programový před nebo za úkol. Například, když koncový uživatel klepne na tlačítko pro vložení prvku DetailsView, na odeslat zpět DetailsView vyvolá jeho `ItemInserting` událostí před zahájením vkládání pracovního postupu. Poté vloží záznam do databáze. Pod ovládacím prvku DetailsView vyvolá jeho `ItemInserted` událostí. Proto, aby bylo možné pracovat na hlavní stránce po přidání nového produktu, vytvořit obslužnou rutinu události pro ovládacím prvku DetailsView `ItemInserted` událostí.

Existují dva způsoby, které stránky obsahu můžete programově rozhraní s jeho hlavní stránky:

- Použití `Page.Master` vlastnost, která vrátí volného typu odkaz na stránku předlohy, nebo
- Zadejte na stránce nebo typu souboru cestu k předlohové stránce prostřednictvím `@MasterType` direktiv; to automaticky přidá vlastnost silného typu na stránku s názvem `Master`.

Podívejme se na oba přístupy.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Použití volného typu`Page.Master`vlastnost

Všechna rozhraní ASP.NET web pages, musí být odvozen od `Page` třídu, která se nachází v `System.Web.UI` oboru názvů. `Page` Obsahuje třídy [ `Master` vlastnost](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) , který vrátí odkaz na hlavní stránku. Pokud na stránce nemá na stránku předlohy `Master` vrátí `null`.

`Master` Vlastnost vrátí objekt typu [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (také umístěny ve `System.Web.UI` oboru názvů) která je základní typ, ze kterého jsou všechny stránky předlohy odvozeny z. Proto pro použití veřejné vlastnosti nebo metody definované v náš web stránku předlohy jsme musíte přetypovat `MasterPage` objekt vrácený z `Master` vlastnost příslušného typu. Protože jsme pojmenovali naše soubor předlohové stránky `Site.master`, označovala jako třída použití modelu code-behind `Site`. Proto následující kód přetypování `Page.Master` vlastnost instance třídy lokality.


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample8.cs)]

Když teď máme zasazena volného typu `Page.Master` vlastnost `Site` typ odkazujeme vlastnostem a metodám, které jsou specifické pro lokalitu. Jak ukazuje obrázek 7, veřejná vlastnost `GridMessageText` se zobrazí v rozevíracím seznamu technologie IntelliSense.


[![Technologie IntelliSense zobrazuje veřejné vlastnosti a metody naši stránku předlohy](interacting-with-the-master-page-from-the-content-page-cs/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image19.png)

**Obrázek 07**: IntelliSense zobrazují naše stránky předlohy veřejné vlastnosti a metody ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image21.png))


> [!NOTE]
> Pokud pojmenujete vaší soubor předlohové stránky `MasterPage.master` je název třídy modelu code-behind na hlavní stránce `MasterPage`. To může vést k nejednoznačný kódu při přetypování z typu `System.Web.UI.MasterPage` do vaší `MasterPage` třídy. Stručně řečeno musíte k plnému určení typu, který se přetypování na, což může být trochu složité, při použití modelu projektu webové stránky. Moje návrhu může být buď Ujistěte se, že při vytváření stránky předlohy ji pojmenujete jinak než `MasterPage.master` nebo ještě lepší vytvořit silného typu odkaz na hlavní stránku.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Vytvoření odkazu silného typu pomocí`@MasterType`– direktiva

Pokud prohlédněte si blíže uvidíte, že třída použití modelu code-behind stránky technologie ASP.NET je částečné třídy (Poznámka: `partial` – klíčové slovo v definici třídy). Částečné třídy byly zavedeny v C# a Visual Basic with.NET Framework 2.0 a řečeno v kostce, umožňují pro členy třídy, které chcete definovat v rámci více souborů. Soubor třídy modelu code-behind - `AddProduct.aspx.cs`, například – obsahuje kód, který, vývojář, vytvoříme. Kromě našeho kódu modulu ASP.NET automaticky vytvoří soubor samostatné třídy s vlastnostmi a obslužné rutiny událostí v, který převede deklarativní na hierarchii tříd na stránce.

Automatické generování kódu, ke kterému dochází pokaždé, když navštíví stránku ASP.NET usnadní cestu pro některé možnosti spíše zajímavé a užitečné. V případě stránky předlohy, pokud nám říct modul ASP.NET používá jaké stránky předlohy naši stránku obsahu vygeneruje silného typu `Master` vlastnost pro nás.

Použití [ `@MasterType` směrnice](https://msdn.microsoft.com/library/ms228274.aspx) informovat modul ASP.NET typ stránky obsahu stránky předlohy. `@MasterType` Směrnice může přijmout typ název stránky předlohy nebo cesta k souboru. Chcete-li určit, že `AddProduct.aspx` stránce používá `Site.master` jako jeho stránku předlohy, přidejte následující direktivy k hornímu okraji `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample9.aspx)]

Tato direktiva dá pokyn modulu ASP.NET přidání silného typu odkazu na hlavní stránku prostřednictvím vlastnosti s názvem `Master`. S `@MasterType` direktiv v místě, můžeme můžete volat `Site.master` hlavním veřejné vlastnosti a metody přímo pomocí stránky `Master` vlastnost bez žádné přetypování.

> [!NOTE]
> Vynecháte-li `@MasterType` směrnice, syntaxe `Page.Master` a `Master` vrátí stejnou věc: objekt volného typu na hlavní stránku. Pokud zahrnete `@MasterType` pak – direktiva `Master` vrátí odkaz na zadanou stránku předlohy silného typu. `Page.Master`, ale stále vrátí volného typu odkaz. Podrobnější rozbor důvod, proč tomu tak a jak `Master` vlastnost je vytvořen při `@MasterType` – direktiva je součástí naleznete v tématu [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)na blogu [ `@MasterType` v technologii ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Aktualizace stránky předlohy se stránkou po přidání nového produktu

Teď, když víme, jak vyvolat veřejné vlastnosti a metody ze stránky obsahu stránky předlohy, jsme připraveni aktualizovat `AddProduct.aspx` stránce tak, aby na hlavní stránce se aktualizují po přidání nového produktu. Na začátku kroku 4 jsme vytvořili obslužnou rutinu události pro ovládací prvek DetailsView `ItemInserting` událost, která spustí ihned po nového produktu se přidala do databáze. Přidejte následující kód do této obslužné rutiny události:


[!code-csharp[Main](interacting-with-the-master-page-from-the-content-page-cs/samples/sample10.cs)]

Ve výše uvedeném kódu používá obě volného typu `Page.Master` vlastnost a silných `Master` vlastnost. Všimněte si, `GridMessageText` je nastavena na "*ProductName* přidané do mřížky..." Produkt právě přidané hodnoty jsou k dispozici prostřednictvím `e.Values` kolekce, jak je vidět, právě přidané `ProductName` hodnota se přistupuje přes `e.Values["ProductName"]`.

Obrázek 8 ukazuje `AddProduct.aspx` stránku ihned po nového produktu - Scottova Soda – byla přidána do databáze. Mějte na paměti, že je název produktu právě přidali jste si poznamenali v popisku stránky předlohy a prvku GridView aktualizovala zahrnout produktu a jeho cenou.


[![Popisek a GridView zobrazit právě přidané produktu na stránce předlohy](interacting-with-the-master-page-from-the-content-page-cs/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-cs/_static/image22.png)

**Obrázek 08**: popisek a prvek GridView na hlavní stránku zobrazit Just-Added produktu ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-master-page-from-the-content-page-cs/_static/image24.png))


## <a name="summary"></a>Souhrn

V ideálním případě stránky předlohy a její obsah stránky jsou plně oddělené od sebe a vyžadovat žádná úroveň interakce. Zatímco stránky předlohy a stránky obsahu by se měly navrhovat s danému cíli v úvahu, existuje mnoho běžných scénářů, ve kterých musí rozhraní stránku obsahu pomocí jeho stránky předlohy. Jeden z nejběžnějších soustředí kolem aktualizuje konkrétní části zobrazení stránky předlohy založené na určitou akci, k čemu na stránce obsahu.

Dobrou zprávou je, že je poměrně přímočarý obsahu stránky programově interagovat s jeho hlavní stránky. Začněte vytvořením veřejné vlastnosti nebo metody ve stránce předlohy, které zapouzdřují funkce, které je potřeba vyvolat stránku obsahu. Potom na stránce obsahu přístup k vlastnosti a metody na hlavní stránce přes volného typu `Page.Master` vlastnosti nebo použití `@MasterType` směrnice vytvoření silného typu odkazu na hlavní stránku.

V dalším kurzu jsme zkoumat, jak programově interagovat s jedním z jeho stránky obsahu stránky předlohy.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přístup k a aktualizace dat v ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Stránek předloh ASP.NET: Tipy, triky a depeše](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` v technologii ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Předávání informací mezi obsah a stránkami předloh](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Práce s daty v kurzy k ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Zack Jones. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](control-id-naming-in-content-pages-cs.md)
> [další](interacting-with-the-content-page-from-the-master-page-cs.md)
