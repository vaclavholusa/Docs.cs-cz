---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Ovládací prvky zdroje dat | Microsoft Docs
author: microsoft
description: DataGrid – ovládací prvek ASP.NET 1.x označený skvělé zlepšování v přístupu k datům ve webové aplikace. Nebyla však jako uživatelsky přívětivý, jako by byly...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: b1ac7fb62767d61c97fe00338bc0f5087f4863b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885892"
---
<a name="data-source-controls"></a>Ovládací prvky zdroje dat
====================
podle [Microsoft](https://github.com/microsoft)

> DataGrid – ovládací prvek ASP.NET 1.x označený skvělé zlepšování v přístupu k datům ve webové aplikace. Nebyla však jako uživatelsky přívětivý, jako by byly. Dosud požadované značné množství kódu získat užitečné funkcí z něj. Například je model v všechny podařilo shromáždit data access v 1.x.


DataGrid – ovládací prvek ASP.NET 1.x označený skvělé zlepšování v přístupu k datům ve webové aplikace. Nebyla však jako uživatelsky přívětivý, jako by byly. Dosud požadované značné množství kódu získat užitečné funkcí z něj. Například je model v všechny podařilo shromáždit data access v 1.x.

ASP.NET 2.0 adresy o částečně se ovládací prvky datových zdrojů. Ovládací prvky zdroje dat v technologii ASP.NET 2.0 poskytují vývojářům deklarativní model pro načítání dat, zobrazení dat a upravovat data. Účelem prvků zdroje dat je zajistit konzistentní znázornění dat na ovládací prvky vázané na data bez ohledu na zdroj těchto dat. Jádrem prvků zdroje dat v technologii ASP.NET 2.0 je abstraktní třída DataSourceControl. Třída DataSourceControl poskytuje základní implementaci rozhraní IDataSource a rozhraní IListSource pozdější z nich umožňuje přiřadit prvek zdroje dat jako zdroj dat ovládacího prvku typu vázané na data (prostřednictvím nového nastavenou vlastnost DataSourceId popsané později) a vystavit data v něm jako seznam. Každý seznam dat z ovládacího prvku zdroje dat je zpřístupněná jako objekt DataSourceView. Poskytuje přístup k zobrazení DataSourceView instancí rozhraní IDataSource. Například metoda GetViewNames vrátí ICollection, který umožňuje vytvořit výčet DataSourceViews spojené s ovládacím prvkem konkrétní datové zdroje a metoda GetView umožňuje přístup ke konkrétní instanci DataSourceView podle názvu.

Ovládací prvky zdroje dat mají žádné uživatelské rozhraní. Jsou implementovány jako server ovládací prvky tak, aby mohou podporovat deklarativní syntaxe a tak, aby v případě potřeby mají přístup k stav stránky. Ovládací prvky zdroje dat nevykreslují libovolný kód HTML pro klienta.

> [!NOTE]
> Jak uvidíte později, že jsou také ukládání do mezipaměti výhody získat pomocí ovládacích prvků zdroje dat.


## <a name="storing-connection-strings"></a>Ukládání připojovacích řetězců

Předtím, než se nám získat do vyhledávání v tom, jak nakonfigurovat ovládací prvky zdroje dat, můžeme by měly zahrnovat nová funkce v technologii ASP.NET 2.0 týkající se připojovací řetězce. Technologie ASP.NET 2.0 přináší novou část v konfiguračním souboru, který umožňuje snadno ukládání připojovacích řetězců, které lze číst dynamicky za běhu. &lt;ConnectionStrings&gt; části usnadňuje ukládání připojovacích řetězců.

Následující fragment přidá nový připojovací řetězec.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Jenom jako s &lt;appSettings&gt; části &lt;connectionStrings&gt; mimo se objeví &lt;system.web&gt; oddíl v konfiguračním souboru.


Pokud chcete použít tento připojovací řetězec, můžete pomocí následující syntaxe při nastavování ConnectionString atribut ovládacího prvku serveru.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; oddíl se dá zašifrovat také tak, aby se nevystaví citlivé informace. Tuto možnost se budeme v novější modulu.

## <a name="caching-data-sources"></a>Ukládání do mezipaměti zdroje dat

Každý DataSourceControl obsahuje čtyři vlastnosti pro konfiguraci ukládání do mezipaměti; EnableCaching, CacheDuration, CacheExpirationPolicy a CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching je, že pro ovládací prvek zdroje dat je povolená vlastnost typu Boolean, která určuje, jestli ukládání do mezipaměti.

## <a name="cacheduration-property"></a>Vlastnost CacheDuration

Vlastnost CacheDuration nastaví počet sekund, po které zůstává v platnosti do mezipaměti. Nastavení této vlastnosti na **0** způsobí, že mezipaměti zůstává v platnosti, dokud explicitně zrušena.

## <a name="cacheexpirationpolicy-property"></a>Vlastnosti CacheExpirationPolicy

Vlastnost CacheExpirationPolicy může být nastaven na hodnotu **absolutní** nebo **posuvné**. Jeho nastavení na absolutní znamená, že maximální množství času, který bude do mezipaměti data počtu sekund zadaného CacheDuration vlastností. Nastavit na posuvné, čas vypršení platnosti se resetuje při každé operace.

## <a name="cachekeydependency-property"></a>Vlastnost CacheKeyDependency

Pokud je hodnota řetězce zadaná pro vlastnost CacheKeyDependency, ASP.NET nastaví závislost nové mezipaměti založené na tento řetězec. To umožňuje explicitně pomocí jednoduše změně nebo odebrání CacheKeyDependency platnost mezipaměti.

**Důležité**: Pokud je povoleno zosobnění a přístup k zdroj dat nebo obsahu dat podle identity klienta, se doporučuje, aby ukládání do mezipaměti je zakázat nastavením EnableCaching na hodnotu False. Pokud je povoleno ukládání do mezipaměti v tomto scénáři a uživatelem, než uživatel, který původně požádal data uživatelem vydá požadavek, autorizace ke zdroji dat se nevynucuje. Data se zpracuje jednoduše z mezipaměti.

## <a name="the-sqldatasource-control"></a>Ovládací prvek SqlDataSource

Ovládací prvek SqlDataSource umožňuje vývojáři přístup k datům uloženým v relační databáze podporující technologii ADO.NET. Zprostředkovatel System.Data.SqlClient může použít pro přístup k databázi systému SQL Server, zprostředkovatele System.Data.OleDb, zprostředkovatele System.Data.Odbc nebo System.Data.OracleClient zprostředkovatele, který má přístup k Oracle. Proto SqlDataSource není určitě používá pouze pro přístup k datům v databázi systému SQL Server.

Chcete-li použít SqlDataSource, jednoduše zadejte hodnotu pro vlastnost ConnectionString a zadáte příkaz SQL nebo uloženou proceduru. Ovládací prvek SqlDataSource postará práce základní architektury ADO.NET. Otevře připojení, dotazuje zdroj dat nebo provede uloženou proceduru, vrátí data o a poté uzavře připojení pro vás.

> [!NOTE]
> Protože třída DataSourceControl automaticky uzavře připojení pro vás, by měl snížit počet volání zákazníka generované úniku připojení databáze.


Následující fragment kódu vytvoří vazbu ovládacího prvku rozevírací seznam SqlDataSource ovládacího prvku s použitím připojovací řetězec, který je uložený v konfiguračním souboru jako v příkladu nahoře.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Jak je popsáno výše, určuje vlastnost DataSourceMode SqlDataSource režim pro zdroj dat. V předchozím příkladu DataSourceMode nastavena na DataReader. V takovém případě SqlDataSource vrátí IDataReader objekt, který používá kurzoru dopředné a jen pro čtení. Zadaný typ objektu, který je vrácen řídí zprostředkovatele, který se používá. V takovém případě používám zprostředkovatele System.Data.SqlClient zadané v &lt;connectionStrings&gt; části souboru web.config. Objekt, který je vrácen bude proto být typu SqlDataReader. Zadáním hodnoty DataSourceMode sady dat data mohou být uložena v datové sadě na serveru. Tento režim umožňuje přidat funkce, jako je například řazení, stránkování, atd. Pokud I kdyby byl datové vazby SqlDataSource do ovládacího prvku GridView, I byste vybrali režim datové sady. V případě rozevírací seznam, ale režim DataReader – je nejvhodnější.

> [!NOTE]
> Při ukládání do mezipaměti SqlDataSource nebo AccessDataSource, musí být vlastnost DataSourceMode nastavena na datovou sadu. Pokud povolíte ukládání do mezipaměti s DataSourceMode DataReader dojde k vyvolání výjimky.


## <a name="sqldatasource-properties"></a>Vlastnosti SqlDataSource

Toto jsou některé z vlastností ovládacího prvku SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Logická hodnota, která určuje, zda příkaz select se zruší, pokud jeden z parametrů má hodnotu null. Hodnota TRUE, ve výchozím nastavení.

### <a name="conflictdetection"></a>ConflictDetection

V situaci, kdy několik uživatelů může být aktualizace zdroje dat ve stejnou dobu určuje vlastnost ConflictDetection chování ovládacího prvku SqlDataSource. Tato vlastnost se vyhodnotí jako jedna z hodnot výčtu ConflictOptions. Tyto hodnoty jsou **CompareAllValues** a **OverwriteChanges**. Pokud je nastaven na OverwriteChanges, poslední osobě zapsat data do zdroje dat se přepíše předchozí změny. Ale pokud je nastavena ConflictDetection na CompareAllValues, parametry vytvářeny pro sloupce vrácený vlastnosti SelectCommand a také pro uložení původní hodnoty v každé z těchto sloupce, takže SqlDataSource pro vytvoření parametrů Určí, zda hodnoty se změnily od posledního vlastnosti SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Nastaví nebo získá řetězec SQL použít při odstranění řádků z databáze. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="deletecommandtype"></a>DeleteCommandType

Nastaví nebo získá typ příkaz delete, buď příkazu jazyka SQL (Text) nebo uložené proceduře (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Vrátí parametry, které jsou používány DeleteCommand SqlDataSourceView objektu přidruženého k ovládacímu prvku SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Tato vlastnost slouží k určení formátu původní hodnoty parametrů v případech, kde je vlastnost ConflictDetection nastavena na CompareAllValues. Výchozí hodnota je {0}, což znamená, že původní parametry s hodnotou bude trvat stejný název jako původní parametr. Jinými slovy, pokud je pole Název EmployeeID, původní hodnota parametru by @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Nastaví nebo získá řetězec SQL, který se používá k načtení dat z databáze. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="selectcommandtype"></a>SelectCommandType

Nastaví nebo získá typu příkazu select, buď příkazu jazyka SQL (Text) nebo uložené proceduře (StoredProcedure).

### <a name="selectparameters"></a>Prvků vlastnosti SelectParameters obsahovat

Vrátí parametry, které jsou používány SelectCommand SqlDataSourceView objektu přidruženého k ovládacímu prvku SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Získá nebo nastaví název parametru uložené procedury, který je používán při řazení dat načíst data zdrojového kódu. Platné jenom v případě, že SelectCommandType je nastaven na StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Oddělené středníky řetězec určující databáze a tabulky použité v mezipaměti závislost SQL Server. (Závislosti mezipaměti SQL budou popsané v novější modulu.)

### <a name="updatecommand"></a>UpdateCommand

Nastaví nebo získá řetězec SQL, který se používá k aktualizaci dat v databázi. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="updatecommandtype"></a>UpdateCommandType

Nastaví nebo získá typ příkazu update buď příkazu jazyka SQL (Text) nebo uložené proceduře (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Vrátí parametry, které jsou používány UpdateCommand SqlDataSourceView objektu přidruženého k ovládacímu prvku SqlDataSource.

## <a name="the-accessdatasource-control"></a>Ovládací prvek AccessDataSource

Ovládací prvek AccessDataSource je odvozena od třídy SqlDataSource a slouží k vazbě dat na databázi aplikace Microsoft Access. Vlastnost ConnectionString pro ovládací prvek AccessDataSource je vlastnost jen pro čtení. Místo použití vlastnost ConnectionString, se používá vlastnost datového souboru tak, aby odkazoval na databázi Access jak je uvedeno níže.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource bude vždy nastavená ProviderName základní SqlDataSource na System.Data.OleDb, připojí se k databázi pomocí zprostředkovatele Microsoft.Jet.OLEDB.4.0 OLE DB. Ovládací prvek AccessDataSource nelze použít pro připojení k databázi aplikace Access chráněný heslem. Pokud máte pro připojení k databázi chráněný heslem, používejte ovládacího prvku SqlDataSource.

> [!NOTE]
> Databáze Access uložené v rámci webové stránky musí být umístěny v aplikaci\_datový adresář. Technologie ASP.NET neumožňuje soubory v tomto adresáři chcete procházet. Je potřeba udělit oprávnění ke čtení a zápisu do aplikace účtu procesu\_adresář dat při použití databáze Access.


## <a name="the-xmldatasource-control"></a>Ovládacího prvku

Prvku se používá k data-bind XML data na ovládací prvky vázané na data. Můžete vázat na soubor XML pomocí vlastnosti datového souboru nebo můžete vázat na řetězec XML pomocí vlastnosti Data. Prvku zpřístupní atributům XML jako vazbu pole. V případech, kdy potřebujete vytvořit vazbu na hodnoty, které nejsou reprezentovány jako atributy budete muset použít transformace XSL. Můžete také použít výrazech XPath na data XML filtru.

Vezměte v úvahu následující soubor XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Všimněte si, že prvku používá podle vlastnosti XPath *osoby nebo osoba* Chcete-li filtrovat jenom v &lt;osoba&gt; uzlů. Rozevírací seznam a vazby dat do atribut LastName pomocí vlastnosti DataTextField.

Při ovládacího prvku je primárně použitého k vazbě dat na data XML jen pro čtení, je možné upravit datový soubor XML. Všimněte si, že v takových případech automatické vkládání, aktualizaci a odstranění informací v souboru XML nedojde automaticky stejně jako s ovládacími prvky další datové zdroje. Místo toho budete muset napsat kód pro ručně upravit data pomocí následujících metod ovládacího prvku.

### <a name="getxmldocument"></a>GetXmlDocument

Načte třídou XMLDocument nastavenou na objekt, který obsahuje kód XML načíst prvku.

### <a name="save"></a>Uložit

Zpět do zdroje dat uloží třídou XMLDocument nastavenou v paměti.

Je důležité si uvědomit, že metoda uložit bude fungovat pouze pokud jsou splněny následující dvě podmínky:

1. Prvku je při vytváření vazby do souboru XML místo vlastnosti dat k vytvoření vazby na data XML v paměti pomocí vlastnosti datového souboru.
2. Přes vlastnost transformace nebo TransformFile je zadána žádná transformace.

Všimněte si také, že metoda uložit přispět neočekávané výsledky při volání více uživatelů současně.

## <a name="the-objectdatasource-control"></a>Ovládacího prvku ObjectDataSource

Ovládací prvky zdroje dat, které jsme mít zahrnuté v tomto okamžiku jsou vynikající volby pro dvouvrstvé aplikace, kde prvek zdroje dat komunikuje přímo k úložišti dat. Však mnoho reálných aplikací jsou vícevrstvé aplikace, kde může ovládací prvek zdroje dat musí komunikovat obchodní objektu, který pak komunikuje s datové vrstvě. V těchto situacích ObjectDataSource doplní kusovníku vhodně. ObjectDataSource funguje ve spojení s ke zdrojovému objektu. Ovládacího prvku ObjectDataSource vytvoří instanci zdrojový objekt, vyvolá určenou metodu a uvolnění instance objektu vše v rámci jednoho požadavku, pokud má váš objekt metody instance namísto statických metod (sdílené v jazyce Visual Basic). Proto musí být objektu bezstavové. To znamená že objektu by měl získat a uvolnit všechny požadované prostředky v rámci rozpětí jedné žádosti. Můžete řídit, jak je zdrojový objekt vytvořený zpracování události ObjectCreating ovládacího prvku ObjectDataSource. Můžete vytvořit instanci objektu zdroje a poté nastavte vlastnost ObjectInstance třídy ObjectDataSourceEventArgs do této instance. Ovládacího prvku ObjectDataSource použije instanci, která je vytvořené v události ObjectCreating místo vytvoření instance svoje vlastní.

Pokud zdrojový objekt ovládacího prvku ObjectDataSource zpřístupní veřejné statické metody (sdílené v jazyce Visual Basic), které lze volat pro načtení a upravovat data, bude ovládacího prvku ObjectDataSource přímo volat tyto metody. Pokud ovládacího prvku ObjectDataSource musí vytvořit instanci objektu zdroje, aby volání metod, objekt musí obsahovat veřejný konstruktor, které nepřijímá žádné parametry. Ovládacího prvku ObjectDataSource zavolá tento konstruktor, když vytvoří novou instanci třídy zdrojový objekt.

Pokud je zdrojový objekt neobsahuje veřejný konstruktor bez parametrů, můžete vytvořit instanci zdrojový objekt, který se použije v ObjectCreating události ovládacího prvku ObjectDataSource.

## <a name="specifying-object-methods"></a>Určení metod objektů

Zdrojový objekt ovládacího prvku ObjectDataSource může obsahovat libovolný počet metody, které se používají k výběru, vložit, aktualizovat nebo odstranit data. Tyto metody jsou volány ovládacího prvku ObjectDataSource na základě názvu metody, jak identifikovat pomocí metody SelectMethod, metodu InsertMethod, metoda UpdateMethod nebo metody DeleteMethod vlastností ovládacího prvku ObjectDataSource. Volitelné SelectCount metoda, který je identifikován pomocí ovládacího prvku ObjectDataSource pomocí SelectCountMethod vlastnosti, která vrátí počet celkový počet objektů ve zdroji dat, které může zahrnovat taky zdrojový objekt. Ovládacího prvku ObjectDataSource bude volat metodu SelectCount po zavolání vyberte metodu pro načtení celkový počet záznamů ve zdroji dat pro použití při stránkování.

## <a name="lab-using-data-source-controls"></a>Prostředí pomocí ovládacích prvků zdroje dat

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Cvičení 1 - zobrazení dat pomocí ovládacího prvku SqlDataSource

Následujícím cvičení používá k připojení k databázi Northwind ovládacího prvku SqlDataSource. Přitom se předpokládá, že máte přístup k databázi Northwind na instanci systému SQL Server 2000.

1. Vytvoření nového webu ASP.NET.
2. Přidáte nový soubor web.config.

    1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na tlačítko Přidat novou položku.
    2. Zvolte konfigurační soubor webu ze seznamu šablon a klikněte na tlačítko Přidat.
3. Upravit &lt;connectionStrings&gt; části následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Přepnout na zobrazení kódu a přidání ConnectionString atribut a atribut SelectCommand k &lt;asp: SqlDataSource&gt; řízení následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. Ze zobrazení návrhu přidejte nový ovládací prvek GridView.
6. Z rozevíracího seznamu vyberte zdroj dat v nabídce Úkoly GridView zvolte SqlDataSource1.
7. Klikněte pravým tlačítkem na Default.aspx a z nabídky zvolte zobrazení v prohlížeči. Po zobrazení výzvy k uložit, klikněte na tlačítko Ano.
8. GridView zobrazí data z tabulky produktů.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Cvičení 2 – úpravy daty pomocí ovládacího prvku SqlDataSource

Následujícím cvičení ukazuje, jak k vazbě dat rozevírací seznam řízení pomocí deklarativní syntaxe a slouží k úpravě data uvedená v ovládací prvek rozevírací seznam.

1. V návrhovém zobrazení odstraňte prvek GridView z Default.aspx. 

    **Důležité**: ponechte ovládacího prvku SqlDataSource na stránce.
2. Přidáte ovládací prvek rozevírací seznam Default.aspx.
3. Přepnout na zobrazení zdroje.
4. Přidat atribut DataSourceId, DataTextField a DataValueField k &lt;asp: rozevírací seznam&gt; řízení následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Uložte Default.aspx a zobrazit v prohlížeči. Všimněte si, že rozevírací seznam obsahuje všechny produkty z databáze Northwind.
6. Zavřete prohlížeč.
7. V zobrazení zdroje Default.aspx přidejte nový ovládací prvek textového pole pod ovládací prvek rozevírací seznam. Změňte vlastnosti ID textové pole na txtProductName.
8. V ovládacím prvku TextBox přidáte nový ovládací prvek tlačítko. Změňte vlastnost ID tlačítka na btnUpdate a vlastnosti textu k **název produktu aktualizace**.
9. V zobrazení zdroje Default.aspx přidejte vlastnost UpdateCommand a dvě nové UpdateParameters ke značce SqlDataSource následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Všimněte si, že jsou dvě aktualizace parametry (ProductName a ProductID) přidali v tomto kódu. Tyto parametry jsou namapované na vlastnosti textu txtProductName textové pole a vlastnost SelectedValue ddlProducts rozevírací seznam.
10. Přepněte do zobrazení návrhu a dvakrát klikněte na ovládací prvek tlačítko pro přidání obslužné rutiny události.
11. Přidejte následující kód do btnUpdate\_klikněte na tlačítko kódu: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Klikněte pravým tlačítkem na Default.aspx a zvolte zobrazíte v prohlížeči. Po zobrazení výzvy k uložení všech změn, klikněte na tlačítko Ano.
13. ASP.NET 2.0 povolit částečné třídy pro kompilaci za běhu. Není nutné pro vytvoření aplikace, chcete-li zobrazit změny kódu vstoupí v platnost.
14. Vyberte produkt rozevírací seznam.
15. Do textového pole zadejte nový název pro vybraný produkt a potom klikněte na tlačítko Aktualizovat.
16. Název produktu je aktualizována v databázi.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Cvičení 3 pomocí ovládacího prvku ObjectDataSource

Toto cvičení ukazují, jak pracovat s databázi Northwind pomocí ovládacího prvku ObjectDataSource a ke zdrojovému objektu.

1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na Přidat novou položku.
2. V seznamu šablon vyberte webového formuláře. Změňte název na object.aspx a klikněte na tlačítko Přidat.
3. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na Přidat novou položku.
4. Vyberte třídu v seznamu šablon. Změňte název třídy NorthwindData.cs a klikněte na tlačítko Přidat.
5. Klikněte na tlačítko Ano, při zobrazení výzvy k přidání třídu do aplikace\_složky kódu.
6. Přidejte následující kód do souboru NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Přidejte následující kód do zdrojového zobrazení object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Uložte všechny soubory a procházet object.aspx.
9. Zobrazení podrobností, úpravy zaměstnanci, zaměstnanci přidáním a odstraněním zaměstnanci komunikovat s rozhraním.
