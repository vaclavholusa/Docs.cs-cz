---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Vylepšení podrobností a metod Delete (C#) | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 5dc786a05822328bc4317c582d135f26fb168dfb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756852"
---
<a name="improving-the-details-and-delete-methods-c"></a>Vylepšení podrobností a metod Delete (C#)
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Je k dispozici aktualizovaná verze tohoto kurzu [tady](../../../getting-started/introduction/getting-started.md) , která používá ASP.NET MVC 5 a Visual Studio 2013. Je bezpečnější, postupujte podle mnohem jednodušší a ukazuje další funkce.
> 
> 
> V tomto kurzu se seznámíte se základy vytváření ASP.NET MVC webovou aplikaci pomocí Microsoft Visual Web Developer 2010 Express Service Pack 1, což je bezplatná verze sady Microsoft Visual Studio. Než začnete, ujistěte se, že jste nainstalovali požadavky uvedené níže. Nainstalujte všechny z nich kliknutím na následující odkaz: [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativně můžete nainstalovat jednotlivě požadavky pomocí následujících odkazů:
> 
> - [Visual Studio Web Developer Express SP1 požadavky](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 nástroje Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(podpora modulu runtime a nástroje)
> 
> Pokud používáte Visual Studio 2010 namísto Visual Web Developer 2010, nainstalujte příslušné požadované součásti po kliknutí na následující odkaz: [požadavky sady Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Projekt aplikace Visual Web Developer se zdrojovým kódem jazyka C# je k dispozici v tomto tématu. [Stáhněte si verzi C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Pokud dáváte přednost jazyka Visual Basic, přejděte [verze jazyka Visual Basic](../vb/intro-to-aspnet-mvc-3.md) tohoto kurzu.


V této části kurzu se provede několik vylepšení pro automaticky generované `Details` a `Delete` metody. Tyto změny se nevyžadují, ale s několika malých bity kódu můžete snadno zvýšit aplikace.

## <a name="improving-the-details-and-delete-methods"></a>Vylepšení podrobností a metod Delete

Když vygenerovanou `Movie` kontroleru, ASP.NET MVC vygeneruje kód, který dobře fungovalo, ale, který lze robustnější s několika malých změn.

Otevřít `Movie` kontroleru a upravit `Details` metodu tak, že vrací `HttpNotFound` při videa se nenašel. Také byste měli upravit `Details` metodu pro nastavení výchozí hodnoty pro Identifikátor, který je předán. (Podobné změny `Edit` metoda ve [6. část](examining-the-edit-methods-and-edit-view.md) této příručky.) Však musíte změnit návratový typ `Details` metodu z `ViewResult` k `ActionResult`, protože `HttpNotFound` metoda nevrací `ViewResult` objektu. Následující příklad ukazuje upravenou `Details` metody.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Kód nejprve umožňuje snadno vyhledat data s využitím `Find` metody. Důležitou funkci zabezpečení, který jsme vytvořili do metody je, že kód ověří, zda `Find` metoda našla filmu předtím, než kód se pokusí provádět s ním. Například se hacker by mohla zanést chyby do lokality tak, že změníte adresu URL vytvořené odkazy z `http://localhost:xxxx/Movies/Details/1` na něco jako `http://localhost:xxxx/Movies/Details/12345` (nebo jinou hodnotu, která nepředstavuje skutečný film). Pokud nemáte kontrolu null film, může vést k chybě databáze.

Podobně, změnit `Delete` a `DeleteConfirmed` metody, které můžete zadat výchozí hodnotu pro parametr ID a vrátit `HttpNotFound` při videa se nenašel. Aktualizovaný `Delete` metody v `Movie` řadiče jsou uvedeny níže.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Všimněte si, že `Delete` metoda neodstraní data. Operace delete v reakci na příkaz GET požádat o (nebo k tomuto účelu, provádění operace úpravy, vytváření operace nebo jiné operace, která se mění data) otevře bezpečnostní riziko. Další informace o tom, najdete v blogu Stephen Walther [46 Tip # aplikace ASP.NET MVC – nepoužívejte odstranit odkazy, protože uživatel vytvořit bezpečnostní díry](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

`HttpPost` Pojmenovanou metodu, která odstraní data `DeleteConfirmed` poskytnout metodu HTTP POST jedinečný podpis nebo název. Níže jsou zobrazena podpisy dvou metod:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Modul CLR (CLR) vyžaduje přetížené metody má jedinečnou signaturu (stejný název, jiný seznam parametrů). Tady však dvou metod Delete – jeden u metody GET - a jeden pro metodu POST, že oba vyžadují stejnou signaturu. (I potřebují tak, aby přijímal celá čísla jako parametr.)

Řazení, to zjistit, můžete provést několik věcí. Jeden je poskytnout metody různými názvy. Je to, co jsme udělali mu předchozí příklad. Ale zavádí malé potíže: ASP.NET mapuje segmentů adresy URL na metody akce podle názvu, a Pokud přejmenujete metodu, obvykle směrování nebude moci najít tuto metodu. Řešení se zobrazí v příkladu, který je přidat `ActionName("Delete")` atribut `DeleteConfirmed` metoda. Efektivně provede mapování pro směrování systém tak, aby adresa URL, která zahrnuje <em>/Delete/</em>příspěvek požadavku najdete `DeleteConfirmed` metody.

Dalším způsobem, jak se vyhnout potížím s metodami, které mají stejné názvy a podpisy se uměle změna podpisu metody POST, která zahrnují Nepoužitý parametr. Někteří vývojáři přidat například typ parametru `FormCollection` , který je předán metodě příspěvku a poté jednoduše nepoužívejte parametr:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Zabalení

Teď máte dokončené aplikace ASP.NET MVC, která ukládá data v databázi systému SQL Server Compact. Můžete vytvořit, číst, aktualizovat, odstranit a hledat videa.

![](improving-the-details-and-delete-methods/_static/image1.png)

V tomto kurzu základní teď vám pomůže začít vytváření kontrolerů, jejich přidružení k zobrazení a předáním kolem údajů o pevně zakódované. Když pak vytvoří a navržené datový model. Entity Framework Code First vytvořili databázi z datového modelu v reálném čase a systém generování uživatelského rozhraní ASP.NET MVC automaticky generované metody akce a zobrazení pro základní operace CRUD. Pak jste přidali hledání formulář, který umožní uživatelům vyhledávání databáze. Změnit databáze, kterou chcete zahrnout nové sloupce dat a potom aktualizuje dvě stránky, které umožňuje vytvořit a zobrazit tato nová data. Přidání ověření označením datového modelu s atributy z `DataAnnotations` oboru názvů. Výsledný ověřování je spuštěno na straně klienta a na serveru.

Pokud chcete nasadit aplikaci, je užitečné pro první testování aplikace na místním serveru služby IIS 7. To může být použito [instalačního programu webové platformy](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) odkaz povolit nastavení služby IIS pro aplikace ASP.NET. V následujících tématech nasazení:

- [Mapa obsahu nasazení technologie ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Povolení služby IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Projekty nasazení webových aplikací](https://msdn.microsoft.com/library/dd394698.aspx)

Nyní neváhejte se podívat na naše středně [vytváření datového modelu Entity Framework pro aplikaci ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) a [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) kurzy, prozkoumávat [technologie ASP.NET články na webu MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)a podívejte se na mnoho videa a materiály v [ https://asp.net/mvc ](https://asp.net/mvc) získat i další informace o ASP.NET MVC. [Fóra ASP.NET MVC](https://forums.asp.net/1146.aspx) jsou skvělé místo, kde můžete klást otázky.

Užijte si!

Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) a [ @shanselman ](http://twitter.com/shanselman) na Twitteru) a Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Předchozí](adding-validation-to-the-model.md)
