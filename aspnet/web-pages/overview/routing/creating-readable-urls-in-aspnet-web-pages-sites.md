---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: Vytváření čitelné adresy URL v rozhraní ASP.NET Web Pages lokalit (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje směrování v webu technologie ASP.NET Web Pages (Razor) a jak to vám umožní používat adresy URL, které jsou čitelnější a lepší pro SEO. Co budete...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26573229"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>Vytváření čitelné adresy URL v lokalitách rozhraní ASP.NET Web Pages (Razor)
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje směrování v webu technologie ASP.NET Web Pages (Razor) a jak to vám umožní používat adresy URL, které jsou čitelnější a lepší pro SEO.
> 
> Získáte informace:
> 
> - Jak technologie ASP.NET používá směrování vám umožňují používat více čitelný a dohledatelné adresy URL.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


## <a name="about-routing"></a>O směrování

Adresy URL pro stránky ve vaší lokalitě může mít dopad na tom, jak dobře funguje webu. Adresa URL to je &quot;popisný&quot; můžete bylo snazší pro osoby využití serveru. Může také pomoct s optimalizací vyhledávání (vyhledávací weby SEO) pro lokalitu. Weby technologie ASP.NET, zahrnují možnost automaticky používat přátelské adresy URL.

Technologie ASP.NET umožňuje vytvářet smysluplný adresy URL, které popisují akce uživatelů místo právě odkazující na soubor na serveru. Zvažte tyto adresy URL pro fiktivní blogu:

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

Porovnejte tyto adresy URL k těm, které jsou následující:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

V první pár uživatel být vědět, že se zobrazí na blogu pomocí *blog.cshtml* stránky a by bylo nutné vytvořit řetězec dotazu, který získá správné kategorie nebo rozsah. Druhá sada příklady je mnohem snazší pochopit a vytvářet.

Adresy URL v prvním příkladu také přejděte přímo na konkrétní soubor (*blog.cshtml*). Pokud z nějakého důvodu blogu byly přesunuty do jiné složky na serveru, nebo pokud byly na blogu přepsaná na jinou stránku, odkazy by být nesprávné. Druhá sada adresy URL neodkazuje na konkrétní stránky, tak i v případě, že změní implementace blogu nebo umístění, adresy URL by nadále platné.

Na webových stránkách ASP.NET, můžete vytvořit příjemnější adresy URL stejně jako v předchozích příkladech vzhledem k tomu, že technologie ASP.NET používá *směrování*. Směrování vytvoří logické mapování z adresy URL stránky (nebo stránky), která může splnění požadavku. Vzhledem k tomu, že je mapování logické (ne fyzických, do určitého souboru), směrování poskytuje flexibilitu v tom, jak definovat adresy URL pro váš web.

## <a name="how-routing-works"></a>Jak funguje směrování

Když ASP.NET zpracovává žádost, načte adresu URL určit, jak ho směrovat. Technologie ASP.NET pokusí vyhledat jednotlivých segmentů adresy URL pro soubory na disku, budete zleva doprava. Pokud je nalezena shoda, nic zbývající v adrese URL je předána na stránku jako *informace o cestě*.

Představte si, že někdo požádá tuto adresu URL:

`http://www.contoso.com/a/b/c`

Hledání přejde takto:

1. Je k dispozici soubor s cesta a název */a/b/c.cshtml*? Pokud ano, spusťte této stránce a předat žádné informace. V opačném případě...
2. Je k dispozici soubor s cesta a název */a/b.cshtml*? Pokud ano, spusťte tuto stránku a předejte hodnotu `c` k němu. V opačném případě...
3. Je k dispozici soubor s cesta a název */a.cshtml*? Pokud ano, spusťte tuto stránku a předejte hodnotu `b/c` k němu.

Pokud hledání najít žádné přesně odpovídá pro *.cshtml* soubory v jejich zadané složky ASP.NET pokračuje v hledání pro tyto soubory pak:

1. */a/b/c/default.cshtml* (žádné informace o cestě).
2. */a/b/c/index.cshtml* (žádné informace o cestě).

> [!NOTE]
> Být jasné, požadavky na konkrétní stránky (to znamená, požadavků, které zahrnují *.cshtml* příponu názvu souboru) pracovat stejně, jako byste očekávali. Žádost o jako `http://www.contoso.com/a/b.cshtml` spustí stránce *b.cshtml* stejně dobře.


Uvnitř stránky, můžete získat informace o cestě prostřednictvím stránky `UrlData` vlastnost, která je slovník. Představte si, že máte soubor s názvem *ViewCustomers.cshtml* a váš web získá tuto žádost:

`http://mysite.com/myWebSite/ViewCustomers/1000`

Jak je popsáno v výše uvedených pravidel, požadavek bude přejděte na stránku. Uvnitř stránky, můžete použít k získání a zobrazení informací o cestě kód jako následující (v tomto případě hodnota &quot;1000&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> Protože směrování nebude zahrnovat názvy dokončení souborů, může být nejednoznačnosti Pokud máte stránky, které mají stejný název přípony názvu souborů, ale odlišným (například *MyPage.cshtml* a *MyPage.html*) . Aby se zabránilo problémům s směrování, je nejvhodnější zajistit, že nemáte stránky ve vaší lokalitě, jejichž názvy se liší pouze v jejich rozšíření.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky

[Služba WebMatrix - adresy URL, UrlData a směrování pro SEO](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO). Tuto položku blogu podle Karel Brind poskytuje některé další podrobnosti o tom, jak směrování funguje na webových stránkách ASP.NET.
