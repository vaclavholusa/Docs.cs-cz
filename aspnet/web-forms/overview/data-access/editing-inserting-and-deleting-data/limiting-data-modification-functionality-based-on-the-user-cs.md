---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
title: Omezení funkcí pro úpravu dat podle uživatele (C#) | Dokumentace Microsoftu
author: rick-anderson
description: Ve webové aplikaci, která umožňuje uživatelům upravovat data může mít různé uživatelské účty jiné úpravy dat oprávnění. V tomto kurzu prozkoumáme jak t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 2b251c82-77cf-4e36-baa9-b648eddaa394
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-cs
msc.type: authoredcontent
ms.openlocfilehash: d8141a47bc7036641a93a0946b43e1f8086b9a93
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372238"
---
<a name="limiting-data-modification-functionality-based-on-the-user-c"></a>Omezení funkcí pro úpravu dat podle uživatele (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_23_CS.exe) nebo [stahovat PDF](limiting-data-modification-functionality-based-on-the-user-cs/_static/datatutorial23cs1.pdf)

> Ve webové aplikaci, která umožňuje uživatelům upravovat data může mít různé uživatelské účty jiné úpravy dat oprávnění. V tomto kurzu prozkoumáme jak dynamicky upravit možnosti úprav dat na základě hostujícími uživatele.


## <a name="introduction"></a>Úvod

Počet webových aplikací podpora uživatelských účtů a poskytují různé možnosti, sestavy a funkce na základě přihlášeného uživatele. Například prostřednictvím našich kurzů jsme chtít umožnit uživatelům od dodavatele společností k přihlášení k webu a aktualizace obecné informace o svých produktech - své jméno a množství na jednotku, možná – společně s informacemi dodavatelů, jako je například název své společnosti Adresa, kontaktní osoby s informace a tak dále. Kromě toho budeme chtít zahrnout některé uživatelské účty pro lidi z naší společnosti tak, aby se mohl přihlásit a aktualizovat informace o produktu jako minimální počet jednotek na skladě a tak dále. Naši webovou aplikaci může také umožnit anonymní uživatele na web (uživatelů, kteří nejste přihlášeni), ale by omezit je jen zobrazení data. S takového uživatelského účtu systému na místě jsme byste měli data webové ovládací prvky na našich stránkách ASP.NET nabízí vložení, úpravy a odstranění funkce vhodné pro aktuálně přihlášeného uživatele.

V tomto kurzu prozkoumáme jak dynamicky upravit možnosti úprav dat na základě hostujícími uživatele. Zejména vytvoříme stránku, která zobrazuje informace o dodavateli v upravitelné DetailsView spolu s prvku GridView, která zobrazuje seznam produktů, poskytnutých dodavatelem. Pokud je uživatel na stránce naší společnosti, mohou: prohlížet jakékoli informace s dodavatelem; upravit jejich adresy; a upravovat informace pro některý z produktů poskytnutých dodavatelem. Pokud však uživatel z určité společnosti, můžou jenom zobrazit a upravit své vlastní informace o adrese a lze upravit pouze jejich produkty, které ještě nebyly označeny jako ukončena.


[![Uživatel z naše společnost, která může upravovat všechny informace s dodavatelem](limiting-data-modification-functionality-based-on-the-user-cs/_static/image2.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image1.png)

**Obrázek 1**: uživatele z náš společnosti můžete upravit libovolného dodavatele s informace ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image3.png))


[![Uživatel z konkrétního dodavatele může pouze zobrazení a upravit jejich informace](limiting-data-modification-functionality-based-on-the-user-cs/_static/image5.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image4.png)

**Obrázek 2**: uživatele z konkrétní dodavatele můžete jenom zobrazit a upravit informace o jejich ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image6.png))


Začínáme s let!

> [!NOTE]
> ASP.NET 2.0 systém členství s poskytuje standardizovanou a rozšiřitelnou platformu pro tvorbu, správu a probíhá ověřování uživatelských účtů. Prozkoumání systému členství je nad rámec těchto kurzů, v tomto kurzu místo "napodobenin" členství tím, že anonymní uživatelé zvolit, jestli jsou od určitého dodavatele nebo z naše společnost, která. Další informace o členství, najdete v části Moje [zkoumání ASP.NET 2.0 s členství, role a profil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx) série článků.


## <a name="step-1-allowing-the-user-to-specify-their-access-rights"></a>Krok 1: Umožňuje uživateli zadat jejich přístupová práva

Ve skutečných webové aplikaci bude zahrnovat informace o účtu uživatele s, jestli je to šlo pro naše společnost, nebo pro konkrétní dodavatele a tyto informace by programově přístupných z našich stránek ASP.NET, jakmile se uživatel přihlásil k webu. Tyto informace můžou být zachytávané ASP.NET 2.0 s rolí systému jako informace o účtu uživatele prostřednictvím profilu systému, nebo prostřednictvím některé vlastní prostředky.

Protože cílem tohoto kurzu je předvést úprava možnosti Změna dat na základě přihlášeného uživatele a není určena k předvedení technologie ASP.NET 2.0 s členství, role a systémů profilu, použijeme mechanismus velmi jednoduše určit možnosti pro uživatele na stránce - DropDownList, ze kterého uživatel může zvolit pokud by měl být schopen zobrazit a upravit všechny informace o dodavateli nebo, případně co konkrétního dodavatele s informace můžete zobrazit a upravit. Pokud uživatel označuje, že může zobrazit a upravovat všechny informace dodavateli (výchozí), Jana stránkovat všichni dodavatelé, upravovat všechny informace o adrese s dodavateli a upravit název a množství na jednotku pro některý z poskytnutých dodavatelem vybraných produktů. Pokud uživatel označuje, že si můžete jenom zobrazit a upravit konkrétního dodavatele, ale pak si můžete podívat na podrobnosti a produktů pro jednoho dodavatele a můžete pouze aktualizovat název a množství na informace o jednotce pro tyto produkty, které jsou *není* ukončena.

Naši první krok v tomto kurzu je pak k vytvoření této DropDownList a přidejte do ní dodavatelů v systému. Otevřít `UserLevelAccess.aspx` stránku `EditInsertDelete` složky, přidejte DropDownList jehož `ID` je nastavena na `Suppliers`a tento DropDownList svázat nového prvku ObjectDataSource s názvem `AllSuppliersDataSource`.


[![Vytvoření nového prvku ObjectDataSource s názvem AllSuppliersDataSource](limiting-data-modification-functionality-based-on-the-user-cs/_static/image8.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image7.png)

**Obrázek 3**: vytvoření nového prvku ObjectDataSource s názvem `AllSuppliersDataSource` ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image9.png))


Protože chceme, aby tento DropDownList mají zahrnout všichni dodavatelé, nakonfigurujte prvek ObjectDataSource, který má být vyvolán `SuppliersBLL` třída s `GetSuppliers()` metody. Také zajistěte, aby ObjectDataSource s `Update()` metoda je namapována na `SuppliersBLL` třída s `UpdateSupplierAddress` metody, jako tento prvek ObjectDataSource se taky použije v ovládacím prvku DetailsView. budeme přidávat v kroku 2.

Po dokončení Průvodce prvek ObjectDataSource, proveďte kroky tím, že nakonfigurujete `Suppliers` DropDownList tak, že ukazuje `CompanyName` pole data a použije `SupplierID` jako hodnota pro každé datové pole `ListItem`.


[![Konfigurace DropDownList dodavatelé CompanyName a KódDodavatele datových polí](limiting-data-modification-functionality-based-on-the-user-cs/_static/image11.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image10.png)

**Obrázek 4**: konfigurace `Suppliers` DropDownList k použití `CompanyName` a `SupplierID` datových polí ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image12.png))


V tomto okamžiku DropDownList uvádí názvy společností, dodavatelů v databázi. Však musíme také možnost "Zobrazit nebo upravit všichni dodavatelé" DropDownList. Chcete-li to provést, nastavte `Suppliers` DropDownList s `AppendDataBoundItems` vlastnost `true` a potom přidat `ListItem` jehož `Text` je vlastnost "Zobrazit nebo upravit všichni dodavatelé" a jehož hodnota je `-1`. To lze přidat přímo pomocí deklarativní, nebo prostřednictvím Návrháře v okně Vlastnosti a kliknutím na symbol tří teček v DropDownList s `Items` vlastnost.

> [!NOTE]
> Vraťte se do [ *filtrování záznamů Master/Detail s DropDownList* ](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) kurz podrobnější informace o přidání datové vazby DropDownList vyberte všechny položky.


Po `AppendDataBoundItems` vlastnost byla nastavena a `ListItem` přidali, DropDownList s deklarativní by měl vypadat:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample1.aspx)]

Obrázek 5 ukazuje snímek obrazovky naše aktuální průběh, když zobrazit pomocí prohlížeče.


[![Dodavatelé DropDownList obsahuje zobrazit všechny ListItem Plus jeden pro každý dodavatele](limiting-data-modification-functionality-based-on-the-user-cs/_static/image14.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image13.png)

**Obrázek 5**: `Suppliers` DropDownList obsahuje zobrazit všechny `ListItem`, Plus jeden pro každý dodavatele ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image15.png))


Protože chceme aktualizovat uživatelské rozhraní ihned po uživateli došlo ke změně jejich výběr, nastavte `Suppliers` DropDownList s `AutoPostBack` vlastnost `true`. V kroku 2 vytvoříme, který se zobrazí informace o dodavatel(é) na základě výběru DropDownList ovládacího prvku DetailsView. Pak v kroku 3, vytvoříme obslužnou rutinu události pro tento DropDownList s `SelectedIndexChanged` událostí, ve kterém přidáme kód, který váže odpovídající informace k ovládacím prvku DetailsView. na základě vybraného poskytovatele.

## <a name="step-2-adding-a-detailsview-control"></a>Krok 2: Přidání ovládacího prvku DetailsView.

Umožní s pomocí DetailsView můžete zobrazit informace o dodavateli. Pro uživatele, který můžete zobrazit a upravit všechny dodavatelů bude ovládacím prvku DetailsView podporovat stránkování, které uživateli umožňují krokovat jeden záznam dodavatele informace najednou. Pokud uživatel pracuje pro konkrétní dodavatele, ale ovládacím prvku DetailsView zobrazí informace s pouze konkrétní dodavatele a nebudou obsahovat rozhraní stránkování. V obou případech musí ovládacím prvku DetailsView. aby uživatel mohl upravit dodavatele adresa, Město a zemi pole.

Přidat na stránku pod DetailsView `Suppliers` DropDownList, nastavte jeho `ID` vlastnost `SupplierDetails`a vytvořte mu vazbu k `AllSuppliersDataSource` ObjectDataSource vytvořili v předchozím kroku. V dalším kroku zaškrtněte zaškrtávací políčka Povolit stránkování a povolit úpravy z inteligentních značek s prvku DetailsView.

> [!NOTE]
> Pokud se zobrazí možnost Povolit úpravy v prvku DetailsView s inteligentní don t označte ji s protože nemapují ObjectDataSource s `Update()` metodu `SuppliersBLL` třída s `UpdateSupplierAddress` metody. Přejít zpět a provést tuto konfiguraci změnit, po jejímž uplynutí možnost Povolit úpravy by se zobrazit v prvku DetailsView s inteligentním chvíli trvat.


Protože `SuppliersBLL` třída s `UpdateSupplierAddress` metoda přijímá pouze čtyři parametry - `supplierID`, `address`, `city`, a `country` – upravte prvek DetailsView s BoundFields tak, aby `CompanyName` a `Phone` BoundFields jsou jen pro čtení. Kromě toho odebrat `SupplierID` zcela Vlastnost BoundField. Nakonec `AllSuppliersDataSource` ObjectDataSource má v současné době jeho `OldValuesParameterFormatString` nastavenou na `original_{0}`. Za chvíli odebrat nastavení této vlastnosti z deklarativní syntaxe zcela nebo ji nastavte na výchozí hodnotu `{0}`.

Po dokončení konfigurace `SupplierDetails` DetailsView a `AllSuppliersDataSource` prvek ObjectDataSource, budeme mít následující kód:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample2.aspx)]

V tomto okamžiku můžete ovládacím prvku DetailsView stránkování prostřednictvím a je možné aktualizovat informace o adrese vybrané dodavatele s, bez ohledu na výběr provedený v `Suppliers` DropDownList (viz obrázek 6).


[![Veškeré informace, Dodavatelé mohou zobrazit a aktualizovat jeho adresy](limiting-data-modification-functionality-based-on-the-user-cs/_static/image17.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image16.png)

**Obrázek 6**: Any dodavatelé informace můžete zobrazit a aktualizovat jeho adresa ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image18.png))


## <a name="step-3-displaying-only-the-selected-supplier-s-information"></a>Krok 3: Zobrazení pouze informace s vybranou dodavatele

Naši stránku aktuálně zobrazuje informace pro všechny dodavatele bez ohledu na to, zda byl vybrán konkrétního dodavatele z `Suppliers` DropDownList. Aby bylo možné zobrazit pouze informace dodavatel pro vybrané dodavatele potřebujeme přidat jiného prvku ObjectDataSource na naši stránku, ten, který načte informace o konkrétní dodavatele.

Přidat nový prvek ObjectDataSource na stránku jeho pojmenování `SingleSupplierDataSource`. Z inteligentních značek, klikněte na odkaz Konfigurovat zdroj dat a jeho použití `SuppliersBLL` třída s `GetSupplierBySupplierID(supplierID)` metody. Stejně jako u `AllSuppliersDataSource` prvek ObjectDataSource, mají `SingleSupplierDataSource` ObjectDataSource s `Update()` metodu mapované na `SuppliersBLL` třída s `UpdateSupplierAddress` metoda.


[![Konfigurace SingleSupplierDataSource ObjectDataSource GetSupplierBySupplierID(supplierID) metody](limiting-data-modification-functionality-based-on-the-user-cs/_static/image20.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image19.png)

**Obrázek 7**: konfigurace `SingleSupplierDataSource` ObjectDataSource k použití `GetSupplierBySupplierID(supplierID)` – metoda ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image21.png))


Dále jsme znovu zobrazit výzva k zadání parametru Zdroj `GetSupplierBySupplierID(supplierID)` metody s `supplierID` vstupního parametru. Vzhledem k tomu, že má být zobrazena informace pro dodavatele vybrali DropDownList, použijte `Suppliers` DropDownList s `SelectedValue` vlastnost jako zdroj parametru.


[![Použít DropDownList dodavatelé jako KódDodavatele zdroji parametru](limiting-data-modification-functionality-based-on-the-user-cs/_static/image23.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image22.png)

**Obrázek 8**: použijte `Suppliers` DropDownList jako `supplierID` zdroji parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image24.png))


I přes tento druhý ObjectDataSource přidali ovládací prvek DetailsView je aktuálně nakonfigurován pro vždy používat `AllSuppliersDataSource` ObjectDataSource. Je potřeba přidat logiku pro zdroj dat používaný v ovládacím prvku DetailsView. v závislosti na nastavení `Suppliers` DropDownList položka vybrána. Chcete-li to provést, vytvořte `SelectedIndexChanged` obslužné rutiny události pro DropDownList dodavatelů. To lze nejsnadněji vytvořit dvojitým kliknutím rozevírací seznam v návrháři. Tato obslužná rutina události musí určit, jaký zdroj dat použít a musíte znovu připojit data do ovládacího prvku DetailsView. To lze provést následujícím kódem:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample3.cs)]

Obslužná rutina události začíná tak, že určíte, zda jste vybrali možnost "Zobrazit nebo upravit všichni dodavatelé". Pokud ano, nastaví `SupplierDetails` prvek DetailsView s `DataSourceID` k `AllSuppliersDataSource` a vrátí uživatele na první záznam v sadě dodavatelů tím, že nastavíte `PageIndex` vlastnost na hodnotu 0. Pokud však uživatel vybral konkrétní dodavatele z DropDownList DetailsView s `DataSourceID` přiřazen `SingleSuppliersDataSource`. Bez ohledu na to, jaká data se používá zdroj, `SuppliersDetails` režimu je vrátit zpět do režimu jen pro čtení a data se znovu připojeno do ovládacího prvku DetailsView voláním `SuppliersDetails` ovládacího prvku s `DataBind()` metody.

Pomocí této obslužné rutiny události na místě ovládacím prvku DetailsView nyní zobrazuje vybrané dodavatele, pokud jste vybrali možnost "Zobrazit nebo upravit všichni dodavatelé", v takovém případě všechny dodavatele lze zobrazit pomocí rozhraní stránkování. Obrázek 9 ukazuje na stránku s možností "Zobrazit nebo upravit všichni dodavatelé"; Mějte na paměti, že rozhraní stránkování je k dispozici, uživatel navštívit a aktualizovat libovolnému dodavateli. Obrázek 10 ukazuje na stránku s dodavatelem Ma Maison vybrali. Pouze informace s Ma Maison se může zobrazit a upravovat v tomto případě.


[![Všechny informace dodavatelé mohou zobrazit a upravit](limiting-data-modification-functionality-based-on-the-user-cs/_static/image26.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image25.png)

**Obrázek 9**: všechny dodavatelé informace můžete zobrazit a upravovaný ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image27.png))


[![Pouze vybrané dodavatele s informace můžete zobrazit a upravit](limiting-data-modification-functionality-based-on-the-user-cs/_static/image29.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image28.png)

**Obrázek 10**: jenom vybrané dodavatele s informace můžou být Viewed a upravovaný ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image30.png))


> [!NOTE]
> Pro účely tohoto kurzu DropDownList i DetailsView ovládací prvek s `EnableViewState` musí být nastaveno na `true` (výchozí) protože DropDownList s `SelectedIndex` a DetailsView s `DataSourceID` změny vlastností s musí uživatel zadat postbacků.


## <a name="step-4-listing-the-suppliers-products-in-an-editable-gridview"></a>Krok 4: Výpis produkty dodavatelů v upravitelné GridView

S ovládacím prvku DetailsView. dokončení naším dalším krokem je zahrnout upravitelné prvku GridView, který obsahuje seznam těchto produktů poskytovaných vybrané dodavatelem. Tento prvek GridView by měl povolit úpravy pouze `ProductName` a `QuantityPerUnit` pole. Kromě toho pokud je uživatel na stránce od konkrétního dodavatele, ji by měl povolit pouze aktualizace těchto produktů, které jsou *není* ukončena. K tomu můžeme budete muset nejdřív přidat přetížení `ProductsBLL` třída s `UpdateProducts` metodu, která přijímá jenom `ProductID`, `ProductName`, a `QuantityPerUnit` pole jako vstupy. Jsme ve předem prošli tento proces v mnoha kurzy, proto nechte s právě prohlédněte si kód, která by měla být přidána do `ProductsBLL`:


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample4.cs)]

Pomocí tohoto přetížení vytvořili, můžeme znovu připraven k přidání ovládacího prvku GridView a její přidružené ObjectDataSource. Přidání nového ovládacího prvku GridView na stránku, nastavte jeho `ID` vlastnost `ProductsBySupplier`a nakonfigurujte ho na použití nového prvku ObjectDataSource s názvem `ProductsBySupplierDataSource`. Protože chceme, aby tento GridView seznam těchto produktů vybrané dodavatelem, použijte `ProductsBLL` třída s `GetProductsBySupplierID(supplierID)` metody. Také namapovat `Update()` metoda k novému `UpdateProduct` přetížení, které jsme právě vytvořili.


[![Konfigurace ObjectDataSource použít přetížení UpdateProduct právě vytvořili](limiting-data-modification-functionality-based-on-the-user-cs/_static/image32.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image31.png)

**Obrázek 11**: Konfigurace ObjectDataSource k použití `UpdateProduct` přetížení právě vytvořili ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image33.png))


Můžeme znovu výzva k výběru parametr zdroj `GetProductsBySupplierID(supplierID)` metody s `supplierID` vstupního parametru. Protože chceme zobrazit produkty pro dodavatele vybraný v ovládacím prvku DetailsView, použijte `SuppliersDetails` ovládací prvek DetailsView s `SelectedValue` vlastnost jako zdroj parametru.


[![Použití vlastnosti SelectedValue prvku SuppliersDetails DetailsView s jako parametr zdroje](limiting-data-modification-functionality-based-on-the-user-cs/_static/image35.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image34.png)

**Obrázek 12**: použijte `SuppliersDetails` prvek DetailsView s `SelectedValue` vlastnost jako zdroj parametru ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image36.png))


Vrácení k prvku GridView, odeberte všechna pole prvku GridView s výjimkou `ProductName`, `QuantityPerUnit`, a `Discontinued`, označování `Discontinued` třídě CheckBoxField jen pro čtení. Navíc zaškrtněte možnost Povolit úpravy z inteligentních značek s ovládacího prvku GridView. Po provedení těchto změn deklarativní ovládacími prvky GridView a ObjectDataSource by měl vypadat nějak takto:


[!code-aspx[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample5.aspx)]

Stejně jako u našich předchozí ObjectDataSources tento jeden s `OldValuesParameterFormatString` je nastavena na `original_{0}`, která by mohla způsobit potíže při pokusu o aktualizaci produkt s názvem nebo množství na jednotku. Úplně odeberte tuto vlastnost z deklarativní syntaxe nebo ji nastavte na jeho výchozí `{0}`.

S touto konfigurací kompletní naši stránku nyní obsahuje produkty poskytnuté dodavatelem v prvku GridView (viz obrázek 13). Aktuálně *jakékoli* produkt s názvem nebo množství na jednotku je možné aktualizovat. Musíme aktualizovat logiku naši stránku tak, aby tato funkce je zakázaná pro odpojené produkty pro uživatele přidružené k určité dodavatele. Jsme budete řešit tento poslední část v kroku 5.


[![Jsou zobrazeny poskytnutých dodavatelem vybrané produkty](limiting-data-modification-functionality-based-on-the-user-cs/_static/image38.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image37.png)

**Obrázek 13**: The produkty poskytnuté dodavatelem vybrané zobrazují ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image39.png))


> [!NOTE]
> Přidání této upravitelné GridView `Suppliers` DropDownList s `SelectedIndexChanged` obslužné rutiny události musí aktualizovat na hodnotu prvku GridView se vrátit do stavu jen pro čtení. V opačném případě jiný dodavatel vybrali při uprostřed úpravy informace o produktech, odpovídající index v prvku GridView pro nové dodavatele bude také upravovat. Chcete-li tomu zabránit, stačí nastavit prvek GridView s `EditIndex` vlastnost `-1` v `SelectedIndexChanged` obslužné rutiny události.


Také Připomínáme, že je důležité, že GridView s zobrazení stav povoleno (výchozí chování). Pokud nastavíte GridView s `EnableViewState` vlastnost `false`, spustíte riskujete souběžných uživatelů zaznamenává neúmyslnému odstranění nebo úpravy. V tématu [upozornění: problém souběžnosti s ASP.NET 2.0 prvků GridViews/DetailsView/FormViews tohoto podpora úpravy nebo odstranění a jejichž stav zobrazení je zakázaná](http://scottonwriting.net/sowblog/posts/10054.aspx) Další informace.

## <a name="step-5-disallow-editing-for-discontinued-products-when-showedit-all-suppliers-is-not-selected"></a>Krok 5: Zakažte úpravy ukončena produkty při zobrazit nebo upravit všechny Dodavatelé je nevybraných

Zatímco `ProductsBySupplier` GridView je plně funkční, aktuálně zajišťuje příliš mnoho přístup uživatelům, kteří jsou od určitého dodavatele. Za naše obchodní pravidla nesmí tyto uživatelé nebudou moct aktualizovat odpojené produkty. Pokud to pokud chcete vynutit, jsme můžete skrýt (nebo zakázat) na tlačítko Upravit tyto řádky GridView s odpojené produkty, když na stránce se právě návštěvě uživatele od jiného dodavatele.

Vytvořte obslužnou rutinu události pro prvek GridView s `RowDataBound` událostí. V této obslužné rutiny události potřebujeme k určení, zda je uživatel přidružený konkrétního dodavatele, které pro účely tohoto kurzu, můžete určit tak, že zkontrolujete dodavatelé DropDownList s `SelectedValue` vlastnost – pokud ji s něco jiné než -1 a pak mu je spojené s konkrétní dodavatele. Pro tyto uživatele následně je potřeba určit, zda se vyřazuje produktu. Jsme získejte odkaz na skutečnou `ProductRow` instance je vázán na řádku prvku GridView prostřednictvím `e.Row.DataItem` vlastnost, jak je popsáno v [ *zobrazuje souhrnné informace v zápatí prvku GridView s* ](../custom-formatting/displaying-summary-information-in-the-gridview-s-footer-cs.md) kurz. Pokud se vyřazuje, produktu, můžeme vzít programový odkaz na tlačítku pro úpravy v prvku GridView s CommandField pomocí techniky popsané v předchozím kurzu [ *přidání Client-Side potvrzení při odstraňování* ](adding-client-side-confirmation-when-deleting-cs.md). Jakmile budeme mít odkaz na jsme pak můžete skrýt nebo zakázat tlačítko.


[!code-csharp[Main](limiting-data-modification-functionality-based-on-the-user-cs/samples/sample6.cs)]

S touto událostí obslužné rutiny na místě, pokud tyto produkty, které jsou vyřazeny navštívit tuto stránku jako uživatel od konkrétního dodavatele se nedají upravovat, na tlačítku pro úpravy je skrytý pro tyto produkty. Například s Chef Anton Gumbo Mix je ukončená produktu pro nový Orleans Cajun Delights dodavatele. Při návštěvě stránky pro tento konkrétní dodavatele, přístup se skryje tlačítko Upravit pro tento produkt (viz obrázek 14). Ale při návštěvě pomocí "Zobrazit nebo upravit všichni dodavatelé", tlačítko pro úpravy je dostupné (viz obrázek 15).


[![Pro konkrétní dodavatele uživatele tlačítko Upravit pro Chef Anton s Gumbo Mix skryté](limiting-data-modification-functionality-based-on-the-user-cs/_static/image41.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image40.png)

**Obrázek 14**: pro konkrétní dodavatele uživatele skryté tlačítko Upravit pro Chef Anton s Gumbo Mix ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image42.png))


[![Pro všechny uživatele dodavatelé zobrazit nebo upravit je zobrazeno tlačítko Upravit pro Chef Anton s Gumbo Mix](limiting-data-modification-functionality-based-on-the-user-cs/_static/image44.png)](limiting-data-modification-functionality-based-on-the-user-cs/_static/image43.png)

**Obrázek 15**: pro zobrazit nebo upravit všichni dodavatelé uživatelé, tlačítko Upravit pro Chef Anton s se zobrazí Gumbo Mix ([kliknutím ji zobrazíte obrázek v plné velikosti](limiting-data-modification-functionality-based-on-the-user-cs/_static/image45.png))


## <a name="checking-for-access-rights-in-the-business-logic-layer"></a>Kontrolují se oprávnění v vrstvy obchodní logiky

V tomto kurzu ASP.NET stránky zpracovává veškerou logiku s ohledem na jaké informace může zobrazit uživatele a jaké produkty, že můžete aktualizovat. V ideálním případě by tato logika by být k dispozici také v vrstvy obchodní logiky. Například `SuppliersBLL` třída s `GetSuppliers()` – metoda (která vrátí všechny dodavatelé) může zahrnovat kontrolu a ujistěte se, že je aktuálně přihlášený uživatel *není* spojené s konkrétní dodavatele. Podobně `UpdateSupplierAddress` metoda může zahrnovat kontrolu a ujistěte se, že aktuálně přihlášeného uživatele buď pracoval pro naše společnost, která (a tedy můžete aktualizovat všechny informace o adrese dodavatelé) nebo je spojen s od dodavatele, jejichž data se aktualizuje.

Můžu nenabízela tyto vrstvy BLL kontroly vzhledem k tomu, že v našem kurzu uživatele s právy stanovuje DropDownList na stránce, která BLL třídy nemůže získat přístup. Pokud používáte systém členství nebo jeden z pole out v ověřovací schémata poskytovaných technologií ASP.NET (například ověřování Windows), aktuálně přihlášeného uživatele s informace a informace o roli je přístupná z knihoven BLL, díky čemuž takový přístup práva zkontroluje v prezentaci a BLL vrstvy je to možné.

## <a name="summary"></a>Souhrn

Většina serverů, které poskytují uživatelské účty potřebovat k přizpůsobení rozhraní pro úpravu dat na základě přihlášeného uživatele. Administrativní uživatelé moci odstranit a upravit libovolný záznam, zatímco uživatelé bez oprávnění správce může být omezena pouze aktualizace nebo odstranění záznamů sami vytvořili. Bez ohledu scénář může být, webové ovládací prvky, prvek ObjectDataSource, dat a vrstvy obchodní logiky třídy je možné rozšířit na Přidat nebo odepřít určité funkce na základě přihlášeného uživatele. V tomto kurzu jsme viděli, jak zobrazit a upravovat data v závislosti na tom, zda byl uživatel spojené s konkrétní dodavatele nebo pokud to šlo pro naše společnost, která omezit.

V tomto kurzu končí naše zkoumání vložení, aktualizace a odstranění dat pomocí ovládacího prvku GridView, DetailsView a FormView ovládacích prvků. Spuštění k dalšímu kurzu, jsme vám zapnout pozornost na přidání stránkování a řazení podpory.

Všechno nejlepší programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a Zakladatel [4GuysFromRolla.com](http://www.4guysfromrolla.com), má práce s Microsoft webových technologiích od roku 1998. Scott funguje jako nezávislý konzultant, trainer a zapisovače. Jeho nejnovější knihy [ *Edice nakladatelství Sams naučit sami ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Může být dosáhl v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím jeho blogu, který lze nalézt v [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Předchozí](adding-client-side-confirmation-when-deleting-cs.md)
> [další](an-overview-of-inserting-updating-and-deleting-data-vb.md)
