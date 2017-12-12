---
uid: web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
title: "Vytvořit rozhraní REST API s atribut směrování v rozhraní ASP.NET Web API 2 | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 23fc77da-2725-4434-99a0-ff872d96336b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing
msc.type: authoredcontent
ms.openlocfilehash: 9ecc233e595716a167ad800a0a21a6162b051648
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="create-a-rest-api-with-attribute-routing-in-aspnet-web-api-2"></a>Vytvořit rozhraní REST API s atributem směrování v rozhraní ASP.NET Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Webové rozhraní API 2 podporuje nový typ směrování, nazývá *směrováním atributů*. Obecné přehled směrováním atributů, najdete v tématu [atribut směrování ve webovém rozhraní API 2](attribute-routing-in-web-api-2.md). V tomto kurzu budete používat směrováním atributů k vytvoření rozhraní REST API pro kolekci knih. Rozhraní API se podporují následující akce:

| Akce | Příklad identifikátoru URI |
| --- | --- |
| Získejte seznam všech knih. | / api/knihy |
| Získání seznamu pomocí ID. | /API/Books/1 |
| Získání podrobností o knihy. | /API/Books/1/details |
| Získáte seznam seznamů genre. | /API/Books/fantasy |
| Získáte seznam seznamů datum publikace. | /API/Books/Date/2013-02-16 /api/books/date/2013/02/16 (alternativní formulář) |
| Získáte seznam seznamů určitého autora. | /API/authors/1/Books |

Všechny metody jsou jen pro čtení (požadavky HTTP GET).

Datová vrstva použijeme Entity Framework. Seznam záznamů, bude mít následující pole:

- ID
- Název
- Genre
- Datum publikování
- Cena
- Popis
- AuthorID (cizí klíč k tabulce Autoři)

Pro většinu požadavky ale rozhraní API vrátí podmnožinu těchto dat (název, autor a genre). Chcete-li získat úplný záznam klienta požadavky `/api/books/{id}/details`.

## <a name="prerequisites"></a>Požadavky

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional nebo Enterprise edition.

## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

Spusťte spuštěním sady Visual Studio. Z **soubor** nabídce vyberte možnost **nový** a pak vyberte **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu &quot;BooksAPI&quot;.

![](create-a-rest-api-with-attribute-routing/_static/image1.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony. V části "Přidat složky a odkazy na základní" vyberte **webového rozhraní API** zaškrtávací políčko. Klikněte na tlačítko **vytvoření projektu**.

![](create-a-rest-api-with-attribute-routing/_static/image2.png)

Tím se vytvoří kostru projektu, který je nakonfigurován pro funkce webového rozhraní API.

### <a name="domain-models"></a>Modely domény

Dál přidejte třídy pro modely domény. V Průzkumníku řešení klikněte pravým tlačítkem na složku modely. Vyberte **přidat**, pak vyberte **třída**. Název třídy `Author`.

![](create-a-rest-api-with-attribute-routing/_static/image3.png)

Nahraďte kód v Author.cs s následujícími službami:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample1.cs)]

Nyní přidejte jinou třídu s názvem `Book`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample2.cs)]

### <a name="add-a-web-api-controller"></a>Přidat řadič webové rozhraní API

V tomto kroku přidáme kontroleru webového rozhraní API, používající rozhraní Entity Framework jako datové vrstvy.

Sestavte projekt stisknutím kombinace kláves CTRL+SHIFT+B. Rozhraní Entity Framework využívá reflexe k vyhledávání vlastností modely, takže vyžaduje kompilované sestavení vytvořit schéma databáze.

V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat**, pak vyberte **řadič**.

![](create-a-rest-api-with-attribute-routing/_static/image4.png)

V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte možnost "Web API 2 řadiče s akcemi čtení/zápisu používající rozhraní Entity Framework."

[![](create-a-rest-api-with-attribute-routing/_static/image6.png)](create-a-rest-api-with-attribute-routing/_static/image5.png)

V **přidat kontroler** dialogu pro **názvu Kontroleru**, zadejte &quot;BooksController&quot;. Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko. Pro **třída modelu**, vyberte &quot;kniha&quot;. (Pokud se nezobrazí `Book` třídy uvedené v rozevírací nabídce, ujistěte se, že jste vytvořili projekt.) Klikněte na tlačítko "+".

![](create-a-rest-api-with-attribute-routing/_static/image7.png)

Klikněte na tlačítko **přidat** v **nový kontext dat** dialogové okno.

![](create-a-rest-api-with-attribute-routing/_static/image8.png)

Klikněte na tlačítko **přidat** v **přidat kontroler** dialogové okno. Přidá třídu s názvem generování uživatelského rozhraní `BooksController` , který definuje kontroler API. Také přidá třídu s názvem `BooksAPIContext` ve složce modely, která definuje kontextu dat rozhraní Entity Framework.

![](create-a-rest-api-with-attribute-routing/_static/image9.png)

### <a name="seed-the-database"></a>Počáteční hodnoty databáze

Z nabídky Nástroje, vyberte **Správce balíčků knihoven**a potom vyberte **Konzola správce balíčků**.

V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample3.ps1)]

Tento příkaz vytvoří složku Migrations a přidá nový kód soubor s názvem Configuration.cs. Umožňuje otevřít tento soubor a přidejte následující kód, který `Configuration.Seed` metoda.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample4.cs)]

V okně konzoly Správce balíčků zadejte následující příkazy.

[!code-powershell[Main](create-a-rest-api-with-attribute-routing/samples/sample5.ps1)]

Tyto příkazy vytvořit místní databázi a vyvolání metody počáteční hodnoty k naplnění databáze.

![](create-a-rest-api-with-attribute-routing/_static/image10.png)

## <a name="add-dto-classes"></a>Přidat DTO třídy

Pokud aplikaci nyní spustit a odeslat požadavek GET na /api/books/1, odpověď bude vypadat podobně jako následující. (Po přidání odsazení čitelnější.)

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample6.json)]

Místo toho chci, aby tento požadavek vrátí podmnožinu pole. Navíc má být vrátit jméno autora, nikoli Autor ID. K tomu, upravíme metody kontroleru vrátit *objekt pro přenos dat* (DTO) namísto EF modelu. DTO je objekt, který je určena pouze pro přenášejí data.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat** | **novou složku**. Název složky &quot;DTOs&quot;. Přidejte třídu s názvem `BookDto` do složky DTOs, s následující definice:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample7.cs)]

Přidejte jinou třídu s názvem `BookDetailDto`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample8.cs)]

Potom aktualizujte `BooksController` třídy vracení `BookDto` instance. Použijeme [Queryable.Select](https://msdn.microsoft.com/en-us/library/system.linq.queryable.select.aspx) metoda do projektu `Book` instance k `BookDto` instance. Tady je aktualizovaný kód pro třídy kontroleru.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample9.cs)]

> [!NOTE]
> Odstraněné `PutBook`, `PostBook`, a `DeleteBook` metody, protože nejsou pro tento kurz potřeba.


Teď Pokud spuštění aplikace a požádat o /api/books/1, text odpovědi by měl vypadat takto:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample10.json)]

## <a name="add-route-attributes"></a>Přidat trasy atributů

V dalším kroku se nám budete převést řadiče, pokud chcete používat směrování atribut. Nejprve přidejte **RoutePrefix** atribut kontroleru. Tento atribut definuje počáteční segmenty identifikátor URI pro všechny metody v tomto řadiči.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample11.cs?highlight=1)]

Pak přidejte **[trasy]** atributy pro akce kontroleru, následujícím způsobem:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample12.cs?highlight=1,7)]

Šablona trasy pro jednotlivé metody kontroleru je předpona plus řetězec zadaný v **trasy** atribut. Pro `GetBook` metoda šablonu trasy zahrnuje parametrizovaný řetězec &quot;{id: int}&quot;, který odpovídá, pokud segment identifikátoru URI obsahuje celočíselnou hodnotu.

| Metoda | Šablona trasy | Příklad identifikátoru URI |
| --- | --- | --- |
| `GetBooks` | "rozhraní api nebo seznamy." | `http://localhost/api/books` |
| `GetBook` | "api/books / {id: int}" | `http://localhost/api/books/5` |

## <a name="get-book-details"></a>Získat podrobnosti adresáře

Chcete-li získat podrobnosti adresáře, klient odešle požadavek GET na `/api/books/{id}/details`, kde *{id}* je ID knihy.

Přidejte následující metodu do `BooksController` třídy.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample13.cs)]

Pokud si vyžádáte `/api/books/1/details`, odpověď bude vypadat takto:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample14.json)]

## <a name="get-books-by-genre"></a>Získání seznamů pomocí Genre

Chcete-li získat seznam seznamů v konkrétní genre, klient odešle požadavek GET na `/api/books/genre`, kde *genre* je název genre. (Například `/get/books/fantasy`.)

Přidejte následující metodu do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample15.cs)]

Zde jsme se definování trasy, která obsahuje parametr {genre} v šabloně identifikátor URI. Všimněte si, že webového rozhraní API je možné rozlišit tyto dva identifikátory URI a směrovat je do různých metod:

`/api/books/1`

`/api/books/fantasy`

Důvodem je, že `GetBook` metoda obsahuje omezení, že segment "id" musí být celočíselná hodnota:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample16.cs?highlight=1)]

Pokud budete požadovat /api/books/fantasy, odpověď vypadá takto:

`[ { "Title": "Midnight Rain", "Author": "Ralls, Kim", "Genre": "Fantasy" }, { "Title": "Maeve Ascendant", "Author": "Corets, Eva", "Genre": "Fantasy" }, { "Title": "The Sundered Grail", "Author": "Corets, Eva", "Genre": "Fantasy" } ]`

## <a name="get-books-by-author"></a>Získat knihy autorem

Pokud chcete získat seznam seznamů, pro konkrétní autora, klient odešle požadavek GET na `/api/authors/id/books`, kde *id* je ID autora.

Přidejte následující metodu do `BooksController`.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample17.cs)]

V tomto příkladu je zajímavé protože &quot;knihy&quot; je zpracováván prostředkem podřízené &quot;autoři&quot;. Tento vzor je celkem běžné rozhraní RESTful API.

Znak tilda (~) v šabloně trasy přepsání předponu trasy v **RoutePrefix** atribut.

## <a name="get-books-by-publication-date"></a>Získat knihy podle data publikování

Pokud chcete získat seznam seznamů, datu publikace, klient odešle požadavek GET na `/api/books/date/yyyy-mm-dd`, kde *rrrr mm-dd* je datum.

Zde je možné provést toto:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample18.cs)]

`{pubdate:datetime}` Parametr je omezena tak, aby odpovídala **data a času** hodnotu. Tento postup funguje, ale je ve skutečnosti mírnější, než rádi bychom znali. Tyto identifikátory URI například bude odpovídat trasy:

`/api/books/date/Thu, 01 May 2008`

`/api/books/date/2000-12-16T00:00:00`

Není co nesprávný s povolením tyto identifikátory URI. Můžete však omezit trasy na konkrétní formát přidáním regulární výraz omezení pro šablonu trasy:

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample19.cs?highlight=1)]

Nyní pouze data ve formě &quot;rrrr mm-dd&quot; bude odpovídat. Všimněte si, že jsme nepoužívejte regulární výraz k ověření, že My skutečná data. Zpracovává když se pokusí převést segment identifikátoru URI do webového rozhraní API **data a času** instance. Neplatné datum například 2012-47-99 se nepodaří převést, a klient získá chybu 404.

Může také podporovat oddělovač lomítko (`/api/books/date/yyyy/mm/dd`) tak, že přidáte další **[trasy]** atribut s jiný regulární výraz.

[!code-html[Main](create-a-rest-api-with-attribute-routing/samples/sample20.html)]

Je zde podrobností jemně ale důležité. Druhá šablona trasy obsahovala zástupný znak (\*) na začátku parametr {pubdate}:

[!code-json[Main](create-a-rest-api-with-attribute-routing/samples/sample21.json)]

Tato hodnota informuje modulu směrování, že tento {pubdate} by měl odpovídat zbytek identifikátor URI. Ve výchozím nastavení odpovídá parametru šablony jeden segment identifikátoru URI. V tomto případě chceme {pubdate} rozmístěny v několika segmentům URI:

`/api/books/date/2013/06/17`

## <a name="controller-code"></a>Kódu kontroleru

Tady je kompletní kód pro třídu BooksController.

[!code-csharp[Main](create-a-rest-api-with-attribute-routing/samples/sample22.cs)]

## <a name="summary"></a>Souhrn

Atribut směrování nabízí další ovládací prvek a větší flexibilitu při navrhování identifikátory URI pro vaše rozhraní API.
