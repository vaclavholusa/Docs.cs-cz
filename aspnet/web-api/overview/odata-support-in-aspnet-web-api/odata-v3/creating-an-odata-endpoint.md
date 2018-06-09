---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Vytváření koncový bod OData v3 s Web API 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) je protokol přístupu dat pro web. OData poskytuje uniform způsob, jak struktury dat, dotaz na data a manipulovat s daty...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 227faacd3f42731e08a4cd2b71075776309961b6
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874626"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Vytváření koncový bod OData v3 s Web API 2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Protokol](http://www.odata.org/) (OData) je protokol přístupu dat pro web. Poskytuje jednotným způsobem struktury dat, dotaz na data a manipulovat s sadu dat prostřednictvím operace CRUD OData (vytvářet, číst, aktualizovat a odstranit). OData podporuje jak formáty JSON a AtomPub (XML). OData také definuje způsob, jak vystavit metadata o datech. Klienti mohou používat metadata ke zjištění informací o typu a vztahů pro datovou sadu.
> 
> Rozhraní ASP.NET Web API usnadňuje vytvořit koncový bod OData pro datovou sadu. Můžete ovládat, přesně OData operací, které podporuje koncový bod. Je možné hostovat několik koncových bodů protokolu OData, spolu s koncových bodů protokolu OData. Máte plnou kontrolu nad vašeho datového modelu, back-end obchodní logiky a datové vrstvy.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6
> - [Webovou aplikaci Fiddler ladění Proxy (volitelné)](http://www.fiddler2.com)
> 
> Přidala se podpora web API OData v [ASP.NET a webové nástroje 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650). Tento kurz používá však generování uživatelského rozhraní, která byla přidána v sadě Visual Studio 2013.


V tomto kurzu vytvoříte jednoduchý koncový bod OData, který klienti mohou odesílat dotazy. Pokud vytvoříte klienta C# pro koncový bod. Po dokončení tohoto kurzu sadu další kurzy ukazují, jak přidat další funkce, včetně vztahy entit, akce, a vyberte možnost rozbalte $/ $.

- [Vytvoření projektu Visual Studio](#create-project)
- [Přidat Entity Model](#add-model)
- [Přidání Kontroleru OData](#add-controller)
- [Přidejte EDM a trasy](#edm)
- [Počáteční hodnoty databázi (volitelné)](#seed-db)
- [Zkoumání koncový bod OData](#explore)
- [Formáty serializace OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

V tomto kurzu vytvoříte koncový bod OData, který podporuje základní operace CRUD. Koncový bod zveřejní jediný zdroj, seznam produktů. Další kurzy přidá další funkce.

Spuštění sady Visual Studio a vyberte **nový projekt** z úvodní stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte uzel Visual C#. V části **Visual C#**, vyberte **webové**. Vyberte **webové aplikace ASP.NET** šablony.

![](creating-an-odata-endpoint/_static/image1.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony. V části &quot;přidat složky a odkazy na základní... &quot;, zkontrolujte **webového rozhraní API**. Click **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Přidat Entity Model

A *modelu* je objekt, který představuje data ve vaší aplikaci. V tomto kurzu potřebujeme model, který představuje produkt. Model odpovídá naše OData typu entity.

V Průzkumníku řešení klikněte pravým tlačítkem na složku modely. V místní nabídce vyberte **přidat** vyberte **třída**.

![](creating-an-odata-endpoint/_static/image3.png)

V **přidat nové** položky dialogové okno, název třídy &quot;produktu&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Podle konvence třídy modelu jsou umístěny ve složce modelů. Nemáte postupujte podle touto konvencí ve vašich vlastních projektů, ale použijeme ho v tomto kurzu.


V souboru Product.cs přidejte následující definici třídy:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Vlastnost ID bude klíč entity. Klienti mohou odesílat dotazy produkty podle ID. Toto pole by také primární klíč v databázi back-end.

Nyní sestavte projekt. V dalším kroku použijeme některé sady Visual Studio generování uživatelského rozhraní, která používá reflexe najít typ produktu.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Přidání Kontroleru OData

A *řadič* je třída, která zpracovává požadavky HTTP. Definujete samostatné řadič pro každou sadu entit v jste službu OData. V tomto kurzu vytvoříme jediného kontroleru.

V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat** a pak vyberte **řadič**.

![](creating-an-odata-endpoint/_static/image5.png)

V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte &quot;webové rozhraní API 2 kontroler OData s akcemi používající rozhraní Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

V **přidat kontroler** dialogové okno, názvu kontroleru "ProductsController". Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko. V **modelu** rozevíracího seznamu, vyberte třídu produktu.

![](creating-an-odata-endpoint/_static/image7.png)

Klikněte **nový kontext data...**  tlačítko. Ponechte výchozí název pro datový typ kontextu a klikněte na tlačítko **přidat**.

![](creating-an-odata-endpoint/_static/image8.png)

V dialogovém okně Přidat kontroler, přidat kontroler, klikněte na tlačítko Přidat.

![](creating-an-odata-endpoint/_static/image9.png)

Poznámka: Pokud se zobrazí chybové hlášení, uvádí &quot;došlo k chybě při načítání typu... &quot;, ujistěte se, jste vytvořili projekt Visual Studio, po přidání třídě produktu. Generování reflexe používá k nalezení třídy.

![](creating-an-odata-endpoint/_static/image10.png)

Generování uživatelského rozhraní přidá do projektu dva soubory kódu:

- Products.cs definuje kontroleru webového rozhraní API, který implementuje koncový bod OData.
- ProductServiceContext.cs poskytuje metody pro dotaz na základní databázi pomocí Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Přidejte EDM a trasy

V Průzkumníku řešení rozbalte aplikace\_spustit složku a otevřete soubor s názvem WebApiConfig.cs. Tato třída obsahuje kód konfigurace pro webové rozhraní API. Nahraďte tento kód s následujícími službami:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Tento kód provede dvě věci:

- Vytvoří modelu Entity Data Model (EDM) pro koncový bod OData.
- Přidá trasy pro koncový bod.

EDM je abstraktní model dat. EDM slouží k vytvoření dokument metadat a zadejte identifikátory URI pro službu. **ODataConventionModelBuilder** vytvoří EDM pomocí sadu výchozí zásady vytváření názvů EDM. Tento postup vyžaduje minimálně kódu. Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření modelu EDM přidáním vlastností, klíčů a navigačních vlastností explicitně.

**EntitySet** metoda přidá k modelu EDM sadu entit:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Řetězec "Produkty" definuje název sady entit. Název kontroleru musí odpovídat názvu sady entit. V tomto kurzu je sada entit s názvem "Produkty" a má název kontroleru `ProductsController`. Pokud jste s názvem "ProductSet" sady entit, by název kontroleru `ProductSetController`. Všimněte si, že koncový bod může mít několik sad entit. Volání **EntitySet&lt;T&gt;**  každé entity nastavení a potom zadejte odpovídající kontroler.

**MapODataRoute** metoda přidá trasu pro koncový bod OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

První parametr je popisný název pro tuto trasu. Klienti služby nejsou vidět tento název. Druhý parametr je předpony identifikátoru URI pro koncový bod. Daný tento kód, je identifikátor URI pro sadu entit produkty http://<em>hostname</em>  /odata/produkty. Vaše aplikace může mít více než jeden koncový bod OData. Pro každý koncový bod volání <strong>MapODataRoute</strong> a zadejte název jedinečné trasy a jedinečný předpony identifikátoru URI.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Počáteční hodnoty databázi (volitelné)

V tomto kroku použijete rozhraní Entity Framework počáteční hodnoty databáze s nějaká testovací data. Tento krok je volitelný, ale umožňuje vám hned otestovat váš koncový bod OData.

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Tento postup přidá složku s názvem migrace a kód soubor s názvem Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Umožňuje otevřít tento soubor a přidejte následující kód, který `Configuration.Seed` metoda.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

V okně konzoly Správce balíčků zadejte následující příkazy:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Tyto příkazy Generovat kód, který vytvoří databázi a potom provede tento kód.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Zkoumání koncový bod OData

V této části, použijeme [ladění Proxy webové aplikaci Fiddler](http://www.fiddler2.com) k odeslání požadavků na koncový bod a zkontrolujte zprávy odpovědi. To vám pomůže Seznamte se s možnostmi koncový bod OData.

Ve Visual Studiu stisknutím klávesy F5 spusťte ladění. Ve výchozím nastavení, Visual Studio otevře prohlížeč tak, aby `http://localhost:*port*`, kde *portu* je číslo portu, který je nakonfigurovaný v nastavení projektu.

Můžete změnit číslo portu v nastavení projektu. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**. V okně vlastností vyberte **webové**. Zadejte číslo portu v části **adresa Url projektu**.

### <a name="service-document"></a>Dokument služby

*Dokument služby* obsahuje seznam sad entit pro koncový bod OData. Získat dokument služby, odeslat požadavek GET na kořenové identifikátor URI služby.

Použijte aplikaci Fiddler, zadejte následující identifikátor URI v **autora** karta: `http://localhost:port/odata/`, kde *portu* je číslo portu.

![](creating-an-odata-endpoint/_static/image13.png)

Klikněte **Execute** tlačítko. Fiddler odešle požadavek HTTP GET do vaší aplikace. Měli byste vidět odpovědi v seznamu webové relace. Pokud se vše funguje stavový kód bude 200.

![](creating-an-odata-endpoint/_static/image14.png)

Dvakrát klikněte na odpovědi v seznamu webové relace a zobrazit podrobnosti zprávy s odpovědí na kartě kontroly.

![](creating-an-odata-endpoint/_static/image15.png)

Nezpracovaná zprávy s odpovědí HTTP by měl vypadat takto:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Ve výchozím nastavení webového rozhraní API vrátí dokumentu služby ve formátu AtomPub. Požádat o JSON, přidejte následující hlavičku požadavku HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Odpověď HTTP, která nyní obsahuje datovou část JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dokument metadat služby

*Dokument metadat služby* popisuje datový model služby, pomocí jazyka XML nazývaného koncepční schéma definice Language (CSDL). Dokument metadat ukazuje strukturu dat v rámci služby a slouží ke generování kódu klienta.

Chcete-li získat dokument metadat, odešlete požadavek GET na `http://localhost:port/odata/$metadata`. Zde je metadata pro koncový bod uvedené v tomto kurzu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Sada entit

Chcete-li získat sadu entit produkty, odeslat požadavek GET na `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entity

Potřebujete jednotlivých produktů, pošlete požadavek GET na `http://localhost:port/odata/Products(1)`, kde "1" je ID produktu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formáty serializace OData

OData podporuje několik formáty serializace:

- Atom Pub (XML)
- JSON "light" (dostupné v OData v3)
- JSON "podrobné" (OData v2)

Ve výchozím nastavení používá formát "light" AtomPubJSON webového rozhraní API. 

Formát AtomPub získáte nastavte hlavičku Accept "application/atom + xml". Tady je odpovědi na příkladu:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Zobrazí jednu zřejmé nevýhodou formát Atom: je mnohem podrobnější než JSON light. Ale pokud máte klienta, který jste srozuměni s tím AtomPub, klient může dáváte přednost tento formát přes JSON.

Tady je JSON light verzi stejné entity:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Formát JSON light byla zavedena v protokolu OData verze 3. Pro zpětnou kompatibilitu může klient požadovat starší "podrobný" formát JSON. Chcete-li požádat o podrobné JSON, nastavte hlavička Accept `application/json;odata=verbose`. Zde je podrobný verze:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Tento formát přenese tak další metadata v textu odpovědi, které lze přidat značné režijní náklady za celou relace. Kromě toho přidá úroveň dereference nástrojem pro zabalení objektu v vlastnost s názvem "d".

## <a name="next-steps"></a>Další kroky

- [Přidat vztahy entit](working-with-entity-relations.md)
- [Přidání akcí OData](odata-actions.md)
- [Volání služby OData z klienta .NET](calling-an-odata-service-from-a-net-client.md)
