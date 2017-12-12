---
title: "Formátování dat odpovědi v aplikaci ASP.NET MVC jádra"
author: ardalis
description: "Zjistěte, jak k formátování data odpovědi v aplikaci ASP.NET MVC jádra."
keywords: "ASP.NET Core, data odpovědi, IOutputFormatter, IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Úvod k formátování data odpovědi v aplikaci ASP.NET MVC jádra

Podle [Steve Smith](https://ardalis.com/)

Jádro ASP.NET MVC má integrovanou podporu pro formátování data odpovědi pomocí pevné formáty nebo v reakci na požadavky klienta.

[Zobrazení nebo stažení ukázky z webu GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Výsledky akce specifické pro formát

Některé typy výsledků akcí, jako jsou specifické pro konkrétní formátu, `JsonResult` a `ContentResult`. Akce může vrátit konkrétní výsledky, které jsou vždy určitým způsobem. Například vrácení `JsonResult` vrátí data ve formátu JSON, bez ohledu na to předvolby klienta. Podobně vrácení `ContentResult` vrátí řetězec ve formátu prostého textu formátu dat (jak se jednoduše vrací řetězec).

> [!NOTE]
> Akce nemusí vrátit žádnou jednotlivou; MVC podporuje žádnou návratovou hodnotu objektu. Akce vrátí-li `IActionResult` implementace a řadič dědí z `Controller`, vývojáři mají mnoho pomocné metody odpovídající mnoho z těchto možností. Výsledky z akcí, které vracejí objekty, které nejsou `IActionResult` typy budou serializovány odpovídající pomocí `IOutputFormatter` implementace.

Vrátit data v konkrétním formátu z řadiče, která dědí z `Controller` základní třídy, použijte metodu integrovaných pomocných `Json` vrátit JSON a `Content` ve formátu prostého textu. Vaše metoda akce by měla vrátit výsledek konkrétní typ (například `JsonResult`) nebo `IActionResult`.

Vrací data ve formátu JSON:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Ukázková odpověď z tuto akci:

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je application/json](formatting/_static/json-response.png)

Všimněte si, že je typ obsahu odpovědi `application/json`, uvedené v seznamu požadavků sítě a v části hlavičky odpovědi. Všimněte si také seznam možností, které jsou uvedené v prohlížeči (v tomto případě Microsoft Edge) v hlavičce Accept v části záhlaví požadavku. Aktuální technika ignoruje tuto hlavičku; obeying ho jsou uvedeny níže.

Chcete-li vrátit data ve formátu prostého textu, použijte `ContentResult` a `Content` pomocné rutiny:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Odpověď z tuto akci:

![Síťové kartě nástrojů pro vývojáře v Microsoft Edge zobrazující typ obsahu odpovědi je text/plain](formatting/_static/text-response.png)

Poznámka: v tomto případě `Content-Type` vrátil je `text/plain`. Můžete také dosáhnout tento stejné chování pomocí právě typ odpovědi řetězec:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Pro netriviální akce s více vrátit typy nebo možnosti (například různé stavové kódy HTTP na základě výsledku operace provedené), raději `IActionResult` jako návratový typ.

## <a name="content-negotiation"></a>Vyjednávání obsahu

Vyjednávání obsahu (*conneg* pro zkrácení) nastane, když klient Určuje [hlavička Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). Výchozí formát používaný ASP.NET Core MVC je JSON. Vyjednávání obsahu je implementováno modulem `ObjectResult`. Je také součástí stavový kód vráceny výsledky určité akce z pomocné metody (které jsou všechny založené na `ObjectResult`). Můžete se taky vrátit typu modelu (třídy, které jste definovali jako typ přenosu dat) a rozhraní budou automaticky zahrnovat ho `ObjectResult` za vás.

Používá následující metody akce `Ok` a `NotFound` pomocné metody:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Odpověď formátu JSON, bude vrácen, pokud byl požadován jiného formátu a server může vracet požadovaný formát. Můžete použít nástroje, jako je [Fiddler](http://www.telerik.com/fiddler) vytvořit požadavek, který obsahuje hlavičku Accept a určit jiného formátu. V takovém případě, pokud má server *formátování* , může vytvořit odpověď v požadovaný formát, výsledkem bude vrácen ve formátu preferované klienta.

![ZÍSKAT aplikaci Fiddler konzola znázorňující, ručně vytvořit požadavek s hodnotou hlavičky Accept application/XML](formatting/_static/fiddler-composer.png)

Výše uvedený snímek obrazovky, autora Fiddler používá k vytvoření žádosti, zadání `Accept: application/xml`. Ve výchozím nastavení, ASP.NET MVC základní podporuje pouze formát JSON, takže i když je zadané jiného formátu, výsledek vrácený je stále ve formátu JSON. Uvidíte, jak přidat další formátování v další části.

Akce kontroleru vrátit POCOs (prostý staré CLR objekty), v takovém případě se automaticky vytvoří ASP.NET MVC `ObjectResult` pro vás, která zabalí objekt. Klient získá formátovaný serializovaný objekt (formátu JSON je výchozí nastavení, můžete nakonfigurovat XML nebo jiných formátů). Pokud se objekt vrácená `null`, pak vrátí rozhraní `204 No Content` odpovědi.

Vrátí typ objektu:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

V ukázce obdrží žádost o alias platný Autor odpovědi 200 OK daty vytvářením obsahu. Žádost o neplatný alias obdrží 204 ne obsahu odpovědi. Níže jsou uvedeny snímky obrazovky zobrazující odpovědi ve formátu XML a JSON.

### <a name="content-negotiation-process"></a>Proces vyjednávání obsahu

Obsahu *vyjednávání* pouze dojde-li `Accept` hlavička se zobrazí v požadavku. Pokud požadavek obsahuje hlavičku accept, rozhraní se zobrazí výčet typů médií v hlavičce accept v upřednostňovaném pořadí a pokusí najít formátovací modul, který může vytvořit odpověď v jednom z formátů zadaná v hlavičce accept. V případě, že je nalezeno žádné formátování, které může stát odpovědí na požadavek klienta, rozhraní se pokusí najít první formátovací modul, který může vytvořit odpověď (pokud vývojář možnost nakonfiguroval na `MvcOptions` vrátit 406 Nepřijatelný místo). Pokud požadavek určuje XML, ale není nakonfigurován formátovací modul XML, bude formátovací modul JSON použít. Obecně platí Pokud je nakonfigurován žádné formátování který může poskytnout požadovaný formát, pak první formátovací modul, který dokáže formátovat objekt se používá. Pokud je zadána žádná hlavička první formátovací modul, který dokáže zpracovat objekt, který má být vrácen se použije k serializaci odpovědi. V takovém případě není k dispozici žádné vyjednávání dochází – server je určení formátu, jaké se bude používat.

> [!NOTE]
> Pokud obsahuje hlavičku Accept `*/*`, záhlaví bude ignorována, pokud `RespectBrowserAcceptHeader` je nastaven na hodnotu true na `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Prohlížeče a vyjednávání obsahu

Na rozdíl od typických rozhraní API klientů webových prohlížečů mívají k poskytování `Accept` hlavičky, které obsahují širokou škálu formátů, včetně zástupné znaky. Ve výchozím nastavení, když rozhraní zjistí, zda požadavek pochází z prohlížeče, se budou ignorovat `Accept` záhlaví a místo toho návratový obsahu v aplikaci je nakonfigurované výchozí formát (JSON Pokud jinak nakonfigurovat). To poskytuje jednotný při používání rozhraní API pomocí různých prohlížečích.

Pokud si přejete hlavičky accept prohlížeč dodržet aplikace, to můžete nakonfigurovat jako součást konfigurace MVC nastavením `RespectBrowserAcceptHeader` k `true` v `ConfigureServices` metoda v *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Konfigurace formátování

Pokud aplikace potřebuje pro podporu dalších formátech nad výchozí hodnotu JSON, můžete přidat balíčků NuGet a nakonfigurovat MVC, které je podporují. Existují samostatné formátování pro vstup a výstup. Vstupní formátování používají [vazby modelu](model-binding.md); výstupní formátování slouží k formátování odpovědi. Můžete také nakonfigurovat [vlastní formátování](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Přidání podpory pro formát XML

Chcete-li přidat podporu pro formátování XML, nainstalujte `Microsoft.AspNetCore.Mvc.Formatters.Xml` balíček NuGet.

Přidat XmlSerializerFormatters do konfigurace MVC v *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Alternativně můžete přidat pouze formátování výstupu:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Tyto dva přístupy se serializovat výsledky pomocí `System.Xml.Serialization.XmlSerializer`. Pokud dáváte přednost, můžete použít `System.Runtime.Serialization.DataContractSerializer` přidáním jeho přidružené formátovací modul:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Po přidání podpory pro formátování XML, vaše řadiče metody by měla vrátit příslušný formát na základě daného požadavku `Accept` záhlaví, jako tento Fiddler ukazuje příklad:

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/xml. Na kartě nezpracovaná pro odpověď zobrazena hodnota hlavičky Content-Type application/XML.](formatting/_static/xml-response.png)

Se zobrazí na kartě kontroly, který byl vyžádán nezpracovaná získat pomocí `Accept: application/xml` hlavičky set. V podokně dozvíte odpovědi `Content-Type: application/xml` záhlaví a `Author` objekt má byl serializován do formátu XML.

Na kartě autora lze upravit žádost o zadejte `application/json` v `Accept` záhlaví. Provedení požadavku a odpovědi se bude formátovat jako JSON:

![Konzole Fiddler: nezpracovaná kartě pro požadavek se zobrazují hodnotu hlavičky Accept je application/json. Na kartě nezpracovaná pro odpověď zobrazena hodnota hlavičky Content-Type application/json.](formatting/_static/json-response-fiddler.png)

Na tomto snímku obrazovky vidíte, nastaví hlavičku ze žádosti `Accept: application/json` a určuje odpověď stejná jako jeho `Content-Type`. `Author` Objektu se zobrazí v textu odpovědi ve formátu JSON.

### <a name="forcing-a-particular-format"></a>Vynucení konkrétní formátu

Pokud chcete omezit formáty odpovědi pro konkrétní akci je možné, můžete použít `[Produces]` filtru. `[Produces]` Filtru určuje formáty odpovědi pro konkrétní akce (nebo řadič). Jako většinu [filtry](../controllers/filters.md), to mohou být použity na akce, řadiče nebo globální obor.

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]` Filtru akce vynutí všechny akce v rámci `AuthorsController` vrátit odpovědí formátu JSON i v případě, že další formátovací moduly byly nakonfigurovány pro aplikace a klient pro zadaný `Accept` hlavičky požadavku liší, k dispozici formát. V tématu [filtry](../controllers/filters.md) Další informace, včetně použití filtrů globálně.

### <a name="special-case-formatters"></a>Speciální případu formátování

Některých zvláštních případech jsou implementovány pomocí předdefinovaných formátování. Ve výchozím nastavení `string` návratové typy se bude formátovat jako *text/plain* (*text/html* Pokud požadovaný prostřednictvím `Accept` záhlaví). Toto chování lze odebrat odstraněním `TextOutputFormatter`. Odebrat formátování v `Configure` metoda v *Startup.cs* (zobrazené dole). Akce, které mají objekt modelu vrátí typ vrátí 204 neodpovídá obsahu při vrácení `null`. Toto chování lze odebrat odstraněním `HttpNoContentOutputFormatter`. Následující kód odebere `TextOutputFormatter` a `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Bez `TextOutputFormatter`, `string` vrátit typy vrátit 406 Nepřijatelný, např. Všimněte si, že pokud formátovací modul XML existuje, ji budou zformátovány `string` návratové typy, pokud `TextOutputFormatter` se odebere.

Bez `HttpNoContentOutputFormatter`, hodnotu null. objekty jsou formátovány pomocí nakonfigurované formátovacího modulu. Například formátovací modul JSON jednoduše vrátí odpověď subjektu, který `null`, zatímco formátovací modul XML vrátí prázdný element XML s atributem `xsi:nil="true"` nastavit.

## <a name="response-format-url-mappings"></a>Mapování adresy URL formát odpovědi

Klienti mohou požadovat konkrétní formátu jako část adresy URL, například na řetězec dotazu nebo části cesty nebo pomocí formátu konkrétní příponu například .xml nebo .json. Mapování z cestu požadavku musí být zadán v trasy, k níž je pomocí rozhraní API. Příklad:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Tato trasa by umožnilo požadovaný formát, který má být zadány jako volitelný soubor rozšíření. `[FormatFilter]` Atribut zkontroluje existenci hodnoty formátu v `RouteData` a namapujete formát odpovědi na odpovídající formátovací modul, při vytváření odpovědi.

| Trasy                      | Formátování                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | Výchozí výstupní formátování       |
| `/products/GetById/5.json` | Formátovací modul JSON (Pokud je nakonfigurováno) |
| `/products/GetById/5.xml`  | Formátovací modul XML (Pokud je nakonfigurováno)  |
