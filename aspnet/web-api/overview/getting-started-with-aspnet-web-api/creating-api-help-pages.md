---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Vytváření stránek nápovědy webového rozhraní API ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/01/2013
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 2d8758034ca4339ed7e9699cf2f2643bfab87ba4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756869"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Vytváření stránek nápovědy pro rozhraní ASP.NET Web API
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Při vytváření webového rozhraní API, často je užitečné vytvořit stránku nápovědy, tak, aby ostatní vývojáři vědět, jak volat rozhraní API. Dokumentace může vytvořit ručně, ale je lepší automaticky vytvořit co největší míře.

Pro usnadnění tohoto úkolu, rozhraní ASP.NET Web API poskytuje knihovnu pro automatické generování stránek nápovědy v době běhu.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Vytváření stránek nápovědy k rozhraní API

Nainstalujte [ASP.NET a Web Tools 2012.2 aktualizace](https://go.microsoft.com/fwlink/?LinkId=282650). Tato aktualizace stránky nápovědy integruje šablony projektu webového rozhraní API.

Dále vytvořte nový projekt ASP.NET MVC 4 a vyberte šablonu projektu webového rozhraní API. Šablona projektu vytvoří řadič ukázkové rozhraní API s názvem `ValuesController`. Šablona vytvoří také stránek nápovědy rozhraní API. Všechny soubory kódu stránky s nápovědou jsou umístěny ve složce oblasti projektu.

![](creating-api-help-pages/_static/image2.png)

Při spuštění aplikace na domovské stránce obsahuje odkaz na stránku nápovědy na rozhraní API. Z domovské stránky je relativní cesta/help.

![](creating-api-help-pages/_static/image3.png)

Tento odkaz vám přináší na souhrnnou stránku rozhraní API.

![](creating-api-help-pages/_static/image4.png)

Pro tuto stránku zobrazení MVC je definována v Areas/HelpPage/Views/Help/Index.cshtml. Můžete upravit této stránky můžete upravit rozložení, úvod, název, styly a tak dále.

Hlavní části stránky je tabulka rozhraní API, seskupené podle kontroleru. Položky tabulky generuje dynamicky pomocí **IApiExplorer** rozhraní. (I více zabývat toto rozhraní později.) Pokud chcete přidat nový kontroler API, se automaticky aktualizuje v tabulce v době běhu.

Sloupce "Rozhraní API" seznam metodu HTTP a relativní identifikátor URI. Ve sloupci "Popis" obsahuje dokumentaci pro každé rozhraní API. Na začátku dokumentace je jen zástupný text. V další části můžu ukážeme, jak přidat dokumentace ke službě z komentářů XML.

Každé rozhraní API obsahuje odkaz na stránku se podrobnější informace, včetně příklad těla požadavku a odpovědi.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Přidání stránky nápovědy k existujícímu projektu

Stránky nápovědy k existujícímu projektu webového rozhraní API můžete přidat pomocí Správce balíčků NuGet. Tato možnost je užitečná, že spusťte ze šablony jiný projekt než má šablona "Webového rozhraní API".

Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**a pak vyberte **Konzola správce balíčků**. V [Konzola správce balíčků](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) okno, zadejte jeden z následujících příkazů:

Pro **jazyka C#** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Pro **jazyka Visual Basic** aplikace: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Existují dva balíčky, jeden pro jazyk C# a jeden pro jazyk Visual Basic. Ujistěte se, že chcete používat ten, který odpovídá projektu.

Tento příkaz nainstaluje potřebná sestavení a přidá zobrazení MVC pro stránky nápovědy (umístěný ve složce plochy/HelpPage). Bude potřeba ručně přidat odkaz na stránku nápovědy. Identifikátor URI je/help. Pokud chcete vytvořit odkaz v zobrazení razor, přidejte následující:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Také ujistěte se, že k registraci oblasti. V souboru Global.asax přidejte následující kód, který **aplikace\_Start** metodu, pokud tam není již:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Přidání dokumentace k rozhraní API

Ve výchozím nastavení, v nápovědě stránky obsahují zástupný text řetězce pro dokumentaci. Můžete použít [dokumentační komentáře XML](https://msdn.microsoft.com/library/b2s063f7.aspx) vytvořit v dokumentaci. Chcete-li tuto funkci povolit, otevřete soubor oblasti nebo HelpPage/aplikace\_Start/HelpPageConfig.cs a zrušte komentář u následujícího řádku:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Nyní povolte dokumentace XML. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**. Vyberte **sestavení** stránky.

![](creating-api-help-pages/_static/image6.png)

V části **výstup**, zkontrolujte **soubor dokumentace XML**. Do textového pole zadejte "aplikace\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

Dále otevřete kód `ValuesController` kontroleru rozhraní API, která je definována v /Controllers/ValuesControler.cs. Přidáte nějaké komentáře k dokumentaci pro metody kontroleru. Příklad:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Tip: Pokud umístit blikající kurzor na řádek nad metodu a zadejte tři lomítka, Visual Studio automaticky vloží prvky XML. Potom můžete vyplnit prázdné hodnoty.


Nyní vytvářet a spusťte aplikaci znovu a přejděte na stránky nápovědy. Dokumentace ke službě řetězce by se zobrazit v tabulce rozhraní API.

![](creating-api-help-pages/_static/image8.png)

Na stránce nápovědy přečte řetězce ze souboru XML v době běhu. (Když nasadíte aplikaci, ujistěte se, že k nasazení souboru XML.)

## <a name="under-the-hood"></a>Pohled pod kapotu

Stránky nápovědy jsou zabudovány nad **ApiExplorer** třídu, která je součástí rozhraní Web API. **ApiExplorer** třída poskytuje suroviny pro vytvoření stránky nápovědy. Pro každé rozhraní API **ApiExplorer** obsahuje **ApiDescription** popisující rozhraní API. V tomto případě "API" je definován jako kombinace relativní URI a metodou HTTP. Například tady jsou některé různá rozhraní API:

- ZÍSKAT /api/Products
- ZÍSKAT/webové rozhraní API/produkty / {id}
- Publikovat/api/produkty

Pokud je akce kontroleru podporuje více metod HTTP, **ApiExplorer** považuje každá metoda různých rozhraní API.

Skrýt rozhraní API z **ApiExplorer**, přidejte **ApiExplorerSettings** akce a nastavte atribut *IgnoreApi* na hodnotu true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

Můžete také přidat tento atribut na řadič, vyloučit celý kontroler.

Třída ApiExplorer získá dokumentaci řetězců z **IDocumentationProvider** rozhraní. Jak jste viděli již dříve, poskytuje knihovna stránky nápovědy **IDocumentationProvider** , který získá dokumentaci z řetězců dokumentaci XML. Kód se nachází v /Areas/HelpPage/XmlDocumentationProvider.cs. Můžete získat dokumentaci z jiného zdroje napsáním vlastní **IDocumentationProvider**. Chcete-li ji nastavit, zavolejte **SetDocumentationProvider** metody rozšíření definované v **HelpPageConfigurationExtensions**

**ApiExplorer** automaticky zavolá **IDocumentationProvider** rozhraní pro získání dokumentace řetězce pro každé rozhraní API. Ukládá je v **dokumentaci** vlastnost **ApiDescription** a **ApiParameterDescription** objekty.

## <a name="next-steps"></a>Další kroky

Nejste omezeni na stránky nápovědy je vidět tady. Ve skutečnosti **ApiExplorer** není omezena pouze na vytváření stránek nápovědy. Ty Huang Yao zapsala si že některé skvělé příspěvky najdete, vám uvažujete úprav:

- [Přidání jednoduchého testovacího klienta stránka nápovědy k ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Vytváření technologie ASP.NET webové rozhraní API pomáhají stránky pracovat na služby v místním prostředí](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Vytvoření v době návrhu nápovědy stránky (nebo klienta) pro rozhraní ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Rozšířená přizpůsobení stránce nápovědy](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
