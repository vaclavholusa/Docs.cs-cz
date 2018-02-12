---
uid: web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
title: "Použití asynchronních metod v technologii ASP.NET 4.5 | Microsoft Docs"
author: Rick-Anderson
description: "V tomto kurzu naučit se základy vytváření asynchronní aplikace webových formulářů ASP.NET pomocí Visual Studio Express 2012 pro Web, který je bezplatný..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/06/2012
ms.topic: article
ms.assetid: a585c9a2-7c8e-478b-9706-90f3739c50d1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/performance-and-caching/using-asynchronous-methods-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: d3eb588aad592605a8e368d1af6e62ece34b79d0
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
---
<a name="using-asynchronous-methods-in-aspnet-45"></a>Použití asynchronních metod v technologii ASP.NET 4.5
====================
Podle [Rick Anderson](https://github.com/Rick-Anderson)

> V tomto kurzu se naučit se základy vytváření, asynchronní aplikace webových formulářů ASP.NET pomocí [Visual Studio Express 2012 pro Web](https://www.microsoft.com/visualstudio/11), což je bezplatnou verzi sady Microsoft Visual Studio. Můžete také použít [Visual Studio 2012](https://www.microsoft.com/visualstudio/11). V následujících částech jsou zahrnuty v tomto kurzu.
> 
> - [Zpracování požadavků ve fondu vláken](#HowRequestsProcessedByTP)
> - [Výběr synchronní nebo asynchronní metody](#ChoosingSyncVasync)
> - [Ukázkové aplikace](#SampleApp)
> - [Synchronní stránce si](#GizmosSynch)
> - [Vytvoření stránky asynchronní si](#CreatingAsynchGizmos)
> - [Paralelní provádění více operací](#Parallel)
> - [Pomocí Token zrušení](#CancelToken)
> - [Konfigurace serveru pro volání vysoké souběžnosti vysoký latence webové služby](#ServerConfig)
> 
> V tomto kurzu se poskytuje ucelenou ukázku  
>  [https://github.com/RickAndMSFT/Async-ASP.NET/](https://github.com/RickAndMSFT/Async-ASP.NET/) na [Githubu](https://github.com/) lokality.


ASP.NET 4.5 webové stránky v kombinaci [.NET 4.5](https://msdn.microsoft.com/library/w0x726c2(VS.110).aspx) můžete registrovat asynchronní metody, které vracejí objekt typu [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Asynchronní programování koncept, označuje jako zavedená rozhraní .NET Framework 4 [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) a podporuje technologii ASP.NET 4.5 [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx). Úlohy jsou reprezentované pomocí **úloh** typu a souvisejících typů v [System.Threading.Tasks](https://msdn.microsoft.com/library/system.threading.tasks.aspx) oboru názvů. Rozhraní .NET Framework 4.5 je založený na tato asynchronní podpora s [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova, která usnadnění práce s [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) objekty mnohem méně složitější než předchozí asynchronní přístupy. [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo je syntaktické sdružená označující, kterou část kódu by měla asynchronně čekat na další část kódu. [Asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo představuje pomocný parametr, který slouží k označení metody jako asynchronní metody založené na úlohách. Kombinace **await**, **asynchronní**a **úloh** objektu je mnohem jednodušší pro psaní asynchronní kódu v rozhraní .NET 4.5. Nový model pro asynchronní metody je volána *Task-based Asynchronous Pattern* (**klepněte na**). V tomto kurzu se předpokládá, máte určitou znalost asynchronní pomocí programing [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) oboru názvů.

Další informace o použití [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) obor názvů, naleznete v následujících odkazech.

- [Dokument White Paper: Asynchrony v rozhraní .NET](https://go.microsoft.com/fwlink/?LinkId=204844)
- [Asynchronní/Await – nejčastější dotazy](https://blogs.msdn.com/b/pfxteam/archive/2012/04/12/10293335.aspx)
- [Asynchronní programování v sadě Visual Studio](https://msdn.microsoft.com/vstudio/gg316360)

## <a id="HowRequestsProcessedByTP"></a>Zpracování požadavků ve fondu vláken

Rozhraní .NET Framework na webový server udržuje fond vláken, které se používají ke zpracování požadavků ASP.NET. Pokud dorazí požadavek, je odeslána vlákno z fondu ke zpracování tohoto požadavku. Pokud je požadavek zpracován synchronně, vláken, který zpracuje žádost je zaneprázdněn při žádost je zpracovávána a že vlákno nemůže obsloužit další požadavek.   
  
Toto nemusí být problém, protože fond vláken můžete provedeny dostatečně velké na to, aby dokázala pojmout mnoho zaneprázdněn vláken. Počet vláken ve fondu vláken je však omezená (pro .NET 4.5 maximální výchozí hodnota je 5 000). V velké aplikace s vysokou souběžnosti dlouho běžící požadavků může být zaneprázdněn všech dostupných vláken. Tento stav se označuje jako přístup z více vláken nedostatku prostředků. Když je dosaženo této podmínky, webový server fronty požadavků. Pokud zaplnění fronty požadavků, webový server odmítá požadavky se stavem HTTP 503 (Server je přetížen). Fond vláken CLR má omezení na nové vlákno injekce. Pokud je shlukovým přenosem souběžnosti (to znamená, že váš web z umístění můžete najednou získat velkého množství požadavků) a všechna vlákna požadavek k dispozici jsou zaneprázdněn z důvodu back-end volání s vysokou latencí, míra vkládání omezený přístup z více vláken můžete provést velmi špatně reakce aplikace. Kromě toho má každý nové vlákno přidané do fondu vláken režijní náklady na (například 1 MB paměti zásobníku). Webové aplikace pomocí synchronních metod k volání služby vysokou latencí, kde fondu vláken zvětšuje, aby výchozí rozhraní .NET 4.5 maximálně 5 000 vláken by využívat přibližně 5 GB více paměti, než je možné aplikaci na stejné žádosti o službu pomocí asynchronní metody a jenom 50 vláken. Pokud jste to asynchronní pracovní, ne vždy používáte vlákna. Například při provádění požadavku asynchronní webové služby ASP.NET nebudete používat žádné podprocesy mezi **asynchronní** volání metody a **await**. Použití fondu vláken pro žádosti o služby s vysokou latencí může vést k velké paměti nároky a nízký využití hardwaru serveru.

## <a name="processing-asynchronous-requests"></a>Asynchronní zpracování požadavků

Ve webové aplikace, které najdete v části velký počet souběžných požadavků na spuštění nebo má shlukovým přenosem zatížení (kde souběžnosti najednou zvyšuje) provedení asynchronní volání webové služby se zvyšuje rychlost reakce aplikace. Asynchronní požadavek trvá stejné množství času na zpracování jako synchronní požadavek. Například pokud žádost o provede webové služby volání, které vyžaduje dva sekund dokončení požadavku trvá dvou sekund zda probíhá synchronně nebo asynchronně. Ale při asynchronním volání, není vlákno blokováno reagovat na požadavky na jiné během čekání na dokončení první žádosti. Proto asynchronní požadavky zabránit růstu žádosti o služby Řízení front a vlákno fondu při mnoho souběžných požadavků, které vyvolají dlouhotrvající operace.

## <a id="ChoosingSyncVasync"></a>Výběr synchronní nebo asynchronní metody

Tato část obsahuje pokyny pro použití synchronní nebo asynchronní metody. Jsou to jenom pokyny; Zkontrolujte každou aplikaci zvlášť k určení, zda asynchronní metody pomoci s výkonem.

Obecně platí používejte synchronní metody byly splněny následující podmínky:

- Operace jsou jednoduché nebo krátké spuštěná.
- Jednoduchost je důležitější než účinnost.
- Operace, které jsou primárně procesoru operace místo operace, které zahrnují rozsáhlé disk nebo nároky na síť. Použití asynchronních metod na operace vázané na procesor poskytuje žádné výhody a další režie výsledkem.

 Obecně platí používejte asynchronní metody byly splněny následující podmínky:

- Jste volání služby, které mohou být využívány prostřednictvím asynchronních metod a používáte rozhraní .NET 4.5 nebo vyšší.
- Operace, které jsou vázané na síti nebo I/čítači místo vázané na procesor.
- Paralelismus je důležitější než jednoduchost kódu.
- Chcete zadat mechanismus, který umožňuje uživatelům zrušit žádost o časově náročné.
- Výhodou přepínání vláken se provede při náklady na kontext přepínače. Obecně platí měl by metodu asynchronní Pokud synchronní metoda blokuje přitom žádné pracovní vlákno žádost ASP.NET. Tím, že volání asynchronní, není vlákno žádost ASP.NET blokované, provádění žádné pracovní, kdy čeká k dokončení žádosti webové služby.
- Testování ukazuje, že blokování operace jsou úzkým místem v výkonu webu a že služby IIS můžete další žádosti o služby s použitím pro tyto blokování volání asynchronních metod.

 Ke stažení ukázkové ukazuje, jak efektivně použít asynchronní metody. Ukázka poskytuje byla určená k poskytnutí jednoduché ukázka asynchronního programování v technologii ASP.NET 4.5. Ukázka neměla být referenční architektura pro asynchronní programování v technologii ASP.NET. Ukázka programu volání [rozhraní ASP.NET Web API](../../../web-api/index.md) metody, které pak volání [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) k simulaci volání dlouho běžící webové služby. Většina aplikací produkční nezobrazí takové zřejmé výhody použití asynchronních metod.   
  
Několik aplikace vyžadují všechny metody jako asynchronní. Převádění několik synchronních metod pro asynchronní metody často poskytuje nejlepší zvýšení efektivity pro množství práce potřebné.

## <a id="SampleApp"></a>Ukázkové aplikace

Si můžete stáhnout ukázkovou aplikaci z [https://github.com/RickAndMSFT/Async-ASP.NET](https://github.com/RickAndMSFT/Async-ASP.NET) na [Githubu](https://github.com/) lokality. Úložiště se skládá ze tří projektů:

- *WebAppAsync*: webových formulářů ASP.NET projekt, který využívá rozhraní Web API **WebAPIpwg** služby. Většinu kódu pro tento kurz je z tohoto projektu.
- *WebAPIpgw*: projekt ASP.NET MVC 4 Web API, který implementuje `Products, Gizmos and Widgets` řadiče. Poskytuje data pro *WebAppAsync* projektu a *Mvc4Async* projektu.
- *Mvc4Async*: ASP.NET MVC 4 projekt, který obsahuje kód použitý v jiné kurzu. Umožňuje volání webového rozhraní API **WebAPIpwg** služby.

## <a id="GizmosSynch"></a>Synchronní stránce si

 Následující kód ukazuje `Page_Load` synchronní metoda, která se používá k zobrazení seznamu si. (V tomto článku gizmo je fiktivních mechanických zařízení.) 

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample1.cs)]

Následující kód ukazuje `GetGizmos` metoda gizmo služby.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample2.cs)]

`GizmoService GetGizmos` Metoda předá identifikátoru URI služby ASP.NET Web API HTTP, který vrátí seznam hodnot si data. *WebAPIpgw* projekt obsahuje implementace rozhraní Web API `gizmos, widget` a `product` řadiče.  
Následující obrázek znázorňuje si stránku z ukázkového projektu.

![Si](using-asynchronous-methods-in-aspnet-45/_static/image1.png)

## <a id="CreatingAsynchGizmos"></a>Vytvoření stránky asynchronní si

Ukázka používá nový [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) a [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) klíčová slova (k dispozici v rozhraní .NET 4.5 a Visual Studio 2012) chcete, aby služba je zodpovědná za údržbu složité transformace potřebné pro kompilátor asynchronní programování. Kompilátor umožňuje psát kód, který vytvoří jazyka C# na synchronní řízení toku pomocí a kompilátor automaticky použije transformace nezbytných k používání zpětných volání k zabránění blokování vláken.

Musí zahrnovat asynchronní stránky ASP.NET [stránky](https://msdn.microsoft.com/library/ydy4x04a.aspx) direktivy s `Async` nastaven atribut na hodnotu "true". Následující kód ukazuje [stránky](https://msdn.microsoft.com/library/ydy4x04a.aspx) direktivy s `Async` nastaven na hodnotu "true" pro atribut *GizmosAsync.aspx* stránky.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample3.aspx?highlight=1)]

Následující kód ukazuje `Gizmos` synchronní `Page_Load` metoda a `GizmosAsync` asynchronní stránky. Pokud váš prohlížeč podporuje [standardu HTML 5 &lt;označit&gt; element](http://www.w3.org/wiki/HTML/Elements/mark), uvidíte změny v `GizmosAsync` v žlutý zvýraznění.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample4.cs)]

Asynchronní verze:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample5.cs?highlight=3,6-7,9,11)]

 Aby byly použity následující změny `GizmosAsync` stránky se asynchronní.

- [Stránky](https://msdn.microsoft.com/library/ydy4x04a.aspx) – direktiva musí mít `Async` nastaven atribut na hodnotu "true".
- `RegisterAsyncTask` Metoda se používá k registraci asynchronní úlohu obsahující kód, který se spouští asynchronně.
- Nové `GetGizmosSvcAsync` metoda je označena [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo, které sděluje kompilátoru generování zpětných volání pro části textu a automaticky vytvářet `Task` , je vrácena.
- &quot;Asynchronní&quot; byl připojeným k názvu asynchronní metody. Připojování "Asynchronní" není povinný, ale je konvence při psaní asynchronních metod.
- Návratový typ nové `GetGizmosSvcAsync` je metoda `Task`. Návratový typ `Task` reprezentuje probíhající práce a poskytuje volající metody s popisovačem, pomocí kterého je možné čekání na dokončení asynchronní operace.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo, které bylo použito pro volání webové služby.
- Volala se asynchronní webového rozhraní API služby (`GetGizmosAsync`).

Uvnitř `GetGizmosSvcAsync` metoda body jinou asynchronní metodu `GetGizmosAsync` je volána. `GetGizmosAsync`Vrátí okamžitě `Task<List<Gizmo>>` , nakonec dokončí když jsou data k dispozici. Vzhledem k tomu, že nechcete dělat žádné další kroky, dokud se gizmo data, kód čeká úlohy (pomocí **await** – klíčové slovo). Můžete použít **await** – klíčové slovo pouze v metodách opatřen poznámkou **asynchronní** – klíčové slovo.

**Await** – klíčové slovo neblokuje vlákno až do dokončení úlohy. Zaregistruje zbytek metodu jako zpětné volání v úloze a vrátí okamžitě. Pokud úlohu awaited nakonec dokončí, bude vyvolání že zpětné volání a proto pokračovat v provádění právo metoda, kde bylo přerušeno. Další informace o používání [await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) a [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) klíčová slova a [úloh](https://msdn.microsoft.com/library/system.threading.tasks.task.aspx) obor názvů, najdete v článku [asynchronní odkazy](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/async).

Následující kód ukazuje `GetGizmos` a `GetGizmosAsync` metody.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample6.cs)]

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample7.cs?highlight=1,4-8)]

 Asynchronní změny jsou podobné těm, které jsou provedeny **GizmosAsync** výše. 

- Podpis metody byl opatřen poznámkou [asynchronní](https://msdn.microsoft.com/library/hh156513(VS.110).aspx) – klíčové slovo, návratový typ byl změněn na `Task<List<Gizmo>>`, a *asynchronní* se připojí k název metody.
- Asynchronní [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx) třída se používá namísto synchronní [WebClient](https://msdn.microsoft.com/library/system.net.webclient.aspx) třídy.
- [Await](https://msdn.microsoft.com/library/hh156528(VS.110).aspx) – klíčové slovo, které bylo použito pro [HttpClient](https://msdn.microsoft.com/library/system.net.http.httpclient(VS.110).aspx)[GetAsync](https://msdn.microsoft.com/library/hh158944(VS.110).aspx) asynchronní metody.

Následující obrázek znázorňuje asynchronní gizmo zobrazení.

![async](using-asynchronous-methods-in-aspnet-45/_static/image2.png)

Prezentace prohlížeče si dat je stejný jako zobrazení vytvořené synchronní volání. Jediným rozdílem je, že asynchronní verzi může být více původce přetížena.

## <a name="registerasynctask-notes"></a>RegisterAsyncTask Notes

Metody připojili s `RegisterAsyncTask` se spustí hned po [PreRender](https://msdn.microsoft.com/library/ms178472.aspx). Asynchronní void stránky událostí můžete taky použít přímo, jak je znázorněno v následujícím kódu:

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample8.cs)]

Nevýhodou asynchronní void události je, že vývojáři už má plnou kontrolu nad při spuštění události. Například pokud obě aspx a nabídku. Definujte hlavní `Page_Load` události a jedna nebo obě z nich jsou asynchronní, nemůže zaručit pořadí zpracování. Stejné pořadí indeterminiate pro obslužné rutiny událostí jiné (například `async void Button_Click` ) se vztahuje. Pro většinu vývojáře, měl by být přijatelné, ale ty, kteří vyžadují úplnou kontrolu nad pořadí provádění měli používat jenom rozhraní API jako `RegisterAsyncTask` , využívat metody, které vrací objekt úlohy.

## <a id="Parallel"></a>Paralelní provádění více operací

Asynchronní metody mít významné výhody přes synchronních metod, při akci musíte provést několik nezávislých operací. V ukázce k dispozici, stránce synchronní *PWG.aspx*(pro produkty, pomůcek a si) se zobrazí výsledky získáte seznam produktů, pomůcek a si tři volání webové služby. [Rozhraní ASP.NET Web API](../../../web-api/index.md) projekt, který poskytuje tyto služby používá [Task.Delay](https://msdn.microsoft.com/library/hh139096(VS.110).aspx) k simulaci latenci nebo pomalé síťové volání. Když je zpoždění nastavená na 500 milisekund asynchronní *PWGasync.aspx* stránky trvá trochu delší než 500 ms do dokončení při synchronní `PWG` verze převezme 1 500 milisekundách. Synchronní *PWG.aspx* stránky je znázorněno v následujícím kódu.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample9.cs)]

Asynchronní `PWGasync` kódu na pozadí jsou uvedeny níže.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample10.cs?highlight=5,11,21)]

Následující obrázek znázorňuje zobrazení vrácených asynchronní *PWGasync.aspx* stránky.

![](using-asynchronous-methods-in-aspnet-45/_static/image3.png)

## <a id="CancelToken"></a>Pomocí Token zrušení

Asynchronní metody vrací `Task`jsou možné zrušit, která je jejich trvat [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken(VS.110).aspx) parametr, pokud je k dispozici s `AsyncTimeout` atribut [stránky](https://msdn.microsoft.com/library/ydy4x04a.aspx) – direktiva. Následující kód ukazuje *GizmosCancelAsync.aspx* stránka s časový limit na druhý.

[!code-aspx[Main](using-asynchronous-methods-in-aspnet-45/samples/sample11.aspx?highlight=1)]

Následující kód ukazuje *GizmosCancelAsync.aspx.cs* souboru.

[!code-csharp[Main](using-asynchronous-methods-in-aspnet-45/samples/sample12.cs?highlight=6,9)]

V ukázkové aplikaci zadané, výběr *GizmosCancelAsync* odkaz volání *GizmosCancelAsync.aspx* stránky a předvádí zrušení asynchronního volání (podle vypršení časového limitu). Doba zpoždění je náhodný rozsahu, vám může být nutné aktualizovat stránku několikrát k získání chybové zprávy vypršení časového limitu.

## <a id="ServerConfig"></a>Konfigurace serveru pro volání vysoké souběžnosti vysoký latence webové služby

Pochopit výhody asynchronní webové aplikace, může být nutné provést některé změny konfigurace serveru výchozí. Mějte na paměti při konfiguraci a zátěžové testování asynchronní webové aplikace.

- Windows 7, Windows Vista, Windows 8 a všechny klientské operační systémy Windows mít maximálně 10 souběžných požadavků. Budete potřebovat operační systém Windows Server zobrazíte výhod asynchronních metod vysoké zatížení.
- Službu IIS zaregistrujte .NET 4.5 z příkazového řádku se zvýšenými pomocí následujícího příkazu:  
 %windir%\Microsoft.NET\Framework64 \v4.0.30319\aspnet\_regiis -i  
 V tématu [ASP.NET IIS Registration Tool (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h.aspx)
- Možná budete muset zvýšit [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) limit fronty z výchozí hodnotu 1000 až 5 000. Pokud toto nastavení je příliš nízké, mohou se zobrazit [HTTP.sys](https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture) zamítal požadavky, se stavem HTTP 503. Chcete-li změnit limit fronty HTTP.sys:

    - Otevřete Správce služby IIS a přejděte do podokna fondů aplikací.
    - Klikněte pravým tlačítkem na cílový fond aplikací a vyberte **Upřesnit nastavení**.  
        ![advanced](using-asynchronous-methods-in-aspnet-45/_static/image4.png)
    - V **Upřesnit nastavení** dialogové okno, změna *délka fronty* od 1 do 5 000 000.  
        ![Délka fronty](using-asynchronous-methods-in-aspnet-45/_static/image5.png)  
  
 Poznámka: v obrázcích výše, rozhraní .NET framework je uveden jako v4.0, i když je fond aplikací pomocí rozhraní .NET 4.5. Tato nesrovnalost pochopit, naleznete v následujících tématech:

        - [.NET Versioning and Multi-Targeting - .NET 4.5 is an in-place upgrade to .NET 4.0](http://www.hanselman.com/blog/NETVersioningAndMultiTargetingNET45IsAnInplaceUpgradeToNET40.aspx)
        - [How to set an IIS Application or AppPool to use ASP.NET 3.5 rather than 2.0](http://www.hanselman.com/blog/HowToSetAnIISApplicationOrAppPoolToUseASPNET35RatherThan20.aspx)
        - [.NET Framework Versions and Dependencies](https://msdn.microsoft.com/library/bb822049(VS.110).aspx)
- Pokud vaše aplikace používá webové služby nebo System.NET ke komunikaci s back-end pomocí protokolu HTTP budete možná muset zvýšit [connectionManagement – / maxconnection](https://msdn.microsoft.com/library/fb6y0fyc(VS.110).aspx) elementu. Pro aplikace ASP.NET to je omezena funkci automatické konfigurace 12krát většího počtu procesorů. To znamená, že na quad-proc, můžete mít maximálně 12 \* 4 = 48 souběžných připojení ke koncovému bodu IP. Protože to je vázaný na [automatické konfigurace](https://msdn.microsoft.com/library/7w2sway1(VS.110).aspx), nejjednodušší způsob, jak zvýšit `maxconnection` v technologie ASP.NET je aplikace nastavit [System.Net.ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit(VS.110).aspx) v prostřednictvím kódu programu z `Application_Start` metoda v *global.asax* souboru. Viz ukázka stáhnout příklad.
- V rozhraní .NET 4.5, výchozí hodnotu 5000 pro [MaxConcurrentRequestsPerCPU](https://blogs.msdn.com/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx) by byl v pořádku.

## <a name="contributors"></a>Contributors

- [Levi Broderick](http://stackoverflow.com/users/59641/levi)
- [Tom Dykstra](http://www.bing.com/search?q=site%3Aasp.net+%22Tom+Dykstra%22+-forums.asp.net&amp;qs=n&amp;form=QBRE&amp;pq=site%3Aasp.net+%22tom+dykstra%22+-forums.asp.net&amp;sc=8-42&amp;sp=-1&amp;sk=)
- [Brad Wilson](http://bradwilson.typepad.com/)
- [HongMei Ge](https://blogs.msdn.com/b/hongmeig/)
