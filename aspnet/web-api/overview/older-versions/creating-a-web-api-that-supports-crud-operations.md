---
uid: web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
title: "Povolení operace CRUD v ASP.NET Web API 1 | Microsoft Docs"
author: MikeWasson
description: "Tento kurz ukazuje, jak pro podporu operací CRUD v službě HTTP pomocí rozhraní ASP.NET Web API. Verze softwaru používány kurz Visual Studio 2012 Web Asie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2012
ms.topic: article
ms.assetid: c125ca47-606a-4d6f-a1fc-1fc62928af93
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations
msc.type: authoredcontent
ms.openlocfilehash: a91bf065c9ce0fc5bd9b7115340edabea975a7e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="enabling-crud-operations-in-aspnet-web-api-1"></a>Povolení operace CRUD v ASP.NET Web API 1
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-c4761894)

> Tento kurz ukazuje, jak pro podporu operací CRUD v službě HTTP pomocí rozhraní ASP.NET Web API.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Visual Studio 2012
> - Webové rozhraní API 1 (taky funguje u webových rozhraní API 2)


Zastupuje CRUD &quot;vytvoření, čtení, aktualizaci a odstraňování,&quot; který jsou čtyři základní databázových operací. Mnoho služeb HTTP také model operace CRUD prostřednictvím rozhraní API REST jako nebo REST.

V tomto kurzu vytvoříte velmi jednoduché webové rozhraní API pro správu seznam produktů. Každý produkt bude obsahovat název, ceny a kategorie (například &quot;toys&quot; nebo &quot;hardwaru&quot;), plus ID produktu.

Následující metody zveřejní produkty rozhraní API.

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání seznamu všech produktů | GET | / api/produkty |
| Získání ID produktu | GET | /API/produkty/*id* |
| Získání produktu podle kategorie | GET | produkty/api /? kategorie =*kategorie* |
| Vytvoření nového produktu | POST | / api/produkty |
| Aktualizace produktu | PUT | /API/produkty/*id* |
| Odstranit produktu | DELETE | /API/produkty/*id* |

Všimněte si, že některé identifikátory URI v cestě zahrnují ID produktu. Chcete-li získat produktu, jejíž ID je 28, klient odešle požadavek GET `http://hostname/api/products/28`.

### <a name="resources"></a>Prostředky

Produkty API definuje identifikátory URI pro dva typy prostředků:

| Prostředek | Identifikátor URI |
| --- | --- |
| Seznam všech produktů. | / api/produkty |
| Jednotlivé produkty. | /API/produkty/*id* |

### <a name="methods"></a>Metody

Čtyři hlavní metody HTTP (GET, PUT, POST a DELETE) lze mapovat na operace CRUD následujícím způsobem:

- GET načte reprezentace prostředek na zadaný identifikátor URI. GET by měl mít žádné vedlejší účinky na serveru.
- PUT aktualizuje prostředek na zadaný identifikátor URI. PUT také slouží k vytvoření nového prostředku na zadaný identifikátor URI, pokud server umožňuje klientům zadejte nové identifikátory URI. V tomto kurzu nebudou podporovat rozhraní API vytváření PUT.
- POST vytvoří nový prostředek. Server přiřadí identifikátor URI pro nový objekt a vrátí tento identifikátor URI jako součást zprávu odpovědi.
- ODSTRANĚNÍ odstraní prostředek na zadaný identifikátor URI.

Poznámka: Metodu PUT nahradí celý produkt entity. To znamená klient se očekává odeslat znázornění dokončení aktualizované produktu. Pokud chcete, aby podporovaly částečné aktualizace, je upřednostňovanou metodu PATCH. Tento kurz neimplementuje opravy.

## <a name="create-a-new-web-api-project"></a>Vytvořte nový projekt webového rozhraní API

Začněte tím, že spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET MVC 4**. Název projektu &quot;ProductStore&quot; a klikněte na tlačítko **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image1.png)

V **nový ASP.NET MVC 4 projekt** dialogovém okně, vyberte **webového rozhraní API** a klikněte na tlačítko **OK**.

![](creating-a-web-api-that-supports-crud-operations/_static/image2.png)

## <a name="adding-a-model"></a>Přidání modelu

A *modelu* je objekt, který představuje data ve vaší aplikaci. V rozhraní ASP.NET Web API objektů se silným typem CLR můžete použít jako modely a budou se automaticky serializovat na XML nebo JSON pro klienta.

Pro rozhraní API ProductStore naše data se skládá z produktů, takže vytvoříme novou třídu s názvem `Product`.

Pokud Průzkumníku již není viditelný, klikněte na tlačítko **zobrazení** nabídku a vyberte **Průzkumníku řešení**. V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky. Z kontextu měny, vyberte **přidat**, pak vyberte **třída**. Název třídy &quot;produktu&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image3.png)

Přidejte následující vlastnosti pro `Product` třídy.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample1.cs)]

## <a name="adding-a-repository"></a>Přidání úložiště

Musíme uložení kolekce produkty. Je vhodné oddělení kolekci z naše implementace služby. Tímto způsobem jsme Změna úložiště zálohování bez přepisování třídu služby. Tento typ návrhu se nazývá *úložiště* vzor. Začněte definováním generické rozhraní pro úložiště.

V Průzkumníku řešení klikněte pravým tlačítkem myši **modely** složky. Vyberte **přidat**, pak vyberte **novou položku**.

![](creating-a-web-api-that-supports-crud-operations/_static/image4.png)

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte uzel C#. V části C#, vyberte **kód**. V seznamu šablon kód, vyberte **rozhraní**. Název rozhraní &quot;IProductRepository&quot;.

![](creating-a-web-api-that-supports-crud-operations/_static/image5.png)

Přidejte následující implementace:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample2.cs)]

Teď přidejte jinou třídu do složku modely, s názvem &quot;ProductRepository.&quot; Tato třída se implementovat `IProductRespository` rozhraní. Přidejte následující implementace:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample3.cs)]

Úložiště zajišťuje seznamu místní paměti. Tento kurz je v pořádku, ale v reálné aplikaci byste ukládat data externě, buď databází nebo do cloudového úložiště. Vzor úložiště bude usnadňují implementace později změnit.

## <a name="adding-a-web-api-controller"></a>Přidání Kontroleru webového rozhraní API

Pokud jste už pracovali s architekturou ASP.NET MVC, pak jste již obeznámeni s řadiči. V rozhraní ASP.NET Web API *řadič* je třída, která zpracovává požadavky HTTP z klienta. Průvodce nový projekt pro můžete vytvořit dva řadiče, vytvoření projektu. Pokud chcete zobrazit, je, rozbalte složku řadiče v Průzkumníku řešení.

- HomeController je řadič tradiční ASP.NET MVC. Zodpovídá za obsluhující stránky HTML pro lokalitu a přímo nesouvisí se naše webové rozhraní API.
- ValuesController je řadič WebAPI příklad.

Pokračujte a odstranit ValuesController, kliknutím pravým tlačítkem myši na soubor v Průzkumníku řešení a výběrem **odstranit.** Nyní přidejte nový řadič, následujícím způsobem:

V **Průzkumníku**, klikněte pravým tlačítkem myši složku řadiče. Vyberte **přidat** a pak vyberte **řadič**.

![](creating-a-web-api-that-supports-crud-operations/_static/image6.png)

V **přidat kontroler** průvodce, názvu kontroleru &quot;ProductsController&quot;. V **šablony** rozevíracího seznamu vyberte **prázdný kontroler API**. Pak klikněte na tlačítko **přidat**.

![](creating-a-web-api-that-supports-crud-operations/_static/image7.png)

> [!NOTE]
> Není nutné uvést vaše contollers do složky s názvem řadiče. Název složky není důležité. je jenom pohodlný způsob, jak uspořádat zdrojové soubory.


**Přidat kontroler** Průvodce vytvoří soubor s názvem ProductsController.cs ve složce řadiče. Pokud tento soubor už není otevřený, poklikejte na soubor otevřete. Přidejte následující **pomocí** příkaz:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample4.cs)]

Přidat pole, které obsahuje **IProductRepository** instance.

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample5.cs)]

> [!NOTE]
> Volání metody `new ProductRepository()` v kontroleru není optimální, protože se sváže řadičem pro konkrétní implementaci `IProductRepository`. Lepší způsob, najdete v článku [pomocí překladače závislostí webové rozhraní API](../advanced/dependency-injection.md).


## <a name="getting-a-resource"></a>Získávání prostředku

Rozhraní API ProductStore zveřejní několik &quot;číst&quot; akce jako metody GET protokolu HTTP. Každá akce bude odpovídat metoda v `ProductsController` třídy.

| Akce | Metoda HTTP | Relativní identifikátor URI |
| --- | --- | --- |
| Získání seznamu všech produktů | GET | / api/produkty |
| Získání ID produktu | GET | /API/produkty/*id* |
| Získání produktu podle kategorie | GET | produkty/api /? kategorie =*kategorie* |

Chcete-li získat seznam všech produktů, přidejte tuto metodu za účelem `ProductsController` třídy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample6.cs)]

Název metody začíná &quot;získat&quot;, takže podle konvence mapy pro požadavky GET. Navíc vzhledem k tomu, že metoda nemá žádné parametry, mapuje k identifikátoru URI, který neobsahuje  *&quot;id&quot;*  segmentu v cestě.

Chcete-li získat produktu podle ID, přidejte tuto metodu za účelem `ProductsController` třídy:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample7.cs)]

Tento název metody také začíná &quot;získat&quot;, ale metoda má parametr s názvem *id*. Tento parametr je namapována na &quot;id&quot; segment cesty identifikátoru URI. ID rozhraní ASP.NET Web API automaticky převede na správného datového typu (**int**) pro parametr.

Metoda GetProduct výjimku typu **HttpResponseException** Pokud *id* není platný. Tato výjimka se přeložit rámcem na chybu 404 (není nalezena).

Nakonec přidejte metody k vyhledání produkty podle kategorie:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample8.cs)]

Pokud řetězec dotazu identifikátoru URI požadavku, webového rozhraní API se pokusí vyhledat parametry dotazu na parametry na metodu řadiče. Proto identifikátoru URI ve tvaru "rozhraní api nebo produkty? kategorie =*kategorie*" bude mapovat tuto metodu.

## <a name="creating-a-resource"></a>Vytvoření prostředku

Potom přidáme metodu pro `ProductsController` třídy za účelem vytvoření nového produktu. Zde je jednoduchá implementace metody:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample9.cs)]

Všimněte si o této metodě dvě věci:

- Název metody začíná &quot;Post... &quot;. Pokud chcete vytvořit nového produktu, klient odešle požadavek HTTP POST.
- Tato metoda přebírá parametr typu produktu. V rozhraní Web API jsou v těle žádosti deserializovat parametry s komplexními typy. Proto Očekáváme, že klientovi umožní odeslat serializovaná reprezentace objekt produktu ve formátu XML nebo JSON.

Tato implementace budou fungovat, ale není poměrně dokončení. V ideálním případě by rádi bychom znali odpověď HTTP na zahrnují následující:

- **Kód odpovědi:** ve výchozím nastavení, rozhraní Web API nastaví stavový kód odpovědi 200 (OK). Ale podle protokol HTTP/1.1, když požadavek POST má za následek vytvoření prostředku, server by měl odpovědět stavem 201 (vytvořeno).
- **Umístění:** serveru vytvoří prostředek, měl by obsahovat identifikátor URI nového prostředku v hlavičce umístění odpovědi.

Rozhraní ASP.NET Web API usnadňuje zpracování zprávy s odpovědí HTTP. Tady je lepší implementace:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample10.cs)]

Všimněte si, že je návratový typ metody nyní **objekt HttpResponseMessage**. Vrácením **objekt HttpResponseMessage** místo produktu, jsme můžete řídit podrobnosti zprávy s odpovědí HTTP, včetně stavový kód a hlavičku Location.

**CreateResponse** metoda vytvoří **objekt HttpResponseMessage** a automaticky zapisuje serializovaná reprezentace produktu objektu do datové části fo zprávu odpovědi.

> [!NOTE]
> Nelze ověřit v tomto příkladu `Product`. Informace o ověření modelu naleznete v tématu [ověření modelu v rozhraní ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).


## <a name="updating-a-resource"></a>Aktualizuje prostředek

Aktualizace produktu pomocí PUT je jednoduchý:

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample11.cs)]

Název metody začíná &quot;Put... &quot;, takže ho webového rozhraní API odpovídá na požadavky PUT. Tato metoda přebírá dva parametry, ID produktu a aktualizované produktu. *Id* parametru jsou převzaty z cesta k identifikátoru URI a *produktu* v textu požadavku se deserializovat parametr. Ve výchozím nastavení trvá rozhraní ASP.NET Web API jednoduché typy parametrů na základě trasy a komplexní typy z textu žádosti.

## <a name="deleting-a-resource"></a>Odstranění prostředku

Chcete-li odstranit resourse, definujte metodu "Odstranit...".

[!code-csharp[Main](creating-a-web-api-that-supports-crud-operations/samples/sample12.cs)]

Pokud žádost o odstranění úspěšné, se může vrátit stav 200 (OK) s obsah entity popisující stav; Stav 202 (platných), pokud je stále odstranění čekající; stav nebo 204 (ne obsahu) s žádné obsahu entity. V takovém případě `DeleteProduct` metoda má `void` návratový typ, tak rozhraní ASP.NET Web API automaticky změní na stav to kód 204 (ne obsahu).
