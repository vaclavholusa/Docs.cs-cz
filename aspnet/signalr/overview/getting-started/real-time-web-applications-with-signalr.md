---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Rukou na testovacího prostředí: V reálném čase webových aplikací pomocí nástroje SignalR | Microsoft Docs'
author: rick-anderson
description: V reálném čase webových aplikací funkci možnost nabízet obsah připojeným klientům, jako dochází v reálném čase serverové. Pro vývojáře využívající technologii ASP.NET, ASP...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 5a2bc120ded18ad2302fd6c5cde65a5323e86ca8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/06/2018
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Rukou na testovacího prostředí: V reálném čase webových aplikací pomocí nástroje SignalR
====================
Podle [webové táborech Team](https://twitter.com/webcamps)

[Stažení webové táborech cvičení Kit](http://aka.ms/webcamps-training-kit)

> V reálném čase webových aplikací funkci možnost nabízet obsah připojeným klientům, jako dochází v reálném čase serverové. Pro vývojáře využívající technologii ASP.NET **funkce SignalR technologie ASP.NET** je knihovnu, kterou chcete přidat funkce webu v reálném čase do svých aplikací. Provádí se několik přenosy, automaticky výběr nejlépe k dispozici přenos daného klienta a serveru nejlépe k dispozici přenos. Provádí se **WebSocket**, rozhraní API jazyka HTML5, které umožňuje obousměrnou komunikaci mezi prohlížečem a serverem.
> 
> **SignalR** také poskytuje jednoduché, vysoké úrovně rozhraní API pro provádění serveru do klienta protokolu RPC (volají funkce JavaScript v prohlížečích vaši klienti z kódu .NET na straně serveru) v aplikaci ASP.NET, jakož i přidání užitečné háky pro správu připojení například připojení/odpojení události, seskupování připojení a autorizaci.
> 
> **SignalR** je abstrakci přes některé přenosů, které jsou požadovány pro práci v reálném čase mezi klientem a serverem. A **SignalR** připojení spustí HTTP a pak je převést **WebSocket** připojení, pokud je k dispozici. **Protokol WebSocket** je ideální přenos pro **SignalR**, protože umožňuje využívat s maximální efektivitou paměti serveru, má nejnižší latenci a má nejvíce základní funkce (jako je plně duplexní komunikace mezi klientem a Server), ale má také nejpřísnější požadavky: **WebSocket** vyžaduje, aby používat server **systému Windows Server 2012** nebo **Windows 8**, společně s **Rozhraní .NET framework 4.5**. Pokud tyto požadavky nejsou splněny, **SignalR** se pokusí použít jiné přenosy, abyste svá připojení (jako je *Ajax dlouhé dotazování*).
> 
> **SignalR** rozhraní API obsahuje dva modely pro komunikaci mezi klienty a servery: **trvalé připojení** a **Hubs**. A **připojení** představuje jednoduchý koncový bod pro odesílání jednoho příjemce, seskupené nebo zpráv všesměrového vysílání. A **rozbočovače** více vysoké úrovně kanál založena na rozhraní API připojení, která umožňuje klientovi i serveru volat metody na sobě navzájem přímo.
> 
> ![Architektura SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Všechny ukázky kódu a fragmenty kódu jsou součástí webové táborech školení sady, k dispozici na [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Přehled

<a id="Objectives"></a>
### <a name="objectives"></a>Cíle

V tomto testovacím prostředí praktických se dozvíte, jak:

- Odesílání oznámení ze serveru do klienta pomocí funkce SignalR.
- Škálování SignalR aplikace pomocí **systému SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Požadavky

Pro dokončení této praktické cvičení je vyžadován následující text:

- [Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/) nebo vyšší

<a id="Setup"></a>
### <a name="setup"></a>Instalace

Aby bylo možné spustit praktická cvičení v tomto testovacím prostředí praktické, musíte nejdřív nastavit svoje prostředí.

1. Otevřete okno Průzkumníka Windows a přejděte do tohoto prostředí **zdroj** složky.
2. Klikněte pravým tlačítkem na **Setup.cmd** a vyberte **spustit jako správce** ke spuštění procesu instalace, který bude konfiguraci prostředí a instalaci sady Visual Studio fragmenty kódu pro toto testovací prostředí.
3. Pokud se zobrazí dialogové okno Řízení uživatelských účtů, zkontrolujte akce, která bude pokračovat.

> [!NOTE]
> Zkontrolujte, zda že jste zkontrolovali všechny závislosti pro toto testovací prostředí před spuštěním instalačního programu.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Používání fragmentů kódu

V dokumentu testovacího prostředí budete vyzváni k vložení bloky kódu. Pro usnadnění vaší práce většina tento kód je k dispozici jako Visual Studio fragmenty kódu, kterým můžete přistupovat z v rámci Visual Studio 2013, abyste nemuseli přidat ručně.

> [!NOTE]
> Každý úkol je přiložena počáteční řešení umístěný v **začít** složky cvičení, která umožňuje postupujte podle jednotlivých cvičení nezávisle na ostatních. Upozorňujeme, že fragmenty kódu, které jsou přidány během cvičení chybí z nich spuštění řešení a nemusí fungovat, dokud nedokončíte výkonu. Uvnitř zdrojový kód pro cvičení, je také k dispozici **End** složku obsahující řešení sady Visual Studio s kódem, který je výsledkem kroků v odpovídající cvičení. Tato řešení můžete použít jako vodítko, pokud potřebujete další pomoc při práci prostřednictvím této praktické cvičení.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Cvičení

Toto praktické cvičení zahrnuje následující cvičení:

1. [Práce s Data v reálném čase pomocí funkce SignalR](#Exercise1)
2. [Škálování pomocí SQL serveru](#Exercise2)

Odhadovaný čas dokončení tohoto testovacího prostředí: **60 minut**

> [!NOTE]
> Při prvním spuštění sady Visual Studio, musíte vybrat jednu z předdefinovaných nastavení kolekce. Každé předdefinované kolekce je navržené tak, aby odpovídala konkrétním vývoj styl a určuje rozložení oken, editor chování, IntelliSense – fragmenty kódu a možnosti dialogového okna. Postupy v tomto testovacím prostředí popisovat akce, které jsou nutné k dokončení daného úkolu v sadě Visual Studio, při použití **obecné nastavení vývoj** kolekce. Pokud si zvolíte jiné nastavení kolekce pro vývojové prostředí, může být rozdíly v krocích, které byste měli vzít v úvahu.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Cvičení 1: Práce s Data v reálném čase pomocí funkce SignalR

Při chat se často používá jako příklad, můžete provést celek mnoho další s v reálném čase funkce webu. Vždy, když uživatel aktualizuje na webové stránce zobrazíte nová data nebo implementuje stránky Ajax dlouhé dotazování načte nová data, můžete použít funkci SignalR.

Podporuje SignalR **serveru nabízených** nebo **všesměrové vysílání** funkce; zpracovává správu připojení automaticky. V classic připojení protokolu HTTP pro komunikaci klienta se serverem bude připojení znovu navázáno pro každý požadavek, ale funkce SignalR poskytuje trvalé připojení mezi klientem a serverem. V systému SignalR, který volá kód serveru kód klienta v prohlížeči pomocí vzdáleného volání procedur (RPC) místo modelu požadavků a odpovědí víme ještě dnes.

V tomto cvičení budete konfigurovat **Geek kvízu** aplikace pro používání SignalR zobrazení řídicího panelu statistiky o aktualizované metriky, aniž by bylo nutné aktualizovat celou stránku.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Úloha 1 – prohlížení stránky Geek kvízu s časovým limitem statistiky

V této úloze budete absolvovat aplikace a ověřte, jak je zobrazen stránce Statistika a jak můžete vylepšit způsobem jsou informace se aktualizuje.

1. Otevřete **Visual Studio Express 2013 pro Web** a otevřete **GeekQuiz.sln** řešení umístěný v **Source\Ex1 WorkingWithRealTimeData\Begin** složky.
2. Stiskněte klávesu **F5** ke spuštění řešení. **Přihlásit** stránka by se měla objevit v prohlížeči.

    ![Spuštění řešení](real-time-web-applications-with-signalr/_static/image2.png "systémem řešení")

    *Spuštění řešení*
3. Klikněte na tlačítko **zaregistrovat** v pravém horním rohu stránky vytvořte nového uživatele v aplikaci.

    ![Zaregistrovat odkaz](real-time-web-applications-with-signalr/_static/image3.png "odkaz registrace")

    *Zaregistrovat odkaz*
4. V **zaregistrovat** stránky, zadejte **uživatelské jméno** a **heslo**a potom klikněte na **zaregistrovat**.

    ![Registrace uživatele](real-time-web-applications-with-signalr/_static/image4.png "registrace uživatele")

    *Registrace uživatele*
5. Aplikace zaregistruje nový účet a uživatel je ověřen a přesměrován zpět na domovskou stránku zobrazující první otázku kvízu s časovým limitem.
6. Otevřete **statistiky** stránky v novém okně a umístí **Domů** stránky a **statistiky** stránky vedle sebe.

    ![Souběžně sdílená windows](real-time-web-applications-with-signalr/_static/image5.png "souběžně straně windows")

    *Windows vedle sebe*
7. V **Domů** stránky, odpovězte na otázku kliknutím na jednu z možností.

    ![Odpovědi na dotaz](real-time-web-applications-with-signalr/_static/image6.png "odpovědi na dotaz")

    *Odpovědi na dotaz*
8. Po kliknutí na tlačítko požadovaného, by se zobrazit odpověď.

    ![Otázka zodpovězena správné](real-time-web-applications-with-signalr/_static/image7.png "otázku odpověděli správný")

    *Otázka správně zodpovězena*
9. Všimněte si, že informací uvedených na stránce Statistika je zastaralé. Chcete-li zobrazit aktualizované výsledky aktualizujte stránku.

    ![Stránka statistiky](real-time-web-applications-with-signalr/_static/image8.png "statistiky stránky")

    *Stránka statistiky*
10. Přejděte zpět na Visual Studio a zastavte ladění.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Úloha 2 – přidání SignalR na Geek kvízu s časovým limitem zobrazíte Online grafy

V této úloze bude do řešení přidat SignalR a odesílání aktualizací na klienty automaticky při novou odpověď pro odeslání na server.

1. Z **nástroje** nabídky v sadě Visual Studio, vyberte **Správce balíčků knihoven**a potom klikněte na **Konzola správce balíčků**.
2. V **Konzola správce balíčků** okno, spusťte následující příkaz:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalace balíčku SignalR](real-time-web-applications-with-signalr/_static/image9.png "instalace balíčku SignalR")

    *Instalace balíčku SignalR*

   > [!NOTE]
   > Při instalaci **SignalR** NuGet balíčky verze bodu 2.0.2 z úplně nová aplikace MVC 5, budete muset ručně aktualizovat **OWIN** balíčky verzi 2.0.1 (nebo vyšší) před instalací SignalR. K tomuto účelu můžete spustit následující skript v **Konzola správce balíčků**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > V budoucích vydání systému SignalR bude OWIN závislosti automaticky aktualizován.
3. V **Průzkumníku řešení**, rozbalte **skripty** složky a Všimněte si, které funkce SignalR *js* soubory byly přidány k řešení.

    ![Odkazuje na SignalR JavaScript](real-time-web-applications-with-signalr/_static/image10.png "odkazuje SignalR JavaScript")

    *Odkazy na SignalR JavaScript*
4. V **Průzkumníku řešení**, klikněte pravým tlačítkem myši **GeekQuiz** projekt, vyberte **přidat** | **novou složku**a pojmenujte ji  **Centra**.
5. Klikněte pravým tlačítkem myši **Hubs** složky a vyberte **přidat | Nová položka**.

    ![Přidat novou položku](real-time-web-applications-with-signalr/_static/image11.png "přidat novou položku")

    *Přidat novou položku*
6. V **přidat novou položku** dialogové okno, vyberte **Visual C# | Web | SignalR** uzlu v levém podokně, vyberte **třídy rozbočovače SignalR (v2)** z podokna center název souboru **StatisticsHub.cs** a klikněte na tlačítko **přidat**.

    ![Přidat položku dialogové okno Nový](real-time-web-applications-with-signalr/_static/image12.png "přidat novou položku dialogové okno")

    *Přidat novou položku – dialogové okno*
7. Nahraďte kód v **StatisticsHub** třídy následujícím kódem.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - StatisticsHubClass*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Otevřete **Startup.cs** a přidejte následující řádek na konci **konfigurace** metoda.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - MapSignalR*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Otevřete **StatisticsService.cs** stránky uvnitř **služby** složky a přidejte následující direktivy using.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - UsingDirectives*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Oznámit připojené klienty aktualizací, je třeba nejprve načíst **kontextu** objekt pro aktuální připojení. **Rozbočovače** objektu obsahuje metody pro odesílání zpráv do jednoho klienta nebo všesměrového vysílání pro všechny připojené klienty. Přidejte následující metodu do **StatisticsService** třída k vysílání dat statistiky.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - NotifyUpdatesMethod*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > Ve výše uvedeném kódu, používáte název libovolný metoda k volání funkce na straně klienta (tj.: *updateStatistics*). Název metody, který zadáte interpretována jako dynamický objekt, což znamená, že není žádná IntelliSense nebo kompilace přijme se. Vyhodnotí výraz v době běhu. Při volání metody, které spouští, odešle SignalR název metody a hodnoty parametru klientovi. Pokud má klient metodu, která odpovídá názvu, že metoda je volána a hodnoty parametrů jsou předat do ní. Pokud žádná odpovídající metoda nenajde na straně klienta, je vyvolána žádná chyba. Další informace najdete v části [ASP.NET SignalR centra API průvodce](../guide-to-the-api/hubs-api-guide-server.md).
11. Otevřete **TriviaController.cs** stránky uvnitř **řadiče** složky a přidejte následující direktivy using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Přidejte následující zvýrazněný kód, který **Post** metody akce.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - NotifyUpdatesCall*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Otevřete **Statistics.cshtml** stránky uvnitř **zobrazení | Domů** složky. Vyhledejte **skripty** a přidejte následující reference skriptu na začátku části.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - SignalRScriptReferences*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Když přidáte do projektu sady Visual Studio SignalR a další skript knihovny, může správce balíčků nainstalujte verzi souboru skriptu SignalR, která je novější než verze uvedené v tomto tématu. Ujistěte se, že odkaz na skript v kódu odpovídá verzi knihovny skriptu nainstalovaná ve vašem projektu.
14. Přidejte následující zvýrazněný kód k připojení klienta k rozbočovači SignalR a aktualizaci statistiky dat při přijetí nové zprávy z rozbočovače.

    (Code fragment kódu - *RealTimeSignalR - Ex1 - SignalRClientCode*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    V tomto kódu jsou vytváření proxy server rozbočovače a registraci obslužné rutiny události pro naslouchání zpráv odeslaných serverem. V takovém případě můžete přijímat zprávy odeslané přes *updateStatistics* metoda.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Úloha 3 – spuštění řešení

V této úloze budete spouštět řešení a ověřte, že zobrazení statistiky aktualizován automaticky pomocí SignalR po odpovědi na novou otázku.

1. Stiskněte klávesu **F5** ke spuštění řešení.

    > [!NOTE]
    > Pokud už nejste přihlášení k aplikaci, přihlaste se pomocí uživatele, kterého jste vytvořili v úloze 1.
2. Otevřete **statistiky** stránky v novém okně a umístí **Domů** stránky a **statistiky** stránky vedle sebe, jako jste to udělali v úloze 1.
3. V **Domů** stránky, odpovězte na otázku kliknutím na jednu z možností.

    ![Odpovědi na jinou otázku](real-time-web-applications-with-signalr/_static/image13.png "odpovědi na jinou otázku")

    *Odpovědi na jinou otázku*
4. Po kliknutí na tlačítko požadovaného, by se zobrazit odpověď. Všimněte si, že statistické informace na stránce se aktualizuje automaticky po odpovědi na otázky se aktualizované informace, aniž by bylo nutné aktualizovat celou stránku.

    ![Stránka statistiky aktualizují po provedení odpovědí](real-time-web-applications-with-signalr/_static/image14.png "stránka statistiky aktualizují po provedení odpovědí")

    *Stránka statistiky aktualizovat po odpovědí*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Cvičení 2: Škálování pomocí SQL serveru

Při příjmu webové aplikace, je obecně možné mezi *vertikálním navýšení kapacity* a *horizontální navýšení kapacity* možnosti. *Vertikální navýšení kapacity* znamená větší server pomocí více prostředků (procesoru, paměti RAM atd.) při *škálovat* znamená přidávat další servery pro zpracování zátěže. Problém s k tomu je, že klienti mohou získat směrované na různých serverech. Klient, který je připojený k jednomu serveru nebude příjem zpráv odeslaných z jiného serveru.

Tyto problémy můžete vyřešit pomocí komponenty s názvem *propojovacího rozhraní*, k předávání zpráv mezi servery. S propojovacího rozhraní povoleno každá instance aplikace odesílá zprávy do propojovacího rozhraní a předává je propojovacího rozhraní dalších instancí aplikace.

Aktuálně existují tři typy backplanes pro SignalR:

- **Windows Azure Service Bus**. Service Bus je zasílání zpráv infrastrukturu, která umožňuje součásti odesílat volně párované zprávy.
- **SQL Server**. Propojovací rozhraní systému SQL Server zapíše zprávy do tabulky SQL. Propojovacího rozhraní používá službu Service Broker pro efektivní zasílání zpráv. Ale spolupracuje také pokud Service Broker není povolena.
- **Redis**. Redis je úložišti klíč hodnota v paměti. Redis podporuje vzor publikovat/odebírat ("pub nebo sub") pro odesílání zpráv.

Každá zpráva se budou odesílat prostřednictvím sběrnice zpráv. Implementuje sběrnice zpráv [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) rozhraní, která poskytuje abstrakci se publikování a přihlášení k odběru. Backplanes fungovat tak, že nahradíte výchozí **IMessageBus** se sběrnicí určené pro tento propojovacího rozhraní.

Každá instance serveru se připojí k propojovacího rozhraní přes sběrnici. Pokud je odeslána zpráva, přejdete do propojovacího rozhraní a propojovacího rozhraní odešle ji do každého serveru. Když server přijme zprávu o z propojovacího rozhraní, ukládá zprávy v místní mezipaměti. Server pak přináší zprávy pro klienty z místní mezipaměti.

Další informace o propojovacího rozhraní SignalR fungování, přečtěte si to [článku](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Je několik scénářů, kde propojovací rozhraní se může stát úzkým místem. Zde jsou některé typické scénáře SignalR:
> 
> - [Všesměrové vysílání server](tutorial-server-broadcast-with-signalr.md) (například běžícími): Backplanes fungovat i pro tento scénář, protože serveru ovládá rychlost, jakou jsou odesílány zprávy.
> - [Klient klient](tutorial-getting-started-with-signalr.md) (například chat): V tomto scénáři můžou propojovacího rozhraní problémové místo, pokud počet zpráv škáluje s počtem klientů; to znamená, pokud počet zpráv zvětšování úměrně Další klienti se připojují.
> - [Vysoká frekvence v reálném čase](tutorial-high-frequency-realtime-with-signalr.md) (např. v reálném čase hry): propojovací rozhraní se nedoporučuje pro tento scénář.


V tomto cvičení použijete **systému SQL Server** k distribuci zpráv mezi **Geek kvízu** aplikace. Tyto úlohy poběží na jeden testovací počítač se dozvíte, jak nastavit konfiguraci, ale pokud chcete získat úplné účinek, budete muset nasadit aplikaci SignalR na dva nebo více serverů. Musíte také nainstalovat SQL Server na jednom ze serverů nebo na samostatný vyhrazený server.

![Horizontální navýšení kapacity pomocí Diagram serveru SQL](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Úloha 1 – Principy scénář

V této úloze budete spouštět 2 instance **Geek kvízu** simulaci IIS více instancí v místním počítači. V tomto scénáři při odpovědi na otázky trivia na jednu aplikaci nebudete nijak upozorněni aktualizace na stránce Statistika druhé instance. Tato simulace vypadá takto: prostředí, kde vaše aplikace je nasazená na několik instancí a použití služby Vyrovnávání zatížení komunikovat s nimi.

1. Otevřete **Begin.sln** řešení umístěný v **zdroj/Ex2-ScalingOutWithSQLServer/Begin** složky. Jakmile načteny, si všimnete na **Průzkumníka serveru** , řešení obsahuje dva projekty s identické struktury ale různými názvy. To bude simulovat spuštěné dvě instance stejné aplikace na místním počítači.

    ![Začněte řešení simulaci 2 instance Geek kvízu s časovým limitem](real-time-web-applications-with-signalr/_static/image16.png "začít řešení simulaci 2 instance Geek kvízu s časovým limitem")

    *Začněte řešení simulaci 2 instance Geek kvízu s časovým limitem*
2. Otevře se stránka vlastností řešení pravým tlačítkem myši na uzel řešení a výběrem **vlastnosti**. V části **spouštěný projekt**, vyberte **více projektů po spuštění** a změňte **akce** hodnotu pro oba projekty tak, aby *spustit*.

    ![Spouštění více projektů](real-time-web-applications-with-signalr/_static/image17.png "spouštění více projektů")

    *Spouštění více projektů*
3. Stiskněte klávesu **F5** ke spuštění řešení. Aplikace spustí dvě instance **Geek kvízu** v jiné porty, simulaci více instancí stejné aplikace. Jeden z prohlížečů PIN kódu na levé straně a druhý na pravé straně obrazovky. Přihlaste se pomocí přihlašovacích údajů nebo registraci nového uživatele. Po přihlášení, zachovat stránce Trivia na levé straně a Přejít **statistiky** stránku v prohlížeči na pravé straně.

    ![Geek kvízu vedle sebe](real-time-web-applications-with-signalr/_static/image18.png)

    *Geek kvízu vedle sebe*

    ![Geek kvízu s časovým limitem v jiné porty](real-time-web-applications-with-signalr/_static/image19.png)

    *Geek kvízu s časovým limitem v jiné porty*
4. Spusťte zodpovězení otázek v levém prohlížeči a bude zjistíte, že **statistiky** stránku v prohlížeči správné není aktualizován. Důvodem je, že **SignalR** používá místní mezipaměti distribuci zprávy přes svým klientům a tento scénář je simulaci víc instancí, proto není mezi nimi sdílené mezipaměti. Ověřte, že **SignalR** pracuje podle testování stejný postup, ale pomocí jediné aplikaci. V následujících úloh můžete nakonfigurovat propojovacího rozhraní k replikaci zprávy napříč instancemi.
5. Přejděte zpět na Visual Studio a zastavte ladění.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Úloha 2 – Vytvoření propojovacího rozhraní SQL Server

V této úloze se vytvoří databázi, která bude sloužit jako propojovací rozhraní systému pro **Geek kvízu** aplikace. Budete používat **Průzkumník objektů systému SQL Server** procházet váš server a inicializace databáze. Kromě toho vám umožní **služby Service Broker**.

1. V **Visual Studio**, otevřete nabídku **zobrazení** a vyberte **Průzkumník objektů systému SQL Server**.
2. Připojení k vaší instanci LocalDB kliknutím pravým tlačítkem myši **systému SQL Server** uzlu a výběrem **přidat SQL Server...**  možnost.

    ![Přidání Instance systému SQL Server](real-time-web-applications-with-signalr/_static/image20.png "přidání Instance systému SQL Server")

    *Přidání instance systému SQL Server do Průzkumník objektů systému SQL Server*
3. Nastavte **název serveru** k *(localdb) \v11.0* a nechte **ověřování systému Windows** jako vaše režim ověřování. Klikněte na tlačítko **Connect** pokračujte.

    ![Připojení k LocalDB](real-time-web-applications-with-signalr/_static/image21.png "připojení na instanci LocalDB")

    *Připojení k instanci LocalDB*
4. Teď, když se připojíte k vaší instanci LocalDB, musíte vytvořit databázi, která bude představovat propojovací rozhraní systému SQL Server pro služby SignalR. Chcete-li to provést, klikněte pravým tlačítkem **databáze** uzel a vyberte možnost **přidat novou databázi**.

    ![Přidávání nové databáze](real-time-web-applications-with-signalr/_static/image22.png "přidávání nové databáze")

    *Přidávání nové databáze*
5. Nastavte název databáze na *SignalR* a klikněte na tlačítko **OK** k jeho vytvoření.

    ![Vytvoření databáze SignalR](real-time-web-applications-with-signalr/_static/image23.png "vytvoření databáze SignalR")

    *Vytvoření databáze SignalR*

    > [!NOTE]
    > Můžete použít libovolný název databáze.
6. Pokud chcete získat aktualizace efektivněji z propojovacího rozhraní, se doporučuje povolte službu Service Broker pro databázi. Service Broker poskytuje nativní podporu pro zasílání zpráv a služby Řízení front v systému SQL Server. Propojovacího rozhraní funguje taky bez služby Service Broker. Otevřete nový dotaz tak, že kliknete pravým tlačítkem na databázi a vyberte **nový dotaz**.

    ![Otevřete nový dotaz](real-time-web-applications-with-signalr/_static/image24.png "otevření nového dotazu")

    *Otevření nového dotazu*
7. Chcete-li zkontrolovat, zda je povoleno služby Service Broker, dotaz **je\_zprostředkovatele\_povoleno** sloupec v **zobrazení sys.databases** katalogu zobrazení. Spusťte následující skript v okně naposledy otevřeného dotazu.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Dotazování na stav služby Broker](real-time-web-applications-with-signalr/_static/image25.png "dotazování na stav zprostředkovatele služby.")

    *Dotazování na stav zprostředkovatele služby.*
8. Pokud hodnota **je\_zprostředkovatele\_povoleno** sloupec v databázi je &quot;0&quot;, použijte následující příkaz k jeho povolení. Nahraďte **&lt;YOUR databáze&gt;** s názvem jste nastavili při vytváření databáze (např: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Povolení služby Service Broker](real-time-web-applications-with-signalr/_static/image26.png "povolení služby Service Broker")

    *Povolení služby Service Broker*

    > [!NOTE]
    > Pokud tento dotaz zobrazí zablokování, ujistěte se, nejsou žádné aplikace, připojení k databázi.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Úloha 3 – konfigurace aplikace SignalR

V této úloze nakonfigurujete **Geek kvízu** pro připojení k systému SQL Server propojovacího rozhraní. Nejprve přidejte **SignalR.SqlServer** balíček NuGet a sady připojení řetězec k vaší databázi propojovacího rozhraní.

1. Otevřete **Konzola správce balíčků** z **nástroje** | **Správce balíčků knihoven**. Ujistěte se, že **GeekQuiz** je vybrán projekt v **výchozí projekt** rozevíracího seznamu. Zadejte následující příkaz k instalaci **Microsoft.AspNet.SignalR.SqlServer** balíček NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Opakujte předchozí krok, ale tentokrát pro projekt **GeekQuiz2**.
3. Ke konfiguraci propojovací rozhraní systému SQL Server, otevřete **Startup.cs** soubor **GeekQuiz** projekt a přidejte následující kód do **konfigurace** metoda. Nahraďte **&lt;YOUR databáze&gt;** názvem vaší databáze, můžete použít při vytváření propojovací rozhraní systému SQL Server. Opakujte tento krok pro **GeekQuiz2** projektu.

    (Code fragment kódu - *RealTimeSignalR - Ex2 - StartupConfiguration*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Teď, když oba projekty jsou nakonfigurovány pro použití propojovací rozhraní systému SQL Server, stiskněte klávesu **F5** je současně spustit.
5. Znovu **Visual Studio** spustí dvě instance **Geek kvízu** v jiné porty. Jeden z prohlížečů připnout na levé straně a druhý na pravé straně obrazovky a přihlaste se pomocí přihlašovacích údajů. Zachovat stránce Trivia na levé straně, přejděte na **statistiky** pagein správné prohlížeče.
6. Spusťte odpovědi na otázky v levém prohlížeči. Tentokrát **statistiky** aktualizaci stránky díky propojovacího rozhraní. Přepínání mezi aplikací (**statistiky** je teď na levé straně a **Trivia** je na pravé straně) a opakujte test pro ověření, zda je funkční pro oba instance. Propojovacího rozhraní slouží jako *sdílené mezipaměti* zpráv pro každý připojený server a každý server budou ukládat zprávy do vlastní místní mezipaměti pro distribuci do připojených klientů.
7. Přejděte zpět na Visual Studio a zastavte ladění.
8. Součást propojovací rozhraní systému SQL Server se automaticky vytvoří nezbytné tabulky se zadanou databází. V **Průzkumník objektů systému SQL Server** panelu, otevřete databázi, který jste vytvořili pro propojovacího rozhraní (např: SignalR) a rozbalte její tabulky. Měli byste vidět v následujících tabulkách:

    ![Propojovací rozhraní systému vygenerované tabulky](real-time-web-applications-with-signalr/_static/image27.png)

    *Propojovací rozhraní systému vygenerované tabulky*
9. Klikněte pravým tlačítkem myši **SignalR.Messages\_0** tabulky a vyberte **Data zobrazení**.

    ![Zobrazení tabulky zpráv propojovací rozhraní systému SignalR](real-time-web-applications-with-signalr/_static/image28.png)

    *Zobrazení tabulky zpráv propojovací rozhraní systému SignalR*
10. Zobrazí se různé zprávy odeslané do **rozbočovače** při odpovědi na otázky trivia. Propojovacího rozhraní distribuuje tyto zprávy na jakoukoli instanci připojené.

    ![Tabulka zpráv propojovací rozhraní systému](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabulka zpráv propojovací rozhraní systému*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Souhrn

V tomto testovacím prostředí praktických jste se naučili postup přidání **SignalR** do aplikace a odesílání oznámení ze serveru vaší připojeným klientům pomocí **Hubs**. Kromě toho jste zjistili, jak chcete škálovat aplikaci pomocí *propojovacího rozhraní* součást při nasazení vaší aplikace na několik instancí služby IIS.
