---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Co je nového v rozhraní Entity Framework 4.0 | Dokumentace Microsoftu
author: tdykstra
description: V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila Začínáme s Entity Framework 4.0 série kurzů. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 402e7ace1abad899d32ed179d6b68de4e5a129f5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755702"
---
<a name="whats-new-in-the-entity-framework-40"></a>Co je nového v rozhraní Entity Framework 4.0
====================
podle [Petr Dykstra](https://github.com/tdykstra)

> V této sérii kurzů staví na Contoso University webovou aplikaci, která se vytvořila [Začínáme s Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) série kurzů. Pokud nebyla dokončena v předchozích kurzech, jako výchozí bod pro účely tohoto kurzu můžete [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stáhnout aplikaci](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , který vytvoří kompletní série kurzů. Pokud máte dotazy týkající se těchto kurzů, můžete je publikovat [fórum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


V předchozím kurzu jste viděli některé metody pro dosažení maximálního výkonu webové aplikace, která používá Entity Framework. V tomto kurzu zkontroluje některé z vašich nejdůležitějších nové funkce ve verzi 4 nástroje Entity Framework a odkazy na prostředky, které poskytují podrobnější Úvod do všech nových funkcí. Funkce zvýrazní v tomto kurzu, patří:

- Cizí klíč přidružení.
- Provádění příkazů SQL definovaný uživatelem.
- Vývoj s včasným modelu.
- Podpora POCO.

Kromě toho kurzu stručně představí *založeno na kódu vývoj*, funkce, která se chystá v další vydané verzi rozhraní Entity Framework.

Zahájit kurz, spusťte sadu Visual Studio a otevřete webové aplikace Contoso University, kterým jste pracovali v předchozím kurzu.

## <a name="foreign-key-associations"></a>Přidružení cizího klíče

Entity Framework verze 3.5 zahrnuta navigační vlastnosti, ale neobsahoval vlastnosti cizího klíče v datovém modelu. Například `CourseID` a `StudentID` sloupce `StudentGrade` tabulky by vynechává `StudentGrade` entity.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Důvod pro tento přístup se, přesněji řečeno, cizí klíče jsou fyzická implementace podrobností a nepatří do koncepční datový model. Ale prakticky, často je jednodušší pracovat s entitami v kódu, až budete mít přímý přístup k cizím klíčům.

Pro příklad jak cizí klíče v datovém modelu můžete zjednodušit kód, zvažte, jak by jste museli jste kód *DepartmentsAdd.aspx* stránky bez nich. V `Department` entity, `Administrator` vlastnost je cizí klíč, který odpovídá `PersonID` v `Person` entity. Aby bylo možné navázat spojení mezi nové oddělení a jeho správce, vše museli dělat byla nastavena hodnota `Administrator` vlastnost `ItemInserting` ovládací prvek databound obslužná rutina události:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Bez cizí klíče v datovém modelu zpracování `Inserting` události ovládacího prvku zdroje dat, nikoli `ItemInserting` událostí datové vazby ovládacího prvku, pokud chcete získat odkaz na entitu, samotný před přidáním entity do sady entit. Až budete mít tento odkaz, je vytvořit přidružení, jako je například pomocí kódu v následujících příkladech:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Jak je vidět v Entity Framework týmu [blogový příspěvek o přidružení cizího klíče](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), existují jiné případech, kdy je mnohem větší rozdíl v složitost kódu. Podle potřeb z těch, kteří dávají přednost smířit podrobnosti implementace v rámcové datový model pro účely jednodušší kódu, Entity Framework teď nabízí možnost, včetně cizí klíče v datovém modelu.

V terminologii Entity Framework, pokud zahrnete cizí klíče v datovém modelu používáte *přidružení cizího klíče*, a pokud používáte cizí klíče vyloučíte *nezávislé přidružení*.

## <a name="executing-user-defined-sql-commands"></a>Provádění příkazů SQL definovaný uživatelem

V dřívějších verzích rozhraní Entity Framework byla jednoduchý způsob, jak vytvářet vlastní příkazy SQL v reálném čase a spouštět je. Entity Framework generuje dynamicky příkazů SQL, které pro vás, nebo jste měli k vytvoření uložené procedury a importujete ho jako funkce. Verze 4 přidá `ExecuteStoreQuery` a `ExecuteStoreCommand` metody `ObjectContext` třídu, která usnadňují předat libovolný dotaz přímo do databáze.

Předpokládejme, že správce společnosti Contoso University chtějí mít možnost k provádění hromadných změn v databázi bez nutnosti projít procesem vytvoření uložené procedury a naimportujete do datového modelu. Jejich první požadavek je pro stránku, která umožňuje měnit počet kredity pro všechny kurzy v databázi. Na webové stránce, které chtějí moct zadat číslo k použití pro vynásobení hodnoty z každé `Course` řádku `Credits` sloupce.

Vytvoří novou stránku, která používá *Site.Master* stránku předlohy a pojmenujte ho *UpdateCredits.aspx*. Pak přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Tento kód vytvoří `TextBox` ovládací prvek, ve kterém může uživatel zadat hodnotu multiplikátor `Button` za účelem provedení příkazu, klikněte na ovládací prvek a `Label` ovládací prvek pro označující počet ovlivněných řádků.

Otevřít *UpdateCredits.aspx.cs*a přidejte následující `using` příkazu a obslužné rutiny pro tlačítka `Click` události:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Tento kód se vykoná SQL `Update` příkaz pomocí hodnoty do textového pole a používá název zobrazíte počet ovlivněných řádků. Než spustíte stránku, *Courses.aspx* stránku, aby získali "před" obrázek nějaká data.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Spustit *UpdateCredits.aspx*zadejte "číslo 10" jako násobitel a potom klikněte na **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Spustit *Courses.aspx* stránku znovu, abyste viděli změněná data.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Pokud chcete nastavit počet kredity zpět na původní hodnoty, v *UpdateCredits.aspx.cs* změnit `Credits * {0}` k `Credits / {0}` a znovu spusťte na stránce zadání 10 jako dělitel.)

Další informace o provádění dotazů, které definujete v kódu, naleznete v tématu [jak: přímo spouštět příkazy pro zdroje dat](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Vývoj s včasným modelu

V těchto kurzech nejprve vytvořit databázi a pak datový model založený na struktuře databáze vygenerován. V rozhraní Entity Framework 4 můžete místo toho začít s datovým modelem a vytvořit databázi na základě struktury dat modelu. Pokud vytváříte aplikaci, pro kterou databázi ještě neexistuje, model-first přístup umožňuje vytvářet entity a vztahy, které dávají smysl pro aplikaci, při není byste se museli starat o podrobnosti fyzická implementace koncepčně . (Toto bude platit pořád pouze prostřednictvím v počátečních fázích vývoje, ale. Nakonec databáze se vytvoří a bude mít produkčních dat v něm a znovu ji vytvořit z modelu již nebudou praktické; v tomto okamžiku budete zpět do databáze první přístup.)

V této části kurzu vytvoříte jednoduchý datový model a generovat databáze z něj.

V **Průzkumníka řešení**, klikněte pravým tlačítkem myši *DAL* a pak zvolte položku **přidat novou položku**. V **přidat novou položku** dialogovém okně **nainstalované šablony** vyberte **Data** a pak vyberte **datový Model Entity ADO.NET** šablony . Pojmenujte nový soubor *AlumniAssociationModel.edmx* a klikněte na tlačítko **přidat**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Otevře se Průvodce datovým modelem Entity. V **výběr obsahu modelu** kroku, vyberte **prázdný Model** a potom klikněte na tlačítko **Dokončit**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer** otevře s prázdnou návrhovou plochu. Přetáhněte **Entity** položky z **nástrojů** na návrhovou plochu.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Změnit název entity z `Entity1` k `Alumnus`, změnit `Id` název vlastnosti pro `AlumnusId`a přidejte nový skalární vlastnost s názvem `Name`. Přidání nových vlastností stisknutí klávesy Enter po změně názvu `Id` sloupce, nebo klikněte pravým tlačítkem na entitu a vyberte **přidat skalární vlastnost**. Výchozí typ pro nové vlastnosti je `String`, což je v pořádku pro tuto jednoduchou ukázku, ale samozřejmě můžete změnit věci, jako je datový typ ve **vlastnosti** okna.

Vytvořte jinou entitou stejným způsobem jako s názvem `Donation`. Změnit `Id` vlastnost `DonationId` a přidejte skalární vlastnost s názvem `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Chcete-li přidat přidružení mezi těmito dvěma entitami, klikněte pravým tlačítkem `Alumnus` entity, vyberte **přidat**a pak vyberte **přidružení**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Výchozí hodnoty v **Přidat přidružení** dialogové okno se, co chcete (jeden na mnoho, zahrnují vlastnosti navigace, zahrnují cizí klíče), takže stačí kliknout na **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Návrhář přidá Asociační čára a vlastnosti cizího klíče.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Nyní jste připraveni vytvořit databázi. Klikněte pravým tlačítkem na návrhové ploše a vyberte **generovat databáze z modelu**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Tím se spustí průvodce vytvořit databáze. (Pokud se zobrazí upozornění, která označuje, že nejsou namapovány entity, můžete ignorovat ty prozatím.)

V **vyberte datové připojení** krok, klikněte na tlačítko **nové připojení**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

V **vlastnosti připojení** dialogové okno pole, vyberte místní instanci systému SQL Server Express a název databáze `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Klikněte na tlačítko **Ano** Pokud se dotaz, pokud chcete vytvořit databázi. Když **vyberte datové připojení** kroku se zobrazí znovu, klikněte na tlačítko **Další**.

V **Summary a nastavení** krok, klikněte na tlačítko **Dokončit**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql* se vytvoří soubor s příkazy jazyka DDL definice dat, ale ještě nejsou spuštěny příkazy.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Použijte nástroj, jako **SQL Server Management Studio** ke spuštění skriptu a vytvoření tabulky, protože to může být způsobeno při vytváření `School` databázi [z prvního kurzu Začínáme série kurzů ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Pokud jste si stáhli v databázi.)

Teď můžete použít `AlumniAssociation` datového modelu ve své webové stránky stejně jako jste používali `School` modelu. Pokud to pokud chcete vyzkoušet si, přidejte data do tabulek a vytvořte webovou stránku, která zobrazí data.

Pomocí **Průzkumníka serveru**, přidejte následující řádky, které mají `Alumnus` a `Donation` tabulky.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Vytvoření nové webové stránky s názvem *Alumni.aspx* , která používá *Site.Master* stránky předlohy. Přidejte následující kód k `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Tento kód vytvoří vnořené `GridView` Určuje vnější jeden zobrazení názvů absolventů a vnitřní jeden k zobrazení odběru a částky.

Otevřít *Alumni.aspx.cs*. Přidat `using` přístup k příkazu pro data, vrstvy a obslužné rutiny pro vnější `GridView` ovládacího prvku `RowDataBound` události:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Tento kód databinds vnitřní `GridView` kontrolovat pomocí `Donations` navigační vlastnost aktuálního řádku `Alumnus` entity.

Spuštění stránky.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Poznámka: Tato stránka je součástí ke stažení projektu, ale aby to fungovalo, je nutné vytvořit databázi v místní instanci systému SQL Server Express; není zahrnut jako databázi *.mdf* ve *aplikace\_ Data* složky.)

Další informace o použití funkce zaměřené na modelu Entity Framework naleznete v tématu [Model-First v rozhraní Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Podpora objektů POCO

Při použití metody návrhu řízeného doménou navrhnete datové třídy, které představují data a chování, které se týkají obchodní domény. Tyto třídy měly být nezávislé na žádné konkrétní technologie používaná k ukládání (zachovat) dat; jinými slovy, by měl být *trvalost ignorant*. Trvalost neznalosti lze také snadněji třídu testu jednotek vzhledem k tomu, že projekt jednotkového testu můžete použít jakýkoli trvalost technologie je nejvhodnější pro testování. Starší verze rozhraní Entity Framework nabízí omezenou podporu pro trvalost neznalosti, protože dědění z tříd entit `EntityObject` třídy a proto zahrnuté spoustu funkce specifická pro Entity Framework.

Rozhraní Entity Framework 4 zavádí možnost používat tříd entit, které Nedědit z `EntityObject` třídy a proto jsou ignorant trvalosti. V souvislosti s Entity Framework se obvykle nazývá třídy takto *objekty CLR prostý staré* (POCO nebo POCOs). Třídy POCO můžete napsat ručně nebo můžete automaticky vygenerovat je založen na existující model dat pomocí nástrojů transformace šablony textu (T4) šablony, které poskytuje rozhraní Entity Framework.

Další informace o používání POCOs v Entity Framework naleznete v následující prostředky:

- [Práce s entitami POCO](https://msdn.microsoft.com/library/dd456853.aspx). Toto je dokumentu MSDN, který je uveden přehled POCOs, s odkazy na další dokumenty, které mít podrobnější informace.
- [Návod: Objektů POCO šablony pro Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) je to blogový příspěvek od týmu vývoje rozhraní Entity Framework, s odkazy na další blogové příspěvky o POCOs.

## <a name="code-first-development"></a>Vývoj s včasným kódu

Podpora POCO v Entity Framework 4 nadále vyžaduje vytvoření datového modelu a propojení vašich tříd entit do datového modelu. Další verze rozhraní Entity Framework bude obsahovat funkci s názvem *založeno na kódu vývoj*. Tato funkce umožňuje používat Entity Framework pomocí vlastní třídy POCO bez nutnosti použití návrháře modelů data nebo soubor XML datového modelu. (Tedy tato možnost také byla volána *pouze kód*; *založeno na kódu* a *pouze kód* odkazují na stejnou funkci Entity Framework.)

Další informace o používání založeno na kódu přístup k vývoji naleznete na následujících odkazech:

- [Založeno na kódu vývoj pomocí rozhraní Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Toto je blogový příspěvek Scotta Guthrieho představení vývoje code first.
- [Blog týmu pro Entity Framework vývoj - příspěvků označených CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog týmu pro Entity Framework vývoj - příspěvků označených Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Kurz pro aplikace MVC Music Store – část 4: modely a přístup k datům](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Začínáme s MVC 3 - 4. část: Entity Framework Code First vývoj](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Kromě toho je nový založeno na kódu MVC kurz, který vytváří aplikaci, podobně jako u aplikace Contoso University plánovaných zveřejnit udělali na jaře roku 2011 v [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Další informace

Dokončení tohoto postupu si přehled, abyste co je nového v rozhraní Entity Framework a tato operace bude pokračovat s Entity Framework série kurzů. Další informace o nových funkcích v rozhraní Entity Framework 4, které nejsou součástí tohoto najdete v následujících zdrojích:

- [Co je nového v ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) tématu MSDN na nové funkce ve verzi 4 nástroje Entity Framework.
- [Oznamujeme vydání sady Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) rozhraní Entity Framework vývojový tým blogový příspěvek o nové funkce ve verzi 4.

> [!div class="step-by-step"]
> [Předchozí](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
