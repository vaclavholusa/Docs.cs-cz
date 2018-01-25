---
uid: web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
title: "Začínáme s rozhraním ASP.NET Web API 2 (C#)"
author: MikeWasson
description: "HTTP není právě pro poskytovat webové stránky. Je také efektivní platformu pro sestavování rozhraní API, která vystavit služby a data. HTTP je jednoduchý, flexibilní a ubiq..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/28/2017
ms.topic: article
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api
msc.type: authoredcontent
ms.openlocfilehash: 6ff9fd279a03197f761454bba3f180d7428b1b1f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-aspnet-web-api-2-c"></a>Začínáme s rozhraním ASP.NET Web API 2 (C#)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Stáhněte si dokončený projekt](https://code.msdn.microsoft.com/Sample-code-of-Getting-c56ccb28)

HTTP není právě pro poskytovat webové stránky. HTTP je také efektivní platformu pro sestavování rozhraní API, která vystavit služby a data. HTTP je jednoduchý, flexibilní a všudypřítomný. Téměř jakékoli platformě, která si můžete představit má knihovna protokolu HTTP, takže služeb HTTP může být využity širokou škálou klientů včetně prohlížečů a mobilních zařízení, tradiční aplikací klasické pracovní plochy.
 
Rozhraní ASP.NET Web API je architektura pro vytváření webových rozhraní API v rozhraní .NET Framework. V tomto kurzu použije k vytvoření webové rozhraní API, který vrátí seznam produktů ASP.NET Web API.

 
 ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
  
 - [Visual Studio 2017](https://www.visualstudio.com/downloads/)
 - Webové rozhraní API 2

V tématu [vytvoření webového rozhraní API pomocí ASP.NET Core a Visual Studio pro Windows](https://docs.microsoft.com/aspnet/core/tutorials/first-web-api) pro novější verze tohoto kurzu.

## <a name="create-a-web-api-project"></a>Vytvoření projektu webového rozhraní API

V tomto kurzu použije k vytvoření webové rozhraní API, který vrátí seznam produktů ASP.NET Web API. Front-endu webové stránky používá jQuery pro zobrazení výsledků.

![](tutorial-your-first-web-api/_static/image1.png)

Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **webové**. V seznamu šablon projektu, vyberte **webové aplikace ASP.NET**. Název projektu "ProductsApp" a klikněte na tlačítko **OK**.

![](tutorial-your-first-web-api/_static/image2.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony. V části &quot;přidat složky a odkazy na základní&quot;, zkontrolujte **webového rozhraní API**. Click **OK**.

![](tutorial-your-first-web-api/_static/image3.png)

> [!NOTE]
> Můžete také vytvořit projekt webového rozhraní API pomocí &quot;webového rozhraní API&quot; šablony. Šablona webového rozhraní API používá rozhraní ASP.NET MVC zajistit stránky nápovědy rozhraní API. Používám prázdné šablonu pro účely tohoto kurzu, protože chcete zobrazit webového rozhraní API bez MVC. Obecně platí nemusíte vědět, ASP.NET MVC používat webového rozhraní API.


## <a name="adding-a-model"></a>Přidání modelu

A *modelu* je objekt, který představuje data ve vaší aplikaci. Rozhraní ASP.NET Web API můžete automaticky serializovat modelu JSON, XML nebo jiném formátu a zapište serializovaná data do datové části zprávy s odpovědí HTTP. Tak dlouho, dokud klient může číst formát serializace, může deserializovat objekt. Většina klientů můžete analyzovat, XML nebo JSON. Kromě toho klienta můžete určit formát, který se chce nastavením hlavička Accept ve zprávě požadavku HTTP.

Začněme vytvořením jednoduchého modelu, který představuje produkt.

Pokud Průzkumníku již není viditelný, klikněte na tlačítko **zobrazení** nabídku a vyberte **Průzkumníku řešení**. V Průzkumníku řešení klikněte pravým tlačítkem na složku modely. V místní nabídce vyberte **přidat** vyberte **třída**.

![](tutorial-your-first-web-api/_static/image4.png)

Název třídy &quot;produktu&quot;. Přidejte následující vlastnosti pro `Product` třídy.

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample1.cs)]

## <a name="adding-a-controller"></a>Přidání Kontroleru

V rozhraní Web API *řadič* je objekt, který zpracovává požadavky HTTP. Přidáme kontroler, který může vrátit seznam produktů nebo jednoho produktu určeného ID.

> [!NOTE]
> Pokud jste použili ASP.NET MVC, jste již obeznámeni s řadiči. Webové rozhraní API řadiče jsou podobná řadiče MVC, ale dědění **objektu ApiController** místo **řadič** – třída.

V **Průzkumníku**, klikněte pravým tlačítkem na složku řadiče. Vyberte **přidat** a pak vyberte **řadič**.

![](tutorial-your-first-web-api/_static/image5.png)

V **přidat vygenerované uživatelské rozhraní** dialogovém okně, vyberte **kontroler Web API – prázdný**. Klikněte na tlačítko **přidat**.

![](tutorial-your-first-web-api/_static/image6.png)

V **přidat kontroler** dialogové okno, názvu kontroleru &quot;ProductsController&quot;. Klikněte na tlačítko **přidat**.

![](tutorial-your-first-web-api/_static/image7.png)

Soubor s názvem ProductsController.cs ve složce řadiče se vytvoří generování uživatelského rozhraní.

![](tutorial-your-first-web-api/_static/image8.png)

> [!NOTE]
> Není nutné uvést řadičů do složky s názvem řadiče. Název složky je jenom pohodlný způsob pro uspořádání zdrojové soubory.


Pokud tento soubor už není otevřený, poklikejte na soubor otevřete. Nahraďte kód v tomto souboru s následujícími službami:

[!code-csharp[Main](tutorial-your-first-web-api/samples/sample2.cs)]

Pro zjednodušení příkladu produkty ukládají na pevné pole uvnitř třídy kontroleru. V reálné aplikaci by samozřejmě dotazu na databázi nebo použijte jiný zdroj externích dat.

Řadičem definuje dvě metody, které vracejí produkty:

- `GetAllProducts` Metoda vrátí celý seznam produktů jako **rozhraní IEnumerable&lt;produktu&gt;**  typu.
- `GetProduct` Metoda vyhledá jednoho produktu pomocí jeho ID.

Je to! Máte pracovní webového rozhraní API. Každá metoda v řadiči odpovídá jeden nebo více identifikátory URI:

| Metoda kontroleru | Identifikátor URI |
| --- | --- |
| GetAllProducts | / api/produkty |
| GetProduct | /api/products/*id* |

Pro `GetProduct` metody *id* v identifikátoru URI je zástupný symbol. Například pokud chcete získat produktu s ID 5, je identifikátor URI `api/products/5`.

Další informace o tom, jak webové rozhraní API směruje požadavky HTTP na metody kontroleru najdete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="calling-the-web-api-with-javascript-and-jquery"></a>Volání webového rozhraní API s Javascript a jQuery

V této části přidáme stránku HTML, který používá rozhraní AJAX k volání webového rozhraní API. Použijeme jQuery volání AJAX a také k aktualizaci stránky s výsledky.

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **novou položku**.

![](tutorial-your-first-web-api/_static/image9.png)

V **přidat novou položku** dialogovém okně, vyberte **webové** pod uzlem **Visual C#**a pak vyberte **stránku HTML** položky. Název stránky &quot;index.html&quot;.

![](tutorial-your-first-web-api/_static/image10.png)

Všechno, co je v tomto souboru nahraďte následujícím textem:

[!code-html[Main](tutorial-your-first-web-api/samples/sample3.html)]

Chcete-li získat jQuery několika způsoby. V tomto příkladu lze použít [Microsoft Ajax CDN](../../../ajax/cdn/overview.md). Také si můžete stáhnout z [http://jquery.com/](http://jquery.com/)a ASP.NET "Webového rozhraní API" Šablona projektu zahrnuje také jQuery.

### <a name="getting-a-list-of-products"></a>Získávání seznam produktů

Pokud chcete získat seznam produktů, odeslání požadavku HTTP GET na &quot;/api/produkty&quot;.

JQuery [getJSON](http://api.jquery.com/jQuery.getJSON/) funkce odešle požadavek AJAX. Odpověď obsahuje pole objektů JSON. `done` Určuje funkce zpětného volání, která je volána, pokud neproběhne. Zpětné volání aktualizujeme modelu DOM s informace o produktu.

[!code-html[Main](tutorial-your-first-web-api/samples/sample4.html)]

### <a name="getting-a-product-by-id"></a>Získání ID produktu

Potřebujete-li produkt podle ID, pošlete žádost HTTP GET na &quot;/api/produkty/*id*&quot;, kde *id* je ID produktu.

[!code-javascript[Main](tutorial-your-first-web-api/samples/sample5.js)]

Stále říkáme `getJSON` odeslat požadavek AJAX, avšak v této verzi jsme uveďte ID v identifikátoru URI požadavku. Odpověď z této žádosti je reprezentaci JSON jednoho produktu.

## <a name="running-the-application"></a>Spuštění aplikace

Stisknutím klávesy F5 spusťte ladění aplikace. Webová stránka by měl vypadat asi takto:

![](tutorial-your-first-web-api/_static/image11.png)

Získání produktu podle ID, zadejte ID a klikněte na tlačítko Hledat:

![](tutorial-your-first-web-api/_static/image12.png)

Pokud zadáte neplatné ID, server vrátí chybu HTTP:

![](tutorial-your-first-web-api/_static/image13.png)

## <a name="using-f12-to-view-the-http-request-and-response"></a>Pomocí F12 zobrazíte HTTP žádosti a odpovědi

Při práci se službou HTTP, může být velmi užitečná pro požadavek HTTP zobrazit a vyžádat zprávy. To provedete pomocí nástrojů pro vývojáře F12 v aplikaci Internet Explorer 9. Z aplikace Internet Explorer 9, stiskněte klávesu **F12** otevřete nástroje. Klikněte **sítě** kartě a stiskněte klávesu **spustit zachytávání**. Nyní přejděte zpět na webovou stránku a stiskněte klávesu **F5** načtením webové stránky. Internet Explorer bude zaznamenávat provoz protokolu HTTP mezi prohlížečem a webový server. Souhrnné zobrazení se zobrazuje veškerý síťový provoz pro stránku:

![](tutorial-your-first-web-api/_static/image14.png)

Vyhledejte položku pro relativní identifikátor URI "rozhraní api nebo produkty /". Vyberte tuto položku a klikněte na **přejít na podrobné zobrazení**. V zobrazení podrobností jsou karty zobrazíte hlavičkách žádostí a odpovědí a subjektů. Například pokud kliknete **hlavičky požadavku** kartě uvidíte, že si klient vyžádal &quot;application/json&quot; v hlavičce Accept.

![](tutorial-your-first-web-api/_static/image15.png)

Pokud kliknete na kartu text odpovědi, uvidíte, jak byl seznam produktů serializovat na JSON. Jiné prohlížeče mají podobné funkce. Další užitečné nástroj je [Fiddler](http://www.fiddler2.com/fiddler2/), webové proxy ladění. Můžete použít aplikaci Fiddler zobrazíte provozu protokolu HTTP a také na vytvoření požadavků protokolu HTTP, která umožňuje plnou kontrolu nad hlavičky protokolu HTTP v požadavku.

## <a name="see-this-app-running-on-azure"></a>Tato aplikace spuštěné v Azure

Chcete zobrazit dokončení web spuštěný jako živou webovou aplikaci? Úplnou verzi aplikace můžete nasadit ke svému účtu Azure jednoduše kliknutím na následující tlačítko.

[![](http://azuredeploy.net/deploybutton.png)](https://deploy.azure.com/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebAPI-ProductsApp#/form/setup)

Potřebujete účet Azure k nasazení tohoto řešení do Azure. Pokud již účet nemáte, máte následující možnosti:

- [Zdarma otevřít účet Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -získáte kredity, můžete použít k vyzkoušení placených služeb Azure a i po jejich použití až můžete účet ponechat a používat bezplatné služby Azure.
- [Aktivovat výhody pro předplatitele MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -vaše předplatné MSDN vám dává kredity každý měsíc, které můžete použít pro placené služby Azure.

## <a name="next-steps"></a>Další kroky

- Více kompletní příklad, jak služby protokolu HTTP, který podporuje akce POST, PUT a DELETE a zapisuje do databáze, najdete v části [pomocí webového rozhraní API 2 s Entity Framework 6](../data/using-web-api-with-entity-framework/part-1.md).
- Další informace o vytváření plynulá práce a rychlé reakce webových aplikací na základě služby HTTP najdete v tématu [jedné stránky aplikace ASP.NET](../../../single-page-application/index.md).
- Informace o tom, jak nasadit webový projekt sady Visual Studio do služby Azure App Service najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).
