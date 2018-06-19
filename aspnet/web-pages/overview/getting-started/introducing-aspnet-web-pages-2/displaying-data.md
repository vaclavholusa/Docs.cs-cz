---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
title: Představení technologie ASP.NET Web Pages – zobrazení dat | Microsoft Docs
author: tfitzmac
description: Tento kurz ukazuje, jak vytvořit databázi ve službě WebMatrix a postup zobrazení dat z databáze na stránce, pokud používáte rozhraní ASP.NET Web Pages (Razor). Předpokládá y...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: b3a006a0-3ea2-4d45-b833-e20e3a3c0a1a
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data
msc.type: authoredcontent
ms.openlocfilehash: 6c66e5fb0a1a49da411286e19c7954f83055c3fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898454"
---
<a name="introducing-aspnet-web-pages---displaying-data"></a>Představení technologie ASP.NET Web Pages – zobrazení dat
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento kurz ukazuje, jak vytvořit databázi ve službě WebMatrix a postup zobrazení dat z databáze na stránce, pokud používáte rozhraní ASP.NET Web Pages (Razor). Předpokládá, že jste dokončili řady prostřednictvím [Úvod do programování webových stránek ASP.NET](../introducing-razor-syntax-c.md).
> 
> Získáte informace:
> 
> - Jak používat nástroje WebMatrix k vytvoření databáze a tabulky databáze.
> - Jak používat nástroje WebMatrix můžete přidat do databáze.
> - Jak zobrazit data z databáze na stránce.
> - Jak spouštět příkazy SQL na webových stránkách ASP.NET.
> - Postup přizpůsobení `WebGrid` pomocná ke změně zobrazení dat a přidat stránkování a řazení.
>   
> 
> Funkce nebo technologie popsané:
> 
> - Databázové nástroje WebMatrix.
> - `WebGrid` pomocné rutiny.


## <a name="what-youll-build"></a>Co budete sestavení

V tomto kurzu předchozí byly zavedeny pro webové stránky ASP.NET (*.cshtml* soubory), základní informace o syntaxi Razor a pomocné rutiny. V tomto kurzu začnete budete vytvářet skutečné webové aplikace, které budete používat pro zbytek řady. Aplikace je jednoduchý filmová aplikace, která vám umožní zobrazit, přidat, změnit a odstranit informace o filmy.

Po dokončení v tomto kurzu, budete moct zobrazit výpis film, který může vypadat této stránce:

![Zobrazení WebGrid, který obsahuje parametry, které jsou nastaveny na názvy šablon stylů CSS – třída](displaying-data/_static/image1.png)

Ale pokud chcete začít, budete muset vytvořit databázi.

## <a name="a-very-brief-introduction-to-databases"></a>Velmi stručný úvod k databázím

Tento kurz vám poskytne pouze briefest Úvod do databáze. Pokud máte zkušenosti s databáze, můžete tento krátký část přeskočit.

Databáze obsahuje jednu nebo více tabulek, které obsahují informace &mdash; například tabulky pro zákazníky, objednávky a dodavatele, nebo pro studenty, učitele, třídy a třídy. Databázové tabulky strukturálně, je třeba tabulkou. Představte si typické adresáře. Pro každou položku v adresáři (který je pro každou osobu) máte několik informací například křestní jméno, příjmení, adresu, e-mailová adresa a telefonní číslo.

![Ukázka databázové tabulky jako jednoduchý mřížky](displaying-data/_static/image2.png)

(Řádky se někdy označují jako *záznamy*, a sloupce se někdy označují jako *pole*.)

Pro většinu tabulky databáze v tabulce musí obsahovat sloupec, který obsahuje jedinečná hodnota, jako je zákaznické číslo, číslo účtu a tak dále. Tato hodnota se označuje jako v tabulce *primární klíč*, a můžete ji použít k identifikaci každý řádek v tabulce. V příkladu je ID sloupce primární klíč pro adresář v předchozím příkladu.

Velká část práce, které můžete provést ve webové aplikace se skládá z čtení informací z databáze nástroje a zobrazení na stránce. Budete také často shromažďovat informace od uživatelů a přidejte ji do databáze, nebo upravte záznamy, které jsou již v databázi. (Jsme zaměříme všechny tyto operace v průběhu této kurz sady.)

Pracovní databáze může být složité, enormously a může vyžadovat specializované znalosti. Pro tento kurz sadu ale musíte pochopit pouze základní koncepty, které budou všechny vysvětleny rovnou.

## <a name="creating-a-database"></a>Vytvoření databáze

Služba WebMatrix nabízí nástroje, které usnadní k vytvoření databáze a k vytváření tabulek v databázi. (Struktura databáze se označuje jako databáze *schématu*.) Pro tento kurz sadu vytvoříte databázi, která se nachází pouze jednu tabulku &mdash; filmy.

Otevřete službu WebMatrix, pokud jste tak již neučinili a otevřete WebPagesMovies web, který jste vytvořili v předchozí kurzu.

V levém podokně klikněte **databáze** pracovního prostoru.

![Karta pracovní prostor databáze služby WebMatrix](displaying-data/_static/image3.png)

Změny pásu karet zobrazíte úkolech týkajících se databáze. Na pásu karet klikněte na tlačítko **novou databázi**.

![V WebMatrix pásu karet na tlačítko 'novou databázi.](displaying-data/_static/image4.png)

Služba WebMatrix vytvoří databázi SQL CE serveru ( *SDF* soubor), má stejný název jako vaše lokalita &mdash; *WebPagesMovies.sdf*. (To nebude provádět zde, ale přejmenujte soubor k ničemu se vám líbí, dokud obsahuje *SDF* rozšíření.)

## <a name="creating-a-table"></a>Vytvoření tabulky

Na pásu karet klikněte na tlačítko **novou tabulku**. Služba WebMatrix otevře návrháře tabulky na nové kartě. (Pokud není k dispozici možnost nové tabulky, ujistěte se, že nové databáze je vybraný ve stromovém zobrazení na levé straně.)

![V WebMatrix pásu karet na tlačítko 'novou tabulku.](displaying-data/_static/image5.png)

V textovém poli nahoře (kde vodoznak říká "Zadejte název tabulky") zadejte "Filmy".

![Zadáte název tabulky v Návrháři databáze služby WebMatrix](displaying-data/_static/image6.png)

V podokně pod název tabulky je, kde můžete definovat jednotlivé sloupce. Pro *filmy* tabulky v tomto kurzu vytvoříte pouze několik sloupců: *ID*, *název*, *Genre*, a *rok*.

V **název** zadejte "ID". Zadáním hodnoty zde aktivuje všechny ovládací prvky pro nový sloupec.

Na kartě **datový typ** seznam a vyberte **int**. Tato hodnota určuje, že sloupec ID bude obsahovat data, celé číslo (number).

> [!NOTE]
> Nebude říkáme mu na všechny zde (mnohem), ale můžete použít standardní gesta klávesnice Windows a přejděte v mřížce. Například můžete kartě mezi poli, můžete právě začněte zadávat text, aby bylo možné, vyberte položku v seznamu a tak dále.


Kartě po **výchozí hodnotu** pole (to znamená, že necháte prázdné). Na kartě **je primární klíč** zaškrtněte políčko a vyberte ho. Tato možnost umožňuje databázi, která *ID* sloupec bude obsahovat data, která identifikuje jednotlivé řádky. (To znamená, každý řádek bude mít jedinečnou hodnotu ve sloupci ID, který můžete použít k vyhledání tento řádek.)

Vyberte **je Identity** možnost. Tato možnost umožňuje databázi, aby měl automaticky generovat další pořadové číslo pro každý nový řádek. ( **Je Identity** možnost funguje jenom v případě, že jste vybrali **je primární klíč** možnost.)

Kliknutím na další řádek mřížky nebo stiskněte klávesu Tab dvakrát na dokončení aktuální řádek. Buď gesto uloží na aktuálním řádku a spuštění dalšímu. Všimněte si, že **výchozí hodnotu** sloupec teď uvádí **Null**. (Null je výchozí hodnota pro výchozí hodnota, tak na speak.)

Když jste dokončili definování nové **ID** sloupce návrháře bude vypadat jako tento obrázek:

![Služba WebMatrix Návrhář databáze po definování ID sloupce pro tabulku filmy](displaying-data/_static/image7.png)

Chcete-li vytvořit další sloupce, klikněte do pole v **název** sloupce. Zadejte "Title" pro sloupec a potom vyberte **nvarchar** pro **datový typ** hodnotu. Část "var" **nvarchar** databáze oznamuje, že data pro tento sloupec bude řetězec, jehož velikost může lišit od záznamy. (Předponu "n" představuje "national", což naznačuje, že pole může obsahovat znak data pro všechny abecedy nebo zápis systému – to znamená, pole obsahuje data ve formátu Unicode.)

Pokud vyberete **nvarchar**, jiného pole se zobrazí, kde můžete zadat maximální délku pole. Zadejte hodnotu 50, za předpokladu, že žádný název filmu, který budete pracovat v tomto kurzu bude delší než 50 znaků.

Přeskočit **výchozí hodnotu** a zrušte zaškrtnutí **povolit hodnoty Null** možnost. Nechcete, aby databázi tak, aby filmy se zapisují do databáze, které nemají název.

Když jste hotovi a přechod na další řádek, vypadá návrháře tomto obrázku:

![Služba WebMatrix Návrhář databáze po definování název sloupce pro tabulku filmy](displaying-data/_static/image8.png)

Opakujte tyto kroky k vytvoření sloupec s názvem "Genre", s výjimkou délky, nastavte na hodnotu pouze 30.

Vytvořte jiný sloupec s názvem "Rok." Datový typ, zvolte **nchar** (ne **nvarchar**) a nastavte délku 4. Rok se chystáte použít 4 číslice jako "1995" nebo "2010", takže nevyžadují proměnlivé velikosti sloupce.

Tady je dokončení návrhu, které bude vypadat takto:

![Návrhář databáze služby WebMatrix po všechna pole jsou definovány pro tabulku filmy](displaying-data/_static/image9.png)

Stisknutím kláves Ctrl + S nebo klikněte na tlačítko **Uložit** tlačítka na panelu nástrojů Rychlý přístup. Zavřete návrháře databáze ukončením kartě.

## <a name="adding-some-example-data"></a>Přidání některé příklad dat

Dále v této série kurzu vytvoříte stránky, kde můžete zadat nový filmy ve formuláři. Teď ale můžete přidat příklad data, která pak můžete zobrazit na stránce.

V **databáze** prostoru ve službě WebMatrix, Všimněte si, že je stromové struktury, která ukazuje *SDF* souborů, které jste vytvořili dříve. Otevřete uzel nové *SDF* souboru a pak otevřete **tabulky** uzlu.

![Pracovní prostor služby WebMatrix databáze s stromu otevřete filmy tabulky](displaying-data/_static/image10.png)

Klikněte pravým tlačítkem myši **filmy** uzel a potom zvolte **Data**. Služba WebMatrix otevře mřížky, kde můžete zadat data *filmy* tabulky:

![Databáze položky mřížky ve službě WebMatrix (prázdný)](displaying-data/_static/image11.png)

Klikněte **název** sloupci a zadejte "Při Harry Sverdlove splněny Jan". Přesunout do **Genre** sloupce (můžete pomocí klávesy Tab) a zadejte "Milostné komedie". Přesunout do **roku** sloupci a zadejte "1989":

![Databáze položky mřížky ve službě WebMatrix s jedním záznamem](displaying-data/_static/image12.png)

Stiskněte klávesu Enter a službě WebMatrix uloží novou film. Všimněte si, že **ID** bylo vyplněno sloupce.

![Databáze položky mřížky ve službě WebMatrix s jedním záznamem a automaticky vygeneruje ID](displaying-data/_static/image13.png)

Zadejte jiný movie (například "zmizel s the běh", "Obrázkům", "1939"). Sloupec ID se znovu vyplní:

![Databáze položky mřížky ve službě WebMatrix se dva záznamy a automaticky vygeneruje ID](displaying-data/_static/image14.png)

Zadejte třetí film (například "Ghostbusters", "Komedie"). Experiment, nechat **roku** sloupec prázdná a stiskněte klávesu Enter. Protože můžete zrušit **povolit hodnoty Null** možnost ukazuje chybu, databáze:

![Zobrazí chyba 'Neplatná data' Pokud hodnota požadovaný sloupec je prázdné](displaying-data/_static/image15.png)

Klikněte na tlačítko **OK** přejít zpět a opravte položka (roku pro hodnotu "Ghostbusters" je 1984) a stiskněte klávesu Enter.

Vyplňte několik filmy, dokud nebudete mít 8, nebo tak. (Zadání 8 usnadňuje práci s stránkování později. Ale pokud, je příliš mnoho, zadejte jenom pár prozatím.) Skutečná data není důležité.

![Databáze položky mřížky ve službě WebMatrix se buď záznamy](displaying-data/_static/image16.png)

Pokud jste zadali všechny filmy bez chyb, jsou hodnoty ID sekvenční. Pokud jste se pokusili uložit záznam neúplné film, nemusí být čísla ID sekvenční. Pokud ano, který je v pořádku. Čísla, která nemají žádný význam vyplývajících a jediné, co je důležité je, že jsou jedinečné v *filmy* tabulky.

Zavřete kartu, která obsahuje Návrhář databáze.

Nyní můžete zapnout k zobrazení tato data na webové stránce.

## <a name="displaying-data-in-a-page-by-using-the-webgrid-helper"></a>Zobrazení dat na stránce pomocí WebGrid pomocné rutiny

K zobrazení dat na stránce, se chystáte použít `WebGrid` pomocné rutiny. Tento Pomocník vytvoří zobrazení v mřížce nebo tabulce (řádků a sloupců). Jak zjistíte, budete moct upravit mřížka s formátování a dalších funkcí.

Pokud chcete spustit mřížky, budete mít k zápisu po zadání několika řádků kódu. Tyto několika řádků bude sloužit jako druh vzor pro téměř všechny přístup k datům, která učiníte v tomto kurzu.

> [!NOTE]
> Máte ve skutečnosti mnoho možností pro zobrazení dat na stránce. `WebGrid` helper je jen jednou. Jsme zvolili ho v tomto kurzu vzhledem k tomu, že je nejjednodušší způsob, jak zobrazit data a je to bude přiměřeně flexibilní. V dalším kurzu sadu uvidíte, jak používat další "Ruční" způsob, jak pracovat s daty na stránce, která vám dává větší kontrolu nad jak zobrazit data.


V levém podokně ve službě WebMatrix, klikněte **soubory** pracovního prostoru.

Nové databáze jste vytvořili *aplikace\_Data* složky. Pokud složce ještě neexistuje, služba WebMatrix se vytvoří pro novou databázi. (Složky může mít existuje Pokud jste dříve nainstalovali pomocné rutiny.)

Ve stromovém zobrazení vyberte kořenovém adresáři webu. Je nutné vybrat kořenovém webu; jinak, může nový soubor přidán do aplikace\_složku Data.

Na pásu karet klikněte na tlačítko **nový**. V **vyberte typ souboru** vyberte **CSHTML**.

V **název** pole, zadejte název nové stránky "Movies.cshtml":

!["Vyberte typ souboru, dialogové okno stránkou, filmy,](displaying-data/_static/image17.png)

Klikněte **OK** tlačítko. Služba WebMatrix otevře nový soubor s některé kostru prvky v ní. Nejprve budete napsat kód přejdete získat data z databáze. Potom přidáte kód na stránku pro zobrazení dat ve skutečnosti.

### <a name="writing-the-data-query-code"></a>Psaní kódu dotazu dat

V horní části stránky mezi `@{` a `}` znaky, zadejte následující kód. (Ujistěte se, abyste zadali tento kód mezi počáteční a koncovou složené závorky.)

[!code-csharp[Main](displaying-data/samples/sample1.cs)]

První řádek otevře databázi, kterou jste vytvořili dříve, což je vždy první krok před provedením něco s databází. Můžete zjistit `Database.Open` metoda název databáze, chcete-li otevřít. Všimněte si, že nechcete zahrnout *SDF* v názvu. `Open` Metoda předpokládá, že je hledá *SDF* souboru (to znamená, *WebPagesMovies.sdf*) a že *SDF* soubor *aplikace\_ Data* složky. (Dříve jsme zjištěno, že *aplikace\_Data* je vyhrazené složky, tento scénář je jedno z míst, kde ASP.NET provádí předpoklady o tímto názvem.)

Po otevření databáze odkaz na jeho umístí do proměnné s názvem `db`. (Který může být s názvem nic.) `db` Proměnná je, jak budete skončili interakce s databází.

Druhý řádek ve skutečnosti načte data databáze pomocí `Query` metoda. Všimněte si, jak funguje tento kód: `db` proměnná nemá odkaz na otevřenou databázi a vyvolání `Query` metoda pomocí `db` proměnné (`db.Query`).

Samotný dotaz je SQL `Select` příkaz. (Pro trochu informace o SQL viz vysvětlení později.) V příkazu `Movies` identifikuje tabulky pro dotaz. `*` Znak určuje, zda má dotaz vrátit všechny sloupce z tabulky. (Může být také seznam sloupců jednotlivě, oddělených čárkami.)

Výsledky dotazu, pokud existuje, se vrátí a dostupná `selectedData` proměnné. Může mít znovu, název proměnné, nic.

Nakonec ve třetím řádku informuje ASP.NET, že chcete použít instanci systému `WebGrid` pomocné rutiny. Vytvoříte (*doložit*) pomocný objekt pomocí `new` – klíčové slovo a předejte ji výsledky dotazu prostřednictvím `selectedData` proměnné. Nové `WebGrid` jsou k dispozici v objektu, spolu s výsledky dotazu databáze `grid` proměnné. Tento výsledek budete potřebovat za chvíli ve skutečnosti zobrazení dat na stránce.

V této fázi databázi otevřít, že jste podmínky data chcete a jste připravili `WebGrid` Pomocník s daty. Dále je vytvoření kód na stránce.

> [!TIP] 
> 
> **Structured Query Language (SQL)**
> 
> SQL je jazyk, který se používá ve většině relační databáze pro správu dat v databázi. Obsahuje příkazy, které umožňují data načíst a aktualizovat je, a které umožňují vytvářet, upravovat a spravovat data v tabulkách databáze. SQL se liší od programovací jazyk (například C#). Pomocí SQL řekněte databázi co chcete použít a je databáze úlohy, zjistěte, jak získat data nebo provést úlohu. Zde jsou příklady některých příkazů SQL a co dělat:
> 
> `Select * From Movies`
> 
> `SELECT ID, Name, Price FROM Product WHERE Price > 10.00 ORDER BY Name`
> 
> První `Select` příkaz načte všechny sloupce (zadáno v `*`) z *filmy* tabulky.
> 
> Druhý `Select` příkaz načte ID, názvu a cena sloupce ze záznamů *produktu* tabulky, jehož hodnota sloupce Price je více než 10. Příkaz vrátí výsledky v abecedním pořadí na základě hodnot název sloupce. Pokud se cena kritériím neodpovídají žádné záznamy, příkaz vrátí prázdnou sadou.
> 
> `INSERT INTO Product (Name, Description, Price) VALUES ('Croissant', 'A flaky delight', 1.99)`
> 
> Tento příkaz vloží nový záznam do *produktu* tabulky, nastavení název sloupce "Croissant", popis sloupce "A nestabilním stavu štěstím" a cenu na 1,99.
> 
> Všimněte si, že když určujete jiné než číselné hodnoty, hodnota se udává v jednoduchých uvozovkách (ne dvojité uvozovky, jako v C#). Můžete použít tyto uvozovky kolem hodnoty text nebo data, ale není kolem čísel.
> 
> `DELETE FROM Product WHERE ExpirationDate < '01/01/2008'`
> 
> Tento příkaz odstraní záznamy v *produktu* tabulku, jejíž sloupce s datem vypršení platnosti je starší než 1. ledna 2008. (Příkaz předpokládá, že *produktu* tabulka obsahuje sloupec, samozřejmě.) Datum je zde uvedeno ve formátu MM/DD/RRRR, ale by měly být zadány ve formátu, který se používá pro národní prostředí.
> 
> `Insert` a `Delete` příkazy nevrací sad výsledků dotazu. Místo toho že budou vracet číslo informující o tom, kolik záznamů situace měla vliv na příkaz.
> 
> Proces, který požaduje operaci pro některé z těchto operací (např. vkládání a odstraňování záznamů), musí mít příslušná oprávnění v databázi. To je důvod, proč pro provozní databáze je často nutné zadat uživatelské jméno a heslo při připojení k databázi.
> 
> Existují desítky příkazy SQL, ale všechny se řídí připomínat příkazy, kterou tady vidíte. Příkazy SQL můžete vytvářet tabulky databáze, počet záznamů v tabulce, výpočet ceny a provádět mnoho dalších operacích.


### <a name="adding-markup-to-display-the-data"></a>Přidání značek k zobrazení dat

Uvnitř `<head>` elementu, obsah sady `<title>` element "Filmy":

[!code-html[Main](displaying-data/samples/sample2.html?highlight=3)]

Uvnitř `<body>` element stránky, přidejte následující:

[!code-html[Main](displaying-data/samples/sample3.html)]

Je to. `grid` Proměnné je hodnota jste vytvořili, když jste vytvořili `WebGrid` objektu v kódu dříve.

Služba WebMatrix stromové zobrazení, klikněte pravým tlačítkem na stránku a vyberte **spustit v prohlížeči**. Zobrazí se něco podobného jako na této stránce:

![Výchozí WebGrid pomocná výstup z tabulky filmy](displaying-data/_static/image18.png)

Kliknutím na odkaz záhlaví sloupce řadit podle sloupce. Schopnost seřadit kliknutím na záhlaví je funkce, která je integrovaná **WebGrid** pomocné rutiny.

`GetHtml` Metoda, jak naznačuje název generuje kód, který zobrazí data. Ve výchozím nastavení `GetHtml` metoda generuje HTML `<table>` elementu. (Pokud chcete, můžete ověřit vykreslování podle zdroj stránku v prohlížeči.)

## <a name="modifying-the-look-of-the-grid"></a>Úpravy vzhledu mřížky

Pomocí `WebGrid` pomocná stejně, jako jste právě je snadno, ale výsledné zobrazení je prostý. `WebGrid` Pomocná má nejrůznějším možnosti, ve kterých můžete řídit, jak se zobrazí data. Příliš mnoho prozkoumat v tomto kurzu, ale v této části získáte představu o některé z těchto možností. V dalších kurzech této série se budeme několik dalších možností.

### <a name="specifying-individual-columns-to-display"></a>Zadání jednotlivých sloupců do zobrazení

Pokud chcete spustit, můžete zadat, že chcete zobrazit jenom určitým sloupce. Ve výchozím nastavení, jako jste se seznámili, mřížky ukazuje všechny čtyři sloupce z *filmy* tabulky.

V *Movies.cshtml* souboru, nahraďte `@grid.GetHtml()` kód, který jste právě přidali následujícím kódem:

[!code-css[Main](displaying-data/samples/sample4.css)]

Zjistit pomocné rutiny, které sloupce budou zobrazeny, zahrnete `columns` parametr pro `GetHtml` metoda a předejte jí v kolekci sloupců. V kolekci určíte každý sloupec zahrnout. Zadejte jednotlivé sloupec, který se zobrazí zahrnutím `grid.Column` objektu a předejte názvu sloupce dat, který chcete. (Tyto sloupce musí být součástí výsledků dotazu SQL – `WebGrid` pomocníka nelze zobrazit sloupce, které nebyly vrácených dotazem.)

Spusťte *Movies.cshtml* stránku v prohlížeči znovu a tentokrát získat zobrazení stejný, jako je následující (Všimněte si, že se zobrazí ve sloupci žádné ID):

![Zobrazení WebGrid zobrazuje pouze vybrané sloupce](displaying-data/_static/image19.png)

### <a name="changing-the-look-of-the-grid"></a>Změna vzhledu mřížky

Existuje několik další možnosti pro zobrazení sloupce, z nichž některé bude prozkoumali v dalších kurzech v této sadě. Prozatím se v této části vás seznámí s způsoby, ve kterém můžete stylů mřížky jako celek.

Uvnitř `<head>` části stránky, těsně před uzavírací `</head>` značky, přidejte následující `<style>` element:

[!code-css[Main](displaying-data/samples/sample5.css)]

Tento kód šablon stylů CSS definuje třídy s názvem `grid`, `head`a tak dále. Tyto definice styl by také mohlo v samostatném *.css* souboru a odkaz, který na stránku. (Ve skutečnosti, můžete to udělat později v tomto kurzu.) Chcete-li věcí snadno pro účely tohoto kurzu, jsou ale uvnitř stejné stránce, která zobrazuje data.

Teď můžete získat `WebGrid` pomocná rutina pro použití těchto tříd stylu. Pomocné rutiny má několik vlastností (například `tableStyle`) pro právě tento účel – přiřadit název třídy CSS styl na ně, a že název třídy je vykreslen jako součást kód, který je vykreslen metodou pomocné rutiny.

Změna `grid.GetHtml` kód tak, aby se teď vypadá jako tento kód:

[!code-css[Main](displaying-data/samples/sample6.css)]

Co je nového v tomto poli je, že jste přidali `tableStyle`, `headerStyle`, a `alternatingRowStyle` parametry, které `GetHtml` metoda. Tyto parametry byly nastaveny na názvy stylů CSS, které jste přidali před chvíli.

Spuštění stránky a Tentokrát uvidíte v mřížce, která vypadá než prostý mnohem menší než:

![Zobrazení WebGrid, který obsahuje parametry, které jsou nastaveny na názvy šablon stylů CSS – třída](displaying-data/_static/image20.png)

Chcete-li zjistit, jaká `GetHtml` metoda vygenerována, můžete se podívat na zdroji stránku v prohlížeči. Jsme nebude přejděte sem podrobností, ale důležité je tak, že zadáte parametry jako `tableStyle`, způsobila mřížky mají vygenerovat značky HTML jako následující:

`<table class="grid">`

`<table>` Značky došlo `class` do ní přidat atribut, který odkazuje na jeden pravidla šablon stylů CSS, která jste přidali dříve. Tento kód ukazuje základní vzor &mdash; různé parametry pro `GetHtml` metoda umožňují vám odkaz šablon stylů CSS třídy, že metoda pak generuje společně s kód. Co se děje s tříd šablon stylů CSS je na vás.

## <a name="adding-paging"></a>Přidání stránkování

Jako poslední úlohy pro účely tohoto kurzu přidáte stránkování mřížky. Tuto chvíli je žádný problém zobrazíte všechny filmy najednou. Ale pokud jste přidali stovky filmy, zobrazení stránky by získat dlouho.

V kódu stránky, změňte řádek, který vytvoří `WebGrid` objekt, který má následující kód:

[!code-csharp[Main](displaying-data/samples/sample7.cs)]

Jediným rozdílem z před je, že jste přidali `rowsPerPage` parametr, který je nastaven na 3.

Spuštění stránky. V mřížce zobrazené na čas, plus navigační odkazy, které umožní stránku prostřednictvím filmy ve vaší databázi 3 řádky:

![Zobrazení WebGrid s stránkování](displaying-data/_static/image21.png)

## <a name="coming-up-next"></a>Objevuje další

V dalším kurzu budete Další informace o použití kódu Razor a C# pro získání vstupu uživatele ve formuláři. Přidáte vyhledávací pole na stránku filmy, aby mohli najít filmy podle názvu nebo genre.

## <a name="complete-listing-for-movies-page"></a>Úplný seznam filmy stránky

[!code-cshtml[Main](displaying-data/samples/sample8.cshtml)]

## <a name="additional-resources"></a>Další prostředky

- [Úvod do rozhraní ASP.NET Web programování pomocí syntaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)

> [!div class="step-by-step"]
> [Předchozí](intro-to-web-pages-programming.md)
> [další](form-basics.md)
