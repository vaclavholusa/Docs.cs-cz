---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Odolnost připojení rozhraní ASP.NET Web Forms a zachycení příkazů | Dokumentace Microsoftu
author: Erikre
description: Tento kurz popisuje, jak upravit ukázkové aplikace pro podporu odolnost připojení a zachycení příkazů.
ms.author: aspnetcontent
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 9a66b297608a5a8cd536b9af2a9ae4bb600a6bbb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807086"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Odolnost připojení rozhraní ASP.NET Web Forms a zachycení příkazů
====================
podle [Erik Reitan](https://github.com/Erikre)

V tomto kurzu se změní na adresář Wingtip Toys ukázkové aplikaci, aby podporovala odolnost připojení a zachycení příkazů. Když povolíte odolnost připojení, ukázkové aplikace Wingtip Toys automaticky zopakuje volání dat Pokud dojde k přechodným chybám, které jsou typické pro cloudové prostředí. Navíc implementací zachycení příkazů ukázkovou aplikaci Wingtip Toys zachytí všechny dotazy SQL odeslané do databáze za účelem protokolu nebo je změnit.

> [!NOTE] 
> 
> V tomto kurzu webových formulářů byl založen na Tom Dykstra následující kurz ASP.NET MVC:  
> [Odolnost připojení a zachycení příkazů s Entity Framework v aplikaci ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Co se dozvíte:

- Jak zajistit odolnost připojení.
- Jak implementovat zachycení příkazů.

## <a name="prerequisites"></a>Požadavky

Než začnete, ujistěte se, že máte v počítači nainstalovaný následující software:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) nebo [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Rozhraní .NET Framework se instaluje automaticky.
- Wingtip Toys ukázkový projekt, takže můžete implementovat funkci uvedených v tomto kurzu v rámci projektu na adresář Wingtip Toys. Pod následujícím odkazem najdete podrobnosti o stahování:

    - [Začínáme s ASP.NET 4.5.1 webové formuláře – na adresář Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (C#)
- Před tímto kurzem, vezměte v úvahu Kontrola souvisejících kurzů [Začínáme se službou webových formulářů ASP.NET 4.5 a Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Série kurzů jste se seznámili s pomůže **Northwind** projektu a kódu.

## <a name="connection-resiliency"></a>Odolnost připojení

Při zvažování nasazení aplikace do Windows Azure je jednou z možností vzít v úvahu nasazení databáze, kterou chcete **Windows** **Azure SQL Database**, Cloudová databázová služba. Připojení přechodné chyby jsou obvykle častější při připojování k Cloudová databázová služba než kdy webový server a databáze serveru byli přímo připojení společně ve stejném datovém centru. I v případě, že webový server cloud a Cloudová databázová služba hostované ve stejném datovém centru, existují další síťová připojení mezi nimi, které mohou mít problémy, jako jsou nástroje pro vyrovnávání zatížení.

Také cloudovou službu se obvykle sdílí jiní uživatelé, což znamená, že jeho reakce mohou být ovlivněny. A váš přístup k databázi může podléhat omezení šířky pásma. Omezení znamená, že databázová služba vyvolá výjimky při pokusu o přístup k častěji, než je povolené ve vaší *smlouvu o úrovni služeb* (SLA).

Mnoho nebo většinu problémy s připojením, ke kterým dochází při přístupech cloudové služby jsou přechodné, to znamená, že tyto možnosti vyřeší sami v krátké době. Proto při operaci databáze a získat typ chyby, je nejčastěji přechodné, můžete zkusit operaci zopakovat po krátkém čekání a operace může být úspěšné. Pokud zpracování přechodných chyb tak, že automaticky zkusíte znovu, můžete zadat mnohem lepší prostředí pro vaše uživatele viditelnosti většina z nich zákazníka. Funkce odolnost připojení v Entity Framework 6 automatizuje, že proces opakování neúspěšných dotazů SQL.

Funkce odolnosti proti chybám připojení musí být správně nakonfigurovaný pro konkrétní databázi služby:

1. Je třeba vědět, jaké výjimky jsou může být přechodná. Chcete opakovat chyb způsobených k dočasné ztrátě v připojení k síti, nejsou chyby způsobené chyby programu, například.
2. Má odpovídající množství času mezi opakovanými pokusy o neúspěšné operaci čekání. Můžete počkat déle mezi opakovanými pokusy pro dávkové zpracování než pro online webovou stránku, kde uživatel je čekání na odpověď.
3. Má to chcete zkusit znovu příslušným počtem niž se. Můžete chtít opakovat je víckrát v dávkové zpracování, která byste to udělali v aplikaci online.

Můžete nakonfigurovat tato nastavení ručně pro všechny databáze prostředí podporovaná službou zprostředkovatele Entity Framework.

Vše, co musíte udělat, aby povolit odolnost připojení, je vytvořit třídu v sestavení, která je odvozena z `DbConfiguration` třídy a v dané třídě nastavte strategie provádění SQL Database, což je jiný výraz zásad pro opakované pokusy v Entity Framework.

### <a name="implementing-connection-resiliency"></a>Implementace odolnost připojení

1. Stáhnout a otevřít [Northwind](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) ukázková aplikace webových formulářů v sadě Visual Studio.
2. V *logiky* složky **Northwind** aplikací, přidejte soubor třídy s názvem *WingtipToysConfiguration.cs*.
3. Nahraďte stávající kód následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Entity Framework automaticky spustí kód, který najde ve třídě, která je odvozena z `DbConfiguration`. Můžete použít `DbConfiguration` třídě, aby provedl úlohy konfigurace v kódu, který by jinak provedete *Web.config* souboru. Další informace najdete v tématu [EntityFramework konfigurace založená na kódu](https://msdn.microsoft.com/data/jj680699).

1. V *logiky* složku, otevřete *AddProducts.cs* souboru.
2. Přidat `using` příkaz pro `System.Data.Entity.Infrastructure` znázorněno zvýrazněné žlutou barvou:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Přidat `catch` bloku `AddProduct` metoda tak, aby `RetryLimitExceededException` je zaznamenána v zvýrazněné žlutou barvou:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Přidáním `RetryLimitExceededException` výjimku, může poskytovat lepší protokolování nebo zobrazení chybové zprávy pro uživatele, kde můžete vybrat znovu procesu. Pomocí zachycování `RetryLimitExceededException` výjimku, může být přechodná pouze chyby se již mít se snažily neúspěšně několikrát. Skutečné výjimky vrátil se bude zabalena do `RetryLimitExceededException` výjimky. Kromě toho jste také přidali obecný zachytávací blok. Další informace o `RetryLimitExceededException` výjimky, naleznete v tématu [odolnost Entity Framework připojení / Logika opakování](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Zachycení příkazů

Teď, když jste zapnuli zásady opakování, jak můžete otestovat ověření, že funguje podle očekávání? Není tak snadné vynutit o přechodnou chybu, která se provede, zejména pokud používáte místně a by být obzvláště složité je integrovat skutečné přechodné chyby testu jednotek automatizované. K otestování funkce odolnosti proti chybám připojení, potřebujete způsob, jak zachytit dotazy, které odešle Entity Framework pro SQL Server a nahraďte odpovědi serveru SQL Server, který je obvykle přechodný typ výjimky.

Aby mohla implementovat osvědčený postup pro cloudové aplikace můžete také použít zachycení dotazu: protokolu latenci a úspěchu nebo selhání všech volání do externí služby, například databázové služby.

V této části kurzu budete používat Entity Framework [ *zachycení funkce* ](https://msdn.microsoft.com/data/dn469464) pro protokolování a pro simulaci přechodné chyby.

### <a name="create-a-logging-interface-and-class"></a>Vytvoření protokolování rozhraní a třídy

Osvědčeným postupem pro protokolování je to udělat pomocí [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) namísto pevně kódováno pomocí volání `System.Diagnostics.Trace` nebo třídu protokolování. Který usnadňuje mechanismu protokolování později změnit, pokud byste někdy potřebovali udělat. Takže v této části vytvoříte protokolování rozhraní a třídy na jeho implementaci.

Podle výše uvedeného postupu, mají stáhne a otevře **Northwind** ukázkovou aplikaci v sadě Visual Studio.

1. Vytvořte složku v **Northwind** projektu a pojmenujte ho *protokolování*.
2. V *protokolování* složce vytvořte soubor třídy *ILogger.cs* a nahraďte kód následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Rozhraní obsahuje tři úrovně trasování a určete relativní důležitost protokoly a je určen k zadání informací o latenci pro volání externích služeb, například databázové dotazy. Protokolování metody mají přetížení, které umožňují předat výjimku. Je to tak, aby se informace o výjimkách včetně zásobníku trasování a vnitřní výjimky spolehlivě protokoluje třídou, která implementuje rozhraní, aniž byste museli spoléhat na, který provádí v jednotlivých voláních metody protokolování v celé aplikaci.  
  
   `TraceApi` Metody umožňují vám umožní sledovat latenci každého volání externí službě, jako je SQL Database.
3. V *protokolování* složce vytvořte soubor třídy *Logger.cs* a nahraďte kód následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Implementace používá `System.Diagnostics` provést trasování. Toto je integrovaná funkce .NET, takže se dá snadno generovat a používat informace o trasování. Existuje několik instancí &quot;naslouchacích procesů&quot; můžete používat s `System.Diagnostics` trasování, zapisují protokoly do souborů, například nebo zapisovat do úložiště objektů blob v Azure s Windows. Některé z možností a odkazy na další zdroje informací pro další informace najdete v tématu v [řešení potíží s Windows webů Azure v sadě Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Pro účely tohoto kurzu budete pouze podíváte na protokoly v sadě Visual Studio **výstup** okna.

V produkční aplikace můžete chtít zvážit použití trasování rozhraní než `System.Diagnostics`a `ILogger` rozhraní je poměrně snadné ho přepnout na mechanismus různých trasování, pokud se rozhodnete provést.

### <a name="create-interceptor-classes"></a>Vytvoření třídy zachycování

V dalším kroku vytvoříte třídy, které rozhraní Entity Framework volat pokaždé, když se chce odešle dotaz do databáze, jeden pro simulaci přechodné chyby a jeden provedete protokolování. Tyto třídy zachycování musí být odvozen z `DbCommandInterceptor` třídy. V nich můžete napsat přepsání metody, které jsou automaticky volána, když je dotaz se pokračovalo. V těchto metodách můžete prozkoumat nebo protokolu dotaz odeslaný do databáze, a můžete změnit dotaz před odesláním do databáze nebo vracely něco k Entity Frameworku sami bez i předá dotaz do databáze.

1. Pro vytvoření třídy zachycování, které budou protokolovány každého dotazu SQL, před odesláním do databáze, vytvořte soubor třídy *InterceptorLogging.cs* v *logiky* složky a nahradit výchozí kódování s Následující kód:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Tento kód pro úspěšné dotazy nebo příkazy, zapíše protokol informací s informace o latenci. Pro výjimky vytvoří protokol chyb.
2. Pro vytvoření třídy zachycování, který vygeneruje fiktivní přechodné chyby, když zadáte &quot;Throw&quot; v **název** textového pole na stránce s názvem *AdminPage.aspx*, vytvořte třídu soubor s názvem *InterceptorTransientErrors.cs* v *logiky* složky a nahradit výchozí kód následujícím kódem:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Tento kód jenom přepsání `ReaderExecuting` metodu, která je volána pro dotazy vracející více řádků dat. Pokud chcete zkontrolovat odolnost připojení pro jiné typy dotazů, může také přepsat `NonQueryExecuting` a `ScalarExecuting` metody, jako protokolování zachycování nepodporuje.  
  
   Později, se přihlaste jako "Admin" a vyberte **správce** odkaz na horním navigačním panelu. Potom na *AdminPage.aspx* stránky se přidat produkt s názvem &quot;Throw&quot;. Kód vytvoří fiktivní výjimka SQL Database pro číslo chyby 20, typ ví, že je nejčastěji přechodné. Jsou různé počty chyb aktuálně rozpoznána jako přechodné 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 a 40613, ale ty se mohou změnit v nových verzích služby SQL Database. Produkt se přejmenují na "TransientErrorExample", který můžete použít v kódu *InterceptorTransientErrors.cs* souboru.  
  
   Vrátí kód výjimky k Entity Frameworku namísto spuštění dotazu a předá zpět výsledky. Vrátí přechodnou výjimkou *čtyři* dobu, a potom vrátí kód na běžný postup předáním dotaz do databáze.

    Vzhledem k tomu, že všechno, co se zaznamená, budete moct zjistit, že Entity Framework pokusí o provedení dotazu čtyřikrát před nakonec úspěšná, a jediným rozdílem v aplikaci je, že trvá déle k vykreslení stránky s výsledky dotazu.  
  
   Počet pokusů, které se bude opakovat Entity Framework je možné konfigurovat; kód určuje čtyřikrát, protože to je výchozí hodnota pro zásady spouštění SQL Database. Pokud změníte zásady spouštění, máte také změnit zde kód, který určuje, kolikrát se generují přechodné chyby. Můžete také změnit kód tak, aby se vyvolá rozhraní Entity Framework generovat další výjimky `RetryLimitExceededException` výjimky.
3. V *Global.asax*, přidejte následující příkazy using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Pak přidejte zvýrazněné řádky a `Application_Start` metody:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Tyto řádky kódu se, co způsobí, že váš kód zachycování má být spuštěn při Entity Framework odesílá dotazy do databáze. Všimněte si, že vzhledem k tomu, že jste vytvořili samostatné zachycování třídy pro přechodná chyba simulace a protokolování, můžete nezávisle na sobě povolit a zakázat.   
  
 Můžete přidat pomocí sběrače `DbInterception.Add` metodu kdekoli v kódu; nemusí být v `Application_Start` metoda. Další možnost, pokud jste nepřidali sběrače v `Application_Start` je metoda, aktualizujte nebo přidejte třídu s názvem *WingtipToysConfiguration.cs* a vložte výše uvedené na konci konstruktoru `WingtipToysbConfiguration` třídy.

Všude, kde vložte tento kód, dejte pozor, ke spuštění `DbInterception.Add` pro stejný zachycování více než jednou, ale zobrazí se další zachycování instance. Například pokud přidáte zachycování protokolování dvakrát, zobrazí se vám dva protokolů pro každý dotaz SQL.

Sběrače jsou provedeny v pořadí registrace (pořadí, ve kterém `DbInterception.Add` volání metody). V závislosti na tom, co děláte v Sběrač může být důležité pořadí. Například zachycování může změnit pomocí příkazu SQL, který získá v `CommandText` vlastnost. Pokud ji změnit pomocí příkazu SQL, se zobrazí další zachycování změněné příkaz SQL, nikoli původní příkaz SQL.

Jste napsali kód přechodná chyba simulace způsobem, který umožňuje způsobit přechodné chyby tak, že zadáte jinou hodnotu v uživatelském rozhraní. Jako alternativu můžete napsat kód zachycování vždy generovat posloupnost přechodným výjimkám bez kontroly pro konkrétní parametr hodnoty. Sběrač, pak můžete přidat pouze v případě, že chcete vygenerovat přechodné chyby. Pokud to uděláte, ale nepřidávejte zachycování až po dokončení inicializace databází. Jinými slovy proveďte nejméně jednu databázi operace, jako například dotaz na jednom ze sady entit, před zahájením generování přechodné chyby. Entity Framework provede několik dotazů během inicializace databází a nebudou provedeny v transakci, takže chyby při inicializaci by mohlo způsobit kontext k získání do nekonzistentního stavu.

## <a name="test-logging-and-connection-resiliency"></a>Test připojení k protokolování a odolnost proti chybám

1. V sadě Visual Studio, stiskněte klávesu **F5** ke spuštění aplikace v režimu ladění a přihlaste se jako "Admin" pomocí "Pa$ $slovo" jako heslo.
2. Vyberte **správce** z navigačního panelu v horní části.
3. Zadejte nový produkt s názvem "Vyvolat" souborem příslušný popis, cena a obrázek.
4. Stisknutím klávesy **přidat produkt** tlačítko.  
   Můžete si všimnout, že v prohlížeči vypadá, že přestane reagovat na několik sekund, zatímco rozhraní Entity Framework se opakovaně pokouší o dotazu několikrát. Velmi rychle se stane první opakování, a pak zvyšuje čekání před každou další zkuste to znovu. Tento proces už čekání před voláním opakováními *exponenciálního omezení rychlosti* .
5. Počkejte, dokud stránce už není atttempting načíst.
6. Zastavit projektu a podívejte se na Visual Studio **výstup** okno a zobrazit výstup trasování. Můžete najít **výstup** okna tak, že vyberete **ladění**  - &gt; **Windows**  - &gt;  **Výstup**. Budete muset posuňte se za několik protokolů autorem vaše protokolovacího nástroje.  
  
   Všimněte si, že vidíte skutečné SQL dotazy odeslané do databáze. Uvidíte některé počáteční dotazy a příkazy, které pokud chcete začít, Entity Framework kontroluje tabulku historie verzí a migraci databáze.   
    ![Okno Výstup](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Všimněte si, že tento test nelze opakovat, pokud aplikaci zastavte a restartujte ji. Pokud chcete mít možnost Testovat odolnost připojení více než jednou v jedné spuštění aplikace, můžete napsat kód Vynulovat čítač Chyba v `InterceptorTransientErrors` .
7. Uvidíte rozdíl strategie provádění (zásady opakování) vystaví, komentář `SetExecutionStrategy` řádku v *WingtipToysConfiguration.cs* ve *logiky* složky, spusťte **správce**  stránku znovu nezobrazovat v režimu ladění a přidat produkt s názvem &quot;Throw&quot; znovu.  
  
   Tentokrát ladicí program se zastaví při první výjimky generované okamžitě při pokusu o provedení dotazu poprvé.  
    ![Ladění – zobrazit podrobnosti](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Zrušením komentáře u `SetExecutionStrategy` řádku v *WingtipToysConfiguration.cs* souboru.

## <a name="summary"></a>Souhrn

V tomto kurzu jste viděli, jak upravit ukázkovou aplikaci webových formulářů k podpoře odolnost připojení a zachycení příkazů.

## <a name="next-steps"></a>Další kroky

Po zkontrolování odolnost připojení a zachycení příkazů do webových formulářů ASP.NET, přečtěte si téma webových formulářů ASP.NET [asynchronních metod v ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). V tématu se seznámíte se základy vytváření asynchronní aplikace webových formulářů ASP.NET pomocí sady Visual Studio.
