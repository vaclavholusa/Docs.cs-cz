---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Směrování adres URL | Dokumentace Microsoftu
author: Erikre
description: V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 556ef01304d0b5a3cca3606d71ef055ce4b2dc5c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389147"
---
<a name="url-routing"></a>Směrování adres URL
====================
podle [Erik Reitan](https://github.com/Erikre)

[Stáhněte si ukázkový projekt Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronickou knihu (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> V této sérii kurzů se seznámíte se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a službu Microsoft Visual Studio Express 2013 for Web. Visual Studio 2013 [projektu se zdrojovým kódem jazyka C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) je k dispozici v této sérii kurzů.


V tomto kurzu budete upravovat ukázkové aplikace Wingtip Toys pro podporu směrování adres URL. Směrování umožňuje webové aplikace pomocí adresy URL, které jsou popisný usnadňuje mějte na paměti a lépe podporuje vyhledávací weby. V tomto kurzu vychází z předchozí kurz o službě "Členství a správa" a je součástí série kurzů na adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Co se dozvíte:

- Postup při registraci trasy pro aplikace webových formulářů ASP.NET.
- Postup přidání tras na webovou stránku.
- Jak vybrat data z databáze pro podporu trasy.

## <a name="aspnet-routing-overview"></a>Přehled směrování ASP.NET

Směrování adres URL umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Adresa URL požadavku je jednoduše adresu URL, které uživatel zadá do svého prohlížeče najděte požadovanou stránku na webu. Používáte směrování k definování adresy URL, která jsou sémanticky srozumitelné pro uživatele a, které mohou pomoci s optimalizací vyhledávače (SEO).

Ve výchozím nastavení, zahrnuje šablony webových formulářů [přátelské adresy URL technologie ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Velkou část základního Směrování práce se dají implementovat pomocí *přátelské adresy URL*. V tomto kurzu upravenými možnostmi směrování přidáte.

Než začnete s přizpůsobováním směrování adres URL, ukázkové aplikace Wingtip Toys můžete propojit s produktem pomocí následující adresy URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Přizpůsobením směrování adres URL ukázkové aplikace Wingtip Toys odkaz na stejný produkt pomocí snadněji číst adresy URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Trasy

Cesta je vzor adresy URL, který je namapovaný na obslužnou rutinu. Obslužná rutina může být fyzický soubor, třeba soubor .aspx v aplikaci webových formulářů. Obslužná rutina může být také třída, která zpracuje požadavek. Definovat trasu, vytvoření instance třídy směrování tak, že zadáte vzor adresy URL, obslužné rutiny a volitelně název pro tuto trasu.

Přidat trasu k aplikaci tak, že přidáte `Route` objekt statické `Routes` vlastnost `RouteTable` třídy. Vlastnost trasy je `RouteCollection` objekt, který uchovává všechny trasy pro aplikaci.

### <a name="url-patterns"></a>Vzory adres URL

Vzor adresy URL může obsahovat literály a zástupné symboly proměnných (označované jako parametry adresy URL). Literály a zástupné symboly jsou umístěny v segmentů adresy URL, které jsou oddělené lomítkem (`/`) znaků.

Po provedení požadavku na webovou aplikaci, adresa URL je analyzován do segmentů a zástupné symboly a hodnoty proměnných jsou k dispozici pro obslužnou rutinu požadavků. Tento proces je podobný způsob, jak je analyzovat a předat obslužné rutiny žádosti o data v řetězci dotazu. V obou případech se informace o proměnných do adresy URL a předané do rutiny v podobě dvojic klíč / hodnota. Pro řetězce dotazu klíče a hodnoty jsou v adrese URL. Pro trasy klíče jsou zástupné názvy definovanou ve vzoru adresy URL a jsou pouze hodnoty v adrese URL.

Ve vzoru adresy URL můžete definovat zástupné symboly uzavřením do složených závorek ( `{` a `}` ). Můžete definovat více než jeden zástupný symbol v segmentu, ale zástupné symboly musí být odděleny literálovou hodnotou. Například `{language}-{country}/{action}` je vzor platnou trasu. Nicméně `{language}{country}/{action}` není platný vzor, protože není žádná hodnota literálu nebo oddělovač mezi zástupné symboly. Proto směrování nemůže určit, kde k oddělení hodnotu pro zástupný symbol jazyka z hodnoty pro zástupný text země.

### <a name="mapping-and-registering-routes"></a>Mapování a registrace trasy

Předtím, než mohou zahrnovat trasy na stránky ukázkové aplikace Wingtip Toys, je nutné zaregistrovat trasy při spuštění aplikace. K registraci trasy, upravíte `Application_Start` obslužné rutiny události.

1. V **Průzkumníka řešení**sady Visual Studio, najít a otevřít *Global.asax.cs* souboru.
2. Přidejte kód zvýrazněné žlutou barvou na *Global.asax.cs* to následujícím způsobem:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Wingtip Toys ukázkové aplikace spustí, zavolá `Application_Start` obslužné rutiny události. Na konci této obslužné rutiny události `RegisterCustomRoutes` metoda je volána. `RegisterCustomRoutes` Metoda přidá každý postup voláním `MapPageRoute` metodu `RouteCollection` objektu. Trasy jsou definovány pomocí názvu trasy, adresa URL trasy a fyzickou adresu URL.

První parametr ("`ProductsByCategoryRoute`") je název trasy. Používá se k volání trasy, pokud je to potřeba. Druhý parametr ("`Category/{categoryName}`") definuje popisný nahrazení adresu URL, která může být dynamické na základě kódu. Když plníte ovládací prvek dat s odkazy, které jsou generovány na základě dat použijete tuto trasu. Trasa se zobrazí takto:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Druhý parametr trasy, která zahrnuje dynamické hodnoty zadané ve složených závorkách (`{ }`). V takovém případě `categoryName` je proměnná, která se použije k určení správné cestu směrování.

> [!NOTE] 
> 
> **Optional**
> 
> Vám může být jednodušší ke správě vašeho kódu tak, `RegisterCustomRoutes` metodu pro samostatné třídy. V *logiky* složku, vytvořte samostatné `RouteActions` třídy. Přesunout výše `RegisterCustomRoutes` metodu z *Global.asax.cs* souboru do nové `RoutesActions` třídy. Použití `RoleActions` třídy a `createAdmin` jako příklad toho, jak volat metodu `RegisterCustomRoutes` metodu z *Global.asax.cs* souboru.


Také jste si všimli `RegisterRoutes` pomocí volání metody `RouteConfig` objekt na začátku `Application_Start` obslužné rutiny události. Při volání k implementaci výchozí směrování. Byl zahrnutý jako výchozí kód, když vytvoříte aplikaci pomocí šablony webových formulářů v sadě Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Načítání a používá Data trasy

Jak je uvedeno výše, lze definovat trasy. Kód, který jste přidali do `Application_Start` obslužné rutině událostí ve *Global.asax.cs* trasy definovatelné načtení souboru.

### <a name="setting-routes"></a>Nastavení trasy

Trasy vyžadují, abyste přidejte další kód. V tomto kurzu použijete vazby modelu pro načtení `RouteValueDictionary` objekt, který se používá při generování tras pomocí dat z ovládacího prvku. `RouteValueDictionary` Objekt bude obsahovat seznam názvů produktů, které patří do určité kategorie produktů. Odkaz se vytvoří pro každý produkt na základě dat a směrování.

#### <a name="enable-routes-for-categories-and-products"></a>Povolit trasy pro kategorie a produkty

Dále budete aktualizovat aplikaci, aby používala `ProductsByCategoryRoute` určit správné trasy, která má zahrnout pro každý odkaz kategorie produktu. Také aktualizujeme *ProductList.aspx* stránky, aby zahrnovala směrované odkaz pro jednotlivé produkty. Odkazy se zobrazí, jako kdyby byly před změnou, ale odkazy budou nyní využívat směrování adres URL.

1. V **Průzkumníka řešení**, otevřete *Site.Master* stránky, pokud ještě není otevřený.
2. Aktualizace **ListView** ovládací prvek s názvem "`categoryList`" změnami zvýrazněné žlutou barvou, takže značky se zobrazí takto:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. V **Průzkumníka řešení**, otevřete *ProductList.aspx* stránky.
4. Aktualizace `ItemTemplate` elementu *ProductList.aspx* stránce s aktualizacemi zvýrazněné žlutou barvou, značky se zobrazí takto:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Otevřete kódu z *ProductList.aspx.cs* a přidat následující obor názvů v zvýrazněné žlutou barvou:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Nahradit `GetProducts` metoda modelu code-behind (*ProductList.aspx.cs*) následujícím kódem:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Přidejte kód pro podrobnosti o produktu

Nyní, aktualizace modelu code-behind (*ProductDetails.aspx.cs*) pro *ProductDetails.aspx* stránku data trasy. Všimněte si, že nový `GetProduct` metoda přijímá také hodnotu řetězce dotazu pro případ, kde má uživatel vytvořili záložku na odkaz, který používá starší nepřátelských, nesměrovaný adresu URL.

1. Nahradit `GetProduct` metoda modelu code-behind (*ProductDetails.aspx.cs*) následujícím kódem:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizace tras.

1. Stisknutím klávesy **F5** ke spuštění ukázkové aplikace Wingtip Toys.  
 V prohlížeči se otevře a zobrazí *Default.aspx* stránky.
2. Klikněte na tlačítko **produkty** odkazu v horní části stránky.  
 Všechny produkty jsou zobrazeny v *ProductList.aspx* stránky. Následující adresu URL (vaše číslo portu) se zobrazí v prohlížeči:  
    `https://localhost:44300/ProductList`
3. Klikněte **auta** kategorie propojení v horní části stránky.  
 Jenom auta se zobrazují na *ProductList.aspx* stránky. Následující adresu URL (vaše číslo portu) se zobrazí v prohlížeči:  
    `https://localhost:44300/Category/Cars`
4. Klikněte na odkaz obsahující název prvního auta najdete na stránce ("**převoditelné Car**") Chcete-li zobrazit podrobnosti o produktu.  
 Následující adresu URL (vaše číslo portu) se zobrazí v prohlížeči:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Pak zadejte následující nesměrovaný adresu URL (vaše číslo portu) do prohlížeče:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kód stále rozpozná adresu URL, která obsahuje řetězec dotazu pro případ, ve kterém má odkaz záložek.

## <a name="summary"></a>Souhrn

V tomto kurzu jste přidali trasy pro kategorie a produkty. Jste se naučili, jak je možné integrovat trasy s ovládacími prvky dat, které používají vazby modelu. V dalším kurzu budete implementovat zpracování globální chyb.

## <a name="additional-resources"></a>Další prostředky

[ASP.NET přátelské adresy URL](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Nasaďte aplikaci zabezpečení rozhraní ASP.NET Web Forms s nástroji Membership, OAuth a SQL Database do služby Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Předchozí](membership-and-administration.md)
> [další](aspnet-error-handling.md)
