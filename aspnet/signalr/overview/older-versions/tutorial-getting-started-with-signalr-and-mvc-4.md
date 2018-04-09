---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'Kurz: Začínáme s SignalR 1.x a MVC 4 | Microsoft Docs'
author: pfletcher
description: Sestavení aplikace chat v reálném čase pomocí funkce SignalR technologie ASP.NET a ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 1ae330be5caf00c3cac7451f326398c0958538af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>Kurz: Začínáme s SignalR 1.x a MVC 4
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí funkce SignalR technologie ASP.NET. SignalR přidáte do aplikace MVC 4 a vytvořit zobrazení chatu a odesílat zprávy o zobrazení.


## <a name="overview"></a>Přehled

Tento kurz vás seznámí s vývoj aplikací webu v reálném čase pomocí funkce SignalR technologie ASP.NET a ASP.NET MVC 4. Tento kurz používá stejný kód aplikace chat jako [kurzu Začínáme SignalR](tutorial-getting-started-with-signalr.md), ale ukazuje, jak přidat do aplikace MVC 4, který je založený na šabloně Internetu.

V tomto tématu se dozvíte následující úlohy vývoj SignalR:

- Přidání knihovně SignalR do aplikace MVC 4.
- Vytvoření třídy rozbočovače pro uložení obsahu do klientů.
- K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.

Následující snímek obrazovky ukazuje dokončené chatovací aplikace spuštěné v prohlížeči.

![Instance chatu](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

Části:

- [Nastavení projektu](#setup)
- [Spustit ukázku](#run)
- [Zkontrolujte kód](#code)
- [Další kroky](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Nastavení projektu

Předpoklady:

- Visual Studio 2010 SP1, Visual Studio 2012 nebo Visual Studio 2012 Express. Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2012 Express vývoj nástroj.
- Pro Visual Studio 2010, nainstalujte [ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683).

V této části ukazuje, jak vytvořit aplikaci ASP.NET MVC 4, přidejte knihovně SignalR a vytvoření chatovací aplikace.

1. 1. V sadě Visual Studio vytvořit aplikaci ASP.NET MVC 4, název SignalRChat a klikněte na tlačítko OK.

        > [!NOTE]
        > V VS 2010, vyberte **rozhraní .NET Framework 4** v rozevíracím seznamu Framework verze. Spustí kód SignalR na rozhraní .NET Framework verze 4 a 4.5.

        ![Vytvoření webové mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. Vyberte šablonu, internetovou aplikaci, zrušte zaškrtnutí políčka pro **vytvoření projektu testování částí**a klikněte na tlačítko OK.

         ![Vytvoření webu na Internetu mvc](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. Otevřete **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spusťte následující příkaz. Tento krok přidává do projektu sadu souborů skriptů a odkazy na sestavení, které umožňují funkce SignalR.

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. V **Průzkumníku řešení** rozbalte složku skripty. Všimněte si, že byly přidány do projektu knihovny skriptů pro SignalR.

         ![Knihovna odkazy](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Nová složka**, a přidejte novou složku s názvem **Hubs**.
      6. Klikněte pravým tlačítkem myši **Hubs** složku, klikněte na tlačítko **přidat | Třída**a vytvořte novou třídu C#, s názvem **ChatHub.cs**. Tato třída použije jako server rozbočovače SignalR zasílání zpráv na všechny klienty.

> [!NOTE]
> Pokud používáte Visual Studio 2012 a nainstalovaný [ASP.NET a webové nástroje 2012.2 aktualizace](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation), můžete použít novou šablonu SignalR položku pro vytvoření třídy rozbočovače. To lze provést, klikněte pravým tlačítkem myši **centra** složku, klikněte na tlačítko **přidat | Nová položka**, vyberte **třídy rozbočovače SignalR (v1)**a název třídy **ChatHub.cs**.


1. Nahraďte kód v **ChatHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. Otevřete **Global.asax** souboru pro projekt a přidejte volání do metody `RouteTable.Routes.MapHubs();` jako první řádek kódu `Application_Start` metoda. Tento kód registruje výchozí trasu pro rozbočovače SignalR a musí být voláno předtím, než zaregistrujete jiným trasám. Dokončené `Application_Start` metoda vypadá jako v následujícím příkladu.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. Upravit `HomeController` nalezena třída v **Controllers/HomeController.cs** a přidejte následující metodu do třídy. Tato metoda vrátí hodnotu **Chat** zobrazení, který chcete vytvořit později.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. Klikněte pravým tlačítkem myši v rámci `Chat` metoda jste právě vytvořili a klikněte na tlačítko **přidat zobrazení** k vytvoření nového souboru zobrazení.
5. V **přidat zobrazení** dialogové okno, ujistěte se, že je zaškrtnuté políčko **použít rozložení stránky předlohy** (zrušte zaškrtnutí políček jiných) a potom klikněte na **přidat**.

    ![Přidání zobrazení](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. Upravit nové zobrazení soubor s názvem **Chat.cshtml**. Po &lt;h2&gt; značky, vložte následující &lt;div&gt; části a `@section scripts` blok kódu na stránku. Tento skript umožňuje stránku k odeslání chatovací zprávy a zobrazení zpráv ze serveru. Kód dokončení chat zobrazení se zobrazí v následující blok kódu.

    > [!IMPORTANT]
    > Když přidáte do projektu sady Visual Studio SignalR a další skript knihovny, může správce balíčků nainstalujte verze skriptů, které jsou novější než verze uvedené v tomto tématu. Ujistěte se, že odkazům na skript v kódu odpovídají verze knihoven skript, který je nainstalován ve vašem projektu.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **Uložte všechny** pro projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Spustit ukázku

1. Stisknutím klávesy F5 spusťte projekt v režimu ladění.
2. V adresním řádku prohlížeče, připojit **/home/chat** na adresu URL výchozí stránky pro projekt. Načtení stránky Chat v instanci prohlížeče a vyzve k uživatelské jméno.

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. Zadejte uživatelské jméno.
4. Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče. V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.
5. V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**. Komentáře by měl zobrazit ve všech instancích prohlížeče.

    > [!NOTE]
    > Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru. Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele. Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.
6. Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči.

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci. Tento uzel je viditelný v režimu ladění, pokud používáte Internet Explorer jako prohlížeč. Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu. Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru. Pokud používáte jiný prohlížeč než Internet Explorer, můžete taky přejít dynamická **hubs** souboru tak, že přejde k němu přímo, například http://mywebsite/signalr/hubs.

    ![Vygenerovaný centra skriptů](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Zkontrolujte kód

Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.

### <a name="signalr-hubs"></a>Rozbočovače SignalR

V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy. Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR. Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z jQuery skriptů na webové stránce.

V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy. Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.addNewMessageToPage**.

**Odeslat** metoda ukazuje několik konceptů rozbočovače:

- Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.
- Použití **Microsoft.AspNet.SignalR.Hub.Clients** vlastnost, která má přístup všichni klienti připojení k této rozbočovače.
- Volání funkce jQuery na straně klienta (například `addNewMessageToPage` funkce) k aktualizaci klientů.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR a jQuery

**Chat.cshtml** zobrazení souboru v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR. Základních úloh v kódu vytváříte odkaz na automaticky generovaný proxy serveru pro rozbočovač, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a spuštění připojení k odesílání zpráv do rozbočovače.

Následující kód deklaruje proxy serveru pro rozbočovač.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> V jQuery je odkaz na třídě serveru a její členy v camelCase. Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jQuery jako **chatHub**. Pokud chcete odkazovat `ChatHub` – třída v jQuery s konvenční Pascal velká a malá písmena stejně jako v jazyce C#, upravte soubor ChatHub.cs třídy. Přidat `using` příkaz tak, aby odkazovaly `Microsoft.AspNet.SignalR.Hubs` oboru názvů. Pak přidejte `HubName` atribut `ChatHub` třídy, například `[HubName("ChatHub")]`. Nakonec aktualizujte vaši jQuery informaci, abyste `ChatHub` třídy.


Následující kód ukazuje, jak vytvořit funkci zpětného volání ve skriptu. Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů. Volitelné volání `htmlEncode` funkce zobrazí způsob, jak HTML kódování obsahu zprávy, než se zobrazí na stránce jako způsob, jak zabránit vložení skriptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

Následující kód ukazuje, jak k otevření připojení do centra. Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce konverzace.

> [!NOTE]
> Tento přístup zajišťuje, že připojení před provedením obslužné rutiny události.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Další kroky

Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase. Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.

Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
