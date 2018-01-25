---
uid: mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
title: "Zkoumání upravit metody a zobrazení | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: 52a4d5fe-aa31-4471-b3cb-a064f82cb791
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-edit-methods-and-edit-view
msc.type: authoredcontent
ms.openlocfilehash: d7e1ba503b8aa815cebf431d2f5ffc9436b3575b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="examining-the-edit-methods-and-edit-view"></a>Zkoumání upravit metody a zobrazení
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

V této části budete zkontrolujte vygenerovaného `Edit` zobrazení pro řadič film a metody akce. Ale nejprve bude trvat krátké zneužívání aby datum vydání vypadat lépe. Otevřete *Models\Movie.cs* souboru a přidejte zvýrazněné řádky vidíte níže:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample1.cs?highlight=2,12-14)]

Můžete provést také jazykovou verzi datum konkrétní takto:

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample2.cs?highlight=3)]

Budeme se zabývat těmito tématy [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) v dalším kurzu. [Zobrazit](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayattribute.aspx) atribut určuje, co má být zobrazen pro název pole (v tomto případě "Datum vydání" místo "ReleaseDate"). [Datový typ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atribut určuje typ dat, v takovém případě je datum, a proto není zobrazit čas informace uložené v poli. [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atribut je potřebný pro chyby v prohlížeči Chrome, který vykreslí dat. formáty dat nesprávně.

Spusťte aplikaci a přejděte do `Movies` řadiče. Podržte ukazatel myši nad **upravit** na adresu URL, která obsahuje odkazy na odkaz.

![EditLink_sm](examining-the-edit-methods-and-edit-view/_static/image1.png)

**Upravit** odkaz vygenerovalo `Html.ActionLink` metoda v *Views\Movies\Index.cshtml* zobrazení:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample3.cshtml)]

![Html.ActionLink](examining-the-edit-methods-and-edit-view/_static/image2.png)

`Html` Objekt je pomocné rutiny, který je zveřejněný pomocí vlastnosti na [System.Web.Mvc.WebViewPage](https://msdn.microsoft.com/library/gg402107(VS.98).aspx) základní třídy. `ActionLink` Metoda pomocníka, který usnadňuje dynamicky generovat hypertextové odkazy HTML, které odkazují na metody akce na řadiče. První argument `ActionLink` metoda je text odkazu k vykreslení (například `<a>Edit Me</a>`). Druhý argument je název metody akce, která bude vyvolána (v takovém případě `Edit` akce). Konečný argument je [anonymní objekt](https://weblogs.asp.net/scottgu/archive/2007/05/15/new-orcas-language-feature-anonymous-types.aspx) který generuje data trasy (v tomto případě ID 4).

Vygenerovaný odkaz viz předchozí obrázek je `http://localhost:1234/Movies/Edit/4`. Výchozí trasa (nastavené v *aplikace\_Start\RouteConfig.cs*) trvá vzor adresy URL `{controller}/{action}/{id}`. Proto ASP.NET překládá `http://localhost:1234/Movies/Edit/4` do požadavek na `Edit` metody akce `Movies` řadiče s parametrem `ID` rovna 4. Zkontrolujte následující kód *aplikace\_Start\RouteConfig.cs* souboru. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda se používá ke směrování požadavků HTTP do správné metody kontroleru a akce a zadat volitelný parametr ID. [MapRoute](../../older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs.md) metoda také používány [HtmlHelpers](https://msdn.microsoft.com/library/system.web.mvc.htmlhelper(v=vs.108).aspx) například `ActionLink` k vygenerování adres URL daného kontroleru, metoda akce a všechna data trasy.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample4.cs?highlight=7)]

Také můžete předat parametry metody akce pomocí řetězce dotazu. Například adresa URL `http://localhost:1234/Movies/Edit?ID=3` také předá parametr `ID` 3 k `Edit` metody akce `Movies` řadiče.

![EditQueryString](examining-the-edit-methods-and-edit-view/_static/image3.png)

Otevřete `Movies` řadiče. Dva `Edit` metody akce, které jsou uvedeny níže.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample5.cs?highlight=19-21)]

Všimněte si, druhý `Edit` předchází metody akce `HttpPost` atribut. Tento atribut určuje, že přetížení `Edit` metoda může být volána pouze pro požadavky POST. Je možné aplikovat `HttpGet` atribut prvního upravit metodu, ale který není nutné protože je výchozí hodnota. (Budeme označovat metody akce, které jsou implicitně přiřazené `HttpGet` atribut jako `HttpGet` metody.) [Vazby](https://msdn.microsoft.com/library/system.web.mvc.bindattribute(v=vs.108).aspx) atribut je jiný mechanismus důležité zabezpečení, který udržuje hackery z typu overpost data modelu. Vlastnosti by měla obsahovat pouze vazby atributu, který chcete změnit. Další informace o overposting a atribut vazby v mé [overposting Poznámka k zabezpečení](https://go.microsoft.com/fwlink/?LinkId=317598). V modelu jednoduché použili v tomto kurzu jsme závazné všechna data v modelu. [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) atribut se používá k ochraně před paděláním požadavku a je spárován s `@Html.AntiForgeryToken()` v souboru zobrazení upravit (*Views\Movies\Edit.cshtml*), část je zobrazena níže:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample6.cshtml?highlight=9)]

`@Html.AntiForgeryToken()`generuje token proti padělání skrytého, který se musí shodovat s v `Edit` metodu `Movies` řadiče. Další informace o webů požadavku padělání (také označované jako XSRF nebo proti útokům CSRF) v mé kurzu [XSRF proti útokům CSRF nebo zabránění v MVC](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md).

`HttpGet` `Edit` Metoda přebírá parametr ID film, vyhledává film používající rozhraní Entity Framework `Find` metoda a vrátí vybraného videa na zobrazení pro úpravy. Pokud nelze nalézt film, [HttpNotFound](https://msdn.microsoft.com/library/gg453938(VS.98).aspx) je vrácen. Při generování uživatelského rozhraní systému vytvoření zobrazení pro úpravy, zkontrolován `Movie` třídy a vytvořený kód k vykreslení `<label>` a `<input>` prvky pro každou vlastnost třídy. Následující příklad ukazuje zobrazení úpravy, který byl vytvořen v sadě visual studio generování uživatelského rozhraní systému:

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample7.cshtml)]

Všimněte si, jak se má zobrazit šablonu `@model MvcMovie.Models.Movie` příkaz v horní části souboru – tato hodnota určuje, že zobrazení očekává, že model pro zobrazení šablony být typu `Movie`.

Automaticky generovaný kód používá několik *pomocné metody* zefektivnění kód HTML. [ `Html.LabelFor` ](https://msdn.microsoft.com/library/gg401864(VS.98).aspx) Pomocné rutiny zobrazuje název pole (&quot;název&quot;, &quot;ReleaseDate&quot;, &quot;Genre&quot;, nebo &quot;cena &quot;). [ `Html.EditorFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.editorextensions.editorfor(VS.98).aspx) Vykreslí pomocné rutiny HTML `<input>` elementu. [ `Html.ValidationMessageFor` ](https://msdn.microsoft.com/library/system.web.mvc.html.validationextensions.validationmessagefor(VS.98).aspx) Pomocník zobrazí všechny zprávy ověření přidružené k této vlastnosti.

Spusťte aplikaci a přejděte do */Movies* adresy URL. Klikněte na tlačítko **upravit** odkaz. V prohlížeči zobrazte zdroj pro stránku. Kód HTML pro form element jsou uvedeny níže.

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample8.cshtml?highlight=1-2)]

`<input>` Prvky jsou v kódu HTML `<form>` element jejichž `action` k vystavování příspěvků na nastavený atribut *nebo filmy nebo upravit* adresy URL. Data formuláře budou odeslány na server při **Uložit** po kliknutí na tlačítko. Druhý řádek ukazuje skrytého [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token vygenerované `@Html.AntiForgeryToken()` volání.

## <a name="processing-the-post-request"></a>Zpracování požadavku POST

Následující seznam uvádí `HttpPost` verzi `Edit` metody akce.

[!code-csharp[Main](examining-the-edit-methods-and-edit-view/samples/sample9.cs)]

[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.108).aspx) ověří atribut [XSRF](../../security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages.md) token vygenerované `@Html.AntiForgeryToken()` volání v zobrazení.

[Vazač modelu ASP.NET MVC](https://msdn.microsoft.com/library/dd410405.aspx) přijímá hodnoty odeslaného formuláře a vytvoří `Movie` objekt, který se předá jako `movie` parametr. `ModelState.IsValid` Metoda ověří, že odeslaná data do formuláře lze použít k úpravě (upravit nebo aktualizace) `Movie` objektu. Pokud jsou data platná, film data jsou uložena do `Movies` kolekce `db(MovieDBContext` instance). Nová data film je uložit do databáze při volání `SaveChanges` metodu `MovieDBContext`. Po uložení dat, kód přesměruje uživatele `Index` metody akce `MoviesController` třídy, která zobrazuje kolekce film, včetně právě změny.

Při ověřování na straně klienta zjistí, že hodnoty pole nejsou platné, zobrazí se chybová zpráva. Pokud zakážete JavaScript, nebude mít ověřování na straně klienta, ale server bude zjišťovat odeslaných hodnot nejsou platné, a hodnot formuláře se zobrazí znovu s chybové zprávy. Později v tomto kurzu jsme zkontrolujte ověření podrobněji.

`Html.ValidationMessageFor` Pomocné rutiny v *Edit.cshtml* zobrazení šablony postará o zobrazení příslušné chybové zprávy.

![abcNotValid](examining-the-edit-methods-and-edit-view/_static/image4.png)

Všechny `HttpGet` podobný Princip podle metody. Získají objekt film (nebo seznam objektů, v případě `Index`) a předat zobrazení modelu. `Create` Metoda předá objekt prázdný film zobrazení pro vytváření. Takže v udělat všechny metody, které vytvořit, upravit, odstranit nebo jinak měnit data `HttpPost` přetížení metody. Úprava dat v metodu GET protokolu HTTP je bezpečnostním rizikem, jak je popsáno v příspěvku blogu [ASP.NET MVC Tip #46 – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx). Úprava dat v metodu GET také porušuje HTTP osvědčené postupy a architektury [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) vzor, který určuje, že požadavky GET nesmí změnit stav vaší aplikace. Jinými slovy provádění operace GET by měl být bezpečné operace, která nemá žádné vedlejší účinky a nezmění trvalá data.

Pokud používáte čeština – počítače, můžete tuto část přeskočte a přejděte na další kurz. Globalize verzi tohoto kurzu si můžete stáhnout [zde](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=16475). Vynikající kurzu dvě části na internacionalizace, najdete v části [Nadeem na ASP.NET MVC 5 internacionalizace](http://afana.me/post/aspnet-mvc-internationalization.aspx).


> [!NOTE]
> Pro podporu ověřování jQuery pro neanglická národní prostředí, které používají čárkou (&quot;,&quot;) pro desetinné čárky a formát data neanglických USA, musíte zahrnout *globalize.js* a konkrétní  *cultures/Globalize.cultures.js* souboru (z [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) a JavaScript používat `Globalize.parseFloat`. Ověřování jiných než anglických jQuery můžete získat z NuGet. (Neinstalujte Globalize Pokud používáte anglickou národního prostředí.)


1. Z **nástroje** nabídce klikněte na tlačítko **Správce balíčků NuGetLibrary**a potom klikněte na **spravovat balíčky NuGet pro řešení**.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image5.png)
2. V levém podokně, vyberte **Procházet*. *** (viz následující obrázek).
3. Do vstupního pole, zadejte * Globalize **.  
  
    ![](examining-the-edit-methods-and-edit-view/_static/image6.png)Zvolte `jQuery.Validation.Globalize`, zvolte `MvcMovie` a klikněte na tlačítko **nainstalovat**. *Scripts\jquery.globalize\globalize.js* soubor bude přidán do projektu. *Scripts\jquery.globalize\cultures\* bude obsahovat soubory JavaScript mnoho jazykovou verzi. Poznámka: může trvat pět minut, chcete-li nainstalovat tento balíček.

 Následující kód ukazuje změny souboru Views\Movies\Edit.cshtml: 

[!code-cshtml[Main](examining-the-edit-methods-and-edit-view/samples/sample10.cshtml)]

Abyste se vyhnuli opakující se tento kód v každé upravit zobrazení, můžete přesuňte jej do souboru rozložení. K optimalizaci stahování skriptu, najdete v části Moje kurzu [sdružování a Minifikace](../../performance/bundling-and-minification.md).

Další informace najdete v části [ASP.NET MVC 3 internacionalizace](http://afana.me/post/aspnet-mvc-internationalization.aspx) a [ASP.NET MVC 3 internacionalizace – část 2 (NerdDinner)](http://afana.me/post/aspnet-mvc-internationalization-part-2.aspx).

Jako dočasné opravu Pokud nelze získat ověření práce v národním prostředí, můžete vynutit počítače pro angličtinu, nebo můžete zakázat JavaScript v prohlížeči. Chcete-li vynutit počítače pro angličtinu, můžete přidat element globalizace do kořenového adresáře projektů *web.config* souboru. Následující kód ukazuje element globalizace jazyková verze nastavena angličtina (Spojené státy).

[!code-xml[Main](examining-the-edit-methods-and-edit-view/samples/sample11.xml)]

<a id="gettingstarted"></a><a id="jQueryAjaxJSON"></a>V dalším kurzu jsme budete implementovat funkce vyhledávání.

>[!div class="step-by-step"]
[Předchozí](accessing-your-models-data-from-a-controller.md)
[další](adding-search.md)
