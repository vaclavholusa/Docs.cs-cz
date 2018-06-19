---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Instalace pomocné rutiny v rozhraní ASP.NET Web Pages lokality (Razor) | Microsoft Docs
author: tfitzmac
description: Tento článek popisuje postup instalace pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). Pomocné rutiny je opakovaně použitelné komponenty, která obsahuje kód a kód do za...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 766fbb87ae8bcb8917eb8fa7f552c00792242cf6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
ms.locfileid: "30896773"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Instalace pomocné rutiny v Web Pages (Razor) technologie ASP.NET
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek popisuje postup instalace pomocné rutiny na webu technologie ASP.NET Web Pages (Razor). A *pomocná* je znovu použitelné komponentní, která obsahuje kód a značky provést úlohu, která může být zdlouhavé nebo komplexní.
> 
> Získáte informace:
> 
> - Postup instalace pomocné rutiny na webové stránce vytvořené pomocí služby WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Služba WebMatrix 3


## <a name="overview-of-helpers"></a>Přehled pomocné rutiny

Některé úlohy, které uživatelé často chtějí provést na webových stránkách vyžadovat velké množství kódu nebo vyžadovat další znalostní báze. Mezi příklady patří zobrazení grafu pro data; ukládání tlačítko "Následovat" Twitter na stránku; odesílání e-mailu z webu; oříznutí nebo změna velikosti obrázků; pomocí služby PayPal pro váš web. Chcete-li snadné provést tyto druhy věcí, rozhraní ASP.NET Web Pages vám umožní používat *Pomocníci*. Pomocné rutiny jsou součásti nainstalovat pro lokalitu a, ve kterých je typické úlohy provádět pomocí právě řádku nebo dva kódu Razor.

ASP.NET – webové stránky má několik Pomocníci součástí. Mnoho pomocné rutiny jsou však k dispozici v balíčcích (doplňky), které jsou k dispozici pomocí Správce balíčků NuGet. NuGet umožňuje vybrat balíček pro instalaci a pak se stará o všechny podrobnosti instalace.

## <a name="installing-a-helper-in-webmatrix-3"></a>Instalace pomocné rutiny ve službě WebMatrix 3

1. Ve službě WebMatrix 3, klikněte na **NuGet** tlačítko.

    ![Dialogové okno Galerie NuGet ve službě WebMatrix](installing-helpers/_static/image1.png)
2. Tím se spustí Správce balíčků NuGet a zobrazí se dostupné balíčky. Do vyhledávacího pole zadejte klíčové slovo pomocné rutiny, které chcete nainstalovat.

    ![Dialogové okno Galerie NuGet ve službě WebMatrix](installing-helpers/_static/image2.png)
3. Vyberte balíček a pak klikněte na tlačítko **nainstalovat**. Klikněte na tlačítko **Ano** když se zobrazí dotaz, pokud chcete nainstalovat balíček a uvést, že souhlasíte s podmínkami.

     Pokud je to první, kdy jste nainstalovali pomocné rutiny, vytvoří NuGet složek ve vašem webu pro kód, který tvoří pomocné rutiny.
4. Chcete-li odinstalovat pomocné rutiny, klikněte na tlačítko **Galerie** tlačítko, klikněte na tlačítko **nainstalovaná** kartě a vyberte balíček, který chcete odinstalovat.

## <a name="installing-the-twitter-helper"></a>Instalace Pomocníka služby Twitter.

Nejnovější verzi rozhraní API služby Twitter není kompatibilní s Pomocník Twitter, který nainstalujete prostřednictvím balíčku NuGet. Místo toho přejděte [Pomocník Twitter pomocí služby WebMatrix](twitter-helper.md) najdete informace o tom, jak nastavit Pomocník Twitter ve vašem projektu.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Další prostředky


[Představení ASP.NET Web Pages 2 – základy programování](../getting-started/introducing-razor-syntax-c.md)

[Pomocník Twitter pomocí služby WebMatrix](twitter-helper.md)
