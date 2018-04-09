---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Přidání dynamický obsah v mezipaměti stránku (C#) | Microsoft Docs
author: microsoft
description: Zjistěte, jak kombinovat dynamické a uložené v mezipaměti obsahu na stejné stránce. Nahrazení po mezipaměti umožňuje zobrazit dynamický obsah, jako je například o banner oznámení o inzerovaném programu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f91cc07bc531cfb3edf577ab79e91fd94a57a3c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Přidání dynamický obsah v mezipaměti stránku (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak kombinovat dynamické a uložené v mezipaměti obsahu na stejné stránce. Nahrazení po mezipaměti umožňuje zobrazit dynamický obsah, jako je například oznámení o inzerovaném programu informační zpráva nebo zprávy položky v rámci stránky, který se má byla výstupu do mezipaměti.


Využití výhod ukládání výstupu do mezipaměti, můžete výrazně zlepšit výkon aplikace ASP.NET MVC. Místo obnovuje na stránce každé, když je zobrazení stránky vyžadováno, může generovat jednou stránky a uložené v mezipaměti v paměti pro více uživatelů.

Ale došlo k potížím. Co když je třeba zobrazit dynamický obsah na stránce? Představte si například, že chcete zobrazit oznámení informační zpráva na stránce. Nechcete, aby inzerování banner ukládat do mezipaměti, aby každý uživatel vidí velmi stejné inzerování. Tímto způsobem by nedávalo peníze!

Naštěstí je snadné řešení. Můžete využít výhod funkce rozhraní ASP.NET volat *substituce mezipaměti*. Nahrazení po mezipaměti umožňuje nahraďte dynamický obsah na stránce, který byl uložený do mezipaměti v paměti.


Za normálních okolností při výstupu mezipaměti na stránce pomocí atributu [OutputCache], stránky se uloží do mezipaměti na serveru a klienta (webový prohlížeč). Při použití mezipaměti po nahrazení stránky se uloží do mezipaměti pouze na serveru.


#### <a name="using-post-cache-substitution"></a>Použití mezipaměti po nahrazení

Použití mezipaměti po nahrazení vyžaduje dva kroky. Nejprve musíte zadat metodu, která vrátí řetězec, který představuje dynamický obsah, který chcete zobrazit na stránce v mezipaměti. Pak zavolejte metodu HttpResponse.WriteSubstitution() vložení dynamický obsah na stránku.

Představte si například, že chcete náhodně zobrazit různé příspěvků na stránce v mezipaměti. Třída v výpis 1 zpřístupňuje jedinou metodu s názvem RenderNews(), který náhodně vrací jednu položku zpráv ze seznamu tři položky zprávy.

**Výpis 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Abyste mohli využívat mezipaměti po nahrazení, zavolejte metodu HttpResponse.WriteSubstitution(). Metoda WriteSubstitution() nastaví kód tak, aby oblasti v mezipaměti stránku nahraďte dynamický obsah. Metoda WriteSubstitution() slouží k zobrazení položky náhodných zpráv v zobrazení v výpis 2.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Metoda RenderNews je předaný metodě WriteSubstitution(). Všimněte si, že není volána metoda RenderNews (existují zde bez závorek). Místo toho je předán odkaz na metodu WriteSubstitution().

Zobrazení Index se uloží do mezipaměti. Zobrazení je vrácený řadič v výpis 3. Všimněte si, že akce Index() opatřen atributem [OutputCache], který způsobí, že zobrazení indexu ukládat do mezipaměti po dobu 60 sekund.

**Výpis 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

Přestože zobrazení Index se uloží do mezipaměti, položky různé náhodné zprávy se zobrazují, když požádáte o indexovou stránku. Pokud budete požadovat indexovou stránku, nezmění se zobrazuje stránkou po dobu 60 sekund (viz obrázek 1). Skutečnost, že čas nezmění prokáže, že stránky se uloží do mezipaměti. Ale obsah vloženy změny metoda – položku náhodných zpráv – WriteSubstitution() spolu s každou žádostí.

**Obrázek 1 – vložení dynamické příspěvků na stránce v mezipaměti**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Použití mezipaměti po nahrazení v pomocné metody

Snadný způsob, jak využít výhod nahrazení po mezipaměti je zapouzdření volání metody WriteSubstitution() v rámci vlastní Pomocná metoda. Tento přístup je zobrazená metodou helper výpis 4.

**Výpis 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Výpis 4 obsahuje statická třída, která zveřejňuje dvě metody: RenderBanner() a RenderBannerInternal(). Metoda RenderBanner() představuje skutečný pomocnou metodu. Tato metoda rozšiřuje standardní třída ASP.NET MVC HtmlHelper, tak, aby Html.RenderBanner() můžete volat v zobrazení stejně jako další metodu helper.

RenderBanner() metoda volá metodu HttpResponse.WriteSubstitution() předávání metodu RenderBannerInternal() metodu WriteSubsitution().

Metoda RenderBannerInternal() je soukromá metoda. Tato metoda nebude zpřístupněná jako metodu helper. Metoda RenderBannerInternal() náhodně vrátí jeden image banner oznámení o inzerovaném programu ze seznamu obrázků oznámení o inzerovaném programu tři hlavičky.

Upravené zobrazení indexu v výpis 5 ukazuje, jak je možné používat RenderBanner() pomocnou metodu. Všimněte si, že další &lt;% @ Import %&gt; – direktiva je zahrnuta v horní části zobrazení pro import MvcApplication1.Helpers obor názvů. Pokud neprovedete importovat tento obor názvů, metoda RenderBanner() nezobrazí jako metodu pro vlastnost Html.

**Výpis 5 – Views\Home\Index.aspx (s RenderBanner() metoda)**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Při žádosti o stránku pro vykreslení zobrazení v výpis 5 inzerování různých hlavička se zobrazí spolu s každou žádostí (viz obrázek 2). Stránky se uloží do mezipaměti, ale hlavička oznámení je vloženy dynamicky RenderBanner() pomocnou metodu.

**Obrázek 2 – zobrazení Index zobrazení náhodných banner inzerování**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Souhrn

V tomto kurzu vysvětlení, jak můžete dynamicky aktualizovat obsah v mezipaměti stránky. Jste zjistili, jak lze pomocí této metody HttpResponse.WriteSubstitution() Povolit dynamický obsah vložit v stránky v mezipaměti. Také jste zjistili, jak má být zapouzdřena volání metody WriteSubstitution() v rámci metodu helper HTML.

Využít výhod ukládání do mezipaměti, kdykoli je to možné – ho může mít výrazný dopad na výkon webových aplikací. Jak je popsáno v tomto kurzu, můžete využít výhod ukládání do mezipaměti i v případě, že je třeba zobrazit dynamický obsah na stránkách.

## 

## 

> [!div class="step-by-step"]
> [Předchozí](improving-performance-with-output-caching-cs.md)
> [další](creating-a-controller-cs.md)
