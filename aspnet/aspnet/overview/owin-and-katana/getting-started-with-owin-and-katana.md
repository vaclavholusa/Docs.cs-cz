---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Začínáme se specifikací OWIN a Katana | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913174"
---
<a name="getting-started-with-owin-and-katana"></a>Začínáme se specifikací OWIN a Katana
====================
podle [Mike Wasson](https://github.com/MikeWasson)

[Otevřete Web Interface pro .NET (OWIN)](http://owin.org/) definuje abstrakce mezi .NET webové servery a webové aplikace. Oddělením webový server z aplikace OWIN usnadňuje vytvoření middleware pro vývoj webových aplikací .NET. Navíc OWIN usnadňuje port webové aplikace do jiných hostitelů&#8212;například samoobslužné hostování ve službě Windows nebo jiný proces.

OWIN je specifikace vlastnictví komunity, ne implementace. Projektu Katana je sada součástí OWIN open source vyvinutý microsoftem. Obecný přehled o OWIN a Katana, naleznete v tématu [Přehled projektu Katana](an-overview-of-project-katana.md). V tomto článku můžu se rovnou do kódu, abyste mohli začít.

Tento kurz používá [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), ale můžete také použít Visual Studio 2012. Některé z kroků se liší v sadě Visual Studio 2012, který jsem mějte na paměti následující.

## <a name="host-owin-in-iis"></a>Hostování specifikace OWIN ve službě IIS

V této části budeme hostovat zde OWIN ve službě IIS. Tato možnost poskytuje flexibilitu a skládání kanál OWIN spolu s sadě až po zralé funkcí služby IIS. Použití této možnosti spuštění aplikace OWIN v kanálu požadavku ASP.NET.

Nejprve vytvořte nový projekt webové aplikace ASP.NET. (V sadě Visual Studio 2012, použijte typ projektu prázdná webová aplikace ASP.NET.)

![](getting-started-with-owin-and-katana/_static/image1.png)

V **nový projekt ASP.NET** dialogového okna, vyberte **prázdný** šablony.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Přidání balíčků NuGet

Dále přidejte požadované balíčky NuGet. Z **nástroje** příkaz **Správce balíčků NuGet**, vyberte **konzoly Správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Přidejte třídu pro spuštění

Dále přidejte třídy pro spuštění OWIN. V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **přidat**a pak vyberte **nová položka**. V **přidat novou položku** dialogového okna, vyberte **třída Owin Startup**. Další informace o konfiguraci třída při spuštění najdete v tématu [rozpoznání spouštěcí třídy OWIN](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Přidejte následující kód, který `Startup1.Configuration` metody:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Tento kód přidá do kanálu OWIN implementovány jako funkce, která přijímá jednoduché část middleware **Microsoft.Owin.IOwinContext** instance. Když server přijme požadavek HTTP, vyvolá se v kanálu OWIN middleware. Middleware nastaví typ obsahu pro odpověď a zapíše text odpovědi.

> [!NOTE]
> Šablona třídy OWIN Startup je k dispozici v sadě Visual Studio 2013. Pokud používáte sadu Visual Studio 2012, stačí přidat novou prázdnou třídu s názvem `Startup1`a vložte následující kód:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Spuštění aplikace

Stisknutím klávesy F5 spusťte ladění. Visual Studio se otevře okno prohlížeče a `http://localhost:*port*/`. Na stránce by měl vypadat nějak takto:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Hostování na vlastním serveru OWIN v konzolové aplikaci

Je snadno převést tuto aplikaci ze služby IIS hostování až po samoobslužné hostování ve vlastním procesu. S hostování IIS IIS funguje jako HTTP server a jako proces, který je hostitelem služby. Hostování na vlastním vaše aplikace vytvoří procesu a používá **HttpListener** třídu jako HTTP server.

V sadě Visual Studio vytvořte novou konzolovou aplikaci. V okně konzoly Správce balíčků zadejte následující příkaz:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Přidat `Startup1` třídy do projektu z části 1 tohoto kurzu. Není nutné upravovat této třídy.

Implementace aplikace `Main` metodu následujícím způsobem.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Když spustíte aplikaci konzoly, spustí se server naslouchá na `http://localhost:9000`. Když přejdete na tuto adresu ve webovém prohlížeči, se zobrazí stránka "Hello world".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Přidat OWIN diagnostiky

Balíček Microsoft.Owin.Diagnostics obsahuje middleware, který zachytává neošetřené výjimky a zobrazí stránku HTML s podrobnosti o chybě. Podobně jako chybová stránka technologie ASP.NET, který se někdy označuje jako funkce v této stránky "[žlutý obrazovky smrti](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Podobně jako YSOD chybová stránka Katana je užitečné při vývoji, ale je dobrým zvykem zakázat v provozním režimu.

K instalaci balíčku diagnostiky ve vašem projektu, zadejte následující příkaz v okně konzoly Správce balíčků:

`install-package Microsoft.Owin.Diagnostics –Pre`

Změnit kód ve vašich `Startup1.Configuration` metodu následujícím způsobem:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Teď pomocí kombinace kláves CTRL + F5 spusťte aplikaci bez ladění, tak, aby Visual Studio nebudou porušovat na výjimku. Aplikace se chová stejně jako dříve, dokud přejdete na `http://localhost/fail`, kdy aplikace vyvolá výjimku. Middleware chybové stránky, se zachytit výjimku a zobrazí stránku HTML s informacemi o chybě. Můžete kliknout na karty se můžete podívat zásobníku, řetězec dotazu, soubory cookie, hlavička požadavku a proměnných prostředí OWIN.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Další kroky

- [Rozpoznání spouštěcí třídy OWIN](owin-startup-class-detection.md)
- [Použití rozhraní OWIN k samoobslužnému hostování webového rozhraní API ASP.NET](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Použití rozhraní OWIN k samoobslužnému hostování SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
