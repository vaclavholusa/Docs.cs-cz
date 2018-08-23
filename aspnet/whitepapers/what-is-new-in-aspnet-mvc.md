---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Co je nového v architektuře ASP.NET MVC 2 | Dokumentace Microsoftu
author: rick-anderson
description: Tento dokument popisuje nové funkce a vylepšení v ASP.NET MVC 2. Tento dokument slouží také k dispozici ke stažení.
ms.author: riande
ms.date: 04/20/2010
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: 82a3fd4fe74202ed9a23298390322458cfc029f8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753448"
---
<a name="whats-new-in-aspnet-mvc-2"></a>Co je nového v architektuře ASP.NET MVC 2
====================
> Tento dokument popisuje nové funkce a vylepšení v ASP.NET MVC 2. Tento dokument je také k dispozici pro [stáhnout](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Úvod](#_TOC1)   
[Aktualizace projektu ASP.NET MVC 1,0 do architektury ASP.NET MVC 2](#_TOC2)   
[Nové funkce](#_TOC3)   
[Pomocnými objekty](#_TOC3_1)   
[Oblasti](#_TOC3_2)   
[Podpora pro asynchronní řadiče](#_TOC3_3)   
[Podpora pro DefaultValueAttribute – v parametrů metody akce](#_TOC3_4)   
[Podpora pro vazby binárních dat s Vazači modelu](#_TOC3_5)   
[ModelMetadata a ModelMetadataProvider třídy](#_TOC3_6)   
[Podpora pro atributy DataAnnotations](#_TOC3_7)   
[Zprostředkovatele ověření modelu](#_TOC3_8)   
[Ověřování na straně klienta](#_TOC3_9)   
[Nové fragmenty kódu pro Visual Studio 2010](#_TOC3_10)   
[Nový filtr akce RequireHttpsAttribute](#_TOC3_11)   
[Přepsání metody HTTP akce](#_TOC3_12)   
[Nová třída HiddenInputAttribute pro pomocnými objekty](#_TOC3_13)   
[Metoda Html.ValidationSummary Helper můžete zobrazit chyby na úrovni modelu](#_TOC3_14)   
[Šablony T4 v aplikaci Visual Studio vygenerovat kód, který je specifické pro cílovou verzi rozhraní .NET Framework](#_TOC3_15)[vylepšení rozhraní API](#_TOC4)  
[Rozbíjející změny v](#_TOC5)  
[Právní omezení](#_TOC6)  

## <a id="_TOC1"></a>  Úvod

ASP.NET MVC 2 staví na ASP.NET MVC 1,0 a představuje velkou sadu vylepšení a funkcí, které jsou zaměřené na zvýšení produktivity. Tato verze je kompatibilní s ASP.NET MVC 1.0, takže všechny znalostní báze, dovednosti, kód a rozšíření pro ASP.NET MVC 1,0 pokračovat v používání.

Další informace o architektuře ASP.NET MVC naleznete na následujících odkazech:

- [Dokumentace k ASP.NET MVC na webu MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Na webu technologie ASP.NET MVC](https://asp.net/mvc/)
- [Fóra ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>  Aktualizace projektu ASP.NET MVC 1,0 do architektury ASP.NET MVC 2

ASP.NET MVC 2 je možné nainstalovat souběžně s ASP.NET MVC 1,0 na stejném serveru, která poskytuje aplikace vývojáři flexibilitu při výběru, kdy se má upgradovat aplikaci ASP.NET MVC 1,0 do ASP.NET MVC 2. Informace o tom, jak upgradovat, naleznete v dokumentu [upgrade aplikace ASP.NET MVC 1,0 ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>  Nové funkce

Tato část popisuje funkce, které se zavedly ve verzi 2 na MVC.

### <a id="_TOC3_1"></a>  Pomocnými objekty

Pomocnými objekty se vám umožní automaticky přidružených elementů HTML pro úpravy a zobrazení s datovými typy. Například když v zobrazení se zobrazí data typu System.DateTime, prvku Výběr data uživatelského rozhraní lze automaticky vykreslit. To se podobá fungování šablony polí v dynamických dat ASP.NET. Další informace najdete v tématu [pomocí pomocnými objekty se k datům zobrazení](https://go.microsoft.com/fwlink/?LinkId=159062) na webové stránce MSDN.

### <a id="_TOC3_2"></a>  Oblasti

Oblasti umožňují organizovat velkého projektu do několika menších oddílů, aby bylo možné zvládnout složité velké webové aplikace. Každý oddíl ("oblasti") obvykle představuje samostatný oddíl velký webové stránky a slouží k seskupení souvisejících sad kontrolerů a zobrazení. Další informace najdete v tématu [návod: uspořádání aplikaci ASP.NET MVC v oblastech](https://go.microsoft.com/fwlink/?LinkId=158978) na webové stránce MSDN.

Chcete-li vytvořit novou oblast, v Průzkumníku řešení klikněte pravým tlačítkem na projekt, klikněte na tlačítko Přidat a potom klikněte na oblast. Zobrazí se dialogové okno, které vás vyzve k zadání názvu oblasti. Po zadání názvu oblasti Visual Studio přidá do projektu novou oblast.

Následující obrázek znázorňuje příklad rozvržení pro projekt se dvě oblasti, správu a blogy.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Když vytvoříte oblast, Visual Studio přidá třídu, která je odvozena z AreaRegistration do každé oblasti. Tato třída je požadované pro registraci oblasti a jeho cesty, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Výchozí šablona projektu pro ASP.NET MVC 2 obsahuje volání metodě RegisterAllAreas v kódu pro soubor Global.asax. Metoda registruje jednotlivých oblastí v projektu tak, že hledá všechny typy, které jsou odvozeny z třídy AreaRegistration, vytvoření instance typu a následným voláním metody RegisterArea na instanci. Následující příklad ukazuje, jak to lze provést.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Pokud nezadáte oboru v metodě RegisterArea voláním kontextu. Metoda Namespaces.Add, obor názvů, třídy registrace se používá ve výchozím nastavení.

### <a id="_TOC3_3"></a>  Podpora pro asynchronní řadiče

ASP.NET MVC 2 teď umožňuje řadiče k asynchronnímu zpracování požadavků. To může vést k zvýšení výkonu tím, že servery, které často vyžadují blokující operace (jako jsou síťové požadavky) místo toho volat neblokující protějšky. Další informace najdete v tématu [pomocí asynchronní kontroler v architektuře ASP.NET MVC](https://msdn.microsoft.com/library/ee728598(v=VS.100).aspx) tématu na webu MSDN.

### <a id="_TOC3_4"></a>  Podpora pro DefaultValueAttribute – v parametrů metody akce

Třída System.ComponentModel.DefaultValueAttribute umožňuje výchozí hodnota pro argument parametru pro metodu akce. Předpokládejme například, že je definována následující výchozí trasu:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Taky se předpokládá, že je definována následující metoda kontroleru a akce:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Některé z následujících požadavek adresy URL se vyvolá metoda akce zobrazení, která je definována v předchozím příkladu.

- / Článku/zobrazit/123
- / Článku/zobrazit/123? stránky = 1 (efektivně stejný jako předchozí žádosti)
- / Článku/zobrazit/123? stránky = 2

Bez atributu DefaultValueAttribute – nebude fungovat první adresa URL z předchozího seznamu, protože stránka argument je typu hodnotu Null, jehož hodnota nebyla zadána.

Pokud váš kód je napsána v jazyce Visual Basic 2010 nebo Visual C# 2010, můžete použít nepovinné parametry místo atributu DefaultValueAttribute – jak je znázorněno v následujícím příkladu:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>  Podpora pro vazby binárních dat s Vazači modelu

Existují dvě nová přetížení Html.Hidden pomocné rutiny, které kódování binární hodnoty jako řetězce s kódováním base-64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Typické použití je časové razítko pro objekt vložit do zobrazení. Vaše aplikace může obsahovat například následující objekt produktu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Formulář pro úpravy může mít za následek vlastnost časového razítka v podobě, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Tento kód vykreslí skrytý element input s hodnotou časové razítko jako řetězec kódování base-64, který vypadá podobně jako v následujícím příkladu:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Tento formulář může odeslat na metodu akce, který má argument typu Product, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

V metodě akce je vlastnost časového razítka naplnit správně, protože odeslaných kódování base-64 řetězec je převeden na bajtové pole.

### <a id="_TOC3_6"></a>  ModelMetadata a ModelMetadataProvider třídy

Třída ModelMetadataProvider poskytuje abstrakci pro získání metadat pro model v zobrazení. MVC 2 zahrnuje výchozího zprostředkovatele, který zpřístupňuje metadat, který je zveřejněný prostřednictvím atributů v oboru názvů System.ComponentModel.DataAnnotations. Je možné vytvořit zprostředkovatele metadat, které poskytují metadata z jiných úložišť dat, jako jsou databáze nebo soubory XML.

Třída objektu ViewDataDictionary zveřejňuje objekt ModelMetadata, který obsahuje metadata, která se extrahují z modelu třídou ModelMetadataProvider. To umožňuje pomocnými objekty využívají tato metadata a odpovídajícím způsobem upravit svůj výstup.

Další informace najdete v tématu v dokumentaci [ModelMetadata](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) a [ModelMetadataProvider](https://msdn.microsoft.com/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) třídy.

### <a id="_TOC3_7"></a>  Podpora pro atributy DataAnnotations

ASP.NET MVC 2 podporuje používání atributů ověření RangeAttribute, RequiredAttribute, StringLengthAttribute a RegexAttribute (definovaný v oboru názvů System.ComponentModel.DataAnnotations) při vázat k modelu k poskytnutí vstupu ověření.

Další informace najdete v tématu [postupy: ověření modelu dat pomocí DataAnnotations atributy](https://go.microsoft.com/fwlink/?LinkId=159063) na webové stránce MSDN. Ukázkový projekt, který ukazuje použití těchto atributů je k dispozici ke stažení na [ https://go.microsoft.com/fwlink/?LinkId=157753 ](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>  Zprostředkovatele ověření modelu

Třída zprostředkovatele ověření modelu představuje abstrakci, který poskytuje logiku ověření pro model. ASP.NET MVC zahrnuje na základě atributů ověření, které jsou zahrnuty v oboru názvů System.ComponentModel.DataAnnotations výchozího zprostředkovatele. Můžete také vytvořit vlastní zprostředkovatele ověřování, definující vlastní ověřovací pravidla a vlastní mapování pravidla ověření do modelu. Další informace najdete v tématu v dokumentaci [ModelValidatorProvider](https://msdn.microsoft.com/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) třídy.

### <a id="_TOC3_9"></a>  Ověřování na straně klienta

Třída zprostředkovatele ověření modelu zveřejňuje metadata ověření do prohlížeče ve formátu JSON serializovat data, která mohou být spotřebovány knihovnu ověřování na straně klienta. ASP.NET MVC 2 obsahuje klientské knihovny pro ověřování a adaptér, který podporuje atributů ověření obor názvů DataAnnotations jste si předtím poznamenali. Třída zprostředkovatele také umožňuje používat další knihovny ověřování na straně klienta napsáním adaptér, který zpracovává JSON data a volání do alternativní knihovny.

### <a id="_TOC3_10"></a>  Nové fragmenty kódu pro Visual Studio 2010

Pomocí sady Visual Studio 2010 je nainstalována sada fragmentů kódu HTML pro technologii ASP.NET MVC 2. Chcete-li zobrazit seznam tyto fragmenty kódu v nabídce Nástroje, vyberte Správce fragmentů kódů. Pro jazyk vyberte ve formátu HTML a jako umístění, ASP.NET MVC 2. Další informace o tom, jak používat fragmenty kódu, naleznete v dokumentaci k sadě Visual Studio.

### <a id="_TOC3_11"></a>  Nový filtr akce RequireHttpsAttribute

ASP.NET MVC 2 zahrnuje novou RequireHttpsAttribute třídu, která lze použít u metody akce a kontrolery. Ve výchozím nastavení filtr přesměruje žádost než SSL HTTP na ekvivalentní s podporou protokolu SSL (HTTPS).

### <a id="_TOC3_12"></a>  Přepsání metody HTTP akce

Při vytváření webu pomocí styl architektury REST příkazy HTTP slouží k určení jakou akci chcete provést pro prostředek. Vyžaduje REST, které aplikace podporují celou škálu běžné příkazy HTTP, včetně GET, PUT, POST a odstranění.

ASP.NET MVC 2 zahrnuje nové atributy, které můžete použít u metody akce a kompaktní syntaxe funkce. Tyto atributy umožňují ASP.NET MVC a vyberte metodu akce založené na příkazu HTTP. V následujícím příkladu požadavek POST bude volat metodu první akci a požadavek PUT bude volat druhou metodu akce.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

V dřívějších verzích rozhraní ASP.NET MVC tyto metody akce vyžaduje podrobnější syntaxe, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Prohlížeče podporují jenom operace GET a POST protokolu HTTP, není možné publikovat na akci, která vyžaduje jinou operací. Proto není možné nativně podporují všechny požadavky rozhraní RESTful.

Ale pro podporu RESTful požadavků během operací POST, ASP.NET MVC 2 představuje nový způsob pomocné rutiny HttpMethodOverride HTML. Tato metoda vykreslí ještě skrytý element input, který způsobí, že formulář efektivně emulovat jakoukoli metodu HTTP. Například můžete pomocí metody pomocné rutiny HttpMethodOverride HTML mít odeslání formuláře se zobrazí být požadavek PUT nebo DELETE. Chování HttpMethodOverride má vliv na následující atributy:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Skrytý element input má jeho název X-HTTP-Method-Override a jeho hodnotu nastavte na příkazu HTTP pro emulaci. Hodnotu přepsání můžete také zadat v hlavičce protokolu HTTP nebo hodnotu řetězce dotazu jako dvojice název/hodnota.

Přepsání jde použít jenom při skutečné požadavek není požadavek POST. Hodnota přepsání bude ignorována pro požadavky, které používají jiné příkaz protokolu HTTP.

### <a id="_TOC3_13"></a>  Nová třída HiddenInputAttribute pro pomocnými objekty

Nový atribut HiddenInputAttribute můžete použít s vlastností modelu k označení, zda má být vykreslen skrytý element input při zobrazení modelu v šabloně editoru. (Atribut nastaví hodnotu implicitní UIHint HiddenInput). Vlastnost atributu položky DisplayValue umožňuje určit, zda hodnota se zobrazí v editoru a režimů zobrazení. Když položky DisplayValue je nastavené na false, nezobrazí se, ne v případě značka jazyka HTML, který se normálně zobrazí se kolem pole. Výchozí hodnota pro položky DisplayValue je true.

Atribut HiddenInputAttribute můžete použít v následujících scénářích:

- Při zobrazení umožňuje uživatelům upravit ID objektu a je nutné k zobrazení hodnoty a aby poskytovala skrytý element input, který obsahuje původní ID tak, aby může být předán zpět do kontroleru.
- Při zobrazení umožňuje uživatelům upravovat binární vlastnost, která by nikdy nezobrazí, jako je například vlastnost časového razítka. V takovém případě se nezobrazují hodnotu a okolního značky HTML (třeba popisek a hodnota).

Následující příklad ukazuje způsob použití třídy HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Když je atribut nastaven na hodnotu true (nebo není zadán žádný parametr), dojde k následujícímu:

- V zobrazení šablony popisek se vykreslí a hodnota se zobrazí uživateli.
- V editoru šablon popisek se vykreslí a hodnota je vykreslen ve skrytém elementu input.

Když je atribut nastaven na hodnotu false, dojde k následujícímu:

- V zobrazení šablony nic se vykreslí pro toto pole.
- V editoru šablon je vykreslen žádný popisek a hodnota je vykreslen ve skrytém elementu input.

### <a id="_TOC3_14"></a>  Metoda Html.ValidationSummary Helper můžete zobrazit chyby na úrovni modelu

Ne vždy zobrazuje všechny chyby ověření, má metodu helper Html.ValidationSummary novou možnost, chcete-li zobrazit pouze chyby na úrovni modelu. Díky tomu chyb na úrovni modelu zobrazený v souhrnu ověření a pole konkrétních chyb, který se má zobrazit vedle jednotlivých polí.

### <a id="_TOC3_15"></a>  Šablony T4 v aplikaci Visual Studio vygenerovat kód, který je specifické pro cílovou verzi rozhraní .NET Framework

Nové vlastnosti je k dispozici k souborům T4 z hostitele technologie ASP.NET MVC T4, která určuje verzi rozhraní .NET Framework, který používá aplikace. To umožňuje šablon T4 pro generování kódu a kód, který je specifický pro verzi rozhraní .NET Framework. V sadě Visual Studio 2008 hodnota je vždy .NET 3.5. V sadě Visual Studio 2010 hodnota je rozhraní .NET 3.5 nebo .NET 4.

## <a id="_TOC4"></a>  Vylepšení rozhraní API

Tato část popisuje změny na stávající technologie ASP.NET MVC typy a členy.

- Přidat chráněnou virtuální metodu CreateActionInvoker ve třídě Controller. Tato metoda vyvolá vlastnost ActionInvoker řadiče a umožňuje opožděné instanciace původce Pokud žádná původce volání je již nastaven.
- Přidat chráněnou virtuální metodu HandleUnauthorizedRequest ve třídě třídy AuthorizeAttribute. To umožňuje filtry, které jsou odvozeny od třídy AuthorizeAttribute pro řízení chování, když se nezdaří autorizace.
- Přidat metodu Add (řetězce klíčů, hodnota objektu) ve třídě ValueProviderDictionary. Díky tomu budete moci použít syntaxi inicializátoru slovníku pro ValueProviderDictionary, jako v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Přidání get\_objekt metody ve třídě Sys.Mvc.AjaxContext. Toto je metoda jazyka JavaScript, který je podobný get\_metoda dat, ale pokud typ obsahu odpovědi je application/json, získejte\_objekt vrátí objekt JSON.
- Přidat vlastnost ActionDescriptor ve třídě AuthorizationContext měl obsahovat.
- Přidání UrlParameter.Optional token, který je možné obejít potíže při vytváření vazby modelu, který obsahoval vlastnost ID, když je vlastnost chybí v odeslaného formuláře. Další podrobnosti naleznete v příspěvku [ASP.NET MVC 2 volitelné parametry adresy URL](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) v blogu phila Haacka.

## <a id="_TOC5"></a>  Rozbíjející změny v

Následující změny může způsobit chyby v existujících aplikacích ASP.NET MVC 1,0.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Změna v chování ověřování vlastnost třídy, které implementují IDataErrorInfo

Pro objekty modelu, které používají IDataErrorInfo provést ověření se ověří každý vlastnost bez ohledu na to, zda byla nastavena nová hodnota. V technologii ASP.NET MVC 1.0 byly ověřeny pouze vlastnosti, které měly nastavte nové hodnoty. V ASP.NET MVC 2 se nazývá vlastnost chybového IDataErrorInfo pouze v případě, že všechny validátory vlastnost proběhly úspěšně.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skript mapování skriptů služby IIS již není k dispozici v instalačním programu

Skript pro mapování skriptů služby IIS je skript příkazového řádku, který se používá ke konfiguraci služby IIS 6 a IIS 7 mapy skriptů v klasickém režimu. Pokud používáte vývojový Server sady Visual Studio, nebo pokud používáte IIS 7 v integrovaném režimu, není nutná skriptu mapování skriptů. Tyto skripty jsou k dispozici jako samostatné nepodporované ke stažení na [skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Pomocná metoda Html.Substitute v termínu MVC již není k dispozici

Z důvodu změn v chování vykreslování pro moduly zobrazení MVC Pomocná metoda Html.Substitute nefunguje, byla odebrána.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Rozhraní IValueProvider nahradí všechny výskyty IDictionary

Každé vlastnosti nebo metody argument, který přijato IDictionary v MVC 1.0 nyní přijímá IValueProvider. Tato změna ovlivní pouze aplikace, které zahrnují poskytovatelé vlastní hodnotu nebo vlastního modelu vazače. Příklady vlastností a metod, které jsou ovlivněny této změny patří:

- Vlastnost položku ValueProvider ControllerBase a ModelBindingContext tříd.
- TryUpdateModel metody třídy Kontroleru.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>V souboru Site.css byly přidány nové třídy šablony stylů CSS

Aktualizovali jsme soubor Site.css v šablonách projektů ASP.NET MVC zahrnout nové styly používané funkce ověřování a pomocnými objekty.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Pomocné rutiny teď vrátit objekt MvcHtmlString

Abyste mohli využívat nové syntaxe výrazu kódování HTML v technologii ASP.NET 4, návratový typ pro pomocných rutin HTML je teď MvcHtmlString namísto řetězce. Pokud používáte ASP.NET MVC 2 a nové pomocné rutiny na technologii ASP.NET 3.5, nebudete moci využít výhod kódování HTML syntaxe; Nová syntaxe je k dispozici pouze v případě spuštění ASP.NET MVC 2 na ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult nyní odpovídá pouze na požadavky HTTP POST

Aby se zmírnilo JSON útokům, které mají potenciál pro zpřístupnění informací, ve výchozím nastavení, třída JsonResult nyní odpovídá pouze na požadavky HTTP POST. ZÍSKAT AJAX volání metody akce, které vracejí objekt JsonResult, měli byste místo místo toho použijte příspěvku. V případě potřeby toto chování můžete přepsat tak, že nastavíte vlastnost nové JsonRequestBehavior JsonResult. Další informace o potenciální zneužití, naleznete v příspěvku blogu [JSON zneužitím](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) v blogu phila Haacka.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model a nastavením vlastností ModelType na ModelBindingContext jsou zastaralé

Do třídy ModelBindingContext se přidala nová nastavitelnou vlastnost ModelMetadata. Nové vlastnosti zapouzdřuje ModelType vlastností a modelu. I když jsou zastaralé vlastnosti modelu a ModelType, z důvodu zpětné kompatibility gettery vlastností fungují. k načtení hodnoty delegují vlastnost ModelMetadata.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Změny třídy DefaultControllerFactory přerušit vlastní objekty Factory, které jsou odvozeny z něj

Třída DefaultControllerFactory šlo vyřešit tak, že odeberete vlastnost kontext požadavku. Namísto této vlastnosti je předána instance kontextu požadavku chráněné virtuální metody GetControllerInstance a GetControllerType. Tato změna ovlivní vlastní objekty Factory, které jsou odvozeny z DefaultControllerFactory.

Vlastní objekty Factory se často používají k zajištění injektáž závislostí pro architekturu ASP.NET MVC aplikace. Aktualizovat vlastní kontrolerů pro podporu technologie ASP.NET MVC 2, změna podpisu metody nebo signatury tak, aby odpovídala nové podpisy a místo vlastnost použít parametr kontextu požadavku.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Oblasti", je teď klíč vyhrazené hodnota trasy

Řetězec "oblasti" v hodnoty trasy nyní stejným způsobem, který "controller" a "action" má zvláštní význam v architektuře ASP.NET MVC. Jeden důsledkem je, že pokud pomocných rutin HTML se dodávají s nástrojem hodnota trasy slovník obsahující "oblasti", pomocné rutiny už připojí "oblasti" v řetězci dotazu.

Pokud používáte funkci oblastí, ujistěte se, že nechcete použít {oblasti} jako část adresy URL trasy.


## <a id="_TOC6"></a>  Právní omezení

Toto je předběžná dokumentu a může podstatně změnit před finální komerční verzi softwaru, které jsou popsány zde.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na tyto problémy k datu publikování. Protože Microsoft reagovat na měnící se podmínky na trhu, neměly by být vykládány jako závazek Microsoftu a společnost Microsoft nemůže zaručit přesnost jakýchkoli informací, které jsou prezentovány po provedení datu publikování.

Tento dokument White Paper se pouze k informačním účelům. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÝCH, ODVOZENÝCH NEBO ZÁKONNÝCH, INFORMACE V TOMTO DOKUMENTU.

V souladu s příslušnými zákony o autorských právech je zodpovědný uživatel. Bez omezení autorská práva, žádná část tohoto dokumentu může být reprodukovat, uložené v zavedené rozšiřován nebo jakýmkoli způsobem (electronic, mechanickým, mechanicky, záznam nebo jinak) nebo pro libovolný účel, bez výslovného písemného povolení společnosti Microsoft Corporation.

Společnost Microsoft může vlastnit patenty, patentové přihlášky, ochranné známky, autorská práva nebo další práva na duševní vlastnictví přihlášky v tomto dokumentu. Výslovně uvedeno v písemné licenční smlouvě se společností Microsoft, poskytnutím tohoto dokumentu vám není udělena licence k těmto patentům, ochranné známky, autorská práva nebo jinému duševnímu vlastnictví.

Pokud není uvedeno jinak, společnosti, organizace, produkty, názvy domén, e-mailové adresy, loga, osoby, místa a události použité v ukázkách jsou smyšlené. proto žádné jejich spojení se žádné skutečnou společností, organizací, produktu, název domény, e-mailu adresy, loga, osoby, místa nebo událostí je určena ji vyvozovat.

© Microsoft 2010 Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech amerických a dalších zemích.

Názvy skutečných společností a produktů mohou být ochrannými známkami příslušných vlastníků.
