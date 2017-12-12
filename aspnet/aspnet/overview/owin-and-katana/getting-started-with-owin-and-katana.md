---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "Začínáme s OWIN a Katana | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a>Začínáme s OWIN a Katana
====================
podle [Wasson Jan](https://github.com/MikeWasson)

[Otevřete Web Interface pro .NET (OWIN)](http://owin.org/) definuje abstrakci mezi .NET webové servery a webové aplikace. Protože webový server z aplikace, OWIN usnadňuje vytvoření middleware pro vývoj webů .NET. Navíc OWIN usnadňuje port webové aplikace k jiným hostitelům &#8212; například vlastní hostování služby systému Windows nebo jiný proces.

OWIN je specifikace vlastněných komunity, není implementace. Projekt Katana je sada open-source OWIN součásti vyvinuté společností Microsoft. Obecné přehled OWIN a Katana, najdete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md). V tomto článku I se pustíme přímo do kódu začít pracovat.

Tento kurz používá [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale můžete také použít Visual Studio 2012. Pár kroků se liší v sadě Visual Studio 2012, který beru na vědomí následující.

## <a name="host-owin-in-iis"></a>Hostitele OWIN ve službě IIS

V této části Pořádáme OWIN ve službě IIS. Tato možnost vám poskytuje flexibilitu a composability kanálu OWIN společně s sadu vyspělé funkce služby IIS. Použití této možnosti se aplikace OWIN spouští kanál požadavku.

Nejdřív vytvořte nový projekt webové aplikace ASP.NET. (V sadě Visual Studio 2012, použijte typ projektu prázdný webové aplikace ASP.NET.)

![](getting-started-with-owin-and-katana/_static/image1.png)

V **nový projekt ASP.NET** dialogovém okně, vyberte **prázdný** šablony.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Přidání balíčků NuGet

Dál přidejte požadované balíčky NuGet. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**, pak vyberte **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Přidání třídy spuštění

Dál přidejte třídy pro spuštění OWIN. V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **přidat**, pak vyberte **novou položku**. V **přidat novou položku** dialogovém okně, vyberte **třídy pro spuštění Owin**. Další informace o konfiguraci třída při spuštění najdete v tématu [OWIN při spuštění třída detekce](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Přidejte následující kód, který `Startup1.Configuration` metoda:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Tento kód přidá do kanálu OWIN, implementovaný jako funkci, která přijímá jednoduché druhem middleware **Microsoft.Owin.IOwinContext** instance. Když server obdrží požadavek HTTP, vyvolá kanálu OWIN middleware. Middleware nastaví typ obsahu pro odpověď a zapíše text odpovědi.

> [!NOTE]
> Šablona třídy OWIN při spuštění je k dispozici v sadě Visual Studio 2013. Pokud používáte Visual Studio 2012, stačí přidat nový prázdný třídy s názvem `Startup1`a vložte následující kód:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Spuštění aplikace

Stisknutím klávesy F5 spusťte ladění. Visual Studio se otevře okno prohlížeče a `http://localhost:*port*/`. Stránce by měl vypadat asi takto:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Vlastní hostování OWIN v konzolové aplikaci

Je snadné převést hostování IIS pro vlastní hostování v vlastní proces, který tuto aplikaci. S hostování IIS IIS funguje jako HTTP server a jako proces tohoto hostitele k serveru. S vlastní hostování, vaše aplikace vytvoří proces a použije **HttpListener** třída jako HTTP server.

V sadě Visual Studio vytvořte novou konzolovou aplikaci. V okně konzoly Správce balíčků zadejte následující příkaz:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Přidat `Startup1` třídy z část 1 v tomto kurzu do projektu. Nemusíte upravovat tuto třídu.

Implementovat aplikace `Main` metoda následujícím způsobem.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Když spustíte aplikaci konzoly, server se spustí naslouchání `http://localhost:9000`. Pokud přejdete na tuto adresu ve webovém prohlížeči, zobrazí se stránka "Hello, world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Přidat OWIN Diagnostics.

Balíček Microsoft.Owin.Diagnostics obsahuje middleware, který zachycuje neošetřené výjimky a zobrazí stránku HTML s podrobnosti o chybě. Tato stránka funguje jako mnohem chybová stránka technologie ASP.NET, která se někdy označuje jako "[žlutý obrazovka smrti](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Podobně jako YSOD chybová stránka Katana je užitečné při vývoji, ale je dobrým zvykem zakázat v produkčním režimu.

K instalaci balíčku diagnostiky ve vašem projektu, zadejte následující příkaz v okně konzoly Správce balíčků:

`install-package Microsoft.Owin.Diagnostics –Pre`

Změnit kód ve vaší `Startup1.Configuration` metoda následujícím způsobem:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Teď použijte CTRL + F5 a spusťte aplikaci bez ladění, tak, aby Visual Studio nebude rozdělit na výjimku. Aplikace se chová stejně jako předtím, dokud přejděte na `http://localhost/fail`, na který bod aplikace vyvolá výjimku. Middleware chybové stránky se zachycení výjimky a zobrazit stránku HTML s informace o této chybě. Kliknutím na karty zobrazíte zásobníku, řetězec dotazu, soubory cookie, hlavička požadavku a proměnných prostředí OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Další kroky

- [Detekce třídy pro spuštění OWIN](owin-startup-class-detection.md)
- [Použít k hostování na vlastním rozhraní ASP.NET Web API OWIN](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Hostování na vlastním SignalR pomocí OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
