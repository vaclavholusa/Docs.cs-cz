---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Omezení dat úpravy funkce na základě uživatele (C#) | Microsoft Docs
author: rick-anderson
description: Ve webové aplikaci, která umožňuje uživatelům upravovat data může mít různé uživatelské účty různých dat úpravy oprávnění. V tomto kurzu podíváme jak t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: b056536eeaa86ef2c73debe23dd38861f41b2a69
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Omezení funkce změny dat na základě uživatele (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) nebo [stáhnout PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> Ve webové aplikaci, která umožňuje uživatelům upravovat data může mít různé uživatelské účty různých dat úpravy oprávnění. V tomto kurzu podíváme, jak dynamicky upravit funkce změny dat na základě hostujícími uživatele.


## <a name="introduction"></a>Úvod

Počet webových aplikací podporují uživatelských účtů a poskytují různé možnosti, sestavy a funkce na základě přihlášeného uživatele. Například s naše kurzy jsme chtít povolit uživatelům od dodavatele společností k přihlášení na aktualizace webu a obecné informace o jejich produktech - jejich název a množství na jednotku, případně – společně s informacemi dodavatele, jako je například název své společnosti, Adresa, kontaktní osoby s informace a tak dále. Kromě toho jsme chtít obsahovat některé uživatelské účty pro osoby z naší společnosti tak, aby se mohl přihlásit a aktualizovat informace o produktu, jako jsou jednotky na skladě, minimální počet a tak dále. Naše webové aplikace může také povolit anonymní uživatelé navštíví (lidem, kteří nejsou přihlášení), ale omezí je povolit pouze zobrazení data. S takového uživatelského účtu systému zavedené by chceme data ovládací prvky webového na našich stránkách ASP.NET a nabídnout vkládání, úpravy a odstraňování možnosti, které jsou vhodné pro aktuálně přihlášeného uživatele.

V tomto kurzu podíváme, jak dynamicky upravit funkce změny dat na základě hostujícími uživatele. Konkrétně vytvoříme stránky, které se zobrazí informace o dodavateli v upravitelné DetailsView společně s GridView, které jsou uvedeny produkty, které poskytuje dodavatel. Pokud je uživatel na stránce z naší společnosti, mohou: prohlížet jakékoli informace dodavatele s; upravit jejich adresu; a upravit informace pro některý z produktů od dodavatele. Pokud uživatel je však z určité společnosti, mohou pouze zobrazit a upravit své vlastní informace o adrese a můžete upravit pouze jejich produktů, které nebyly byla označena jako zastaveny.


[![Uživatel z naší společnosti můžete upravit všechny informace s dodavatele](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Obrázek 1**: A uživatele z naše společnosti můžete upravit libovolného dodavatele s informace ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Uživatel z konkrétní dodavatele může pouze zobrazení a úpravy své informace](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Obrázek 2**: A uživatele z konkrétní dodavatele můžete pouze zobrazení a úpravy své informace ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Umožňují s začít!

> [!NOTE]
> ASP.NET 2.0 systém členství s poskytuje standardizovaná rozšiřitelná platforma pro vytváření, správu a ověřování uživatelských účtů. Vzhledem k tomu, že prozkoumání systém členství je nad rámec tyto kurzy, v tomto kurzu místo "fakes" členství tím, že anonymní návštěvníky zvolit, jestli jsou od jiného dodavatele konkrétní nebo z naší společnosti. Další informace o členství, najdete v části Moje [zkoumání technologii ASP.NET 2.0 s členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) článek řady.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Krok 1: Umožní uživateli zadat jejich přístupová práva

Ve skutečných webové aplikaci bude zahrnovat informace o uživatelském s účtu zda šlo pro naše společnost nebo pro určité dodavatele, a tyto informace by prostřednictvím kódu programu přístupné z našich stránek ASP.NET, jakmile se uživatel přihlásil k webu. Tyto informace může zachytit pomocí technologie ASP.NET 2.0 s rolí systému, jako informace o účtu uživatele prostřednictvím profilu systému, nebo prostřednictvím některé vlastní prostředky.

Vzhledem k tomu, že cílem tohoto kurzu je ukázka, jak přizpůsobit funkce změny dat na základě přihlášeného uživatele a není určena k představením technologii ASP.NET 2.0 s členství, role a profil systémy, k určení použijeme mechanismus velmi jednoduše možnosti pro uživatele na stránce - rozevírací seznam, ze kterého můžete určit uživatele, pokud by mohli zobrazit a upravit informace o dodavateli nebo, případně co konkrétní dodavatele s informace se mohou zobrazit a upravit. Pokud uživatel uvádí, že se můžete zobrazit a upravit všechny informace dodavatele (výchozí), Jana stránky prostřednictvím všech dodavatelů, upravit všechny informace o adrese s dodavateli a upravit název a množství pro některý z produktů poskytnuté dodavatelem vybrané jednotce. Pokud uživatel označuje, že se dá jenom zobrazit a upravit konkrétního dodavatele, ale pak se můžete zobrazit podrobnosti a produktů pro tuto jednoho dodavatele a můžete pouze aktualizovat název a množství na informace o jednotce pro tyto produkty, které jsou *není* zastaveny.

Naše prvním krokem v tomto kurzu, pak je vytvořit tento rozevírací seznam a jeho naplnění dodavatelů v systému. Otevřete `UserLevelAccess.aspx` stránky v `EditInsertDelete` složky, přidejte rozevírací seznam jejichž `ID` je nastavena na `Suppliers`a vytvořte vazbu Tento rozevírací seznam s novou ObjectDataSource s názvem `AllSuppliersDataSource`.


[![Vytvořit nový ObjectDataSource s názvem AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Obrázek 3**: vytvoření nové ObjectDataSource s názvem `AllSuppliersDataSource` ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Vzhledem k tomu, že chceme, aby tento rozevírací seznam zahrnout všechny dodavatele, nakonfigurujte ObjectDataSource má být vyvolán `SuppliersBLL` třídu s `GetSuppliers()` metoda. Také zajistěte, aby ObjectDataSource s `Update()` metoda je namapována na `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda, jako tento ObjectDataSource také použije DetailsView jsme přidali v kroku 2.

Po dokončení Průvodce ObjectDataSource, proveďte kroky nakonfigurováním `Suppliers` rozevírací seznam tak, aby se zobrazí `CompanyName` pole data a používá `SupplierID` jako hodnota pro každé datové pole `ListItem`.


[![Konfigurace rozevírací seznam dodavatelé používat NázevSpolečnosti a KódDodavatele datových polí](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Obrázek 4**: konfigurace `Suppliers` rozevírací seznam pro použití `CompanyName` a `SupplierID` datových polí ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


V tomto okamžiku rozevírací seznam obsahuje seznam názvů společnosti dodavatelů v databázi. Ale musíme také obsahují možnost "Zobrazit a upravit všechna dodavatelé" k rozevírací seznam. K tomu, nastavte `Suppliers` rozevírací seznam s `AppendDataBoundItems` vlastnost `true` a poté přidejte `ListItem` jejichž `Text` vlastnost je "Zobrazit a upravit všechna dodavatelé" a jehož hodnota je `-1`. Tím přidáte přímo pomocí deklarativní nebo pomocí návrháře tak, že přejdete do okna vlastností a kliknete na tři tečky v rozevírací seznam s `Items` vlastnost.

> [!NOTE]
> Odkazovat zpět [ *a podrobností filtrování s rozevírací seznam* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurz podrobné podrobnější informace o přidání vyberte všechny položky k Vycentrovat rozevírací seznam.


Po `AppendDataBoundItems` vlastnost byla nastavena a `ListItem` přidali, rozevírací seznam s deklarativní by měl vypadat jako:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Obrázek 5 ukazuje snímek obrazovky naše aktuální průběh, při zobrazení prostřednictvím prohlížeče.


[![Dodavatelé rozevírací seznam obsahuje zobrazit všechny ListItem a jeden pro každou dodavatele](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Obrázek 5**: `Suppliers` rozevírací seznam obsahuje zobrazit všechny `ListItem`, a jeden pro každou dodavatele ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Vzhledem k tomu, že chceme ihned po uživatel změnil jejich výběr aktualizovat uživatelské rozhraní, nastavte `Suppliers` rozevírací seznam s `AutoPostBack` vlastnost `true`. V kroku 2 vytvoříme DetailsView ovládací prvek, který se zobrazí informace o dodavatel(é) na základě výběru rozevírací seznam. Potom v kroku 3, vytvoříme obslužné rutiny události pro tento rozevírací seznam s `SelectedIndexChanged` událostí, ve kterém přidáme kód, který sváže informace odpovídající dodavatele DetailsView podle vybrané dodavatele.

## <a name="step-2-adding-a-detailsview-control"></a>Krok 2: Přidání ovládacího prvku DetailsView

Umožní s nástrojem DetailsView můžete zobrazit informace o dodavateli. Pro uživatele, který můžete zobrazit a upravit všech dodavatelů bude DetailsView podporovat stránkování, které uživateli umožňují krok prostřednictvím jeden záznam informace dodavatele najednou. Pokud uživatel pracuje konkrétního dodavatele, ale DetailsView se zobrazí pouze konkrétní dodavatele s informace a nebude obsahovat rozhraní stránkování. V obou případech je potřeba povolit uživatelům upravit adresu s jiného dodavatele, města a pole země DetailsView.

Přidejte na stránku pod DetailsView `Suppliers` nastavte rozevírací seznam, jeho `ID` vlastnost `SupplierDetails`a navázat jej na `AllSuppliersDataSource` ObjectDataSource vytvořili v předchozím kroku. Dále zkontrolujte zaškrtávací políčka Povolit stránkování a povolit úpravy z DetailsView s inteligentním.

> [!NOTE]
> Pokud se tento t možnost Povolit úpravy v inteligentní DetailsView s značky ho s vzhledem k tomu, že nemapují ObjectDataSource s `Update()` metodu `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda. Přejít zpět a tuto konfiguraci změnit, po jejímž uplynutí má možnost Povolit úpravy zobrazí v DetailsView s inteligentním nastavit chvíli trvat.


Vzhledem k tomu `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda přijímá pouze čtyři parametry - `supplierID`, `address`, `city`, a `country` -upravit DetailsView s BoundFields tak, aby `CompanyName` a `Phone` BoundFields jsou jen pro čtení. Kromě toho odebrat `SupplierID` BoundField úplně. Nakonec `AllSuppliersDataSource` ObjectDataSource má aktuálně jeho `OldValuesParameterFormatString` vlastnost nastavena na hodnotu `original_{0}`. Za chvíli odebrat nastavení této vlastnosti z úplně deklarativní syntaxe, nebo ji nastavte na výchozí hodnotu `{0}`.

Po dokončení konfigurace `SupplierDetails` DetailsView a `AllSuppliersDataSource` ObjectDataSource, budeme mít následující kód:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

V tomto okamžiku můžete prostřednictvím stránkování DetailsView a aktualizovat informace o adrese vybrané dodavatele s, bez ohledu na výběru provedeném `Suppliers` rozevírací seznam (viz obrázek 6).


[![Žádné informace o dodavateli lze zobrazit a aktualizovat svou adresu](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Obrázek 6**: Libovolná dodavatelé informace lze zobrazit a aktualizovat její adresu ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Krok 3: Zobrazení jenom vybraných dodavatele s informací o

Naší stránce s aktuálně zobrazí informace o všech dodavatelů, bez ohledu na to, zda byl vybrán konkrétní dodavatele z `Suppliers` rozevírací seznam. Aby bylo možné zobrazit pouze informace dodavatele pro vybrané dodavatele je potřeba přidat další ObjectDataSource na naší stránce, který načte informace o konkrétní dodavatele.

Přidat nové ObjectDataSource na stránku, pojmenování ho `SingleSupplierDataSource`. Z inteligentních značek, klikněte na odkaz Konfigurace zdroje dat a mějte ho používat `SuppliersBLL` třídu s `GetSupplierBySupplierID(supplierID)` metoda. Stejně jako u `AllSuppliersDataSource` ObjectDataSource, mají `SingleSupplierDataSource` ObjectDataSource s `Update()` metoda namapované na `SuppliersBLL` třídu s `UpdateSupplierAddress` metoda.


[![Konfigurace SingleSupplierDataSource ObjectDataSource lze pomocí této metody GetSupplierBySupplierID(supplierID)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Obrázek 7**: konfigurace `SingleSupplierDataSource` ObjectDataSource pro použití `GetSupplierBySupplierID(supplierID)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


V dalším kroku jsme znovu zobrazit výzva k zadání parametru Zdroj `GetSupplierBySupplierID(supplierID)` metoda s `supplierID` vstupní parametr. Vzhledem k tomu, že má být zobrazena informace pro dodavatele, vybrat z rozevírací seznam, použijte `Suppliers` rozevírací seznam s `SelectedValue` vlastnost jako zdroj parametru.


[![Použijte rozevírací seznam dodavatelé jako KódDodavatele Zdroj parametru](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Obrázek 8**: použití `Suppliers` rozevírací seznam jako `supplierID` Zdroj parametru ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


I když tento druhý ObjectDataSource přidat, je aktuálně konfigurována ovládací prvek DetailsView vždy nutné použít `AllSuppliersDataSource` ObjectDataSource. Je potřeba přidat logiku upravit zdroj dat používaný v DetailsView v závislosti na tom `Suppliers` vybrána položka rozevírací seznam. Chcete-li dosáhnout, vytvořte `SelectedIndexChanged` obslužné rutiny události pro rozevírací seznam dodavatelů. To můžete snadno vytvořit dvojitým kliknutím na rozevírací seznam v návrháři. Této obslužné rutiny události musí určit, jaké zdroje dat použít a musí rebind data, která mají DetailsView. To se provádí následujícím kódem:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Obslužné rutiny události začíná tak, že určíte, zda jste vybrali možnost "Zobrazit a upravit všechna dodavatelé". Pokud byl, nastaví `SupplierDetails` DetailsView s `DataSourceID` k `AllSuppliersDataSource` a vrátí uživatele na první záznam v sadě dodavatelé nastavením `PageIndex` vlastnost na hodnotu 0. Pokud však uživatel vybral konkrétní dodavatele z rozevírací seznam DetailsView s `DataSourceID` je přiřazena k `SingleSuppliersDataSource`. Zdroj bez ohledu na to, jaká data se používá, `SuppliersDetails` režimu je vrátit zpátky do režimu jen pro čtení a dat je odrážejí do ovládacího prvku DetailsView voláním `SuppliersDetails` ovládacího prvku s `DataBind()` metoda.

Pomocí této obslužné rutiny události na místě ovládacího prvku DetailsView nyní zobrazuje vybrané dodavatele, pokud jste vybrali možnost "Zobrazit a upravit všechna dodavatelé", v takovém případě všechny dodavatele můžete zobrazit pomocí rozhraní stránkování. Obrázek 9 ukazuje na stránku s možností "Zobrazit a upravit všechna dodavatelé"; Všimněte si, že rozhraní stránkování existuje, které uživateli umožňují přejděte a aktualizovat všechny dodavatele. Obrázek 10 ukazuje na stránku s dodavatelem Ma Maison vybrané. Jenom informace o s Ma Maison se v takovém případě lze zobrazit a upravovat.


[![Všechny informace dodavatelé lze zobrazit a upravit](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Obrázek 9**: všechny dodavatelé informace lze zobrazit a upravovaný ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Můžete zobrazit a upravovat pouze vybrané dodavatele s informace](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Obrázek 10**: pouze s vybrané dodavatele informace mohou být Viewed a upravovaný ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> V tomto kurzu řízení rozevírací seznam a DetailsView s `EnableViewState` musí být nastavena na `true` (výchozí) protože rozevírací seznam s `SelectedIndex` a DetailsView s `DataSourceID` změny vlastností s musí být zapamatovaných napříč postback.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Krok 4: Výpis dodavatelé produkty v upravitelné GridView

S kompletní DetailsView naše dalším krokem je zahrnout upravitelné rutina GridView obsahující seznam těchto produktů poskytnutých vybrané dodavatelem. Tato rutina GridView by měl povolit úpravy jenom `ProductName` a `QuantityPerUnit` pole. Navíc pokud je uživatel na stránce od určitého dodavatele, se musí povolit pouze aktualizace těchto produktů, které jsou *není* zastaveny. K tomu je třeba nejprve přidat přetížení `ProductsBLL` třídu s `UpdateProducts` metoda, která přebírá jenom na `ProductID`, `ProductName`, a `QuantityPerUnit` pole vstupy. Jsme sunout provedl tento proces předem v mnoha kurzy, a můžete s právě podívejte se na sem kód, který má být přidán do `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

S toto přetížení vytvořili, jsme re připraveno k přidání GridView a jeho přidružené ObjectDataSource. Přidat nové GridView na stránku, nastavte jeho `ID` vlastnost `ProductsBySupplier`a nakonfigurujte ho na použití nové ObjectDataSource s názvem `ProductsBySupplierDataSource`. Vzhledem k tomu, že chceme, aby tato rutina GridView seznam těchto produktů vybrané dodavatel, `ProductsBLL` třídu s `GetProductsBySupplierID(supplierID)` metoda. Také mapovat `Update()` metoda do nového `UpdateProduct` přetížení jsme právě vytvořili.


[![Konfigurace ObjectDataSource použít přetížení UpdateProduct právě vytvořili](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Obrázek 11**: Konfigurace ObjectDataSource pro použití `UpdateProduct` přetížení právě vytvořili ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Jsme znovu zobrazí výzva k výběru Zdroj parametru `GetProductsBySupplierID(supplierID)` metoda s `supplierID` vstupní parametr. Vzhledem k tomu, že chceme zobrazit tyto produkty pro dodavatele vybraný v ovládacím prvku DetailsView, použijte `SuppliersDetails` ovládací prvek DetailsView s `SelectedValue` vlastnost jako zdroj parametru.


[![Použijte vlastnost SelectedValue s SuppliersDetails DetailsView jako zdroj parametru](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Obrázek 12**: použití `SuppliersDetails` DetailsView s `SelectedValue` vlastnost jako zdroj parametru ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Vrácením GridView, odeberte všechna pole GridView s výjimkou `ProductName`, `QuantityPerUnit`, a `Discontinued`, označování `Discontinued` Vlastnost CheckBoxField jen pro čtení. Zkontrolujte také, možnost Povolit úpravy GridView s inteligentním. Až tyto změny byly provedeny, deklarativní GridView a ObjectDataSource by měl vypadat podobně jako následující:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Stejně jako u našich předchozí ObjectDataSources tento jeden s `OldValuesParameterFormatString` je nastavena na `original_{0}`, které způsobí problémy při pokusu o aktualizaci název produktu s nebo množství na jednotku. Tato vlastnost odebrání deklarativní syntaxi zcela nebo ji nastavte na svou výchozí `{0}`.

Dokončení této konfigurace, naši stránku teď seznamy produktů, poskytuje dodavatel v prvku GridView (viz obrázek 13). Aktuálně *žádné* lze aktualizovat název produktu s nebo množství na jednotku. Musíme aktualizovat logiku stránky tak, aby tato funkce je zakázána pro – starší formáty produktů pro uživatele, kteří s konkrétní dodavatele. Jsme budete řešení tento poslední díl v kroku 5.


[![Produkty, které poskytuje dodavatel vybrané jsou zobrazeny.](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Obrázek 13**: The produkty poskytuje dodavatel vybrané zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Po přidání tohoto upravitelné GridView `Suppliers` rozevírací seznam s `SelectedIndexChanged` obslužné rutiny události musí aktualizovat na GridView vrátit do stavu jen pro čtení. Jinak hodnota pokud jiný jiného dodavatele je vybraná při uprostřed úpravy informací o produktu, odpovídající indexu v GridView pro nové dodavatele bude také možné upravovat. Chcete-li tomu zabránit, jednoduše nastavte GridView s `EditIndex` vlastnost `-1` v `SelectedIndexChanged` obslužné rutiny události.


Odvolat také, že je důležité, aby GridView být stav zobrazení s povoleno (výchozí nastavení). Pokud jste nastavili GridView s `EnableViewState` vlastnost `false`, riskujete, že náhodně odstraněním nebo úpravou zaznamenává počet uživatelů. V tématu [upozornění: souběžnosti problém s ASP.NET 2.0 GridViews nebo DetailsView/FormViews tento podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázána](http://scottonwriting.net/sowblog/posts/10054.aspx) Další informace.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Krok 5: Zakáže úpravy zastaví produkty při zobrazit a upravit všechna Dodavatelé je Nevybráno

Když `ProductsBySupplier` GridView je plně funkční, aktuálně zajišťuje příliš mnoho přístup k uživatelům, kteří jsou z konkrétní dodavatele. Podle našich obchodní pravidla takové uživatelé neměli mít možnost aktualizace – starší formáty produktů. Chcete-li vynutit, jsme můžete skrýt (nebo zakázat) na tlačítko Upravit v těchto řádcích GridView – starší formáty produkty při přístupu stránce uživatelem od jiného dodavatele.

Vytvoření obslužné rutiny události pro GridView s `RowDataBound` událostí. V této obslužné rutiny události, je potřeba určit, zda uživatel je přidružen konkrétní dodavatele, který v tomto kurzu se dá určit kontrolou dodavatelé rozevírací seznam s `SelectedValue` vlastnost – pokud ji s něco jiných než -1, pak uživatel je související s konkrétní dodavatele. Pro tyto uživatele musíme pak určit, zda je přerušeno produktu. Jsme můžete získat odkaz na skutečnou `ProductRow` instance vázaný na řádek GridView prostřednictvím `e.Row.DataItem` vlastnost, jak je popsáno v [ *zobrazení souhrn informací v GridView s zápatí* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) kurz. Pokud už není podporovaný produktu, jsme získat programový odkaz na tlačítko Upravit v GridView s CommandField pomocí technik popsaných v tomto kurzu předchozí [ *přidání Client-Side potvrzení při odstranění* ](adding-client-side-confirmation-when-deleting-cs.md). Jakmile jsme odkaz jsme potom můžete skrýt nebo na tlačítko zakázat.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

Pomocí této události obslužné rutiny na místě, pokud tyto produkty, které jsou zastaveny návštěvou tuto stránku jako uživatel od jiného dodavatele konkrétní nejsou upravitelné, jako tlačítko Upravit skryt pro tyto produkty. Například s Chef Anton Gumbo Mix je deaktivovaný produkt pro nový Orléans Cajun Delights dodavatele. Pokud na stránce pro tento konkrétní dodavatele, tlačítko Upravit pro tento produkt je skryta nebyl zřejmý (viz obrázek 14). Při návštěvě pomocí "Zobrazit a upravit všechna dodavatelů", je tlačítko Upravit však k dispozici (viz obrázek 15).


[![Pro uživatele specifické pro dodavatele je skrytý na tlačítko Upravit Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Obrázek 14**: pro specifické pro dodavatele uživatele je skrytý na tlačítko Upravit Chef Anton s Gumbo Mix ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Pro všechny uživatele dodavatelé zobrazit a upravit se zobrazí tlačítko Upravit pro Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Obrázek 15**: pro zobrazit či upravit všichni dodavatelé uživatelé, na tlačítko Upravit Chef Anton s Gumbo Mix se zobrazí ([Kliknutím zobrazit obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Kontrola přístupová práva v vrstvu obchodní logiky

V tomto kurzu ASP.NET zpracovává stránky veškerou logiku s ohledem na informace, co uživatel uvidí a jaké produkty, si můžete aktualizovat. V ideálním případě by tato logika by být k dispozici také na vrstvu obchodní logiky. Například `SuppliersBLL` třídu s `GetSuppliers()` (která vrátí všechny dodavatelé) může zahrnovat kontrola zkontrolujte, zda je aktuálně přihlášený uživatel *není* spojené s konkrétní dodavatele. Podobně `UpdateSupplierAddress` metoda můžou zahrnovat kontrolu zajistit, aby aktuálně přihlášeného uživatele buď práce na incidentu pro naše společnost (a proto můžete aktualizovat všechny informace o adrese dodavatelé) nebo je přidružen dodavatele, jejichž data se právě aktualizuje.

Tyto vrstvy BLL kontroly nemělo být vzhledem k tomu, že v našem kurzu, uživatel s právy vyplývají z rozevírací seznam na stránce, které třídy BLL nemůže získat přístup. Pokud používáte systém členství nebo jeden z schémat ověřování na více systémů v poli poskytované ASP.NET (například ověřování systému Windows), aktuálně přihlášeného uživatele s údaje o role a je přístupný z BLL, a díky takový přístup práva kontroluje možné na prezentační a BLL vrstvy.

## <a name="summary"></a>Souhrn

Většina serverů, které poskytují uživatelské účty se muset přizpůsobit rozhraní změny dat na základě přihlášeného uživatele. Správci můžou odstranit a upravit všechny záznam, zatímco uživatelé bez oprávnění správce může být omezena pouze aktualizace nebo odstranění záznamů vytvářely sami. Bez ohledu scénáři může být, ovládací prvky webového, ObjectDataSource, data a vrstvu obchodní logiky třídy lze rozšířit pro přidání nebo odepřít určité funkce na základě přihlášeného uživatele. V tomto kurzu jsme viděli, jak omezit lze zobrazit a upravovat data v závislosti na tom, jestli je uživatel spojené s konkrétní dodavatele nebo pokud to šlo pro naše společnost.

V tomto kurzu se ukončí součást vložení, aktualizace a odstranění dat pomocí ovládacích prvků GridView DetailsView a FormView. Od verze v dalším kurzu, jsme budete zapněte naše pozornost přidání stránkování a řazení podpory.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](adding-client-side-confirmation-when-deleting-cs.md)
> [další](an-overview-of-inserting-updating-and-deleting-data-vb.md)
