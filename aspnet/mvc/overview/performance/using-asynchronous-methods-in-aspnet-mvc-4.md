---
uid: mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
title: Použití asynchronních metod v architektuře ASP.NET MVC 4 | Dokumentace Microsoftu
author: Rick-Anderson
description: V tomto kurzu se seznámíte se základy vytváření asynchronní aplikace ASP.NET MVC Web pomocí Visual Studio Express 2012 pro Web, který je zdarma ve...
ms.author: riande
ms.date: 06/06/2012
ms.assetid: a56572ba-81c3-47af-826d-941e9c4775ec
msc.legacyurl: /mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 5469ac18f55001b441def5b547b7f1836ee9d052
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752210"
---
<a name="using-asynchronous-methods-in-aspnet-mvc-4"></a>Použití asynchronních metod v architektuře ASP.NET MVC 4
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se seznámíte se základy vytváření asynchronní aplikace v prostředí ASP.NET MVC pomocí [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11), což je bezplatná verze sady Microsoft Visual Studio. Můžete také použít [Visual Studio 2012](https://www.microsoft.com/visualstudio/11).
> 
> Úplnou ukázku je k dispozici pro účely tohoto kurzu na githubu  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/)


ASP.NET MVC 4 [řadič](https://msdn.microsoft.com/library/system.web.mvc.controller(VS.108).aspx) třídy v kombinaci [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) umožňuje psát asynchronní akce metody, které vracejí objekt typu [úloh&lt;ActionResult&gt; ](https://msdn.microsoft.com/library/dd321424(VS.110).aspx). Rozhraní .NET Framework 4 zavedena asynchronní programovací koncept se označuje jako [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) a podporuje ASP.NET MVC 4 [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Úkoly jsou reprezentovány **úloh** typu a souvisejících typů v [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) oboru názvů. Rozhraní .NET Framework 4.5 je založena na této asynchronní podporu [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova, která usnadňuje práci s [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objekty mnohem méně složitý než předchozí asynchronní přístupy. [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo je syntaktické sdružená hodnota určující, které jsou části kódu by měla asynchronně čekat na další část kódu. [Asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo představuje pomocného parametru, který můžete použít k označení metod jako úkolově orientovanou asynchronní metody. Kombinace **await**, **asynchronní**a **úloh** objektu je snazší pro vás bude psaní asynchronního kódu v rozhraní .NET 4.5. Nový model pro asynchronní metody je volána *Task-based Asynchronous Pattern* (**klepněte**). Tento kurz předpokládá, že máte některé znalost asynchronní programování pomocí [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) oboru názvů.

Další informace na pomocí [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) obor názvů, naleznete v následujících odkazech.

- [Dokument White Paper: Asynchronii v rozhraní .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Async/Await – nejčastější dotazy](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchronní programování Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>  Zpracování požadavků ve fondu vláken

Rozhraní .NET Framework na webový server udržuje fondu vláken, které se používají ke zpracování požadavků ASP.NET. Když přijde požadavek, vlákno z fondu odesílá ke zpracování této žádosti. Pokud je požadavek zpracován synchronně, vlákna, která zpracuje požadavek je zaneprázdněn při žádost se zpracovává, a že vlákno nemůže jiná žádost.   
  
To nemusí být problém, protože fond vláken můžete převést na dostatečně velký, aby mnoho zaneprázdněná vlákna. Ale je omezený počet vláken ve fondu vláken (výchozí maximum pro rozhraní .NET 4.5 je 5 000). Ve velkých aplikací s vysokou souběžnosti dlouhodobé požadavky může být k dispozici všechna vlákna zaneprázdněn. Tento stav se označuje jako vyčerpání vlákna. Když je dosaženo této podmínce, webový server zařadí do fronty požadavků. Pokud zaplnění fronty požadavků, webový server odmítá požadavky se stavem HTTP 503 (příliš zaneprázdněný Server). Fondu vláken CLR má omezení na nové vlákno injektáže. Pokud je souběžnost vzorcům (to znamená, vaše webová stránka náhle získáte velký počet požadavků) a všechna vlákna k dispozici žádosti je zaneprázdněná kvůli volání back-end s vysokou latencí, míra vkládání omezené vlákna můžete provádět reagovat velmi špatný. Kromě toho má každém novém vláknu přidán do fondu vláken režijní náklady (jako je například 1 MB paměti zásobníku). V případě webové aplikace pomocí synchronní metody volání mezi službami vysokou latencí, kde fondu vláken se zvětšuje výchozí rozhraní .NET 4.5 maximálně 5 000 vlákna by využívat přibližně 5 GB více paměti, než je možné aplikaci na stejné žádosti o službu pomocí asynchronní metody a pouze 50 vláken. Když provádíte asynchronní práce, ne vždy používáte vlákno. Například pokud provedete požadavek asynchronní webové služby, ASP.NET nebudete používat žádného vlákna mezi **asynchronní** volání metody a **await**. Použití fondu vláken k žádosti o služby s vysokou latencí může vést k velké paměťové nároky a nízké využití hardwaru serveru.

## <a name="processing-asynchronous-requests"></a>Asynchronní zpracování požadavků

Ve webové aplikaci, která se zobrazí velký počet souběžných požadavků na objem požadovaný při spuštění nebo má vzorcům zatížení (kde souběžnosti prudce zvyšuje) provádění asynchronní volání webové služby zvyšuje rychlost odezvy aplikace. Asynchronní požadavek trvá stejně jako požadavek na synchronní zpracování. Pokud požadavek webové služby volání, které vyžaduje dva sekund na dokončení, požadavek trvá 2 sekundy, zda je provedena synchronně nebo asynchronně. Ale při asynchronním volání není vlákno blokováno reagovat na další požadavky a bude čekat na dokončení první žádosti. Proto asynchronních žádostí zabránit růstu žádosti o služby Řízení front a vlákna fondu pokud existuje velký počet souběžných požadavků, které vyvolají dlouho běžící operace.

## <a id="ChoosingSyncVasync"></a>  Volba metody synchronní nebo asynchronní akce

Tato část uvádí pokyny pro použití metody synchronní nebo asynchronní akce. Toto jsou jen pokyny; Prozkoumejte každou aplikaci zvlášť pro určení, zda asynchronní metody pomoci s výkonem.

Obecně platí používejte synchronní metody byly splněny následující podmínky:

- Operace jsou jednoduché nebo krátce běžící.
- Zjednodušení je mnohem důležitější než efektivitu.
- Operace jsou primárně procesoru operace místo operace, které zahrnují rozsáhlou disku nebo nároky na síť. Pomocí metod asynchronní akce pro operace vázané na procesor žádné výhody a má za následek další režie.

  Obecně platí použijte asynchronní metody byly splněny následující podmínky:

- Volání služby, které lze využívat pomocí asynchronní metody a používáte .NET 4.5 nebo vyšší.
- Operace jsou vázané na síť nebo vstupně-výstupní místo vázané na procesor.
- Paralelismu je důležitější než zjednodušení kódu.
- Chcete poskytovat mechanismus, který umožňuje uživatelům zrušit dlouho probíhající požadavek.
- Když výhodu, že přepínání vlákna větší váhu než náklady na přepnutí kontextu. Obecně platí měli byste zajistit metodu asynchronní Pokud se synchronní metoda čeká na vlákno požadavku ASP.NET v průběhu zpracování operace žádná práce. Tím, že volání asynchronní, vlákno požadavku ASP.NET není zastaven, přičemž to nic a bude čekat na dokončení žádosti webové služby.
- Testování ukazuje, že jsou blokující operace problémové místo výkonu webu a, že služba IIS můžete další žádosti o služby s použitím těchto blokujících volání asynchronní metody.

  Ukázky ke stažení ukazuje, jak efektivně používat metody asynchronní akce. Poskytnutou ukázkou, je navržená k poskytování jednoduchého ukázka asynchronního programování v architektuře ASP.NET MVC 4 pomocí rozhraní .NET 4.5. Ukázka nemá představovat referenční architekturu pro asynchronní programování v architektuře ASP.NET MVC. Ukázkový program volání [rozhraní ASP.NET Web API](../../../web-api/index.md) metody, které pak volat [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) pro simulaci volání dlouhotrvající webové služby. Většinu provozních aplikací nezobrazí takové zřejmé výhody použití metody asynchronní akce.   
  
Několik aplikace vyžadují všechny metody akce byla asynchronní. Převod několik akcí synchronní metody na asynchronní metody často, poskytuje nejlepší účinnosti zvýšení množství práce potřebné.

## <a id="SampleApp"></a>  Ukázkové aplikace

Můžete stáhnout ukázkovou aplikaci z [ https://github.com/RickAndMSFT/Async-ASP.NET/ ](https://github.com/RickAndMSFT/Async-ASP.NET) na [Githubu](https://github.com/) lokality. Úložiště se skládá ze tří projektů:

- *Mvc4Async*: projekt ASP.NET MVC 4, který obsahuje kód použitý v tomto kurzu. Provede volání webového rozhraní API **WebAPIpgw** služby.
- *WebAPIpgw*: projekt Web API ASP.NET MVC 4, který implementuje `Products, Gizmos and Widgets` řadiče. Poskytuje data pro *WebAppAsync* projektu a *Mvc4Async* projektu.
- *WebAppAsync*: projekt webových formulářů ASP.NET používá v jiném kurzu.

## <a id="GizmosSynch"></a>  Metoda Gizma synchronní akce

 Následující kód ukazuje `Gizmos` metody synchronní akce, která se používá k zobrazení seznamu gizma. (Pro účely tohoto článku gizmo je fiktivní mechanickým zařízení.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample1.cs)]

Následující kód ukazuje `GetGizmos` metody gizmo služby.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample2.cs)]

`GizmoService GetGizmos` Metoda předává identifikátor URI služby technologie ASP.NET webové rozhraní API HTTP, který vrátí seznam hodnot gizma data. *WebAPIpgw* projekt obsahuje implementaci webového rozhraní API `gizmos, widget` a `product` řadiče.  
Následující obrázek zobrazuje gizma z ukázkového projektu.

![Gizma](using-asynchronous-methods-in-aspnet-mvc-4/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>  Vytváření Gizma asynchronní metody akce

Ukázka používá nový [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) a [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) klíčová slova (k dispozici v rozhraní .NET 4.5 a Visual Studio 2012) nechat kompilátor, zodpovídá za údržbu složitější transformace nezbytné pro asynchronní programování. Kompilátor umožňuje psát kód pomocí konstrukce jazyka C# pro synchronní řízení toku a kompilátor automaticky použije transformace, která je potřeba použít zpětná volání k zabránění blokování vlákna.

Následující kód ukazuje `Gizmos` synchronní metoda a `GizmosAsync` asynchronní metody. Pokud váš prohlížeč podporuje [HTML 5 `<mark>` element](http://www.w3.org/wiki/HTML/Elements/mark), uvidíte změny v `GizmosAsync` v žlutý zvýraznění.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample3.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample4.cs?highlight=1,3,5)]

 Následující změny byly použity na Povolit `GizmosAsync` byla asynchronní.

- Metoda je označena třídou [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo, které instruuje kompilátor generovat zpětná volání pro část textu a automaticky vytvářet `Task<ActionResult>` , která je vrácena.
- &quot;Asynchronní&quot; byl připojeným k názvu metody. Připojení "Async" se nevyžadují, ale je konvence při psaní asynchronních metod.
- Návratový typ se změnil z `ActionResult` k `Task<ActionResult>`. Návratový typ `Task<ActionResult>` představuje probíhající práci a poskytuje volající metody s popisovačem, prostřednictvím které se má počkat na dokončení asynchronní operace. V tomto případě se volající webové služby. `Task<ActionResult>` představuje probíhající práci s jako výsledek `ActionResult.`
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo byla použita k volání webové služby.
- Volala se asynchronní webového rozhraní API služby (`GetGizmosAsync`).

Uvnitř `GetGizmosAsync` body jiné asynchronní metody, metoda `GetGizmosAsync` je volána. `GetGizmosAsync` okamžitě se vrátí `Task<List<Gizmo>>` , který se nakonec dokončí když jsou data dostupná. Vzhledem k tomu, že nechcete dělat nic dalšího, dokud nebudete mít gizmo data, kód očekává úkol (pomocí **await** – klíčové slovo). Můžete použít **await** – klíčové slovo pouze v metodách opatřen poznámkou **asynchronní** – klíčové slovo.

**Await** – klíčové slovo neblokuje vlákno, dokud je úloha dokončena. Zaregistruje pro zbývající metody jako zpětné volání na úkolu a okamžitě se vrátí. Když nakonec dokončení očekávaného úkolu, bude vyvolání tohoto zpětného volání a tak pokračuje v provádění metody vpravo tam, kde skončila. Další informace o použití [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) obor názvů, najdete v článku [asynchronní odkazy](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Následující kód ukazuje `GetGizmos` a `GetGizmosAsync` metody.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample5.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample6.cs?highlight=1,4-8)]

 Asynchronní změny jsou podobné těm, které jsou provedeny **GizmosAsync** výše. 

- Podpis metody je opatřen poznámkou [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo, návratový typ se změnil na `Task<List<Gizmo>>`, a *asynchronní* byl připojeným k názvu metody.
- Asynchronní [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) třída se používá namísto [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) třídy.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo aplikováno [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) asynchronní metody.

Následující obrázek ukazuje asynchronní gizmo zobrazení.

![async](using-asynchronous-methods-in-aspnet-mvc-4/_static/image2.png)

Prezentace prohlížeče gizma dat je stejný jako zobrazení vytvořené pomocí synchronní volání. Jediným rozdílem je, že asynchronní verze může být výkonnější pod velkým zatížením.

## <a id="Parallel"></a>  Paralelní provádění více operací

Asynchronní akce metody mají výraznou výhodu nad synchronních metod při akci musíte provést několik nezávislých operací. V ukázce k dispozici, synchronní metoda `PWG`(pro produkty, Widgetů a Gizma) zobrazuje výsledky zobrazíte seznam produktů, widgetů a gizma tři volání webové služby. [Rozhraní ASP.NET Web API](../../../web-api/index.md) projekt, který poskytuje tyto služby používá [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) k simulaci latence pomalého volání. Pokud zpoždění nastavená na 500 milisekund, asynchronní `PWGasync` metoda trvá o něco více než 500 milisekund do dokončení při synchronní `PWG` verze převezme 1 500 milisekund. Synchronní `PWG` je znázorněna v následujícím kódu.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample7.cs)]

Asynchronní `PWGasync` je znázorněna v následujícím kódu.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample8.cs?highlight=1,3,12)]

Následující obrázek ukazuje zobrazení vrácených **PWGasync** metody.

![pwgAsync](using-asynchronous-methods-in-aspnet-mvc-4/_static/image3.png)

## <a id="CancelToken"></a>  Pomocí tokenu zrušení

Asynchronní akce metody vracející `Task<ActionResult>`jsou možné zrušit, které se přijaly [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametr, pokud je k dispozici s [hodnota vlastnosti AsyncTimeout](https://msdn.microsoft.com/library/system.web.mvc.asynctimeoutattribute(VS.108).aspx) atribut. Následující kód ukazuje `GizmosCancelAsync` metoda s časovým limitem 150 milisekund.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample9.cs?highlight=1-3,5,10)]

Následující kód ukazuje GetGizmosAsync přetížení, která přijímá [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametru.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-mvc-4/samples/sample10.cs)]

V ukázkové aplikaci k dispozici, vyberete *ukázku Token zrušení* propojit volání `GizmosCancelAsync` metoda a ukazuje, kterém jste předplatné zrušili asynchronního volání.

## <a id="ServerConfig"></a>  Konfigurace serveru pro volání webové služby vysokou souběžnosti/vysoká latence

Jak začít využívat výhod asynchronní webovou aplikaci, můžete potřebovat udělat nějaké změny ve výchozí konfiguraci serveru. Zachovat následující v úvahu při konfiguraci a zátěžové testování asynchronní webové aplikace.

- Windows klient operačních systémů Windows 7, Windows Vista a mít maximálně 10 souběžných požadavků. Je třeba operační systém Windows Server na jeho výhody asynchronních metod v případě velkého zatížení.
- Registrace rozhraní .NET 4.5 se službou IIS z příkazového řádku se zvýšenými oprávněními:  
  %windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet\_regiis -i  
  Zobrazit [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Možná budete muset zvýšit [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limit fronty z výchozí hodnoty 1 000 až 5 000. Pokud je nastavení příliš nízké, může se zobrazit [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) zamítnout žádosti se stavem HTTP 503. Chcete-li změnit HTTP.sys limit fronty:

    - Otevřete Správce služby IIS a přejděte do podokna fondy aplikací.
    - Klikněte pravým tlačítkem myši na cílový fond aplikací a vyberte **Upřesnit nastavení**.  
        ![Upřesnit](using-asynchronous-methods-in-aspnet-mvc-4/_static/image4.png)
    - V **Upřesnit nastavení** dialogovém okně Změnit *délka fronty* od 1 do 5 000 000.  
        ![Délka fronty](using-asynchronous-methods-in-aspnet-mvc-4/_static/image5.png)  
  
  Poznámka: výše uvedené, Image rozhraní .NET framework je uveden jako verzi 4.0, i když se fond aplikací používá rozhraní .NET 4.5. Informace o tom této nesrovnalosti, naleznete v následujících tématech:

    - [Správa verzí rozhraní .NET a cílení na více platforem - .NET 4.5 je místní upgrade na rozhraní .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
    - [Nastavení aplikace služby IIS nebo fondu aplikací, používat technologii ASP.NET 3.5 spíše než 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
    - [Verze rozhraní .NET framework a závislosti](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Pokud vaše aplikace používá webové služby nebo System.NET ke komunikaci s back-end pomocí protokolu HTTP budete možná muset zvýšit [connectionManagement/maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elementu. Pro aplikace ASP.NET je omezen pomocí funkce automatické konfigurace 12krát větší počet procesorů. To znamená, že na quad-proc, můžete mít nejvýše 12 \* 4 = 48 souběžných připojení na koncový bod IP. Protože to je vázán na [autoConfig](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), nejjednodušší způsob, jak zvýšit `maxconnection` v ASP.NET, aplikace je nastavit [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) programově v z `Application_Start` metodu *global.asax* souboru. Najdete v ukázce stahovat pro příklad.
- V rozhraní .NET 4.5, výchozí 5000 pro [maxconcurrentrequestspercpu technologie](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) by měla být v pořádku.
