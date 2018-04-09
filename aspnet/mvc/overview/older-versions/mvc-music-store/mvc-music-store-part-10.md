---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 'Část 10: Poslední aktualizace navigace a návrhu webu, uzavření | Microsoft Docs'
author: jongalloway
description: Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 10 obsahuje poslední aktualizace navigaci a S....
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>Část 10: Poslední aktualizace navigace a návrhu webu, uzavření
====================
podle [Jon Galloway](https://github.com/jongalloway)

> Úložiště Hudba MVC je kurz aplikace, která představuje a vysvětluje krok za krokem, jak používat rozhraní ASP.NET MVC a Visual Studio pro vývoj webů.  
>   
> Úložiště Hudba MVC je implementace úložiště lightweight ukázkové, který prodává hudebních alb online a implementuje základní Správa serveru, přihlášení uživatele a nákupního košíku funkce.  
>   
> Tento kurz řady podrobnosti všechny kroky k vytvoření ukázkové aplikace ASP.NET MVC Hudba úložiště. Část 10 obsahuje poslední aktualizace navigační a návrhu webu, uzavření.


Dokončení všechny hlavní funkce pro náš web, ale stále k dispozici některé funkce pro přidání do webové navigace, domovské stránky a stránky procházet úložiště.

## <a name="creating-the-shopping-cart-summary-partial-view"></a>Vytváření nákupní košík souhrnné částečné zobrazení

Chceme zpřístupnit počet položek v nákupní košík uživatele celého webu.

![](mvc-music-store-part-10/_static/image1.png)

Jsme to snadno implementovat vytvořením částečné zobrazení, která se přidá do našich Site.master.

Jak je uvedeno dříve, zahrnuje řadičem ShoppingCart CartSummary metoda akce, která vrátí hodnotu částečné zobrazení:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

Chcete-li vytvořit CartSummary částečné zobrazení, klikněte pravým tlačítkem na složku, zobrazení nebo ShoppingCart a vyberte Přidat zobrazení. Název zobrazení CartSummary a zaškrtnutím políčka "Vytvořit částečné zobrazení", jak je uvedeno níže.

![](mvc-music-store-part-10/_static/image2.png)

Částečné zobrazení CartSummary skutečně jednoduché – je právě odkaz na zobrazení ShoppingCart Index, který zobrazuje počet položek v košíku. Kompletní kód CartSummary.cshtml vypadá takto:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

Do libovolné stránce webu, včetně hlavním lokality pomocí metody Html.RenderAction jsme můžete zahrnout částečné zobrazení. RenderAction vyžaduje nám zadejte název akce ("CartSummary") a názvu Kontroleru ("ShoppingCart") jako níže.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

Před přidáním to k webu rozložení, vytvoříme i v nabídce Genre, abychom mohli provádět všechny aktualizace našich Site.master najednou.

## <a name="creating-the-genre-menu-partial-view"></a>Vytváření Genre částečné zobrazení nabídky

Jsme můžete bylo mnohem snazší pro naši uživatelé procházet úložišti přidáním Genre nabídky, které jsou uvedeny všechny žánry k dispozici v našem úložišti.

![](mvc-music-store-part-10/_static/image3.png)

Jsme bude postupovat podle stejné kroky také vytvořit částečné zobrazení GenreMenu a pak můžete přidáme oba k hlavnímu serveru lokality. Nejprve přidejte následující akce kontroleru GenreMenu StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

Tato akce vrátí seznam hodnot žánry, které se zobrazí v částečné zobrazení, které vytvoříme Další.

*Poznámka: Jsme přidali atribut [ChildActionOnly] s touto akcí kontroleru, který označuje, že chceme jenom této akci je možné použít částečné zobrazení. Tento atribut bude brání provedení akce kontroleru vykonáván procházením /Store/GenreMenu. Není to povinné pro částečné zobrazení, ale je dobrým zvykem, vzhledem k tomu, že chceme, abyste měli jistotu, že naše akce kontroleru se používají jako plánujeme. Také jsme se vrací PartialView spíše než zobrazení, která umožní zobrazovací modul vědět, že nepoužívejte rozložení pro toto zobrazení, jako je nebudou zahrnuty v ostatních zobrazeních.*

Klikněte pravým tlačítkem na akce kontroleru GenreMenu a vytvořit částečné zobrazení s názvem GenreMenu, což je silného typu pomocí třídy Genre zobrazení dat, jak je uvedeno níže.

![](mvc-music-store-part-10/_static/image4.png)

Aktualizujte kód zobrazení pro částečné zobrazení GenreMenu a zobrazit seznam položek pomocí neuspořádaný seznam následujícím způsobem.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>Aktualizace lokality rozložení-li zobrazit naše částečné zobrazení

Přidáme naše částečné zobrazení k rozložení lokality (nebozobrazení/sdílené/\_Layout.cshtml) voláním Html.RenderAction(). Přidáme je obě, jakož i některé další značek k zobrazení, jak je uvedeno níže:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

Nyní když jsme aplikaci spustit, jsme se zobrazí souhrn košíku v horní části a Genre v levé navigační oblasti.

## <a name="update-to-the-store-browse-page"></a>Aktualizovat na stránce úložiště Procházet

Stránka procházet úložiště je funkční, ale nevypadá velmi vhodné. Aktualizujeme stránku můžete zobrazit alb v lepší rozložení aktualizací zobrazení kód (nalezené v /Views/Store/Browse.cshtml) následujícím způsobem:

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

Zde vydáváme používat Url.Action než Html.ActionLink tak, aby jsme můžete použít zvláštní formátování odkaz zahrnout kresby alb.

*Poznámka: Jsme se zobrazení Obecné alb pro tyto alb. Tyto informace jsou uloženy v databázi a upravovat přes Správce úložiště. Jste Vítejte přidat vlastní kresby.*

Nyní když jsme vyhledejte Genre, jsme se zobrazí alb v mřížce s kresby alb.

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>Aktualizace na domovskou stránku a zobrazí prodejní alb horní

Chceme funkci Naše nejlépe prodávané alb na domovské stránce zvýšit prodej. Naše HomeController ke zpracování a přidat některé další grafického jsme budete provádět některé aktualizace.

Nejprve přidáme navigační vlastnosti do našich Album třídy tak, aby EntityFramework ví, že jsou přidružené. Posledních několika řádků naše **Album** třída by teď měl vypadat takto:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*Poznámka: To bude vyžadovat přidání pomocí příkazu mají být předány System.Collections.Generic – obor názvů.*

Nejprve přidáme pole storeDB a MvcMusicStore.Models příkazy, jako v našich ostatních řadičů. Potom přidáme metodu HomeController, který se dotazuje databáze najít nejvyšší prodejní alb podle Rozpis objednávek.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

To je soukromá metoda, protože jsme nechcete zpřístupní ji jako akce kontroleru. Jsme včetně v HomeController pro jednoduchost, ale doporučujeme přesunout obchodní logiku do třídy samostatné služby podle potřeby.

S třídou na místě budeme aktualizovat Index akce kontroleru pro dotazování top 5 prodávané alb a vrátí je do zobrazení.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

Kód dokončení pro aktualizovaný HomeController je, jak je uvedeno níže.

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

Nakonec jsme budete muset aktualizovat naše Domů Index zobrazení tak, aby ho můžete zobrazit seznam alb aktualizace typu modelu a přidáním seznamu alb do dolní části. Tato možnost také přidat nadpis a povýšení část na stránku jsme bude trvat.

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

Nyní když jsme aplikaci spustit, uvidíme aktualizované domovské stránky s nejvyšší prodejní alb a naše propagační zprávou.

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>Závěr

Jste viděli, že rozhraní ASP.NET MVC lze snadno vytvořit sofistikované web s přístupem k databázi, členství, AJAX, atd. poměrně rychle. Zpravidla v tomto kurzu jste obdrželi nástroje, které potřebujete, abyste mohli začít vytváření vlastní rozhraní ASP.NET MVC aplikací!


> [!div class="step-by-step"]
> [Předchozí](mvc-music-store-part-9.md)
