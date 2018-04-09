---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Směrování adres URL | Microsoft Docs
author: Erikre
description: Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro jsme...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: a195b36517bcae4bbeaf43fe7386e7787fd00212
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="url-routing"></a>Směrování adres URL
====================
Podle [Erik Reitan](https://github.com/Erikre)

[Stáhnout adresář Wingtip Toys ukázkového projektu (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) nebo [stáhnout elektronická kniha (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Tento kurz řady naučit se základy vytváření aplikace webových formulářů ASP.NET pomocí technologie ASP.NET 4.5 a Microsoft Visual Studio Express 2013 pro Web. Visual Studio 2013 [projekt pomocí zdrojového kódu C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) dispozici je pro tento kurz řady.


V tomto kurzu budete upravovat adresář Wingtip Toys ukázkovou aplikaci pro podporu směrování adres URL. Směrování umožňuje webové aplikace pomocí adresy URL, které jsou popisný, usnadňují mějte na paměti a lépe nepodporuje vyhledávacích webů. V tomto kurzu vychází z předchozí kurz "A Správa členství" a je součástí řady kurz adresář Wingtip Toys.

## <a name="what-youll-learn"></a>Získáte informace:

- Postup registrace trasy pro aplikace webových formulářů ASP.NET.
- Jak přidat trasy na webovou stránku.
- Jak vybrat data z databáze pro podporu trasy.

## <a name="aspnet-routing-overview"></a>Přehled směrování ASP.NET

Směrování adres URL umožňuje nakonfigurovat aplikaci tak, aby přijímal žádosti adresy URL, které nelze namapovat na fyzické soubory. Adresu URL požadavku je jednoduše adresa URL, uživatel zadá do svého prohlížeče na stránce najít na webu. Používáte směrování k definování adresy URL, které jsou sémanticky smysl pro uživatele a které mohou pomoci s optimalizací vyhledávání (vyhledávací weby SEO).

Ve výchozím nastavení, šablony webových formulářů zahrnuje [přátelské adresy URL technologie ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Velká část základní Směrování práce se implementovaná pomocí *přátelské adresy URL*. V tomto kurzu přizpůsobené směrování možnosti přidáte.

Než začnete s přizpůsobováním směrování adres URL, adresář Wingtip Toys ukázkovou aplikaci můžete propojit produktu pomocí následující adresu URL:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Přizpůsobením směrování adres URL ukázkovou aplikaci adresář Wingtip Toys odkaz na stejnou produktu pomocí snadněji číst adresy URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Trasy

Trasa je vzor adresy URL, který je namapovaný na obslužnou rutinu. Obslužná rutina může být fyzického souboru, jako je soubor .aspx v aplikaci Web Forms. Obslužná rutina může být také třídu, která zpracuje požadavek. K definování trasy, vytvoření instance třídy trasy zadáním vzor adresy URL, obslužná rutina a volitelně název trasy.

Přidat trasy k aplikaci tak, že přidáte `Route` objekt, který má statické `Routes` vlastnost `RouteTable` třídy. Vlastnost trasy `RouteCollection` objektu, která ukládá všechny tras pro aplikaci.

### <a name="url-patterns"></a>Vzory adres URL

Vzor adresy URL může obsahovat hodnoty literály a zástupné symboly proměnných (označované jako parametry adresy URL). Literály a zástupné symboly umístěné v segmentů adresy URL, které jsou odděleny lomítkem (`/`) znaků.

Při požadavku na webovou aplikaci, adresa URL je analyzován do segmentů a zástupné symboly a hodnoty proměnných jsou dostupné na žádost o obslužnou rutinu. Tento proces je podobný jako data v řetězci dotazu je analyzovat a předaný obslužné rutiny požadavku. V obou případech se informace o proměnných v adrese URL a předaný obslužná rutina ve formě páry klíč hodnota. Pro řetězce dotazů klíče a hodnoty jsou v adrese URL. Pro trasy u klíčů se zástupný symbol názvy definované v vzor adresy URL a pouze hodnoty jsou v adrese URL.

Ve vzoru adresy URL definujete zástupné symboly uzavřené v závorkách ( `{` a `}` ). Můžete definovat více než jeden zástupný symbol v segmentu, ale zástupné symboly musí být odděleny literálovou hodnotou. Například `{language}-{country}/{action}` je vzor platná trasa. Ale `{language}{country}/{action}` není platný vzor, protože není hodnota literálu nebo odděleny zástupné symboly. Proto směrování nemůže určit, kde oddělit hodnotu zástupného textu jazyk z hodnoty pro zástupný symbol země.

### <a name="mapping-and-registering-routes"></a>Mapování a registraci trasy

Předtím, než mohou být trasy na stránky adresář Wingtip Toys ukázkové aplikace, je nutné zaregistrovat trasy při spuštění aplikace. Pokud chcete zaregistrovat trasy, můžete upravit `Application_Start` obslužné rutiny události.

1. V **Průzkumníku řešení**sady Visual Studio, najít a otevřít *Global.asax.cs* souboru.
2. Přidejte kód zvýrazněných v žlutý k *Global.asax.cs* následujícím způsobem:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Adresář Wingtip Toys ukázkové aplikace spustí, zavolá `Application_Start` obslužné rutiny události. Na konci této obslužné rutiny události `RegisterCustomRoutes` metoda je volána. `RegisterCustomRoutes` Metoda přidá každý postup voláním `MapPageRoute` metodu `RouteCollection` objektu. Trasy jsou definovány pomocí názvu trasy, adresa URL trasy a fyzické adresy URL.

První parametr ("`ProductsByCategoryRoute`") je název trasy. Slouží k volání trasy, pokud je to potřeba. Druhý parametr ("`Category/{categoryName}`") definuje popisný nahrazení adresa URL, která může být dynamické založené na kódu. Tato trasa se používají, když se vyplní ovládací prvek dat s odkazy, které jsou generované na základě dat. Trasa se zobrazí následujícím způsobem:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Druhý parametr trasy, která zahrnuje dynamické hodnoty zadané ve složených závorkách (`{ }`). V takovém případě `categoryName` je proměnná, která se použije k určení správné cestu směrování.

> [!NOTE] 
> 
> **Optional**
> 
> Může být snadněji spravovat váš kód přesunutím `RegisterCustomRoutes` metodu pro samostatné třídy. V *logiku* složky, vytvořte samostatné `RouteActions` třídy. Přesunout výše `RegisterCustomRoutes` metoda z *Global.asax.cs* souboru do nové `RoutesActions` třídy. Použití `RoleActions` třídy a `createAdmin` jako příklad toho, jak volat metodu `RegisterCustomRoutes` metoda z *Global.asax.cs* souboru.


Může také dojít k tomu `RegisterRoutes` pomocí volání metody `RouteConfig` objektu na začátku `Application_Start` obslužné rutiny události. Při volání implementovat výchozí směrování. Zahrnuta jako výchozí kód byl při vytvoření aplikace pomocí šablony sady Visual Studio webových formulářů.

## <a name="retrieving-and-using-route-data"></a>Načítání a používání dat trasy

Jak je uvedeno nahoře, může být definováno trasy. Kód, který jste přidali do `Application_Start` obslužné rutiny událostí v *Global.asax.cs* soubor načte definovatelným trasy.

### <a name="setting-routes"></a>Nastavení směrování

Trasy vyžadují můžete přidat další kód. V tomto kurzu budete používat vazby modelu k načtení `RouteValueDictionary` objekt, který se používá při generování tras pomocí dat z ovládacího prvku data. `RouteValueDictionary` Objektu bude obsahovat seznam názvů produktů, které patří do určité kategorie produktů. Vytvoří se propojení pro jednotlivé produkty na základě dat a trasy.

#### <a name="enable-routes-for-categories-and-products"></a>Povolit trasy pro kategorií a produkty

Dále budete aktualizovat aplikaci, aby používala `ProductsByCategoryRoute` k určení správné trasy, která má obsahovat pro každý odkaz kategorii produktu. Budete také aktualizovat *ProductList.aspx* stránky vložte směrované odkaz pro každý produkt. Odkazy se zobrazí jako byly před provedením změny, ale odkazy nyní využívat směrování adres URL.

1. V **Průzkumníku řešení**, otevřete *Site.Master* stránky, pokud ještě není otevřené.
2. Aktualizace **ListView** ovládací prvek s názvem "`categoryList`" se změnami zvýrazněných v žlutý, takže kód vypadat takto:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. V **Průzkumníku řešení**, otevřete *ProductList.aspx* stránky.
4. Aktualizace `ItemTemplate` element *ProductList.aspx* stránky s aktualizacemi zvýrazněných v žlutý, takže kód zobrazí následujícím způsobem:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Otevřete kódu z *ProductList.aspx.cs* a přidejte následující oboru názvů jako zvýrazněných v žlutý:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Nahraďte `GetProducts` metoda modelu code-behind (*ProductList.aspx.cs*) s následujícím kódem:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Přidejte kód pro podrobnosti o produktu

Nyní, aktualizovat modelu code-behind (*ProductDetails.aspx.cs*) pro *ProductDetails.aspx* stránky používat data trasy. Všimněte si, že nové `GetProduct` metoda přijímá také hodnotu řetězce dotazu pro případ, kde uživatel má odkaz aktuálně přihlášeni, který používá starší nepřátelských, nesměrovaný adresu URL.

1. Nahraďte `GetProduct` metoda modelu code-behind (*ProductDetails.aspx.cs*) s následujícím kódem:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Spuštění aplikace

Můžete spustit nyní aplikaci zobrazíte aktualizované trasy.

1. Stiskněte klávesu **F5** ke spuštění ukázkové aplikace adresář Wingtip Toys.  
 Otevře se prohlížeč a ukazuje *Default.aspx* stránky.
2. Klikněte **produkty** odkaz v horní části stránky.  
 Zobrazí se na všechny produkty *ProductList.aspx* stránky. Zobrazí se následující adresu URL (s použitím vaše číslo portu) pro prohlížeč:  
    `https://localhost:44300/ProductList`
3. Klikněte na tlačítko **aut** kategorie odkaz v horní části stránky.  
 Zobrazí se pouze automobilů na *ProductList.aspx* stránky. Zobrazí se následující adresu URL (s použitím vaše číslo portu) pro prohlížeč:  
    `https://localhost:44300/Category/Cars`
4. Kliknutím na odkaz, který obsahuje název prvního auta uvedené na stránce ("**převoditelné Car**") Chcete-li zobrazit podrobnosti o produktu.  
 Zobrazí se následující adresu URL (s použitím vaše číslo portu) pro prohlížeč:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Potom zadejte následující nesměrovaný adresu URL (s použitím vaše číslo portu) do prohlížeče:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Kód stále rozpozná adresu URL, která obsahuje řetězec dotazu pro případ, kdy uživatel, který má odkaz aktuálně přihlášeni.

## <a name="summary"></a>Souhrn

V tomto kurzu jste přidali trasy pro kategorií a produkty. Jste se naučili, jak trasy lze integrovat s ovládacími prvky dat, které používají vazby modelu. V dalším kurzu budete implementovat zpracování globální chyb.

## <a name="additional-resources"></a>Další prostředky

[ASP.NET přátelské adresy URL](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Nasazení aplikace zabezpečení rozhraní ASP.NET Web Forms členství, OAuth a databáze SQL Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure – bezplatná zkušební verze](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Předchozí](membership-and-administration.md)
> [další](aspnet-error-handling.md)
