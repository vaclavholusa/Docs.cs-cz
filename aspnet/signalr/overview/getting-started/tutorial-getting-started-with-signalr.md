---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 'Kurz: Začínáme s SignalR 2 | Microsoft Docs'
author: pfletcher
description: V tomto kurzu se naučíte používat funkci SignalR k vytvoření aplikace pro chatování v reálném čase. Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvoření pa HTML...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>Kurz: Začínáme s SignalR 2
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> V tomto kurzu se naučíte používat funkci SignalR k vytvoření aplikace pro chatování v reálném čase. Přidáte SignalR prázdnou webovou aplikaci ASP.NET a vytvořit stránku HTML a odesílat zprávy o zobrazení. 
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
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

Tento kurz představuje SignalR vývoj ukazuje, jak sestavit aplikaci Jednoduchý chat založené na prohlížeči. Přidáte knihovně SignalR prázdnou webovou aplikaci ASP.NET, vytvořte třídu rozbočovače pro odesílání zpráv do klientů a vytvořit stránku HTML, která umožňuje uživatelům odesílat a přijímat zprávy v konverzaci. Podobný kurz, který ukazuje vytvoření chatovací aplikace v MVC 5 pomocí zobrazení MVC, najdete v části [Začínáme s SignalR 2 a MVC 5](tutorial-getting-started-with-signalr-and-mvc.md).

> [!NOTE]
> Tento kurz ukazuje, jak vytvářet aplikace SignalR ve verze 2. Podrobnosti o změny mezi SignalR 1.x a 2, najdete v části [projekty 1.x upgrade SignalR](../releases/upgrading-signalr-1x-projects-to-20.md) a [k verzi Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md#TOC13).

SignalR je knihovna .NET open source pro tvorbu webových aplikací, které vyžadují zásah uživatele za provozu nebo aktualizace dat v reálném čase. Mezi příklady patří sociálních aplikací, s více uživateli hry, obchodní spolupráce a zprávy, počasí nebo finančních aktualizaci aplikací. Toto nastavení se často nazývá aplikací v reálném čase.

SignalR zjednodušuje proces vytváření aplikace v reálném čase. Obsahuje knihovnu serveru server ASP.NET a knihovny JavaScript klienta, aby bylo snazší spravovat připojení klient server a předat obsahu aktualizace do klientů. Knihovna SignalR můžete přidat do existující aplikace ASP.NET pro získání funkce v reálném čase.

Tento kurz ukazuje následující úlohy vývoj SignalR:

- Přidání knihovně SignalR k webové aplikaci ASP.NET.
- Vytvoření třídy rozbočovače pro uložení obsahu do klientů.
- Vytvoření třídy pro spuštění OWIN při konfiguraci této aplikace.
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

V této části ukazuje, jak pomocí sady Visual Studio 2013 a SignalR verze 2 můžete vytvořit prázdnou webovou aplikaci ASP.NET, přidejte SignalR a vytvoření chatovací aplikace.

Předpoklady:

- Visual Studio 2013. Pokud nemáte Visual Studio, najdete v části [ASP.NET stáhne](https://www.asp.net/downloads) získat volné Visual Studio 2013 Express vývoj nástroj.

Následující kroky použijte k vytvoření prázdné webové aplikace ASP.NET a přidání knihovny SignalR Visual Studio 2013:

1. V sadě Visual Studio vytvořte webovou aplikaci ASP.NET.

    ![Vytvořit web](tutorial-getting-started-with-signalr/_static/image2.png)
2. V **nový projekt ASP.NET** okno, ponechejte **prázdný** vybraný a klikněte na tlačítko **vytvořit projekt**.

    ![Vytvořit prázdnou webovou](tutorial-getting-started-with-signalr/_static/image3.png)
3. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt, vyberte **přidat | Třídy rozbočovače SignalR (v2)**. Název třídy **ChatHub.cs** a přidejte ji do projektu. Tento krok vytvoří **ChatHub** třídy a přidá do projektu sadu souborů skriptů a odkazy na sestavení, které podporují funkce SignalR.

    > [!NOTE]
    > SignalR můžete také přidat do projektu otevřením **nástroje | Správce balíčků knihoven | Konzola správce balíčků** a spuštění příkazu:

    `install-package Microsoft.AspNet.SignalR`

    Používáte-li přidat SignalR konzole, můžete vytvořte třídy rozbočovače SignalR jako samostatný krok po přidání SignalR.

    > [!NOTE]
    > Pokud používáte Visual Studio 2012, **třídy rozbočovače SignalR (v2)** šablony nebudete mít k dispozici. Můžete přidat prostý **třída** názvem `ChatHub` místo.
4. V **Průzkumníku**, rozbalte uzel skripty. Jsou viditelné v projektu knihovny skriptů pro knihovny jQuery a SignalR.
5. Nahraďte kód v novém **ChatHub** třídy následujícím kódem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Třídy pro spuštění OWIN**. Pojmenujte novou třídu `Startup` a klikněte na tlačítko OK.

    > [!NOTE]
    > Pokud používáte Visual Studio 2012, **třídy pro spuštění OWIN** šablony nebudete mít k dispozici. Můžete přidat prostý **třída** názvem `Startup` místo.
7. Změňte obsah novou třídu spuštění následujícím způsobem.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a pak klikněte na tlačítko **přidat | Stránky HTML**. Zadejte název nové stránky `index.html`.
    >[!NOTE]
    >Možná budete muset změnit čísla verzí pro odkazy na knihovny JQuery a SignalR
9. V **Průzkumníku řešení**, klikněte pravým tlačítkem na stránku HTML, kterou jste právě vytvořili a klikněte na **nastavit jako úvodní stránku**.
10. Ve výchozím kódu na stránce HTML nahraďte následujícím kódem.

    > [!NOTE]
    > Novější verze skriptů SignalR mohou být nainstalovány službou Správce balíčků. Ověřte, zda skript odkazy níže odpovídají verze souborů skriptu v projektu (bude jiné, pokud jste přidali pomocí nástroje NuGet a místo přidávání rozbočovači SignalR.)

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **Uložte všechny** pro projekt.

<a id="run"></a>

## <a name="run-the-sample"></a>Spustit ukázku

1. Stisknutím klávesy F5 spusťte projekt v režimu ladění. Načtení stránky HTML v instanci prohlížeče a vyzve k uživatelské jméno.

    ![Zadejte uživatelské jméno](tutorial-getting-started-with-signalr/_static/image4.png)
2. Zadejte uživatelské jméno.
3. Zkopírujte adresu URL v adresním řádku prohlížeče a použít ho k otevřít dva další instance prohlížeče. V každé instanci prohlížeče zadejte jedinečné uživatelské jméno.
4. V každé instanci prohlížeče přidat komentář a klikněte na tlačítko **odeslat**. Komentáře by měl zobrazit ve všech instancích prohlížeče.

    > [!NOTE]
    > Tento jednoduchý chatovací aplikace neudržuje kontext diskuzi na serveru. Rozbočovač, všesměrově komentáře pro všechny aktuálního uživatele. Uživatelé, kteří připojení chat později zobrazí zprávy přidat od okamžiku že připojí.

    Následující snímek obrazovky ukazuje chatovací aplikace běžící v tři instance prohlížeče, které aktualizují po jedné instance odešle zprávu:

    ![Konverzace prohlížečů](tutorial-getting-started-with-signalr/_static/image5.png)
5. V **Průzkumníku řešení**, zkontrolujte **dokumentů skriptu** uzel pro běžící aplikaci. Existuje soubor skriptu s názvem **centra** vygenerovanou knihovně SignalR dynamicky za běhu. Tento soubor spravuje komunikace mezi skriptu jQuery a kódu na straně serveru.

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>Zkontrolujte kód

Chatovací aplikace SignalR ukazuje dva základní úlohy vývoj SignalR: vytváření rozbočovač jako hlavní koordinaci objekt na serveru a odesílat a přijímat zprávy pomocí knihovny jQuery SignalR.

### <a name="signalr-hubs"></a>Rozbočovače SignalR

V ukázce kódu **ChatHub** třída odvozená z **Microsoft.AspNet.SignalR.Hub** třídy. Odvozování z **rozbočovače** třída je užitečný způsob, jak sestavit aplikaci pro SignalR. Můžete vytvořit veřejné metody na vaší třídě rozbočovače a pak tyto metody přístup voláním z skriptů na webové stránce.

V kódu chat klienti volání **ChatHub.Send** metodu pro odeslání nové zprávy. Rozbočovače pak odešle zprávu do všech klientů voláním **Clients.All.broadcastMessage**.

**Odeslat** metoda ukazuje několik konceptů rozbočovače:

- Deklarujte veřejné metody v rozbočovači, aby klienti mohou volat je.
- Použití **Microsoft.AspNet.SignalR.Hub.Clients** dynamických vlastností pro přístup k všechny klienty připojené k této rozbočovače.
- Volání funkce na straně klienta (například `broadcastMessage` funkce) k aktualizaci klientů.

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR a jQuery

Stránky HTML v ukázce kódu ukazuje, jak používat knihovny jQuery SignalR ke komunikaci s rozbočovači SignalR. Proxy server tak, aby odkazovaly rozbočovače, deklarace funkci, která serveru můžete volat k předávaný obsah pro klienty a počáteční připojení k odeslání zprávy do centra jsou deklarace základních úloh v kódu.

Následující kód deklaruje odkaz na proxy server rozbočovače.

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> V jazyce JavaScript je odkaz na třídě serveru a její členy v camelCase. Odkazuje na ukázka kódu jazyka C# **ChatHub** třídy v jazyce JavaScript jako **chatHub**.


Následující kód je, jak vytvořit funkci zpětného volání ve skriptu. Třídy rozbočovače na serveru volá tuto funkci tak, aby nabízel aktualizace obsahu do jednotlivých klientů. Dva řádky, HTML kódování obsahu před jejich zobrazením jsou volitelné a zobrazit jednoduchý způsob, jak zabránit vložení skriptu.

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

Následující kód ukazuje, jak k otevření připojení do centra. Kód spustí připojení a předává je funkce, která se zpracovat událost kliknutím na **odeslat** tlačítka na stránce HTML.

> [!NOTE]
> Tento přístup zajišťuje, že připojení před provedením obslužné rutiny události.


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>Další kroky

Jste zjistili, že SignalR je architektura pro vytváření aplikací webu v reálném čase. Také jste zjistili několik úloh vývoj SignalR: jak přidat do aplikace ASP.NET SignalR, jak vytvořit třídy rozbočovače a jak odesílat a přijímat zprávy z rozbočovače.

Podrobný postup nasazení ukázkové aplikace SignalR do Azure, najdete v části [pomocí SignalR s webovými aplikacemi ve službě Azure App Service](../deployment/using-signalr-with-azure-web-sites.md). Podrobné informace o tom, jak nasadit webový projekt sady Visual Studio na webu systému Windows Azure najdete v tématu [vytvoření webové aplikace ASP.NET ve službě Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

Informace o pokročilejší SignalR vývoji koncepty, najdete na následujících stránkách pro SignalR zdrojového kódu a prostředky:

- [Projekt SignalR](http://signalr.net)
- [SignalR Githubu a ukázky](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
