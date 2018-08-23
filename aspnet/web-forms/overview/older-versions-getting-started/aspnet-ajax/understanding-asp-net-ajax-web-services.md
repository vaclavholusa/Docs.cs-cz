---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Principy webových služeb ASP.NET AJAX | Dokumentace Microsoftu
author: scottcate
description: Webové služby jsou nedílnou součástí rozhraní .NET framework, která poskytují řešení napříč platformami pro výměnu dat mezi distribuované systémy. I když Web...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 5e59077373b68b907391eff5349e1925222792a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756723"
---
<a name="understanding-aspnet-ajax-web-services"></a>Principy webových služeb ASP.NET AJAX
====================
podle [– Scott Cate](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Webové služby jsou nedílnou součástí rozhraní .NET framework, která poskytují řešení napříč platformami pro výměnu dat mezi distribuované systémy. I když webové služby se obvykle používají pro různé operační systémy, objektové modely a programovacích jazyků k odesílání a příjmu dat, jsou také umožňuje dynamicky vložení dat do stránky technologie ASP.NET AJAX nebo odesílat data ze stránky do back endovému systému. To všechno můžete udělat bez nutnosti uchýlit se operace odeslání.


## <a name="calling-web-services-with-aspnet-ajax"></a>Volání webové služby pomocí ASP.NET AJAX

DaN Wahlin

Webové služby jsou nedílnou součástí rozhraní .NET framework, která poskytují řešení napříč platformami pro výměnu dat mezi distribuované systémy. I když webové služby se obvykle používají pro různé operační systémy, objektové modely a programovacích jazyků k odesílání a příjmu dat, jsou také umožňuje dynamicky vložení dat do stránky technologie ASP.NET AJAX nebo odesílat data ze stránky do back endovému systému. To všechno můžete udělat bez nutnosti uchýlit se operace odeslání.

Zatímco ovládacího prvku UpdatePanel technologie ASP.NET AJAX poskytuje jednoduchý způsob, jak AJAX povolit jakékoli stránky technologie ASP.NET, může nastat situace, kdy je potřeba dynamicky přístup k datům na serveru bez použití ovládacího prvku UpdatePanel. V tomto článku uvidíte, jak to provést pomocí vytváření a používání webových služeb v rámci stránky technologie ASP.NET AJAX.

Tento článek se zaměřuje na funkce, které jsou k dispozici v rozšíření základní technologie ASP.NET AJAX, stejně jako webové služby povolena ovládacího prvku v ASP.NET AJAX Toolkit, volá se, AutoCompleteExtender. Probíraná témata zahrnují definice webové služby s povoleným AJAX, vytváření proxy serverů klientských a volání webových služeb s použitím jazyka JavaScript. Uvidíte také, jak volání webové služby můžete provést přímo do metody stránky technologie ASP.NET.

## <a name="web-services-configuration"></a>Konfigurace webové služby

Když pomocí sady Visual Studio 2008 se vytvoří nový projekt webu, v souboru web.config má několik nové doplňky, které mohou být obeznámeni uživatelům z předchozích verzí sady Visual Studio. Některé tyto úpravy, aby je bylo možné použít na stránkách zatímco ostatní definovat povinné HttpHandlers a HttpModules mapování předpony "asp" na technologie ASP.NET AJAX – ovládací prvky. Výpis 1 zobrazuje změny provedené `<httpHandlers>` element v souboru web.config, který má vliv volání webové služby. Výchozí httpHandlers používá ke zpracování .asmx volání je odebrán a nahrazen parametrem ScriptHandlerFactory třídy v sestavení System.Web.Extensions.dll. System.Web.Extensions.dll obsahuje všechny základní funkce používané technologie ASP.NET AJAX.

**Výpis 1. Konfigurace obslužnou rutinu ASP.NET AJAX webové služby**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Toto nahrazení httpHandlers tvoří umožní se zápis JSON (JavaScript Object) volání provést ze stránek technologie ASP.NET AJAX, webové služby .NET pomocí proxy webové služby jazyka JavaScript. ASP.NET AJAX odesílá zprávy JSON do webové služby na rozdíl od standardních volání jednoduchý objekt přístup protokolu (SOAP), které jsou typicky spojované s webovými službami. Výsledkem je menší žádosti a zprávy s odezvami na celkové. Také umožňuje efektivnější zpracování dat na straně klienta od knihovny JavaScript AJAX technologie ASP.NET je optimalizovaná pro práci s objekty JSON. Výpis 2 a 3 výpis příklady požadavků a odpovědí zprávy webové služby serializovat do formátu JSON. Zprávy s požadavkem je znázorněno na výpis 2 předává země parametr s hodnotou "Belgie", zatímco zprávy s odpovědí ve výpisu 3 předává pole objektů zákazníků a jejich vlastnosti.

**Výpis 2. Zpráva požadavku webové služby serializovat do formátu JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] Název operace je definován jako část adresy URL webové služby; Kromě toho zprávy žádosti nejsou vždy odeslat prostřednictvím formátu JSON. Webové služby můžete využít ScriptMethod atribut s UseHttpGet parametrem nastaveným na hodnotu true, což způsobí, že parametry se mají být předány prostřednictvím parametrů řetězce dotazu.*


**Výpis 3. Zpráva odpovědi webové služby serializovat do formátu JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

V další části uvidíte, jak vytvořit webové služby schopné zpracování zprávy požadavku JSON a reagovat s jednoduché a komplexní typy.

## <a name="creating-ajax-enabled-web-services"></a>Vytváření webových služeb podporou jazyka AJAX

ASP.NET AJAX framework nabízí několik různých způsobů pro volání webové služby. Můžete použít ovládací prvek AutoCompleteExtender (k dispozici v ASP.NET AJAX Toolkit) nebo JavaScript. Ale před voláním služby budete muset povolit AJAX ho tak, že je možné vyvolat v kódu skriptu klienta.

Jestli jste nové webové služby ASP.NET, najdete ho k vytvoření jednoduché a AJAX – povolení služby. Rozhraní .NET framework je podporováno vytváření webových služeb ASP.NET od původní verze v roce 2002 a rozšíření ASP.NET AJAX poskytují další funkce AJAX, která staví na rozhraní .NET framework výchozí sadu funkcí. Visual Studio .NET 2008 Beta 2 obsahuje integrovanou podporu pro vytváření .asmx souborů webové služby a automaticky přidružený kód vedle třídy je odvozen od třídy System.Web.Services.WebService. Jak přidat metody do třídy, musíte použít atribut WebMethod je možné vyvolat v příjemci webové služby.

Výpis 4 ukazuje příklad použití atributu WebMethod metodu s názvem GetCustomersByCountry().

**Výpis 4. Použít atribut WebMethod ve webové službě**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Metoda GetCustomersByCountry() přijímá parametr země a vrátí pole objektů zákazníka. Země hodnoty předané do metody je předán do třídy obchodní vrstvu, která volá třída vrstvy dat k načtení dat z databáze, zadejte vlastnosti objektu zákazníků s daty a vrátí pole.

## <a name="using-the-scriptservice-attribute"></a>Pomocí atributu ScriptService

Při přidávání WebMethod atribut umožňuje GetCustomersByCountry() metoda má být volána klienty, kteří odesílají standardní zprávy protokolu SOAP k webové službě, neumožňuje volání JSON má být provedeno z aplikací ASP.NET AJAX úprav. Chcete-li povolit volání JSON má být provedeno, budete muset použít rozšíření ASP.NET AJAX `ScriptService` atribut pro třídu webové služby. To umožňuje odesílat zprávy s odezvami ve formátu JSON pomocí webové služby a umožní skript na straně klienta pro volání služby odesíláním zpráv JSON.

Výpis 5 ukazuje příklad použití atributu ScriptService třída webové služby s názvem CustomersService.

**Seznam 5. Pomocí atributu ScriptService AJAX povolit webové služby**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Atribut ScriptService funguje jako značku, která označuje, že jej lze volat z kódu skriptu AJAX. Ho nemůže pracovat s ve skutečnosti JSON serializaci nebo deserializaci úkoly, ke kterým dochází na pozadí. ScriptHandlerFactory (nakonfigurovaný v souboru web.config) a další související třídy provést hromadné zpracování JSON.

## <a name="using-the-scriptmethod-attribute"></a>Pomocí atributu ScriptMethod

Atribut ScriptService není jediným atributem technologie ASP.NET AJAX, který má být definován ve webové službě .NET v pořadí, které bude využívat stránky technologie ASP.NET AJAX. Jiný atribut s názvem ScriptMethod však lze také použít přímo na metody webové služby. ScriptMethod definuje tři vlastnosti včetně `UseHttpGet`, `ResponseFormat` a `XmlSerializeString`. Změna hodnoty těchto vlastností, může být užitečné v případech, kde je potřeba změnit tak, aby GET, když Web metoda musí vracet nezpracovaná data XML ve formě typu požadavek přijal metody webové `XmlDocument` nebo `XmlElement` objekt nebo když se data vrácená z  služby by měl vždy být serializován jako XML namísto formátu JSON.

UseHttpGet vlastnost se dá použít při webové metodě by měla přijímat žádosti o požadavky POST na rozdíl od získání. Požadavky se odešlou pomocí adresy URL se vstupními parametry webové metody převedeny na parametry řetězce dotazu. UseHttpGet vlastnost Výchozí hodnota je false a pouze by měl být nastaven na `true` když operace jsou známé jako bezpečné a pokud není citlivá data předán webové služby. Výpis 6 ukazuje příklad použití atributu ScriptMethod s vlastností UseHttpGet.

**Výpis 6. Pomocí atributu ScriptMethod s vlastností UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Příklad hlavičky posílané při volání metody webové HttpGetEcho ukazuje výpis 6 jsou uvedeny další:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Kromě povolení metody Web tak, aby přijímal požadavky HTTP GET, atribut ScriptMethod lze také při odpovědi XML se muset vrátit ze služby spíše než formátu JSON. Webová služba může například načtení informačního kanálu RSS ze vzdálené lokality a vraťte jej jako objekt XmlDocument nebo XmlElement. Zpracování XML data poté provést na straně klienta.

Výpis 7 ukazuje příklad použití vlastnost Formát odpovědi k určení, že má být vrácen XML data z webové metody.

**Výpis 7. Pomocí atributu ScriptMethod s vlastností formát odpovědi.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Vlastnost Formát odpovědi lze také společně s vlastností XmlSerializeString. Vlastnost XmlSerializeString má výchozí hodnotu false, což znamená, že všechny návratové typy s výjimkou řetězce vrácené z metody webové serializují jako kód XML při `ResponseFormat` je nastavena na `ResponseFormat.Xml`. Když `XmlSerializeString` je nastavena na `true`, všechny typy vrácené z metody webové serializují jako kód XML, včetně typy řetězců. Pokud má vlastnost Formát odpovědi hodnotu `ResponseFormat.Json` XmlSerializeString vlastnost se ignoruje.

Výpis 8 ukazuje příklad použití vlastnost XmlSerializeString přinutit řetězců, které mají být serializován jako XML.

**Výpis 8. Pomocí atributu ScriptMethod s vlastností XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Hodnota vrácená z volání metody webové GetXmlString ukazuje výpis 8 se zobrazí jako další:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

I když výchozí formát JSON minimalizuje celkové velikosti zprávy požadavků a odpovědí a snadněji využívá klienti technologie ASP.NET AJAX v podobě prohlížečů, formát odpovědi a XmlSerializeString vlastnosti lze využít při klienta data XML, který se má vrátit z metody webové očekávat, že aplikace, jako je například Internet Explorer 5 nebo vyšší.

## <a name="working-with-complex-types"></a>Práce s komplexní typy

Výpis 5 vám ukázal, příklad vrací složitý typ s názvem zákazníka z webové služby. Třída zákazník definuje několik různých typů jednoduché interně jako vlastnosti, jako je jméno a příjmení. Komplexní typy použité jako vstupní parametr nebo návratový typ na webové metody s povoleným AJAX jsou automaticky serializovat do formátu JSON před odesláním na straně klienta. Ale vnořené komplexní typy (pokud jsou definované interně v rámci jiného typu) nejsou k dispozici klient jako samostatné objekty ve výchozím nastavení.

V případech, kdy vnořené komplexní typ, který používá webové služby musí být rovněž použita na stránce klienta můžete přidat atribut GenerateScriptType technologie ASP.NET AJAX k webové službě. Například třída CustomerDetails je vidět na výpisu 9 obsahuje vlastnosti adresy a pohlaví který ***představují vnořené komplexní typy.***

**Výpis 9. Třída CustomerDetails je vidět tady obsahuje dvě vnořené komplexní typy.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Adresa a pohlaví objekty, které jsou definované v rámci třídy CustomerDetails je vidět na výpisu 9 nebude automaticky dostupná pro použití na straně klienta pomocí JavaScriptu vzhledem k tomu, že jsou vnořené typy (adresa je třída a pohlaví je výčet). V situacích, kde vnořený typ používaný v rámci webové služby musí být k dispozici na na straně klienta, můžete použít atribut GenerateScriptType již bylo zmíněno dříve (viz seznam 10). Tento atribut lze přidat více než jednou v případech, kdy se různé typy vnořených komplexní vrátí ze služby. Můžete použít přímo na třídu webové služby nebo vyšší. konkrétní webové metody.

**Seznam 10. Pomocí atributu GenerateScriptService definovat vnořené typy, které by měly být k dispozici ke klientovi.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Použitím `GenerateScriptType` atribut na typy webové služby a adresu pohlaví bude automaticky k dispozici pro použití kódu jazyka JavaScript technologie ASP.NET AJAX na straně klienta. V informacích 11 je uveden příklad jazyka JavaScript, která je automaticky generována a odeslat klientovi, tak, že přidáte atribut GenerateScriptType na webovou službu. Uvidíte jak používat komplexní typy vnořených později v tomto článku.

**Výpis 11. Vnořené komplexní typy, které jsou k dispozici na stránku ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Teď, když už víte, jak vytvořit webové služby a zajistíte jejich přístupnost na stránky technologie ASP.NET AJAX, Pojďme se podívat, jak vytvořit a použít třídy proxy JavaScript, takže data můžete načíst nebo odesílány do webové služby.

## <a name="creating-javascript-proxies"></a>Vytvoření třídy proxy JavaScript

Volání standardní webové služby (.NET nebo jinou platformu) obvykle zahrnuje vytvoření objekt proxy serveru, který chrání před složitým odesílání požadavků a odpovědí zprávy protokolu SOAP. Při volání webové služby ASP.NET AJAX můžete vytvořit a použít snadno volat služby bez starostí o serializaci a deserializaci zpráv JSON třídy proxy JavaScript. Třídy proxy JavaScript je automaticky generovat pomocí ovládacího prvku ASP.NET AJAX ovládacímu prvku ScriptManager.

Vytváření JavaScript třídu proxy, která může volat webové služby se provádí pomocí vlastnosti služby prvku ScriptManager. Tuto vlastnost můžete zadat jednu nebo víc služeb, které může stránka technologie ASP.NET AJAX můžete odesílat nebo přijímat data bez nutnosti postback operace asynchronní volání. Službu definujete pomocí technologie ASP.NET AJAX `ServiceReference` ovládacího prvku a přiřadí adresu URL webové služby ovládacího prvku `Path` vlastnost. Výpis 12 znázorňuje příklad odkazuje službu s názvem CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Výpis 12. Definice webové služby používané stránku ASP.NET AJAX.**

Přidání odkazu na CustomersService.asmx prostřednictvím ovládacího prvku ScriptManager způsobí, že proxy server JavaScript dynamicky generované a odkazuje na stránku. Proxy server se vloží pomocí &lt;skript&gt; označit a dynamicky načíst voláním CustomersService.asmx souboru a přidávání /js na konec. Následující příklad ukazuje, jak proxy server JavaScript je vložená na stránce při ladění je zakázána v souboru web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Pokud chcete zobrazit skutečný kód jazyka JavaScript proxy server, který je generován můžete zadat adresu URL požadované webové služby .NET do pole Adresa aplikace Internet Explorer a připojte na konec /js.*


Pokud je povoleno ladění v souboru web.config, který vloží ladicí verzi proxy server JavaScript na stránce jako ukazuje následující:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Proxy server JavaScript vytvořené ScriptManager můžete také vložit přímo na stránku spíše než odkazovat pomocí &lt;skript&gt; atributu src značky. To můžete udělat tak, že nastavíte vlastnost InlineScript ServiceReference ovládacího prvku na hodnotu true (výchozí hodnota je false). To může být užitečné, když se proxy server není sdílí na několika stránkách a když chcete snížit počet volání sítě na serveru. Pokud InlineScript nastavená na hodnotu true, skript proxy serveru nebudou zapisována do mezipaměti v prohlížeči tak výchozí hodnotu false, je vhodné v případech, ve kterém proxy server používá více stránek v aplikaci technologie ASP.NET AJAX. Příklad použití vlastnost InlineScript se zobrazí následující:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Pomocí třídy proxy JavaScript

Jakmile webová služba se odkazuje pomocí ovládacího prvku ScriptManager stránce technologie ASP.NET AJAX, může být k volání webové služby a vrácená data mohou být zpracovávány pomocí funkce zpětného volání. Webová služba je volán odkazující na svůj obor názvů (pokud existuje), název třídy a název webové metody. Všechny parametry předané k webové službě mohou být definovány spolu s funkce zpětného volání, která zpracovává vracená data.

Příklad použití proxy server JavaScript volat metodu Web s názvem GetCustomersByCountry() se zobrazí v zobrazení 13. GetCustomersByCountry() funkce je volána, když koncový uživatel klikne na tlačítko na stránce.

**Výpis 13. Volání webové služby s proxy server JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Toto volání odkazuje na obor názvů InterfaceTraining, CustomersService třídy a metody webové GetCustomersByCountry definované ve službě. Předá hodnotu země získaných z objektu textbox, stejně jako funkce zpětného volání s názvem OnWSRequestComplete, který má být volána, když se vrátí asynchronní volání webové služby. OnWSRequestComplete zpracovává pole vrácené službou zákaznických objektů a převede je do tabulky, který se zobrazí na stránce. Výstup generovaný z volání je znázorněno na obrázku 1.


[![Vazba dat získali tím, že asynchronní volání jazyka AJAX k webové službě.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Obrázek 1**: vytvoření vazby dat získali tím, že asynchronní volání jazyka AJAX k webové službě.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image3.png))


Třídy proxy JavaScript lze také nastavit jednosměrné volání webové služby v případech, kdy by měla být volána metoda webových, ale proxy nesmí čekat na odpověď. Můžete například volání webové služby sloužící ke spuštění procesu, jako je například pracovní postup, ale nečeká návratová hodnota ze služby. V případech, kde jednosměrné volání je nutné provést, služby můžete jednoduše vynechat funkce zpětného volání, které jsou uvedené v informacích 13. Protože je definována žádná funkce zpětného volání objektu proxy nebude čekat pro webovou službu vrátit data.

## <a name="handling-errors"></a>Zpracování chyb

Asynchronními zpětnými voláními k webovým službám můžou mít různé typy chyb, jako jsou například sítě se dolů, webová služba není k dispozici nebo se vrátila výjimka. Naštěstí generovaných ScriptManager objekty proxy JavaScript povolit více zpětných volání, aby byla definována pro zpracování chyb a selhání kromě úspěch zpětného volání je uvedeno výše. K chybě funkce zpětného volání lze definovat ihned po zpětného volání standardní funkce při volání metody webové, jak je uvedeno v informacích 14.

**Výpis 14. Definování k chybě funkce zpětného volání a zobrazování chyb.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Všechny chyby, ke kterým dochází při volání webové služby se aktivuje OnWSRequestFailed() funkce zpětného volání k volání, která přijímá objekt, který reprezentuje chyby jako parametr. Objekt error poskytuje několik různých funkcí můžete zjistit příčinu chyby a zda volání vypršel časový limit. Výpis 14 ukazuje příklad použití funkce různých chyb a obrázek 2 ukazuje příklad výstupu generovaného pomocí funkcí.


[![Výstup vygenerovaný pomocí volání funkce chybě technologie ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Obrázek 2**: výstup vygenerovaný pomocí volání funkce chybě technologie ASP.NET AJAX.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Zpracování dat XML vrácený webovou službu

Dříve jste viděli, jak webové metodě může vrátit nezpracovaná data XML použitím atributu ScriptMethod spolu s vlastností formát odpovědi. Pokud formát odpovědi je nastavena na ResponseFormat.Xml, data vrácená z webové služby je serializován jako XML spíše než formátu JSON. To může být užitečné, když XML data je nutné předat přímo do klienta pro zpracování pomocí jazyka JavaScript nebo XSLT. V současné době aplikace Internet Explorer 5 nebo vyšší poskytuje nejlepší model objektu na straně klienta k parsování a filtrování dat XML z důvodu jeho integrovanou podporu pro analyzátor MSXML.

Načítání dat XML z webové služby se nijak neliší od načítání jiné datové typy. Začněte tím, že volání proxy server JavaScript volat odpovídající funkce a definovat funkci zpětného volání. Po volání vrátí lze následně zpracovat data ve funkci zpětného volání.

Výpis 15 ukazuje příklad volá metodu Web s názvem GetRssFeed(), která vrací objekt XmlElement. GetRssFeed() přijímá jeden parametr představující adresu URL pro načtení informačního kanálu RSS.

**Výpis 15. Práce s daty XML vrácený webovou službu.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Tento příklad předá adresu URL informačního kanálu RSS a zpracovává vrácená data XML ve funkci OnWSRequestComplete(). OnWSRequestComplete() nejprve zkontroluje, zda je v prohlížeči Internet Explorer vědět, zda analyzátor MSXML je k dispozici. Pokud se jedná, příkaz jazyka XPath je používaná k nalezení všech &lt;položky&gt; značky v informačním kanálu RSS. Jednotlivé položky potom provést iteraci pomocí a přidružené &lt;title&gt; a &lt;odkaz&gt; značky jsou umístěné a zpracování pro každou položku dat zobrazení. Obrázek 3 ukazuje příklad výstupu generovaného v provádění volání přes proxy server JavaScript webové metodě GetRssFeed() technologie ASP.NET AJAX.

## <a name="handling-complex-types"></a>Zpracování komplexních typů

Komplexní typy přijata nebo vrácené webové služby jsou automaticky přístupné přes proxy server JavaScript. Vnořené komplexní typy však nejsou přímo přístupné na straně klienta, pokud se atribut GenerateScriptType používá ke službě, jak je uvedeno výše. Proč byste měli použít vnořené komplexní typ, na straně klienta?

Na tuto otázku odpovědět, předpokládají, že zákaznická data se zobrazí stránku ASP.NET AJAX a umožňuje koncovým uživatelům aktualizovat adresy zákazníka. Pokud webová služba určuje, že typ adresy (komplexní typ definovaný v rámci třídy CustomerDetails) může odeslat klientovi proces aktualizace je možné rozdělit do samostatné funkce pro lepší opakovanému použití kódu.


[![Vytváření z volání webové služby, která vrací RSS data výstup.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Obrázek 3**: výstup vytváření z volání webové služby, která vrací RSS data.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image9.png))


Výpis 16 ukazuje příklad kódu na straně klienta, který vyvolá adresu objekt definovaný v oboru názvů modelu, vyplní aktualizovaná data a přiřadí ji k vlastnost adresy CustomerDetails objektu. CustomerDetails objekt je pak předán k webové službě pro zpracování.

**Výpis 16. Pomocí vnořeného komplexní typy**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Vytváření a používání metody stránky

Webové služby poskytují vynikající příležitost k vystavení služby znovu použitelný pro celou řadu klientů, včetně stránek technologie ASP.NET AJAX. Mohou však nastat případy, kde stránku potřebuje načíst data, která nebude nikdy používat nebo sdílet další stránky. V takovém případě provádění soubor ASMX umožňující přístup k datům stránky může jevit jako přehnaně vzhledem k tomu, že služba používá jenom jednu stránku.

ASP.NET AJAX obsahuje jiný mechanismus pro volání webové služby jako bez vytváření souborů .asmx samostatné. To se provádí pomocí techniky označované jako "metody stránky". Stránka metod jsou statické (sdílené v VB.NET) metody, které mají aplikovaný atribut WebMethod je součástí stránku nebo kód vedle souboru. Použitím atributu WebMethod bylo možné volat pomocí speciální objekt JavaScriptu s názvem PageMethods, která se dynamicky vytvoří za běhu. Objekt PageMethods funguje jako proxy server, který chrání před procesu serializaci nebo deserializaci JSON. Mějte na paměti, chcete-li použít objekt PageMethods musíte nastavit vlastnost EnablePageMethods prvku ScriptManager na hodnotu true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Výpis 17 ukazuje příklad definuje dvě metody stránky v kódu vedle třídu technologie ASP.NET. Tyto metody načíst data ze třídy obchodní vrstvy v aplikaci\_složka kódu webové stránky.

**Výpis 17. Definování metody stránky.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Když ScriptManager zjistí přítomnost webové metody ve stránce generuje dynamické odkaz na objekt PageMethods již bylo zmíněno dříve. Volání metody webové se provádí pomocí odkazu na třídu PageMethods následovaný názvem metody a všechny nezbytné parametr data, která by se měl předávat. Výpis 18 jsou uvedeny příklady volání dvě metody stránky je uvedeno výše.

**Výpis 18. Volání metody stránky s objektem PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Pomocí objektu PageMethods je velmi podobný pomocí objektu proxy server JavaScript. Nejdřív musíte určit všechna data parametr, který by měly být předány metodě stránky a potom definovat, která by měla být volána, když se vrátí asynchronního volání funkce zpětného volání. Je taky možné specifikovat zpětné volání chyby (viz část o výpis 14 příklad zpracování chyb).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender a ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (k dispozici z [ http://ajax.asp.net ](http://ajax.asp.net)) nabízí několik ovládacích prvků, které lze použít pro přístup k webovým službám. Konkrétně sada nástrojů obsahuje užitečných ovládací prvek s názvem `AutoCompleteExtender` , který slouží k volání webové služby a zobrazení dat bez psaní kódu jazyka JavaScript na všech stránkách.

Ovládací prvek AutoCompleteExtender slouží k rozšíření stávajících funkcí textové pole a uživatelům snadno najít data, která se snaží zjistit. Psaní do pole textbox ovládací prvek lze použít k dotazování webové služby a zobrazí výsledky pod textovým polem dynamicky. Obrázek 4 ukazuje příklad použití ovládacího prvku AutoCompleteExtender zobrazíte ID zákazníka pro podporu aplikace. Uživatel zadá různých znaků do textového pole, zobrazí se pod ní na základě jejich vstupu různé položky. Uživatelé pak mohou vybrat požadovanou CustomerID.

Použití AutoCompleteExtender stránky technologie ASP.NET AJAX vyžaduje, aby sestavení AjaxControlToolkit.dll přidají do složky bin na webu. Po sestavení toolkit se přidala, budete chtít tak, že jsou dostupné pro všechny stránky v aplikaci, která obsahuje ovládací prvky na něj odkazovat v souboru web.config. To můžete udělat tak, že přidáte následující značky v souboru web.config &lt;ovládací prvky&gt; značky:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

V případech, kdy je potřeba jenom pomocí ovládacího prvku na konkrétní stránce můžete na něj mohli odkazovat tak, že přidáte direktiva odkazu do horní části stránky, jak je ukázáno dále místo aktualizaci souboru web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Použití ovládacího prvku AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Obrázek 4**: použití AutoCompleteExtender ovládacího prvku.  ([Kliknutím ji zobrazíte obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image12.png))


Po web nakonfigurovaný se sadou nástrojů ASP.NET AJAX Toolkit, ovládací prvek AutoCompleteExtender lze přidat do stránky podobně jako byste přidali regulární serverový ovládací prvek ASP.NET. Výpis 19 ukazuje příklad použití ovládacího prvku k volání webové služby.

**Výpis 19. Použití ovládacího prvku ASP.NET AJAX Toolkit AutoCompleteExtender.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender má několik různých vlastností, včetně standardní vlastnosti ID a runat na serverové ovládací prvky. Kromě toho umožňuje definovat, kolik znaků typy koncového uživatele před webová služba je dotazován na data. Vlastnost MinimumPrefixLength ukazuje výpis 19 způsobí, že služba má být volána pokaždé, když je zadaný znak do textového pole. Bude nutné mít na paměti, že tuto hodnotu nastavíte, protože pokaždé, když uživatel zadá znak webová služba bude volána k vyhledání hodnoty, které odpovídají znaky do textového pole. Volání webové služby, jakož i cílové webové metody jsou definovány pomocí vlastnosti ServicePath a ServiceMethod v uvedeném pořadí. Vlastnost TargetControlID nakonec identifikuje které textové pole k připojení AutoCompleteExtender ovládacího prvku.

Volání webové služby musí mít atribut ScriptService použít, protože jsme probírali výše a cílové webové metody, musíte přijmout dva parametry s názvem prefixText a počet. Parametr prefixText představuje zadané znaky koncovým uživatelem, a parametr počet představuje, kolik položek k vrácení (výchozí hodnota je 10). Výpis 20 ukazuje příklad GetCustomerIDs webové metody volané AutoCompleteExtender ovládací prvek výše v informacích 19. Metodu webové volá metodu obchodní vrstvy, pak volání datové vrstvy metoda, která zpracovává filtrování dat a vrátí odpovídající výsledky. Kód pro metodu vrstvy dat je zobrazena ve výpisu 21.

**Výpis 20. Filtrování dat odesílaných z ovládacího prvku AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Výpis 21. Filtrování výsledků na základě vstupního koncového uživatele.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Závěr

ASP.NET AJAX poskytuje skvělou podporu pro volání webové služby, aniž byste museli psát spoustu vlastní kód JavaScript ke zpracování zpráv požadavků a odpovědí. V tomto článku už víte, jak povolit AJAX .NET webové služby je chcete povolit zpracování zpráv JSON a definování třídy proxy JavaScript pomocí ovládacího prvku ScriptManager. Také jste viděli, jak JavaScript proxy slouží k volání webové služby, zpracování jednoduché a komplexní typy a řešit chyby. Nakonec jste viděli, jak lze pomocí metody stránky zjednodušit proces vytváření a volání webové služby a jak AutoCompleteExtender ovládacího prvku může poskytnout nápovědu pro koncové uživatele jako jejich typu. I když k dispozici v technologii ASP.NET AJAX UpdatePanel bude určitě ovládacího prvku volba pro mnoho programátorů AJAX z důvodu jeho jednoduchost, může být užitečné v mnoha aplikacích vědět, jak pomocí třídy proxy JavaScript volání webové služby.

## <a name="bio"></a>Životopis

DaN Wahlin (Microsoft Most Valuable Professional pro ASP.NET a webových služeb XML) je .NET development instruktorem a architektura konzultant na technické školení rozhraní ([http://www.interfacett.com](http://www.interfacett.com)). DaN založil XML pro vývojáře v prostředí ASP.NET Web ([www.XMLforASP.NET](http://www.XMLforASP.NET)), která je na mluvčího INETA kancelář a přednáší na konferencích několik. DaN spoluautorem DNA Windows Professional (Wrox), technologie ASP.NET: tipy, kurzy a kód (edice), technologie ASP.NET 1.1 Insider řešení, Professional ASP.NET 2.0 AJAX (Wrox), technologie ASP.NET 2.0 MVP změní a vytvořené XML pro vývojáře využívající technologii ASP.NET (edice). Když mu není psaní kódu, články nebo knihy, Dan infrastrukturální, zápis a Hudba záznam a přehrávání golf a Basketbalový se svou ženou a děti.

Scott Cate má práce s Microsoft webových technologiích od roku 1997 a je prezident myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na technologie ASP.NET psaní aplikací, zaměřuje na znalostní báze softwarová řešení založených na. Scott můžete kontaktovat prostřednictvím e-mailové adrese [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo na svém blogu [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-localization.md)
> [další](understanding-asp-net-ajax-debugging-capabilities.md)
