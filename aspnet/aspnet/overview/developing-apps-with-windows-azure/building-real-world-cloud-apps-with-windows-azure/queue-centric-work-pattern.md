---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Vzor založený na frontě práce (vytváření skutečných cloudových aplikací s Azure) | Dokumentace Microsoftu
author: MikeWasson
description: Vytváření reálného světa cloudových aplikací s Azure e kniha je založená na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které se dají mu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: fd8f9165de333d22bed0001bb932d7bab8430672
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370255"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Vzor založený na frontě práce (vytváření skutečných cloudových aplikací s Azure)
====================
podle [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Petr Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronickou knihu](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** e knihy je založena na prezentaci vypracovanou organizací cccppf Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám pomůžou být úspěšný vývoj webových aplikací v cloudu. Informace o e kniha najdete v tématu [první kapitoly](introduction.md).


Dříve jsme viděli, že pomocí několika služeb může způsobit "složený" smlouvou SLA, kde je aplikace platné smlouvy SLA *produktu* jednotlivých smlouvách SLA. Například aplikace Fix It používá webové servery, úložiště a SQL Database. Pokud selže některý z těchto služeb aplikaci vrátí chybu uživateli.

Ukládání do mezipaměti je dobrým způsobem, jak řešit přechodná selhání pro obsah jen pro čtení. Ale co když vaše aplikace potřebuje k práci? Například když uživatel odešle nový úkol Fix It, aplikace nejde stačí umístit úlohy do mezipaměti. Aplikace musí zapisovat úloha opravit do do trvalého úložiště dat, takže je možné je zpracovat.

Je to, kde vzor založený na frontě práci dodává. Tento model umožňuje volné párování mezi webovou vrstvu a back-end službu.

Zde je, jak funguje vzor. Když aplikace načte žádost, vloží pracovní položky do fronty a okamžitě se vrátí odpověď. Samostatné back-endový proces pak načte pracovní položky z fronty a provádí.

Pracovní postup založený na frontě model je vhodný pro:

- Práce, která je časově náročné (vysoká latence).
- Práce, která vyžaduje externí služby, která nemusí být k dispozici.
- Práce, který je náročné (vysoké využití procesoru).
- Práce, která je výhodná míra vyrovnání (v souladu s nárůstem zatížení i s náhlými).

## <a name="reduced-latency"></a>Nižší latence

Fronty jsou užitečné pokaždé, když provádíte časově náročné práce. Pokud úloha trvá několik sekund nebo delší, místo blokuje koncovému uživateli, vložit do fronty pracovní položku. Informace pro uživatele "Pořád pracujeme na něm" a potom použití naslouchací proces fronty ke zpracování úloh na pozadí.

Například když zakoupíte na online prodejce, na webu společnosti potvrdí vaši objednávku okamžitě. Ale to neznamená, že váš obsah je již v truck distribuována. Úlohu se umístit do fronty, a na pozadí jsou to kredibility Příprava položek pro přesouvání a tak dále.

Pro scénáře s krátkou latencí může být celkový čas začátku do konce delší použití fronty jako ve srovnání s synchronně provádění úkolu. Ale i pak, můžete další výhody převáží nad tuto nevýhodou.

## <a name="increased-reliability"></a>Zvýšení spolehlivosti

Ve verzi Fix It, který jste se díváme na zatím webového front-endu těsně spjat s back endovou databází SQL. Pokud službu SQL database k dispozici, uživatel obdrží chybu. Pokud nefunguje opakovaných pokusů (to znamená, je více než přechodné selhání) je jediné, co můžete dělat zobrazit chybu a požádat uživatele, a zkuste to znovu později.

![Diagram znázorňující webový front-end selhání back endovou databází SQL v případě selhání](queue-centric-work-pattern/_static/image1.png)

Použití front, když uživatel odešle úlohu Fix It, aplikace zapíše zprávu do fronty. Je datovou část zprávy [JSON](http://json.org/) reprezentace úlohy. Jakmile zapíše se do fronty, vrátí aplikace a okamžitě zobrazí uživateli zprávu o úspěšném dokončení.

Pokud některý z back-endové služby – jako je SQL database nebo fronty naslouchací proces – přejdou do režimu offline, mohou uživatelé stále odeslat novou Fix It úkolů. Zprávy pouze fronty až do back-endových služeb jsou k dispozici znovu. Back-endových služeb se od tohoto okamžiku se dohnat v nevyřízených položkách.

![Diagram znázorňující webového front-endu nadále fungovat v případě, že dochází k chybě SQL Database](queue-centric-work-pattern/_static/image2.png)

Kromě toho teď můžete přidat další logiku back-end bez starostí o odolnosti front-endu. Například můžete odesílat e-mailu nebo SMS zprávy o vlastníkovi pokaždé, když je přiřazen nové Fix It. Pokud e-mailu nebo služba SMS přestane být dostupný, můžete zpracovávat všechno ostatní a potom vložit zprávu do samostatné fronty pro odesílání e-mailu nebo SMS zprávy.

SLA efektivní byl předtím Web Apps &times; úložiště &times; SQL Database = 99,7 %. (Viz [návrh odolný proti chybám](design-to-survive-failures.md).)

Když se změní app používáte frontu, webového front-endu závisí jenom na webové aplikace a úložiště, pro složené SLA 99,8 %. (Všimněte si, že fronty jsou součástí služby Azure storage, budou obsaženy ve stejné SLA jako na úložiště objektů blob.)

Pokud budete potřebovat ještě lepší než 99,8 %, můžete vytvořit dvě fronty ve dvou různých oblastech. Určete jeden jako primární a druhý jako sekundární. Ve vaší aplikaci převzetí služeb při selhání do sekundární fronty. Pokud se primární fronta není k dispozici. Možnost je k dispozici ve stejnou dobu je velmi malý.

## <a name="rate-leveling-and-independent-scaling"></a>Míra vyrovnávání a nezávislé škálování

Fronty jsou také užitečné se nazývá *ohodnotit vyrovnání* nebo *vyrovnávání zátěže*.

Webové aplikace jsou často náchylné k náhlým nárůstům provozu. Při použití automatického škálování můžete automaticky přidávat webové servery pro zpracování zvýšené webového provozu, automatické škálování nemusí být schopné reagovat dostatečně rychle k náhlému poraďte se špičkami zatížení. Pokud webové servery může převzít některé činnosti, které se mají provést napsáním zprávu do fronty, které zpracovávají další provoz. Back-end službu můžete číst zprávy z fronty a jejich zpracování. Hloubka fronty bude zvětšovat nebo zmenšovat podle příchozí zátěží se mění.

S mnohem časově náročné práce úlovek přeložen na back-end službu můžete snadněji webové vrstvy reagovat k nečekaným špičkám provozu. A ušetřit peníze, protože jakýkoli daný objem provozu může být zpracována méně webových serverů.

Můžete nezávisle škálovat webovou vrstvu a back-end službu. Například můžete potřebovat tří webových serverů, ale pouze jeden server zpracování fronty zpráv. Nebo pokud spouštíte úlohy náročné na výpočetní prostředky na pozadí, bude pravděpodobně nutné další back-end serverů.

![](queue-centric-work-pattern/_static/image3.png)

Automatické škálování funguje s back-endových služeb můžou mít webové vrstvy. Můžete vertikálně navýšit kapacitu nebo vertikálně snížit kapacitu počtu virtuálních počítačů, které jsou zpracování úloh ve frontě a na základě využití procesoru z back-endový virtuální počítače. Nebo můžete automaticky škálovat podle toho, kolik položek se do fronty. Například můžete zjistit, automatické škálování se pokouší uchovávat maximálně 10 položek ve frontě. Pokud fronta má více než 10 položek, přidá automatické škálování virtuálních počítačů. Když se dozvíte, automatické škálování se dovolí nadbytečné virtuální počítače.

## <a name="adding-queues-to-the-fix-it-application"></a>Přidání zařadí do fronty pro opravu se aplikace

Pro implementaci vzoru fronty, musíme provést dva změny do aplikace Fix It.

- Když uživatel odešle nový úkol Fix It, úloha má zařadit do fronty, namísto zapisuje do databáze.
- Vytvoření back endové služby, který zpracovává zprávy ve frontě.

Do fronty, použijeme [služby Azure Queue Storage](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Další možností je použít [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Rozhodněte, které služby front, zvažte, jak vaše aplikace musí posílat a přijímat zprávy ve frontě:

- Pokud máte spolupracující výrobci a konkurenčních spotřebitelů, zvažte použití služby Azure Queue Storage. "Spolupracující producenti" znamená, že více procesů se přidání zprávy do fronty. "Modelu konkurenčních příjemců" znamená více procesů se stahují se zprávy z fronty pro jejich zpracování, ale jakékoli dané zprávy může zpracovat pouze jednoho "příjemce." Pokud potřebujete větší propustnost než můžete začít s jednou frontou, použijte další fronty a/nebo dalších účtů úložiště.
- Pokud potřebujete [publikování/odběr modelu](http://en.wikipedia.org/wiki/Publish/subscribe), zvažte použití fronty Azure Service Bus.

Aplikace Fix It vyhovovat spolupracující výrobce a model konkurenčních spotřebitelů.

Je potřeba zamyslet se dostupnost aplikace. Služba fronty úložiště je součástí stejnou službu, kterou používáme pro úložiště objektů blob, takže jeho použití nemá žádný vliv na naše smlouva SLA. Azure Service Bus je samostatná služba s vlastní smlouvu SLA. Použili fronty služby Service Bus, budeme něco muset zvážit další procentní smlouvy SLA a našich Složená hodnota SLA bude nižší. Služba front, abyste dokázali vybrat, ujistěte se, že rozumíte jejímu dopadu podle vašeho výběru na dostupnost aplikace. Další informace najdete v tématu [prostředky](#resources) oddílu.

## <a name="creating-queue-messages"></a>Vytvoření zprávy fronty

Pokud chcete změnit úlohu oprava ve frontě, webového front-endu provede následující kroky:

1. Vytvoření [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instance. `CloudQueueClient` Instance se používá k provádění požadavků služby front.
2. Vytvořte frontu, pokud ještě neexistuje.
3. Úloha opravit Serializujte.
4. Volání [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) vložit zprávu do fronty.

Provedeme tuto práci v konstruktoru a `SendMessageAsync` metoda nové `FixItQueueManager` třídy.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Tady se používá [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) knihovny k serializaci automatickou do formátu JSON. Můžete použít jakýkoli přístup serializace dáváte přednost. Výhodou čitelný, přičemž je míň podrobné než XML má kód JSON.

Produkční kvality kódu by přidat logiku zpracování chyb, pozastavit, pokud databáze začal být nedostupný, zpracování obnovení čistěji, vytvořte frontu na spuštění aplikace a spravovat "[nezpracovatelných" zprávy](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Nezpracovatelná zpráva se zpráva, která z nějakého důvodu nelze zpracovat. Nechcete nezpracovatelných zpráv se vám sedět ve frontě, kde role pracovního procesu se průběžně pokusí zpracovat, nezdaří, zkuste to znovu, selhání a tak dále.)

Ve front-endové aplikace MVC musíme aktualizovat kód, který vytvoří nový úkol. Namísto vložení hodnoty úlohy do úložiště, zavolejte `SendMessageAsync` metody uvedené výše.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Zpracování zpráv fronty

Ke zpracování zpráv ve frontě, vytvoříme back-end službu. Back-end služba se spustí nekonečnou smyčku, která provede následující kroky:

1. Získáte další zprávy z fronty.
2. Deserializaci zprávy k úloze Fix It.
3. Úloha opravit zapisovat do databáze.

K hostování back-end službu Azure Cloud Service, která obsahuje vytvoříme *role pracovního procesu*. Role pracovního procesu se skládá z jednoho nebo více virtuálních počítačů, které můžou provádět zpracování back-endem. Kód, který běží v těchto virtuálních počítačů přetáhne zprávy z fronty, jakmile budou k dispozici. Pro každou zprávu vytvoříme datovou část JSON deserializovat a instanci entity, opravte ji úloh zapisovat do databáze, používat stejné úložiště, kterou jste použili dříve ve webové vrstvě.

Následující kroky ukazují, jak přidat pracovní roli projektu do řešení, která má standardní webového projektu. Tyto kroky již byla v Fix It projekt, který si můžete stáhnout udělali.

Nejprve přidáte projekt cloudové služby do řešení sady Visual Studio. Klikněte pravým tlačítkem na řešení a vyberte **přidat**, pak **nový projekt**. V levém podokně rozbalte **Visual C#** a vyberte **cloudu**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

V **novou Cloudovou službu Azure** dialogového okna, rozbalte **Visual C#** uzlu v levém podokně. Vyberte **Role pracovního procesu** a klikněte na ikonu se šipkou vpravo.

![](queue-centric-work-pattern/_static/image6.png)

(Všimněte si, že můžete také přidat *webová role*. Spouštět Fix It front-endu v rámci stejné cloudové služby namísto jeho spuštění ve webech Azure. Který má několik výhod v usnadnění připojení mezi front-endu a back endu ke koordinaci. Ale pro zjednodušení této ukázce jsme uchovávání front-endu v Azure App Service Web App a jenom spuštěné back-end v cloudové službě.)

Výchozí název je přiřazen k roli pracovního procesu. Chcete-li změnit název, najeďte myší role pracovního procesu v pravém podokně a pak klikněte na ikonu tužky.

![](queue-centric-work-pattern/_static/image7.png)

Klikněte na tlačítko **OK** dokončete dialogového okna. Tento postup přidá dva projekty do řešení sady Visual Studio.

- Projekt Azure, který definuje cloudové služby, včetně informací o konfiguraci.
- Projekt role pracovního procesu, který definuje roli pracovního procesu.

![](queue-centric-work-pattern/_static/image8.png)

Další informace najdete v tématu [vytvoření projektu Azure pomocí sady Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

V roli pracovního procesu jsme dotazovat na zprávy voláním `ProcessMessageAsync` metodu `FixItQueueManager` třídu, která jsme viděli dříve.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Metoda zkontroluje, zda je zpráva čekání. Pokud existuje, deserializuje zprávy do `FixItTask` entitu a entitu uloží do databáze. Opakuje cyklus, dokud je fronta prázdná.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Dotazování pro fronty zpráv s sebou nese náklady malé transakce účtovat, takže když neexistuje žádná zpráva čeká na zpracování, role pracovního procesu `RunAsync` metoda čeká sekunda předcházející dotazování znovu voláním `Task.Delay(1000)`.

Ve webovém projektu přidání asynchronní kód automaticky zlepšit výkon, protože služba IIS spravuje fondu omezená vláken. To není případ v projektu role pracovního procesu. Cílem vylepšit škálovatelnost, role pracovního procesu, můžete psát vícevláknový kód nebo použít asynchronní kód pro implementaci [paralelní programování](https://msdn.microsoft.com/library/ff963553.aspx). Ukázka neimplementuje paralelní programování, ale ukazuje, jak provést asynchronní kód, tak můžete implementovat paralelní programování.

## <a name="summary"></a>Souhrn

V této kapitole jste viděli, jak můžete zlepšit odezvu aplikace, spolehlivost a škálovatelnost implementace vzoru fronty.

Toto je poslední 13 vzorce popsané v této e knihy, ale jsou samozřejmě mnoha dalšími vzory a postupy, které vám pomůžou vytvářet úspěšné cloudové aplikace. [Konečné kapitoly](more-patterns-and-guidance.md) poskytuje odkazy na zdroje informací pro témata, která ještě popsaná v těchto 13 vzory.

<a id="resources"></a>
## <a name="resources"></a>Prostředky

Další informace o frontách najdete v následující prostředky.

Dokumentace ke službě:

- [Microsoft Azure Storage fronty 1. část: Začínáme](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Článek o Schacherl římské číslice.
- [Spouštění úloh na pozadí](https://msdn.microsoft.com/library/ff803365.aspx), kapitola 5 [přesouvání aplikací do cloudu, 3. vydání](https://msdn.microsoft.com/library/ff728592.aspx) z Microsoft Patterns and Practices. (Konkrétně část ["Pomocí úložiště front Azure"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Osvědčené postupy pro maximalizaci škálovatelnost a finanční efektivita na základě fronty zasílání zpráv řešení v Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Dokument White paper o Valery Mizonov.
- [Srovnání front Azure a fronty služby Service Bus](https://msdn.microsoft.com/magazine/jj159884.aspx). Zpravodaj MSDN Magazine článek poskytuje další informace, které vám můžou pomoct vybrat které fronty služby. Tento článek uvádí, že Service Bus je závislá na ACS pro ověřování, což znamená, že vaše SB fronty nebude k dispozici, když ACS není k dispozici. Nicméně, protože byl článek napsán, SB byl změněn na vám umožní používat [tokeny SAS](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) jako alternativu k ACS.
- [Microsoft Vzory a postupy – doprovodné materiály k Azure](https://msdn.microsoft.com/library/dn568099.aspx). Úvod do asynchronního zasílání zpráv, kanálů a filtrů, model kompenzační transakce, modelu konkurenčních příjemců, model CQRS, naleznete v tématu.
- [CQRS cesty](https://msdn.microsoft.com/library/jj554200). Elektronická kniha o CQRS od Microsoft Patterns and Practices.

Video:

- [Bezporuchový: Sestavování škálovatelných, odolných cloudových služeb](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Marc Mercuri a Mark Simms. Nabídne základními koncepty a Principy architektury tak vysoce dostupné a zajímavé příběhy z prostředí Microsoft zákazníka poradní tým (CAT) se skutečným zákazníkům. Úvod do služby Azure Storage a fronty naleznete v tématu epizodě 5 počínaje 35:13.

> [!div class="step-by-step"]
> [Předchozí](distributed-caching.md)
> [další](more-patterns-and-guidance.md)
