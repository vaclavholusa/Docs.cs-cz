---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interakce se stránkou obsahu ze stránky předlohy (VB) | Dokumentace Microsoftu
author: rick-anderson
description: Zkoumá, jak volat metody, nastavte vlastnosti atd stránky obsahu z kódu na stránce předlohy.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 7ace3873ecb525afcb64a0aa144742eab467f8f6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756938"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interakce se stránkou obsahu ze stránky předlohy (VB)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si kód](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) nebo [stahovat PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Zkoumá, jak volat metody, nastavte vlastnosti atd stránky obsahu z kódu na stránce předlohy.


## <a name="introduction"></a>Úvod

V předchozím kurzu prověřit, jak programově interagovat s jeho hlavní stránky obsahu stránky. Vzpomínáte, že jsme aktualizovali, hlavní stránky, aby zahrnovala ovládacím prvku GridView, uvedený pět naposledy přidat produkty. Potom jsme vytvořili obsahu stránky, ze kterého může uživatel přidat nový produkt. Při přidání nového produktu, stránky obsahu potřeba dát pokyn stránka předlohy a aktualizovat jeho GridView tak, aby měl by obsahovat produktu právě přidali. Tato funkce byla lze provést přidáním veřejnou metodu na stránce předlohy, aktualizují data svázaná s prvku GridView, a pak volání této metody ze stránky obsahu.

Nejběžnější forma interakce stránky předlohy a obsahu pochází ze stránky obsahu. Nicméně je možné stránky předlohy k rouse aktuální stránku obsahu akce na základě a tato funkce může být potřeba, když hlavní stránka obsahuje prvky uživatelského rozhraní, které umožňují uživatelům měnit data, která se zobrazuje taky na stránce obsahu. Zvažte možnost obsahu stránky, ovládací prvek zobrazí informace o produktech v GridView a stránku předlohy, která obsahuje tlačítko ovládací prvek, který, při kliknutí na, zdvojnásobí ceny pro všechny produkty. Podobně jako v příkladu v předchozím kurzu prvku GridView. je potřeba aktualizovat po cena dvojité kliknutí na tlačítko tak, aby zobrazil nové ceny, ale v tomto scénáři je stránky předlohy, kterou je potřeba rouse akce na stránce obsahu.

Tento kurz popisuje, jak jste na stránce předlohy volání funkce je definována v obsahu stránky.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Pokus programové interakce prostřednictvím události a obslužné rutiny událostí

Volání funkce stránky obsahu ze stránky předlohy je náročnější než naopak. Protože obsah stránky má jednu stránku předlohy, pokus programové interakce stránky obsahu víme veřejné metody a vlastnosti jsou naše službám. Stránky předlohy, však může mít mnoho různých obsahu stránek, každý s vlastní sadu vlastností a metod. Jak pak můžete jsme psát kód, na stránce předlohy k provedení nějaké akce v jeho obsahu stránky při nevíme, jaké stránky obsahu, který bude vyvolán do běhu?

Vezměte v úvahu ovládací prvek technologie ASP.NET, jako je například ovládací prvek tlačítko. Ovládací prvek tlačítko se může objevit v libovolném počtu stránek ASP.NET a je třeba mechanismus, podle kterého ho může upozornit na stránce, že bylo stisknuto. Využívá se při něm *události*. Zejména ovládacího prvku tlačítko vyvolá jeho `Click` událostí při kliknutí; stránka ASP.NET, která obsahuje tlačítko můžete volitelně reakce na oznámení prostřednictvím *obslužná rutina události*.

Tento stejný vzor je možné mít funkce triggeru stránky předlohy v jeho obsahu stránky:

1. Přidejte událost do stránky předlohy.
2. Vyvolejte událost pokaždé, když se na hlavní stránce potřebuje ke komunikaci s jeho obsahu stránky. Například pokud stránky předlohy musí upozornění jeho obsahu stránky, že uživatel má dvojitá ceny, jeho událost by být vyvolány okamžitě po ceny být dvojitá.
3. Vytvořte obslužnou rutinu události v obsahu stránek, které musíte provést určitou akci.

Tato zbývající část tohoto kurzu implementuje uvedené v úvodu; stránky obsahu, který zobrazuje seznam produktů, které v databázi a stránku předlohy, která obsahuje tlačítko, konkrétně, řízení na dvojnásobek ceny.

## <a name="step-1-displaying-products-in-a-content-page"></a>Krok 1: Zobrazení produktů ve stránce obsahu

Naše první je k vytvoření obsahu stránky, která zobrazuje seznam produktů z databáze Northwind. (Databázi Northwind k projektu jsme přidali v předchozím kurzu [ *interakce stránky obsahu se stránkou předlohy*](interacting-with-the-master-page-from-the-content-page-vb.md).) Začněte tím, že přidání nové stránky ASP.NET do `~/Admin` složku s názvem `Products.aspx`a vytvořte mu vazbu k `Site.master` stránky předlohy. Obrázek 1 ukazuje Průzkumník řešení po přidání této stránce na webu.


[![Přidejte novou stránku ASP.NET ke složce Admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Obrázek 01**: přidejte novou stránku ASP.NET `Admin` složky ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))


Připomínáme, že [ *zadáním názvu, metaznaček a ostatní hlaviček HTML na stránce předlohy* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) kurzu jsme vytvořili vlastní stránku základní třídu s názvem `BasePage` název stránky, který generuje, pokud není Nastavte explicitně. Přejděte `Products.aspx` kódu stránky třídy a nechat ji odvodit z `BasePage` (místo z `System.Web.UI.Page`).

Nakonec aktualizujte `Web.sitemap` soubor zahrnout položku pro tento účel. Přidejte následující kód pod `<siteMapNode>` pro obsah, který se interakce stránky předlohy lekce:


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

Přidání tohoto `<siteMapNode>` element se projeví v lekcí seznamu (viz obrázek 5).

Vraťte se na `Products.aspx`. V ovládacím prvku obsahu pro `MainContent`, přidejte ovládací prvek GridView a pojmenujte ho `ProductsGrid`. Svázání prvku GridView. nový ovládací prvek SqlDataSource s názvem `ProductsDataSource`.


[![Nový ovládací prvek SqlDataSource svázání prvku GridView.](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Obrázek 02**: nový ovládací prvek SqlDataSource svázání prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))


Nakonfigurujte průvodce tak, aby používal databázi Northwind. Pokud jste pracovali kroky v předchozím kurzu, pak byste už měli mít připojovací řetězec s názvem `NorthwindConnectionString` v `Web.config`. Z rozevíracího seznamu zvolte tento připojovací řetězec, jak je znázorněno na obrázku 3.


[![Konfigurace ve třídě SqlDataSource používat databázi Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Obrázek 03**: konfigurace ve třídě SqlDataSource k použití databáze Northwind ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))


Dále určete ovládací prvek zdroje dat `SELECT` příkaz tabulky produktů výběrem z rozevíracího seznamu a vrací `ProductName` a `UnitPrice` sloupce (viz obrázek 4). Klikněte na tlačítko Další a pak dokončete průvodce Konfigurace zdroje dat dokončit.


[![Vrátí pole ProductName a UnitPrice z tabulky produktů](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Obrázek 04**: vrátit `ProductName` a `UnitPrice` pole z `Products` tabulky ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))


To je všechno je to! Visual Studio po dokončení průvodce přidá do ovládacího prvku GridView zrcadlící dvě pole vrácené ovládacím prvkem SqlDataSource dvě BoundFields. Následující značky prvku GridView a SqlDataSource řízení. Obrázek 5 ukazuje výsledky při prohlížení prostřednictvím prohlížeče.


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


[![Každý produkt a cena je uveden v prvku GridView.](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Obrázek 05**: každý produkt a cena je uveden v prvku GridView ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))


> [!NOTE]
> Nebojte se vyčistit vzhled prvku GridView. Některé návrhy zahrnovat formátování zobrazená hodnota UnitPrice jako měnu a použití písma a barvy pozadí ke zlepšení vzhledu mřížky. Další informace o zobrazení a formátování dat v technologii ASP.NET, najdete v mé [práce s Data sérii](../../data-access/index.md).


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Krok 2: Přidání tlačítka Double ceny na stránku předlohy

Naše dalším krokem je přidání ovládacího prvku tlačítko webového k hlavní stránky, která po kliknutí na bude double price všechny produkty v databázi. Otevřít `Site.master` stránku předlohy a přetáhněte z panelu nástrojů na Návrhář, uvedení ho pod tlačítko `RecentProductsDataSource` SqlDataSource řízení jsme přidali v předchozím kurzu. Tlačítka nastavte `ID` vlastnost `DoublePrice` a jeho `Text` vlastnost "Double ceny produktů".

Dále přidejte ovládacím prvkem SqlDataSource na hlavní stránku pojmenování `DoublePricesDataSource`. Tato SqlDataSource se použije ke spuštění `UPDATE` příkaz na dvojnásobek všechny ceny. Konkrétně, musíme nastavit jeho `ConnectionString` a `UpdateCommand` vlastnosti na příslušný připojovací řetězec a `UPDATE` příkazu. Pak potřebujeme k volání tohoto ovládacího prvku SqlDataSource `Update` metoda při `DoublePrice` po kliknutí na tlačítko. Chcete-li nastavit `ConnectionString` a `UpdateCommand` vlastnosti, vyberte ovládacím prvkem SqlDataSource a potom přejděte do okna Vlastnosti. `ConnectionString` Seznamů vlastností těchto připojovací řetězce, které už jsou uložené ve `Web.config` v rozevíracím seznamu, zvolte `NorthwindConnectionString` možnosti, jak je znázorněno na obrázku 6.


[![Konfigurace ve třídě SqlDataSource používat NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Obrázek 06**: konfigurace ve třídě SqlDataSource k použití `NorthwindConnectionString` ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))


Chcete-li nastavit `UpdateCommand` vlastnost, vyhledejte možnost UpdateQuery v okně Vlastnosti. Tato vlastnost, pokud je vybráno, zobrazí tlačítko se třemi tečkami; Kliknutím na toto tlačítko zobrazit dialogové okno Editor příkazů a parametrů je znázorněno na obrázku 7. Zadejte následující příkaz `UPDATE` příkaz do textového pole v dialogovém okně:


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Tento příkaz při spuštění bude dvakrát `UnitPrice` hodnotu pro každý záznam v `Products` tabulky.


[![Nastavte vlastnost UpdateCommand SqlDataSource.](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Obrázek 07**: nastavte SqlDataSource `UpdateCommand` vlastnosti ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))


Po nastavení těchto vlastností, vaše tlačítko a SqlDataSource ovládacích prvků deklarativní by měl vypadat nějak takto:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Už jen zbývá volat jeho `Update` metoda při `DoublePrice` po kliknutí na tlačítko. Vytvoření `Click` obslužné rutiny události pro `DoublePrice` tlačítko a přidejte následující kód:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Pokud chcete vyzkoušet tuto funkci, přejděte `~/Admin/Products.aspx` stránku jsme vytvořili v kroku 1 a klikněte na tlačítko "Double ceny produktů". Kliknutím na tlačítko vyvolá zpětné volání a spustí `DoublePrice` tlačítka `Click` obslužná rutina události, možnost zdvojnásobení ceny pro všechny produkty. Na stránce se pak znovu vykreslí a je kód vrátí a znovu zobrazí v prohlížeči. Na stránce obsahu prvku GridView však vypíše stejné ceny před "Cen Double produktu" došlo ke kliknutí na tlačítko. Je to proto, že data původně načten v prvku GridView měl stavu uložená v zobrazení stavu, takže není zatížit na zpětná volání, pokud nedostanete. Pokud navštívíte na jinou stránku a pak se vraťte k `~/Admin/Products.aspx` stránky uvidíte aktualizované ceny.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Krok 3: Vyvolání události při the ceny jsou zdvojnásobí.

Protože v prvku GridView `~/Admin/Products.aspx` stránky neprojeví okamžitě zdvojnásobení cena, uživatel může understandably myslíte, že jejich není klepněte na tlačítko "Double cen produktu" nebo že nefungovalo. Možná snaží kliknutím na tlačítko několik dalších krát, znovu a znovu zdvojnásobení ceny. Chcete-li vyřešit to budeme muset mít mřížky v obsahu zobrazení stránky nové ceny ihned poté, co se zdvojnásobí.

Jak je popsáno dříve v tomto kurzu, potřebujeme k vyvolání události na hlavní stránce pokaždé, když uživatel klikne `DoublePrice` tlačítko. Událost je způsob pro jednu třídu (vydavatel události), jak upozornit jiné sady jiné třídy (odběratelů událostí), které něco zajímavého došlo k chybě. V tomto příkladu je na hlavní stránce zdroj události; ty obsahu stránky, které při záleží `DoublePrice` po kliknutí na tlačítko jsou odběratele.

Třída přihlásí se k odběru události tak, že vytvoříte *obslužná rutina události*, což je metoda, který se spouští v reakci na vyvolanou událost. Vydavatel definuje události, která vyvolává definováním *delegát události*. Delegát události Určuje, jaké vstupní parametry obslužné rutiny události musí přijmout. V rozhraní .NET Framework – delegáti událostí vracet žádnou hodnotu a přijímají dva vstupní parametry:

- `Object`, Který identifikuje zdroj události a
- Třídy odvozené od `System.EventArgs`

Druhý parametr předaný obslužné rutiny události může obsahovat další informace o události. Zatímco základní `EventArgs` třídy nepředává podél veškeré informace, rozhraní .NET Framework obsahuje několik tříd, které rozšiřují `EventArgs` a zahrnovat další vlastnosti. Například `CommandEventArgs` instance předána obslužné rutiny událostí, které odpovídají `Command` události a obsahuje dvě vlastnosti, informační: `CommandArgument` a `CommandName`.

> [!NOTE]
> Další informace o vytváření vyvolávání a zpracování událostí, naleznete v tématu [události a delegáti](https://msdn.microsoft.com/library/17sde2xt.aspx) a [– delegáti událostí v jednoduché anglické](http://www.codeproject.com/KB/cs/eventdelegates.aspx).


Chcete-li definovat událost, použijte následující syntaxi:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Vzhledem k tomu, že musíme upozornit stránky obsahu, že uživatel klikl `DoublePrice` tlačítko a není potřeba předávají jakýchkoli dalších informací, můžeme použít delegát události `EventHandler`, která definuje obslužnou rutinu události, která přijímá jako jeho sekunda Parametr objektu typu `System.EventArgs`. Vytvořit událost na stránce předlohy, přidejte následující řádek kódu do třídy modelu code-behind na hlavní stránce:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

Výše uvedený kód přidá veřejná událost na hlavní stránku s názvem `PricesDoubled`. Nyní potřebujeme vyvolat tuto událost po ceny být dvojitá. Pro vyvolání události použijte následující syntaxi:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Kde *odesílatele* a *eventArgs* jsou hodnoty, které chcete předat do obslužné rutiny události odběratele.

Aktualizace `DoublePrice` `Click` obslužné rutiny události s následujícím kódem:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Stejně jako předtím `Click` obslužné rutiny události spustí zavoláním `DoublePricesDataSource` SqlDataSource ovládacího prvku `Update` metoda na dvojnásobek ceny pro všechny produkty. Následující, že jsou dva přidání obslužné rutiny události. Nejprve je potřeba `RecentProducts` aktualizaci dat prvku GridView. Tento prvek GridView byl přidán do stránky předlohy v předchozím kurzu a zobrazí pěti nejvíce nedávno přidaných produktech. Potřebujeme aktualizovat tuto mřížku tak, aby se zobrazuje jenom dvojitá ceny za těchto pět produktů. Pod `PricesDoubled` událost se vyvolá. Odkaz na samotné stránky předlohy (`Me`) se odešle do obslužné rutiny události jako zdroj události a prázdnou `EventArgs` objektu se odešle jako argumenty události.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Krok 4: Zpracování událostí v obsahu stránky

V tuto chvíli na hlavní stránce vyvolá jeho `PricesDoubled` událost pokaždé, když `DoublePrice` dojde ke kliknutí na ovládací prvek tlačítko. Ale to je jen půlka zocelenou – musíme zpracování událostí v odběrateli. To zahrnuje dva kroky: vytváření obslužné rutiny události a přidání události její kód tak, aby při událost je vyvolána obslužná rutina události je proveden.

Začněte vytvořením obslužné rutiny události s názvem `Master_PricesDoubled`. Z důvodu jak jsme definovali `PricesDoubled` událostí na hlavní stránce dva vstupní parametry obslužné rutiny události musí být typy `Object` a `EventArgs`v uvedeném pořadí. Ve volání obslužné rutiny události `ProductsGrid` prvku GridView `DataBind` metoda znovu připojit data do mřížky.


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

Hotový kód pro obslužnou rutinu události, ale ještě jsme do svážete stránky předlohy `PricesDoubled` událost pro tuto obslužnou rutinu události. Odběrateli vedení vodiče událost na obslužnou rutinu události prostřednictvím následující syntaxi:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Vydavatel* je odkaz na objekt, který nabízí události *eventName*, a *methodName* je název obslužné rutiny události definované v odběrateli.

Tento kód její události musí být provedeny na první návštěvě stránky a následné zpětného odeslání a se budou objevovat v určitém bodě životního cyklu stránky, která předchází při události mohou být vyvolány. Vhodná doba přidejte událost její kód je ve fázi PreInit dojde k velmi brzy v životní cyklus stránky.

Otevřít `~/Admin/Products.aspx` a vytvořit `Page_PreInit` obslužné rutiny události:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Aby bylo možné dokončit tento kód její potřebujeme programový odkaz na hlavní stránku ze stránky obsahu. Jak je uvedeno v předchozím kurzu, existují dva způsoby, jak to udělat:

- Pomocí přetypování volného typu `Page.Master` vlastnost typ příslušné stránky předlohy, nebo
- Přidáním `@MasterType` direktivu `.aspx` stránky a pak pomocí silných `Master` vlastnost.

Použijeme druhý přístup. Přidejte následující `@MasterType` direktiv k hornímu okraji deklarativním označení stránky:


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Přidejte následující kód její události v `Page_PreInit` obslužné rutiny události:


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

S tímto kódem na místě GridView na stránce obsahu aktualizují pokaždé, když `DoublePrice` po kliknutí na tlačítko.

Toto chování ilustrují obrázky 8 a 9. Obrázek 8 ukazuje na stránku, když první uživatel. Všimněte si, že cena hodnoty v obou `RecentProducts` ovládacího prvku GridView (v levém sloupci předlohové stránky) a `ProductsGrid` ovládacího prvku GridView (na stránce obsahu). Obrázek 9 ukazuje stejné obrazovce ihned po `DoublePrice` kliknutí na tlačítko. Jak je vidět nové ceny se okamžitě projeví v obou prvků GridViews.


[![Počáteční cena hodnoty](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Obrázek 08**: počáteční hodnoty cena ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))


[![Ceny Just-Doubled jsou zobrazeny v prvků GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Obrázek 09**: The Just-Doubled ceny jsou zobrazeny v prvků GridViews ([kliknutím ji zobrazíte obrázek v plné velikosti](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))


## <a name="summary"></a>Souhrn

V ideálním případě stránky předlohy a její obsah stránky jsou plně oddělené od sebe a vyžadovat žádná úroveň interakce. Ale pokud máte stránky předlohy nebo obsahu stránky, která zobrazuje data, která lze změnit na hlavní stránku nebo stránky obsahu, pak budete muset mít hlavní stránky upozornění stránka obsahu (nebo naopak) při změně dat tak, že je možné aktualizovat zobrazení. V předchozím kurzu jsme viděli, jak programově interagovat s jeho hlavní stránky; obsahu stránky v tomto kurzu jsme se podívali na postupy mají stránky předlohy initiate interakce.

Zatímco programové interakce mezi stránky předlohy a obsahu můžou pocházet z obsahu nebo stránky předlohy, interakce používaným závisí na vzniku. Rozdíly jsou vzhledem k tomu, že stránku obsahu má jednu stránku předlohy, ale na hlavní stránce může být mnoho různých obsahu stránek. Namísto nutnosti pracovat přímo se stránkou obsahu stránky předlohy, lepším řešením je, aby na hlavní stránce vyvolat událost, která signalizuje, že některé akce proběhla. Tyto stránky obsahu, které vám jde o akci, můžete vytvořit obslužné rutiny událostí.

Všechno nejlepší programování!

### <a name="further-reading"></a>Další čtení

Další informace o tématech, které jsou popsané v tomto kurzu najdete na následujících odkazech:

- [Přístup k a aktualizace dat v ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Události a delegáti](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Předávání informací mezi obsah a stránkami předloh](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Práce s daty v kurzy k ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor více ASP/ASP.NET knih a Zakladatel 4GuysFromRolla.com pracuje s Microsoft webových technologií od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami technologie ASP.NET 3.5 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott může být dostupný na adrese [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím na svém blogu [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Speciální k

V této sérii kurzů byl recenzován uživatelem mnoho užitečných revidující. Vedoucí kontrolor pro účely tohoto kurzu byla Suchi Banerjee. Zajímat téma Moje nadcházejících článcích MSDN? Pokud ano, vyřaďte mě řádek na [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Předchozí](interacting-with-the-master-page-from-the-content-page-vb.md)
> [další](master-pages-and-asp-net-ajax-vb.md)
