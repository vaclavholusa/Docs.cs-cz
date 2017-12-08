---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: "Fronty způsob práce zaměřený (vytváření reálných cloudových aplikací s Azure) | Microsoft Docs"
author: MikeWasson
description: "Cloudové aplikace skutečné World sestavení s Azure elektronická kniha je založena na prezentace vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které můžete mu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 125d555a9e170ef35dd99e0409a2442d5f9ae34a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Fronty způsob práce zaměřený (vytváření reálných cloudových aplikací s Azure)
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [tní Dykstra](https://github.com/tdykstra)

[Stažení opravit projektu](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) nebo [stáhnout elektronická kniha](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Vytváření reálného světa cloudových aplikací s Azure** elektronická kniha je založena na prezentaci vyvinuté Scott Guthrie. Vysvětluje 13 vzory a postupy, které vám mohou pomoci být úspěšná, vývoj webových aplikací pro cloud. Informace o elektronická kniha najdete v tématu [první kapitoly](introduction.md).


Jsme viděli dříve, že pomocí několika služeb může vést ke "složené" SLA, kde je aplikace efektivní SLA *produktu* z jednotlivých SLA. Například aplikace opravte používá webové servery, úložiště a databáze SQL. V případě selhání některého z těchto služeb aplikaci vrátí chybu pro uživatele.

Ukládání do mezipaměti je dobrý způsob, jak zpracovat přechodných chyb obsahu jen pro čtení. Ale co když aplikace potřebuje fungují? Například když uživatel odešle nové úlohy opravte ji, aplikace nelze právě přidat úloha do mezipaměti. Aplikace potřebuje k zápisu do úložiště dat, úloha opravit, tak může být zpracována.

To je, kde vzoru fronty způsob práce se dodává. Tento vzor umožňuje volné párování mezi webovou vrstvu a back-end službu.

Zde je, jak funguje vzoru. Když aplikace získá žádost, zařadí do fronty pracovní položku a hned vrátí odpověď. Proces samostatné back-end pak vrátí pracovní položky z fronty a provádí práce.

Vzor fronty způsob práce je užitečné pro:

- Práci, kterou je časově náročná (vysokou latencí).
- Práci, kterou vyžaduje externí služba, která nemusí být k dispozici.
- Fungovat, který je náročný (vysoké využití procesoru).
- Práci, kterou by těžit z míry vyrovnávání (přičemž podléhá shluky nečekané zatížení).

## <a name="reduced-latency"></a>Menší latence

Fronty jsou užitečné, když budete věnovat práci časově náročná. Pokud úloha trvá několik sekund nebo déle místo blokování koncového uživatele, umístí do fronty pracovní položku. Informace pro uživatele "Pracujeme na něm" a pak používat naslouchací proces fronty ke zpracování úloh na pozadí.

Například při nákupu něco v online prodejce, webová stránka potvrzuje vaši objednávku okamžitě. Ale který neznamená, že se vaše vlastní položky již vůz, doručování. Jejich uvést úlohu do fronty, a na pozadí jsou to pro účely ověření kredibility, Příprava položek pro přesouvání a tak dále.

Pro scénáře s krátkou latencí může být delší, pomocí fronty, porovná se provádění úlohy synchronně celkový čas začátku do konce. Ale dokonce i pak můžete další výhody převáží nad tuto nevýhody.

## <a name="increased-reliability"></a>Větší spolehlivost

Ve verzi opravte ji, který jsme jste byla prohlížení dosavadní webového front-endu úzce spojeny s back-end databáze SQL. Pokud není k dispozici služba SQL databáze, uživatel získá chybu. Pokud nefungují opakovaných pokusů (to znamená, je více než přechodné selhání) je jediné, co můžete provést zobrazí chybu a požádat uživatele, a zkuste to znovu později.

![Diagram znázorňující webový front-end selhání při selhání SQL Database back-end](queue-centric-work-pattern/_static/image1.png)

Používání front, když uživatel odešle úlohu opravte ji aplikace zapíše zprávu do fronty. Datovou část zprávy je [JSON](http://json.org/) reprezentace úlohy. Jakmile je zpráva zapsána do fronty, aplikace vrátí a okamžitě zobrazí zpráva o úspěšném provedení uživateli.

Pokud některé z služeb back-end – například databázi SQL nebo naslouchací proces fronty – přejít do režimu offline, uživatelé stále odesílat nové opravte ji úkoly. Zprávy se právě fronty, dokud znovu jsou k dispozici služby back-end. V tomto bodě se back-end služby aktualizovány na nevyřízené položky.

![Diagram zobrazující webové front-endu nadále fungovat, když dojde k chybě SQL Database](queue-centric-work-pattern/_static/image2.png)

Kromě toho nyní můžete přidat další logiku back-end bez obav o odolnost front-endu. Například můžete chtít odeslat e-mail nebo SMS zprávy o vlastníkovi vždy, když je přiřazen nový opravte ji. E-mailu nebo služba SMS přestane být dostupný, můžete zpracovat všechno ostatní a pak přesuňte zprávu do samostatné frontu pro odesílání e-mailu nebo SMS zprávy.

Naše efektivní SLA byla dřív, webové aplikace &times; úložiště &times; SQL Database = 99,7 %. (Viz [návrhu, které zůstanou platné i po selhání](design-to-survive-failures.md).)

Pokud jsme změnit aplikaci, aby používala fronty, má front-end webový závisí pouze na webové aplikace a úložiště pro složené SLA 99.8 %. (Všimněte si, že fronty jsou součástí služby úložiště Azure, jsou součástí stejné SLA jako úložiště objektů blob.)

Pokud potřebujete i lépe než 99.8 %, můžete vytvořit dvě fronty ve dvou různých oblastech. Určete jeden primární a druhý jako sekundární. V aplikaci převzetí služeb při selhání sekundární fronty je-li primární fronta není k dispozici. Možnost je k dispozici ve stejnou dobu jsou velmi malé.

## <a name="rate-leveling-and-independent-scaling"></a>Míra vyrovnání a nezávislé škálování

Fronty jsou užitečné pro takzvaný *míra vyrovnávání* nebo *vyrovnávání zátěže*.

Webové aplikace jsou často náchylné k nečekané shluky v provozu. I když můžete používat automatické škálování a automaticky tak přidejte webové servery pro zpracování zvýšené webový provoz, nemusí být schopné dostatečně rychle reagovat na zpracování náhlému špičky v zatížení automatické škálování. Pokud na webových serverech může přenést některé úkoly, které se mají udělat napsáním zprávu do fronty, jejich zvládli větší provoz. Back-end službu můžete číst zprávy z fronty a jejich zpracování. Hloubka fronty budou zvětšovat a zmenšovat jako příchozí zátěží se mění.

Mnohem časově náročné práce úlovek přeložen na back-end službu můžete se webová úroveň snadněji reagovat na nečekané špičky v provozu. A ušetřit peníze, protože jakékoli dané objem provozu, můžete ji zpracovat méně webové servery.

Můžete nezávisle škálovat webovou vrstvu a back-end službu. Například může vyžadovat tři webových serverů, ale pouze jeden server zpracování fronty zpráv. Nebo pokud spouštíte výpočetně náročných úloh na pozadí, bude pravděpodobně nutné další back-end serverů.

![](queue-centric-work-pattern/_static/image3.png)

Automatické škálování funguje s back-end služby a také s webovou vrstvu. Můžete škálovat nebo snížit počet virtuálních počítačů, které jsou zpracování úloh ve frontě, na základě využití procesoru z back-end virtuálních počítačů. Nebo můžete použít automatické škálování na základě toho, kolik položek se ve frontě. Můžete například zjistit škálování pokusit se zachová maximálně 10 položek ve frontě. Pokud fronta má více než 10 položek, přidá škálování virtuálních počítačů. Pokud budou aktualizovány, bude škálování přerušit další virtuální počítače.

## <a name="adding-queues-to-the-fix-it-application"></a>Přidání fronty pro opravu jeho aplikace

Implementace vzoru fronty, musíme mít dvě změny do aplikace opravit.

- Když uživatel odešle nové úlohy opravte ji, uveďte úlohu ve frontě, místo zápis do databáze.
- Vytvoření služby back-end, který zpracovává zprávy ve frontě.

Do fronty, použijeme [Azure Queue Storage Service](https://www.windowsazure.com/en-us/develop/net/how-to-guides/queue-service/). Další možností je použít [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Při rozhodování, služby, která fronty používat, zvažte, jak vaše aplikace musí odesílat a přijímat zprávy ve frontě:

- Pokud máte spolupracující výrobci a konkurenčních spotřebitelů, zvažte použití služby úložiště front Azure. "Spolupracující výrobci" znamená, že přidáváte více procesů zprávy do fronty. "Neslučitelných příjemci" znamená více procesů jsou stahování zpráv vypnout fronty pro jejich zpracování, ale jakékoli dané zprávy mohou být zpracovány pouze podle jednoho "příjemce." Pokud potřebujete další propustnosti, než můžete získat pomocí jedné frontě, použijte další fronty nebo dalších účtů úložiště.
- Pokud potřebujete [publikování a přihlášení k odběru modelu](http://en.wikipedia.org/wiki/Publish/subscribe), zvažte použití fronty Azure Service Bus.

Aplikace opravte odpovídá spolupracující výrobci a modelu konkurenčních spotřebitelů.

Je potřeba zamyslet se dostupnosti aplikace. Služba fronty úložiště je součástí služby používáme pro úložiště objektů blob, tak ho pomocí nemá žádný vliv na našem SLA. Azure Service Bus je samostatná služba s vlastním SLA. Pokud jsme použili fronty služby Service Bus, nám okolnosti další procento SLA System a naše složené SLA by být nižší. Když zvolíte služby front, ujistěte se, že rozumíte dopad podle svého výběru na dostupnosti aplikace. Další informace najdete v tématu [prostředky](#resources) části.

## <a name="creating-queue-messages"></a>Vytváření zprávy fronty

Do úlohy opravte ji umístit fronty, má front-end webový provede následující kroky:

1. Vytvoření [CloudQueueClient](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) instance. `CloudQueueClient` Instance se používá k provedení požadavků na službu fronty.
2. Vytvořte frontu, pokud ještě neexistuje.
3. Serializuje úloha opravit.
4. Volání [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) uvést zprávy do fronty.

Provedeme tato práce v konstruktoru a `SendMessageAsync` metoda nové `FixItQueueManager` třídy.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Tady se používá [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) knihovny určená k serializaci automatickou do formátu JSON. Můžete použít libovolnou metodu serializace dáváte přednost. JSON má výhodu v podobě se čitelný, aniž by byly míň podrobné než XML.

Kód produkční kvality by přidat logiku zpracování chyb, pozastavení, pokud jsou databáze není k dispozici, více řádně zpracovávat obnovení, vytvořit frontu na spuštění aplikace a spravovat "[poškozených" zprávy](https://msdn.microsoft.com/en-us/library/ms789028(v=vs.110).aspx). (Zprávu, která nelze zpracovat z nějakého důvodu je poškozená zpráva. Nechcete, aby poškozených zprávy a pokuste se nacházejí ve frontě, kde role pracovního procesu průběžně pokusí zpracovat, nezdaří, zkuste to znovu, nezdaří atd.)

Ve front-endu aplikace MVC musíme aktualizovat kód, který vytvoří novou úlohu. Místo uvedení úlohu do úložiště, volání `SendMessageAsync` metoda uvedené výše.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Zpracování zprávy fronty

Při zpracování zprávy ve frontě, vytvoříme back-end službu. Back-end službu spustí nekonečnou smyčku, která provede následující kroky:

1. Získáte další zprávy z fronty.
2. Deserializuje zprávy k úloze opravte ji.
3. Úloha opravit zapsat do databáze.

K hostování back-end službu, Cloudová služba Azure, který obsahuje vytvoříme *role pracovního procesu*. Role pracovního procesu se skládá z jedné nebo více virtuálních počítačů, které můžete provést zpracování back-end. Kód, který běží v těchto virtuálních počítačů bude načítat zprávy z fronty, jakmile budou k dispozici. Pro každou zprávu jsme budete deserializovat datové části JSON a instanci entity, opravte ji úloh zapsat do databáze, použití stejné úložiště, který jsme použili dříve v webová vrstva.

Následující kroky ukazují, jak přidat pracovní projekt role na řešení, které má standardní webového projektu. Tyto kroky byla neudělali v opravte ji projekt, který si můžete stáhnout.

Nejprve přidáte projekt cloudové služby na řešení sady Visual Studio. Klikněte pravým tlačítkem na řešení a vyberte **přidat**, pak **nový projekt**. V levém podokně rozbalte **Visual C#** a vyberte **cloudu**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

V **nová Cloudová služba Azure** dialogové okno, rozbalte **Visual C#** uzlu v levém podokně. Vyberte **Role pracovního procesu** a klikněte na ikonu se šipkou vpravo.

![](queue-centric-work-pattern/_static/image6.png)

(Všimněte si, že můžete také přidat *webovou roli*. Jsme by mohl spustit opravte ji front-endu v rámci stejné cloudové služby namísto spuštění se webu v Azure. Který má několik výhod v snadněji připojení mezi front-end a back-end pro koordinaci. Ale zjednodušení této ukázku jsme se udržuje front-endu ve webové aplikaci Azure App Service a pouze spuštěný back-end v cloudové službě.)

Výchozí název je přiřazen k roli pracovního procesu. Chcete-li změnit název, najeďte myší role pracovního procesu v pravém podokně a pak klikněte na ikonu tužky.

![](queue-centric-work-pattern/_static/image7.png)

Klikněte na tlačítko **OK** k dokončení dialogové okno. Tento postup přidá dva projekty do řešení sady Visual Studio.

- Azure projekt, který definuje cloudové služby, včetně informací o konfiguraci.
- Projekt role pracovního procesu, který definuje roli pracovního procesu.

![](queue-centric-work-pattern/_static/image8.png)

Další informace najdete v tématu [vytvoření projektu Azure pomocí sady Visual Studio.](https://msdn.microsoft.com/en-us/library/windowsazure/ee405487.aspx)

V roli pracovního procesu, jsme dotazování pro zprávy voláním `ProcessMessageAsync` metodu `FixItQueueManager` třídu, která jsme viděli dříve.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Metoda prověří, pokud je zpráva čekání. Pokud existuje, je deserializuje zprávy do `FixItTask` entity a uloží entitu v databázi. Ho opakovací je fronta prázdná.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Dotazování pro zprávy fronty způsobuje malé transakce účtují, takže pokud není žádná zpráva čeká na zpracování, role pracovního procesu `RunAsync` druhý před dotazování metoda znovu čeká voláním `Task.Delay(1000)`.

Ve webovém projektu přidání asynchronní kódu automaticky zlepšit výkon, protože služba IIS spravuje fondu vláken omezené. To není případ v projektu role pracovního procesu. Pokud chcete zlepšit škálovatelnost role pracovního procesu, můžete zapsat Vícevláknová kódu nebo použít asynchronní kód pro implementaci [paralelní programování](https://msdn.microsoft.com/en-us/library/ff963553.aspx). Ukázka neimplementuje paralelní programování ale ukazuje, jak chcete-li kód asynchronní, takže můžete implementovat paralelní programování.

## <a name="summary"></a>Souhrn

V této kapitole jste viděli, jak zlepšit odezvu aplikací, spolehlivosti a škálovatelnosti implementací fronty způsob práce zaměřený.

Toto je poslední vzoru 13 popsané v této e knihy, ale jsou samozřejmě mnoha dalších schémat a postupy, které vám mohou pomoci sestavení úspěšné cloudových aplikací. [Konečné kapitoly](more-patterns-and-guidance.md) poskytuje odkazy na zdroje informací pro témata, které nebyly popsaná v 13 vzorce.

<a id="resources"></a>
## <a name="resources"></a>Prostředky

Další informace o frontách najdete v následujících zdrojích informací.

Dokumentace:

- [Microsoft Azure Storage fronty část 1: Začínáme](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Článek podle římské Schacherl.
- [Provádění úlohy na pozadí](https://msdn.microsoft.com/en-us/library/ff803365.aspx), kapitoly 5 z [přesunutí aplikace do cloudu, 3. vydání](https://msdn.microsoft.com/en-us/library/ff728592.aspx) z Microsoft Patterns and Practices. (Zejména v části ["Pomocí Azure úložiště fronty"](https://msdn.microsoft.com/en-us/library/ff803365.aspx#sec7).)
- [Osvědčené postupy pro maximalizaci škálovatelnost a finanční efektivita na základě fronty zasílání zpráv řešení v Azure](https://msdn.microsoft.com/en-us/library/windowsazure/hh697709.aspx). Dokument White paper podle Valery Mizonov.
- [Porovnání Azure fronty a fronty služby Service Bus](https://msdn.microsoft.com/en-us/magazine/jj159884.aspx). Článek v časopise MSDN, poskytuje další informace, které vám pomohou vybrat fronty služby, která chcete použít. Článek uvádí, že Service Bus je závislá na služby ACS pro ověřování, což znamená, že vaše SB fronty nebude k dispozici, když není k dispozici služby ACS. Ale vzhledem k tomu, že byl článek napsán, SB byl změněn na umožňují používat [tokeny SAS](https://msdn.microsoft.com/en-us/library/windowsazure/dn170477.aspx) jako alternativu k služby ACS.
- [Microsoft Patterns and Practices - Azure pokyny](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Viz Úvod do asynchronního zasílání zpráv, kanály a filtry vzor, kompenzace transakce vzor, neslučitelných příjemci vzor, CQRS vzor.
- [Cesty CQRS](https://msdn.microsoft.com/en-us/library/jj554200). Elektronická kniha o CQRS podle Microsoft Patterns and Practices.

Video:

- [Bezporuchový: Vytváření škálovatelné, odolné cloudové služby](https://channel9.msdn.com/Series/FailSafe). Série videí devět částí Ulrich Homann, Mercuri matolin a moduly SIMM značky. Uvede základními koncepty a architektury zásady způsobem, velmi přístupné a zajímavé, s scénářů čerpají z prostředí Microsoft zákazníka poradní tým (CAT) s skutečným zákazníkům. Úvod do služby Azure Storage a fronty najdete v části díl 5 počínaje 35:13.

>[!div class="step-by-step"]
[Předchozí](distributed-caching.md)
[další](more-patterns-and-guidance.md)
