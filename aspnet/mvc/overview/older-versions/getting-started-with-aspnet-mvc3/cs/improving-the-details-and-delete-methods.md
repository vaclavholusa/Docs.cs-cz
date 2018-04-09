---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Vylepšení podrobnosti a Delete metody (C#) | Microsoft Docs
author: Rick-Anderson
description: V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 55945eb373c79fd6ae018fe8f896dc5e6bbe7744
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="improving-the-details-and-delete-methods-c"></a>Vylepšení podrobnosti a Delete metody (C#)
====================
podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [sem](../../../getting-started/introduction/getting-started.md) používající ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu naučit se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, který je bezplatnou verzi sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Kliknutím na následující odkaz můžete nainstalovat všechny z nich: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aktualizace nástrojů rozhraní ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podporu runtime + nástroje)
> 
> Pokud používáte Visual Studio 2010 místo Visual Web Developer 2010, nainstalujte součásti kliknutím na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt Visual Web Developer se zdrojový kód C# je k dispozici v tomto tématu. [Stáhnout verzi jazyka C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přepnout [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V této části kurzu budete provést některé zlepšení automaticky generované `Details` a `Delete` metody. Tyto změny nejsou povinná, ale s několika malé bity kódu, můžete snadno vylepšit aplikace.

## <a name="improving-the-details-and-delete-methods"></a>Vylepšení podrobnosti a metody odstranění

Když vygeneroval `Movie` řadiče, ASP.NET MVC generovaného kódu, který skvěle fungovala, ale který můžete provedeny robustnější s několika malé změny.

Otevřete `Movie` řadiče a upravovat `Details` metoda vrácením `HttpNotFound` při film nebyl nalezen. Musí také upravit `Details` metodu a nastavit výchozí hodnotu pro ID, který je předán. (Podobně jako změny provedené `Edit` metoda v [část 6](examining-the-edit-methods-and-edit-view.md) tohoto kurzu.) Však musíte změnit návratový typ `Details` metoda z `ViewResult` k `ActionResult`, protože `HttpNotFound` metoda nevrací `ViewResult` objektu. Následující příklad ukazuje upravenou `Details` metoda.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Kód nejprve usnadňuje vyhledání dat pomocí `Find` metoda. O funkci důležité zabezpečení, která jsme vytvořili do metody je, že kód ověřuje, že `Find` nalezena film metoda, než kód pokusí udělat nic s ním. Například se hacker může představovat chyby na webu tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` například `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud to neuděláte kontrolu null film, to může způsobit chybu databáze.

Podobně změnit `Delete` a `DeleteConfirmed` metody, které chcete zadat výchozí hodnotu pro parametr ID a vrátit `HttpNotFound` při film nebyl nalezen. Aktualizovaný `Delete` metody v `Movie` řadiče jsou uvedeny níže.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Všimněte si, že `Delete` metoda neodstraní data. Provádění operace odstranění v reakci na příkaz GET požadavku (nebo k tomuto účelu, provádění operace upravit, vytvořit operaci, nebo jiná operace, která se mění data) otevře bezpečnostní riziko. Další informace o tom, najdete v položkách blogu Stephen Walther [46 Tip # aplikace ASP.NET MVC – nepoužívejte odkazy odstranit, protože uživatel vytvořit celistvosti](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Metodu, která odstraňuje data jmenuje `DeleteConfirmed` umožnit metodu HTTP POST jedinečný podpis nebo název. Níže jsou uvedeny podpisy dvou metod:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Modul CLR (CLR) vyžaduje přetížené metody jedinečný podpis (stejný název, jiný seznam parametrů). Zde však, je nutné odstranit způsobů – jeden pro GET--a jeden pro metodu POST obě vyžadují, aby stejným podpisem. (I potřebují tak, aby přijímal jeden celé číslo jako parametr.)

Seřadit to zjistit, můžete provést několik věcí. Jeden je umožnit metody odlišné názvy. To je, co jsme se v mu předcházející příklad. Ale vzniká malé problému: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, normálně směrování nebude schopna nalézt dané metody. Je řešení, najdete v příkladu, který je přidání `ActionName("Delete")` atribut `DeleteConfirmed` metoda. To efektivně provede mapování pro směrování systém, tak, že adresu URL, která obsahují <em>/Delete/</em>pro metodu POST SMĚŘUJÍCÍ požadavku najdete `DeleteConfirmed` metoda.

Dalším způsobem vyhnout potížím s metodami, které mají stejné názvy a podpisy je uměle změnit podpis metodu POST zahrnout nepoužívané parametr. Například někteří vývojáři přidat typ parametru `FormCollection` předá metodu POST a potom jednoduše nepoužívejte parametr:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Zabalení

Nyní máte dokončení aplikace ASP.NET MVC, která ukládá data v databázi systému SQL Server Compact. Můžete vytvořit, číst, aktualizovat, odstranit a hledat filmy.

![](improving-the-details-and-delete-methods/_static/image1.png)

V tomto kurzu základní tu můžete spustit provedením řadiče a jejich přidružení k zobrazení a předání kolem pevně data. Potom můžete vytvořit a určené datový model. Entity Framework Code First vytvořili databázi z modelu dat za chodu a systém generování uživatelského rozhraní ASP.NET MVC jsou automaticky generovány metody akce a zobrazení pro základní operace CRUD. Pak přidá vyhledávání formulář, který umožní uživatelům vyhledávat databázi. Změnit databázi zahrnout nové sloupce dat a pak aktualizuje dvě stránky k vytváření a zobrazování tato nová data. Přidání ověření označením datového modelu s atributy z `DataAnnotations` oboru názvů. Výsledný ověření se spouští na klientovi a na serveru.

Pokud chcete nasadit aplikaci, je užitečné první testovací aplikace na místním serveru IIS 7. Můžete to použít [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) odkaz povolit nastavení služby IIS pro aplikace ASP.NET. V následujících tématech nasazení:

- [Mapa obsahu nasazení technologie ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Povolení služby IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Nasazení projekty webových aplikací](https://msdn.microsoft.com/library/dd394698.aspx)

I nyní doporučujeme přesunout našem zprostředkující úrovně [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) a [MVC Hudba úložiště](../../mvc-music-store/mvc-music-store-part-1.md) kurzy a prozkoumejte [ASP.NET články na webu MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)a podívejte se na mnoha videa a prostředkům v [ https://asp.net/mvc ](https://asp.net/mvc) i podrobné informace o architektuře ASP.NET MVC! [ASP.NET MVC fóra](https://forums.asp.net/1146.aspx) jsou skvělým místem klást otázky.

Užijte si ji!

– Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru) a Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Předchozí](adding-validation-to-the-model.md)
