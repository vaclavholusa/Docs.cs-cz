---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Odolnost připojení a zachycením příkaz s použitím Entity Framework v aplikaci ASP.NET MVC | Microsoft Docs"
author: tdykstra
description: "Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1a28284e203904cc943e5e46b369e8a58ea5c820
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Odolnost připojení a zachycením příkaz s použitím Entity Framework v aplikaci ASP.NET MVC
====================
podle [tní Dykstra](https://github.com/tdykstra)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) nebo [stáhnout PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso univerzity ukázkovou webovou aplikaci demonstruje postup vytvoření aplikace ASP.NET MVC 5 s použitím Entity Framework 6 Code First a Visual Studio 2013. Informace o kurzu řady najdete v tématu [z prvního kurzu řady](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Pokud spuštění aplikace místně v IIS Express na vašem vývojovém počítači. Reálné aplikaci zpřístupnit ostatním uživatelům používat přes Internet, je nutné nasadit do webové poskytovatele hostitelských služeb a je nutné nasadit databázi k databázovému serveru.

V tomto kurzu se dozvíte jak používat dvě funkce Entity Framework 6, které jsou obzvláště cenná tehdy, když nasazujete do prostředí cloudu: odolnost připojení (automatické opakování pokusů pro přechodné chyby) a příkaz zachycením (dotazy SQL catch všechny odeslal do databáze za účelem protokolu nebo změnit).

V tomto kurzu pro zachycení odolnost a příkaz připojení je volitelný. Pokud přeskočíte v tomto kurzu, bude mít několik menší nastavení musí být uvedeny v následujících kurzech.

## <a name="enable-connection-resiliency"></a>Povolit odolnost připojení

Když nasadíte aplikaci do služby Windows Azure, budete nasazovat databáze k databázi SQL Windows Azure, Cloudová služba databáze. Připojení přechodné chyby jsou obvykle častější při připojení k databázi Cloudová služba než pokud váš webový server a serverem databáze jsou přímo připojené společně ve stejném datovém centru. I v případě cloudu webového serveru a databáze cloudové služby jsou hostované ve stejném datovém centru, existují další síťové připojení mezi nimi, které může mít problémy, například nástroje pro vyrovnávání zatížení.

Také Cloudová služba je obvykle sdílet jinými uživateli, což znamená, že jeho reakce mohou být ovlivněny je. A váš přístup k databázi může být předmětem omezení. Omezení znamená služba databáze vyvolá výjimek při pokusu o přístup k jeho další často než je povolený v smlouvy úrovni služeb (SLA).

Mnoho nebo většina připojení problémy při přístupech cloudové služby jsou přechodné, to znamená, že vyřešit samy v krátké době. Proto při operaci databáze a načíst typ chyby, který je obvykle přechodný, vám může opakujte akci po krátké počkejte a operace může být úspěšné. Pokud zpracovat přechodné chyby tak, že automaticky zkusíte znovu, můžete zadat mnohem lepší podmínky pro vaše uživatele viditelnosti většina z nich zákazníka. Funkce odolnost připojení na Entity Framework 6 automatizuje, že proces opakování se nezdařilo dotazy SQL.

Funkci odolnost připojení musí bylo správně nakonfigurované pro konkrétní databázi služby:

- Je třeba vědět, výjimek, které by mohly být přechodné. Chcete opakovat chyby způsobené k dočasné ztrátě v připojení k síti, nejsou chyby způsobené chybami programu, například.
- Má čekat odpovídající množství času mezi opakování operace se nezdařila. Můžete počkat již mezi opakovanými pokusy pro zpracování balíku než pro online webovou stránku, kde je uživatel čekání na odpověď.
- Má odpovídající počet doby, než se bude opakovat. Můžete chtít opakování vícekrát v batch proces, který by v aplikaci online.

Můžete nakonfigurovat tato nastavení ručně pro jakékoli prostředí databáze nepodporuje zprostředkovatele Entity Framework, ale výchozí hodnoty, které obvykle fungují dobře u online aplikace, která používá databázi SQL Windows Azure byla již nakonfigurována, a ty jsou nastavení, které budete implementovat pro aplikaci univerzity Contoso.

Všechny stačí povolit odolnost připojení je vytvořte třídu v vaše sestavení, která je odvozena z [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) třídy a v této třídě nastavte databázi SQL *strategii provádění*, což v EF je jiný termín pro *zásady opakování*.

1. Ve složce DAL přidejte soubor třídy s názvem *SchoolConfiguration.cs*.
2. Kód šablony nahraďte následujícím kódem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Rozhraní Entity Framework automaticky spustí kód najde v třídu odvozenou z `DbConfiguration`. Můžete použít `DbConfiguration` třídy konfigurační úkony v kódu, který by jinak uděláte *Web.config* souboru. Další informace najdete v tématu [EntityFramework konfigurace založené na kódu](https://msdn.microsoft.com/data/jj680699).
3. V *StudentController.cs*, přidejte `using` příkaz pro `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Změňte všechny `catch` blokuje této catch `DataException` výjimky tak, aby se catch `RetryLimitExceededException` výjimky místo. Příklad:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Jste používali `DataException` pokusit se identifikovat chyby, které můžou být přechodné, aby měla přátelskou zprávou "opakujte". Ale nyní, když jste zapnuli zásady opakovaných pokusů, jenom chyby, které by mohly být přechodné bude již byly se pokusila a se nezdařilo několikrát, a skutečný vrátil výjimku bude uzavřen do `RetryLimitExceededException` výjimka.

Další informace najdete v tématu [odolnost připojení Entity Framework nebo opakujte logiku](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Povolit příkaz zachycení

Teď, když jste zapnuli zásady opakovaných pokusů, jak můžete otestovat ověření, že funguje podle očekávání? Není tak snadno vynutit přechodná chyba provést, hlavně, když spouštíte místně, a je obzvláště složité je skutečný přechodné chyby integrovat do test automatizované jednotky. Chcete-li otestovat funkci odolnost připojení, potřebujete způsob, jak zachytávat dotazy, které Entity Framework odešle na SQL Server a nahraďte typ výjimky, který je obvykle přechodná odpověď serveru SQL.

Můžete také použít dotaz zachycení kvůli implementaci osvědčený postup pro cloudové aplikace: [protokolu latenci a úspěch nebo selhání všechna volání do externích služeb](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) například databázové služby. Poskytuje EF6 [vyhrazené protokolování API](https://msdn.microsoft.com/data/dn469464) , může být snazší protokolování, ale v této části kurzu budete Další informace o použití rozhraní Entity Framework [funkci zachycení](https://msdn.microsoft.com/data/dn469464) přímo, i pro protokolování a pro simulaci přechodné chyby.

### <a name="create-a-logging-interface-and-class"></a>Vytvoření rozhraní protokolování a – třída

A [nejvhodnější pro protokolování](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) se to udělat pomocí rozhraní místo pevně kódováno volání System.Diagnostics.Trace nebo třída protokolování. Který usnadňuje mechanismu protokolování později změnit, pokud byste někdy potřebovali udělat. Proto v této části vytvoříte rozhraní protokolování a třídu pro implementaci jej/p > 

1. Vytvořte složku v projektu s názvem *protokolování*.
2. V *protokolování* složky, vytvořte soubor třídy s názvem *ILogger.cs*a kód šablony nahraďte následujícím kódem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Rozhraní obsahuje tři úrovně trasování a určete relativní důležitost protokolů a z nich slouží k zadání informací o latence pro volání externí služby, jako je například databázové dotazy. Metody protokolování, mají přetížení, které umožňují předat výjimku. Toto je tak, aby informace o výjimce, včetně protokolů trasování a vnitřní výjimky je spolehlivě podle třídy, který implementuje rozhraní, aniž byste museli spoléhat na které se provádí v každém volání metody protokolování v celé aplikaci.

    Metody TraceApi umožňují sledovat latence každé volání externí služba například databáze SQL.
3. V *protokolování* složky, vytvořte soubor třídy s názvem *Logger.cs*a kód šablony nahraďte následujícím kódem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Implementace používá pro trasování System.Diagnostics. Toto je integrované funkce rozhraní .NET, což usnadňuje generování a použijte informace trasování. Existuje mnoho "naslouchací procesy" můžete použít s trasováním System.Diagnostics, zápis do souborů, například protokoly a jejich zápis do úložiště objektů blob v Azure. Některé z možností a odkazy na další zdroje informací pro další informace najdete v tématu v [řešení potíží s weby Azure v sadě Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Pro tento kurz se budete pouze podíváte na protokoly v sadě Visual Studio **výstup** okno.

    V případě produkční aplikace byste měli vzít v úvahu trasování balíčky než System.Diagnostics a rozhraní objektu ILogger umožňuje přepnout do různých trasování mechanismus, pokud se rozhodnete k tomu je poměrně snadné.

### <a name="create-interceptor-classes"></a>Vytvořit zachytávací modul třídy

Dále vytvoříte třídy, které rozhraní Entity Framework zavolá do pokaždé, když se má odeslat dotaz na databázi, jeden pro simulaci přechodné chyby a jeden udělat protokolování. Tyto třídy interceptoru musí být odvozeny od `DbCommandInterceptor` třídy. V nich zapisovat metoda přepsání, které jsou automaticky volána, když dotaz se chystá provést. V těchto metod můžete prozkoumat nebo protokolu dotaz odeslaný do databáze, a můžete změnit dotaz před odesláním do databáze nebo vrátit něco Entity Framework sami bez i předávání dotaz do databáze.

1. Pro vytvoření třídy interceptoru, který bude protokolovat každý dotaz SQL, který je odeslal do databáze, vytvořte soubor třídy s názvem *SchoolInterceptorLogging.cs* v *DAL* složku a nahraďte kód šablony s Následující kód:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Pro úspěšné dotazy nebo příkazy tento kód zapíše protokol informací s informacemi o latence. Pro výjimky vytvoří se protokol chyb.
2. Pro vytvoření třídy interceptoru, který vygeneruje fiktivní přechodné chyby, když zadáte "Výjimku" v **vyhledávání** pole, vytvořte soubor třídy s názvem *SchoolInterceptorTransientErrors.cs* v *DAL* složky a nahraďte kód šablony s následujícím kódem:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Tento kód pouze přepsání `ReaderExecuting` metodu, která je volána pro dotazy, které může vrátit více řádků dat. Pokud jste chtěli zkontrolujte odolnost připojení pro jiné typy dotazů, může také přepsat `NonQueryExecuting` a `ScalarExecuting` metody, jako interceptoru protokolování nemá.

    Při spuštění stránky Student a zadejte hledaný řetězec "Výjimku", tento kód vytvoří fiktivní výjimka databáze SQL pro chyby číslo 20, typu ví, nejčastěji přechodné. Další chyba čísla aktuálně rozpoznán jako přechodný jsou 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 a 40613, ale to se může měnit v nové verze databáze SQL.

    Kód vrátí Entity Framework namísto spuštění dotazu a předání výsledky dotazu back výjimku. Přechodný výjimka je vrácen čtyřikrát a pak se kód vrátí do běžný postup předat dotaz do databáze.

    Vzhledem k tomu, že je všechno, co přihlášení, budete moci zobrazit Entity Framework se pokusí provést dotaz čtyřikrát před nakonec úspěšné, a v aplikaci jediným rozdílem je, že trvá déle k vykreslení stránky s výsledky dotazu.

    Počet, který se bude opakovat rozhraní Entity Framework se dá konfigurovat; kód určuje čtyřikrát, protože se jedná o výchozí hodnotu pro zásady spouštění databáze SQL. Pokud změníte zásady spouštění, musíte také změnit zde kód, který určuje, kolikrát se generují přechodné chyby. Můžete také změnit kód ke generování další výjimky tak, aby rozhraní Entity Framework vyvolá výjimku `RetryLimitExceededException` výjimka.

    Hodnota, zadejte do vyhledávacího pole bude v `command.Parameters[0]` a `command.Parameters[1]` (jeden se používá pro křestní jméno a jeden pro název poslední). Když se najde hodnotu "% Throw %", "Výjimku" je nahrazena v těchto parametrů "služby" tak, aby některé studenti, kteří budou nalezena a vrácena.

    Toto je jenom pohodlný způsob pro testování odolnost připojení na základě změny některé vstup aplikace uživatelského rozhraní. Můžete také psát kód, který generuje přechodné chyby pro všechny dotazy nebo aktualizace, jak je popsáno dále v komentáře o *DbInterception.Add* metoda.
3. V *Global.asax*, přidejte následující `using` příkazy:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Přidat zvýrazněné řádky, které se `Application_Start` metoda:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Tyto řádky kódu jsou co způsobí, že váš kód interceptoru spustit, když Entity Framework odesílá dotazy do databáze. Všimněte si, že vzhledem k tomu, že jste vytvořili samostatné interceptoru třídy pro simulaci přechodné chybě a protokolování, můžete nezávisle povolit či zakázat.

    Můžete přidat pomocí sběrače `DbInterception.Add` metoda kdekoli v kódu; nemá v `Application_Start` metoda. Další možností je uvést tento kód ve třídě DbConfiguration, který jste vytvořili dříve a nastavte zásady spouštění.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Kdekoli vložte tento kód, pečlivě není možné provést `DbInterception.Add` pro stejné interceptoru více než jednou, nebo budete získat další interceptoru instance. Například pokud přidáte interceptoru protokolování dvakrát, zobrazí dva protokoly pro každý dotaz SQL.

    Sběrače jsou spouštěny v pořadí registrace (pořadí, v jakém `DbInterception.Add` metoda je volána). Pořadí může v závislosti na tom, jaké úlohy v zachycovač podstatné. Například může interceptoru změnit příkaz SQL, který získá v `CommandText` vlastnost. Pokud ho změnit příkazu SQL, získáte další interceptoru změněné příkazu SQL, není původní příkaz SQL.

    Napsání kódu simulace přechodné chybě způsobem, který umožňuje způsobit přechodné chyby zadáním jiné hodnoty v uživatelském rozhraní. Jako alternativu můžete napsat kód interceptoru vždy generovat pořadí přechodný výjimek bez kontroly hodnotu konkrétní parametru. Potom můžete přidat zachycovač jenom v případě, že chcete vygenerovat přechodné chyby. Pokud to uděláte, ale nepřidáte interceptoru až po dokončení inicializace databáze. Jinými slovy proveďte operaci nejméně jednu databázi jako dotaz na jednom z vaší sady entit, před zahájením generování přechodné chyby. Rozhraní Entity Framework provede několik dotazů při inicializaci databáze a nejsou provést v transakci, takže chyby při inicializaci by mohlo způsobit kontext k získání nekonzistentním stavu.

## <a name="test-logging-and-connection-resiliency"></a>Testovací připojení a protokolování odolnost proti chybám

1. Stisknutím klávesy F5 spusťte aplikaci v režimu ladění a klikněte **studenty** kartě.
2. Podívejte se na sady Visual Studio **výstup** a zobrazte výstup trasování. Možná budete muset posunout nahoru po některé chyby jazyka JavaScript, abyste se dostali na protokoly zapsaných správcem vaší protokolovacího nástroje.

    Všimněte si, zda se zobrazí skutečné příkazy jazyka SQL odeslal do databáze. Zobrazí některé počáteční dotazy a příkazy, které Entity Framework nemá Pokud chcete začít, kontrola verze databáze a tabulky historie migrací (získáte informace o migrace v dalším kurzu). Zobrazit dotaz pro stránkování, chcete-li zjistit, kolik studenti, kteří existují, a nakonec se zobrazí dotaz, který získá student data.

    ![Protokolování pro normální dotazu](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. V **studenty** stránky, zadejte hledaný řetězec "Výjimku" a klikněte na tlačítko **vyhledávání**.

    ![Throw – řetězec pro hledání](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Můžete si všimnout, že v prohlížeči zdá se, že zablokuje pro několik sekund, zatímco Entity Framework opakuje dotaz několikrát. První opakování se stane velmi rychle potom počkejte před zvyšuje před každou další opakování. Tento proces nebude čekání před každou opakování nazývá *exponenciálního omezení rychlosti*.

    Pokud na stránce se zobrazuje, zobrazující studenty, kteří mají "na" v jejich názvy, podívejte se na ve výstupním okně a zobrazí stejný dotaz byl proveden pokus o pětkrát, nejprve čtyřikrát vrácením přechodné výjimky. Pro každý přechodná chyba se zobrazí v protokolu, který zapisovat při generování přechodná chyba v `SchoolInterceptorTransientErrors` – třída ("Returning přechodná chyba pro příkaz...") a zobrazí se v protokolu zapisovat při `SchoolInterceptorLogging` získá výjimku.

    ![Protokolování výstupu zobrazující opakování](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Vzhledem k tomu, že zadaný hledaný řetězec dotazu, který vrací student data je parametry:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Hodnota parametry nejsou protokolování, ale můžete to udělat. Pokud chcete zobrazit hodnoty parametrů, můžete napsat kód protokolování k získání hodnot parametrů z `Parameters` vlastnost `DbCommand` objekt, který můžete získat v interceptoru metody.

    Poznámka: Tento test nelze opakovat, pokud zastavte aplikaci a restartujte ji. Pokud chcete otestovat odolnost připojení více než jednou. v jedné spustit aplikace, můžete napsat kód, chcete-li obnovit čítač chyby v `SchoolInterceptorTransientErrors`.
4. Rozdíl v strategii provádění (zásady opakování) provede, komentář `SetExecutionStrategy` řádek v *SchoolConfiguration.cs*, znovu spusťte stránce studenty v režimu ladění a vyhledejte řetězec "Throw" znovu.

    Tentokrát ladicí program zastaví v první generovaného výjimka při pokusu o provedení dotazu poprvé okamžitě.

    ![Fiktivní výjimky](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Zrušením komentáře u *SetExecutionStrategy* řádek v *SchoolConfiguration.cs*.

## <a name="summary"></a>Souhrn

V tomto kurzu jste se seznámili povolení odolnost připojení a protokolu příkazy SQL, které Entity Framework vytvoří a odešle do databáze. V dalším kurzu nasadíte aplikaci do Internetu, použít migrace Code First pro nasazení databáze.

Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit. Můžete také vyžádat nová témata na [zobrazit mi jak s kód](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Odkazy na další zdroje Entity Framework najdete v [přístup k datům ASP.NET - doporučené prostředky](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Předchozí](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[další](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
