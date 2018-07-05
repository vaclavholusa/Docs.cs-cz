---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Poznámky k verzi pro ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012 | Dokumentace Microsoftu
author: microsoft
description: Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 85cd45c25e0f2ad3c8d6d6de73a1a493533e7f7b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374257"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Zpráva k vydání verze pro ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012
====================
podle [Microsoft](https://github.com/microsoft)

> Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.


## <a name="contents"></a>Obsah

- [Poznámky k instalaci](#install)
- [Požadavky na software](#requirements)
- Novinky v ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Šablony](#templates)

        - [Šablony ASP.NET MVC 5](#mvc5template)
        - [Šablony ASP.NET Web API 2](#apitemplate)
        - [Šablony položek](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET generování uživatelského rozhraní](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Známé problémy a změny způsobující chyby

    - [ASP.NET generování uživatelského rozhraní](#issuescaffolding)

        - [MVC a generování rozhraní Web API – HTTP 404, nebyla nalezena chyba](#404issue)
        - [Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky](#expressissue)
    - [Syntaxe Razor rozhraní ASP.NET 3](#issuerazor)

        - [Zobrazení souboru cshtml procházet s nebo F5 způsobí chybu serveru](#browseissue)
        - [Přepsání adresy URL a Tilde(~)](#rewriteissue)
    - [Šablony](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

[Nainstalujte](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) technologie ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Novinky v ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Při generování uživatelského rozhraní zobrazení a kontrolerů MVC 5, používá značky pro zobrazení [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Šablony

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Šablony ASP.NET MVC 5

Přidali jsme nové šablony MVC 5. Odkazuje na nejnovější balíčky MVC 5 NuGet a generování uživatelského rozhraní můžete použít k přidání kontrolerů a zobrazení.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Šablony ASP.NET Web API 2

Přidali jsme nové šablony webové rozhraní API 2. Odkazuje na nejnovější balíčky NuGet 2 webové rozhraní API a generování uživatelského rozhraní můžete použít k přidání kontrolerů a zobrazení.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Šablony položek

Přidali jsme nové šablony položek pro zobrazení MVC 5, Web Pages (Razor 3) a webovým rozhraním API 2 řadiče. Při přidání nové položky nainstalují související balíčků NuGet do projektu.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Při generování uživatelského rozhraní řadič MVC nebo webového rozhraní API pomocí Entity Frameworku používáme Framework 6. Další informace o rozhraní Entity Framework naleznete v tématu [historie verzí Entity Framework](https://msdn.com/data/jj574253).

Můžete také stáhnout a nainstalovat Entity Framework 6 Tools for Visual Studio 2012. Zobrazit [získat Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

ASP.NET generování uživatelského rozhraní je architektura generování kódu pro webové aplikace ASP.NET. To umožňuje snadno přidat do projektu, který komunikuje s datovým modelem často používaný kód.

V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC. S touto aktualizací teď můžete generování uživatelského rozhraní pro libovolný projekt ASP.NET, včetně webových formulářů. Tato aktualizace nepodporuje generování stránek pro projekt webových formulářů, ale můžete pořád používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte k projektu závislosti MVC. Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.

Při používání generování uživatelského rozhraní, zajišťujeme, že všechny požadované závislosti jsou nainstalovány v projektu. Například pokud začínat projekt webových formulářů ASP.NET a poté použijte generování uživatelského rozhraní pro přidání Kontroleru webového rozhraní API, požadované balíčky NuGet a odkazy jsou přidány do projektu automaticky.

Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně. Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, pouze balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do vašeho projektu. Pokud vyberete možnost Úplná minimální závislosti jsou přidány, a také požadované soubory obsahu pro projekt MVC.

Podpora pro generování kontrolerů asynchronní používá nové asynchronní funkce z Entity Framework 6.

Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](../2013/aspnet-scaffolding-overview.md). Tyto kurzy vám ukážou generování uživatelského rozhraní pomocí sady Visual Studio 2013, ale navíc se vztahuje na ASP.NET a webové nástroje 2013.1 pro Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

V této aktualizaci Visual Studio 2012 teď podporuje nástroje a úpravy Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zahrnuje bohatou sadu nových funkcí, které jsou popsány podrobně v [zpráva k vydání verze NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Tato verze NuGet eliminuje potřebu uživatelům výslovně povolit NuGet Chcete-li obnovit chybějící balíčky. Při instalaci NuGet 2.7, uživatelé se implicitně souhlasit s automaticky obnovit chybějící balíčky. Uživatele můžete výslovně nesouhlasit obnovení balíčku NuGet nastavení v sadě Visual Studio. Tato změna zjednodušuje, jak funguje obnovení balíčku.

## <a name="known-issues-and-breaking-changes"></a>Známé problémy a změny způsobující chyby

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC a generování rozhraní Web API – HTTP 404, nebyla nalezena chyba

Pokud narazíte na chybu při přidání vygenerované položky do projektu, je možné, že váš projekt bude ponechána v nekonzistentním stavu. Některé změny se generování uživatelského rozhraní bude vrácena zpět, ale jiné změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět. Pokud se vrátí zpět změny konfigurace směrování, uživatelům se zobrazí chyba HTTP 404 při přechodu na automaticky generovaný položky.

Chcete-li vyřešit tuto chybu pro MVC, přidejte nová vygenerovaná položka a vybrat závislosti MVC 5 (minimální nebo úplná). Tento proces se všechny požadované změny, přidejte do projektu.

Chcete-li vyřešit tuto chybu pro webové rozhraní API:

1. Do projektu přidejte následující třídy WebApiConfig.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. V aplikaci nakonfigurovat WebApiConfig.Register\_začátek metody v souboru Global.asax následujícím způsobem:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky

Pokud sadu Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky s Entity Framework (například Kontroleru webového rozhraní API 2 s akcemi používající nástroj Entity Framework), je možné, že Visual Studio Express se nepodařilo načíst nativní bitové kopie sestavení závisí na System.Web.Extensions.

Chcete-li tento problém, nakonfigurujte Visual Studio Express pro práci s MSIL obrázek System.Web.Extensions:

1. Otevřete příkazový řádek v režimu správce.
2. Přejděte na %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE nebo % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pro 64bitová verze Windows).
3. V textovém editoru otevřete VWDExpress.exe.config.
4. Přidejte následující řádek pod &lt;konfigurace&gt;/&lt;runtime&gt; element:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Restartování sady Visual Studio Express 2012 pro Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>Syntaxe Razor rozhraní ASP.NET 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Zobrazení withBrowse souboru cshtml WithorF5causes chyba serveru

Při vytvoření projektu aplikace MVC 5 v sadě Visual Studio 2012 (nebo otevřít v sadě Visual Studio 2012 MVC 5 projekt, který byl vytvořen v sadě Visual Studio 2013) a pokusíte se k zobrazení souboru cshtml pomocí procházet s nebo F5, zobrazí se chybové zprávy, - **chyba serveru Aplikace '/'**. Server se pokusí přejít na `http://localhost:XXXX/Views/../XXXX.cshtml`

Chcete-li tento problém vyřešit, změňte **spustit akci** nastavení ve vašem projektu a **konkrétní stránka**. Není potřeba zadat hodnotu pro stránku.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Po provedení této změny výběru F5 přejde do kořenového adresáře aplikace (`http://localhost:XXXX`). Toto chování není stejný jako chování pro projekty MVC 5 v sadě Visual Studio 2013, kde **aktuální stránku** spustí otevřete stránku nastavení.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Přepsání adresy URL a Tilde(~)

Po upgradu na 3 Razor technologie ASP.NET nebo ASP.NET MVC 5, zápis tilde(~) už nemusí fungovat správně Pokud používáte adresu URL přepisů. Přepsání adresy URL ovlivňuje zápis tilde(~) prvků HTML, jako &lt;A /&gt;, &lt;skript /&gt;, &lt;odkaz /&gt;, a v důsledku tilda už mapuje na kořenový adresář.

Například, pokud přepsání požadavků pro **asp.net/content** k **asp.net**, atribut href v &lt;A href = "~/content/" /&gt; přeloží na **/content/ obsah /** místo **/**. Chcete-li potlačit tuto změnu, můžete nastavit **IIS\_WasUrlRewritten** na hodnotu false v jednotlivých webových stránkách nebo v kontextu **aplikace\_BeginRequest** v souboru Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Šablony

Při vytváření technologie ASP.NET MVC projektů s Visual Studio 2012 ve Windows 8.1 nebo Windows Server 2012 R2, Visual Studio zobrazí chybovou zprávu s oznámením "Webové konfigurace [url] pro technologii ASP.NET 4.5 se nezdařilo."

![Chyba konfigurace](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Tato chyba se může zobrazit, protože Visual Studio 2012 není funkce technologie ASP.NET 4.5 nainstalovaném v těchto verzích Windows. Pokud chcete povolit technologii ASP.NET 4.5, proveďte kroky popsané v [Windows zapnout nebo vypnout funkce](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![zapnout nebo vypnout funkce Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternativně můžete povolit technologii ASP.NET 4.5 prostřednictvím příkazového řádku.

1. Otevřete příkazový řádek v režimu správce.
2. Spuštěním následujícího příkazu povolte ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
