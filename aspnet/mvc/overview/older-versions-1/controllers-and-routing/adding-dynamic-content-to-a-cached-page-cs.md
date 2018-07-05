---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: Přidání dynamického obsahu do stránky v mezipaměti (C#) | Dokumentace Microsoftu
author: microsoft
description: Zjistěte, jak kombinovat dynamická a uložená v mezipaměti obsahu na stejné stránce. Substituce mezipaměti po umožňuje zobrazit dynamický obsah, jako je například o oznámení o inzerovaných programech banner...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 08f680e8d057f47a3f2802b1136edfb00634637d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37364142"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>Přidání dynamického obsahu do stránky v mezipaměti (C#)
====================
podle [Microsoft](https://github.com/microsoft)

> Zjistěte, jak kombinovat dynamická a uložená v mezipaměti obsahu na stejné stránce. Substituce mezipaměti po umožňuje zobrazit dynamický obsah, jako je například reklamy nebo příspěvků v rámci stránky, který má výstup do mezipaměti.


Výhod ukládání výstupu do mezipaměti, může výrazně zlepšit výkon aplikace ASP.NET MVC. Na stránce může místo obnovení na stránce každého, když je zobrazení stránky vyžadováno, generovat jednou a uložit do mezipaměti v paměti pro více uživatelů.

Ale dojde k nějakému problému. Co když budete potřebovat zobrazit dynamický obsah na stránce? Představte si například, že chcete zobrazit na stránce proužkové reklamy. Nechcete, aby oznámení o inzerovaném programu banner ukládat do mezipaměti tak, aby každý uživatel uvidí stejné oznámení o inzerovaném programu. Nebylo by si peníze za tímto způsobem.

Naštěstí je jednoduché řešení. Můžete využít funkci ASP.NET Framework, volá *substituce mezipaměti*. Substituce mezipaměti po umožňuje nahradit dynamický obsah na stránce, která se má uložit do mezipaměti v paměti.


Za normálních okolností při výstupní mezipaměť stránku pomocí atributu [OutputCache], na stránce je do mezipaměti na serveru a klienta (webový prohlížeč). Při použití mezipaměti po nahrazení stránka je uložit do mezipaměti pouze na serveru.


#### <a name="using-post-cache-substitution"></a>Použití mezipaměti po nahrazení

Použití mezipaměti po nahrazení sestává ze dvou kroků. Nejprve budete muset definovat metodu, která vrací řetězec představující dynamický obsah, který chcete zobrazit stránky v mezipaměti. Pak zavolejte metodu HttpResponse.WriteSubstitution() vkládat dynamického obsahu do stránky.

Představte si například, že chcete náhodně zobrazení položek různé informační v stránky v mezipaměti. Třída v informacích 1 poskytuje jedinou metodu s názvem RenderNews(), který náhodně vrátí jednu položku zprávy ze seznamu položek zpráv tři.

**Výpis 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

Výhod substituce mezipaměti po volání metody HttpResponse.WriteSubstitution(). Metoda WriteSubstitution() nastaví kód k nahrazení oblast stránky v mezipaměti s dynamickým obsahem. Metoda WriteSubstitution() slouží k zobrazení náhodných příspěvek v zobrazení na výpis 2.

**Listing 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

Metoda RenderNews se předá metodě WriteSubstitution(). Všimněte si, že není volána metoda RenderNews (neexistují žádné závorek). Místo toho je předán odkaz na metodu WriteSubstitution().

Zobrazení Index se uloží do mezipaměti. Zobrazení je vrácený řadič v informacích 3. Všimněte si, že je akce Index() opatřen atributem [OutputCache], který způsobí, že zobrazení indexu ukládat do mezipaměti po dobu 60 sekund.

**Výpis 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

I v případě, že zobrazení Index se uloží do mezipaměti, se zobrazují různé náhodné informační položky při požadavku indexovou stránku. Pokud budete požadovat indexovou stránku, času zobrazeného na stránce se nezmění po dobu 60 sekund (viz obrázek 1). Skutečnost, že čas nezmění prokáže, že je do mezipaměti na stránce. Nicméně obsah vloženy WriteSubstitution() změny metody – položky náhodné zpráv – s každou žádostí.

**Obrázek 1 – vkládá dynamické příspěvků v stránky v mezipaměti**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>Použití mezipaměti po nahrazení v pomocné metody

K zapouzdření volání metody WriteSubstitution() v rámci vlastní pomocné metody je snadný způsob, jak využít výhod mezipaměti po nahrazení. Tento přístup je znázorněn ve pomocnou metodu v informacích 4.

**Část 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

Výpis 4 obsahuje statickou třídu, která poskytuje dvě metody: RenderBanner() a RenderBannerInternal(). Metoda RenderBanner() představuje skutečný pomocnou metodu. Tato metoda rozšiřuje standardní třídu ASP.NET MVC HtmlHelper, takže můžete volat Html.RenderBanner() v zobrazení stejně jako další metodu helper.

Metoda RenderBanner() volá metodu HttpResponse.WriteSubstitution() předávání metodu RenderBannerInternal() WriteSubsitution() metody.

Metoda RenderBannerInternal() je privátní metodu. Tato metoda nebude vystavena jako metoda pomocné rutiny. Metoda RenderBannerInternal() náhodně vrátí jednu image banner oznámení o inzerovaném programu ze seznamu tři Image banner oznámení o inzerovaném programu.

Upravené zobrazení indexu v informacích 5 ukazuje, jak je možné používat RenderBanner() Pomocná metoda. Všimněte si, že další &lt;% @ Import %&gt; direktiva je zahrnuté v horní části zobrazení pro import oboru názvů MvcApplication1.Helpers. Pokud opomenete importovat tento obor názvů, metoda RenderBanner() nezobrazí jako metoda na vlastnost ve formátu Html.

**Výpis 5 – Views\Home\Index.aspx (pomocí metody RenderBanner())**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

Při žádosti o stránku zpracovanou zobrazení výpisu 5 různých banner oznámení se zobrazí spolu s každou žádostí (viz obrázek 2). Na stránce se uloží do mezipaměti, ale inzerování banner se vloží dynamicky podle RenderBanner() Pomocná metoda.

**Obrázek 2 – Index zobrazení náhodných banner inzerování**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>Souhrn

Tento kurz vysvětluje, jak dynamicky aktualizovat obsah stránky v mezipaměti. Jste zjistili, jak používat metodu HttpResponse.WriteSubstitution() umožňující dynamický obsah vložit do stránky v mezipaměti. Také jste zjistili, jak k zapouzdření volání metody WriteSubstitution() v rámci metody pomocné rutiny HTML.

Využijte výhod ukládání do mezipaměti, kdykoli je to možné – může mít výrazný dopad na výkon webových aplikací. Jak je popsáno v tomto kurzu, můžete využít výhod ukládání do mezipaměti i v případě, že budete muset zobrazují dynamický obsah na stránkách.

## 

## 

> [!div class="step-by-step"]
> [Předchozí](improving-performance-with-output-caching-cs.md)
> [další](creating-a-controller-cs.md)
