---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: '10. část: Závěrečné aktualizace návrhu navigace a webu, závěr | Dokumentace Microsoftu'
author: jongalloway
description: V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. Část 10 popisuje závěrečné aktualizace navigace a S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: a5b1d02d268530d6288ed2bc502679336d06d403
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366317"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>10. část: Závěrečné aktualizace návrhu navigace a webu, závěr
====================
podle [Jon Galloway](https://github.com/jongalloway)

> MVC Music Store jde o kurz, který se seznámíte, podrobné postupy pro vývoj pro web pomocí ASP.NET MVC a sady Visual Studio.  
>   
> Music Store MVC je jednoduché ukázku implementace úložiště prodává hudebních alb online, který implementuje správu základního webu, přihlášení uživatele a nákupního košíku funkce.  
>   
> V této sérii kurzů podrobně popisuje všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Music Store. 10. část se věnuje závěrečné aktualizace návrhu navigace a webu, závěr.


Jsme pro náš web dokončili všechny hlavní funkce, ale máme některé funkce, které chcete přidat k navigaci na webu, na domovské stránce a stránce procházet Store.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Vytvoření nákupního košíku částečné zobrazení se souhrnnými

Chcete vystavit počet položek v nákupním košíku uživatele napříč celou lokalitu.

![](mvc-music-store-part-10/_static/image1.png)

Můžeme to snadno implementovat tak, že vytvoříte částečné zobrazení, která se přidá do našich Site.master.

Jak bylo uvedeno výše, zahrnuje kontroleru ShoppingCart CartSummary metody akce, které vrací částečné zobrazení:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Chcete-li vytvořit CartSummary částečné zobrazení, klikněte pravým tlačítkem na složku zobrazení/ShoppingCart a vyberte Přidat zobrazení. Název zobrazení CartSummary a zaškrtněte políčko "Vytvořit částečné zobrazení", jak je znázorněno níže.

![](mvc-music-store-part-10/_static/image2.png)

Částečné zobrazení CartSummary je opravdu jednoduché – je pouze odkaz na zobrazení ShoppingCart Index, který zobrazuje počet položek v košíku. Kompletní kód CartSummary.cshtml vypadá takto:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Na libovolné stránce na webu, včetně hlavnímu serveru pomocí metody Html.RenderAction jsme může zahrnovat částečné zobrazení. RenderAction vyžaduje, abychom mohli zadat název akce ("CartSummary") a názvu Kontroleru ("ShoppingCart") jako níže.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Před přidáním to k webu rozložení, vytvoříme i nabídce žánr proto všechny naše Site.master aktualizace najednou.

## <a name="creating-the-genre-menu-partial-view"></a>Vytváření žánr částečného zobrazení nabídky

Můžeme provádět je mnohem snazší pro naši uživatelé procházet úložiště tak, že přidáte žánr nabídky, který vypíše všechny žánry k dispozici v našem úložišti.

![](mvc-music-store-part-10/_static/image3.png)

Budeme postupovat stejný postup také vytvořit GenreMenu částečné zobrazení, a potom můžeme přidat je do hlavní větve serveru. Nejprve přidejte následující akce kontroleru GenreMenu k StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Tato akce vrátí seznam hodnot žánry, které se zobrazí podle částečné zobrazení, které budou dalších krocích vytvoříme.

*Poznámka: Jsme přidali atribut [ChildActionOnly] do této akce kontroleru, který označuje, že chceme jenom tato akce pro použití v částečné zobrazení. Tento atribut nebudou moct být prováděn tak, že přejdete do /Store/GenreMenu akce kontroleru. Není to povinné pro částečné zobrazení, ale je dobrým zvykem, protože chceme mít jistotu, že naše akce kontroleru se používají jako plánujeme. Můžeme také vrací PartialView spíše než zobrazení, které vám umožní zobrazovací modul ví, že neměli používat rozložení pro toto zobrazení, jak je zahrnuto v ostatních zobrazeních.*

Klikněte pravým tlačítkem na akce kontroleru GenreMenu a vytvořit částečné zobrazení s názvem GenreMenu, což je silně typováno pomocí třídy rozšířením podle tematických zobrazení dat, jak je znázorněno níže.

![](mvc-music-store-part-10/_static/image4.png)

Aktualizace kódu zobrazení pro částečné zobrazení GenreMenu pro zobrazení položek neuspořádaný seznam následujícím způsobem.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aktualizace rozložení webu do našich částečné zobrazení

Přidáme náš částečné zobrazení k rozložení webu (/Zobrazit/Shared/\_Layout.cshtml) voláním Html.RenderAction(). Přidáme je do, jakož i některé další značky se zobrazí, jak je znázorněno níže:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Nyní když jsme aplikaci spustit, uvidíme žánr v levé navigační oblasti a košíku v horní části.

## <a name="update-to-the-store-browse-page"></a>Aktualizovat stránku procházet Store

Stránka procházet Store je funkční, ale nevypadá velmi dobré. Abychom mohli aktualizovat stránku zobrazíte alba lepšího rozložení aktualizováním zobrazení kódu (nachází se /Views/Store/Browse.cshtml) následujícím způsobem:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Tady jsou nyní používají Url.Action spíše než Html.ActionLink, můžete použít speciální formátování na propojení zahrnout alba obrázky.

*Poznámka: Jsme zobrazujete obecný alb tyto alb. Tyto informace jsou uloženy v databázi a je možné upravovat prostřednictvím Správce Store. Určitě chcete-li přidat vlastní obrázky.*

Nyní když jsme procházet rozšířením podle tematických, uvidíme alb je znázorněno v mřížka s obrázky alba.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualizuje se na domovskou stránku a zobrazit prodej alb nahoru

Chceme, aby funkce naší nejvyšší prodeje alb na domovskou stránku a zvýšit prodej. Provedeme některé aktualizace s naší HomeController ke zpracování a přidat některé další grafiky.

Nejprve přidáme navigační vlastnost pro naše třída alba tak, aby EntityFramework ví, že jsou přidružené. Několik posledních řádků z našich **alba** třídy by teď měl vypadat takto:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Poznámka: To vyžaduje přidání pomocí příkazu pro System.Collections.Generic – obor názvů.*

Nejprve přidáme storeDB pole a MvcMusicStore.Models pomocí příkazů, stejně jako v našich řadiče. V dalším kroku přidáme následující metodu HomeController, který se dotazuje databázi najít hlavní alb prodej podle OrderDetails.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

Toto je privátní metodu, protože nechceme, aby byl k dispozici jako akce kontroleru. Jsme včetně v HomeController pro zjednodušení, ale doporučujeme přesunout vaši obchodní logiku do třídy samostatné služby podle potřeby.

To na místě abychom mohli aktualizovat Index akce kontroleru pro dotazování hlavních 5 prodej alba a vrátit do zobrazení.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Kompletní kód pro aktualizovaný HomeController je uvedeno dále.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Nakonec potřebujeme aktualizace našich zobrazení Home Index tak, aby ho můžete zobrazit seznam alb aktualizace typu modelu a přidáním do dolní části seznamu alb. My podnikneme této příležitosti a také přidat nadpis a povýšení části na stránku.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Nyní když jsme aplikaci spustit, uvidíme aktualizované domovské stránky s nejvyšší prodejní alba a naše propagační zprávy.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Závěr

Viděli jsme, že technologie ASP.NET MVC lze snadno vytvořit sofistikované web se přístup k databázi, členství, AJAX, atd. poměrně rychle. Snad v tomto kurzu, má udělené vám nástroje, které potřebujete, abyste mohli začít vytvářet své vlastní technologie ASP.NET MVC aplikace!


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-9.md)
