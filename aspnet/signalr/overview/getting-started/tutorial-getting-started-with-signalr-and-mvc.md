---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: "Kurz: Začínáme s SignalR 2 a MVC 5 | Microsoft Docs"
author: pfletcher
description: "Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí ASP.NET SignalR 2. SignalR přidáte do aplikace MVC 5 a vytvořit zobrazení chat..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.openlocfilehash: 96a6f6446b26d96b2bcffe4354375ab9c444bbbb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-2-and-mvc-5"></a>Kurz: Začínáme s SignalR 2 a MVC 5
====================
podle [Patrik Fletcher](https://github.com/pfletcher), [Tim Teebken](https://github.com/timlt)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Getting-Started-with-c366b2f3)

> Tento kurz ukazuje, jak vytvořit v reálném čase chatovací aplikace pomocí ASP.NET SignalR 2. SignalR přidáte do aplikace MVC 5 a vytvořit zobrazení chatu a odesílat zprávy o zobrazení. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - MVC 5
> - SignalR verze 2
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>Tento kurz pomocí sady Visual Studio 2012
> 
> 
> Pokud chcete používat Visual Studio 2012 v tomto kurzu, postupujte takto:
> 
> - Aktualizace vašeho [Správce balíčků](http://docs.nuget.org/docs/start-here/installing-nuget) na nejnovější verzi.
> - Nainstalujte [webové platformy](https://www.microsoft.com/web/downloads/platform.aspx).
> - Ve webové platformy, vyhledání a instalace **ASP.NET a webové nástroje 2013.1 pro sadu Visual Studio 2012**. Šablony sady Visual Studio pro třídy SignalR dojde k instalaci, jako **rozbočovače**.
> - Některé šablony (jako například **třídy pro spuštění OWIN**) nebudou k dispozici; pro tyto třídy soubor použít místo.
> 
> 
> ## <a name="tutorial-versions"></a>Kurz verze
> 
> Informace o předchozích verzích SignalR najdete v tématu [starší verze funkce SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Přehled

Tento kurz vás seznámí s vývoj v reálném čase webové aplikace s ASP.NET SignalR 2 a ASP.NET MVC 5. Tento kurz používá stejný kód aplikace chat jako [kurzu Začínáme SignalR](tutorial-getting-started-with-signalr.md), ale ukazuje, jak přidat do aplikace MVC 5.

V tomto tématu se dozvíte následující úlohy vývoj SignalR:

- Přidání do aplikace MVC 5 knihovně SignalR.
- Vytvoření třídy spuštění OWIN tak, aby nabízel obsah pro klienty a rozbočovače.
- K odesílání zpráv a zobrazení aktualizací z centra pomocí knihovny jQuery SignalR na webové stránce.

Následující snímek obrazovky ukazuje dokončené chatovací aplikace spuštěné v prohlížeči.

![Instance chatu](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

Části:

- [Nastavení projektu](#setup)
- [Spustit ukázku](#run)
- [Zkontrolujte kód](#code)
- [Další kroky](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>Nastavení projektu

Předpoklady:

- Visual Studio 2013. Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2013 Express vývoj nástroj.

V této části ukazuje, jak vytvořit aplikaci ASP.NET MVC 5, přidejte knihovně SignalR a vytvoření chatovací aplikace.

1. V sadě Visual Studio vytvořte aplikaci C# ASP.NET s cílem rozhraní .NET Framework 4.5, pojmenujte ji SignalRChat a klikněte na tlačítko OK.

    ![Vytvořit web](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)
2. V `New ASP.NET Project` dialogové okno a vyberte **MVC**a klikněte na tlačítko **změna ověřování**.

    ![Vytvořit web](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)
3. Vyberte **bez ověřování** v **změna ověřování** dialogové okno a klikněte na tlačítko **OK**.

    ![Vyberte bez ověřování](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

    > [!NOTE]
    > Pokud vyberete zprostředkovatele různých ověřování pro aplikaci, `Startup.cs` pro vás bude možné vytvořit třídu; nebudete muset vytvořit vlastní `Startup.cs` – třída v kroku 10 níže.
4. Klikněte na tlačítko **OK** v **nový projekt ASP.NET** dialogové okno.
5. Otevřete **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spusťte následující příkaz. Tento krok přidává do projektu sadu souborů skriptů a odkazy na sestavení, které umožňují funkce SignalR.

    `install-package Microsoft.AspNet.SignalR`
6. V **Průzkumníku**, rozbalte složku skripty. Všimněte si, že byly přidány do projektu knihovny skriptů pro SignalR.

    ![Složka skripty](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)
7. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Nová složka**, a přidejte novou složku s názvem **Hubs**.
8. Klikněte pravým tlačítkem myši **Hubs** složku, klikněte na tlačítko **přidat | Nová položka**, vyberte **Visual C# | Web | SignalR** uzlu **nainstalovaná** podokně, vyberte **třídy rozbočovače SignalR (v2)** z podokna center a vytvoření nového centra s názvem **ChatHub.cs**. Tato třída použije jako server rozbočovače SignalR zasílání zpráv na všechny klienty.

    ![Vytvoření nového centra](tutorial-getting-started-with-signalr-and-mvc/_static/image6.png)
9. Nahraďte kód v **ChatHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]
10. Vytvořte novou třídu s názvem souboru Startup.cs. Změňte obsah souboru následujícím způsobem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]
11. Upravit `HomeController` nalezena třída v **Controllers/HomeController.cs** a přidejte následující metodu do třídy. Tato metoda vrátí hodnotu **Chat** zobrazení, který chcete vytvořit později.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]
12. Klikněte pravým tlačítkem myši **zobrazení Domů** složky a vyberte **přidat... | Zobrazení**.
13. V **přidat zobrazení** dialogové okno, název nového zobrazení **Chat**.

    ![Přidání zobrazení](tutorial-getting-started-with-signalr-and-mvc/_static/image7.png)
14. Nahraďte obsah **Chat.cshtml** následujícím kódem.

    > [!IMPORTANT]
    > Když přidáte do projektu sady Visual Studio SignalR a další skript knihovny, může správce balíčků nainstalujte verzi souboru skriptu SignalR, která je novější než verze uvedené v tomto tématu. Ujistěte se, že odkaz na skript v kódu odpovídá verzi knihovny skriptu nainstalovaná ve vašem projektu.

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]
15. **Uložte všechny** pro projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Spustit ukázku

1. Stisknutím klávesy F5 spusťte projekt v režimu ladění.
2. V adresním řádku prohlížeče, připojit **/home/chat** na adresu URL výchozí stránky pro projekt. Načtení stránky Chat v instanci prohlížeče a vyzve k uživatelské jméno.

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr-and-mvc/_static/image8.png)
3. Zadejte uživatelské jméno.
4. Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče. V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.
5. V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**. Komentáře by měl zobrazit ve všech instancích prohlížeče.

    > [!NOTE]
    > Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru. Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele. Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.
6. Následující snímek obrazovky ukazuje chatovací aplikace spuštěné v prohlížeči.

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr-and-mvc/_static/image9.png)
7. V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci. Tento uzel je viditelný v režimu ladění, pokud používáte Internet Explorer jako prohlížeč. Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu. Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru. Pokud používáte jiný prohlížeč než Internet Explorer, můžete taky přejít dynamická **centra** soubor procházením ji přímo, například http://mywebsite/signalr/hubs.

<a id="code"></a>

## <a name="examine-the-code"></a>Zkontrolujte kód

Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.

### <a name="signalr-hubs"></a>Rozbočovače SignalR

V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy. Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR. Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z skriptů na webové stránce.

V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy. Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.addNewMessageToPage**.

**Odeslat** metoda ukazuje několik konceptů rozbočovače:

- Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.
- Použití **Microsoft.AspNet.SignalR.Hub.Clients** vlastnost, která má přístup všichni klienti připojení k této rozbočovače.
- Volání funkce na straně klienta (například `addNewMessageToPage` funkce) k aktualizaci klientů.

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR a jQuery

**Chat.cshtml** zobrazení souboru v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR. Základních úloh v kódu vytváříte odkaz na automaticky generovaný proxy serveru pro rozbočovač, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a spuštění připojení k odesílání zpráv do rozbočovače.

Následující kód deklaruje odkaz na proxy server rozbočovače.

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> V jazyce JavaScript je odkaz na třídě serveru a její členy v camelCase. Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jazyce JavaScript jako **chatHub**. Pokud chcete odkazovat `ChatHub` – třída v jQuery s konvenční Pascal velká a malá písmena stejně jako v jazyce C#, upravte soubor ChatHub.cs třídy. Přidat `using` příkaz tak, aby odkazovaly `Microsoft.AspNet.SignalR.Hubs` oboru názvů. Pak přidejte `HubName` atribut `ChatHub` třídy, například `[HubName("ChatHub")]`. Nakonec aktualizujte vaši jQuery informaci, abyste `ChatHub` třídy.


Následující kód ukazuje, jak vytvořit funkci zpětného volání ve skriptu. Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů. Volitelné volání `htmlEncode` funkce zobrazí způsob, jak HTML kódování obsahu zprávy, než se zobrazí na stránce jako způsob, jak zabránit vložení skriptu.

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

Následující kód ukazuje, jak k otevření připojení do centra. Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce konverzace.

> [!NOTE]
> Tento přístup zajišťuje, že připojení před provedením obslužné rutiny události.


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>Další kroky

Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase. Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.

Podrobný postup nasazení ukázkové aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-get-started/).

Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
