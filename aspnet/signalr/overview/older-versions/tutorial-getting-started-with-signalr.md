---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "Kurz: Začínáme s SignalR 1.x | Microsoft Docs"
author: pfletcher
description: "Sestavení aplikace chat v reálném čase na stránce HTML pomocí funkce SignalR technologie ASP.NET."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>Kurz: Začínáme s SignalR 1.x
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

> V tomto kurzu se naučíte používat funkci SignalR k vytvoření aplikace pro chatování v reálném čase. Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvořit stránku HTML a odesílat zprávy o zobrazení.


## <a name="overview"></a>Přehled

Tento kurz představuje SignalR vývoj ukazuje, jak sestavit aplikaci Jednoduchý chat založené na prohlížeči. Přidáte knihovně SignalR prázdnou webovou aplikaci ASP.NET, vytvořte třídu rozbočovače pro odesílání zpráv do klientů a vytvořit stránku HTML, která umožňuje uživatelům odesílat a přijímat zprávy v konverzaci. Podobný kurz, který ukazuje vytvoření chatovací aplikace v MVC 4 pomocí zobrazení MVC, najdete v části [Začínáme s SignalR a MVC 4](index.md).

> [!NOTE]
> Tento kurz používá (1.x) verzi SignalR. Podrobnosti o změny mezi SignalR 1.x a 2.0, najdete v části [upgrade SignalR 1.x projekty](../releases/upgrading-signalr-1x-projects-to-20.md).

SignalR je knihovna .NET open source pro tvorbu webových aplikací, které vyžadují zásah uživatele za provozu nebo aktualizace dat v reálném čase. Mezi příklady patří sociálních aplikací, s více uživateli hry, obchodní spolupráce a zprávy, počasí nebo finančních aktualizaci aplikací. Toto nastavení se často nazývá aplikací v reálném čase.

SignalR zjednodušuje proces vytváření aplikace v reálném čase. Obsahuje knihovnu serveru server ASP.NET a knihovny JavaScript klienta, aby bylo snazší spravovat připojení klient server a předat obsahu aktualizace do klientů. Knihovna SignalR můžete přidat do existující aplikace ASP.NET pro získání funkce v reálném čase.

Tento kurz ukazuje následující úlohy vývoj SignalR:

- Přidání knihovně SignalR k webové aplikaci ASP.NET.
- Vytvoření třídy rozbočovače pro uložení obsahu do klientů.
- K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.

Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči. Každý nový uživatel, můžete odeslat komentáře a najdete v části poznámky přidané po uživatel připojí chat.

![Instance chatu](tutorial-getting-started-with-signalr/_static/image1.png)

Části:

- [Nastavení projektu](#setup)
- [Spustit ukázku](#run)
- [Zkontrolujte kód](#code)
- [Další kroky](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Nastavení projektu

V této části ukazuje, jak vytvořit prázdnou webovou aplikaci ASP.NET, přidejte SignalR a vytvoření chatovací aplikace.

Předpoklady:

- Visual Studio 2010 SP1 nebo 2012. Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2012 Express vývoj nástroj.
- [Microsoft ASP.NET a webové nástroje 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941). Pro sadu Visual Studio 2012 tento instalační program přidá nové funkce ASP.NET, včetně SignalR šablony sady Visual Studio. Pro Visual Studio 2010 SP1 instalační program není k dispozici, ale můžete dokončit kurz nainstalováním balíček SignalR NuGet, jak je popsáno v krocích instalační program.

Následující postup použijte sadu Visual Studio 2012 vytvoření prázdné webové aplikace ASP.NET a přidání knihovně SignalR:

1. V sadě Visual Studio vytvořte prázdný webové aplikace ASP.NET.

    ![Vytvořit prázdnou webovou](tutorial-getting-started-with-signalr/_static/image2.png)
2. Otevřete **Konzola správce balíčků** výběrem **nástroje | Správce balíčků knihoven | Konzola správce balíčků**. Do okna konzoly, zadejte následující příkaz:

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    Tento příkaz nainstaluje nejnovější verzi SignalR 1.x.
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třída**. Pojmenujte novou třídu **ChatHub**.
4. V **Průzkumníku řešení** rozbalte uzel skripty. Jsou viditelné v projektu knihovny skriptů pro knihovny jQuery a SignalR.

    ![Knihovna odkazy](tutorial-getting-started-with-signalr/_static/image3.png)
5. Nahraďte kód v **ChatHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**. V **přidat novou položku** dialogovém okně, vyberte **globální třídy aplikace** a klikněte na tlačítko **přidat**.

    ![Přidejte globální](tutorial-getting-started-with-signalr/_static/image4.png)
7. Přidejte následující `using` příkazy po poskytnutého `using` příkazy ve třídě Global.asax.cs.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. Přidejte následující řádek kódu `Application_Start` metoda globální třídy k registraci výchozí trasu pro rozbočovače SignalR.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Nová položka**. V **přidat novou položku** dialogové okno, vyberte stránku Html a klikněte na **přidat**.
10. V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku HTML, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.
11. Ve výchozím kódu na stránce HTML nahraďte následujícím kódem.

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **Uložte všechny** pro projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Spustit ukázku

1. Stisknutím klávesy F5 spusťte projekt v režimu ladění. Načtení stránky HTML v instanci prohlížeče a vyzve k uživatelské jméno.

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr/_static/image5.png)
2. Zadejte uživatelské jméno.
3. Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče. V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.
4. V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**. Komentáře by měl zobrazit ve všech instancích prohlížeče.

    > [!NOTE]
    > Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru. Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele. Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.

    Následující snímek obrazovky ukazuje chatovací aplikace běžící v tři instance prohlížeče, které aktualizují po jedné instance odešle zprávu:

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr/_static/image6.png)
5. V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci. Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu. Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru.

    ![Vygenerovaný centra skriptů](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Zkontrolujte kód

Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.

### <a name="signalr-hubs"></a>Rozbočovače SignalR

V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy. Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR. Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z jQuery skriptů na webové stránce.

V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy. Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.broadcastMessage**.

**Odeslat** metoda ukazuje několik konceptů rozbočovače:

- Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.
- Použití **Microsoft.AspNet.SignalR.Hub.Clients** dynamických vlastností pro přístup k všechny klienty připojené k této rozbočovače.
- Volání funkce jQuery na straně klienta (například `broadcastMessage` funkce) k aktualizaci klientů.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR a jQuery

Stránky HTML v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR. Proxy server tak, aby odkazovaly rozbočovače, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a počáteční připojení k odeslání zprávy do centra jsou deklarace základních úloh v kódu.

Následující kód deklaruje proxy serveru pro rozbočovač.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> V jQuery je odkaz na třídě serveru a její členy v camelCase. Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jQuery jako **chatHub**.


Následující kód je, jak vytvořit funkci zpětného volání ve skriptu. Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů. Dva řádky, HTML kódování obsahu před jejich zobrazením jsou volitelné a zobrazit jednoduchý způsob, jak zabránit vložení skriptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

Následující kód ukazuje, jak k otevření připojení do centra. Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce HTML.

> [!NOTE]
> Tento přístup zajistí, že připojení před provedením obslužné rutiny události.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Další kroky

Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase. Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.

Můžete zpřístupnit ukázkové aplikace v tomto kurzu nebo jinými aplikacemi SignalR přes Internet pomocí jejich nasazení do hostujícího zprostředkovatele. Společnost Microsoft nabízí bezplatné webových hostitelských služeb pro až 10 webových serverů ve bezplatný [zkušební účet systému Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Podrobný postup nasazení ukázkové aplikace SignalR, najdete v části [SignalR získávání spustit ukázku jako webu systému Windows Azure publikování](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx). Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [nasazení aplikace ASP.NET na webu systému Windows Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet). (Poznámka: přenos protokolu WebSocket aktuálně nepodporuje pro weby systému Windows Azure. Přenos protokolu WebSocket když není k dispozici, SignalR používá k dispozici přenosy, jak je popsáno v části přenosy [Úvod k tématu SignalR](index.md).)

Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
