---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Model 2.0 stránky ASP.NET | Dokumentace Microsoftu
author: microsoft
description: V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind. Použití modelu Code-behind může implementovat s využitím buď Src attr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: 9d62aee5e0754b1910b923ad9ae501ebed91097e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379884"
---
<a name="the-aspnet-20-page-model"></a>Model 2.0 stránky ASP.NET
====================
podle [Microsoft](https://github.com/microsoft)

> V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind. Použití modelu Code-behind může být implementovaná pomocí atributu Src nebo atribut CodeBehind @Page směrnice. V technologii ASP.NET 2.0 vývojáři stále mít možnost volby mezi vloženého kódu a použití modelu code-behind, ale došlo k použití modelu code-behind modelu významná vylepšení.


V technologii ASP.NET 1.x, vývojáři měli možnost volby mezi model pomocí vloženého kódu a model kódu použití modelu code-behind. Použití modelu Code-behind může být implementovaná pomocí atributu Src nebo atribut CodeBehind @Page směrnice. V technologii ASP.NET 2.0 vývojáři stále mít možnost volby mezi vloženého kódu a použití modelu code-behind, ale došlo k použití modelu code-behind modelu významná vylepšení.

## <a name="improvements-in-the-code-behind-model"></a>Vylepšení v modelu kódu

Chcete-li plně pochopit změny v modelu kódu v technologii ASP.NET 2.0, je nejlepší rychle prohlédnout modelu jako jeho existoval v technologii ASP.NET 1.x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>Model použití modelu Code-Behind v technologii ASP.NET 1.x

V technologii ASP.NET 1.x, model použití modelu code-behind se skládal z souboru ASPX (webového formuláře) a použití modelu code-behind soubor obsahující programového kódu. Příslušné dva soubory byly připojené pomocí @Page direktivy v souboru ASPX. Každý ovládací prvek na stránce ASPX měl odpovídající deklarace v souboru kódu na pozadí jako proměnná instance. Soubor kódu na pozadí také obsahovala kód pro navázání událostí a generovaného kódu, které jsou nezbytné pro návrháře aplikace Visual Studio. Tento model poměrně dobře fungovalo, ale protože každý prvek technologie ASP.NET na stránce ASPX vyžaduje odpovídající kód v souboru kódu na pozadí, neexistuje žádná true oddělení kódu a obsahu. Například pokud návrháře do souboru ASPX mimo rozhraní IDE sady Visual Studio přidá nový serverový ovládací prvek, aplikace by narušil vzhledem k absenci deklarace pro tento ovládací prvek v souboru kódu na pozadí.

## <a name="the-code-behind-model-in-aspnet-20"></a>Model použití modelu Code-Behind v technologii ASP.NET 2.0

ASP.NET 2.0 výrazně vylepšuje tento model. V technologii ASP.NET 2.0, je implementováno pomocí nového modelu code-behind *částečné třídy* v technologii ASP.NET 2.0 k dispozici. Použití modelu code-behind třída v technologii ASP.NET 2.0 je definován jako částečné třídy, což znamená, že obsahuje pouze část definice třídy. Zbývající část definice třídy generuje dynamicky pomocí technologie ASP.NET 2.0 pomocí stránky ASPX za běhu nebo na webu je předkompilována. Propojení mezi souborem kódu na pozadí a stránku ASPX stále se naváže s využitím – Direktiva @ Page. Místo atribut CodeBehind nebo Src, je však ASP.NET 2.0 nyní používá atribut CodeFile. Atribut Inherits je také použít k určení názvu třídy stránky.

Typické – Direktiva @ Page může vypadat takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Definice typické třídy v souboru kódu ASP.NET 2.0 může vypadat takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C# a Visual Basic jsou pouze spravované jazyky, které aktuálně podporují částečné třídy. Proto vývojáři, kteří používají J# nebude možné použít model použití modelu code-behind v technologii ASP.NET 2.0.


Nový model zvyšuje model použití modelu code-behind, protože vývojáři budou mít soubory kódu, které obsahují pouze kód, který jste vytvořili. Také nabízí true oddělení kódu a obsahu vzhledem k tomu, že neexistují žádné instance deklarace proměnných v souboru kódu na pozadí.

> [!NOTE]
> Částečné třídy pro stránku ASPX je, kde vazby události dojde, vývojáře jazyka Visual Basic můžete realizovat zvýšení snížený výkon pomocí klíčového slova popisovače v modelu code-behind svázat události. C# nemá žádný ekvivalent klíčového slova.


## <a name="new--page-directive-attributes"></a>Nové atributy @ Page – direktiva

ASP.NET 2.0 přidá mnoho nových atributů – Direktiva @ Page. Následující atributy jsou v technologii ASP.NET verze 2.0 nový.

## <a name="async"></a>Async

Atribut Async umožňuje nakonfigurovat stránky, který se spustí asynchronně. Dobře titulní asynchronní stránky později v tomto modulu.

## <a name="asynctimeout"></a>Hodnota vlastnosti AsyncTimeout

Zadaný časový limit pro asynchronní stránky. Výchozí hodnota je 45 sekund.

## <a name="codefile"></a>CodeFile

Atribut CodeFile je náhradou pro atribut CodeBehind v aplikaci Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

Atribut CodeFileBaseClass se používá v případech, kde má více stránky, které jsou odvozeny z jediné základní třídy. Z důvodu implementace částečné třídy v technologii ASP.NET, bez tohoto atributu základní třídu, která používá sdílené společných polí tak, aby odkazovaly ovládací prvky deklarován na stránce ASPX nebude fungovat správně, protože ASP. Modul kompilace sítě automaticky vytvoří nové členy v závislosti na ovládací prvky na stránce. Proto pokud chcete obecná základní třída pro dva nebo více stránek v ASP.NET, budete muset definovat v Atribut CodeFileBaseClass zadat základní třídy a pak jsou odvozeny jednotlivé třídy stránky z této základní třídy. Atribut CodeFile je také nutný, pokud tento atribut se používá.

## <a name="compilationmode"></a>Vlastnost CompilationMode

Tento atribut umožňuje nastavit vlastnost CompilationMode stránky ASPX. Vlastnost CompilationMode je výčet, který obsahuje hodnoty **vždy**, **automaticky**, a **nikdy**. Výchozí hodnota je **vždy**. **Automaticky** nastavení zabrání ASP.NET Dynamická kompilace na stránce, pokud je to možné. Kromě stránek v dynamických kompilačních zvyšuje výkon. Ale stránka, která je vyloučená obsahuje tento kód, který musí být zkompilovány, chybu bude vyvolána výjimka při prohlížení stránky.

## <a name="enableeventvalidation"></a>EnableEventValidation

Tento atribut určuje, zda jsou ověřeny události zpětného odeslání a zpětného volání. Pokud je tato možnost povolena, argumenty odeslání nebo zpětného volání události jsou kontrola, že pocházejí z ovládacího prvku serveru, který je původně vykreslil.

## <a name="enabletheming"></a>EnableTheming

Tento atribut určuje, zda jsou na stránce použít motivů ASP.NET. Výchozí hodnota je **false**. Motivů ASP.NET jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Tento atribut určuje, zda řádkové direktivy pragma by měl být přidány během kompilace. Řádkové direktivy pragma jsou možnosti, které používají ladicí programy k označení určité části kódu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Tento atribut určuje, zda jazyka JavaScript se vloží do stránky pro zachování pozice posuvníku mezi jednotlivými zpětnými odesláními. Tento atribut je **false** ve výchozím nastavení.

Pokud tento atribut je **true**, přidá technologie ASP.NET &lt;skript&gt; blok na zpětné volání, který vypadá takto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Všimněte si, že je src pro tento blok skriptu WebResource.axd. Tento prostředek není fyzická cesta. Pokud tento skript se požaduje, ASP.NET dynamicky vytvoří skript.

### <a name="masterpagefile"></a>MasterPageFile

Tento atribut určuje soubor předlohové stránky pro aktuální stránku. Cesta může být relativní nebo absolutní. Stránky předlohy se věnují [modulu 4](master-pages.md).

## <a name="stylesheettheme"></a>Vlastnost StyleSheetTheme

Tento atribut umožňuje přepsat vlastnosti vzhled uživatelského rozhraní definované motiv ASP.NET 2.0. Motivy jsou popsané v [modulu 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Motiv

Určuje motivu stránky. Pokud není zadána hodnota pro atribut StyleSheetTheme, přepíše atribut motivu všechny styly použít na ovládací prvky na stránce.

## <a name="title"></a>Název

Nastaví název stránky. Hodnota zadaná v tomto poli se zobrazí v &lt;název&gt; prvek vykreslené stránky.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Nastaví hodnotu pro výčet ViewStateEncryptionMode. Dostupné hodnoty jsou **vždy**, **automaticky**, a **nikdy**. Výchozí hodnota je **automaticky**. Když tento atribut je nastaven na hodnotu **automaticky**, stav zobrazení je zašifrovaný je ovládací prvek požaduje voláním **RegisterRequiresViewStateEncryption** metoda.

## <a name="setting-public-property-values-via-the--page-directive"></a>Nastavení hodnot veřejnou vlastnost prostřednictvím @ direktiva stránky

Další nová funkce – Direktiva @ Page v technologii ASP.NET 2.0 je možnost nastavit počáteční hodnotu veřejných vlastností základní třídy. Předpokládejme například, že máte veřejnou vlastnost názvem **SomeText** v základní třídy a d líbí se vám mají být inicializovány na **Hello** při načtení stránky. Toho lze dosáhnout jednoduše nastavením hodnoty v Direktiva @ Page takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

**SomeText** atribut – Direktiva @ Page nastaví počáteční hodnotu vlastnosti SomeText v základní třídě pro *Hello!*. Následující video je návod, nastaví počáteční hodnotu veřejnou vlastnost v základní třídě – Direktiva @ Page.


![](the-asp-net-2-0-page-model/_static/image1.png)


[Otevřít Video na celou obrazovku](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a>Nové veřejné vlastnosti třídy stránky

Následující veřejné vlastnosti jsou v technologii ASP.NET verze 2.0 nový.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Stránka nebo ovládací prvek vrátí aplikace relativní cestu. Například pro stránku se nachází na http://app/folder/page.aspx, vrátí vlastnost ~ / slozka /.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Stránka nebo ovládací prvek vrátí relativní virtuální adresář. Například pro stránku se nachází na http://app/folder/page.aspx, vrátí vlastnost ~ / folder/page.aspx.

## <a name="asynctimeout"></a>Hodnota vlastnosti AsyncTimeout

Získá nebo nastaví časový limit používá pro zpracování asynchronní stránky. (Asynchronní stránky se budeme dále v tomto modulu.)

## <a name="clientquerystring"></a>ClientQueryString

Vlastnosti jen pro čtení, která vrací část řetězce dotazu požadovanou adresu URL. Tato hodnota je kódování URL. Metoda UrlDecode třídy HttpServerUtility můžete dekódovat.

## <a name="clientscript"></a>ClientScript

Tato vlastnost vrátí objekt ClientScriptManager, který slouží ke správě ASP.NETs emise skriptu na straně klienta. (Třída ClientScriptManager je popsané dále v tomto modulu.)

## <a name="enableeventvalidation"></a>EnableEventValidation

Tato vlastnost určuje, jestli ověření události je povoleno pro události zpětného odeslání a zpětným voláním. Pokud povolená, argumenty odeslání nebo zpětného volání události se ověřit, ujistěte se, že pocházejí z ovládacího prvku serveru, který je původně vykreslil.

## <a name="enabletheming"></a>EnableTheming

Tato vlastnost získá nebo nastaví logickou hodnotu, která určuje, zda motiv ASP.NET 2.0 se vztahuje na stránce.

## <a name="form"></a>Formulář

Tato vlastnost vrátí formuláře HTML na stránce ASPX jako objekt HtmlForm.

## <a name="header"></a>Záhlaví

Tato vlastnost vrátí odkaz na objekt HtmlHead, který obsahuje záhlaví stránky. Vrácený objekt HtmlHead slouží k get/set šablony stylů, metaznaček atd.

## <a name="idseparator"></a>IdSeparator

Tato vlastnost jen pro čtení získá znak, který se používá k oddělení identifikátory ovládacího prvku při sestavuje jedinečné ID pro ovládací prvky na stránce ASP.NET. Není určena pro použití přímo v kódu.

## <a name="isasync"></a>Isasync:

Tato vlastnost umožňuje asynchronní stránky. Asynchronní stránky jsou popsány dále v tomto modulu.

## <a name="iscallback"></a>IsCallback

Vrátí tato vlastnost jen pro čtení **true** Pokud na stránce je výsledkem zpětného volání. Zpětnými voláními jsou popsány dále v tomto modulu.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Vrátí tato vlastnost jen pro čtení **true** Pokud na stránce je součástí zpětné volání mezi stránkami. Mezi stránkami postbacky jsou popsané dále v tomto modulu.

## <a name="items"></a>Položky

Vrátí odkaz na rozhraní IDictionary instance, která obsahuje všechny objekty uložené v rámci stránky. Můžete přidat položky na tento objekt IDictionary a budou k dispozici v celém životním kontextu.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Tato vlastnost určuje, zda technologie ASP.NET generuje jazyka JavaScript, který udržuje, že na stránkách Posune pozici v prohlížeči po zpětném odeslání. (Podrobnosti o této vlastnosti bylo popsáno dříve v tomto modulu.)

## <a name="master"></a>Hlavní

Tato vlastnost jen pro čtení vrátí odkaz na instanci MasterPage pro stránku, do které byl použit na stránku předlohy.

## <a name="masterpagefile"></a>MasterPageFile

Získá nebo nastaví název souboru hlavní stránky pro stránku. Tuto vlastnost lze nastavit pouze v metodě PreInit.

## <a name="maxpagestatefieldlength"></a>Vlastnost MaxPageStateFieldLength

Tato vlastnost získá nebo nastaví maximální délku stavu stránky v bajtech. Pokud je nastavena na kladné číslo, stav zobrazení stránky bude možné rozdělit do více skrytá pole tak, aby nepřekračuje počet bajtů. Pokud je vlastnost nastavena na záporné číslo, nebude stav zobrazení rozdělen do bloků.

## <a name="pageadapter"></a>PageAdapter

Vrátí odkaz na objekt PageAdapter změní na stránce pro požadujícího prohlížeče.

## <a name="previouspage"></a>PreviousPage

Vrátí odkaz na předchozí stránku v případech Server.Transfer nebo zpětné volání mezi stránkami.

## <a name="skinid"></a>Identifikátor SkinID

Určuje skinu ASP.NET 2.0 použít na stránce.

## <a name="stylesheettheme"></a>Vlastnost StyleSheetTheme

Tato vlastnost získá nebo nastaví šablony stylů, která se uplatňuje na stránku.

## <a name="templatecontrol"></a>Třída TemplateControl

Vrátí odkaz na nadřazený ovládací prvek pro stránku.

## <a name="theme"></a>Motiv

Získá nebo nastaví název motivu ASP.NET 2.0 použité pro stránku. Tato hodnota musí být nastavena před PreInit – metoda.

## <a name="title"></a>Název

Tato vlastnost získá nebo nastaví název stránky získaný ze záhlaví stránky.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Získá nebo nastaví ViewStateEncryptionMode stránky. Podívejte se na podrobnou diskuzi o tuto vlastnost dříve v tomto modulu.

## <a name="new-protected-properties-of-the-page-class"></a>Nové chráněné vlastnosti třídy stránky

Toto jsou nové chráněné vlastnosti třídy stránky v technologii ASP.NET 2.0.

## <a name="adapter"></a>Adaptér

Vrátí odkaz na ControlAdapter vykreslující danou stránku na zařízení, která je požadovaná.

## <a name="asyncmode"></a>AsyncMode

Tato vlastnost určuje, zda je na stránce zpracovány asynchronně. Je určena pro použití modulem runtime a ne přímo v kódu.

## <a name="clientidseparator"></a>ClientIDSeparator

Tato vlastnost vrátí znak, který slouží jako oddělovač při vytváření klienta Jedinečný ID pro ovládací prvky. Je určena pro použití modulem runtime a ne přímo v kódu.

## <a name="pagestatepersister"></a>PageStatePersister

Tato vlastnost vrátí objekt PageStatePersister stránky. Tato vlastnost se používá především vývojáři ovládací prvek technologie ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Tato vlastnost vrátí jedinečný suffic, která se připojuje k souboru na cestě pro ukládání do mezipaměti prohlížeče. Výchozí hodnota je \_ \_ufps = a 6 číslic.

## <a name="new-public-methods-for-the-page-class"></a>Nové veřejné metody pro třídu stránky

Nová třída stránky v technologii ASP.NET 2.0 jsou tyto veřejné metody.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Metoda registruje delegátů obslužných rutin událostí pro provádění asynchronní stránky. Asynchronní stránky jsou popsány dále v tomto modulu.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Vlastnosti v šabloně stylů stránky se vztahuje na stránce.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Tato metoda bytostí asynchronní úlohu.

### <a name="getvalidators"></a>GetValidators

Vrátí kolekci validátory pro skupiny ověření zadaný nebo výchozí skupiny ověření, pokud není zadaný žádný.

## <a name="registerasynctask"></a>RegisterAsyncTask

Metoda registruje nový asynchronní úkol. Asynchronní stránky jsou popsané dále v tomto modulu.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Tato metoda říká technologie ASP.NET, musí jako trvalý stav ovládacího prvku stránky.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Tato metoda říká technologie ASP.NET, že stav zobrazení stránky vyžaduje šifrování.

## <a name="resolveclienturl"></a>ResolveClientUrl

Vrátí relativní adresu URL, které lze použít pro požadavky klientů pro obrázky, atd.

## <a name="setfocus"></a>SetFocus

Tato metoda nastaví fokus na ovládací prvek, který je zadán při počátečním načtení stránky.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Tato metoda zruší registraci ovládacího prvku, který je předán jako už vyžadující trvalost stavu ovládacího prvku.

## <a name="changes-to-the-page-lifecycle"></a>Změny životního cyklu stránky

Životní cyklus stránky v technologii ASP.NET 2.0 nedošlo ke změně výrazně, ale existují některé nové metody, které byste měli vědět. Životní cyklus stránky technologie ASP.NET 2.0 je popsaný níže.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (v technologii ASP.NET 2.0)

Událost PreInit je nejstarší fázi životního cyklu, které může vývojář přístup. Díky přidání této události je možné programově změnit ASP.NET 2.0 motivy, hlavní stránky, přístup k vlastnostem pro ASP.NET 2.0 profilu atd. Pokud jste v postback stavu, navíc je důležité si uvědomit, že vlastnost Viewstate nebyla dosud nebyly použity k ovládacím prvkům v tuto chvíli v životní cyklus. Proto pokud vývojář změní vlastnost ovládacího prvku v této fázi, bude pravděpodobně přepsána dále v cyklu stránky.

## <a name="init"></a>Init

Z ASP.NET nedošlo ke změně události Init 1.x. To je, kde chcete číst nebo inicializace vlastností ovládacích prvků na stránce. V této fázi, hlavní stránky, motivy se použijí na stránce již atd.

## <a name="initcomplete-new-in-20"></a>InitComplete (nově ve verzi 2.0)

Událost InitComplete je volána na konci fáze inicializace stránky. V tomto okamžiku v životního cyklu, dostanete ovládacích prvků na stránce, ale jejich stavu ještě není naplněná.

## <a name="preload-new-in-20"></a>Předběžné načtení (nově ve verzi 2.0)

Tato událost je volána po odeslání všech dat a před stránku\_zatížení.

## <a name="load"></a>Zatížení

Událost načtení nedošlo ke změně z ASP.NET 1.x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (nově ve verzi 2.0)

Událost LoadComplete je poslední události ve fázi načtení stránky. V této fázi byl připsán všechna data zpětné volání a stav zobrazení na stránce.

## <a name="prerender"></a>Provedení operace preRender

Pokud chcete pro stav zobrazení správně udržovat pro ovládací prvky, které jsou dynamicky přidat na stránku, událost PreRender je poslední příležitostí je přidat.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (nově ve verzi 2.0)

Ve fázi PreRenderComplete všechny ovládací prvky byly přidány na stránku a je připravený k vykreslení stránky. Událostí PreRenderComplete je poslední událost vyvolanou před uložením stav zobrazení stránky.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (nově ve verzi 2.0)

Událost SaveStateComplete je volána hned po uložení všech stav viewstate a ovládací prvek stránky. Toto je poslední událostí před skutečně vykreslení stránky v prohlížeči.

## <a name="render"></a>Vykreslení

Metody vykreslení nebyl změněn od ASP.NET 1.x. Toto je ve kterém je inicializován HtmlTextWriter a na stránce se zobrazí v prohlížeči.

## <a name="cross-page-postback-in-aspnet-20"></a>Zpětné volání mezi stránkami v ASP.NET 2.0

V technologii ASP.NET 1.x, postbacků byly zapotřebí k odeslání na stejné stránce. Zpětná volání mezi stránkami nebyly povoleny. ASP.NET 2.0 přidává možnost publikovat zpět na jinou stránku prostřednictvím rozhraní IButtonControl. Libovolný ovládací prvek, který implementuje rozhraní IButtonControl (tlačítko, odkazem (LinkButton) a ImageButton kromě vlastní ovládací prvky třetích stran) můžete využít tyto nové funkce prostřednictvím použití atributu PostBackUrl. Následující kód ukazuje ovládací prvek tlačítka, který odešle zpět na druhé stránce.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Na stránce, když se pošle zpět na stránku, která zahájí zpětné volání, jsou přístupná přes vlastnost PreviousPage na druhé stránce. Tato funkce se implementuje prostřednictvím nového webového formuláře\_DoPostBackWithOptions funkce na straně klienta, která ASP.NET 2.0 vykresluje na stránku, když ovládací prvek odeslat zpět na jiné stránce. Tato funkce jazyka JavaScript poskytuje nové obslužná rutina WebResource.axd, který generuje skript do klienta.

Následující video je návod, zpětné volání mezi stránkami.


![](the-asp-net-2-0-page-model/_static/image2.png)


[Otevřít Video na celou obrazovku](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a>Další podrobnosti o postbacků mezi stránkami

### <a name="viewstate"></a>Stav zobrazení

Můžete mít vyzváni sami už o co se stane vlastnost viewstate od první stránky ve scénáři postback mezi stránkami. Po všech libovolný ovládací prvek, který neimplementuje rozhraní IPostBackDataHandler zachová svůj stav prostřednictvím vlastnosti viewstate, tak přístup k vlastnostem ovládacího prvku na druhé stránce zpětné volání mezi stránkami, musí mít přístup k vlastnost viewstate pro stránku. Tento scénář s využitím nového skrytého pole na druhé stránce volá postará technologie ASP.NET 2.0 \_ \_PREVIOUSPAGE. \_ \_PREVIOUSPAGE pole formuláře obsahuje vlastnost viewstate na první stránce tak, že máte přístup k vlastnostem všechny ovládací prvky na druhé stránce.

### <a name="circumventing-findcontrol"></a>Obcházení FindControl

V video s názorným postupem postbacku mezi stránkami můžu FindControl metoda používá k získání odkazu na ovládací prvek TextBox na první stránce. Tato metoda funguje dobře pro tento účel, ale FindControl je nákladné a psaním dalšího kódu, vyžaduje. Naštěstí ASP.NET 2.0 poskytuje alternativu k FindControl pro tento účel, který bude fungovat v mnoha scénářích. Direktiva PreviousPageType umožňuje mít silný odkaz na předchozí stránku pomocí TypeName nebo VirtualPath atribut. Atribut TypeName umožňuje určit typ na předchozí stránku, zatímco Atribut VirtualPath umožňuje odkazovat na předchozí stránku pomocí virtuální cesty. Po direktivě PreviousPageType, musí pak vystavit ovládací prvky, ke kterému chcete povolit přístup pomocí veřejné vlastnosti atd.

## <a name="lab-1-cross-page-postback"></a>Praktické cvičení 1 mezi stránkami postbacku

V tomto testovacím prostředí vytvoříte aplikaci, která využívá nové funkce zpětného volání mezi stránkami ASP.NET 2.0.

1. Otevřít Visual Studio 2005 a vytvořit nový web ASP.NET.
2. Přidání nového webového formuláře volá page2.aspx.
3. V návrhovém zobrazení otevřete Default.aspx a přidejte ovládací prvek Button a ovládací prvek textového pole. 

    1. Zadejte ID ovládacího prvku tlačítko **tlacitkoOdeslat** a textovém poli ID **uživatelské jméno**.
    2. Nastavte vlastnost PostBackUrl tlačítka na page2.aspx.
4. Otevřete page2.aspx v zobrazení zdroje.
5. Direktiva @ PreviousPageType přidáte, jak je znázorněno níže:
6. Přidejte následující kód na stránku\_zatížení vaší page2.aspx kódu: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Sestavte projekt kliknutím na sestavení v nabídce sestavení.
8. Přidejte následující kód do kódu pro Default.aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Změnit na stránce\_zatížení v page2.aspx takto: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Sestavte projekt.
11. Spusťte projekt.
12. Zadejte název do textového pole a klikněte na tlačítko.
13. Co je výsledkem?

## <a name="asynchronous-pages-in-aspnet-20"></a>Asynchronní stránek v ASP.NET 2.0

Mnohé problémy kolizí v technologii ASP.NET jsou způsobeny latence externí volání (například volání webové služby nebo databáze), souboru vstupně-výstupní latence, atd. Po odeslání žádosti se pro aplikaci ASP.NET, ASP.NET používá jednu z jeho pracovní vlákna ke zpracování této žádosti. Tento požadavek na vlastní bylo vlákno až do dokončení požadavku a byl odeslán odpovědi. ASP.NET 2.0 vyhledá vyřešit problémy s latencí s těmito typy chyb tak, že přidáte funkci stránky spustit asynchronně. To znamená, že můžete začít požadavek a pak předá další provádění do jiného vlákna, a tím vrátí do fondu vláken k dispozici rychle pracovní podproces. Po dokončení vstupně-výstupní operace souboru, volání databáze a další, nové vlákno se získávají z fondu vláken k dokončení požadavku.

Prvním krokem při vytváření stránky spustit asynchronně, je nastavena **asynchronní** atribut direktivy stránky takto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Tento atribut oznamuje implementovat IHttpAsyncHandler stránky technologie ASP.NET.

Dalším krokem je volání metody AddOnPreRenderCompleteAsync v určitém bodě životní cyklus stránky před provedením operace PreRender. (Tato metoda je obvykle volána na stránce\_zatížení.) Metoda AddOnPreRenderCompleteAsync přebírá dva parametry. BeginEventHandler a EndEventHandler. BeginEventHandler vrátí objekt IAsyncResult, který je pak předán jako parametr EndEventHandler.

Video níže je návod požadavku asynchronní stránky.


![](the-asp-net-2-0-page-model/_static/image3.png)


[Otevřít Video na celou obrazovku](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> Stránku asynchronní nevykresluje do prohlížeče, dokud se nedokončí EndEventHandler. Žádné nejisté, ale někteří vývojáři budou představit asynchronních jako podobný zpětná volání asynchronní. Je důležité si uvědomit, že nejsou. Výhoda pro asynchronní požadavků je, že první pracovní vlákno mohou být vráceny do fondu vláken zpracování nových požadavků, a tím snižuje kolize kvůli se vstupně-výstupních operací, které jsou vázány, atd.


## <a name="script-callbacks-in-aspnet-20"></a>Zpětná volání skriptu v ASP.NET 2.0

Weboví vývojáři se vždy podívat způsoby, jak zabránit blikání přidružené zpětné volání. V technologii ASP.NET 1.x SmartNavigation bylo nejběžnější metodou pro předcházení blikání, ale SmartNavigation způsobil problémy pro vývojáře v některé z důvodu složitosti implementace na straně klienta. ASP.NET 2.0 řeší tento problém s zpětná volání skriptu. Zpětná volání skriptu využívat XMLHttp k podání žádostí o u webového serveru pomocí jazyka JavaScript. Žádost o XMLHttp vrátí data XML, který lze potom manipulovat prostřednictvím prohlížeče modelu DOM. XMLHttp kód je pro uživatele skryté nová obslužná rutina WebResource.axd.

Existuje několik kroků, které jsou nezbytné, aby se konfigurace zpětného volání skriptu v technologii ASP.NET 2.0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Krok 1: Implementace rozhraní ICallbackEventHandler

Aby ASP.NET rozpoznat stránce jako účasti ve zpětném volání skriptu musí implementovat rozhraní ICallbackEventHandler. Můžete to provést v souboru modelu code-behind takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

Můžete také Uděláte to pomocí direktiv jako @ implementuje:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Obvykle použijete – direktiva @ implementuje při použití vloženého kódu ASP.NET.

## <a name="step-2--call-getcallbackeventreference"></a>Krok 2: Volání GetCallbackEventReference

Jak už bylo zmíněno dříve, je zapouzdřena volání XMLHttp v obslužná rutina WebResource.axd. Při vykreslování stránky ASP.NET se přidejte volání do webového formuláře\_DoCallback klientský skript, který je poskytován WebResource.axd. Webového formuláře\_nahrazuje funkce DoCallback \_ \_doPostBack funkce pro zpětné volání. Nezapomeňte, že \_ \_doPostBack programově odeslání formuláře na stránce. Ve scénáři zpětné volání, ale chcete zabránit zpětné volání, takže \_ \_doPostBack nestačí.

> [!NOTE]
> \_\_doPostBack se zobrazí stránku ve scénáři skript zpětného volání klienta. Ale není použit pro zpětné volání.


Argumenty pro webovém formuláři\_DoCallback funkce na straně klienta jsou k dispozici prostřednictvím funkce na straně serveru GetCallbackEventReference, která by obvykle nazývat stránce\_zatížení. Typické volání GetCallbackEventReference může vypadat takto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> V takovém případě je cm instancí ClientScriptManager. Třída ClientScriptManager se budeme dále v tomto modulu.


Existuje několik přetížené verze GetCallbackEventReference. V takovém případě argumenty jsou následující:

`this`

Odkaz na ovládací prvek, ve kterém je volána GetCallbackEventReference. V takovém případě je samotné stránky.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Argument řetězce, které budou předány z kódu na straně klienta pro událost na straně serveru. V tomto případě zasílání rychlých zpráv, předejte hodnotu z rozevíracího seznamu název ddlCompany.

`ShowCompanyName`

Název funkce na straně klienta, který bude přijímat návratovou hodnotu (jako řetězce) z události zpětného volání na straně serveru. Tato funkce bude volat pouze po úspěšné zpětného volání na straně serveru. Proto z důvodu odolnosti, obecně doporučujeme používat přetížené verze GetCallbackEventReference, která přebírá argument další řetězec určující název funkce na straně klienta pro spuštění v případě chyby.

`null`

Řetězec představující funkce na straně klienta, která bylo zahájeno před zpětného volání k serveru. V takovém případě není žádný skript, takže má argument hodnotu null.

`true`

Logická hodnota určující, jestli se mají asynchronně provádění zpětného volání.

Volání webového formuláře\_DoCallback na straně klienta předá tyto argumenty. Proto se při vykreslení této stránky na straně klienta, tento kód bude vypadat takto:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Všimněte si, že podpis funkce na straně klienta je trochu jiná. Funkce na straně klienta předá 5 řetězce a logickou hodnotu. Další řetězec (který má hodnotu null v příkladu výše) obsahuje funkce na straně klienta, který bude zpracovávat chyby ze zpětného volání na straně serveru.

## <a name="step-3--hook-the-client-side-control-event"></a>Krok 3: Připojení událostí ovládacího prvku na straně klienta

Všimněte si, že návratová hodnota GetCallbackEventReference výše byl přiřazen k proměnné řetězce. Tento řetězec se používá k připojení události na straně klienta pro ovládací prvek, který iniciuje zpětného volání. V tomto příkladu rozevíracího seznamu na stránce inicioval zpětné volání, chci se zapojit *OnChange* událostí.

K připojení události na straně klienta, jednoduše přidejte obslužnou rutinu kód na straně klienta následujícím způsobem:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Vzpomeňte si, že *cbRef* je návratová hodnota z volání GetCallbackEventReference. Obsahuje volání do webového formuláře\_DoCallback zobrazená výše.

## <a name="step-4--register-the-client-side-script"></a>Krok 4: Registrace skriptu na straně klienta

Vzpomínáte, který volání GetCallbackEventReference zadán, skript na straně klienta volána **ShowCompanyName** by byl proveden po úspěšném zpětného volání na straně serveru. Tento skript je potřeba přidat na stránku pro použití ClientScriptManager instance. (Třída ClientScriptManager bude dicussed později v tomto modulu.) To provedete jako v tomto:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Krok 5: Volání metody rozhraní ICallbackEventHandler

Rozhraní ICallbackEventHandler obsahuje dvě metody, které je nutné implementovat v kódu. Jsou **RaiseCallbackEvent** a **GetCallbackEvent**.

**RaiseCallbackEvent** přijímá řetězec jako argument a vrátí hodnotu nothing. Řetězcový argument je předán z volání na straně klienta webového formuláře\_DoCallback. V takovém případě je tato hodnota *hodnotu* atribut volá ddlCompany rozevíracího seznamu. Váš kód na straně serveru musí být umístěné ve RaiseCallbackEvent metody. Například pokud je vaše zpětné volání žádosti WebRequest proti externímu prostředku, tento kód musí být umístěné ve RaiseCallbackEvent.

**GetCallbackEvent** je zodpovědná za zpracování vrácení zpětné volání do klienta. Nepřijímá žádné argumenty a vrátí hodnotu typu string. Řetězec, který vrátí se předá jako argument pro funkci na straně klienta v tomto případě *ShowCompanyName*.

Po dokončení výše uvedené kroky, jste připraveni k provádění zpětného volání skriptu v ASP.NET 2.0.


![](the-asp-net-2-0-page-model/_static/image4.png)


[Otevřít Video na celou obrazovku](the-asp-net-2-0-page-model/_static/callback1.wmv)


Zpětná volání skriptu v ASP.NET jsou podporovány v jakémkoli prohlížeči, který podporuje provedete XMLHttp volání. To zahrnuje všechny moderní prohlížeče používá ještě dnes. Aplikace Internet Explorer používá objektu XMLHttp ActiveX při použití vnitřního objektu XMLHttp dalších moderních prohlížečů (včetně nadcházející aplikace Internet Explorer 7). K určení prostřednictvím kódu programu, pokud je prohlížeč podporuje zpětná volání, můžete použít **Request.Browser.SupportCallback** vlastnost. Tato vlastnost vrátí **true** Pokud klienta, který podporuje zpětná volání skriptu.

## <a name="working-with-client-script-in-aspnet-20"></a>Práce s klientským skriptem v technologii ASP.NET 2.0

Skripty klienta v technologii ASP.NET 2.0 jsou spravované prostřednictvím použití třídy ClientScriptManager. Třída ClientScriptManager uchovává informace o klientských skriptů pomocí typu a název. Díky tomu stejný skript programově vloženého na stránce více než jednou.

> [!NOTE]
> Skript po úspěšné registraci na stránce, jakékoli následné pokusy o registraci stejný skript jednoduše výsledkem skriptu není zaregistrovaný podruhé. Budou přidány žádné duplicitní skripty a dojde k žádné výjimce. Aby se zabránilo zbytečným výpočtu, jsou metody, které můžete použít k určení, jestli skript je už zaregistrovaný, takže není pokusí zaregistrovat víc než jednou.


Metody ClientScriptManager by měl být všechny aktuální vývojáře využívající technologii ASP.NET:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Tato metoda přidá do horní části na vykreslené stránce skriptu. To je užitečné pro přidávání funkcí, které se explicitně volat na straně klienta.

Existují dvě přetížené verze této metody. Tři ze čtyř argumentů jsou běžné mezi nimi. Možnosti jsou následující:

`type (string)`

***Typ*** argument určuje typ skriptu. Obecně je vhodné použít typ stránky (this. GetType()) typu.

`key (string)`

***Klíč*** argument je uživatelský klíč pro skript. To by měl být jedinečný pro každý skript. Pokud se pokusíte přidat skript se stejným klíčem a typ již přidané skriptu, nebude přidán.

`script (string)`

***Skript*** argument je řetězec obsahující skutečné skript pro přidání. Je doporučeno použít StringBuilder vytvořit skript a pak použijte metodu ToString() ve třídě StringBuilder nebyla přiřadit ***skript*** argument.

Pokud používáte přetížené RegisterClientScriptBlock, který zabere jenom tři argumenty, musí obsahovat prvky skriptu (&lt;skript&gt; a &lt;/script&gt;) ve vašem skriptu.

Můžete použít přetížení RegisterClientScriptBlock, která přijímá čtvrtý argument. Čtvrtý argument je logická hodnota, která určuje, zda technologie ASP.NET přidala prvky skriptu za vás. Pokud je tento argument **true**, váš skript by neměla obsahovat prvky skriptu explicitně.

Pomocí této metody IsClientScriptBlockRegistered určit, jestli je skript už zaregistrovaný. To umožňuje vyhnout se znovu zaregistrovat skript, který už je zaregistrovaný.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (nově ve verzi 2.0)

Značka RegisterClientScriptInclude vytvoří blok skriptu odkazující na externí soubor skriptu. Má dvě přetížení. Přijímá jeden klíč a adresu URL. Druhá přidá třetí argument určující typ.

Následující kód například vygeneruje blok skriptu, který odkazuje na jsfunctions.js v kořenové složce skriptů aplikace:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Tento kód vytvoří následující kód na vykreslené stránce:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> Blok skriptu se vykreslí v dolní části stránky.


Pomocí této metody IsClientScriptIncludeRegistered určit, jestli je skript už zaregistrovaný. To umožňuje vyhnout se znovu zaregistrovat skript.

## <a name="registerstartupscript"></a>RegisterStartupScript

Metoda RegisterStartupScript používá stejné argumenty jako metodu RegisterClientScriptBlock. Skript zaregistrovaného RegisterStartupScript provádí po načtení stránky, ale před událostí načtení na straně klienta. V 1.X, skripty, které jsou zaregistrované RegisterStartupScript zadal těsně před uzavírací &lt;/form&gt; označit při skripty zaregistrovaného RegisterClientScriptBlock byly umístěny ihned po otevření &lt;formuláře&gt; značky. V technologii ASP.NET 2.0, obě jsou umístěné bezprostředně před uzavírací &lt;/form&gt; značky.

> [!NOTE]
> Když si zaregistrujete funkce s RegisterStartupScript, tato funkce nebude spuštěno, dokud ho explicitně volat v kódu na straně klienta.


Pomocí metody IsStartupScriptRegistered můžete určit, jestli je skript už zaregistrovaný a vyhnout se znovu zaregistrovat skript.

## <a name="other-clientscriptmanager-methods"></a>Jiné metody ClientScriptManager

Tady jsou některé z dalších užitečných metod ClientScriptManager třídy.


|  <strong>GetCallbackEventReference</strong>   |                                                 Viz zpětná volání skriptu dříve v tomto modulu.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Získá odkaz JavaScript (jazyk javascript:&lt;volání&gt;), který je možné publikovat zpět z události na straně klienta.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Získá řetězec, který slouží k zahájení příspěvek zpět od klienta.                                    |
|      <strong>GetWebResourceUrl</strong>       | Vrátí adresu URL k prostředku, který je vložen do sestavení. Je potřeba použít ve spojení s <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Zaregistruje prostředek webové stránky. Toto jsou prostředky součástí sestavení a zpracovat nová obslužná rutina WebResource.axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Zaregistruje skryté pole formuláře se stránkou.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Zaregistruje kód na straně klienta, který se spustí, když se odešle formulář HTML.                                   |

