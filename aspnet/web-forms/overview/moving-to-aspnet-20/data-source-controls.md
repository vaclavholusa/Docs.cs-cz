---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Ovládací prvky zdroje dat | Dokumentace Microsoftu
author: microsoft
description: DataGrid – ovládací prvek technologie ASP.NET 1.x označené skvělé zlepšení v přístupu k datům ve webových aplikacích. Však nebyl jako uživatelsky přívětivé, jako by byly...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 225e23c0e7f4c2f4b8a54ee3c07e2614e9ba1038
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388448"
---
<a name="data-source-controls"></a>Ovládací prvky zdroje dat
====================
podle [Microsoft](https://github.com/microsoft)

> DataGrid – ovládací prvek technologie ASP.NET 1.x označené skvělé zlepšení v přístupu k datům ve webových aplikacích. Však nebyl jako uživatelsky přívětivé, jako by byly. Stále vyžaduje značné množství kódu, který z něj získat mnoho užitečných funkcí. Například je model v všechna data access přejeme v 1.x.


DataGrid – ovládací prvek technologie ASP.NET 1.x označené skvělé zlepšení v přístupu k datům ve webových aplikacích. Však nebyl jako uživatelsky přívětivé, jako by byly. Stále vyžaduje značné množství kódu, který z něj získat mnoho užitečných funkcí. Například je model v všechna data access přejeme v 1.x.

ASP.NET 2.0 adresy se v části ovládací prvky zdroje dat. Ovládací prvky zdroje dat v technologii ASP.NET 2.0 poskytují vývojářům deklarativní model pro načítání dat, zobrazení dat a úpravy dat. Účelem ovládací prvky zdroje dat je poskytnout konzistentní reprezentace dat na ovládací prvky vázané na data bez ohledu na zdroj těchto údajů. V srdci ovládací prvky zdroje dat v technologii ASP.NET 2.0 je abstraktní třída DataSourceControl. Třída DataSourceControl poskytuje základní implementaci rozhraní IDataSource a rozhraní IListSource, druhá možnost, které vám umožní přiřadit ovládací prvek zdroje dat jako zdroj dat ovládací prvek vázaný na data (prostřednictvím nové vlastnosti DataSourceId prodiskutována později) a zobrazit data v něm jako seznam. Každý seznam data z ovládacího prvku zdroje dat je přístupný jako objekt zobrazení DataSourceView. Poskytuje přístup k zobrazení DataSourceView instance rozhraní IDataSource. Například metoda GetViewNames vrátí ICollection, která umožňuje vytvořit výčet DataSourceViews spojené s konkrétní data správy zdrojového kódu a metoda GetView umožňuje přístup ke konkrétní instanci zobrazení DataSourceView podle názvu.

Ovládací prvky zdroje dat k dispozici žádné uživatelské rozhraní. Jsou implementovány jako serverové ovládací prvky, aby se podporovaly deklarativní syntaxe, a tak, aby v případě potřeby mají přístup do stavu stránky. Ovládací prvky zdroje dat zobrazené libovolný kód HTML do klienta.

> [!NOTE]
> Jak uvidíte později, že jsou také ukládání do mezipaměti výhody získat pomocí ovládací prvky zdroje dat.


## <a name="storing-connection-strings"></a>Ukládání připojovacích řetězců

Předtím, než se dostaneme k pohledu na tom, jak nakonfigurovat ovládací prvky zdroje dat, by měla zahrnovat jsme novou funkci v technologii ASP.NET 2.0 týkající se připojovací řetězce. Technologie ASP.NET 2.0 přináší nový oddíl v konfiguračním souboru, který umožňuje snadno ukládání připojovacích řetězců, které může číst dynamicky za běhu. &lt;ConnectionStrings&gt; části zjednodušuje ukládání připojovacích řetězců.

Následující fragment kódu přidá nový připojovací řetězec.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Stejně jako u &lt;appSettings&gt; části &lt;connectionStrings&gt; mimo se zobrazí část &lt;system.web&gt; oddílu v konfiguračním souboru.


Pokud chcete použít tento připojovací řetězec, můžete použijte následující syntaxi, při nastavení atributu ConnectionString serverového ovládacího prvku.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; oddílu se dá zašifrovat taky tak, aby se nevystaví citlivé informace. Tuto možnost najdete v novějším modulu.

## <a name="caching-data-sources"></a>Ukládání do mezipaměti zdroje dat

Každý DataSourceControl poskytuje čtyři vlastnosti ke konfiguraci ukládání do mezipaměti; EnableCaching CacheDuration, CacheExpirationPolicy a CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching je pro ovládací prvek zdroje dat je povolena vlastnost typu Boolean určující, jestli ukládání do mezipaměti.

## <a name="cacheduration-property"></a>Vlastnost CacheDuration

Vlastnost CacheDuration nastaví počet sekund, po které zůstává v platnosti do mezipaměti. Nastavení této vlastnosti na **0** způsobí, že v mezipaměti platnosti, dokud není výslovně zrušena.

## <a name="cacheexpirationpolicy-property"></a>Vlastnosti CacheExpirationPolicy

Vlastnost CacheExpirationPolicy může být nastaven na hodnotu **absolutní** nebo **posuvné**. Nastavení na absolutní znamená, že je počet sekund, po určeném vlastností CacheDuration maximální množství času, které se data uložená v mezipaměti. Nastavením jeho posuvné čas vypršení platnosti se vynuluje, když každá operace provádí.

## <a name="cachekeydependency-property"></a>Vlastnost CacheKeyDependency

Pokud je hodnota řetězce zadaná pro vlastnost CacheKeyDependency, bude technologie ASP.NET nastavit novou závislost mezipaměti na základě tohoto řetězce. To umožňuje tak jednoduše změně nebo odebrání CacheKeyDependency explicitně zneplatnění mezipaměti.

**Důležité**: Pokud je povolené zosobnění a přístup ke zdroji dat a/nebo obsah dat jsou založeny na identity klienta, se doporučuje že ukládání do mezipaměti zakázáno nastavením EnableCaching na hodnotu False. Pokud je povoleno ukládání do mezipaměti v tomto scénáři a uživatelem, než uživatel, který původně požadovali data vydá požadavek na autorizaci ke zdroji dat se nevynucuje. Data se jednoduše obsluhovat z mezipaměti.

## <a name="the-sqldatasource-control"></a>Ovládacím prvkem SqlDataSource

Ovládacím prvkem SqlDataSource umožňuje vývojáři přístup k datům uloženým v relační databázi, který podporuje technologii ADO.NET. Zprostředkovatele System.Data.SqlClient může použít pro přístup k databázi serveru SQL Server, poskytovatel System.Data.OleDb, System.Data.Odbc zprostředkovatele nebo zprostředkovatele System.Data.OracleClient pro přístup k Oracle. Proto ve třídě SqlDataSource není jistě používá jenom pro přístup k datům v databázi serveru SQL Server.

Pokud chcete používat ve třídě SqlDataSource, jednoduše zadejte hodnotu pro vlastnost ConnectionString a zadáte příkaz SQL nebo uloženou proceduru. Ovládacím prvkem SqlDataSource postará o práci s základní architekturu ADO.NET. Otevře připojení k, dotazuje zdroje dat nebo provede uloženou proceduru, vrátí data a potom jej zavře spojení za vás.

> [!NOTE]
> Protože třída DataSourceControl automaticky uzavře spojení za vás, by měla snížit počet volání zákazníka generovaných nevrácení připojení k databázi.


Následující fragment kódu váže ovládací prvek DropDownList s ovládacím prvkem SqlDataSource pomocí připojovacího řetězce, která je uložena v konfiguračním souboru, jak je znázorněno výše.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Jak je znázorněno výše, určuje vlastnost DataSourceMode ve třídě SqlDataSource režim datového zdroje. V předchozím příkladu nastavena vlastnost DataSourceMode na hodnotu objektu DataReader. V takovém případě ve třídě SqlDataSource vrátí objekt IDataReader pomocí kurzoru dopředné a jen pro čtení. Zadaný typ, který je vrácen řídí poskytovatele, který se používá. V takovém případě používám zprostředkovatele System.Data.SqlClient jako zadané v poli &lt;connectionStrings&gt; část souboru web.config. Objekt, který je vrácen proto bude typu SqlDataReader. Zadáním hodnoty DataSourceMode datové sady, můžete data uložená v datové sadě na serveru. Tento režim umožňuje přidat funkce, jako je řazení, stránkování, atd. Pokud jsem byl datové vazby ve třídě SqlDataSource do ovládacího prvku GridView, můžu byste zvolili režim datové sady. V případě DropDownList, je režim DataReader správnou volbu.

> [!NOTE]
> Při ukládání do mezipaměti SqlDataSource nebo prvku AccessDataSource, vlastnost DataSourceMode je nastavena na hodnotu DataSet. Pokud povolíte ukládání do mezipaměti s DataSourceMode DataReader dojde k vyvolání výjimky.


## <a name="sqldatasource-properties"></a>Vlastnosti SqlDataSource

Následují některé vlastnosti ovládacím prvkem SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Logická hodnota, která určuje, zda je příkazu select zrušena, pokud jeden z parametrů má hodnotu null. Hodnota TRUE, ve výchozím nastavení.

### <a name="conflictdetection"></a>ConflictDetection

V situaci, kdy více uživatelů může být aktualizace zdroje dat ve stejnou dobu určuje vlastnost ConflictDetection chování ovládacím prvkem SqlDataSource. Tato vlastnost je vyhodnocen jako jedna z hodnot výčtu ConflictOptions. Tyto hodnoty jsou **CompareAllValues** a **hodnotu OverwriteChanges**. Pokud je nastaveno na hodnotu OverwriteChanges jméno poslední osoby k zápisu dat do zdroje dat přepíše jakékoli předchozí změny. Pokud ale vlastnost ConflictDetection je nastavena na CompareAllValues, parametry vytvořené pro sloupce vrácený SelectCommand a parametry jsou také vytvořená k uložení původní hodnoty v každém z těchto sloupců ve třídě SqlDataSource k povolení určení, zda hodnoty se změnily od posledního SelectCommand.

### <a name="deletecommand"></a>Událost DeleteCommand

Nastaví nebo získá SQL řetězec použitý při odstranění řádků z databáze. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="deletecommandtype"></a>DeleteCommandType

Nastaví nebo získá typ příkazu pro odstranění, buď dotaz SQL (Text) nebo uložené procedury (uložené procedury StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Vrátí parametry, které jsou používány DeleteCommand SqlDataSourceView objektu souvisejícího s ovládacím prvkem SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Tato vlastnost se používá k určení formátu původní hodnoty parametrů v případech, ve kterém je nastavena vlastnost ConflictDetection na CompareAllValues. Výchozí hodnota je {0} to znamená, že původní hodnoty parametrů bude trvat stejný název jako původní parametr. Jinými slovy, pokud je název pole EmployeeID, původní hodnota parametru by @EmployeeID.

### <a name="selectcommand"></a>Vlastnost SelectCommand

Nastaví nebo získá řetězec SQL, který se používá k načtení dat z databáze. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="selectcommandtype"></a>SelectCommandType

Nastaví nebo získá typu příkazu select, buď dotaz SQL (Text) nebo uložené procedury (uložené procedury StoredProcedure).

### <a name="selectparameters"></a>Prvků vlastnosti SelectParameters obsahovat

Vrátí parametry, které jsou používány SelectCommand SqlDataSourceView objektu souvisejícího s ovládacím prvkem SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Získá nebo nastaví název parametru uložené procedury, která se používá při řazení dat načíst ovládací prvek zdroje dat. Platné pouze v případě, že SelectCommandType je nastavena na uložené procedury StoredProcedure.

### <a name="sqlcachedependency"></a>Vlastnost sqlCacheDependency

Středníkem oddělený řetězec určující databáze a tabulky používané v závislosti mezipaměti SQL Server. (Závislosti mezipaměti SQL probereme v novějším modulu.)

### <a name="updatecommand"></a>Událost UpdateCommand

Nastaví nebo získá řetězec SQL, který se používá při aktualizaci dat v databázi. To může být buď dotaz SQL nebo název uložené procedury.

### <a name="updatecommandtype"></a>UpdateCommandType

Nastaví nebo získá typ příkazu pro aktualizaci, buď dotaz SQL (Text) nebo uložené procedury (uložené procedury StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Vrátí parametry, které jsou používány UpdateCommand SqlDataSourceView objektu souvisejícího s ovládacím prvkem SqlDataSource.

## <a name="the-accessdatasource-control"></a>Ovládací prvek AccessDataSource

Ovládací prvek AccessDataSource je odvozena od třídy SqlDataSource a je použitého k vazbě dat na databázi aplikace Microsoft Access. Vlastnost ConnectionString prvku AccessDataSource je vlastnost jen pro čtení. Namísto použití vlastnosti ConnectionString, je vlastnost DataFile použít tak, aby odkazoval na databázi aplikace Access, jak je znázorněno níže.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

Prvku AccessDataSource ProviderName základní SqlDataSource bude vždycky nastavený na System.Data.OleDb a připojuje k databázi pomocí zprostředkovatele Microsoft.Jet.OLEDB.4.0 OLE DB. Ovládací prvek AccessDataSource nelze použít pro připojení k databázi aplikace Access chráněný heslem. Pokud máte připojení k databázi chráněn heslem, používejte ovládacím prvkem SqlDataSource.

> [!NOTE]
> Accessové databáze, které jsou uloženy v rámci tohoto webu musí být umístěné ve aplikace\_datový adresář. ASP.NET nebude povolovat ukládání souborů do tohoto adresáře, které budete procházet. Budete muset udělit oprávnění ke čtení a zápis do ní účet procesu\_adresář dat při použití databáze aplikace Access.


## <a name="the-xmldatasource-control"></a>Ovládací prvek XmlDataSource

Prvek XmlDataSource slouží k data-bind XML data do ovládacích prvků vázaných na data. Můžete svázat do souboru XML pomocí vlastnosti DataFile nebo můžete vytvořit vazbu na řetězec XML pomocí vlastnosti Data. Prvek XmlDataSource poskytuje atributy ve formátu XML jako pole s možností vazby. V případech, kdy potřebujete k vytvoření vazby na hodnoty, které nejsou reprezentovány jako atributy je potřeba použít transformace XSL. Můžete také použít výrazy XPath pro filtrování dat XML.

Vezměte v úvahu následující soubor XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Všimněte si, že prvek XmlDataSource používá výraz XPath vlastnosti *osoby nebo osoby* Chcete-li filtrovat na jenom &lt;osoba&gt; uzly. DropDownList a data vazbu pomocí vlastnosti DataTextField atributu příjmení.

Zatímco ovládacího prvku XmlDataSource se používá především k vazbě dat na jen pro čtení dat XML, je možné upravit datový soubor XML. Všimněte si, že v takových případech automatické vložení, aktualizace nebo odstranění informací v souboru XML neprobíhá automaticky stejně jako s jinými ovládacími prvky dat zdroje. Místo toho budete muset psát kód ručně upravovat data, pomocí následujících metod ovládacího prvku XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Načte třídou XMLDocument nastavenou na objekt, který obsahuje kód XML pomocí prvku XmlDataSource.

### <a name="save"></a>Uložit

Uloží zpět do zdroje dat. XmlDocument v paměti.

Je důležité si uvědomit, že metodu Save bude fungovat jenom Pokud jsou splněny následující dvě podmínky:

1. Prvek XmlDataSource používá vlastnost DataFile k vytvoření vazby k souboru XML, nikoliv vlastnost datového připojení k datům XML v paměti.
2. Žádná transformace je zadáno pomocí vlastnosti Transform nebo TransformFile.

Všimněte si také, že metodu Save může vést k neočekávaným výsledkům při volání více uživatelů současně.

## <a name="the-objectdatasource-control"></a>Ovládací prvek ObjectDataSource

Ovládací prvky zdroje dat, které jsme pokryli do této chvíle jsou výborné možnosti pro dvouvrstvé aplikace, ve kterém ovládací prvek zdroje dat komunikuje přímo do úložiště dat. Mnoho skutečných aplikací jsou však vícevrstvých aplikací ve kterém ovládací prvek zdroje dat může být nutné ke komunikaci se obchodního objektu, které pak komunikuje s datové vrstvě. V těchto situacích ObjectDataSource krásně vyplní faktury. Prvku ObjectDataSource funguje ve spojení s zdrojový objekt. Ovládací prvek ObjectDataSource vytvoří instanci objektu zdroje, volání zadanou metodu a uvolnění instance objektu vše v rámci jedné žádosti, pokud objekt obsahuje instanci metody místo statické metody (Shared v jazyce Visual Basic). Proto musí být objekt bezstavové. To znamená že váš objekt by měl získat a uvolnit všechny prostředky v rámci rozsahu jednoho požadavku. Můžete řídit způsob vytvoření zdrojového objektu zpracováním událostí ObjectCreating ovládacího prvku ObjectDataSource. Můžete vytvořit instanci objektu zdroje a potom nastavte vlastnost ObjectInstance třídy ObjectDataSourceEventArgs do této instance. Ovládací prvek ObjectDataSource, bude používat instanci, která se vytvoří v případě ObjectCreating namísto vytvoření instance sama o sobě.

Pokud zdrojový objekt pro ovládací prvek ObjectDataSource zpřístupní veřejné statické metody (Shared v jazyce Visual Basic), které lze volat pro načítání a úpravy dat, bude ovládací prvek ObjectDataSource přímo volat tyto metody. Pokud ovládací prvek ObjectDataSource musí vytvořit instanci objektu zdroje abyste mohli volat metodu, musí obsahovat objekt veřejný konstruktor, který nepřijímá žádné parametry. Ovládací prvek ObjectDataSource zavolá tento konstruktor vytvoří novou instanci objektu zdroje.

Pokud zdrojový objekt neobsahuje veřejný konstruktor bez parametrů, můžete vytvořit instanci objektu zdroje, který se použije v případě ObjectCreating ovládacího prvku ObjectDataSource.

## <a name="specifying-object-methods"></a>Určení objektových metod

Zdrojového objektu ovládacího prvku ObjectDataSource může obsahovat libovolný počet metod, které se používají k výběru, vložit, aktualizovat nebo odstranit data. Tyto metody jsou volány ovládacího prvku ObjectDataSource vycházet z názvu metody, jsme uvedli, pomocí metody SelectMethod, InsertMethod, UpdateMethod a DeleteMethod vlastnosti ovládacího prvku ObjectDataSource. Zdrojový objekt také může obsahovat volitelné SelectCount metodu, která je identifikovaná ovládacího prvku ObjectDataSource vlastnost SelectCountMethod, který vrací počet celkový počet objektů ve zdroji dat. Ovládací prvek ObjectDataSource bude volat metodu SelectCount po zavolání metody Select pro načtení celkový počet záznamů ve zdroji dat pro použití při stránkování.

## <a name="lab-using-data-source-controls"></a>Ovládací prvky zdroje dat pomocí testovacího prostředí

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Cvičení 1 – zobrazení dat ovládacím prvkem SqlDataSource(VB)

Následující cvičení používá ovládacím prvkem SqlDataSource pro připojení k databázi Northwind. Předpokládá, že máte přístup k databázi Northwind v instanci systému SQL Server 2000.

1. Vytvoření nového webu technologie ASP.NET.
2. Přidejte nový soubor web.config.

    1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na tlačítko Přidat novou položku.
    2. Ze seznamu šablon vyberte soubor webové konfigurace a klikněte na tlačítko Přidat.
3. Upravit &lt;connectionStrings&gt; části následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Přepnout na zobrazení kódu a přidejte atribut ConnectionString a SelectCommand atributu &lt;asp: SqlDataSource&gt; řídit následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. V návrhovém zobrazení přidání nového ovládacího prvku GridView.
6. V rozevírací nabídce zvolit zdroj dat v nabídce úlohy ovládacího prvku GridView zvolte SqlDataSource1.
7. Klikněte pravým tlačítkem myši na Default.aspx a zvolte v nabídce zobrazení v prohlížeči. Po zobrazení výzvy k uložení klikněte na tlačítko Ano.
8. GridView zobrazí data z tabulky produktů.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Cvičení 2 - Úprava dat ovládacím prvkem SqlDataSource(VB)

Následující cvičení předvádí, jak vytvořit vazbu na data DropDownList kontrolovat pomocí deklarativní syntaxe a slouží k úpravě dat v ovládacím prvku DropDownList.

1. V návrhovém zobrazení odstraňte z Default.aspx ovládacím prvku GridView. 

    **Důležité**: ponechat SqlDataSource ovládací prvek na stránce.
2. Přidejte ovládací prvek DropDownList Default.aspx.
3. Přepněte do zobrazení zdroje.
4. Přidat atribut DataSourceId, DataTextField a DataValueField k &lt;asp: DropDownList&gt; řídit následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Uložit Default.aspx a zobrazit v prohlížeči. Poznámka: aby DropDownList obsahuje všechny produkty z databáze Northwind.
6. Zavřete prohlížeč.
7. V zobrazení zdroje Default.aspx přidání nového ovládacího prvku textového pole pod ovládacím prvkem DropDownList. Změňte vlastnost ID textového pole na txtProductName.
8. V části ovládacího prvku textového pole přidáte nový ovládací prvek tlačítko. Změnit vlastnosti ID na tlačítko na btnUpdate a vlastnost Text na **název produktu aktualizace**.
9. V zobrazení zdroje Default.aspx přidejte vlastnost UpdateCommand a dva nové UpdateParameters ke značce SqlDataSource následujícím způsobem: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Všimněte si, že existují dvě aktualizovat parametry (ProductName a ProductID) přidá tento kód. Tyto parametry jsou mapovány na vlastnosti Text txtProductName textového pole a vlastnosti SelectedValue ddlProducts DropDownList.
10. Přepněte do zobrazení návrhu a dvakrát klikněte na ovládací prvek tlačítko pro přidání obslužné rutiny události.
11. Přidejte následující kód btnUpdate\_klikněte na tlačítko kódu: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Klikněte pravým tlačítkem myši na Default.aspx a zvolte, chcete-li zobrazit v prohlížeči. Po zobrazení výzvy k uložení všech změn, klikněte na tlačítko Ano.
13. ASP.NET 2.0 povolit částečné třídy pro kompilaci za běhu. Není nutné vytvořit aplikaci, chcete-li zobrazit změny kódu projeví.
14. Vyberte produkt z DropDownList.
15. Zadejte nový název pro vybraný produkt do textového pole a potom klikněte na tlačítko Aktualizovat.
16. Název produktu je aktualizována v databázi.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Cvičení 3 pomocí ovládacího prvku ObjectDataSource

V tomto cvičení vám ukáže způsob použití ovládacího prvku ObjectDataSource a zdrojový objekt interakci s databází Northwind.

1. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na Přidat novou položku.
2. V seznamu šablon vyberte webový formulář. Změňte název na object.aspx a klikněte na tlačítko Přidat.
3. Klikněte pravým tlačítkem na projekt v Průzkumníku řešení a klikněte na Přidat novou položku.
4. V seznamu šablon vyberte třídu. Změnit název třídy NorthwindData.cs a klikněte na tlačítko Přidat.
5. Klikněte na tlačítko Ano, po zobrazení výzvy k přidání třídy do aplikace\_složky s kódem.
6. Přidejte následující kód do souboru NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Přidejte následující kód k zobrazení zdroje object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Uložte všechny soubory a procházet object.aspx.
9. Zobrazení podrobností, úpravy zaměstnanci, zaměstnanci přidáním a odstraněním zaměstnanci pracovat s rozhraním.
