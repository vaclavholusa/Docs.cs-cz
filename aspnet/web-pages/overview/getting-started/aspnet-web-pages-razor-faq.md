---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages – nejčastější dotazy (Razor) | Microsoft Docs
author: tfitzmac
description: V tomto článku jsou uvedené některé nejčastější dotazy týkající se rozhraní ASP.NET Web Pages (Razor) a službě WebMatrix. Použité v kurzu ASP.NET webové stránky (R... verze softwaru
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042456"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages – nejčastější dotazy (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages. Použití [sady Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) nebo [Visual Studio Code](https://code.visualstudio.com/).
>
> V tomto článku jsou uvedené některé nejčastější dotazy týkající se rozhraní ASP.NET Web Pages (Razor) a službě WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - Služba WebMatrix 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2, služba WebMatrix 2 a Visual Studio 2012.


- [Jaký je rozdíl mezi webových stránek ASP.NET, webových formulářů ASP.NET a architektura ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [Chcete-li pracovat s webovými stránkami potřeba WebMatrix?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Můžete použít ovládací prvky webových formulářů ASP.NET na stránce webové stránky?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Je nutné použít pomocnou metodou WebSecurity pro podporu přihlášení?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Podporuje rozhraní ASP.NET Web Pages HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Můžete použít JavaScript a jQuery s webovými stránkami?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Další zdroje informací](#AdditionalResources)

Otázky týkající se chyby a další problémy, najdete v článku [Průvodce odstraňováním potíží technologie ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Jaký je rozdíl mezi webových stránek ASP.NET, webových formulářů ASP.NET a architektura ASP.NET MVC?

Všechny tři jsou technologie ASP.NET pro vytváření dynamické webové aplikace:

- ASP.NET – webové stránky se zaměřuje na přidání stránky HTML a funkce jednoduché a jednoduché syntaxe dynamický kód (na straně serveru) a přístup k databázi.
- Webové formuláře ASP.NET je založen na modelu objektu stránky a ovládací prvky tradiční okno – typ (tlačítka, seznamy, atd.). Webové formuláře používá model na základě událostí, který je dobře známé pro osoby, které jste pracovali na základě klienta vývoj (Windows forms).
- ASP.NET MVC implementuje vzor model-view-controller pro technologii ASP.NET. Důraz je na "oddělené oblasti zájmu" (zpracování, dat a vrstvy uživatelského rozhraní).

Všechny tři architektury jsou plně podporované a pokračovat na tým ASP.NET. Obecně platí, volba které framework použít závisí na pozadí a zkušenosti s technologií ASP.NET.

ASP.NET – webové stránky na konkrétní byla navržena pro usnadnění pro osoby, které již víte, chcete-li přidat server zpracování na jejich stránky HTML. Je vhodná pro studenty, hobbyists, obecně uživatelům, kteří jsou nové pro programování. Může být také dobrou volbou pro vývojáře, kteří mají zkušenosti s technologiemi web mimo technologii ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>Chcete-li pracovat s webovými stránkami potřeba WebMatrix?

Ne. Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages. Použití [sady Visual Studio](program-asp-net-web-pages-in-visual-studio.md) nebo [Visual Studio Code](https://code.visualstudio.com/).

Pokud nechcete použít Visual Studio nebo Visual Studio Code, můžete nainstalovat součásti produkty jednotlivě pomocí [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx). Budete potřebovat následující produkty:

- Rozhraní Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (který se nainstaluje rozhraní ASP.NET Web Pages)
- Služba IIS Express (webový server)
- Microsoft SQL Server Compact 4.0 (databáze)

Můžete pomocí textového editoru upravit *.cshtml* (nebo *.vbhtml*) stránky.

Správa databází systému SQL Server Compact (*SDF* soubory) bez nástroj je trochu složitější. Visual Studio containds nástroje pro správu *SDF* databáze. Můžete také spustit příkazy SQL v kódu k provádění úloh správy systému SQL Server.

K testování *.cshtml* stránky bez použití integrované vývojové prostředí (IDE), je můžete nasadit na aktivní server. (Viz [můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Spuštění služby IIS Express bez použití IDE

Při instalaci služby IIS Express na počítače jako webový server, můžete použít k testování stránek. Můžete spustit službu IIS Express z příkazového řádku a přidružte ji k určité číslo portu. Pak zadejte tento port při žádosti o *.cshtml* soubory v prohlížeči.

V systému Windows, otevřete příkazový řádek s oprávněními správce a změňte na *C:\Program Files\IIS Express.* (Pro 64bitové systémy, použijte složku *Express \IIS C:\Program Files (x86).)* Potom zadejte následující příkaz, pomocí skutečné cesty na váš web:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Můžete použít libovolné číslo portu, který není rezervovaná jiný proces. (Čísla portů nad 1024 jsou obvykle volné). Pro `path` hodnoty, použijte cestu ke složce webu kde *.cshtml* jsou soubory.

Po spuštění tohoto příkazu k nastavení služby IIS Express k obsluze svoje stránky, můžete otevřít prohlížeč a vyhledejte *.cshtml* souboru. Použijte adresu URL podobnou následující:

`http://localhost:35896/default.cshtml`

Nápovědu služby IIS Express možnosti příkazového řádku, zadejte `iisexpress.exe /?` na příkazovém řádku.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Můžete použít ovládací prvky webových formulářů ASP.NET na stránce webové stránky?

Ne. Ovládací prvky webových formulářů jako [políčko](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) ovládací prvek, [ovládací prvky pro ověřování](https://msdn.microsoft.com/library/bwd43d0x)a [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) řízení fungovat pouze v stránky webových formulářů (*.aspx* soubory). Tyto ovládací prvky vyžadují rozhraní stránky webových formulářů.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?

Ano. Soubory webové stránky můžete ručně zkopírovat na server (obvykle pomocí FTP). Pokud provádíte ruční kopírování, máte také kopírovat soubory, které podporují systém SQL Server Compact (databáze). Podrobnosti najdete v tématu v položce blogu [nasazení webové stránky aplikace bez nástroj](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Je nutné použít pomocnou metodou WebSecurity pro podporu přihlášení?

Ne. `SimpleMembership` Zprostředkovatele, který je součástí z webové stránky ASP.NET je jednou z možností. Zprostředkovatele zabezpečení, které jsou součástí technologie ASP.NET, (která je lze použít pro práci s v webových formulářů) jsou k dispozici. Například můžete ověřování pomocí formulářů na webových stránkách ASP.NET stejně jako v webových formulářů. Jedním z příkladů použití ověřování pomocí formulářů, najdete v části článek Microsoft Support [jak implementovat ověřování ve vaší aplikaci technologie ASP.NET pomocí C# .NET](https://support.microsoft.com/kb/301240). Chcete-li stáhnout jednoduchý příklad, přečtěte si téma [verzi ASP.NET "přihlášení &amp; heslo](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Informace o tom, jak použít ověřování systému Windows, naleznete v příspěvku blogu [ověřování pomocí systému Windows na webových stránkách ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Podporuje rozhraní ASP.NET Web Pages HTML5?

Ano. Na stránkách můžete vytvořit pomocí rozhraní ASP.NET Web Pages (*.cshtml* nebo *.vbhtml* stránky) jsou v podstatě stránky HTML, které také obsahovat kód, který běží na serveru před vykreslením stránky. Tak dlouho, dokud prohlížeče uživatele podporuje HTML5, můžete použít prvků HTML5 v *.cshtml* nebo *.vbhtml* stránky.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Můžete použít JavaScript a jQuery s webovými stránkami?

Absolutně. Na stránkách můžete vytvořit pomocí rozhraní ASP.NET Web Pages (*.cshtml* nebo *.vbhtml* stránky) jsou právě HTML stránky s kódem serveru v nich. Proto nic můžete provést na normální stránce HTML pomocí pomocí jazyka JavaScript nebo jQuery můžete provést také *.cshtml* nebo *.vbhtml* stránky.

**Starter Site** šablony ve službě WebMatrix obsahuje řadu knihovny jQuery. Pokud vytvoříte pomocí této šablony, web *skripty* složka obsahuje základní knihovna jQuery (*jquery 1.6.2.js)* a knihovny pro ověřování jQuery (*jquery.validate.js*atd.).

Zde jsou některé příspěvky blogu, které ilustrují způsoby použití jQuery s webových stránek ASP.NET:

- [Přidání jQuery přesnosti pro webové stránky ASP.NET pomocí služby WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) podle Rachel Appel
- [5 minut: WebMatrix + jQuery UI + json + jQuery šablony](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) podle Jonas Eriksson
- [Služba WebMatrix a formulářů jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) podle Brind Jan

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Další prostředky


[Webové stránky ASP.NET (Razor) – průvodce řešením potíží](https://go.microsoft.com/fwlink/?LinkId=253001)

[Fórum pro službu WebMatrix a webové stránky ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) na webu technologie ASP.NET
