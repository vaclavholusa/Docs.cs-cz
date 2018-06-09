---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Vytvořit koncový bod OData v4 pomocí rozhraní ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) je protokol přístupu dat pro web. OData poskytuje jednotným způsobem pro dotazování a zpracování datových sad prostřednictvím operace CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566719"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Vytvořit koncový bod OData v4 pomocí rozhraní ASP.NET Web API 2.2
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Open Data Protocol (OData) je protokol přístupu dat pro web. Poskytuje jednotným způsobem pro dotazování a zpracování datových sad prostřednictvím operace CRUD OData (vytvářet, číst, aktualizovat a odstranit).
> 
> Rozhraní ASP.NET Web API podporuje v3 a v4 protokolu. Můžete mít i v4 koncový bod, který běží souběžně sdílená s koncovým bodem v3.
> 
> Tento kurz ukazuje, jak vytvořit koncový bod OData v4, který podporuje operace CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> OData verze 3, najdete v části [vytváření koncový bod OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Vytvoření projektu Visual Studio

V sadě Visual Studio z **soubor** nabídce vyberte možnost **nový** &gt; **projektu**.

Rozbalte položku **nainstalovaná** &gt; **šablony** &gt; **Visual C#** &gt; **webové**a vyberte  **Webové aplikace ASP.NET** šablony. Název projektu &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

V **nový projekt** dialogovém okně, vyberte **prázdný** šablony. V části &quot;přidat složky a odkazy na základní... &quot;, klikněte na tlačítko **webového rozhraní API**. Click **OK**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Instalovat balíčky OData

Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Tento příkaz nainstaluje nejnovější balíčky OData NuGet.

## <a name="add-a-model-class"></a>Přidejte třídu modelu

A *modelu* je objekt, který představuje data entity ve vaší aplikaci.

V Průzkumníku řešení klikněte pravým tlačítkem na složku modely. V místní nabídce vyberte **přidat** &gt; **třída**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Podle konvence třídy modelu jsou umístěny ve složce modely, ale nemáte postupujte podle touto konvencí ve vašich vlastních projektů.


Název třídy `Product`. V souboru Product.cs nahraďte často používaný kód následující:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

`Id` Je vlastnost klíčem entity. Klienti mohou odesílat dotazy entit podle klíče. Například pokud chcete získat produktu s ID 5, je identifikátor URI `/Products(5)`. `Id` Vlastnost bude také primární klíč v databázi back-end.

## <a name="enable-entity-framework"></a>Povolit rozhraní Entity Framework

V tomto kurzu použijeme Entity Framework (EF) Code First k vytvoření databáze back-end.

> [!NOTE]
> Web API OData nevyžaduje EF. Použijte všechny vrstva přístupu k datům, která může překládat entity databáze do modely.


Nejdřív nainstalujte balíček NuGet pro EF. Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet** &gt; **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Otevřete soubor Web.config a přidejte následující části uvnitř **konfigurace** element, po **configSections** elementu.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Toto nastavení přidá připojovací řetězec databáze LocalDB. Tato databáze se použije při spuštění aplikace místně.

V dalším kroku přidejte třídu s názvem `ProductsContext` ke složce modely:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

V konstruktoru `"name=ProductsContext"` poskytuje název připojovacího řetězce.

## <a name="configure-the-odata-endpoint"></a>Konfigurace koncového bodu OData

Otevřete soubor aplikace\_Start/WebApiConfig.cs. Přidejte následující **pomocí** příkazy:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

Pak přidejte následující kód, který **zaregistrovat** metoda:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Tento kód provede dvě věci:

- Vytvoří datový Model Entity (EDM).
- Přidá trasy.

EDM je abstraktní model dat. EDM se používá k vytvoření dokumentu metadat služby. **ODataConventionModelBuilder** třída vytvoří EDM pomocí výchozí zásady vytváření názvů. Tento postup vyžaduje minimálně kódu. Pokud chcete mít větší kontrolu nad EDM, můžete použít **Tvůrce ODataModelBuilder** třídy za účelem vytvoření modelu EDM přidáním vlastností, klíčů a navigačních vlastností explicitně.

A *trasy* informuje webového rozhraní API, jak pro směrování požadavků HTTP na koncový bod. Chcete-li vytvořit trasu OData v4, zavolejte **MapODataServiceRoute** metoda rozšíření.

Pokud aplikace obsahuje více koncových bodů protokolu OData, vytvořte pro každý samostatný trasy. Každý postup poskytněte název jedinečný trasy a předponu.

## <a name="add-the-odata-controller"></a>Přidat kontroler OData

A *řadič* je třída, která zpracovává požadavky HTTP. Můžete vytvořit samostatný řadič pro každou sadu entit v služby OData. V tomto kurzu vytvoříte jeden řadič pro `Product` entity.

V Průzkumníku řešení klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** &gt; **třída**. Název třídy `ProductsController`.

> [!NOTE]
> Verze tohoto kurzu pro OData v3 používá **přidat kontroler** generování uživatelského rozhraní. V současné době není k dispozici žádné generování uživatelského rozhraní pro OData v4.


Nahraďte kód často používaný v ProductsController.cs následující.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Používá řadič `ProductsContext` třídy pro přístup k databázi pomocí EF. Všimněte si, že přepíše řadičem **Dispose** metodu pro odstranění **ProductsContext**.

Toto je výchozí bod pro kontroler. Potom přidáme metody pro všechny operace CRUD.

## <a name="querying-the-entity-set"></a>Dotaz na sadu entit

Přidejte následující metody, které `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

Bez parametrů verzi `Get` metoda vrátí celou kolekci produkty. `Get` Metoda s *klíč* parametr vyhledá produkt na základě klíče (v takovém případě `Id` vlastnost).

**[EnableQuery]** atribut umožňuje klientům upravit dotaz, pomocí možnosti dotazu, jako je například $filter, $sort a $page. Další informace najdete v tématu [podporu možností dotazu OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Přidání Entity do sady entit

Povolit klientům přidání nového produktu do databáze, přidejte následující metodu do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Aktualizace Entity

OData podporuje dva různé sémantiku pro aktualizaci entity, opravy a PUT.

- OPRAVA provede částečné aktualizace. Klient určuje vlastnosti aktualizace.
- PUT nahradí celý entity.

Nevýhodou PUT je, že klient musí odeslat hodnoty pro všechny vlastnosti entity, včetně hodnot, které nejsou změna. [OData specifikace](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) uvádí, že je upřednostňovaný opravy.

V každém případě tady je kód pro opravy a PUT metody:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

V případě opravy, používá řadičem **rozdílů&lt;T&gt;**  typ sledovat změny.

## <a name="deleting-an-entity"></a>Odstranění Entity

Pokud chcete povolit klientům z databáze odstranit produktu, přidejte následující metodu do `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
