---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: "Co je nového v architektuře ASP.NET MVC 2 | Microsoft Docs"
author: rick-anderson
description: "Tento dokument popisuje nové funkce a vylepšení, která byla zavedená v ASP.NET MVC 2. Tento dokument je také k dispozici ke stažení."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: e7f92dd7a09d1986ad775203effcbce76fb0e6f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-2"></a>Co je nového v architektuře ASP.NET MVC 2
====================
> Tento dokument popisuje nové funkce a vylepšení, která byla zavedená v ASP.NET MVC 2. Tento dokument je také k dispozici pro [stáhnout](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Úvod](#_TOC1)   
[Upgrade projektu ASP.NET MVC 1,0 do architektury ASP.NET MVC 2](#_TOC2)   
[Nové funkce](#_TOC3)   
[Objekty se šablonami za](#_TOC3_1)   
[Oblasti](#_TOC3_2)   
[Podpora pro asynchronní řadiče](#_TOC3_3)   
[Podpora pro DefaultValueAttribute – v parametrů metody akce](#_TOC3_4)   
[Podpora pro vazbu s vazače modelů binární Data](#_TOC3_5)   
[ModelMetadata a ModelMetadataProvider třídy](#_TOC3_6)   
[Podpora pro DataAnnotations atributy](#_TOC3_7)   
[Zprostředkovatele ověření modelu](#_TOC3_8)   
[Ověřování na straně klienta](#_TOC3_9)   
[Nové fragmenty kódu pro Visual Studio 2010](#_TOC3_10)   
[Nový filtr akce RequireHttpsAttribute](#_TOC3_11)   
[Přepsání příkaz metoda HTTP](#_TOC3_12)   
[Nová třída HiddenInputAttribute pro objekty se šablonami za](#_TOC3_13)   
[Metoda Html.ValidationSummary Helper můžete zobrazit chyby na úrovni modelu](#_TOC3_14)   
[Šablony T4 v Visual Studio generování kódu, který se liší podle cílové verzi rozhraní .NET Framework](#_TOC3_15)[vylepšení rozhraní API](#_TOC4)  
[Nejnovější změny](#_TOC5)  
[Právní omezení](#_TOC6)  

## <a id="_TOC1"></a>Úvod

ASP.NET MVC 2 staví na ASP.NET MVC 1,0 a zavádí velké sady vylepšení a funkce, které jsou zaměřené na zvýšení produktivity. Tato verze je kompatibilní s ASP.NET MVC 1.0, takže všechny znalostní báze, znalosti, kód a rozšíření pro ASP.NET MVC 1,0 nadále platí.

Další informace o architektuře ASP.NET MVC naleznete v následujících zdrojích informací:

- [ASP.NET MVC dokumentaci na webu MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [Webu technologie ASP.NET MVC](https://asp.net/mvc/)
- [Fóra ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>Upgrade projektu ASP.NET MVC 1,0 do architektury ASP.NET MVC 2

ASP.NET MVC 2 lze nainstalovat na stejný server, který poskytuje flexibilitu vývojáři aplikace při volbě, když chcete upgradovat aplikaci ASP.NET MVC 1,0 ASP.NET MVC 2 node souběžně s ASP.NET MVC 1,0. Informace o tom, jak upgradovat, naleznete v dokumentu [upgrade aplikace ASP.NET MVC 1,0 ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>Nové funkce

Tato část popisuje funkce, které byly zavedeny v MVC 2 release.

### <a id="_TOC3_1"></a>Objekty se šablonami za

Objekty se šablonami za umožňují automaticky přidružit elementů HTML pro úpravy a zobrazit s datovými typy. Například když data typu System.DateTime se zobrazí v zobrazení, prvku uživatelského rozhraní pro výběr data lze automaticky vykreslit. Toto je podobná jak fungují šablony polí v Dynamická Data technologie ASP.NET. Další informace najdete v tématu [pomocí objekty se šablonami za k datům zobrazení](https://go.microsoft.com/fwlink/?LinkId=159062) na webu MSDN.

### <a id="_TOC3_2"></a>Oblasti

Oblasti umožňují uspořádat velké projektu do více oddílů menší za účelem správy složitost velké webové aplikace. Každá část ("oblast") obvykle představuje samostatné části velké webové stránky a slouží k seskupení souvisejících sady kontrolery a zobrazení. Další informace najdete v tématu [návod: uspořádání aplikaci ASP.NET MVC v oblastech](https://go.microsoft.com/fwlink/?LinkId=158978) na webu MSDN.

Pokud chcete vytvořit novou oblast v Průzkumníku řešení, klikněte pravým tlačítkem na projekt, klikněte na tlačítko Přidat a pak klikněte na tlačítko oblasti. Zobrazí se dialogové okno s výzvou k zadání názvu oblasti. Když zadáte název oblasti, Visual Studio přidá do projektu do nové oblasti.

Následující obrázek znázorňuje příklad rozvržení pro projekt se dvě oblasti, správce a blogů.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Když vytvoříte oblast, Visual Studio přidá třídu odvozenou z AreaRegistration pro každou oblast. Tato třída je požadované pro registraci oblasti a jeho trasy, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

Výchozí šablona projektu pro ASP.NET MVC 2 obsahuje volání metodě RegisterAllAreas v kódu na soubor Global.asax. Tato metoda registruje každou oblast v projektu vyhledávání pro všechny typy, které jsou odvozeny od třídy AreaRegistration, vytváření instancí instance typu a potom voláním metody RegisterArea v instanci. Následující příklad ukazuje, jak to provést.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Pokud nezadáte žádný obor názvů v metodě RegisterArea voláním kontextu. Ve výchozím nastavení se používá metoda Namespaces.Add, obor názvů registrace třídy.

### <a id="_TOC3_3"></a>Podpora pro asynchronní řadiče

ASP.NET MVC 2 teď umožňuje řadiče k asynchronnímu zpracování požadavků. To může vést k větší zvýšení výkonu tím, že servery, které často místo toho volat neblokující svými protějšky volání blokování operací (jako jsou síťové požadavky). Další informace najdete v tématu [pomocí asynchronní kontroler v architektuře ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ee728598(v=VS.100).aspx) tématu na webu MSDN.

### <a id="_TOC3_4"></a>Podpora pro DefaultValueAttribute – v parametrů metody akce

Třída System.ComponentModel.DefaultValueAttribute umožňuje výchozí hodnota je nutné zadat pro parametr argument pro metodu akce. Předpokládejme například, že je definován následující výchozí trasu:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

Také předpokládají, že je definován následující metodu kontroleru a akce:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Některé z následujících požadavek adresy URL se vyvolání metody akce zobrazení, která je definována v předchozím příkladu.

- / Článek nebo zobrazení/123
- / Článek nebo zobrazení/123? stránky = 1 (efektivně stejný jako předchozí požadavek)
- / Článek nebo zobrazení/123? stránky = 2

Bez DefaultValueAttribute – atribut nebude fungovat první adresa URL z předchozího seznamu, protože stránka argument je typu hodnot neumožňující hodnotu Null, jehož hodnota nebyl zadán.

Pokud je váš kód napsaný ve Visual Basic 2010 a program Visual C# 2010, můžete použít volitelné parametry místo DefaultValueAttribute – atribut, jak je znázorněno v následujícím příkladu:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>Podpora pro vazbu s vazače modelů binární Data

Existují dva nové přetížení Html.Hidden pomocné rutiny, které kódování binární hodnoty jako řetězce s kódováním base-64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Typické použití je časové razítko pro objekt vložit do zobrazení. Aplikace může například obsahovat následující objekt produktu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Formuláře upravit může vykreslit vlastnost časového razítka v podobě, jak je znázorněno v následujícím příkladu:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Tento kód vykreslí ještě skrytý element input s hodnotou časového razítka jako řetězec s kódováním base-64 připomínající následující příklad:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Tento formulář může být publikované na metodu akce, který má argument typu produktu, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

V metodě akce je vlastnost časového razítka naplnit správně, protože odeslané kódováním base-64 řetězec je převést na bajtové pole.

### <a id="_TOC3_6"></a>ModelMetadata a ModelMetadataProvider třídy

Třída ModelMetadataProvider poskytuje abstrakci pro získání metadat pro model v rámci zobrazení. MVC 2 zahrnuje výchozího zprostředkovatele, který zpřístupní metadat, který je zveřejněný prostřednictvím atributy v oboru názvů System.ComponentModel.DataAnnotations. Je možné vytvořit zprostředkovatelé metadat, které poskytují metadata z jiným úložištím dat, například databáze nebo soubory XML.

Třída objektu ViewDataDictionary zpřístupňuje objekt ModelMetadata, který obsahuje metadata, která je v třídě ModelMetadataProvider extrahovat z modelu. To umožňuje objekty se šablonami za využívání těchto metadat a odpovídajícím způsobem upravit jejich výstup.

Další informace najdete v dokumentaci pro [ModelMetadata](https://msdn.microsoft.com/en-us/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) a [ModelMetadataProvider](https://msdn.microsoft.com/en-us/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) třídy.

### <a id="_TOC3_7"></a>Podpora pro DataAnnotations atributy

ASP.NET MVC 2 podporuje používání atributů ověření RangeAttribute, RequiredAttribute, StringLengthAttribute a RegexAttribute (definovanou v oboru názvů System.ComponentModel.DataAnnotations) při vázat k modelu k poskytnutí vstupu ověření.

Další informace najdete v tématu [postupy: ověření modelu dat pomocí DataAnnotations atributy](https://go.microsoft.com/fwlink/?LinkId=159063) na webu MSDN. Ukázkový projekt, který znázorňuje použití těchto atributů je k dispozici ke stažení na [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>Zprostředkovatele ověření modelu

Třída zprostředkovatele ověření modelu představuje abstrakci, která poskytuje logiku ověření pro model. ASP.NET MVC zahrnuje výchozí poskytovatel na základě atributů ověření, které jsou zahrnuty v oboru názvů System.ComponentModel.DataAnnotations. Můžete také vytvořit vlastní zprostředkovatele ověřování, které definují vlastní pravidla ověřování a mapování vlastních ověřovacích pravidel do modelu. Další informace najdete v dokumentaci pro [ModelValidatorProvider](https://msdn.microsoft.com/en-us/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) třídy.

### <a id="_TOC3_9"></a>Ověřování na straně klienta

Třída zprostředkovatele ověření modelu zveřejňuje metadata ověření v prohlížeči ve formě serializací JSON data, která mohou být spotřebovávána knihovnu ověřování na straně klienta. ASP.NET MVC 2 zahrnuje klientské knihovny pro ověřování a adaptér, který podporuje atributy ověření obor názvů DataAnnotations si předtím poznamenali. Třída zprostředkovatele také umožňuje používat další knihovny ověřování klienta napsáním adaptér, který zpracovává JSON data a volání do alternativní knihovny.

### <a id="_TOC3_10"></a>Nové fragmenty kódu pro Visual Studio 2010

Pomocí sady Visual Studio 2010 je nainstalována sada fragmenty kódu HTML pro ASP.NET MVC 2. Chcete-li zobrazit seznam tyto fragmenty kódu v nabídce Nástroje, vyberte Správce fragmentů kódu. Pro daný jazyk vyberte HTML a k umístění, vyberte ASP.NET MVC 2. Další informace o tom, jak používat fragmenty kódu najdete v dokumentaci k sadě Visual Studio.

### <a id="_TOC3_11"></a>Nový filtr akce RequireHttpsAttribute

ASP.NET MVC 2 zahrnuje novou třídu RequireHttpsAttribute, který lze použít metody akce a řadičů. Ve výchozím nastavení filtr přesměruje žádost než SSL HTTP na ekvivalentní povolen protokol SSL (HTTPS).

### <a id="_TOC3_12"></a>Přepsání příkaz metoda HTTP

Když vytvoříte webovou stránku pomocí styl architektury REST, příkaz HTTP se používají k určení jakou akci chcete provést pro prostředek. REST vyžaduje, které aplikace podporují celou řadu běžných příkazů HTTP, včetně GET, PUT, POST a odstranění.

ASP.NET MVC 2 zahrnuje nové atributy, které můžete použít pro tuto syntaxi compact funkce a metody akce. Tyto atributy povolte ASP.NET MVC a vyberte metodu akce založené na příkazu HTTP. V následujícím příkladu požadavek POST bude volat první metody akce a požadavek PUT zavolá druhé metody akce.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

V dřívějších verzích rozhraní ASP.NET MVC tyto metody akce vyžaduje více podrobná syntaxe, jak je znázorněno v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Prohlížeče podporují jenom operace GET a POST protokolu HTTP, není možné k vystavování příspěvků na akce, která vyžaduje jinou operaci. Proto není možné tak, aby podporovaly všech RESTful požadavků.

Pro podporu dosáhl standardu RESTful však požadavky během operací POST, ASP.NET MVC 2 představuje nový způsob pomocné rutiny HttpMethodOverride HTML. Tato metoda vykreslí ještě skrytý element input, který způsobí, že formulář emulují efektivně libovolné metody HTTP. Například můžete pomocí metody pomocné rutiny HttpMethodOverride HTML mít odeslání formuláře, zobrazí se požadavek PUT nebo odstranit. Chování HttpMethodOverride ovlivňuje následující atributy:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

Skrytý element input má jeho název X-HTTP-Method-Override a jeho hodnotu nastavte na příkazu HTTP emulovat. Hodnotu přepsání můžete také uvést v hlavičce protokolu HTTP nebo hodnotu řetězce dotazu jako dvojici název/hodnota.

Přepsání lze použít pouze v případě skutečné požadavek má požadavek POST. Pro požadavky, které používají jiné akce HTTP, bude hodnota přepsání ignorována.

### <a id="_TOC3_13"></a>Nová třída HiddenInputAttribute pro objekty se šablonami za

Nový atribut HiddenInputAttribute můžete použít s vlastností modelu k označení, zda má být vykreslen skrytý element input při zobrazení modelu v šabloně editor. (Atribut nastaví má implicitní UIHint hodnotu HiddenInput). Vlastnost atributu položky DisplayValue umožňuje určit, jestli je hodnota zobrazí v editoru a režimy zobrazení. Pokud položky DisplayValue je nastavená na hodnotu false, nezobrazí se žádná hodnota, není i jazyka HTML, který obvykle obklopuje pole. Výchozí hodnota pro položky DisplayValue je true.

Atribut HiddenInputAttribute můžete použít v následujících scénářích:

- Při zobrazení umožňuje uživatelům upravit Identifikátor objektu a je nutné zobrazit hodnotu a aby poskytovala skrytý element input, který obsahuje staré ID tak, aby mohla být předána zpět k řadiči.
- Při zobrazení mohou uživatelé upravovat binární vlastnost, která se nikdy mají zobrazovat, jako je například vlastnost časového razítka. V takovém případě se nezobrazují hodnotu a okolního značky HTML (například popis a hodnotu).

Následující příklad ukazuje způsob použití třídy HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Pokud je atribut nastaven na hodnotu true (nebo je zadaný žádný parametr), dojde k následujícímu:

- V zobrazení šablony je vykreslen štítek a hodnota se zobrazí uživateli.
- V editoru šablony je vykreslen štítek a hodnota je vykreslen ve skrytém elementu input.

Pokud je atribut nastaven na hodnotu false, dojde k následujícímu:

- V zobrazení šablony nic je vykreslen pro toto pole.
- V editoru šablony je vykreslen žádný štítek a hodnota je vykreslen ve skrytém elementu input.

### <a id="_TOC3_14"></a>Metoda Html.ValidationSummary Helper můžete zobrazit chyby na úrovni modelu

Místo zobrazení vždy všechny chyby ověřování, obsahuje pomocnou metodu Html.ValidationSummary nové možnosti se zobrazí pouze chyby na úrovni modelu. To umožňuje chyby na úrovni modelu, který se má zobrazit v souhrnu ověření a pole konkrétních chyb, který se má zobrazit vedle každého pole.

### <a id="_TOC3_15"></a>Šablony T4 v Visual Studio generování kódu, který se liší podle cílové verzi rozhraní .NET Framework

Novou vlastnost je k dispozici pro soubory T4 v ASP.NET MVC T4 hostitele, který určuje verzi rozhraní .NET Framework, který se používá v aplikaci. To umožňuje šablon T4 pro generování kódu a značky, které jsou specifické pro verzi rozhraní .NET Framework. V sadě Visual Studio 2008 hodnota je vždy rozhraní .NET 3.5. V sadě Visual Studio 2010 hodnota je rozhraní .NET 3.5 nebo .NET 4.

## <a id="_TOC4"></a>Vylepšení rozhraní API

Tato část popisuje změny existující ASP.NET MVC typy a členy.

- Přidaná chráněné virtuální CreateActionInvoker metodu do třídy Kontroleru. Tato metoda je volána vlastností ActionInvoker řadiče a umožňuje opožděné konkretizaci původce Pokud žádné původce volání je již nastaven.
- Přidat chráněné virtuální HandleUnauthorizedRequest metody ve třídě třídy AuthorizeAttribute. To umožňuje filtry, které jsou odvozeny od třídy AuthorizeAttribute, které řídí chování, když se nezdaří autorizace.
- Ve třídě ValueProviderDictionary přidat metodu Add (řetězce klíčů, hodnota objektu). To umožňuje pomocí syntaxe inicializátoru slovník pro ValueProviderDictionary, jako v následujícím příkladu:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Přidat get\_objektu metodu v třídě Sys.Mvc.AjaxContext. Toto je metoda JavaScript, která je podobná get\_metoda data, ale pokud typ obsahu odpovědi je application/json, získat\_objekt vrací objekt JSON.
- Přidat ActionDescriptor vlastnost ve třídě kontext ověřování.
- Přidat UrlParameter.Optional token, který lze vyřešit problémy při vytváření vazby pro model, který obsahoval vlastnost ID, když je vlastnost chybí v příspěvku na formuláři. Další podrobnosti naleznete v příspěvku [ASP.NET MVC 2 volitelné parametry adresy URL](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) na blogu Phila Haacka.

## <a id="_TOC5"></a>Nejnovější změny

Následující změny může způsobit chyby v existujících aplikací ASP.NET MVC 1,0.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Změna v chování při ověřování pro třídy, které implementují idataerrorinfo – vlastnost

Pro objekty modelu, které používají idataerrorinfo – provést ověření je ověřen všech vlastností, bez ohledu na to, zda byla nastavena novou hodnotu. V technologii ASP.NET MVC 1.0 byly ověřeny pouze vlastnosti, které měl nastavte nové hodnoty. V ASP.NET MVC 2 Chyba vlastnost idataerrorinfo – nazývá jenom v případě, že všechny validátory vlastnost byla úspěšná.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Skript mapování skriptů služby IIS již není k dispozici v instalačním programu

Skript mapování skriptů služby IIS je skript příkazového řádku, který slouží ke konfiguraci mapy skriptů pro služby IIS 6 a IIS 7 v klasickém režimu. Skript mapování skriptu není potřeba, pokud používáte vývojový Server sady Visual Studio, nebo pokud používáte službu IIS 7 v integrovaném režimu. Skripty, které jsou k dispozici jako samostatné nepodporované stáhnout na [skvělá webová sada ASP.NET](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>Pomocná metoda Html.Substitute v MVC Futures již není k dispozici

Z důvodu změn v chování vykreslování modulů zobrazení MVC pomocnou metodu Html.Substitute nefunguje a byla odebrána.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>Rozhraní IValueProvider nahradí všechny používá IDictionary

Každé vlastnosti nebo metody argument, který teď přijata IDictionary v MVC 1,0 přijme IValueProvider. Tato změna ovlivní pouze aplikace, které zahrnují vlastní hodnotu zprostředkovatelé nebo vazače modelů vlastní. Mezi vlastností a metod, které jsou ovlivněné tuto změnu, patří:

- Vlastnost ValueProvider ControllerBase a ModelBindingContext tříd.
- Metody TryUpdateModel třídy Controller.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>V souboru Site.css byly přidány nové třídy CSS

Zahrnout nové styly použít funkci ověřování a objekty se šablonami za byla aktualizována souboru Site.css v šablon projektu ASP.NET MVC.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Pomocné rutiny se teď vrátit objekt MvcHtmlString

Aby bylo možné využívat nové syntaxe výrazu kódování HTML v technologii ASP.NET 4, návratový typ pro pomocné objekty HTML je nyní MvcHtmlString místo řetězec. Pokud používáte ASP.NET MVC 2 a nové pomocníky na technologii ASP.NET 3.5, nebudete moct využít kódování HTML syntaxe; nové syntaxe je k dispozici jenom v případě, že spustíte ASP.NET MVC 2 na technologii ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult nyní odpovídá pouze na požadavky HTTP POST

S cílem zmírnit JSON útokům, které by mohly pro přístup k informacím, ve výchozím nastavení, třída JsonResult nyní reagovat jenom na požadavky HTTP POST. ZÍSKAT AJAX volání do metody akce, které vracejí objekt JsonResult by mělo být změněno místo toho použít POST. V případě potřeby můžete přepsat toto chování nastavením novou vlastnost JsonRequestBehavior JsonResult. Další informace o potenciální zneužití, naleznete v příspěvku blogu [JSON napadení](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) na blogu Phila Haacka.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Model a nastavením vlastností ModelType na ModelBindingContext jsou zastaralé

Nové nastavit vlastnost ModelMetadata byla přidána k třídě ModelBindingContext. Nové vlastnosti zapouzdří ModelType vlastností a modelu. I když jsou vlastnosti modelu a ModelType zastaralé, z důvodu zpětné kompatibility mechanismy získání vlastnost i nadále fungovat; budou delegovat na vlastnost ModelMetadata k načtení hodnoty.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Objekty Factory vlastní zařízení, které jsou odvozeny od jeho rozdělit změny DefaultControllerFactory – třída

Třída DefaultControllerFactory byl opraven odebrat vlastnost kontext požadavku. Místo tuto vlastnost instance kontextu požadavku předaný chráněné virtuální GetControllerInstance a GetControllerType metody. Tato změna ovlivňuje objekty Factory vlastní zařízení, které jsou odvozeny od DefaultControllerFactory.

Vlastní objekty Factory se často používají k aplikacím poskytnout vkládání závislostí pro architekturu ASP.NET MVC. Aktualizace vlastní objekty Factory pro podporu ASP.NET MVC 2, změňte podpis metody nebo podpisy tak, aby odpovídala nové podpisů a použijte parametr kontextu požadavku místo vlastnost.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Plocha" je teď klíč vyhrazené hodnota trasy

Řetězec "oblasti" v hodnoty trasy teď má zvláštní význam v architektuře ASP.NET MVC stejným způsobem, který "controller" a "action". Jeden vyplývá je, že pokud se hodnota trasy slovník obsahující "plocha" jsou zadány pomocné rutiny HTML, pomocníky už připojí "plocha" v řetězci dotazu.

Pokud používáte funkci oblastí, ujistěte se, zda je nepoužívat {oblasti} v rámci adresy URL trasy.


## <a id="_TOC6"></a>Právní omezení

Toto je předběžná dokument a mohou být změněny podstatně uvedením produktu softwaru popsaných v tomto dokumentu.

Informace obsažené v tomto dokumentu představují aktuální pohled společnosti Microsoft Corporation na uvedené problémy k datu publikování. Protože společnost Microsoft musí reagovat na měnící se podmínky na trhu, by neměl být interpretovat jako závazek společnosti Microsoft a společnost Microsoft nemůže zaručit přesnost informací jsou prezentovány po provedení datu publikování.

Tento dokument White Paper je pouze informativní charakter. MICROSOFT NEPOSKYTUJE ŽÁDNÉ ZÁRUKY, VÝSLOVNÉ, PŘEDPOKLÁDANÉ NEBO STATUTÁRNÍ INFORMACE V TOMTO DOKUMENTU.

Dodržovat všechny platné zákony o autorských právech zodpovídá uživatele. Bez omezení autorských práv, tento dokument může být opakuje, uložené v systému načtení nebo přenést jakýmkoli způsobem (ať už elektronicky, mechanických, včetně kopírování nebo) nebo pro jakýkoli účel bez předchozího písemného svolení společnosti Microsoft Corporation.

Společnost Microsoft může být patentů, patentová aplikací, ochranné známky, autorská práva nebo jiná duševní vlastnictví předmět uvedený v tomto dokumentu. Výslovně uvedeno v licenční smlouvě společnost Microsoft neposkytuje tento dokument žádnou licenci na tyto patenty, ochranné známky, autorská práva nebo jiné duševní vlastnictví.

Pokud není uvedeno jinak, příklady společností, organizací, produktů, názvů domén, e-mailové adresy, loga, osob, míst a událostí použité v ukázkách jsou smyšlené a spojení se žádné skutečnou společností, organizace, produktu, název domény, e-mailu adresu, logem, osoba, místní nebo událostí je určený nebo událostmi.

© 2010 Microsoft Corporation. Všechna práva vyhrazena.

Microsoft a Windows jsou registrované ochranné známky nebo ochranné známky společnosti Microsoft Corporation ve Spojených státech a dalších zemích.

Názvy společností a produktů může být ochranné známky příslušných vlastníků.
