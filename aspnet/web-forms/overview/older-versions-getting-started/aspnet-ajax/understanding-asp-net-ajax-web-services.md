---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Principy webových služeb ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Webové služby jsou nedílnou součástí rozhraní .NET framework, které poskytují řešení a platformy pro výměnu dat mezi distribuované systémy. I když Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-web-services"></a>Principy webových služeb ASP.NET AJAX
====================
podle [Scott Dikovat](https://github.com/scottcate)

[Stáhnout PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Webové služby jsou nedílnou součástí rozhraní .NET framework, které poskytují řešení a platformy pro výměnu dat mezi distribuované systémy. I když webové služby se obvykle používají k povolit různých operačních systémech, objektové modely a programovací jazyky odesílat a přijímat data, můžete také používají pro dynamicky vložit data do stránku ASP.NET AJAX nebo odeslání dat z stránky systému back-end. Všechny tyto jde provést bez opětné řazení odeslání operace.


## <a name="calling-web-services-with-aspnet-ajax"></a>Volání webové služby pomocí prvku ASP.NET AJAX

Dana Wahlin

Webové služby jsou nedílnou součástí rozhraní .NET framework, které poskytují řešení a platformy pro výměnu dat mezi distribuované systémy. I když webové služby se obvykle používají k povolit různých operačních systémech, objektové modely a programovací jazyky odesílat a přijímat data, můžete také používají pro dynamicky vložit data do stránku ASP.NET AJAX nebo odeslání dat z stránky systému back-end. Všechny tyto jde provést bez opětné řazení odeslání operace.

Zatímco ovládacího prvku ASP.NET AJAX UpdatePanel poskytuje jednoduchý způsob, jak AJAX povolit jakoukoli stránku ASP.NET, může nastat situace, když potřebujete dynamicky bez použití ovládacího prvku UpdatePanel přístup k datům na serveru. V tomto článku se zobrazí, jak dosáhnout vytvořením a využívání webových služeb v rámci prvku ASP.NET AJAX stránky.

Tento článek se soustředí na funkce, které jsou k dispozici v rozšíření základní ASP.NET AJAX, jakož i ovládacího prvku webové služby, které jsou povolené v ASP.NET AJAX Toolkit volat AutoCompleteExtender. Témata zahrnují definování podporou AJAXU webové služby, vytvoření klienta proxy a volání webové služby v jazyce JavaScript. Dozvíte se taky, jak volání webové služby můžete provést přímo do metody stránky ASP.NET.

## <a name="web-services-configuration"></a>Konfigurace webové služby

Při vytvoření nového projektu webové stránky s Visual Studio 2008, v souboru web.config má počet novými doplňky, které mohou být obeznámeni uživatelům předchozích verzí sady Visual Studio. Některé tyto úpravy, takže je pak můžete použít na stránkách, zatímco ostatní zadejte požadované HttpHandlers a HttpModules mapování předponu "asp" na ovládací prvky ASP.NET AJAX. Výpis 1 zobrazuje změny provedené `<httpHandlers>` element v souboru web.config, který má vliv na volání webové služby. Výchozí hodnota, které httpHandlers používá ke zpracování .asmx volání je odebrána a nahradí třídu ScriptHandlerFactory umístěný v sestavení System.Web.Extensions.dll. System.Web.Extensions.dll obsahuje všechny základní funkce používá prvku ASP.NET AJAX.

**Výpis 1. Konfiguraci obslužné rutiny ASP.NET AJAX webové služby**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Tato nahrazení httpHandlers přišla Chcete-li povolit volání jazyka JavaScript Object Notation (JSON) provést ze stránek ASP.NET AJAX, webové služby .NET pomocí proxy JavaScript webové služby. Technologie ASP.NET AJAX odešle zprávy JSON k webovým službám oproti standardní volání objektu přístup protokolu SOAP (Simple) obvykle související s webovými službami. Výsledkem je menší požadavku a odpovědi na zprávy celkové. Umožňuje také pro efektivnější zpracování na straně klienta dat vzhledem k tomu, že knihovna ASP.NET AJAX JavaScript je optimalizovaná pro práci s objekty JSON. Výpis 2 a 3 výpis příklady zobrazit zprávy požadavku a odpovědi webové služby serializován do formátu JSON. Zpráva požadavku na seznam 2 předává země parametr s hodnotou "Belgie", zatímco zprávu odpovědi v výpis 3 předá pole zákaznických objektů a jejich vlastnosti.

**Výpis 2. Zpráva požadavku webové služby serializovat na JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] Název operace je definován jako část adresy URL k webové službě; Kromě toho zprávy žádosti nejsou vždy odeslat prostřednictvím formátu JSON. Webové služby můžete využít atribut ScriptMethod s parametrem UseHttpGet nastavit na hodnotu true, což způsobí, že předávání pomocí parametrů parametrů řetězce dotazu.*


**Výpis 3. Zpráva odpovědi webové služby serializovat na JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

V další části se zobrazí, jak vytvořit Web Services umožňující zpracování zprávy požadavku JSON a reagovat s jednoduché a komplexní typy.

## <a name="creating-ajax-enabled-web-services"></a>Vytváření podporou AJAXU webové služby

Rozhraní ASP.NET AJAX poskytuje několik různých způsobů, jak volání webové služby. Můžete použít ovládací prvek AutoCompleteExtender (k dispozici v sadě nástrojů ASP.NET AJAX) nebo JavaScript. Ale před voláním služby budete muset povolit AJAX ho tak, aby ji můžete volat kód klientského skriptu.

Jestli začínáte webových služeb ASP.NET, najdete ho přehledné k vytvoření a povolení AJAX služby. Rozhraní .NET framework má podporované vytváření webových služeb ASP.NET od prvního vydání v 2002 a rozšíření ASP.NET AJAX poskytují další funkce AJAX, která staví na rozhraní .NET framework výchozí sadu funkcí. Visual Studio .NET 2008 Beta 2 má integrovanou podporu pro vytváření .asmx souborů webové služby a automaticky přidružený kód vedle třídy je odvozena ze třídy System.Web.Services.WebService. Po přidání metody do třídy je nutné použít atribut WebMethod, aby k volání webové služby příjemci.

Výpis 4 ukazuje příklad použití atribut WebMethod na metodu s názvem GetCustomersByCountry().

**Výpis 4. Používání atributu WebMethod ve webové službě**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

Metoda GetCustomersByCountry() přijme parametr země a vrátí objekt pole zákazníka. Hodnota země předán do metody se předají na obchodní vrstvu třídu, která volá Datová vrstva třída k načtení dat z databáze, zadejte vlastnosti objektu zákazníků s daty a vrátit pole.

## <a name="using-the-scriptservice-attribute"></a>Používání atributu ScriptService

Při přidávání WebMethod atribut umožňuje GetCustomersByCountry() metoda má být volána klienty, kteří odesílají standardní protokolu SOAP zprávy k webové službě, neumožňuje volání JSON má být provedeno z prvku ASP.NET AJAX aplikací mimo pole. Chcete-li povolit volání JSON má být provedeno, budete muset použít rozšíření ASP.NET AJAX `ScriptService` atribut třída webové služby. To umožňuje odesílat zprávy odpovědi formátu JSON pomocí webové služby a umožňuje klientský skript k volání služby odesláním zprávy JSON.

Výpis 5 ukazuje příklad použití atribut ScriptService pro třídu webové služby s názvem CustomersService.

**Výpis 5. Pomocí atributu ScriptService AJAX-povolit webové služby**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

Atribut ScriptService funguje jako značku, která označuje, že nelze volat z kódu skriptu AJAX. Ho nemůže pracovat ve skutečnosti žádné z JSON serializaci nebo deserializaci úkoly, které nastanou na pozadí. ScriptHandlerFactory (nakonfigurovat v souboru web.config) a další související třídy provést hromadné zpracování JSON.

## <a name="using-the-scriptmethod-attribute"></a>Používání atributu ScriptMethod

ScriptService se pouze atribut prvku ASP.NET AJAX, který má být definován ve webové službě .NET v pořadí pro něj má být používána stránky ASP.NET AJAX. Přímo do webové metody ve službě však můžete také použít jiný atribut s názvem ScriptMethod. ScriptMethod definuje tři vlastnosti včetně `UseHttpGet`, `ResponseFormat` a `XmlSerializeString`. Změna hodnoty těchto vlastností může být užitečná v případech, kdy je typ požadavku akceptovat webové metody změnit tak, aby GET, když musí vrátit nezpracovaná data XML ve formě metody webové `XmlDocument` nebo `XmlElement` objekt nebo když se data vrácená z  služby by měl být vždy serializovanou jako XML místo JSON.

UseHttpGet vlastnost lze použít, když by měl přijímají webové metody GET požadavky a požadavky POST. Požadavky jsou odesílány, že použijete adresu URL s vstupních parametrů metody webové převedeny na parametry řetězce dotazu. Nastavit UseHttpGet vlastnost, bude jako výchozí nastavena na hodnotu false a má pouze na `true` při operations známé jako bezpečné a pokud není předán citlivá data k webové službě. Výpis 6 ukazuje příklad použití atributu ScriptMethod s vlastností UseHttpGet.

**Výpis 6. Používání atributu ScriptMethod s vlastností UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Příklad hlavičky odeslána při volání metody webové HttpGetEcho, ukazuje výpis 6 jsou uvedeny další:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Kromě toho, že webové metody tak, aby přijímal požadavky HTTP GET, atribut ScriptMethod lze také při odpovědi XML musí vrátit ze služby spíše než JSON. Webová služba může například načtení informačního kanálu RSS ze vzdálené lokality a vrátit jako objekt třídou XMLDocument nastavenou na nebo XmlElement. Zpracování kódu XML dat pak dochází na straně klienta.

Výpis 7 znázorňuje příklad pomocí formátu ResponseFormat vlastnosti k určení, zda má být vrácen XML data z webové metody.

**Výpis 7. Používání atributu ScriptMethod s vlastností formátu ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

Vlastnost formátu ResponseFormat můžete použít také vlastnost XmlSerializeString. Vlastnost XmlSerializeString má výchozí hodnotu false, to znamená, že všechny vrátí typy kromě řetězce vrácená z webové metody jsou serializovanou jako XML při `ResponseFormat` je nastavena na `ResponseFormat.Xml`. Když `XmlSerializeString` je nastaven na `true`, všechny typy vrácená z webové metody jsou serializovanou jako XML, včetně typů řetězec. Pokud má vlastnost formátu ResponseFormat hodnotu `ResponseFormat.Json` XmlSerializeString vlastnost je ignorována.

Výpis 8 ukazuje příklad pomocí vlastnosti XmlSerializeString vynutit řetězců, které mají být serializovanou jako XML.

**Výpis 8. Používání atributu ScriptMethod s vlastností XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

Hodnota vrácená z volání metody webové GetXmlString ukazuje výpis 8 se zobrazí další:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

I když výchozí formát JSON minimalizuje celkovou velikost žádosti a odpovědi a snadněji spotřebované klienty prvku ASP.NET AJAX v různých prohlížečích způsobem, formátu ResponseFormat a XmlSerializeString vlastnosti mohou být použity při klienta aplikace, jako je například Internet Explorer 5 nebo novější očekávají, že data XML, který se má vrátit z webové metody.

## <a name="working-with-complex-types"></a>Práce s komplexní typy

Výpis 5 vám ukázal, příklad vrací komplexního typu s názvem zákazníka z webové služby. Několik různých jednoduché typy definuje třídu Customer interně jako vlastnosti, jako je jméno a příjmení. Komplexní typy použít jako vstupní parametr nebo návratový typ na webové metody technologie AJAX jsou automaticky serializován do formátu JSON před odesláním na straně klienta. Ale vnořené komplexní typy (jsou vymezeny vnitřně v rámci jiného typu) nejsou k dispozici klienta jako samostatné objekty ve výchozím nastavení.

V případech, kde vnořených komplexní typ, který používá webové služby musí být rovněž použit na stránce klienta můžete přidat atribut ASP.NET AJAX GenerateScriptType k webové službě. Například třída CustomerDetails vidět na výpisu 9 obsahuje vlastnosti adresy a pohlaví který ***představují vnořené komplexní typy.***

**Výpis 9. Třída CustomerDetails tady uvedené obsahuje dva vnořené komplexní typy.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Adresa a pohlaví objekty definované v rámci třídy CustomerDetails vidět na výpisu 9 nebude automaticky být k dispozici pro použití na straně klienta pomocí jazyka JavaScript vzhledem k tomu, že jsou vnořené typy (adresa je třída a pohlaví je výčet). V situacích, kde vnořené typy používaných v rámci webové služby musí být k dispozici na straně klienta, můžete použít atribut GenerateScriptType již bylo zmíněno dříve (viz seznam 10). Tento atribut lze přidat více než jednou. v případech, kdy se různé typy vnořených komplexní vrátí ze služby. Můžete použít přímo na třída webové služby nebo vyšší konkrétní webové metody.

**Výpis 10. Používání atributu GenerateScriptService k definování vnořené typy, které by měly být dostupné pro klienta.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

S použitím `GenerateScriptType` atribut na typy webové služby, adresa a pohlaví bude automaticky k dispozici pro použití v kódu ASP.NET AJAX JavaScript na straně klienta. V výpis 11 je uveden příklad jazyka JavaScript, který je automaticky generované a odeslat klientovi, přidáním atribut GenerateScriptType na webové službě. Uvidíte později v článku použití vnořených komplexní typy.

**Výpis 11. Vnořené komplexní typy, které jsou k dispozici na stránku ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Teď, když už víte, jak vytvořit webové služby a aby byly přístupné na stránky ASP.NET AJAX, Podívejme se na to, jak vytvořit a použít třídy proxy JavaScript tak, aby data můžete načíst nebo odesílány do webové služby.

## <a name="creating-javascript-proxies"></a>Vytvoření třídy proxy JavaScript

Volání metody standardní webové služby (.NET nebo jinou platformu) obvykle zahrnuje vytvoření objekt proxy serveru, který chrání před složitosti odesílání zprávy požadavku a odpovědi protokolu SOAP. Pomocí volání webové služby ASP.NET AJAX můžete vytvořit třídy proxy JavaScript a používá k snadno volání služby bez obav o serializaci a deserializaci JSON zprávy. Třídy proxy JavaScript můžete automaticky generovat pomocí prvku ASP.NET AJAX ScriptManager.

Vytváření proxy JavaScript, který můžete volat webové služby se provádí pomocí vlastnosti služby prvek ScriptManager. Tato vlastnost umožňuje definovat jednu nebo více služeb, které můžete odesílat nebo přijímat data bez nutnosti zpětného volání operace asynchronní volání stránku ASP.NET AJAX. Službu definujete pomocí prvku ASP.NET AJAX `ServiceReference` řízení a přiřazení adresa URL webové služby k ovládacímu prvku `Path` vlastnost. Výpis 12 znázorňuje příklad odkazující na služby s názvem CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Výpis 12. Definování webové služby používané stránku ASP.NET AJAX.**

Přidání odkazu na CustomersService.asmx prostřednictvím ovládací prvek ScriptManager způsobí, že proxy server JavaScript dynamicky vygenerování a odkazuje na stránku. Vložené proxy server pomocí &lt;skriptu&gt; značky a dynamicky načíst volání soubor CustomersService.asmx a připojením algoritmu /js na konec. Následující příklad ukazuje, jak proxy server JavaScript vložený na stránce, když je v souboru web.config zakázat ladění:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Pokud byste chtěli vidět skutečný kód proxy JavaScript, který se vygeneruje můžete do pole Adresa Internet Exploreru zadejte adresu URL k webové službě požadované rozhraní .NET a připojit /js na konec.*


Pokud je povoleno ladění v souboru web.config, které budou vloženy ladicí verze proxy server JavaScript na stránce jako ukazuje následující:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Proxy server JavaScript vytvořené prvek ScriptManager lze také vložit přímo na stránku místo odkazovat pomocí &lt;skriptu&gt; atribut src značky společnosti. Tento krok můžete provést nastavením vlastnosti ServiceReference InlineScript ovládacího prvku na hodnotu true (výchozí hodnota je false). To může být užitečné, pokud proxy server není sdílen napříč více stránek a pokud chcete snížit počet volání sítě na server. Pokud je InlineScript nastavená na hodnotu true, skript proxy serveru nebudou zapisována do mezipaměti v prohlížeči, doporučuje se v případech, kde je proxy server používán více stránek v aplikaci ASP.NET AJAX na výchozí hodnotu false. Příklad použití vlastnost InlineScript se zobrazí další:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Pomocí třídy proxy JavaScript

Jakmile webová služba je odkazován objektem stránku ASP.NET AJAX pomocí ovládacího prvku ScriptManager, provádět volání webové služby a vrácená data mohou být zpracovány pomocí funkce zpětného volání. Webová služba je volána odkazující na jeho obor názvů (pokud existuje), název třídy a název webové metody. Všechny parametry předané k webové službě mohou být definovány společně s funkce zpětného volání, která zpracovává vrácená data.

Příklad použití JavaScript proxy pro volání metody Web s názvem GetCustomersByCountry() je uveden v výpis 13. Funkce GetCustomersByCountry() je volána, když koncový uživatel klikne na tlačítko na stránce.

**Výpis 13. Volání webové služby s proxy server JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Toto volání odkazuje na obor názvů InterfaceTraining, CustomersService třídy a metody webové GetCustomersByCountry definované ve službě. Pak předá hodnotu země získané z textové pole, jakož i zpětného volání funkce s názvem OnWSRequestComplete, která by měla být volána, když se vrátí asynchronní volání webové služby. OnWSRequestComplete zpracovává pole zákaznických objektů vrácená ze služby a převede je na tabulku, která se zobrazí na stránce. Výstup generovaný z volání je znázorněno na obrázku 1.


[![Vazba dat získat tak, že asynchronní volání AJAX k webové službě.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Obrázek 1**: vytvoření vazby dat získat tak, že asynchronní volání AJAX k webové službě.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image3.png))


Třídy proxy JavaScript můžete také provést jednosměrný volání k webovým službám v případech, kdy by měla být volána metoda webové ale proxy server by neměla čekat na odpověď. Můžete například volání webové služby spuštění procesu, jako je například pracovní postup, ale nečekat na návratovou hodnotu ze služby. V případech, kdy je jednosměrný volání služby provést jednoduše lze vynechat ukazuje výpis 13 funkce zpětného volání. Vzhledem k tomu, že je definována žádná funkce zpětného volání nebude Počkejte, než objekt proxy pro webovou službu vrátit data.

## <a name="handling-errors"></a>Zpracování chyb

Asynchronní zpětná volání a webové služby může dojít k různých typů chyb, jako jsou například sítě se dolů, že webová služba není k dispozici nebo výjimku nevrátila. Naštěstí generované prvek ScriptManager objekty proxy JavaScript povolit více zpětná volání a definovat za účelem zpracování chyb a selhání kromě zpětné volání úspěšné uvedena výše. K chybě funkce zpětného volání může být definováno ihned po funkce standardní zpětného volání ve volání webové metody, jak je znázorněno v výpis 14.

**Výpis 14. Definování k chybě funkce zpětného volání a zobrazení chyb.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Všechny chyby, ke kterým dochází při volání webové služby se aktivuje OnWSRequestFailed() funkce zpětného volání k volání, která přijímá objekt představující chybu jako parametr. Objekt error zpřístupňuje několik různých funkcí můžete zjistit příčinu chyby, a zda volání vypršel časový limit. Výpis 14 ukazuje příklad použití funkcí různé chyby a obrázku 2 je znázorněný příklad výstupu generované funkce.


[![Výstup generovaný voláním funkce Chyba prvku ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Obrázek 2**: výstup generovaný voláním funkce Chyba prvku ASP.NET AJAX.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Zpracování kódu XML Data vrácená z webové služby

Dříve jste viděli, jak může vrátit nezpracovaná data XML pomocí atributu ScriptMethod společně s jeho vlastnost formátu ResponseFormat webové metody. Pokud formátu ResponseFormat je nastaven na ResponseFormat.Xml, data vrácená z webové služby je serializovanou jako XML místo JSON. To může být užitečné, když XML data musí být předán přímo na klienta pro zpracování pomocí jazyka JavaScript nebo XSLT. V aktuálním čase Internet Explorer 5 nebo novější poskytuje nejlepší klienta objektový model pro analýzy a filtrování dat XML z důvodu jeho integrovanou podporu pro MSXML.

Načítání dat XML z webové služby je nejsou jiné než načítání jiné datové typy. Spusťte vyvoláním proxy server JavaScript volání odpovídající funkce a definice funkce zpětného volání. Jakmile se volání vrátí můžete pak zpracovat data v funkce zpětného volání.

Výpis 15 ukazuje příklad volání webové metody s názvem GetRssFeed(), která vrací objekt XmlElement. GetRssFeed() přijímá jeden parametr představující adresu URL pro načtení informačního kanálu RSS.

**Výpis 15. Práce s XML data vrácená z webové služby.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Tento příklad předá adresu URL informačního kanálu RSS a zpracuje vrácená data XML ve funkci OnWSRequestComplete(). OnWSRequestComplete() nejdřív zkontroluje, zda je v prohlížeči Internet Explorer vědět, zda analyzátor MSXML je k dispozici. Pokud se jedná, příkaz jazyka XPath se používá k vyhledání všech &lt;položky&gt; značky v rámci informačního kanálu RSS. Každá položka je pak vstupní prostřednictvím a přidružené &lt;název&gt; a &lt;odkaz&gt; značky jsou umístěné a zpracována pro každou položku dat zobrazení. Obrázek 3 ukazuje příklad výstupu generovaného v provádění prvku ASP.NET AJAX volání přes proxy server JavaScript GetRssFeed() webové metody.

## <a name="handling-complex-types"></a>Zpracování komplexní typy

Komplexní typy přijímat nebo vrácený webové služby se automaticky zveřejňují přes proxy server JavaScript. Vnořené komplexní typy však nejsou přímo přístupné na straně klienta, pokud je použit atribut GenerateScriptType ke službě, jak je popsáno výše. Proč byste měli být použít vnořené komplexní typ na straně klienta?

Na tuto otázku odpovědět, předpokládají, že stránku ASP.NET AJAX zobrazuje zákaznická data a umožňuje koncovým uživatelům aktualizovat adresu zákazníka. Pokud webová služba určuje, že typ adresy (komplexního typu definované v rámci třídy CustomerDetails) může odeslat klientovi proces aktualizace je možné rozdělit do samostatné funkce pro lepší kód nové použití.


[![Výstup vytváření z volání webové služby, která vrací data informačního kanálu RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Obrázek 3**: výstup vytváření z volání webové služby, která vrací data informačního kanálu RSS.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image9.png))


Výpis 16 ukazuje příklad kód na straně klienta, která volá adresu objekt definovaný v oboru názvů modelu, vyplní s aktualizovaná data a přiřadí ji k vlastnost objekt CustomerDetails adresy. Objekt CustomerDetails jsou předána do webové služby pro zpracování.

**Výpis 16. Použití vnořených komplexní typy**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Vytvoření a použití metody stránky

Webové služby poskytují vynikající způsob vystavit znovu funkční služby do různých klientů včetně stránky ASP.NET AJAX. Však může být případech, kdy je na stránce načíst data, která se někdy používá nebo sdíleny dalších stránek. V takovém případě provedení soubor ASMX umožnit přístup k datům stránky mohou jevit jako přehnaně vzhledem k tomu, že služba využívají pouze jednu stránku.

Technologie ASP.NET AJAX poskytuje jiný mechanismus pro volání webové služby jako bez vytvoření souborů .asmx samostatné. To se provádí pomocí technik, označuje jako "metody stránky". Stránka metody jsou statické (sdílené v VB.NET) metody vložených přímo v souboru stránky nebo vedle položky kódu, které mají atribut WebMethod na ně použity. Použitím atribut WebMethod bylo možné volat pomocí speciální objekt jazyka JavaScript s názvem PageMethods, která je vytvořena dynamicky za běhu. Objekt PageMethods funguje jako proxy server, který chrání před proces serializaci nebo deserializaci JSON. Všimněte si, aby bylo možné použít objekt PageMethods musí nastavit prvek ScriptManager EnablePageMethods vlastnost na hodnotu true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Výpis 17 ukazuje příklad definování dvě metody stránky v kódu vedle třídu ASP.NET. Tyto metody načtení dat z třídy obchodní vrstvy umístěný v aplikaci\_složky kód webu.

**Výpis 17. Definování metody stránky.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Když ScriptManager zjistí přítomnost webové metody na stránce generuje dynamické odkaz na objekt PageMethods již bylo zmíněno dříve. Volání metody webové se dosahuje prostřednictvím odkazu na třídu PageMethods, za nímž následuje název metody a všechna data povinný parametr, která mají být předány. Výpis 18 jsou uvedeny příklady volání tyto dvě metody stránky, která je uvedena výše.

**Výpis 18. Volání metody stránky s objektem PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Pomocí objektu PageMethods je velmi podobný používání objekt proxy server JavaScript. Je nejdřív zadat všechna data parametr, který by měl být předány metodě stránky a pak zadejte funkce zpětného volání, která by měla být volána, když se vrátí asynchronního volání. Zpětné volání selhání může být zadaná také (odkazovat výpis 14 pro příklad zpracování selhání).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender a ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (k dispozici z [ http://ajax.asp.net ](http://ajax.asp.net)) nabízí několik ovládacích prvků, které lze použít pro přístup k webovým službám. Konkrétně sada nástrojů obsahuje užitečné ovládací prvek s názvem `AutoCompleteExtender` který slouží k volání webové služby a zobrazit data bez psaní kódu jazyka JavaScript na všech stránkách.

Ovládací prvek AutoCompleteExtender lze rozšířit stávající funkce textové pole a pomůže snadno najít data, která se snaží zjistit další uživatelé. Psaní do textové pole řízení lze použít k dotazování webové služby a dynamicky zobrazuje výsledky pod textové pole. Obrázek 4 ukazuje příklad použití ovládacího prvku AutoCompleteExtender zobrazíte ID zákazníka pro podporu aplikace. Jako uživatel zadá různých znaků do textového pole, zobrazí se pod ním podle jejich vstup různé položky. Uživatelé pak můžete vybrat id požadované zákazníka.

Použití AutoCompleteExtender v rámci stránku ASP.NET AJAX vyžaduje, aby sestavení AjaxControlToolkit.dll přidat do složky Bin přejít Web. Po přidání sestavení toolkit, budete chtít odkazovat v souboru web.config, aby byly dostupné pro všechny stránky v aplikaci ovládací prvky, které obsahuje. To lze provést přidáním následující značky v souboru web.config na &lt;ovládací prvky&gt; značky:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

V případech, kdy potřebujete jenom v konkrétní stránky, použijte ovládací prvek něj mohli odkazovat přidáním – direktiva odkaz na horním okraji stránky, jak je vidět další místo aktualizaci souboru web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Použití ovládacího prvku AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Obrázek 4**: použití AutoCompleteExtender ovládacího prvku.  ([Kliknutím zobrazit obrázek v plné velikosti](understanding-asp-net-ajax-web-services/_static/image12.png))


Jakmile web byl nakonfigurován pro použití ASP.NET AJAX Toolkit, ovládacího prvku AutoCompleteExtender lze přidat na stránku mnohem stejně, jako byste přidali regulární ovládací prvek ASP.NET serveru. Výpis 19 ukazuje příklad použití ovládacího prvku pro volání webové služby.

**Výpis 19. Použití ovládacího prvku ASP.NET AJAX Toolkit AutoCompleteExtender.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender má několik různých vlastností včetně standardní ID runat vlastnosti a na ovládací prvky serveru nalezen. Kromě toho umožňuje určit, kolik znaků typy koncového uživatele před webové služby je dotazován na data. Vlastnost MinimumPrefixLength ukazuje výpis 19 způsobí, že službu, kterou chcete volat pokaždé, když je zadán znak do textového pole. Budete chtít dbejte na to, že nastavení této hodnoty od pokaždé, když uživatel zadá znak webová služba bude volána k vyhledání hodnot, které odpovídají znaků do textového pole. Webovou službu k volání, jakož i cílové webové metody jsou definovány pomocí vlastnosti ServicePath a ServiceMethod v uvedeném pořadí. Nakonec vlastnost TargetControlID identifikuje které textbox napojit do ovládacího prvku AutoCompleteExtender.

Webová služba volané musí mít atribut ScriptService použít jak už jsme probírali výše a cílové webové metody musí přijmout dva parametry s názvem prefixText a počet. Parametr prefixText reprezentuje koncovým uživatelem zadané znaky a parametr počet reprezentuje, kolik položek k vrácení (výchozí hodnota je 10). Výpis 20 ukazuje příklad volá ovládacího prvku AutoCompleteExtender uvedené dříve v výpis 19 GetCustomerIDs webové metody. Webové metody volá metodu obchodní vrstvy, pak volání datové vrstvy metoda, která zpracovává filtrování data a vrátilo se odpovídající výsledky. Kód pro metodu vrstvy dat se zobrazí v výpis 21.

**Výpis 20. Filtrování dat odesílaných z ovládacího prvku AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Výpis 21. Filtrování výsledků na základě vstupní koncový uživatel.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Závěr

Technologie ASP.NET AJAX poskytuje vynikající podporu pro volání webové služby bez nutnosti psaní spoustu vlastní kód JavaScript ke zpracování zpráv žádostí a odpovědí. V tomto článku jste se seznámili postup povolení AJAX .NET webové služby a umožňuje jim zpracování zpráv JSON a jak definovat třídy proxy JavaScript pomocí ovládacího prvku ScriptManager. Seznámili jste také způsobu zpracování jednoduché a komplexní typy JavaScript proxy lze použít k volání webové služby a pracují s chybami. Nakonec už víte, jak lze pomocí metody stránky zjednodušit proces vytváření a volání webové služby a jak ovládacího prvku AutoCompleteExtender může poskytnout nápovědu pro koncové uživatele, jejich psaní. I když k dispozici v prvku ASP.NET AJAX UpdatePanel jistě bude ovládací prvek výběru pro mnoho programátory v jazyce AJAX z důvodu jeho jednoduchost, může být užitečné v mnoha aplikacích zároveň budete vědět, jak volat webové služby prostřednictvím třídy proxy JavaScript.

## <a name="bio"></a>Bio

Dana Wahlin (Microsoft nejvíc hodí v situaci Professional pro technologii ASP.NET a webové služby XML) je rozhraní .NET development lektorem a architektura konzultantem na technické školení rozhraní ([http://www.interfacett.com](http://www.interfacett.com)). Dana založena XML pro web ASP.NET s vývojáři ([www.XMLforASP.NET](http://www.XMLforASP.NET)), je na INETA mluvčího kancelář a mluví na několik konferencí. Dana společně vytvořené Professional Windows DNA (Wrox), ASP.NET: tipy, kurzy a kód (Edice nakladatelství Sams), ASP.NET 1.1 vnitřních řešení, Professional ASP.NET 2.0 AJAX (Wrox), útoky zvenku MVP ASP.NET 2.0 a vytvořené XML pro vývojáře využívající technologii ASP.NET (Edice nakladatelství Sams). Při mu není psaní kódu, články nebo knihy, Dana požívá zápis a Hudba záznam a přehrávání golf a Basketbalový s jeho manželku a dětí.

Scott Dikovat pracuje s technologií Microsoft Web od 1997 a je ředitel myKB.com ([www.myKB.com](http://www.myKB.com)) kde mu se specializuje na psaní ASP.NET na základě aplikací, které jsou zaměřené na řešení softwaru znalostní báze Knowledge Base. Scott nelze kontaktovat prostřednictvím e-mailu v [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) nebo jeho blog na [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Předchozí](understanding-asp-net-ajax-localization.md)
> [další](understanding-asp-net-ajax-debugging-capabilities.md)
