---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Vykreslování rozhraní ASP.NET Web Pages (Razor) lokality pro mobilní zařízení | Microsoft Docs
author: tfitzmac
description: 'Tento článek popisuje postup vytváření stránek v serveru technologie ASP.NET Web Pages (Razor), který vykreslí správně na mobilních zařízeních. Co se dozvíte: jak můžete...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 641798c5b835be959d02dd0d854b61ca21d83016
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897316"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Vykreslování ASP.NET – webové stránky (Razor) servery pro mobilní zařízení
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje postup vytváření stránek v serveru technologie ASP.NET Web Pages (Razor), který vykreslí správně na mobilních zařízeních.
> 
> Získáte informace:
> 
> - Jak používat zásady vytváření názvů určíte, že na stránce je určený speciálně pro mobilní zařízení.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2.


ASP.NET – webové stránky umožňuje vytvářet vlastní zobrazení pro vykreslování obsah na mobilní telefon nebo jiné zařízení.

Nejjednodušší způsob, jak vytvořit stránku specifické pro zařízení v stránku webové stránky ASP.NET je pomocí vzor pojmenovávání souborů takto: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Můžete vytvořit dvě verze stránky (například jednu s názvem <em>MyFile.cshtml</em> a jednu s názvem <em>MyFile.Mobile.cshtml</em>). V době, kdy požadavky na mobilní zařízení spuštění <em>MyFile.cshtml</em>, ASP.NET vykreslí obsah z <em>MyFile.Mobile.cshtml</em>. V opačném <em>MyFile.cshtml</em> je vykreslen.

Následující příklad ukazuje, jak můžete povolit mobilní vykreslování přidáním stránky obsahu pro mobilní zařízení. *Page1.cshtml* obsahuje obsah a bočním panelu navigace. *Page1.Mobile.cshtml* obsahuje stejný obsah, ale vynechá na bočním panelu.

1. V lokalitě služby ASP.NET Web Pages, vytvořte soubor s názvem *Page1.cshtml* a nahraďte aktuální obsah následující kód.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Vytvořte soubor s názvem *Page1.Mobile.cshtml* a nahraďte následující kód existujícího obsahu. Všimněte si, že mobilní verzi stránce vynechá části navigace pro lepší vykreslování na menší obrazovce.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Spustit prohlížeč pro stolní počítač a přejděte do *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Spustit prohlížeč pro mobilní zařízení (nebo emulátoru mobilního zařízení) a přejděte do *Page1.cshtml*. (Všimněte si, že neuvedete *.mobile.* jako součást adresy URL.) I když požadavek je pro *Page1.cshtml*, rozhraní ASP.NET vykreslí *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> K testování mobilních stránek, můžete použít simulátoru mobilních zařízení, která běží na stolním počítači. Tento nástroj umožňuje testovací webové stránky, jako by vypadal na mobilních zařízeních (to znamená, obvykle s mnohem menšími umožňuje zobrazit oblast). Jedním z příkladů simulátoru je [uživatele agenta přepínači rozšíření](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) pro aplikaci Mozilla Firefox, která vám umožňuje emulovat různé mobilní prohlížeče z plochy verzi Firefox.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Emulátor Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
