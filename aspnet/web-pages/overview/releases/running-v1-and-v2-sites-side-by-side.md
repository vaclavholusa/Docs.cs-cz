---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: "S různými verzemi technologie ASP.NET Web Pages (Razor) vedle sebe | Microsoft Docs"
author: tfitzmac
description: "Tento článek vysvětluje, jak spustit weby technologie ASP.NET Web Pages (Razor) na stejném počítači nebo serveru, když k webům, které jsou nakonfigurovány pro použití různých verzí..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: c11399b0bde59d18fa378ed48c15844454c1f956
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>S různými verzemi ASP.NET Web Pages (Razor) vedle sebe
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

> Tento článek vysvětluje, jak spustit weby technologie ASP.NET Web Pages (Razor) na stejném počítači nebo serveru, když k webům, které jsou nakonfigurovány pro použití různých verzí z webové stránky ASP.NET.
> 
> Získáte informace:
> 
> - Výchozí chování Novinky v technologii ASP.NET Pokud máte webů vytvořených pomocí technologie ASP.NET Web Pages.
> - Postup konfigurace nové lokality ke spuštění ze starší verze z technologie ASP.NET Web Pages.
>   
> 
> Toto je funkce technologie ASP.NET byla zavedená v článku:
> 
> - `webPages:Version` Nastavení konfigurace.
>   
> 
> ## <a name="software-versions"></a>Verze softwaru
> 
> 
> - Rozhraní ASP.NET Web Pages (Razor) 3
>   
> 
> V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2 a ASP.NET Web Pages 1.0.


ASP.NET – webové stránky podporuje možnost spouštění weby vedle sebe. Díky tomu můžete i nadále spouštět starší aplikace technologie ASP.NET Web Pages, vytvářet nové aplikace ASP.NET Web Pages a spustit všechny z nich na stejném počítači.

Zde jsou některé věci pamatovat při instalaci webových stránek pomocí služby WebMatrix:

- Ve výchozím nastavení bude existující webové stránky aplikace v počítači spustit jako na nejnovější verzi. (Sestavení jsou nainstalované v globální mezipaměti sestavení (GAC) a používají automaticky.)
- Pokud chcete spustit lokalitě v jiné verzi z webových stránek ASP.NET, můžete nakonfigurovat k tomuto webu. Pokud váš web již nemá *web.config* souboru v kořenovém adresáři serveru, vytvořte novou a zkopírujte následující kód XML do ní, přepsání existujícího obsahu. Pokud lokalita již obsahuje *web.config* soubor, přidejte `<appSettings>` element stejný, jako je následující k `<configuration>` oddílu.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
' – Pokud nezadáte verzi v *web.config* soubor, lokalitu je nasadit jako na nejnovější verzi. (Sestavení se zkopírují do *bin* složku v nasazené lokalitě.)
- Nové aplikace, které vytvoříte pomocí šablony webů v matici webové zahrnují verze sestavení webové stránky na webu *bin* složky.

Obecně platí, můžete vždy řídit kterou verzi webové stránky pro použití s vaší lokality pomocí NuGet pro instalaci příslušné sestavení do lokality *bin* složky. Balíčky naleznete [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Další prostředky

[Hlavní funkce v rozhraní ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
