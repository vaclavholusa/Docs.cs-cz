---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "Poznámky k verzi pro ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012 | Microsoft Docs"
author: microsoft
description: "Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1e4ee8eb4901305bf6a8c9c5b949dc4ee10290e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Poznámky k verzi pro ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012
====================
podle [Microsoft](https://github.com/microsoft)

> Tento dokument popisuje verzi technologie ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.


## <a name="contents"></a>Obsah

- [Poznámky k instalaci](#install)
- [Požadavky na software](#requirements)
- Nové funkce v technologii ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Šablony](#templates)

        - [Šablony ASP.NET MVC 5](#mvc5template)
        - [Šablony ASP.NET Web API 2](#apitemplate)
        - [Šablony položek](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET generování uživatelského rozhraní](#scaffold)
    - [Razor Editor](#razor)
    - [NuGet 2.7](#nuget)
- Známé problémy a nejnovější změny

    - [ASP.NET generování uživatelského rozhraní](#issuescaffolding)

        - [MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba](#404issue)
        - [Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky](#expressissue)
    - [Syntaxe Razor rozhraní ASP.NET 3](#issuerazor)

        - [Prohlížení souboru cshtml s procházet s nebo F5 způsobí chybu serveru](#browseissue)
        - [Přepisování adres URL a Tilde(~)](#rewriteissue)
    - [Šablony](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Poznámky k instalaci

[Nainstalujte](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Požadavky na software

Musíte mít Visual Studio 2012 nebo Visual Studio Express 2012 pro Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nové funkce v technologii ASP.NET a nástroje pro Web 2013.1 pro sadu Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Když generujete řadiče MVC 5 a zobrazení, kód pro zobrazení používá [Bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Šablony

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Šablony ASP.NET MVC 5

Jsme přidali novou šablonu MVC 5. Odkazuje na nejnovější balíčky MVC 5 NuGet a generování uživatelského rozhraní můžete použít k přidání řadiče a zobrazení.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Šablony ASP.NET Web API 2

Jsme přidali novou šablonu webovém rozhraní API 2. Odkazuje na nejnovější balíčky NuGet 2 webové rozhraní API a generování uživatelského rozhraní můžete použít k přidání řadiče a zobrazení.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Šablony položek

Jsme přidali nové šablony položek pro zobrazení MVC 5, webové stránky (Razor 3) a řadiče webovém rozhraní API 2. Při přidávání nové položky instalaci související balíčky NuGet do projektu.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Když generujete řadič MVC nebo webového rozhraní API pomocí rozhraní Entity Framework, použijeme Framework 6. Další informace o rozhraní Entity Framework najdete v tématu [Entity Framework verze historie](https://msdn.com/data/jj574253).

Můžete také stáhnout a nainstalovat nástroje Entity Framework 6 pro sadu Visual Studio 2012. Najdete v článku [získat rozhraní Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

Generování uživatelského rozhraní ASP.NET je architektura generování kódu pro webové aplikace ASP.NET. Usnadňuje přidat často používaný kód do projektu, který komunikuje s datovým modelem.

V předchozích verzích sady Visual Studio generování uživatelského rozhraní omezovala na projekty ASP.NET MVC. S touto aktualizací teď můžete generování uživatelského rozhraní pro všechny projekt ASP.NET, včetně webových formulářů. Tato aktualizace nepodporuje generování stránky pro projekt webové formuláře, ale když můžete nadále používat generování uživatelského rozhraní s webovými formuláři tak, že přidáte do projektu závislosti MVC. Podpora pro generování stránky pro webové formuláře bude přidána v budoucí aktualizaci.

Při použití generování uživatelského rozhraní, je zajištěno, že všechny požadované závislosti jsou nainstalované v projektu. Například když začít s projektem webových formulářů ASP.NET a přidání Kontroleru webového rozhraní API pomocí generování uživatelského rozhraní, požadované balíčky NuGet a odkazy se přidají do projektu automaticky.

Chcete-li přidat generování uživatelského rozhraní MVC do projektu webových formulářů, přidejte **novou vygenerovanou položku** a vyberte **závislosti MVC 5** v dialogovém okně. Existují dvě možnosti pro generování uživatelského rozhraní MVC; Minimální a úplné. Pokud vyberete minimální, jenom balíčky NuGet a odkazy pro architekturu ASP.NET MVC se přidají do projektu. Pokud vyberete možnost Úplná minimální závislosti přidají, a také požadované soubory obsahu pro projekt MVC.

Podpora pro generování uživatelského rozhraní asynchronní řadiče využívá nové funkce asynchronní z Entity Framework 6.

Další informace a podrobné pokyny najdete v tématu [generování uživatelského rozhraní ASP.NET: Přehled](../2013/aspnet-scaffolding-overview.md). Tyto kurzy zobrazit generování uživatelského rozhraní s Visual Studio 2013, ale jsou i pro technologii ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Editor

Pomocí této aktualizace Visual Studio 2012 teď podporuje nástrojů nebo úpravách Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 zahrnuje celou řadu nových funkcí, které jsou popsány podrobně v [poznámky k verzi 2.7 NuGet](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Tato verze NuGet odebírá potřebu uživatelům výslovně povolit NuGet, chcete-li obnovit chybějící balíčky. Při instalaci NuGet 2.7, uživatelé implicitně svůj souhlas automaticky obnovují se balíčky chybí. Uživatelé mohou explicitně vyjádření výslovného nesouhlasu balíček obnovení prostřednictvím nastavení NuGet v sadě Visual Studio. Tato změna zjednodušuje, jak funguje obnovení balíčku.

## <a name="known-issues-and-breaking-changes"></a>Známé problémy a nejnovější změny

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET generování uživatelského rozhraní

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC a webových rozhraní API generování uživatelského rozhraní – chyby HTTP 404, nebyla nalezena chyba

Pokud dojde k chybě při přidání vygenerované položky do projektu, je možné, že projekt bude ponechána v nekonzistentním stavu. Některé změny provedené být generování uživatelského rozhraní se vrátí zpátky, ale ostatní změny, jako je například nainstalované balíčky NuGet, nebude vrácena zpět. Směrování změny konfigurace se vrátí zpátky, uživatelé obdrží chybu HTTP 404 při navigaci na generované uživatelské rozhraní položky.

Chcete-li vyřešit tuto chybu pro MVC, přidat novou položku vygenerované a vyberte závislosti MVC 5 (minimální nebo úplná). Tento proces bude do projektu přidejte všechny požadované změny.

Odstranění této chyby pro webového rozhraní API:

1. Do projektu přidejte následující třídy WebApiConfig.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Konfigurace WebApiConfig.Register v aplikaci\_Start – metoda v souboru Global.asax následujícím způsobem:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky

Pokud sadu Visual Studio Express 2012 pro Web přestane pracovat po přidání vygenerované položky s platformou Entity Framework (například webové 2 kontroler API s akcemi používající rozhraní Entity Framework), je možné, že Visual Studio Express Nepodařilo se načíst nativní bitové kopie sestavení závisí na System.Web.Extensions.

Chcete-li tento problém, nakonfigurujte Visual Studio Express pro práci s MSIL bitové kopie System.Web.Extensions:

1. Otevřete příkazový řádek v režimu správce.
2. Přejděte na 11.0\Common7\IDE %ProgramFiles%\Microsoft Visual Studio nebo % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (pro 64bitové Windows).
3. V textovém editoru otevřete VWDExpress.exe.config.
4. Přidejte následující řádek v části &lt;konfigurace&gt;/&lt;runtime&gt; element:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Restartování sady Visual Studio Express 2012 pro Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>Syntaxe Razor rozhraní ASP.NET 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Zobrazení withBrowse soubor cshtml WithorF5causes chyby serveru

Při vytváření projektu MVC 5 v sadě Visual Studio 2012 (nebo otevřete v projektu sady Visual Studio 2012 MVC 5, který byl vytvořen v sadě Visual Studio 2013) a pokus o zobrazení souboru cshtml pomocí procházet s nebo F5, se zobrazí chyba s oznámením - **chyba serveru v Aplikace '/'**. Server se pokusí přejděte na`http://localhost:XXXX/Views/../XXXX.cshtml`

Chcete-li problém vyřešit, změňte **spustit akci** nastavení ve vašem projektu a **konkrétní stránky**. Není nutné zadat hodnotu pro stránku.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Po provedení této změny, výběr F5 přejde do kořenového adresáře aplikace (`http://localhost:XXXX`). Toto chování není stejný jako chování pro projekty MVC 5 v sadě Visual Studio 2013, kde **aktuální stránku** nastavení spustí otevřete stránku.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Přepisování adres URL a Tilde(~)

Po upgradu na ASP.NET Razor 3 nebo ASP.NET MVC 5, zápis tilde(~) může již nebude fungovat správně, pokud používáte přepisů adresy URL. Přepisování adres URL ovlivňuje tilde(~) zápis v elementů HTML jako &lt;A /&gt;, &lt;skriptu nebo&gt;, &lt;odkaz nebo&gt;, a v důsledku tilda už mapuje na kořenový adresář.

Například, pokud přepsání žádosti o **asp.net/content** k **asp.net**, atribut href v &lt;A href = "~/content/" /&gt; přeloží na **/content/ obsah nebo** místo  **/** . Chcete-li potlačit tuto změnu, můžete nastavit **IIS\_WasUrlRewritten** kontext, který má hodnotu false na každé webové stránce nebo v **aplikace\_BeginRequest** v souboru Global.asax.

<a id="templateissue"></a>
### <a name="templates"></a>Šablony

Při vytváření rozhraní ASP.NET MVC projektů pomocí sady Visual Studio 2012 na Windows 8.1 nebo Windows Server 2012 R2, Visual Studio zobrazí chybovou zprávu s oznámením "Web konfigurace [url] pro technologii ASP.NET 4.5 se nezdařilo."

![Chyba konfigurace.](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Tato chyba se může zobrazit, protože Visual Studio 2012 není funkce technologie ASP.NET 4.5 při instalaci v těchto verzích Windows. Pokud chcete povolit technologii ASP.NET 4.5, proveďte kroky popsané v [Windows zapnout nebo vypnout funkce](https://windows.microsoft.com/en-us/windows-8/turn-windows-features-on-off).

![zapnout nebo vypnout funkce systému Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternativně můžete povolit technologii ASP.NET 4.5 pomocí příkazového řádku.

1. Otevřete příkazový řádek v režimu správce.
2. Spusťte následující příkaz k povolení technologie ASP.NET 4.5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
