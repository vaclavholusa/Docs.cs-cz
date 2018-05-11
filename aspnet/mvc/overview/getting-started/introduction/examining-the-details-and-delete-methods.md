---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Zkoumání podrobností a metody odstranění | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: f534080fe9aa22eb9092932babc74c5ab96aabbf
ms.sourcegitcommit: 74be78285ea88772e7dad112f80146b6ed00e53e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/10/2018
---
<a name="examining-the-details-and-delete-methods"></a>Zkoumání podrobností a metody odstranění
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

V této části kurzu budete zkontrolujte automaticky generované `Details` a `Delete` metody.

## <a name="examining-the-details-and-delete-methods"></a>Zkoumání podrobností a metody odstranění

Otevřete `Movie` řadiče a prozkoumat `Details` metoda.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Modul generování uživatelského rozhraní MVC, který vytvořili této metodě akce přidá komentář zobrazující požadavek HTTP, která volá metodu. V takovém případě je `GET` žádost s tři segmenty adres URL, `Movies` řadiče, `Details` metoda a `ID` hodnotu.

Kód nejprve usnadňuje vyhledání dat pomocí `Find` metoda. Důležitou funkci zabezpečení součástí metodu je, že kód ověřuje, že `Find` nalezena film metoda, než kód pokusí udělat nic s ním. Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud jste nekontrolovaly film hodnotu null, null film by způsobilo chyby databáze.

Zkontrolujte `Delete` a `DeleteConfirmed` metody.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Všimněte si, že HTTP GET `Delete` metoda nedojde k odstranění určený film, vrátí zobrazení filmu kde můžete odeslat (`HttpPost`) k odstranění. Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádění operace upravit, vytvořit operaci, nebo jiná operace, která se mění data) otevře bezpečnostní riziko. Další informace o tom, najdete v položkách blogu Stephen Walther [46 Tip # aplikace ASP.NET MVC – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Metodu, která odstraňuje data jmenuje `DeleteConfirmed` umožnit metodu HTTP POST jedinečný podpis nebo název. Níže jsou uvedeny podpisy dvou metod:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Modul CLR (CLR) vyžaduje přetížené metody jedinečný podpis (stejný název metody, ale jiné seznam parametrů). Zde však, je nutné odstranit způsobů – jeden pro GET--a jeden pro metodu POST, že mají stejný parametr podpis. (I potřebují tak, aby přijímal jeden celé číslo jako parametr.)

Seřadit to zjistit, můžete provést několik věcí. Jeden je umožnit metody odlišné názvy. To je mechanismus generování uživatelského rozhraní, které jste provedli v předchozím příkladu. Ale vzniká malé problému: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, normálně směrování nebude schopna nalézt dané metody. Je řešení, najdete v příkladu, který je přidání `ActionName("Delete")` atribut `DeleteConfirmed` metoda. To efektivně provede mapování pro směrování systém, tak, že adresu URL, která obsahují */Delete/* pro metodu POST SMĚŘUJÍCÍ požadavku najdete `DeleteConfirmed` metoda.

Dalším běžným způsobem vyhnout potížím s metodami, které mají stejné názvy a podpisy je uměle změnit podpis metodu POST zahrnout nepoužívané parametr. Například někteří vývojáři přidat typ parametru `FormCollection` předá metodu POST a potom jednoduše nepoužívejte parametr:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Souhrn

Nyní máte dokončení aplikace ASP.NET MVC, která ukládá data v místní databázi DB. Můžete vytvořit, číst, aktualizovat, odstranit a hledat filmy.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Další kroky

Po vytvořené a otestovat webovou aplikaci, dalším krokem je zpřístupnit ostatním používat přes Internet. K tomu, budete muset nasadit do webové poskytovatele hostitelských služeb. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve [Bezplatný zkušební účet Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Mohu navrhnout další v mé kurzu [nasazení aplikace zabezpečení rozhraní ASP.NET MVC s členství, OAuth a databáze SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Vynikající kurzu je tní Dykstra úrovni zprostředkující [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Stackoverflow](http://stackoverflow.com/help) a [ASP.NET MVC fóra](https://forums.asp.net/1146.aspx) jsou skvělá umístí klást otázky. Postupujte podle [mi](https://twitter.com/RickAndMSFT) na twitteru, abyste získali aktualizace na můj nejnovější kurzy.

Zpětná vazba je úvodní.

– [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
– [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Předchozí](adding-validation.md)
