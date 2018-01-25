---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: "Rozhraní ASP.NET Web Forms připojení odolnost proti chybám a příkaz zachycením | Microsoft Docs"
author: Erikre
description: "Tento kurz popisuje, jak změnit ukázková aplikace pro podporu odolnost připojení a zachycením příkaz."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: e3347657fb5c7bf8c7bb4e51a2e810a1edde826a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Rozhraní ASP.NET Web Forms připojení odolnost proti chybám a příkaz zachycením
====================
Podle [Erik Reitan](https://github.com/Erikre)

V tomto kurzu budete upravovat adresář Wingtip Toys ukázkovou aplikaci pro podporu odolnost připojení a zachycením příkaz. Povolením odolnost připojení bude ukázkovou aplikaci adresář Wingtip Toys automaticky opakovat volání dat, když dojde k přechodné chyby, které jsou typické pro cloudové prostředí. Navíc implementací příkaz zachycení ukázkovou aplikaci adresář Wingtip Toys zachytí všechny dotazy SQL odeslal do databáze za účelem protokolu nebo je změnit.

> [!NOTE] 
> 
> Tento kurz webové formuláře se podle následující kurz MVC tní Dykstra:  
> [Odolnost připojení a zachycením příkaz s použitím Entity Framework v aplikaci ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Získáte informace:

- Jak zajistit odolnost připojení.
- Popisuje, jak implementovat příkaz zachycení.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte v počítači nainstalován následující software:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [sady Microsoft Visual Studio Express 2013 pro Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky.
- Adresář Wingtip Toys ukázkový projekt, proto, že můžete implementovat funkci uvedených v tomto kurzu v rámci projektu adresář Wingtip Toys. Podrobnosti stažení naleznete na následující odkaz:

    - [Začínáme s ASP.NET 4.5.1 webové formuláře – adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Před dokončení tohoto kurzu, zvažte kontrola související kurz řady [Začínáme s webovými formuláři ASP.NET 4.5 a Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Kurz řady pomůže vám seznámit se s **Northwind** projektu a kód.

## <a name="connection-resiliency"></a>Odolnost připojení

Pokud uvažujete o nasazení aplikace do systému Windows Azure, je jednou z možností vzít v úvahu nasazení databáze, která se **Windows** **Azure SQL Database**, Cloudová služba databáze. Připojení přechodné chyby jsou obvykle častější při připojení k databázi Cloudová služba než pokud váš webový server a serverem databáze jsou přímo připojené společně ve stejném datovém centru. I v případě cloudu webového serveru a databáze cloudové služby jsou hostované ve stejném datovém centru, existují další síťové připojení mezi nimi, které může mít problémy, například nástroje pro vyrovnávání zatížení.

Také Cloudová služba je obvykle sdílet jinými uživateli, což znamená, že jeho reakce mohou být ovlivněny je. A váš přístup k databázi může být předmětem omezení. Omezení znamená služba databáze vyvolá výjimek při pokusu o přístup k častěji, než je povolený v vaše *smlouvy o úrovni služeb* (SLA).

Mnoho nebo většinu potíže s připojením, ke kterým dochází při přístupech cloudové služby jsou přechodné, to znamená, že vyřešit samy v krátké době. Proto při operaci databáze a načíst typ chyby, který je obvykle přechodný, vám může opakujte akci po krátké počkejte a operace může být úspěšné. Pokud zpracovat přechodné chyby tak, že automaticky zkusíte znovu, můžete zadat mnohem lepší podmínky pro vaše uživatele viditelnosti většina z nich zákazníka. Funkce odolnost připojení na Entity Framework 6 automatizuje, že proces opakování se nezdařilo dotazy SQL.

Funkci odolnost připojení musí bylo správně nakonfigurované pro konkrétní databázi služby:

1. Je třeba vědět, výjimek, které by mohly být přechodné. Chcete opakovat chyby způsobené k dočasné ztrátě v připojení k síti, nejsou chyby způsobené chybami programu, například.
2. Má čekat odpovídající množství času mezi opakování operace se nezdařila. Můžete počkat již mezi opakovanými pokusy pro zpracování balíku než pro online webovou stránku, kde je uživatel čekání na odpověď.
3. Má odpovídající počet doby, než se bude opakovat. Můžete chtít opakování vícekrát v batch proces, který by v aplikaci online.

Můžete nakonfigurovat tato nastavení pro všechny databáze prostředí podporovaná službou zprostředkovatele Entity Framework ručně.

Všechny stačí povolit odolnost připojení je vytvořte třídu v vaše sestavení, která je odvozena z `DbConfiguration` a v této třídě nastavit strategii provádění databáze SQL, který je v rozhraní Entity Framework jiný termín pro zásady opakování.

### <a name="implementing-connection-resiliency"></a>Implementace odolnost připojení

1. Stáhněte si a nainstalujte [Northwind](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) ukázkové aplikace webových formulářů v sadě Visual Studio.
2. V *logiku* složky **Northwind** aplikací, přidejte soubor třídy s názvem *WingtipToysConfiguration.cs*.
3. Existujícího kódu nahraďte následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Rozhraní Entity Framework automaticky spustí kód najde v třídu odvozenou z `DbConfiguration`. Můžete použít `DbConfiguration` třídy konfigurační úkony v kódu, který by jinak uděláte *Web.config* souboru. Další informace najdete v tématu [EntityFramework konfigurace založené na kódu](https://msdn.microsoft.com/data/jj680699).

1. V *logiku* složku, otevřete *AddProducts.cs* souboru.
2. Přidat `using` příkaz pro `System.Data.Entity.Infrastructure` znázorněné zvýrazněných v žlutý:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Přidat `catch` zablokujte `AddProduct` metoda tak, aby `RetryLimitExceededException` se zaznamená jako zvýrazněných v žlutý:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Přidáním `RetryLimitExceededException` výjimky, můžete poskytovat lepší protokolování nebo zobrazit chybovou zprávu pro uživatele, kde můžete vybrat a zkuste to znovu proces. Podle zachytávání `RetryLimitExceededException` výjimky, které by mohly být přechodné pouze chyby bude již jste se pokusili a několikrát se nezdařilo. Skutečné vrátil výjimku bude uzavřen do `RetryLimitExceededException` výjimka. Kromě toho jste také přidali bloku catch Obecné. Další informace o `RetryLimitExceededException` výjimky, najdete v části [odolnost připojení Entity Framework nebo opakujte logiku](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Příkaz zachycení

Teď, když jste zapnuli zásady opakovaných pokusů, jak můžete otestovat ověření, že funguje podle očekávání? Není tak snadno vynutit přechodná chyba provést, hlavně, když spouštíte místně, a je obzvláště složité je skutečný přechodné chyby integrovat do test automatizované jednotky. Chcete-li otestovat funkci odolnost připojení, potřebujete způsob, jak zachytávat dotazy, které Entity Framework odešle na SQL Server a nahraďte typ výjimky, který je obvykle přechodná odpověď serveru SQL.

Můžete také použít dotaz zachycení kvůli implementaci osvědčený postup pro cloudové aplikace: protokolu latenci a úspěch nebo selhání všech volání na externí službami, jako je databáze služby.

V této části kurzu budete používat rozhraní Entity Framework [ *funkci zachycení* ](https://msdn.microsoft.com/data/dn469464) pro protokolování i pro simulaci přechodné chyby.

### <a name="create-a-logging-interface-and-class"></a>Vytvoření rozhraní protokolování a – třída

Osvědčeným postupem pro protokolování je to udělat pomocí [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) místo pevně kódováno volání `System.Diagnostics.Trace` nebo třída protokolování. Který usnadňuje mechanismu protokolování později změnit, pokud byste někdy potřebovali udělat. Proto v této části vytvoříte rozhraní protokolování a třída pro implementaci.

Podle výše uvedeného postupu, jste stáhli a otevřeli **Northwind** ukázkovou aplikaci v sadě Visual Studio.

1. Vytvořte složku v **Northwind** projektu a pojmenujte ji *protokolování*.
2. V *protokolování* složky, vytvořte soubor třídy s názvem *ILogger.cs* a ve výchozím kódu nahraďte následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

 Rozhraní obsahuje tři úrovně trasování a určete relativní důležitost protokolů a z nich slouží k zadání informací o latence pro volání externí služby, jako je například databázové dotazy. Metody protokolování, mají přetížení, které umožňují předat výjimku. Toto je tak, aby informace o výjimce, včetně protokolů trasování a vnitřní výjimky je spolehlivě podle třídy, který implementuje rozhraní, aniž byste museli spoléhat na které se provádí v každém volání metody protokolování v celé aplikaci.  
  
 `TraceApi` Metody vám umožňují sledovat latence každé volání externí služba například databáze SQL.
3. V *protokolování* složky, vytvořte soubor třídy s názvem *Logger.cs* a ve výchozím kódu nahraďte následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Implementace používá `System.Diagnostics` udělat trasování. Toto je integrované funkce rozhraní .NET, což usnadňuje generování a použijte informace trasování. Existuje mnoho &quot;naslouchací procesy&quot; můžete používat s `System.Diagnostics` trasování, zápis do souborů, například protokoly a jejich zápis do úložiště objektů blob ve službě Windows Azure. Některé z možností a odkazy na další zdroje informací pro další informace najdete v tématu v [řešení potíží s weby systému Windows Azure v sadě Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). V tomto kurzu budete pouze najít v protokolech v sadě Visual Studio **výstup** okno.

V případě produkční aplikace můžete chtít zvažte použití trasování rozhraní než `System.Diagnostics`a `ILogger` rozhraní umožňuje přepnout do různých trasování mechanismus, pokud se rozhodnete k tomu je poměrně snadné.

### <a name="create-interceptor-classes"></a>Vytvořit zachytávací modul třídy

V dalším kroku vytvoříte třídy, které rozhraní Entity Framework zavolá do pokaždé, když se má odeslat dotaz na databázi, jeden pro simulaci přechodné chyby a jeden udělat protokolování. Tyto třídy interceptoru musí být odvozeny od `DbCommandInterceptor` třídy. V nich zapisovat metoda přepsání, které jsou automaticky volána, když dotaz se chystá provést. V těchto metod můžete prozkoumat nebo protokolu dotaz odeslaný do databáze, a můžete změnit dotaz před odesláním do databáze nebo vrátit něco Entity Framework sami bez i předávání dotaz do databáze.

1. Pro vytvoření třídy interceptoru, který bude protokolovat každý dotaz SQL, před odesláním do databáze, vytvořte soubor třídy s názvem *InterceptorLogging.cs* v *logiku* složky a nahradit výchozí kód s Následující kód:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

 Pro úspěšné dotazy nebo příkazy tento kód zapíše protokol informací s informacemi o latence. Pro výjimky vytvoří se protokol chyb.
2. Pro vytvoření třídy interceptoru, který vygeneruje fiktivní přechodné chyby, když zadáte &quot;Throw&quot; v **název** textové pole na stránce s názvem *AdminPage.aspx*, vytvořte třídu soubor s názvem *InterceptorTransientErrors.cs* v *logiku* složky a nahradit výchozí kód následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Tento kód pouze přepsání `ReaderExecuting` metodu, která je volána pro dotazy, které může vrátit více řádků dat. Pokud jste chtěli zkontrolujte odolnost připojení pro jiné typy dotazů, může také přepsat `NonQueryExecuting` a `ScalarExecuting` metody, jako interceptoru protokolování nemá.  
  
 Později, se přihlaste jako "Admin" a vyberte **správce** spojení v horním navigačním panelu. Potom na *AdminPage.aspx* stránky přidáte produkt s názvem &quot;Throw&quot;. Kód vytvoří fiktivní výjimka databáze SQL pro chyby číslo 20, typu ví, nejčastěji přechodné. Další chyba čísla aktuálně rozpoznán jako přechodný jsou 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 a 40613, ale to se může měnit v nové verze databáze SQL. Produkt bude přejmenován na "TransientErrorExample", které můžete provést v kódu *InterceptorTransientErrors.cs* souboru.  
  
 Kód vrátí Entity Framework namísto spuštění dotazu a předání back výsledky výjimku. Je vrácen přechodný výjimka *čtyři* dobu, a potom se vrátí kód do běžný postup předat dotaz do databáze.

    Vzhledem k tomu, že je všechno, co přihlášení, budete moci zobrazit Entity Framework se pokusí provést dotaz čtyřikrát před nakonec úspěšné, a v aplikaci jediným rozdílem je, že trvá déle k vykreslení stránky s výsledky dotazu.  
  
 Počet, který se bude opakovat rozhraní Entity Framework se dá konfigurovat; kód určuje čtyřikrát, protože se jedná o výchozí hodnotu pro zásady spouštění databáze SQL. Pokud změníte zásady spouštění, musíte také změnit zde kód, který určuje, kolikrát se generují přechodné chyby. Můžete také změnit kód ke generování další výjimky tak, aby rozhraní Entity Framework vyvolá výjimku `RetryLimitExceededException` výjimka.
3. V *Global.asax*, přidejte následující příkazy:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Pak přidejte zvýrazněné řádky, které se `Application_Start` metoda:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Tyto řádky kódu jsou co způsobí, že váš kód interceptoru spustit, když Entity Framework odesílá dotazy do databáze. Všimněte si, že vzhledem k tomu, že jste vytvořili samostatné interceptoru třídy pro simulaci přechodné chybě a protokolování, můžete nezávisle povolit či zakázat.   
  
 Můžete přidat pomocí sběrače `DbInterception.Add` metoda kdekoli v kódu; nemá v `Application_Start` metoda. Jinou možnost, pokud jste nepřidali sběrače v `Application_Start` metoda, by mohla být aktualizujte nebo přidejte třídu s názvem *WingtipToysConfiguration.cs* a umístí výše uvedený kód na konci tohoto konstruktoru `WingtipToysbConfiguration` třídy.

Kdekoli vložte tento kód, pečlivě není možné provést `DbInterception.Add` pro stejné interceptoru více než jednou, nebo budete získat další interceptoru instance. Například pokud přidáte interceptoru protokolování dvakrát, zobrazí dva protokoly pro každý dotaz SQL.

Sběrače jsou spouštěny v pořadí registrace (pořadí, v jakém `DbInterception.Add` metoda je volána). Pořadí může v závislosti na tom, jaké úlohy v zachycovač podstatné. Například může interceptoru změnit příkaz SQL, který získá v `CommandText` vlastnost. Pokud ho změnit příkazu SQL, získáte další interceptoru změněné příkazu SQL, není původní příkaz SQL.

Napsání kódu simulace přechodné chybě způsobem, který umožňuje způsobit přechodné chyby zadáním jiné hodnoty v uživatelském rozhraní. Jako alternativu můžete napsat kód interceptoru vždy generovat pořadí přechodný výjimek bez kontroly hodnotu konkrétní parametru. Potom můžete přidat zachycovač jenom v případě, že chcete vygenerovat přechodné chyby. Pokud to uděláte, ale nepřidáte interceptoru až po dokončení inicializace databáze. Jinými slovy proveďte operaci nejméně jednu databázi jako dotaz na jednom z vaší sady entit, před zahájením generování přechodné chyby. Rozhraní Entity Framework provede několik dotazů při inicializaci databáze a nejsou provést v transakci, takže chyby při inicializaci by mohlo způsobit kontext k získání nekonzistentním stavu.

## <a name="test-logging-and-connection-resiliency"></a>Testovací připojení a protokolování odolnost proti chybám

1. V sadě Visual Studio, stiskněte klávesu **F5** ke spuštění aplikace v režimu ladění a přihlaste se jako "Admin" pomocí "Pa$ $slovo" jako heslo.
2. Vyberte **správce** z navigačního panelu v horní části.
3. Zadejte nový produkt s názvem "Výjimku" souborem odpovídající popis, ceny a bitové kopie.
4. Stiskněte **přidat produktu** tlačítko.  
 Můžete si všimnout, že v prohlížeči zdá se, že zablokuje pro několik sekund, zatímco Entity Framework opakuje dotaz několikrát. První opakování se stane velmi rychle a pak zvyšuje čekání před každou další opakování. Tento proces nebude čekání před každou opakování nazývá *exponenciálního omezení rychlosti* .
5. Počkejte, dokud stránku už atttempting načíst.
6. Zastavte projektu a podívejte se na sady Visual Studio **výstup** a zobrazte výstup trasování. Můžete najít **výstup** okno výběrem **ladění**  - &gt; **Windows**  - &gt;  **Výstup**. Můžete chtít přejděte po několik další protokoly, které jsou zapsány ve vašem protokolovacího nástroje.  
  
 Všimněte si, zda se zobrazí skutečné příkazy jazyka SQL odeslal do databáze. Zobrazí některé počáteční dotazy a příkazy, které nemá Entity Frameworku Pokud chcete začít, kontrola tabulce historie verze a migraci databáze.   
    ![Okno Výstup](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
 Poznámka: Tento test nelze opakovat, pokud zastavte aplikaci a restartujte ji. Pokud chcete otestovat odolnost připojení více než jednou. v jedné spustit aplikace, můžete napsat kód, chcete-li obnovit čítač chyby v `InterceptorTransientErrors` .
7. Rozdíl strategii provádění (zásady opakování) provede, komentář `SetExecutionStrategy` řádek v *WingtipToysConfiguration.cs* v soubor *logiku* složky, spusťte **správce**  stránku znovu v režimu ladění a přidání produktu s názvem &quot;Throw&quot; znovu.  
  
 Tentokrát ladicí program zastaví v první generovaného výjimka při pokusu o provedení dotazu poprvé okamžitě.  
    ![Ladění – zobrazit podrobnosti](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Zrušením komentáře u `SetExecutionStrategy` řádek v *WingtipToysConfiguration.cs* souboru.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak upravit ukázkovou aplikaci webového formuláře pro podporu odolnost připojení a zachycením příkaz.

## <a name="next-steps"></a>Další kroky

Po kontrole odolnost připojení a příkaz zachycením v webových formulářů ASP.NET, přečtěte si téma webových formulářů ASP.NET [asynchronních metod v technologii ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Téma naučit se základy vytváření asynchronní aplikace webových formulářů ASP.NET pomocí sady Visual Studio.
