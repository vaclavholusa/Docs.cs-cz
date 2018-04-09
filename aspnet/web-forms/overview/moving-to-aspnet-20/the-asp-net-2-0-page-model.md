---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model 2.0 stránky ASP.NET | Microsoft Docs
author: microsoft
description: V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu. Kódu může být implementovaná pomocí buď Line Src...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: fda85ec03f845cafa7720382bf85652937932c44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="the-aspnet-20-page-model"></a>Model 2.0 stránky ASP.NET
====================
podle [Microsoft](https://github.com/microsoft)

> V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu. Kódu může být implementovaná pomocí atribut Src nebo atribut CodeBehind @Page – direktiva. V aplikaci ASP.NET 2.0 vývojáři stále mají volba mezi vloženého kódu a kódu, ale byla významná vylepšení modelu kódu na pozadí.


V technologii ASP.NET 1.x, vývojáři měli volba mezi model vloženého kódu a model kódu kódu. Kódu může být implementovaná pomocí atribut Src nebo atribut CodeBehind @Page – direktiva. V aplikaci ASP.NET 2.0 vývojáři stále mají volba mezi vloženého kódu a kódu, ale byla významná vylepšení modelu kódu na pozadí.

## <a name="improvements-in-the-code-behind-model"></a>Vylepšení v modelu kódu

Chcete-li plně pochopit změny v modelu kódu v aplikaci ASP.NET 2.0, je nejlepší rychle zkontrolovat modelu jak existovalo v ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Model kódu technologie ASP.NET 1.x

V technologii ASP.NET 1.x, modelu kódu na pozadí se skládal z soubor ASPX (webového formuláře) a souboru kódu, který obsahuje programového kódu. Dva soubory byly připojené pomocí @Page direktivy v souboru ASPX. Každý ovládací prvek na stránce ASPX měl deklaraci odpovídající v souboru kódu na pozadí jako proměnnou instance. Soubor kódu také obsahuje kód pro vazbu událostí a generovaného kódu, které jsou nezbytné pro návrháře Visual Studio. Tento model poměrně osvědčil, ale protože každý ASP.NET element na stránce ASPX vyžaduje odpovídající kód v souboru kódu na pozadí, byl žádné true oddělení kódu a obsahu. Například pokud návrháře přidali nový ovládací prvek serveru do souboru ASPX mimo prostředí Visual Studio IDE, aplikace by došlo k přerušení kvůli absenci deklaraci pro ovládací prvek v souboru kódu na pozadí.

## <a name="the-code-behind-model-in-aspnet-20"></a>Model kódu v technologii ASP.NET 2.0

ASP.NET 2.0 se výrazně zlepšuje při tomto modelu. V aplikaci ASP.NET 2.0, je kódu implementovaná pomocí nové *částečné třídy* uvedené v technologii ASP.NET 2.0. Třída kódu v aplikaci ASP.NET 2.0 je definován jako částečné třídy, což znamená, že obsahuje pouze část definice třídy. Zbývající část definice třídy dynamicky vygeneruje technologii ASP.NET 2.0 pomocí stránky ASPX za běhu, nebo když je předkompilovaných webu. Propojení mezi souboru kódu na pozadí a stránky ASPX bude vytvořeno pomocí direktivy @ Page. Místo atribut CodeBehind nebo Src, je však technologii ASP.NET 2.0 teď používá atribut CodeFile. Atribut Inherits slouží také k určení název třídy pro stránku.

Typické @ Page – direktiva může vypadat například takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Typické – třída definice v souboru kódu ASP.NET 2.0 může vypadat například takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# a Visual Basic jsou pouze spravované jazyky, které aktuálně podporují částečné třídy. Vývojáře, kteří používají J# proto nebudou moci používat model kódu v aplikaci ASP.NET 2.0.


Nový model vylepšuje model kódu, protože vývojáři teď bude mít soubory kódu, které obsahují pouze kód, který jste vytvořili jejich. Umožňuje také true oddělit kódu a obsahu protože nejsou k dispozici žádné instance deklarace proměnných v souboru kódu na pozadí.

> [!NOTE]
> Protože třídu pro stránky ASPX je, kde probíhá událost vazby, vývojáře jazyka Visual Basic můžou realizovat zvýšit výkon mírnou pomocí zpracovává klíčové slovo v kódu pro vazbu události. C# nemá žádné ekvivalentní – klíčové slovo.


## <a name="new--page-directive-attributes"></a>Nové atributy @ Page – direktiva

ASP.NET 2.0 přidá nové atributy mnoho @ Page – direktiva. Následující atributy jsou v technologii ASP.NET 2.0 nové.

## <a name="async"></a>Async

Atribut asynchronní umožňuje nakonfigurovat stránky se spustí asynchronně. Dobře titulní asynchronní stránky později v tomto modulu.

## <a name="asynctimeout"></a>AsyncTimeout

Zadaný časový limit pro asynchronní stránky. Výchozí hodnota je 45 sekund.

## <a name="codefile"></a>CodeFile

Atribut CodeFile je náhradou atribut CodeBehind v Visual Studio 2002 nebo 2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Atribut CodeFileBaseClass se používá v případech, kde je potřeba více stránek k odvozování z jedné základní třídy. Z důvodu provedení částečné třídy v technologii ASP.NET, aniž by tento atribut základní třídu, která používá sdílené běžné pole tak, aby odkazovaly ovládací prvky deklarované v stránky ASPX nebude správně fungovat protože ASP. Modul kompilace sítěmi automaticky vytvoří nové členy na základě na ovládací prvky na stránce. Proto, pokud chcete obecná základní třída pro dva nebo více stránek technologie ASP.NET, je nutné definovat zadejte základní třídu v atributu CodeFileBaseClass a pak každá třída stránky odvozena od základní třídy. Atribut CodeFile je také nutný, pokud tento atribut slouží.

## <a name="compilationmode"></a>compilationMode

Tento atribut umožňuje nastavit vlastnost CompilationMode stránky ASPX. Vlastnost CompilationMode je výčet obsahující hodnoty **vždy**, **automaticky**, a **nikdy**. Výchozí hodnota je **vždy**. **Automaticky** nastavení zabrání ASP.NET Dynamická kompilace stránky, pokud je to možné. Vyloučení stránky z dynamická kompilace zvyšuje výkon. Ale pokud na stránce, která je vyloučená obsahuje tento kód, který musí být zkompilovány, chybu bude vyvolána při zobrazení stránky v prohlížeči.

## <a name="enableeventvalidation"></a>EnableEventValidation

Tento atribut určuje, zda se ověřují zpětné volání a zpětného volání události. Pokud je tato možnost povolena, argumenty odeslání nebo zpětného volání události se kontroluje, ujistěte se, že pochází z ovládacího prvku serveru, který původně je vykreslen.

## <a name="enabletheming"></a>EnableTheming

Tento atribut určuje, zda jsou na stránce používá motivy ASP.NET. Výchozí hodnota je **false**. ASP.NET motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Tento atribut určuje, zda direktivy řádku by měl být přidán během kompilace. Direktivy řádku jsou možnosti používané ladicí programy označit určité části kódu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Tento atribut určuje, zda je JavaScript vloženy do stránce kvůli zachování pozici posunutí mezi postback. Tento atribut je **false** ve výchozím nastavení.

Když tento atribut je **true**, přidá ASP.NET &lt;skriptu&gt; bloku na zpětné volání, které vypadá takto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Všimněte si, že blokovat src pro tento skript je WebResource.axd. Tento prostředek není na fyzickou cestu. Pokud se požaduje tento skript, ASP.NET dynamicky vytvoří skript.

### <a name="masterpagefile"></a>MasterPageFile

Tento atribut určuje soubor předlohové stránky pro aktuální stránku. Cesta může být relativní nebo absolutní. Hlavní stránky jsou popsané v [modulu 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Tento atribut umožňuje potlačit vlastnosti vzhled uživatelského rozhraní definované motiv technologie ASP.NET 2.0. Motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Motiv

Určuje motivu pro stránku. Pokud není zadaná hodnota pro atribut StyleSheetTheme atributu motivu přepíše všechny styly aplikovat na ovládací prvky na stránce.

## <a name="title"></a>Název

Nastaví název webové stránky. Hodnota zadaná v tomto poli se zobrazí v &lt;název&gt; element vykreslené stránky.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Nastaví hodnotu pro ViewStateEncryptionMode výčtu. Dostupné hodnoty jsou **vždy**, **automaticky**, a **nikdy**. Výchozí hodnota je **automaticky**. Pokud je tento atribut nastavíte na hodnotu **automaticky**, se šifrují viewstate je ovládací prvek požaduje voláním **RegisterRequiresViewStateEncryption** metoda.

## <a name="setting-public-property-values-via-the--page-directive"></a>Nastavení hodnot veřejná vlastnost prostřednictvím @ Page – direktiva

Další nová funkce @ Page – direktiva v technologii ASP.NET 2.0 je možnost nastavit počáteční hodnotu veřejné vlastnosti základní třídy. Předpokládejme například, že máte veřejná vlastnost názvem **SomeText** ve základní třídy a d jako ho k inicializována tak, aby **Hello** při načtení stránky. Můžete to provést pomocí jednoduše nastavením hodnoty v direktivě @ Page takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** atribut @ Page – direktiva nastaví počáteční hodnotu vlastnosti SomeText v základní třídě a *Hello!*. Níže je návod, nastavení počáteční hodnota veřejné vlastnosti v základní třídě using – direktiva @ stránky.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Open Full-Screen Video](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nové veřejné vlastnosti třídy stránky

Následující veřejné vlastnosti jsou v technologii ASP.NET 2.0 nové.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Vrací aplikace relativní cestu k stránka nebo ovládací prvek. Například pro stránku se nachází na http://app/folder/page.aspx, vrátí vlastnost ~ / složky /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Vrátí cesta relativní virtuálního adresáře na stránce nebo ovládací prvek. Například pro stránku nacházející se v http://app/folder/page.aspx, vrátí vlastnost ~ / folder/page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Získá nebo nastaví časový limit použitý pro zpracování asynchronní stránky. (Asynchronní stránky bude zmínka v dalších částech tohoto modulu.)

## <a name="clientquerystring"></a>ClientQueryString

Vlastnosti jen pro čtení, která vrátí část řetězce dotazu požadovanou adresu URL. Tato hodnota je kódovaná adresou URL. Metoda UrlDecode třídy HttpServerUtility můžete dekódovat.

## <a name="clientscript"></a>ClientScript

Tato vlastnost vrátí ClientScriptManager objekt, který můžete použít ke správě ASP.NETs emisí klientský skript. (Třída ClientScriptManager je zmínka v dalších částech tohoto modulu.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Tato vlastnost určuje, zda je povoleno ověření události pro události se zpětné volání a zpětného volání. Když je povolené, je zajistit, že pochází z ovládacího prvku serveru, který původně je vykreslen ověřeno argumentů odeslání nebo události zpětného volání.

## <a name="enabletheming"></a>EnableTheming

Tato vlastnost získá nebo nastaví logická hodnota, která určuje, zda motiv technologie ASP.NET 2.0 se vztahuje na stránku.

## <a name="form"></a>Formulář

Tato vlastnost vrátí formuláře HTML na stránce ASPX jako objekt HtmlForm.

## <a name="header"></a>Záhlaví

Tato vlastnost vrátí odkaz na objekt HtmlHead, který obsahuje záhlaví stránky. Vrácený objekt HtmlHead můžete použít k získání nebo nastavení stylů, značky Meta atd.

## <a name="idseparator"></a>IdSeparator

Tato vlastnost jen pro čtení získá znak, který slouží k oddělení řízení identifikátory při ASP.NET je sestavování jedinečné ID pro ovládací prvky na stránce. Rozhraní není určeno pro použití přímo z vašeho kódu.

## <a name="isasync"></a>IsAsync

Tato vlastnost umožňuje pro asynchronní stránky. Asynchronní stránky jsou popsány dále v tomto modulu.

## <a name="iscallback"></a>IsCallback

Tato vlastnost jen pro čtení vrací **true** Pokud stránky je výsledkem volání zpět. Zpětnými voláními jsou popsány dále v tomto modulu.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Tato vlastnost jen pro čtení vrací **true** Pokud stránky je součástí zpětné volání mezi stránkami. Mezi stránkami postback jsou popsané dále v tomto modulu.

## <a name="items"></a>Položky

Vrátí odkaz na instanci IDictionary, který obsahuje všechny objekty, které jsou uložené v kontextu stránky. Položky můžete přidat na tento objekt IDictionary a bude k dispozici po celou dobu kontextu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Tato vlastnost určuje, zda technologie ASP.NET vysílá JavaScript, která zajišťuje, že na stránkách posuňte pozici v prohlížeči se potom, co dojde zpětné volání. (Podrobnosti o této vlastnosti byly popsané dříve v tomto modulu.)

## <a name="master"></a>hlavní server

Tato vlastnost jen pro čtení vrátí odkaz na instanci MasterPage pro stránku, do které byl použit stránky předlohy.

## <a name="masterpagefile"></a>MasterPageFile

Získá nebo nastaví název souboru stránky předlohy pro stránku. Tuto vlastnost lze nastavit pouze v metodě PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Tato vlastnost získá nebo nastaví maximální délku stav stránky v bajtech. Pokud je vlastnost nastavena na kladné číslo, stav zobrazení stránky bude rozdělit do několika skrytá pole nemá být vyšší než počet bajtů určený. Pokud je vlastnost nastavena na záporné číslo, nebude se stav zobrazení rozdělen do bloků.

## <a name="pageadapter"></a>PageAdapter

Vrátí odkaz na objekt PageAdapter, která upraví na stránce pro prohlížeč.

## <a name="previouspage"></a>PreviousPage

Vrátí odkaz na předchozí stránku v případě Server.Transfer nebo zpětné volání mezi stránkami.

## <a name="skinid"></a>SkinID

Určuje vzhledu technologii ASP.NET 2.0 pro použití na stránku.

## <a name="stylesheettheme"></a>StyleSheetTheme

Tato vlastnost získá nebo nastaví šablony stylů, který se použije na stránku.

## <a name="templatecontrol"></a>TemplateControl

Vrátí odkaz na nadřazeného ovládacího prvku pro stránku.

## <a name="theme"></a>Motiv

Získá nebo nastaví název motivu technologii ASP.NET 2.0, použité pro stránku. Tato hodnota musí být nastavena dříve než metoda PreInit.

## <a name="title"></a>Název

Tato vlastnost získá nebo nastaví název pro stránku získaný záhlaví stránky.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Získá nebo nastaví ViewStateEncryptionMode stránky. Najdete podrobnou diskuzi o tuto vlastnost dříve v tomto modulu.

## <a name="new-protected-properties-of-the-page-class"></a>Nové vlastnosti protokolu Protected třídy stránky

Níže jsou nové chráněné vlastnosti třídy stránky v aplikaci ASP.NET 2.0.

## <a name="adapter"></a>Adaptér

Vrátí odkaz na ControlAdapter vykreslující danou stránku na zařízení, která je požadována.

## <a name="asyncmode"></a>AsyncMode

Tato vlastnost určuje, zda asynchronně zpracování stránky. Je určený pro použití modulem runtime a ne přímo v kódu.

## <a name="clientidseparator"></a>ClientIDSeparator

Tato vlastnost vrátí znak používá jako oddělovač při vytváření klienta jedinečné ID pro ovládací prvky. Je určený pro použití modulem runtime a ne přímo v kódu.

## <a name="pagestatepersister"></a>PageStatePersister

Tato vlastnost vrací objekt PageStatePersister pro stránku. Tato vlastnost slouží především vývojáři ovládací prvek ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Tato vlastnost vrátí jedinečný suffic, který se připojí k cesta k souboru pro ukládání do mezipaměti prohlížeče. Výchozí hodnota je \_ \_ufps = a 6 číslice.

## <a name="new-public-methods-for-the-page-class"></a>Nové veřejné metody pro třídu stránky

Následující veřejné metody jsou nové třídy stránky v aplikaci ASP.NET 2.0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Tato metoda zaregistruje delegátů obslužných rutin událostí pro provádění asynchronní stránky. Asynchronní stránky jsou popsány dále v tomto modulu.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Vlastnosti v šabloně stylů stránky se vztahuje na stránce.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Tato metoda lidé asynchronní úlohu.

### <a name="getvalidators"></a>GetValidators

Vrátí kolekci validátory pro ověření zadané skupiny nebo výchozí ověřování, pokud není zadaný žádný.

## <a name="registerasynctask"></a>RegisterAsyncTask

Tato metoda zaregistruje nový asynchronní úlohy. Asynchronní stránky jsou popsané dále v tomto modulu.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Tato metoda informuje technologii ASP.NET, musí být trvalý stav řízení stránek.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Tato metoda informuje ASP.NET, že stav zobrazení stránky vyžaduje šifrování.

## <a name="resolveclienturl"></a>ResolveClientUrl

Vrátí relativní adresu URL, která lze použít pro požadavky klientů pro obrázky, atd.

## <a name="setfocus"></a>Akce SetFocus

Tato metoda nastaví fokus na ovládací prvek, který je zadán při počátečním načtení stránky.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Tato metoda zruší registraci ovládací prvek, který je předán jako už vyžadující trvalost stavu ovládacího prvku.

## <a name="changes-to-the-page-lifecycle"></a>Změny v životním cyklu stránky

Životní cyklus stránky v aplikaci ASP.NET 2.0 nezměnil výrazně, ale existují některé nové metody, které byste měli vědět. Níže jsou uvedeny informace o životním cyklu stránky ASP.NET 2.0.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (nového v technologii ASP.NET 2.0)

Událost PreInit je do nejdřívější fáze životního cyklu, kterým může přistupovat vývojář. Přidání této události je umožněno prostřednictvím kódu programu změnit technologii ASP.NET 2.0 motivy, hlavní stránky, přístup k vlastnostem pro technologii ASP.NET 2.0 profilu atd. Pokud jste v postback stavu, jeho důležité si uvědomit, že Viewstate nebyla dosud nebyly použity k ovládacím prvkům v tomto okamžiku v životním cyklu. Proto pokud vývojář změní vlastnost ovládacího prvku v této fázi, bude pravděpodobně přepsán později v průběhu životního cyklu stránky.

## <a name="init"></a>Init

Události Init nebylo změněno z ASP.NET 1.x. Toto je místo by pro čtení nebo inicializace vlastností ovládacích prvků na stránku. V této fázi, hlavní stránky, motivy jsou použité pro stránku již atd.

## <a name="initcomplete-new-in-20"></a>InitComplete (nové ve verzi 2.0)

Na konci fáze inicializace stránky se nazývá InitComplete události. V tomto okamžiku v průběhu životního cyklu, dostanete ovládací prvky na stránce, ale jejich stavu nebyl ještě vyplněna.

## <a name="preload-new-in-20"></a>Předběžné načtení (nové ve verzi 2.0)

Tato událost je volána po odeslání všech dat a těsně před stránky\_zatížení.

## <a name="load"></a>Zatížení

Událost zatížení nebylo změněno z ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nové ve verzi 2.0)

Událost LoadComplete je poslední události ve fázi zatížení stránky. V této fázi se všechna data zpětné volání a stavu zobrazení použilo na stránku.

## <a name="prerender"></a>PreRender

Pokud chcete pro stav zobrazení pro ovládací prvky, které jsou přidány na stránku dynamicky správně udržovat, událost PreRender je poslední možnost k je přidat.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nové ve verzi 2.0)

Ve fázi PreRenderComplete byly přidány všechny ovládací prvky na stránku a je připravena k vykreslení stránky. Událost PreRenderComplete je poslední událost vyvolána před uložením stavu zobrazení stránky.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nové ve verzi 2.0)

Událost SaveStateComplete je volána hned po byl uložen stav viewstate a řízení všechny stránky. Toto je poslední událost před ve skutečnosti vykreslení stránky v prohlížeči.

## <a name="render"></a>Vykreslení

Metoda vykreslení nezměnil od ASP.NET 1.x. Toto je kde je inicializován HtmlTextWriter a vykreslení stránky v prohlížeči.

## <a name="cross-page-postback-in-aspnet-20"></a>Zpětné volání mezi stránkami v technologii ASP.NET 2.0

V technologii ASP.NET 1.x, postback musely k vystavování příspěvků na stejné stránce. Mezi stránkami postback nebyly povoleny. ASP.NET 2.0 přidává možnost odeslat zpět na jinou stránku prostřednictvím rozhraní IButtonControl. Libovolný ovládací prvek, který implementuje nové rozhraní IButtonControl (tlačítko, LinkButton a ImageButton kromě vlastní ovládací prvky třetích stran) můžete využít výhod tyto nové funkce prostřednictvím použití atributu PostBackUrl. Následující kód ukazuje ovládacího prvku tlačítka, který odesílá zpět na druhé stránce.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Když je stránka vrácena zpět, je přístupná prostřednictvím Vlastnost PreviousPage na druhé stránce stránka, která zahájí zpětné volání. Tato funkce se implementuje prostřednictvím nového webového formuláře\_DoPostBackWithOptions klientské funkce, která vykreslí technologii ASP.NET 2.0 na stránku při ovládacího prvku odešle zpět na jinou stránku. Tato funkce jazyka JavaScript je poskytovaný nové WebResource.axd obslužná rutina, která vydá skriptu do klienta.

Níže je návod, zpětné volání mezi stránkami.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Open Full-Screen Video](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Další informace o postback mezi stránkami

### <a name="viewstate"></a>Stav zobrazení

Můžete být vyzváni, sami již o co se stane vlastnost viewstate od první stránky ve scénáři mezi stránkami zpětného volání. Po všech libovolný ovládací prvek, který neimplementuje rozhraní IPostBackDataHandler zachová stav prostřednictvím viewstate, takže tak, aby měl přístup k vlastnostem tohoto ovládacího prvku na druhé stránce zpětné volání mezi stránkami, musí mít přístup k vlastnost viewstate pro stránku. Tento scénář pomocí nové skrytá pole na druhé stránce názvem postará technologie ASP.NET 2.0 \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE pole formuláře obsahuje viewstate na první stránce tak, že máte přístup k vlastnosti všech ovládacích prvků na druhé stránce.

### <a name="circumventing-findcontrol"></a>Obcházení FindControl

V video s návodem zpětné volání mezi stránkami I metodu FindControl použít k získání odkazu do ovládacího prvku textového pole na první stránce. Tato metoda funguje dobře k tomuto účelu, ale FindControl je nákladné a vyžaduje zápis další kód. Naštěstí technologii ASP.NET 2.0 představuje alternativu ke FindControl pro tento účel, který bude fungovat v řadě scénářů. Direktiva PreviousPageType umožňuje mít silného typu odkaz na předchozí stránku s použitím TypeName nebo atribut VirtualPath. Atribut TypeName umožňuje určit typ na předchozí stránku, přestože Atribut VirtualPath umožňuje odkazovat na předchozí stránku pomocí virtuální cesty. Jakmile nastavíte PreviousPageType direktiva, musí pak vystavit ovládací prvky, ke kterému chcete povolit přístup pomocí veřejné vlastnosti atd.

## <a name="lab-1-cross-page-postback"></a>Zpětné volání mezi stránkami testovacím 1

V tomto testovacím prostředí vytvoříte aplikaci, která používá nové funkce zpětného volání mezi stránkami ASP.NET 2.0.

1. Otevřete Visual Studio 2005 a vytvořte nový web ASP.NET.
2. Přidáte nový webový formulář, názvem page2.aspx.
3. Otevřete Default.aspx v zobrazení návrhu a přidání ovládacího prvku tlačítko a ovládacího prvku textového pole. 

    1. Zadejte ID ovládacího prvku tlačítko **odeslat** a do textového pole řídit ID **uživatelské jméno**.
    2. Nastavte vlastnost PostBackUrl tlačítka na page2.aspx.
4. Otevřete page2.aspx v zobrazení zdroje.
5. Přidáte direktivu @ PreviousPageType, jak je uvedeno níže:
6. Přidejte následující kód na stránku\_zatížení na page2.aspx kódu: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Sestavte projekt kliknutím na sestavení v nabídce sestavení.
8. Přidejte následující kód do kódu pro Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Změnit stránce\_zatížení v page2.aspx pro následující: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Sestavte projekt.
11. Spusťte projekt.
12. Zadejte název do textového pole a klikněte na tlačítko.
13. Co je výsledkem?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchronní stránky v technologii ASP.NET 2.0

Mnoho problémů kolizí v technologii ASP.NET jsou způsobeny latence externí volání (jako například volání webové služby nebo databáze), latence vstupně-výstupní operace souboru atd. Po odeslání žádosti na aplikace ASP.NET používá jeden z jeho pracovních vláken ASP.NET ke zpracování tohoto požadavku. Tento požadavek vlastní daném vláknu až po dokončení požadavku a byl odeslán do odpovědi. ASP.NET 2.0 se snaží vyřešit problémy s latencí s těmito typy problémů přidáním možnosti spouštění stránek asynchronně. Která znamená, že pracovní vlákno může spuštění požadavku sady a pak přebírají další provádění na jiné vlákno, a tím vrácením fondu k dispozici vláken rychle. Po dokončení soubor vstupně-výstupní operace, volání databáze, atd., nové vlákno se získávají z fondu podprocesů na dokončení požadavku.

Prvním krokem při vytváření stránky asynchronní je nastavit **asynchronní** direktivy stránky takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Tento atribut informuje technologii ASP.NET pro implementaci IHttpAsyncHandler pro stránku.

Dalším krokem je třeba volat metodu AddOnPreRenderCompleteAsync na místo v životním cyklu stránky před PreRender. (Tato metoda je volána obvykle stránce\_zatížení.) Metoda AddOnPreRenderCompleteAsync přebírá dva parametry; BeginEventHandler a EndEventHandler. BeginEventHandler vrátí IAsyncResult, který se potom předá jako parametr pro EndEventHandler.

Níže je návod, požadavek asynchronní stránky.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Open Full-Screen Video](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Stránku asynchronní nevykresluje do prohlížeče, dokud se nedokončí EndEventHandler. Žádné nejistých ale si bude myslet někteří vývojáři asynchronních požadavků, že je podobná asynchronních zpětných volání. Je důležité si uvědomit, že nejsou. Výhodou na asynchronní požadavky je, že první pracovní vlákno se může vracet do fondu vláken pro zpracování nových požadavků, a tím snižuje kolize, protože nejsou vstupně-výstupní operace vázaný atd.


## <a name="script-callbacks-in-aspnet-20"></a>Zpětná volání skriptu v technologii ASP.NET 2.0

Vývojáři webů vždy vyhledávaly způsoby, jak zabránit blikání přidružené zpětné volání. V technologii ASP.NET 1.x SmartNavigation byl nejběžnější metodou pro vyloučení blikání, ale SmartNavigation způsobeno problémy pro vývojáře v některé z důvodu složitosti jeho implementace na straně klienta. ASP.NET 2.0 řeší tento problém s zpětná volání skriptu. Zpětná volání skriptu využívat XMLHttp provádět požadavky na webový server prostřednictvím jazyka JavaScript. Žádost o XMLHttp vrací data XML, který lze potom manipulovat prostřednictvím prohlížeče DOM Uživatel skrytá XMLHttp kód nové WebResource.axd obslužnou rutinou.

Existuje několik kroků, které jsou nezbytné, aby se konfigurace zpětné volání skriptu v technologii ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Krok 1: Implementace rozhraní ICallbackEventHandler

Aby pro technologii ASP.NET rozpoznat stránku jako účastní zpětné volání skriptu musí implementovat rozhraní ICallbackEventHandler. Můžete to provést v souboru kódu takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Můžete to můžete provést taky pomocí direktivy jako @ implementuje proto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Direktiva @ implementuje by obvykle používají při použití vloženého kódu ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Krok 2: Volání GetCallbackEventReference

Jak je uvedeno nahoře, je v rutině WebResource.axd zapouzdřený XMLHttp volání. Když se zobrazí vaše stránka ASP.NET přidá volání webového formuláře\_DoCallback, klientský skript, který zajišťuje WebResource.axd. Webovém formuláři\_DoCallback funkce nahrazuje \_ \_doPostBack funkce pro zpětné volání. Nezapomeňte, že \_ \_doPostBack prostřednictvím kódu programu odešle formuláře na stránce. Ve scénáři zpětného volání, ale chcete zabránit zpětné volání, takže \_ \_doPostBack nestačí.

> [!NOTE]
> \_\_doPostBack je stále vykreslen na stránku ve scénáři klientský skript zpětného volání. Nicméně se nepoužije pro zpětné volání.


Argumenty pro webovém formuláři\_DoCallback klientské funkce jsou k dispozici prostřednictvím funkce serverové GetCallbackEventReference, která by za normálních okolností volána stránce\_zatížení. Typické volání GetCallbackEventReference může vypadat například takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> V takovém případě cm představuje instanci ClientScriptManager. Třída ClientScriptManager bude zmínka v dalších částech tohoto modulu.


Existuje několik přetížené verzích GetCallbackEventReference. V takovém případě argumenty jsou následující:

`this`

Odkaz na ovládací prvek, kde je volána GetCallbackEventReference. V takovém případě je vlastní stránky.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument řetězce, který bude předán z kódu na straně klienta pro událost na straně serveru. V tomto případě Im předávání hodnotu rozevírací název ddlCompany.

`ShowCompanyName`

Název funkce klienta, která bude přijímat návratovou hodnotu (jako řetězec) z události zpětného volání na straně serveru. Tato funkce bude volat pouze po úspěšné zpětného volání na straně serveru. Proto v zájmu odolnosti, obvykle doporučujeme používat přetížené verzi GetCallbackEventReference, který používá argument další řetězec určující název klienta funkce provést v případě chyby.

`null`

Řetězec představující klientské funkce, která ho inicializovat před zpětného volání k serveru. V takovém případě není žádný skript, tak argument má hodnotu null.

`true`

Logická hodnota určující, zda má být asynchronně chování zpětné volání.

Volání webového formuláře\_DoCallback v klientovi předá tyto argumenty. Proto při této stránky na straně klienta, tento kód bude vypadat například takto:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Všimněte si, že podpis funkce na straně klienta je trochu liší. Funkce klienta předá 5 řetězce a logickou hodnotu. Další řetězec (která je null v předchozím příkladu) obsahuje klientské funkce, která zpracuje všechny chyby ze zpětného volání na straně serveru.

## <a name="step-3--hook-the-client-side-control-event"></a>Krok 3: Připojení událostí prvek na straně klienta

Všimněte si, že návratová hodnota GetCallbackEventReference výše byl přiřazen na proměnnou string. Tento řetězec se používá k připojení na straně klienta událostí pro ovládací prvek, který spouští zpětné volání. V tomto příkladu rozevíracího seznamu na stránce iniciované zpětné volání, chcete připojit *při změně* událostí.

Napojit událost na straně klienta, jednoduše přidejte obslužnou rutinu kód klienta následujícím způsobem:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Odvolat, *cbRef* je vrácená hodnota z volání GetCallbackEventReference. Obsahuje volání webového formuláře\_DoCallback zobrazená výše.

## <a name="step-4--register-the-client-side-script"></a>Krok 4: Registrace klientský skript

Odvolání, který volání GetCallbackEventReference zadali, že klientský skript volá **ShowCompanyName** by byl proveden, pokud úspěšná zpětného volání na straně serveru. Tento skript je nutné přidat na stránku pomocí ClientScriptManager instance. (Třída ClientScriptManager bude dicussed později v tomto modulu.) Proto je provést tento jako:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Krok 5: Volání metody rozhraní ICallbackEventHandler

ICallbackEventHandler obsahuje dvě metody, které je nutné implementovat v kódu. Jsou **RaiseCallbackEvent** a **GetCallbackEvent**.

**RaiseCallbackEvent** přijímá jako argument řetězec a vrátí nic. Argument řetězce předaný z na straně klienta volání webového formuláře\_DoCallback. V takovém případě je tato hodnota *hodnotu* atribut názvem ddlCompany rozevíracího seznamu. Serverový kód musí být umístěny v metodě RaiseCallbackEvent. Například pokud je vaše zpětné volání WebRequest proti externí prostředek, tento kód musí být umístěny v RaiseCallbackEvent.

**GetCallbackEvent** je zodpovědná za zpracování vrácení zpětné volání do klienta. Přebírá bez argumentů a vrátí řetězec. Řetězec, který vrátí budou předány jako argument funkce na straně klienta v tomto případě *ShowCompanyName*.

Po dokončení výše uvedené kroky, jste připraveni k provedení skriptu zpětné volání v technologii ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Open Full-Screen Video](the-asp-net-2-0-page-model/_static/callback1.wmv)


Zpětná volání skriptu v technologii ASP.NET jsou podporovány v žádný prohlížeč, který podporuje vytváření volání XMLHttp. Který zahrnuje všechny moderní prohlížeče ve použití ještě dnes. Internet Explorer používá objekt XMLHttp ActiveX při vnitřní objekt XMLHttp používat další moderní prohlížeče (včetně nadcházející aplikaci Internet Explorer 7). K určení prostřednictvím kódu programu, pokud prohlížeč podporuje zpětná volání, můžete použít **Request.Browser.SupportCallback** vlastnost. Tato vlastnost vrátí **true** Pokud klienta, který podporuje zpětná volání skriptu.

## <a name="working-with-client-script-in-aspnet-20"></a>Práce s klientského skriptu v technologii ASP.NET 2.0

Skripty klienta v technologii ASP.NET 2.0 jsou spravované prostřednictvím použití třídy ClientScriptManager. Třída ClientScriptManager uchovává informace o klientských skriptů pomocí typu a název. To brání stejného skriptu prostřednictvím kódu programu vkládání na stránce více než jednou.

> [!NOTE]
> Po skript byl úspěšně registrován na stránce, bude další pokus o registraci stejného skriptu jednoduše výsledkem skript není registrovaný ještě jednou. Budou přidány žádné duplicitní skripty a dojde k žádná výjimka. Aby se zabránilo zbytečným výpočetní, jsou metody, které můžete použít k určení, zda je již zaregistrován skript tak, aby vám nepokoušejte a zaregistrujte ho více než jednou.


Metody ClientScriptManager by měla být pro všechny aktuální vývojáře využívající technologii ASP.NET:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Tato metoda skript přidá do horní části na vykreslené stránce. To je užitečné pro přidání funkcí, které bude explicitně volána na straně klienta.

Existují dvě přetížené verze této metody. Tři ze čtyř argumentů jsou běžné mezi nimi. Možnosti jsou následující:

`type (string)`

***Typ*** argument identifikuje typ skriptu. Obecně je vhodné použít typ stránky (to. GetType()) typu.

`key (string)`

***Klíč*** argument je klíč uživatelem definované pro skript. To by měl být jedinečný pro každý skript. Pokud se pokusíte přidat skript se stejným klíčem a typ skriptu, který již přidané, nebude přidáno.

`script (string)`

***Skriptu*** argument je řetězec obsahující skutečné skript, který chcete přidat. Doporučujeme použít k vytvoření skriptu a potom použijte metodu ToString() na StringBuilder přiřadit StringBuilder ***skriptu*** argument.

Pokud používáte přetížené RegisterClientScriptBlock, který má jenom tři argumenty, je nutné zahrnout elementů skriptu (&lt;skriptu&gt; a &lt;/script&gt;) ve vašem skriptu.

Můžete použít přetížení RegisterClientScriptBlock, která přebírá čtvrtý argument. Poslední argument je logická hodnota, která určuje, zda technologie ASP.NET měli přidat elementů skriptu pro vás. Pokud tento argument je **true**, váš skript nesmí obsahovat elementy skriptu explicitně.

Použijte metodu IsClientScriptBlockRegistered k určení, pokud skript už je zaregistrovaný. To umožňuje vyhnout pokus znovu registrovat skript, který je již zaregistrován.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nové ve verzi 2.0)

Značky RegisterClientScriptInclude vytvoří blok skriptu, který odkazuje na soubor externího skriptu. Má dva přetížení. Má klíč a adresu URL. Přidá druhý třetí argument určení typu.

Následující kód například vygeneruje blok skriptu odkazující na jsfunctions.js v kořenové složce skripty aplikace:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Tento kód vytvoří následující kód na vykreslené stránce:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Blok skriptu je vykreslen v dolní části stránky.


Použijte metodu IsClientScriptIncludeRegistered k určení, pokud skript už je zaregistrovaný. To umožňuje vyhnout pokus znovu registrovat skript.

## <a name="registerstartupscript"></a>RegisterStartupScript

Metoda RegisterStartupScript přijímá stejné argumenty jako metodu RegisterClientScriptBlock. Skript zaregistrována RegisterStartupScript spustí po načtení stránky, ale před události při načtení straně klienta. V 1.X, skripty, které jsou registrovány RegisterStartupScript byly umístěny těsně před uzavírací &lt;/form&gt; značky při skripty, které jsou registrovány RegisterClientScriptBlock byly umístěny ihned po otevření &lt;formuláře&gt; značky. V aplikaci ASP.NET 2.0, jak jsou umisťovaný bezprostředně před uzavírací &lt;/form&gt; značky.

> [!NOTE]
> Když si zaregistrujete funkci s RegisterStartupScript, nebude této funkce spustit, dokud ji explicitně volání v kódu na straně klienta.


Použijte metodu IsStartupScriptRegistered k určení, pokud již byl zaregistrován skript a vyhnout se pokus znovu registrovat skript.

## <a name="other-clientscriptmanager-methods"></a>Ostatní metody ClientScriptManager

Zde jsou některé další užitečné metody třídy ClientScriptManager.


|  <strong>GetCallbackEventReference</strong>   |                                                 Viz zpětná volání skriptu dříve v tomto modulu.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Získá referenční dokumentace technologie JavaScript (javascript:&lt;volání&gt;), lze použít ke zveřejnění zpět z událostí na straně klienta.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Získá řetězec, který slouží k zahájení post zpět od klienta.                                    |
|      <strong>GetWebResourceUrl</strong>       | Vrátí adresu URL k prostředku, který je součástí sestavení. Musí použít ve spojení s <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registruje prostředek webové stránky. Toto jsou prostředky vložené do sestavení a zpracovávat nové WebResource.axd obslužnou rutinou.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Zaregistruje skryté pole formuláře se stránkou.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Zaregistruje kód na straně klienta, který se spustí po odeslání formuláře HTML.                                   |

