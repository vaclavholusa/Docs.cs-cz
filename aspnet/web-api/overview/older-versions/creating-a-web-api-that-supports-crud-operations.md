---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: Povolení operací CRUD v ASP.NET Web API 1 | Dokumentace Microsoftu
author: MikeWasson
description: Tento kurz ukazuje, jak podporovat operace CRUD v rámci protokolu HTTP služby pomocí rozhraní ASP.NET Web API. Verze softwaru používaných kurz Visual Studio 2012 Web přístupový bod...
ms.author: riande
ms.date: 01/28/2012
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: ba061b26b8527e447f25f6046057542a54f989a8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752826"
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Povolení operací CRUD v ASP.NET Web API 1
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Tento kurz ukazuje, jak podporovat operace CRUD v rámci protokolu HTTP služby pomocí rozhraní ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Visual Studio 2012
> - Webové rozhraní API 1 (také funguje s webovým rozhraním API 2)


Zastupuje CRUD &quot;vytvoření, čtení, aktualizace a odstraňování,&quot; jsou čtyři základní databázových operací. Mnoho služeb HTTP taky model operace CRUD prostřednictvím REST nebo jako REST API.

V tomto kurzu vytvoříte velmi jednoduché webové rozhraní API ke správě seznamu produktů. Každý produkt bude obsahovat název, cena a kategorie (například &quot;toys&quot; nebo &quot;hardwaru&quot;), plus ID produktu.

Produkty rozhraní API bude vystavovat následující metody.

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání seznamu všech produktů | ZÍSKAT | / api/produkty |
| Získání produktu podle ID | ZÍSKAT | / webové rozhraníAPI/produkty/*id* |
| Získání produktu podle kategorie | ZÍSKAT | / api/produkty? kategorie =*kategorie* |
| Vytvořit nový produkt | PŘÍSPĚVEK | / api/produkty |
| Aktualizace produktu | PUT | / webové rozhraníAPI/produkty/*id* |
| Odstranit produkt | DELETE | / webové rozhraníAPI/produkty/*id* |

Všimněte si, že některé identifikátory URI obsahovat číslo product ID v cestě. Pokud chcete získat produktů, jehož ID je 28, klient odešle požadavek GET `http://hostname/api/products/28`.

### <a name="resources"></a>Prostředky

Produkty API definuje identifikátory URI pro dva typy prostředků:

| Prostředek | Identifikátor URI |
| --- | --- |
| Seznam všech produktů. | / api/produkty |
| Jednotlivé produkty. | / webové rozhraníAPI/produkty/*id* |

### <a name="methods"></a>Metody

Čtyři hlavní metody HTTP (GET, PUT, POST a DELETE) lze mapovat na operace CRUD následujícím způsobem:

- GET načte reprezentaci prostředku na zadaný identifikátor URI. GET by měl mít žádné vedlejší účinky na serveru.
- PUT aktualizuje prostředek na zadaný identifikátor URI. PUT také umožňuje vytvořit nový prostředek na zadaný identifikátor URI, pokud server umožňuje klientům k určení nové identifikátory URI. Pro účely tohoto kurzu nebude podporovat rozhraní API pro vytváření PUT.
- POST vytvoří nový prostředek. Server přiřadí identifikátor URI pro nový objekt a vrátí tento identifikátor URI jako část zprávy s odpovědí.
- Odstranit Odstraní prostředek na zadaný identifikátor URI.

Poznámka: Metodu PUT nahradí entitu celého produktu. To znamená Klient by měl odeslat úplnou reprezentaci aktualizace produktu. Pokud chcete zajistit podporu částečné aktualizace, je upřednostňovanou metodu PATCH. Tento kurz neimplementuje opravy.

## <a name="create-a-new-web-api-project"></a>Vytvořte nový projekt webového rozhraní API

Začněte tím, že spustíte Visual Studio a vyberte **nový projekt** z **Start** stránky. Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **webové aplikace ASP.NET MVC 4**. Pojmenujte projekt &quot;ProductStore&quot; a klikněte na tlačítko **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

V **nového projektu ASP.NET MVC 4** dialogového okna, vyberte **webového rozhraní API** a klikněte na tlačítko **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Přidání modelu

A *modelu* je objekt, který představuje data ve vaší aplikaci. V rozhraní ASP.NET Web API objektů se silným typem CLR slouží jako modelů a jejich bude automaticky serializovat do XML nebo JSON pro klienta.

Pro rozhraní API ProductStore naše data se skládá z produktů, takže vytvoříme novou třídu s názvem `Product`.

Pokud již není Průzkumník řešení viditelný, klikněte na tlačítko **zobrazení** nabídky a vybereme **Průzkumníka řešení**. V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky. V kontextu měny, vyberte **přidat**a pak vyberte **třídy**. Název třídy &quot;produktu&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Přidejte následující vlastnosti pro `Product` třídy.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Přidání úložiště

Potřebujeme k uložení kolekce produktů. Je vhodné oddělit kolekce z naší implementaci služby. Tímto způsobem můžeme změnit záložní úložiště bez přepsání třídu služby. Tento typ návrhu se nazývá *úložiště* vzor. Začněte tím, že definování obecné rozhraní pro úložiště.

V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky. Vyberte **přidat**a pak vyberte **nová položka**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

V **šablony** vyberte **nainstalované šablony** a rozbalte uzel jazyka C#. V jazyce C# vyberte **kód**. Vyberte v seznamu šablon kódu **rozhraní**. Název rozhraní &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Přidejte následující implementaci:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Nyní přidejte další třídu ke složce modelů s názvem &quot;ProductRepository.&quot; Tato třída implementuje `IProductRespository` rozhraní. Přidejte následující implementaci:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Úložiště zajišťuje seznamu místní paměti. Tento kurz je v pořádku, ale v reálné aplikaci bude ukládat data externě, buď databázi nebo v cloudovém úložišti. Vzor úložiště vám usnadní změňte implementaci později.

## <a name="adding-a-web-api-controller"></a>Přidání Kontroleru webového rozhraní API

Pokud jste už pracovali s ASP.NET MVC, pak jste již obeznámeni s řadiči. V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP od klienta. Průvodce novým projektem vytvoří dva řadiče za vás při vytváření projektu. Neuvidíte, rozbalte složku řadiče v Průzkumníku řešení.

- HomeController je tradiční kontroler ASP.NET MVC. Zodpovídá za poskytování stránky HTML pro web a přímo nesouvisí s naší webové rozhraní API.
- Kontroleru ValuesController se příklad řadiče WebAPI.

Pokračujte a odstranit kontroleru ValuesController, tak, že kliknete pravým tlačítkem soubor v Průzkumníku řešení a vyberete **odstranit.** Nyní přidejte nový kontroler, následujícím způsobem:

V **Průzkumníka řešení**, klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat** a pak vyberte **řadič**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

V **přidat kontroler** průvodce, názvu kontroleru &quot;ProductsController&quot;. V **šablony** rozevíracího seznamu vyberte **prázdný kontroler API**. Pak klikněte na tlačítko **přidat**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Není nutné převést vaše contollers do složky s názvem řadiče. Název složky není důležité. je jednoduše pohodlný způsob, jak uspořádat zdrojové soubory.


**Přidat kontroler** průvodce vytvořit soubor s názvem ProductsController.cs ve složce řadiče. Pokud tento soubor ještě není otevřený, klikněte dvakrát na soubor otevřete. Přidejte následující **pomocí** – příkaz:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Přidat pole, která obsahuje **IProductRepository** instance.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Volání `new ProductRepository()` v řadiči není optimální, protože propojuje kontroleru pro konkrétní implementaci `IProductRepository`. Lepším řešením, naleznete v tématu [použitím překladač závislostí webové rozhraní API](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Získání prostředku

Rozhraní API ProductStore bude vystavovat několik &quot;čtení&quot; akce jako metody GET protokolu HTTP. Každá akce, které budou odpovídat na metodu v `ProductsController` třídy.

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání seznamu všech produktů | ZÍSKAT | / api/produkty |
| Získání produktu podle ID | ZÍSKAT | / webové rozhraníAPI/produkty/*id* |
| Získání produktu podle kategorie | ZÍSKAT | / api/produkty? kategorie =*kategorie* |

Pokud chcete získat seznam všech produktů, přidejte tuto metodu za účelem `ProductsController` třídy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Název metody, který začíná &quot;získat&quot;, takže podle konvence je namapován na požadavky GET. Navíc vzhledem k tomu, že metoda nemá žádné parametry, mapuje se na identifikátor URI, který neobsahuje *&quot;id&quot;* segment v cestě.

Chcete-li získat produktů podle ID, přidejte tuto metodu za účelem `ProductsController` třídy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Tento název metody také začíná &quot;získat&quot;, ale metoda nemá parametr pojmenovaný *id*. Tento parametr je namapována na &quot;id&quot; segment cesty identifikátoru URI. ID rozhraní ASP.NET Web API automaticky převede na správného datového typu (**int**) pro parametr.

Metoda GetProduct vyvolá výjimku typu **HttpResponseException** Pokud *id* není platný. Tato výjimka bude fungovat v rámci rozhraní chybu 404 (Nenalezeno).

Nakonec přidejte metodu k vyhledat produkty podle kategorie:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Pokud identifikátor URI požadavku obsahuje řetězce dotazu, se pokusí odpovídat parametrům dotazu k parametrům metody kontroleru webového rozhraní API. Proto se identifikátor URI ve tvaru "rozhraní api a produktů? kategorie =*kategorie*" se namapuje do této metody.

## <a name="creating-a-resource"></a>Vytvoření prostředku

V dalším kroku přidáme metodu `ProductsController` třídy za účelem vytvoření nového produktu. Tady je jednoduchý implementace metody:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Mějte na paměti o této metodě dvě věci:

- Název metody, který začíná &quot;příspěvek... &quot;. Pokud chcete vytvořit nový produkt, klient odešle požadavek HTTP POST.
- Tato metoda přebírá parametr typu produktu. V rozhraní Web API parametry s komplexní typy jsou v daném kontextu deserializovat tělo požadavku. Proto Očekáváme, že klientovi umožní odeslat serializovaná reprezentace objektu produktu ve formátu XML nebo JSON.

Tato implementace bude fungovat, ale není úplně kompletní. V ideálním případě by rádi bychom odpověď HTTP, patří:

- **Kód odpovědi:** ve výchozím nastavení, rozhraní webového rozhraní API nastaví stavový kód odpovědi 200 (OK). Ale podle protokolu HTTP/1.1, když požadavek POST má za následek vytvoření prostředku, server by měl odpovídat se stavem 201 (vytvořeno).
- **Umístění:** při serveru vytvoří prostředek, měl by obsahovat identifikátor URI nového prostředku v hlavičce umístění odpovědi.

Rozhraní ASP.NET Web API usnadňuje zpracování zprávy s odpovědí HTTP. Tady je lepší implementace:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Všimněte si, že je návratový typ metody teď **objekt HttpResponseMessage**. Vrácením **objekt HttpResponseMessage** místo produktu, jsme můžete řídit podrobnosti zprávy s odpovědí HTTP, včetně stavový kód a hlavičku Location.

**CreateResponse** metoda vytvoří **objekt HttpResponseMessage** a automaticky zapisuje do těla serializovaná reprezentace objektu Product fo zprávy s odpovědí.

> [!NOTE]
> V tomto příkladu neověřuje, `Product`. Informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualizuje se prostředek

Aktualizace produktu pomocí PUT je jednoduchý:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Název metody, který začíná &quot;vložit... &quot;, takže webového rozhraní API patřil k žádosti PUT. Tato metoda přebírá dva parametry, ID produktu a aktualizace produktu. *Id* parametr je převzata z cesty identifikátoru URI a *produktu* parametru se v daném kontextu deserializovat tělo požadavku. Ve výchozím nastavení použije rozhraní ASP.NET Web API jednoduché typy parametrů z trasy a komplexní typy z textu požadavku.

## <a name="deleting-a-resource"></a>Odstranění prostředku

Pokud chcete odstranit resourse, definujte metodu "Odstranit...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Pokud je žádost o odstranění úspěšné, může vrátit stav 200 (OK) s – obsah entity, která popisuje stav; Stav 202 (přijato), pokud se odstranění je stále čeká na; stav nebo 204 (žádný obsah) se žádný obsah entity. V takovém případě `DeleteProduct` metoda má `void` návratový typ, takže rozhraní ASP.NET Web API automaticky to převede do stavu kód 204 (žádný obsah).
