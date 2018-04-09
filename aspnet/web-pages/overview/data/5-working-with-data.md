---
uid: web-pages/overview/data/5-working-with-data
title: Úvod k práci s databází v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tato kapitola popisuje, jak přistupovat k datům z databáze a zobrazit ji pomocí rozhraní ASP.NET Web Pages.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 673d502f-2c16-4a6f-bb63-dbfd9a77ef47
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/5-working-with-data
msc.type: authoredcontent
ms.openlocfilehash: 563074cf3e60717c2e6c336a2c282b4203f73b8b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="introduction-to-working-with-a-database-in-aspnet-web-pages-razor-sites"></a>Úvod k práci s databází v rozhraní ASP.NET Web Pages lokalit (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje, jak vytvořit databázi na webu technologie ASP.NET Web Pages (Razor) pomocí nástroje Microsoft WebMatrix a postup vytvoření stránky, které vám umožní zobrazit, přidat, upravit a odstranit data.
> 
> **Získáte informace:** 
> 
> - Postup vytvoření databáze.
> - Jak se připojit k databázi.
> - Jak zobrazit data na webové stránce.
> - Postup vložení, aktualizace a odstranění záznamů databáze.
> 
> Tyto jsou funkce, zavedená v článku:
> 
> - Práce s databáze Microsoft SQL Server Compact Edition.
> - Práce s dotazy SQL.
> - `Database` Třídy.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 2
> - Služba WebMatrix 2
>   
> 
> V tomto kurzu taky spolupracuje se službou WebMatrix 3. Můžete použít ASP.NET Web Pages 3 a Visual Studio 2013 (nebo Visual Studio Express 2013 pro Web); uživatelské rozhraní se ale liší.


## <a name="introduction-to-databases"></a>Úvod do databáze

Představte si typické adresáře. Pro každou položku v adresáři (který je pro každou osobu) máte několik informací například křestní jméno, příjmení, adresu, e-mailová adresa a telefonní číslo.

Typické způsob, jak data obrázku takto je jako tabulku s řádky a sloupce. V databázi podmínky každý řádek je často označuje jako záznam. Každý sloupec (někdy označované jako pole) obsahuje hodnotu pro každý typ dat: název první, poslední název a tak dále.

| **ID** | **FirstName** | **LastName** | **Adresa** | **E-mailu** | **Telefon** |
| --- | --- | --- | --- | --- | --- |
| 1 | Jima | Abrus | 210 100th St SE Orcas WA 98031 | jim@contoso.com | 555 0100 |
| 2 | Terry | Adams | 1234 hlavní Svatý Seattle WA 99011 | terry@cohowinery.com | 555 0101 |

Pro většinu tabulky databáze v tabulce musí obsahovat sloupec, který obsahuje jedinečný identifikátor, zákaznické číslo, číslo účtu, atd. To se označuje jako v tabulce *primární klíč*, a můžete ji použít k identifikaci každý řádek v tabulce. V příkladu je ID sloupce primární klíč pro adresář.

S Tento základní znalosti databází, jste připravení zjistěte, jak vytvořit jednoduchou databázi a provádět operace, jako je přidání, úpravy a odstraňování dat.

> [!TIP] 
> 
> **Relační databáze**
> 
> Data můžete uložit v mnoha různými způsoby, včetně textových souborů a tabulek. U většiny obchodní použití ale data uložena v relační databázi.
> 
> V tomto článku není velmi úzce přejděte do databáze. Však může pro vás užitečné zjistit, o něco o nich. V relační databázi informace logicky rozděleny do samostatných tabulkách. Databáze pro školy může například obsahovat samostatných tabulkách pro studenty a pro třídu nabídky. Databáze softwaru (jako je například SQL Server) podporuje výkonné příkazy, které vám umožní dynamicky vytvořit relace mezi tabulkami. Relační databáze můžete například použít k vytvoření vztahu logické mezi studenty a třídy, aby bylo možné vytvořit plán. Ukládání dat v samostatných tabulkách snižuje složitost struktura tabulky a snižuje nutnost ponechat redundantní data v tabulkách.


## <a name="creating-a-database"></a>Vytvoření databáze

Tento postup ukazuje, jak vytvořit databázi s názvem SmallBakery pomocí nástroje návrhu databáze systému SQL Server Compact, která je součástí služby WebMatrix. I když můžete vytvořit databázi pomocí kódu, je typičtější pro vytvoření databáze a tabulky databáze pomocí nástroje návrhu, například služby WebMatrix.

1. Spusťte službu WebMatrix a na stránce Rychlý Start klikněte na **šablonu z webu**.
2. Vyberte **prázdný web**a v **název lokality** pole zadejte "SmallBakery" a pak klikněte na tlačítko **OK**. Tento web je vytvořen a zobrazí ve službě WebMatrix.
3. V levém podokně klikněte **databáze** pracovního prostoru.
4. Na pásu karet klikněte na tlačítko **novou databázi**. Se stejným názvem jako web služby se vytvoří prázdnou databázi.
5. V levém podokně rozbalte **SmallBakery.sdf** uzel a pak klikněte na tlačítko **tabulky**.
6. Na pásu karet klikněte na tlačítko **novou tabulku**. Služba WebMatrix otevře návrháře tabulky.

    ![[Obrázek]](5-working-with-data/_static/image1.jpg)
7. Kliknutím na tlačítko ve **název** sloupci a zadejte &quot;Id&quot;.
8. V **datový typ** sloupce, vyberte **int**.
9. Nastavte **je primární klíč?** a **je identifikovat?** , které chcete **Ano**.

    Jak již název naznačuje, **je primární klíč** databáze oznamuje, zda bude primární klíč v tabulce. **Je identita** informuje databázi pro automatické vytvoření číslo ID pro každý nový záznam a přiřaďte ho Další pořadové číslo (počínaje 1).
10. Klepněte na další řádek. Editor spustí novou definici sloupce.
11. Zadejte hodnotu názvu &quot;název&quot;.
12. Pro **datový typ**, zvolte &quot;nvarchar&quot; a nastavte délku na 50. *Var* součástí `nvarchar` databáze oznamuje, že data pro tento sloupec bude řetězec, jehož velikost může lišit od záznamy. ( *n* předpony představuje *national*, která určuje, že pole může obsahovat znak data, která představuje všechny abecedy nebo zápis systému &#8212; to znamená, že pole obsahuje data ve formátu Unicode.)
13. Nastavte **povolit hodnoty Null** možnost k **ne**. To bude vynucení, který *název* sloupec není ponecháno prázdné.
14. Pomocí tohoto procesu stejné vytvořte sloupec s názvem *popis*. Nastavit **datový typ** "nvarchar" a 50 délky a sadu **povolit hodnoty Null** má hodnotu false.
15. Vytvořit sloupec s názvem *cena*. Nastavit **datový typ "money"** a nastavte **povolit hodnoty Null** má hodnotu false.
16. Do pole v horní části, název tabulky &quot;produktu&quot;.

    Když jste hotovi, definice bude vypadat například takto:

    ![[Obrázek]](5-working-with-data/_static/image2.jpg)
17. Stisknutím kláves Ctrl + S uložit v tabulce.

## <a name="adding-data-to-the-database"></a>Přidání dat do databáze

Teď můžete přidat ukázková data do databáze, který bude fungovat s později v článku.

1. V levém podokně rozbalte **SmallBakery.sdf** uzel a pak klikněte na tlačítko **tabulky**.
2. Klikněte pravým tlačítkem v tabulce produktu a pak klikněte na tlačítko **Data**.
3. V podokně upravit zadejte následující záznamy:

    | **Jméno** | **Popis** | **Cena** |
    | --- | --- | --- |
    | Chléb | Zaručená čerstvé každý den. | 2.99 |
    | Jahodová Shortcake | Z našich zahrada žádost s organickým jahody. | 9.99 |
    | Výsečový Apple | Druhý pouze na výseč vaší mom. | 12.99 |
    | Výsečový pecan | Pokud chcete Pekanové ořechy, je to pro vás. | 10.99 |
    | Citronový výseč | Vytvořené pomocí nejlepší citrony na světě. | 11.99 |
    | Cupcakes | Děti bude rádi tyto dětský ve službě. | 7.99 |

    Mějte na paměti, že nemáte nic pro zadejte *Id* sloupce. Při vytváření *Id* sloupce, nastavíte jeho **je Identity** vlastnost na hodnotu true, což způsobí, že se automaticky vyplní.

    Jakmile budete hotovi, zadávání dat, Návrháře tabulky bude vypadat například takto:

    ![[Obrázek]](5-working-with-data/_static/image3.jpg)
4. Zavřete kartu, která obsahuje data databáze.

## <a name="displaying-data-from-a-database"></a>Zobrazení dat z databáze

Jakmile máte databázi s daty v něm, můžete zobrazit data na webovou stránku ASP.NET. Pokud chcete vybrat řádky tabulky, které chcete zobrazit, pomocí příkazu SQL, který je příkaz, který je předat do databáze.

1. V levém podokně klikněte **soubory** pracovního prostoru.
2. V kořenovém adresáři webu vytvořte novou stránku CSHTML s názvem *ListProducts.cshtml*.
3. Nahraďte stávající značky s následujícími službami:

    [!code-cshtml[Main](5-working-with-data/samples/sample1.cshtml)]

    V prvním bloku kódu, otevřete *SmallBakery.sdf* soubor (databáze), který jste vytvořili dříve. `Database.Open` Metoda předpokládá, že *SDF* soubor je ve vašem webu *aplikace\_Data* složky. (Všimněte si, že nemusíte určit *SDF* rozšíření &#8212; v faktu, pokud tak učiníte, `Open` metoda nebude fungovat.)

    > [!NOTE]
    > *Aplikace\_Data* složka je speciální technologie ASP.NET, který se používá k ukládání dat souborů. Další informace najdete v tématu [připojení k databázi](#SB_ConnectingToADatabase) dále v tomto článku.

    Potom odešlete požadavek k dotazování databáze pomocí následující SQL `Select` příkaz:

    [!code-sql[Main](5-working-with-data/samples/sample2.sql)]

    V příkazu `Product` identifikuje tabulky pro dotaz. `*` Znak určuje, zda má dotaz vrátit všechny sloupce z tabulky. (Může být také seznam sloupců jednotlivě, oddělených čárkami, pokud jste chtěli jenom některé sloupce.) `Order By` Klauzuli Určuje, jak by měla být data seřazená &#8212; v tomto případě podle *název* sloupce. To znamená, že data jsou seřazená podle abecedy hodnotu *název* sloupec pro každý řádek.

    V těle stránky vytvoří kód HTML tabulku, která se použije k zobrazení dat. Uvnitř `<tbody>` elementu, můžete použít `foreach` smyčky jednotlivě získat každý řádek dat, který je vrácených dotazem. Pro každý řádek dat vytvoříte řádek tabulky HTML (`<tr>` element). Pak vytvoříte buněk tabulky HTML (`<td>` elementy) pro každý sloupec. Pokaždé, když přejdete pomocí smyčky, je k dispozici další řádek z databáze v `row` proměnné (můžete nastavit v `foreach` příkaz). Pokud chcete u jednotlivých sloupců z řádku, můžete použít `row.Name` nebo `row.Description` nebo jakoukoli název je sloupce mají.
4. Spusťte stránku v prohlížeči. (Ujistěte se, že je vybraný stránky v **soubory** pracovního prostoru, než ji spustit.) Na stránce se zobrazuje seznam takto:

    ![[Obrázek]](5-working-with-data/_static/image4.jpg)

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL je jazyk, který se používá ve většině relační databáze pro správu dat v databázi. Obsahuje příkazy, které umožňují data načíst a aktualizovat je, a které umožňují vytvářet, upravovat a spravovat databázových tabulek. SQL je jiný než programovací jazyk (jako je ten, že používáte ve službě WebMatrix) protože s SQL, cílem je, že oznámit databázi co chcete a je databáze úlohy, zjistěte, jak získat data nebo provést úlohu. Zde jsou příklady některých příkazů SQL a co dělat:
> 
> `SELECT Id, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> To načte *Id*, *název*, a *cena* sloupců z záznamy v *produktu* tabulky, pokud hodnota *cena* je více než 10 a vrátí výsledky v abecedním pořadí podle hodnot *název* sloupce. Tento příkaz vrátí je sada výsledků dotazu, který obsahuje záznamy, které splňují kritéria nebo prázdnou sadou pokud neodpovídají žádné záznamy.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ("Croissant", "A flaky delight", 1.99)`
> 
> Vloží nový záznam do *produktu* tabulky, nastavení *název* sloupec, který se &quot;Croissant&quot;, *popis* sloupec &quot; Nestabilním stavu štěstím&quot;a cena na 1,99.
> 
> `DELETE FROM Product WHERE ExpirationDate < "01/01/2008"`
> 
> Tento příkaz odstraní záznamy v *produktu* tabulku, jejíž sloupce s datem vypršení platnosti je starší než 1. ledna 2008. (Předpokladem je, který *produktu* tabulka obsahuje sloupec, samozřejmě.) Datum je zde uvedeno ve formátu MM/DD/RRRR, ale by měly být zadány ve formátu, který se používá pro národní prostředí.
> 
> `Insert Into` a `Delete` příkazy nevrací sad výsledků dotazu. Místo toho že budou vracet číslo informující o tom, kolik záznamů situace měla vliv na příkaz.
> 
> Proces, který požaduje operaci pro některé z těchto operací (např. vkládání a odstraňování záznamů), musí mít příslušná oprávnění v databázi. To je důvod, proč pro provozní databáze je často nutné zadat uživatelské jméno a heslo při připojení k databázi.
> 
> Existují desítky příkazy SQL, ale všechny se řídí vzor takto. Příkazy SQL můžete vytvářet tabulky databáze, počet záznamů v tabulce, výpočet ceny a provádět mnoho dalších operacích.


## <a name="inserting-data-in-a-database"></a>Vkládání dat v databázi

V této části ukazuje, jak vytvořit stránku, která umožňuje uživatelům přidání nového produktu na *produktu* databázové tabulky. Po vložení nového záznamu produktu, na stránce se zobrazí aktualizovaná tabulky pomocí *ListProducts.cshtml* stránku, kterou jste vytvořili v předchozí části.

Stránka obsahuje ověření a ujistěte se, zda je data, která uživatel zadá platný pro databázi. Například kódu na stránce je zajištěno, že byla zadána hodnota pro všechny požadované sloupce.

1. Na webu, vytvořte nový soubor CSHTML s názvem *InsertProducts.cshtml*.
2. Nahraďte stávající značky s následujícími službami:

    [!code-cshtml[Main](5-working-with-data/samples/sample3.cshtml)]

    Text stránky obsahuje formuláře HTML s tři textová pole, která umožní uživatelům zadat název, popis a cena. Když uživatelé kliknou na **vložit** tlačítko kód v horní části stránky otevře připojení k *SmallBakery.sdf* databáze. Potom získat hodnoty, které uživatel odeslal pomocí `Request` objektu a přiřaďte tyto hodnoty na místní proměnné.

    K ověření, že uživatel zadal hodnotu pro každý požadovaný sloupec, je každý zaregistrovat `<input>` element, který chcete ověřit:

    [!code-csharp[Main](5-working-with-data/samples/sample4.cs)]

    `Validation` Kontroluje pomocná rutina, že je v každé pole, které jste registrováni hodnota. Můžete zkontrolovat, zda všechna pole zdařilo ověření kontrolou `Validation.IsValid()`, což obvykle děláte před zpracovat informace získáte od uživatele:

    [!code-csharp[Main](5-working-with-data/samples/sample5.cs)]

    ( `&&` Znamená operátor AND – tento test je *Pokud je odeslání formuláře a všechna pole uplynulo ověření*.)

    Je-li ověřit všechny sloupce (žádné byly prázdné), pokračujte a vytvoření příkazu SQL pro vložení dat a potom spusťte jak ukazuje následující:

    [!code-csharp[Main](5-working-with-data/samples/sample6.cs)]

    Hodnoty k vložení, zahrnete parametr zástupné symboly (`@0`, `@1`, `@2`).

    > [!NOTE]
    > Jako bezpečnostní opatření vždy předejte hodnoty příkazu SQL pomocí parametrů, jak je uvedeno v předchozím příkladu. To vám dává možnost ověřit data uživatele a navíc pomáhá chránit před pokusy o odeslání škodlivý příkazy ke své databázi (někdy označované jako prostřednictvím injektáže SQL).

    K provedení dotazu, použijete tento příkaz předání proměnné, které obsahují hodnoty, které mají nahraďte zástupné symboly:

    [!code-csharp[Main](5-working-with-data/samples/sample7.cs)]

    Po `Insert Into` má spustit příkaz, odesílat uživatele na stránku, které jsou uvedeny produkty, které používají tento řádek:

    [!code-javascript[Main](5-working-with-data/samples/sample8.js)]

    Pokud ověření nebylo úspěšné, můžete přeskočit insert. Místo toho máte pomocné rutiny na stránce, která může zobrazovat Akumulovaná chybové zprávy (pokud existuje):

    [!code-cshtml[Main](5-working-with-data/samples/sample9.cshtml)]

    Všimněte si, že styl bloku do kódu obsahuje definice třídy CSS s názvem `.validation-summary-errors`. Toto je název třídy CSS, která se používá ve výchozím nastavení `<div>` elementu, který obsahuje všechny chyby ověření. V takovém případě třída CSS, která určuje, že souhrn chyb při ověřování se zobrazí červeně a tučně, ale můžete definovat `.validation-summary-errors` třída zobrazíte všechny formátování se vám líbí.

### <a name="testing-the-insert-page"></a>Testování stránce vložení

1. Zobrazte stránku v prohlížeči. Na stránce se zobrazí formulář, který je podobný ten, který je znázorněno na následujícím obrázku.

    ![[Obrázek]](5-working-with-data/_static/image5.jpg)
2. Zadejte hodnoty pro všechny sloupce, ale ujistěte se, že necháte *cena* sloupec prázdné.
3. Klikněte na tlačítko **vložit**. Na stránce zobrazuje chybovou zprávu, jak je znázorněno na následujícím obrázku. (Žádný nový záznam se vytvoří.)

    ![[Obrázek]](5-working-with-data/_static/image6.jpg)
4. Úplně vyplnění formuláře a potom klikněte na **vložit**. Tentokrát *ListProducts.cshtml* stránka se zobrazí a novém záznamu.

## <a name="updating-data-in-a-database"></a>Aktualizace dat v databázi

Po zadání dat do tabulky musíte může jej aktualizovat. Tento postup ukazuje, jak vytvořit dvě stránky, které jsou podobné ty, které jste vytvořili pro vložení dat dříve. První stránka zobrazí produkty a umožňuje uživateli vybrat jednu změnit. Druhá stránka umožňuje uživatelům ve skutečnosti proveďte úpravy a uložíte.

> [!NOTE] 
> 
> **Důležité** v produkční web, obvykle omezíte kdo má povoleno provádět změny v datech. Informace o tom, jak nastavit členství a o způsobech autorizaci uživatelů k provádění úloh v lokalitě najdete v tématu [členství na web webových stránek ASP.NET a přidání zabezpečení](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Na webu, vytvořte nový soubor CSHTML s názvem *EditProducts.cshtml*.
2. Nahraďte stávající kód v souboru s následujícími službami:

    [!code-cshtml[Main](5-working-with-data/samples/sample10.cshtml)]

    Jediným rozdílem mezi této stránce a *ListProducts.cshtml* stránky z dříve je, že tabulka HTML na této stránce obsahuje další sloupec, který zobrazuje **upravit** odkaz. Když kliknete na tento odkaz, dojde k *UpdateProducts.cshtml* stránky (což vytvoříte další) kde můžete upravit vybraný záznam.

    Podívejte se na kód, který vytvoří **upravit** odkaz:

    [!code-cshtml[Main](5-working-with-data/samples/sample11.cshtml)]

    Tím se vytvoří HTML `<a>` element jejichž `href` dynamicky nastavený atribut. `href` Určuje atribut, na stránce zobrazit, když uživatel klikne na odkaz. Také předá `Id` hodnotu na aktuálním řádku na odkaz. Při spuštění stránky zdroji stránky může obsahovat třeba tyto odkazy:

    [!code-html[Main](5-working-with-data/samples/sample12.html)]

    Všimněte si, že `href` je atribut nastaven na `UpdateProducts/n`, kde *n* je číslo produktu. Když uživatel klikne na jeden z těchto odkazů, Výsledná adresa URL bude vypadat přibližně takto:

    `http://localhost:18816/UpdateProducts/6`

    Úpravy čísla produktu, jinými slovy, budou předány v adrese URL.
3. Zobrazte stránku v prohlížeči. Stránka zobrazí data ve formátu, například takto:

    ![[Obrázek]](5-working-with-data/_static/image7.jpg)

    V dalším kroku vytvoříte stránky, která umožňuje uživatelům ve skutečnosti aktualizovat data. Stránka pro aktualizaci zahrnuje ověření ověřit data, která uživatel zadá. Například kódu na stránce je zajištěno, že byla zadána hodnota pro všechny požadované sloupce.
4. Na webu, vytvořte nový soubor CSHTML s názvem *UpdateProducts.cshtml*.
5. Nahraďte existující kód v souboru následující.

    [!code-cshtml[Main](5-working-with-data/samples/sample13.cshtml)]

    Text stránky obsahuje formuláře HTML, kde se zobrazí produktu a kde mohou uživatelé upravovat je. Získat zobrazení produktu, použijte tento příkaz SQL:

    [!code-sql[Main](5-working-with-data/samples/sample14.sql)]

    To bude vyberte produkt, jehož ID odpovídá hodnotě, je předaná `@0` parametr. (Protože *Id* primární klíč, a proto musí být jedinečný, pouze jeden produkt záznam někdy lze vybrat tímto způsobem.) K získání hodnoty ID předat to `Select` příkazu si můžete přečíst hodnotu, která je předána na stránku jako část adresy URL, pomocí následující syntaxe:

    [!code-csharp[Main](5-working-with-data/samples/sample15.cs)]

    Ve skutečnosti načíst záznam produktu, můžete použít `QuerySingle` metoda, která budou vracet jenom jeden záznam:

    [!code-csharp[Main](5-working-with-data/samples/sample16.cs)]

    Jeden řádek je vrácen do `row` proměnné. Můžete získat data z jednotlivých sloupců a přiřaďte ho na místní proměnné takto:

    [!code-csharp[Main](5-working-with-data/samples/sample17.cs)]

    Do kódu pro daný formulář tyto hodnoty se zobrazí automaticky do jednotlivých polí pomocí vloženého kódu takto:

    [!code-html[Main](5-working-with-data/samples/sample18.html)]

    Část kód zobrazí záznam produktu, který chcete aktualizovat. Jakmile záznamu byla zobrazena, můžete upravit uživatele jednotlivé sloupce.

    Když uživatel odešle formulář kliknutím **aktualizace** tlačítko kód `if(IsPost)` blokovat spuštění. To získá hodnoty uživatele z `Request` objekt, ukládá hodnoty v seznamu proměnných a ověří, že každý sloupec má vyplnit. V případě úspěšného ověření kód vytvoří následující příkaz SQL Update:

    [!code-sql[Main](5-working-with-data/samples/sample19.sql)]

    V SQL `Update` příkazu, zadejte k aktualizaci a hodnota k nastavení každý sloupec. V tomto kódu jsou hodnoty zadané použití parametru zástupné symboly `@0`, `@1`, `@2`a tak dále. (Jak jsme uvedli dříve, zabezpečení, by měla vždycky předáte hodnoty příkazu SQL pomocí parametrů.)

    Při volání `db.Execute` metoda, předáte proměnné, které obsahují hodnoty v pořadí, která odpovídá parametry v příkazu SQL:

    [!code-csharp[Main](5-working-with-data/samples/sample20.cs)]

    Po `Update` příkaz provedl, se volá metodu přesměruje uživatele zpět na stránku upravit:

    [!code-cshtml[Main](5-working-with-data/samples/sample21.cshtml)]

    Efekt je, že uživatel uvidí aktualizovaný seznam dat v databázi a můžete upravit jiného produktu.
6. Uložte stránky.
7. Spustit *EditProducts.cshtml* stránku (není stránka pro aktualizaci) a pak klikněte na tlačítko **upravit** vyberte chcete upravit. *UpdateProducts.cshtml* se zobrazí stránka zobrazující záznam, který jste vybrali.

    ![[Obrázek]](5-working-with-data/_static/image8.jpg)
8. Provedení změny a klikněte na tlačítko **aktualizace**. Seznam produktů se znovu zobrazí s aktualizovaná data.

## <a name="deleting-data-in-a-database"></a>Odstraňování dat v databázi

V této části ukazuje, jak uživatelům odstranit z produktu *produktu* databázové tabulky. V příkladu se skládá z dvě stránky. Na první stránce vyberte uživatelé záznam odstranit. Odstranit záznam se následně zobrazí na druhé stránce, která umožňuje jejich potvrďte, že chcete odstranit záznam.

> [!NOTE] 
> 
> **Důležité** v produkční web, obvykle omezíte kdo má povoleno provádět změny v datech. Informace o tom, jak nastavit členství a o způsobech autorizovat uživatele k provádění úloh v lokalitě najdete v tématu [členství na web webových stránek ASP.NET a přidání zabezpečení](https://go.microsoft.com/fwlink/?LinkId=202904).


1. Na webu, vytvořte nový soubor CSHTML s názvem *ListProductsForDelete.cshtml*.
2. Nahraďte stávající značky s následujícími službami:

    [!code-cshtml[Main](5-working-with-data/samples/sample22.cshtml)]

    Tato stránka je podobná *EditProducts.cshtml* stránky z dříve. Však místo **upravit** odkaz pro každý produkt, zobrazuje **odstranit** odkaz. **Odstranit** připojení se vytvoří pomocí následujícího kódu vloženého do kódu:

    [!code-cshtml[Main](5-working-with-data/samples/sample23.cshtml)]

    Tím se vytvoří adresa URL, když uživatelé kliknou na odkaz vypadat třeba takto:

    `http://<server>/DeleteProduct/4`

    Adresa URL volá stránku s názvem *DeleteProduct.cshtml* (aplikaci, kterou vytvoříte další) a předává je ID produktu, který se má odstranit (tady 4).
3. Uložte soubor, ale nechat otevřené.
4. Vytvořte další CHTML soubor s názvem *DeleteProduct.cshtml*. Nahradí existující obsah s následujícími službami:

    [!code-cshtml[Main](5-working-with-data/samples/sample24.cshtml)]

    Tato stránka je volána metodou *ListProductsForDelete.cshtml* a umožňuje uživatelům potvrďte, že chcete odstranit produktu. Pro zobrazení seznamu produktu, který má být odstraněn, můžete získat ID produktu, který se má odstranit z adresy URL pomocí následujícího kódu:

    [!code-csharp[Main](5-working-with-data/samples/sample25.cs)]

    Stránky pak požádá uživatele, klikněte na tlačítko se ve skutečnosti odstranit záznam. To je důležitým bezpečnostním opatřením: při provádění citlivých operací ve vašem webu, jako je aktualizace nebo odstranění dat, musí tyto operace vždy provést pomocí operace POST, Ne operaci GET. Pokud váš web je nastaven tak, aby operace odstranění je možné provést pomocí operace GET, každý, kdo může předat adresu URL podobnou `http://<server>/DeleteProduct/4` a nic chtějí z databáze odstranit. Přidáním potvrzení a kódování stránky tak, aby odstranění lze provést pouze na základě POST, přidat míru zabezpečení na váš web.

    Operace skutečného odstranění se provádí pomocí následující kód, který nejdřív potvrdí, že se jedná o operaci post a že není Identifikátor prázdný:

    [!code-csharp[Main](5-working-with-data/samples/sample26.cs)]

    Příkaz SQL, který odstraní zadaný záznam a pak přesměruje uživatele zpět na stránku výpis spuštění kódu.
5. Spustit *ListProductsForDelete.cshtml* v prohlížeči.

    ![[Obrázek]](5-working-with-data/_static/image9.jpg)
6. Klikněte **odstranit** odkaz pro jeden z produktů. *DeleteProduct.cshtml* zobrazí se stránka pro potvrzení, že chcete odstranit tento záznam.
7. Klikněte **odstranit** tlačítko. Odstranění záznamu produktu a aktualizaci stránky k výpisu aktualizované produktu.

> [!TIP]
> 
> <a id="SB_ConnectingToADatabase"></a>
> ### <a name="connecting-to-a-database"></a>Připojení k databázi
> 
> Můžete připojit k databázi dvěma způsoby. První je použití `Database.Open` metoda a zadejte název souboru databáze (méně *SDF* rozšíření):
> 
> `var db = Database.Open("SmallBakery");`
> 
> `Open` Metoda předpokládá, že. *SDF* soubor je na webu *aplikace\_Data* složky. Tato složka je určený speciálně pro ukládání dat. Například má příslušná oprávnění k tomu, aby web ke čtení a zápisu dat a jako bezpečnostní opatření, služba WebMatrix neumožňuje přístup k souborům z této složky.
> 
> Druhý způsob je použít připojovací řetězec. Připojovací řetězec obsahuje informace o tom, jak připojit k databázi. To může zahrnovat cestu k souboru nebo může obsahovat název databáze systému SQL Server na místním nebo vzdáleném serveru, spolu s uživatelské jméno a heslo pro připojení k tomuto serveru. (Pokud data uchováváte v centrálně řízené verzi systému SQL Server, například v lokalitě poskytovatele hostingu, vždy používáte připojovací řetězec k zadání informací o připojení databáze.)
> 
> Ve službě WebMatrix, připojovací řetězce jsou obvykle se uloží do souboru XML s názvem *Web.config*. Jak již název napovídá, můžete použít *Web.config* soubor v kořenovém adresáři vašeho webu k uložení informace o konfiguraci webu, včetně libovolné púřipojovací řetězce, které mohou vyžadovat váš web. Příklad připojovacího řetězce v *Web.config* souboru může vypadat třeba takto:
> 
> [!code-xml[Main](5-working-with-data/samples/sample27.xml)]
> 
> V příkladu připojovací řetězec odkazuje na databázi v instanci systému SQL Server, který běží na serveru někde (oproti místní *SDF* souboru). By bylo potřeba nahradit názvy vhodné pro `myServer` a `myDatabase`a zadejte hodnoty přihlášení serveru SQL Server pro `username` a `password`. (Hodnoty uživatelské jméno a heslo, nemusí nutně být stejné jako pověření systému Windows nebo jako hodnoty, které poskytovatele hostingu udělil pro přihlašování na jejich serverech. Obraťte se na správce pro přesné hodnoty, které potřebujete.)
> 
> `Database.Open` Metoda je flexibilní, protože vám umožňuje předat název databáze *SDF* název připojovacího řetězce, který je uložený v souboru nebo *Web.config* souboru. Následující příklad ukazuje, jak se připojit k databázi pomocí připojovacího řetězce zobrazené v předchozím příkladu:
> 
> [!code-cshtml[Main](5-working-with-data/samples/sample28.cshtml)]
> 
> Jak jsme uvedli, `Database.Open` metoda umožňuje předat název databáze nebo připojovací řetězec a budete-li zjistit, který se použije. To je užitečné, když nasazujete (publikovat) vašeho webu. Můžete použít *SDF* v soubor *aplikace\_Data* složku, když jste vývoj a testování vaší lokality. Když přesouváte vaše lokality na provozním serveru, můžete použít připojovací řetězec *Web.config* soubor, který má stejný název jako vaše *SDF* souboru, ale, že body k poskytovateli hostingu databáze &#8212;všechny bez nutnosti měnit kód.
> 
> Nakonec, pokud chcete pracovat přímo s připojovacím řetězcem, můžete zavolat `Database.OpenConnectionString` metoda a předejte jí ho skutečné připojovací řetězec, nikoli jen název v *Web.config* souboru. To může být užitečné v situacích, kde z nějakého důvodu nemáte přístup do připojovacího řetězce (nebo hodnoty v něm, jako *SDF* název souboru) dokud běží stránky. Pro většinu scénářů, ale můžete použít `Database.Open` jak je popsáno v tomto článku.


## <a name="additional-resources"></a>Další prostředky

- [SQL Server Compact](https://www.microsoft.com/sqlserver/2008/en/us/compact.aspx)
- [Připojení k systému SQL Server nebo databáze MySQL ve službě WebMatrix](https://go.microsoft.com/fwlink/?LinkId=208661)
- [Ověřování uživatelských vstupů na webech s webovými stránkami ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
