---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Vytvoření koncového bodu OData v3 s webovým rozhraním API 2 | Dokumentace Microsoftu
author: MikeWasson
description: Open Data Protocol (OData) je protokol data access pro web. OData nabízí jednotné způsob, jak strukturovat data, zadávat dotazy na data a manipulaci s daty...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813701"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Vytvoření koncového bodu OData v3 s webovým rozhraním API 2
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Stáhnout dokončený projekt](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> [Open Data Protocol](http://www.odata.org/) (OData) je protokol data access pro web. OData nabízí jednotné způsob, jak strukturovat data, zadávat dotazy na data a manipulovat s sady dat prostřednictvím operace CRUD (vytváření, čtení, aktualizace a odstranění). OData podporuje formáty JSON a AtomPub (XML). OData definuje také způsob, jak vystavit metadata o datech. Klienti mohou používat metadata a zjistit informace o typu a vztahy pro datovou sadu.
> 
> Rozhraní ASP.NET Web API usnadňuje vytvoření koncového bodu OData pro datovou sadu. Můžete řídit přesně OData operace, které podporuje koncový bod. Můžete hostovat víc koncových bodů protokolu OData, společně s koncovými body mimo prostředí OData. Máte plnou kontrolu nad datový model, back-end obchodní logiku a data vrstev.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Webové rozhraní API 2
> - OData verze 3
> - Entity Framework 6
> - [Fiddler webový ladicí proxy server (volitelné)](http://www.fiddler2.com)
> 
> Přidala se podpora web API OData v [technologie ASP.NET a Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Tento kurz používá však generování uživatelského rozhraní, která byla přidána do sady Visual Studio 2013.


V tomto kurzu vytvoříte jednoduchou koncový bod OData, který můžou klienti dotazovat. Pokud vytvoříte klienta jazyka C# pro koncový bod. Po dokončení tohoto kurzu, další sadu kurzy ukazují, jak přidat další funkce, včetně vztahů entit, akce, a vyberte rozbalte $/ $.

- [Vytvoření projektu sady Visual Studio](#create-project)
- [Přidání modelu Entity](#add-model)
- [Přidat kontroler OData](#add-controller)
- [Přidat EDM a trasy](#edm)
- [Přidání dat do databáze (volitelné)](#seed-db)
- [Zkoumání koncového bodu OData](#explore)
- [Formáty serializace OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Vytvoření projektu sady Visual Studio

V tomto kurzu vytvoříte koncový bod OData, který podporuje základní operace CRUD. Koncový bod bude vystavovat jeden prostředek, seznam produktů. Dalších kurzech přidáte další funkce.

Spusťte sadu Visual Studio a vyberte **nový projekt** z úvodní stránky. Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** vyberte **nainstalované šablony** a rozbalte uzel Visual C#. V části **Visual C#** vyberte **webové**. Vyberte **webová aplikace ASP.NET** šablony.

![](creating-an-odata-endpoint/_static/image1.png)

V **nový projekt ASP.NET** dialogového okna, vyberte **prázdný** šablony. V části &quot;přidat složky a základní odkazy pro... &quot;, zkontrolujte **webového rozhraní API**. Klikněte na tlačítko **OK**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Přidání modelu Entity

A *modelu* je objekt, který představuje data ve vaší aplikaci. Pro účely tohoto kurzu potřebujeme model, který představuje produkt. Model odpovídá naše prostředí OData typu entity.

V Průzkumníku řešení klikněte pravým tlačítkem na složku modely. V místní nabídce vyberte **přidat** vyberte **třídy**.

![](creating-an-odata-endpoint/_static/image3.png)

V **přidat nový** položky dialogového okna, název třídy &quot;produktu&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Podle konvence tříd modelu jsou umístěny ve složce modely. Nemusíte si odpovídají této konvenci ve vašich vlastních projektů, ale použijeme ho pro účely tohoto kurzu.


V souboru Product.cs přidejte následující definici třídy:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

Vlastnost ID bude klíč entity. Klienti mohou odesílat dotazy produkty podle ID. Toto pole by také být primární klíč v back-end databáze.

Sestavte projekt. V dalším kroku použijeme některé sady Visual Studio generování uživatelského rozhraní, který používá reflexi najít typ produktu.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Přidat kontroler OData

A *řadič* je třída, která zpracovává požadavky HTTP. Definujte samostatný kontrolerem pro každou sadu entit v můžete službu OData. V tomto kurzu vytvoříme jediného kontroleru.

V Průzkumníku řešení klikněte pravým tlačítkem myši na složku řadiče. Vyberte **přidat** a pak vyberte **řadič**.

![](creating-an-odata-endpoint/_static/image5.png)

V **přidat vygenerované uživatelské rozhraní** dialogového okna, vyberte &quot;Kontroleru webového rozhraní API 2 OData s akcemi používající nástroj Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

V **přidat kontroler** dialogového okna, názvu kontroleru "ProductsController". Vyberte &quot;použít asynchronní akce kontroleru&quot; zaškrtávací políčko. V **modelu** rozevíracího seznamu vyberte třídu produktu.

![](creating-an-odata-endpoint/_static/image7.png)

Klikněte na tlačítko **nový kontext dat...**  tlačítko. Ponechte výchozí název typu datového kontextu a klikněte na **přidat**.

![](creating-an-odata-endpoint/_static/image8.png)

V dialogovém okně Přidat kontroler, přidat kontroler, klikněte na tlačítko Přidat.

![](creating-an-odata-endpoint/_static/image9.png)

Poznámka: Pokud se zobrazí chybová zpráva, která uvádí, že &quot;došlo k chybě při získávání typu... &quot;, ujistěte se, že jste sestavili projekt sady Visual Studio po přidání třídy produktu. Základní kostry aplikace používá reflexi k nalezení třídy.

![](creating-an-odata-endpoint/_static/image10.png)

Základní kostry aplikace přidá do projektu dva soubory kódu:

- Products.cs definuje kontroler Web API, která implementuje koncový bod OData.
- ProductServiceContext.cs poskytuje metody pro dotazování databáze pomocí Entity Frameworku.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Přidat EDM a trasy

V Průzkumníku řešení rozbalte aplikace\_spusťte složky a otevřete soubor s názvem WebApiConfig.cs. Tato třída obsahuje konfigurační kód pro webové rozhraní API. Nahraďte tento kód následujícím kódem:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Tento kód provede dvě věci:

- Vytvoří Entity Data Model (EDM) pro koncový bod OData.
- Přidá trasu pro koncový bod.

EDM je abstraktní modelem data. EDM slouží k vytvoření dokumentu metadata a definovat identifikátory URI pro službu. **ODataConventionModelBuilder** vytvoří EDM tak, že používáte sadu výchozích konvencí pojmenování EDM. Tento přístup vyžaduje nejméně kódu. Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření tak, že explicitně přidáte vlastností, klíčů a navigačních vlastností EDM.

**Objektu EntitySet** metoda přidá do modelu EDM sadu entit:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

Řetězec "Produktů" definuje název sady entit. Název kontroleru musí odpovídat názvu sady entit. V tomto kurzu je název sady entit s názvem "Produktů" a má název kontroleru `ProductsController`. Pokud pojmenujete "ProductSet" sada entit, bude název kontroleru `ProductSetController`. Všimněte si, že koncový bod může mít více sad entit. Volání **objektu EntitySet&lt;T&gt;**  pro každou entitu nastavení a pak definovat odpovídající kontroler.

**MapODataRoute** metoda přidá trasu pro koncový bod OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

První parametr je popisný název pro tuto trasu. Klienti služby se tento název nezobrazuje. Druhý parametr je předpona identifikátoru URI pro koncový bod. Zadaný tento kód, je identifikátor URI pro sadu entit produkty http://<em>hostname</em>  /odata/produktů. Vaše aplikace může mít více než jeden koncový bod OData. Pro každý koncový bod, volání <strong>MapODataRoute</strong> a zadejte název jedinečné cesty a jedinečný identifikátor URI předponu.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Přidání dat do databáze (volitelné)

V tomto kroku použijete rozhraní Entity Framework pro přidání dat do databáze s daty testu. Tento krok je volitelný, ale umožňuje vám to hned otestovat na váš koncový bod OData.

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Tento postup přidá složku s názvem migrace a s názvem Configuration.cs soubor kódu.

![](creating-an-odata-endpoint/_static/image12.png)

Otevřete tento soubor a přidejte následující kód, který `Configuration.Seed` metody.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

V okně konzoly Správce balíčků zadejte následující příkazy:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Tyto příkazy Generovat kód, který vytvoří databázi a potom tento kód spustí.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Zkoumání koncového bodu OData

V této části, použijeme [ladění Proxy webové aplikace Fiddler](http://www.fiddler2.com) posílat žádosti na koncový bod a zkontrolujte zprávy odpovědi. To vám pomůže pochopit možnosti koncový bod OData.

V sadě Visual Studio stisknutím klávesy F5 spusťte ladění. Ve výchozím nastavení, Visual Studio otevře prohlížeč na `http://localhost:*port*`, kde *port* je číslo portu, který je nakonfigurovaný v nastavení projektu.

Můžete změnit číslo portu v nastavení projektu. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**. V okně Vlastnosti vyberte **webové**. Zadejte číslo portu v rámci **adresa Url projektu**.

### <a name="service-document"></a>Dokument služby

*Dokument služby* obsahuje seznam ze sady entit pro koncový bod OData. Získat dokument služby, odeslat požadavek GET na kořenový identifikátor URI služby.

Použití aplikace Fiddler, zadejte následující identifikátor URI v **Composer** kartě: `http://localhost:port/odata/`, kde *port* je číslo portu.

![](creating-an-odata-endpoint/_static/image13.png)

Klikněte na tlačítko **Execute** tlačítko. Fiddler odesílá požadavek HTTP GET do vaší aplikace. Měli byste vidět odpovědi v seznamu webovými relacemi. Pokud všechno funguje, bude se stavovým kódem 200.

![](creating-an-odata-endpoint/_static/image14.png)

Dvakrát klikněte na panel odpovědi v seznamu webových relací a zobrazit podrobnosti zprávy s odpovědí na kartě kontroly.

![](creating-an-odata-endpoint/_static/image15.png)

Nezpracované zprávy s odpovědí HTTP by měl vypadat nějak takto:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

Ve výchozím nastavení webové rozhraní API vrátí dokument služby ve formátu AtomPub. Požádat o JSON, přidejte následující záhlaví požadavku HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Odpověď HTTP, která nyní obsahuje datovou část JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Dokument metadat služby

*Dokumentu metadat služby* popisuje datového modelu služby pomocí jazyka XML nazývaného Konceptuální schéma definici jazyka (CSDL). Dokument metadat znázorňuje strukturu dat ve službě a slouží ke generování kódu klienta.

K získání dokumentu metadata, odeslat požadavek GET na `http://localhost:port/odata/$metadata`. Tady jsou metadata pro koncový bod je znázorněno v tomto kurzu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Sada entit

Pokud chcete získat sadu entit produkty, odeslat požadavek GET na `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entity

Chcete-li získat jednotlivé produkty, odeslat požadavek GET na `http://localhost:port/odata/Products(1)`, kde "1" je ID produktu.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formáty serializace OData

OData podporuje několik formátů serializace:

- Publikování a odběru Atom (XML)
- JSON výraz "light" (představíme v OData v3)
- JSON "verbose" (OData v2)

Ve výchozím nastavení používá formát "výraz light" AtomPubJSON webového rozhraní API. 

Získat AtomPub formát, nastavte hlavičku Accept "application/atom + xml". Tady je text odpovědi příkladu:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Zobrazí se jednou z nevýhod zřejmé ve formátu Atom: je mnohem podrobnější než JSON light. Nicméně pokud máte klienta, která analyzuje AtomPub upřednostňují klienta tento formát přes JSON.

Tady je JSON light verze stejné entity:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

Formát JSON light byla zavedena ve verzi 3 protokolu OData. Z důvodu zpětné kompatibility může klient požadovat starší "verbose" formátu JSON. Požádat o podrobné JSON, nastavte hlavičku Accept na `application/json;odata=verbose`. Tady je podrobné verze:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Tento formát přenáší další metadata v textu odpovědi, které lze přidat značné režijní náklady za celou relaci. Kromě toho přidá určitou úroveň dereference obalením objektu ve vlastnosti s názvem "d".

## <a name="next-steps"></a>Další kroky

- [Přidání relace Entity](working-with-entity-relations.md)
- [Přidání akcí OData](odata-actions.md)
- [Volání služby OData z klienta .NET](calling-an-odata-service-from-a-net-client.md)
