---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Vytvoření stránky nápovědy pro rozhraní ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037900"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Vytvoření stránky nápovědy pro rozhraní ASP.NET Web API
====================
podle [Wasson Jan](https://github.com/MikeWasson)

Při vytváření webového rozhraní API je často užitečné vytvořit stránku nápovědy, aby ostatní vývojáři věděli, jak volat rozhraní API. Veškerá dokumentace může vytvořit ručně, ale je lepší automaticky vytvořit co největší míře.

Chcete-li tato úloha jednodušší, rozhraní ASP.NET Web API poskytuje knihovnu pro automatické generování stránky nápovědy v době běhu.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Vytvoření stránky nápovědy rozhraní API

Nainstalujte [ASP.NET a webové nástroje 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650). Tato aktualizace se integruje stránky nápovědy do šablony projektu webového rozhraní API.

Dále vytvořte nový projekt ASP.NET MVC 4 a výběr šablony projektu webového rozhraní API. Šablona projektu vytvoří řadič příklad rozhraní API s názvem `ValuesController`. Šablona vytvoří také stránky nápovědy rozhraní API. Všechny soubory kódu pro stránku nápovědy jsou umístěny ve složce oblastí projektu.

![](creating-api-help-pages/_static/image2.png)

Při spuštění aplikace Domovská stránka obsahuje odkaz na tuto stránku nápovědy rozhraní API. Na domovské stránce je relativní cesta/help.

![](creating-api-help-pages/_static/image3.png)

Tento odkaz umožňuje souhrnnou stránku rozhraní API.

![](creating-api-help-pages/_static/image4.png)

Tato stránka zobrazení MVC je definována v Areas/HelpPage/Views/Help/Index.cshtml. Můžete upravit této stránce můžete upravit rozložení, úvod, název, styly a tak dále.

Hlavní část stránky je tabulka rozhraní API, seskupené podle řadiče. Položky tabulky jsou generována dynamicky, pomocí **IApiExplorer** rozhraní. (I mluvit o další informace o tomto rozhraní později.) Pokud přidáte nový řadič rozhraní API, v tabulce se automaticky aktualizuje v době běhu.

Sloupec "API" seznam Metoda HTTP a relativní identifikátor URI. Sloupec "Popis" obsahuje dokumentace pro každé rozhraní API. Na začátku dokumentace je právě zástupný text. V další části I budete ukazují, jak přidat dokumentace z komentáře XML.

Každé rozhraní API obsahuje odkaz na stránku s podrobnější informace, včetně příklad těla požadavku a odpovědi.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Přidání stránky nápovědy do existujícího projektu

Stránky nápovědy můžete přidat do existujícího projektu webového rozhraní API pomocí Správce balíčků NuGet. Tato možnost je užitečná, že spustíte z šablonu jiný projekt, než má šablona "Webového rozhraní API".

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a potom vyberte **Konzola správce balíčků**. V [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okno, zadejte jeden z následujících příkazů:

Pro **C#** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Pro **jazyka Visual Basic** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Existují dva balíčky, jeden pro jazyk C# a jeden v jazyce Visual Basic. Nezapomeňte použít ten, který odpovídá projektu.

Tento příkaz nainstaluje potřebné sestavení a přidá zobrazení MVC pro stránky nápovědy (umístěný ve složce oblasti nebo HelpPage). Budete muset ručně přidejte odkaz na stránku nápovědy. Identifikátor URI je/help. Chcete-li vytvořit odkaz v zobrazení syntaxe razor, přidejte následující:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Také zajistěte, aby k registraci oblasti. V souboru Global.asax, přidejte následující kód, který **aplikace\_spustit** metodu, pokud tam není již:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Přidání dokumentaci k rozhraní API

Ve výchozím nastavení v nápovědě stránky máte řetězce zástupný symbol pro dokumentaci. Můžete použít [dokumentační komentáře XML](https://msdn.microsoft.com/library/b2s063f7.aspx) vytvořit v dokumentaci. Chcete-li povolit tuto funkci, otevřete soubor oblasti nebo HelpPage nebo aplikace\_Start/HelpPageConfig.cs a zrušte komentář u následujícího řádku:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Teď povolte dokumentace XML. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**. Vyberte **sestavení** stránky.

![](creating-api-help-pages/_static/image6.png)

V části **výstup**, zkontrolujte **souborů dokumentace XML**. Do textového pole zadejte "aplikace\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Dále otevřete kód pro `ValuesController` kontroler API, která je definována v /Controllers/ValuesControler.cs. Přidejte do metody kontroleru některé dokumentační komentáře. Příklad:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Tip: Pokud umístit pomocí kurzoru na řádku výše metodu a zadejte tři lomítka, Visual Studio automaticky vloží elementy XML. Potom můžete vyplnit prázdné znaky.


Nyní sestavení a spusťte aplikaci znovu a přejděte na stránky nápovědy. Řetězce dokumentace by se zobrazit v tabulce rozhraní API.

![](creating-api-help-pages/_static/image8.png)

Na stránce nápovědy přečte řetězce ze souboru XML v době běhu. (Když nasadíte aplikaci, ujistěte se, k nasazení souboru XML.)

## <a name="under-the-hood"></a>Pod pokličkou

Stránky nápovědy jsou postavený na **ApiExplorer** třídy, která je součástí rozhraní Web API. **ApiExplorer** třída poskytuje suroviny pro vytvoření stránky nápovědy. Pro každé rozhraní API **ApiExplorer** obsahuje **ApiDescription** , který popisuje rozhraní API. Pro tento účel "API" se rozumí kombinace metodu HTTP a relativní identifikátor URI. Tady jsou například některé odlišné rozhraní API:

- ZÍSKAT /api/Products
- ZÍSKAT /api/produkty / {id}
- POST/api/produkty

Pokud je akce kontroleru podporuje více metod HTTP, **ApiExplorer** každá metoda zpracovává jako odlišné rozhraní API.

Skrytí rozhraní API z **ApiExplorer**, přidejte **ApiExplorerSettings** atribut akci a sadu *IgnoreApi* na hodnotu true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Můžete také přidat tento atribut řadiče, vyloučit celý kontroler.

Třída ApiExplorer získá dokumentaci řetězců z **IDocumentationProvider** rozhraní. Jak už jste viděli dříve, knihovny pomoci stránek poskytuje **IDocumentationProvider** , který získá dokumentaci z řetězců dokumentace XML. Kód se nachází v /Areas/HelpPage/XmlDocumentationProvider.cs. Můžete získat dokumentaci z jiného zdroje vytvořením vlastní **IDocumentationProvider**. Chcete-li propojit je s nahoru, zavolejte **SetDocumentationProvider** rozšíření metody definované v **HelpPageConfigurationExtensions**

**ApiExplorer** automaticky volání **IDocumentationProvider** rozhraní pro získání dokumentace řetězce pro každé rozhraní API. Ukládá je do **dokumentace** vlastnost **ApiDescription** a **ApiParameterDescription** objekty.

## <a name="next-steps"></a>Další kroky

Nejste omezená na stránky nápovědy, které jsou tady uvedené. Ve skutečnosti **ApiExplorer** není omezeno na vytváření stránky nápovědy. Některé skvělé příspěvky si myslím mimo pole má zapisovat Yao Huang odkazů:

- [Přidání jednoduché testovacího klienta na stránce nápovědy k serveru ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Provedení ASP.NET Web API stránce nápovědy k pracovat s vlastním hostováním služby](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Vytvoření v době návrhu nápovědy stránky (nebo klienta) pro ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Rozšířené přizpůsobení stránce nápovědy](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
