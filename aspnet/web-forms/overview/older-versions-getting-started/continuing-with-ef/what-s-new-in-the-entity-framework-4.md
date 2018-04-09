---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Co je nového v rozhraní Entity Framework 4.0 | Microsoft Docs
author: tdykstra
description: Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený Začínáme s řadou kurz Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-the-entity-framework-40"></a>Co je nového v rozhraní Entity Framework 4.0
====================
Podle [tní Dykstra](https://github.com/tdykstra)

> Tento kurz řady staví na webové aplikace Contoso univerzity, který byl vytvořený [Začínáme s platformou Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) kurz řady. Pokud nebyla dokončena starší kurzy, jako výchozí bod pro účely tohoto kurzu můžete [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , kterou by jste vytvořili. Můžete také [stažení aplikace](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) vytvořené dokončení kurzu řady. Pokud máte dotazy týkající se kurzy, můžete je do příspěvku [ASP.NET Entity Framework fórum](https://forums.asp.net/1227.aspx).


V předchozích kurzu jste viděli některé metody pro tím se maximalizuje výkon webové aplikace, která používá rozhraní Entity Framework. V tomto kurzu zkontroluje některé z vašich nejdůležitějších nových funkcí v Entity Framework verze 4 a obsahuje odkazy na prostředky, které obsahují podrobnější informace o všech nových funkcí. Následující funkce zvýrazněných v tomto kurzu:

- Přidružení cizí klíč.
- Provádění příkazů SQL definovaný uživatelem.
- Vývoj s včasným modelu.
- Podpora objektů POCO.

Kromě toho tento kurz vás seznámí stručně *kód – první vývoj*, funkce, která pochází v další verzi rozhraní Entity Framework.

Pokud chcete spustit tento kurz, spuštění sady Visual Studio a otevřete Contoso univerzity webovou aplikaci, kterou jste pracovali v předchozí kurzu.

## <a name="foreign-key-associations"></a>Přidružení cizího klíče

Rozhraní Entity Framework verze 3.5 zahrnuté navigační vlastnosti, ale nezahrnuli vlastnosti cizího klíče v datovém modelu. Například `CourseID` a `StudentID` sloupce `StudentGrade` tabulky by tento parametr vynechán z `StudentGrade` entity.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Důvody, proč tento přístup se, že přesněji řečeno, cizí klíče jsou fyzické implementace podrobností a nepatří do konceptuálního datový model. Ale prakticky, je často snazší práce s entitami v kódu, až budete mít přímý přístup k cizí klíče.

Pro příklad, jak cizích klíčů v datovém modelu můžete zjednodušit kód, zvažte, jak jste by předtím kódu *DepartmentsAdd.aspx* stránku bez nich. V `Department` entity, `Administrator` vlastnost je cizí klíč, který odpovídá `PersonID` v `Person` entity. Za účelem vytvoření přidružení mezi nové oddělení a správce, všechny jste museli provést byla nastavena hodnota `Administrator` vlastnost `ItemInserting` obslužné rutiny události ovládacího prvku Vycentrovat:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Bez cizí klíče v datovém modelu zpracování `Inserting` události ovládacího prvku zdroje dat místo `ItemInserting` události ovládacího prvku vycentrovat, aby bylo možné získat odkaz na samotné entity než entity je přidat do sady entit. Až budete mít tento odkaz, můžete vytvořit přidružení, jako je například pomocí kódu v následujících příkladech:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Jak vidíte v týmu rozhraní Entity Framework [příspěvku na blogu o přidružení cizího klíče](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), existují další případech, kdy je mnohem větší rozdíl ve složitost kódu. Ke splnění potřeb uživatelů, kteří přednost za provozu s podrobnosti implementace v konceptuální datový model z důvodu jednodušší kódu, rozhraní Entity Framework nyní vám dává možnost včetně cizí klíče v datovém modelu.

V terminologii rozhraní Entity Framework, pokud zahrnete cizí klíče v datovém modelu používáte *přidružení cizího klíče*, a pokud používáte cizí klíče vyloučíte *nezávislá přidružení*.

## <a name="executing-user-defined-sql-commands"></a>Spuštění uživatelem definované SQL příkazy

V dřívějších verzích rozhraní Entity Framework se žádný snadný způsob, jak vytvořit vlastní příkazy SQL za chodu a jejich spuštění. Buď rozhraní Entity Framework pro vás dynamicky vygeneroval příkazy SQL, nebo bylo nutné vytvořit uložené procedury a importujte ji jako funkce. Přidá verze 4 `ExecuteStoreQuery` a `ExecuteStoreCommand` metody `ObjectContext` třídu, která usnadnili předat jakýkoli dotaz přímo do databáze.

Předpokládejme, že správci Contoso univerzity chcete být schopni provést hromadně změny v databázi bez nutnosti přejít provede procesem vytváření uložené procedury a jeho import do datového modelu. Jejich první požadavek je pro stránky, který umožňuje změnit počet kredity u všech kurzů v databázi. Na webové stránce, chtějí moct zadat číslo k použití mají vynásobit hodnotu každých `Course` řádku `Credits` sloupce.

Vytvořte novou stránku, která používá *Site.Master* hlavní stránky a pojmenujte ji *UpdateCredits.aspx*. Pak přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Tento kód vytvoří `TextBox` řízení, ve kterém může uživatel zadat hodnotě násobitel `Button` řízení k provedení příkazu, klikněte na a `Label` řízení pro určující počet ovlivněných řádků.

Otevřete *UpdateCredits.aspx.cs*a přidejte následující `using` prohlášení a obslužnou rutinu pro tlačítka `Click` událostí:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Tento kód provede SQL `Update` příkaz pomocí hodnoty do textového pole a pomocí popisek zobrazí počet ovlivněných řádků. Než spustíte stránky, spusťte *Courses.aspx* stránku a získat přehled "před" některá data.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Spustit *UpdateCredits.aspx*, zadejte jako násobitel "10" a pak klikněte na tlačítko **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Spustit *Courses.aspx* stránky znovu zobrazíte změněná data.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Pokud chcete nastavit počet kredity zpět na původní hodnoty, v *UpdateCredits.aspx.cs* změnit `Credits * {0}` k `Credits / {0}` a znovu spusťte stránce zadání 10 jako dělitel.)

Další informace o provádění dotazů, které definujete v kódu najdete v tématu [postup: přímo spustit příkazy proti Data Source](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Vývoj s včasným modelu

V těchto kurzech nejprve vytvořit databázi a poté generuje datový model na základě struktury databáze. V rozhraní Entity Framework 4 můžete místo toho začínat datový model a generovat databázi podle strukturu datového modelu. Pokud vytváříte aplikace, pro který databáze ještě neexistuje, první model přístup umožňuje vytvořit entity a vztahy, které dávají smysl koncepčně pro aplikaci, při není znalosti fyzické implementace podrobností . (Toto zůstává true pouze prostřednictvím počáteční fáze vývoje, ale. Nakonec databáze se vytvoří a bude mít provozními daty v něm a jeho opětné vytvoření z modelu bude praktické; v tomto okamžiku budete zpět na první databáze přístup.)

V této části kurzu vytvoříte jednoduchý datový model a generovat databázi z něj.

V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *DAL* složky a vyberte **přidat novou položku**. V **přidat novou položku** dialogovém **nainstalovaných šablonách** vyberte **Data** a pak vyberte **ADO.NET Entity Data Model** šablony . Název nového souboru *AlumniAssociationModel.edmx* a klikněte na tlačítko **přidat**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Spustí se průvodce Entity Data Model. V **zvolte obsah modelu** krok, vyberte **prázdný Model** a pak klikněte na **Dokončit**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Entity Data Model Designer** otevře s prázdným návrhovou plochu. Přetáhněte **Entity** položky z **sada nástrojů** na návrhovou plochu.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Změňte název entity z `Entity1` k `Alumnus`, změnit `Id` název vlastnosti pro `AlumnusId`a přidejte novou skalární vlastnost s názvem `Name`. K přidání nových vlastností stisknutí klávesy Enter po změně názvu `Id` sloupce, nebo klikněte pravým tlačítkem na entity a vyberte **přidat vlastnost Scalar**. Je výchozím typem pro nové vlastnosti `String`, která je vhodná pro tento ukázkový jednoduché, ale samozřejmě můžete změnit takové věci, jako datový typ v **vlastnosti** okno.

Vytvoření stejným způsobem jako jiné entity a pojmenujte ji `Donation`. Změna `Id` vlastnost `DonationId` a přidejte skalární vlastnost s názvem `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Chcete-li přidat přidružení mezi tyto dvě entity, klikněte pravým tlačítkem `Alumnus` entity, vyberte **přidat**a potom vyberte **přidružení**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Výchozí hodnoty v **Přidat přidružení** dialogové okno se, co chcete použít (jeden mnoho, zahrnují navigační vlastnosti, zahrnují cizí klíče), takže jednoduše klikněte na tlačítko **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Návrhář přidá řádku s přidružení a vlastnosti cizího klíče.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Nyní jste připraveni vytvořit databázi. Klikněte pravým tlačítkem na návrhové plochy a vyberte možnost **generovat databázi z modelu**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Spustí se Průvodce generováním databáze. (Pokud se zobrazí upozornění, která označuje, že nejsou namapovány entity, můžete ignorovat ty prozatím.)

V **vybrat datové připojení** krok, klikněte na tlačítko **nové připojení**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

V **vlastnosti připojení** dialogovém okně vyberte místní instanci systému SQL Server Express a název databáze `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Klikněte na tlačítko **Ano** Pokud se dotaz, pokud chcete vytvořit databázi. Když **vybrat datové připojení** krok se zobrazí znovu, klikněte na tlačítko **Další**.

V **souhrn a nastavení** krok, klikněte na tlačítko **Dokončit**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *.sql* vytvoří soubor s příkazy jazyka DDL definice dat, ale příkazy ještě nespustili.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Pomocí některého nástroje, jako například **SQL Server Management Studio** vytváření tabulky, protože to může být způsobeno při vytvoření a spuštění skriptu `School` databáze pro [z prvního kurzu řady kurzu Začínáme ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Pokud jste si stáhli databáze.)

Teď můžete použít `AlumniAssociation` datového modelu ve vaší webové stránky stejným způsobem, kterým jste pracovali `School` modelu. Pokud chcete vyzkoušet tento limit, některá data přidat do tabulky a vytvořit webovou stránku, která zobrazí data.

Pomocí **Průzkumníka serveru**, přidejte následující řádky, které `Alumnus` a `Donation` tabulky.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Vytvořit novou webovou stránku s názvem *Alumni.aspx* používající *Site.Master* stránky předlohy. Přidejte následující kód do `Content` ovládací prvek s názvem `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Tento kód vytvoří vnořené `GridView` ovládací prvky, vnější jedno zobrazení absolventů názvů a vnitřní jeden k zobrazení odběru a částky.

Otevřete *Alumni.aspx.cs*. Přidat `using` příkaz pro data přístup vrstvu a obslužnou rutinu pro vnější `GridView` ovládacího prvku `RowDataBound` událostí:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Tento kód databinds vnitřní `GridView` řídit pomocí `Donations` vlastnost navigace na aktuálním řádku `Alumnus` entity.

Spuštění stránky.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Poznámka: Tato stránka je součástí ke stažení projektu, ale ke spolupráci je musíte vytvořit databázi v místní instanci systému SQL Server Express; není zahrnuta jako databázi *.mdf* v soubor *aplikace\_ Data* složky.)

Další informace o použití funkce první model Entity Framework najdete v tématu [první Model Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Podpora objektů POCO

Při použití metody funguje na základě domény navrhnete datové třídy, které představují data a chování, které se týkají obchodní domény. Tyto třídy by měla být nezávislé na žádné konkrétní technologie používá k ukládání (zachovat) data; jinými slovy, by měla být *které ignorují trvalost*. Které trvalost můžete také usnadnit třídu testování částí protože projektu testů jednotek můžete použít libovolnou trvalost technologie je nejvhodnější pro testování. Starší verze Entity Framework nabízí omezenou podporu pro trvalost které protože museli dědění z tříd entit `EntityObject` třídy a proto zahrnuty značnou část funkce specifické pro Entity Framework.

Rozhraní Entity Framework 4 zavádí možnost používat tříd entit, které není dědí od `EntityObject` třídy a proto jsou které ignorují trvalost. V kontextu Entity Framework, jsou třídy, jako to obvykle nazývá *objekty CLR prostý starý* (objektů POCO nebo POCOs). Třídy objektů POCO můžete napsat ručně nebo můžete automaticky generovat, je založené na existující model dat pomocí šablon Text šablony transformace Toolkit (T4) poskytované rozhraní Entity Framework.

Další informace o používání POCOs v Entity Framework najdete v následujících zdrojích informací:

- [Práce s entity objektů POCO](https://msdn.microsoft.com/library/dd456853.aspx). Toto je dokument MSDN, který je přehled POCOs, s odkazy na jiné dokumenty, které mít podrobnější informace.
- [Návod: Objektů POCO šablony rozhraní Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) jde blogu z Entity Framework vývojový tým, s odkazy na další příspěvcích na blogu o POCOs.

## <a name="code-first-development"></a>Vývoj s včasným kódu

Podpora objektů POCO v rozhraní Entity Framework 4 stále vyžaduje vytvoření datového modelu a propojit třídy entity do datového modelu. Další verzi rozhraní Entity Framework, bude obsahovat funkci *kód – první vývoj*. Tato funkce umožňuje používat rozhraní Entity Framework se vaše vlastní třídy objektů POCO bez nutnosti používat návrháře modelu dat nebo soubor XML data modelu. (Proto tato možnost byla také volána *jen kód*; *kód první* a *jen kód* odkazují na stejný funkcí rozhraní Entity Framework.)

Další informace o použití kódu první přístup k vývoji, najdete v následujících zdrojích informací:

- [Kód – první vývoj pomocí rozhraní Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Toto je blogový příspěvek ve Scott Guthrie představení vývoj s včasným kódu.
- [Blog týmu nástroje Entity Framework vývoj - odešle CodeOnly s příznakem](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog týmu nástroje Entity Framework vývoj - odešle s příznakem Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Kurz MVC Hudba úložiště – část 4: modely a přístup k datům](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Začínáme s MVC 3 – část 4: vývoj Entity Framework Code First](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Kromě toho se nové MVC kódu-první kurz, který vytváří podobná aplikace Contoso univerzity aplikaci promítá do publikovány pružiny 2011 v [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Další informace

Tím dokončíte přehled co je nového v Entity Framework a tento pokračování s řadou kurz Entity Framework. Další informace o nových funkcích rozhraní Entity Framework 4, které nejsou zahrnuty v tomto poli najdete v následujících zdrojích informací:

- [Co je nového v technologii ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) tématu MSDN na nové funkce ve verzi rozhraní Entity Framework 4.
- [Uvedení verze Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) Entity Framework vývojový tým příspěvku na blogu o nových funkcích ve verze 4.

> [!div class="step-by-step"]
> [Předchozí](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
