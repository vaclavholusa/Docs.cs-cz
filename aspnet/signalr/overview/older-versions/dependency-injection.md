---
uid: signalr/overview/older-versions/dependency-injection
title: Vkládání závislostí v systému SignalR 1.x | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/15/2013
ms.topic: article
ms.assetid: eaa206c4-edb3-487e-8fcb-54a3261fed36
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 6fd155adc9a0aa71b66db7a51062a51fb0c1feca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
ms.locfileid: "26565951"
---
<a name="dependency-injection-in-signalr-1x"></a>Vkládání závislostí v systému SignalR 1.x
====================
podle [Karel Wasson](https://github.com/MikeWasson), [Patrik Fletcher](https://github.com/pfletcher)

Vkládání závislostí je způsob, jak odebrat pevně zakódované závislosti mezi objekty, usnadnit nahradit objektu závislosti, buď pro testování (pomocí mock objektů), nebo chcete změnit chování. Tento kurz ukazuje, jak provádět vkládání závislostí na rozbočovače SignalR. Také ukazuje, jak používat technologie IoC kontejnery s SignalR. Kontejner IoC je obecné rozhraní pro vkládání závislostí.

## <a name="what-is-dependency-injection"></a>Co je vkládání závislostí?

Tuto část přeskočte, pokud jste již obeznámeni s vkládání závislostí.

*Vkládání závislostí* (DI) je vzor, kde objekty nejsou zodpovědný za vytváření vlastní závislosti. Zde je jednoduchý příklad ke DI. Předpokládejme, že máte objekt, který potřebuje k protokolování zpráv. Můžete třeba definovat rozhraní protokolování:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

V objektu, můžete vytvořit `ILogger` k protokolování zpráv:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Tento postup funguje, ale není optimální. Pokud chcete nahradit `FileLogger` s jinou `ILogger` implementace, budete muset upravit `SomeComponent`. Předpokládající, že mnoho dalších objektů pomocí `FileLogger`, budete muset změnit všechny z nich. Nebo pokud se rozhodnete, aby `FileLogger` typu singleton, budete také muset provést změny v celé aplikaci.

Lepším řešením je "Vložit" `ILogger` do objektu – například pomocí argument konstruktoru:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Teď objekt není zodpovědná za vybrat, které `ILogger` používat. Můžete swich `ILogger` implementace, aniž byste museli měnit objekty, které jsou na ní závislé.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Tento vzor nazývá [konstruktor vkládání](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Jiné vzor je setter vkládání, kde můžete nastavit závislost prostřednictvím setter metody nebo vlastnosti.

## <a name="simple-dependency-injection-in-signalr"></a>Vkládání jednoduché závislostí v systému SignalR

Vezměte v úvahu chatovací aplikace z tohoto kurzu [Začínáme s SignalR](../getting-started/tutorial-getting-started-with-signalr.md). Tady je třídy rozbočovače z této aplikace:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Předpokládejme, že chcete ukládat chatovací zprávy na serveru před jejich odesláním. Můžete například definovat rozhraní, které abstrahuje tuto funkci a pomocí DI vložení do rozhraní `ChatHub` třídy.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Jediným problémem je, že aplikace SignalR přímo nevytvoří centra; SignalR je automaticky vytvoří. Ve výchozím nastavení očekává SignalR třídy rozbočovače mít konstruktor bez parametrů. Však můžete snadno zaregistrovat funkci pro vytvoření instance hub a tuto funkci můžete používat k provedení DI. Zaregistrovat funkci voláním **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Teď SignalR bude volat tuto funkci anonymní, kdykoli bude potřeba vytvořit `ChatHub` instance.

## <a name="ioc-containers"></a>Technologie IoC kontejnery

Předchozí kód je vhodná pro jednoduché případy. Ale stále museli zapsat toto:

[!code-css[Main](dependency-injection/samples/sample8.css)]

V komplexní aplikace s velký počet závislostí může být nutné zapsat spoustu tento kód "kabeláž". Tento kód může být obtížné spravovat, zejména v případě, že jsou vnořeny závislosti. Také je obtížné testování částí.

Jeden řešením je použití kontejner IoC. Kontejner IoC je softwarová součást, která je zodpovědná za správu závislosti. Zaregistrujte typy s kontejner a poté použít k vytvoření objektů kontejneru. Kontejner se automaticky hodnoty se vztahy závislostí. Mnoho IoC kontejnery umožňují řídit třeba životní cyklus objektů a oboru.

> [!NOTE]
> "IoC" znamená "inverzi řízení", což je obecný vzor, kde rozhraní volání do kódu aplikace. Kontejner IoC vytvoří vašich objektů, které "Invertuje" obvyklý tok řízení.


## <a name="using-ioc-containers-in-signalr"></a>Použití technologie IoC kontejnerů v systému SignalR

Chatovací aplikace je pravděpodobně příliš jednoduchý, abyste mohli využívat výhod kontejner IoC. Místo toho Podíváme se na [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) ukázka.

Ukázka StockTicker definuje dvě hlavní třídy:

- `StockTickerHub`Třída: rozbočovače, která spravuje připojení klientů.
- `StockTicker`: Typu singleton, která obsahuje uložených ceny a je pravidelně aktualizuje.

`StockTickerHub`obsahuje odkaz na `StockTicker` typu singleton, zatímco `StockTicker` obsahuje odkaz na **IHubConnectionContext** pro `StockTickerHub`. Toto rozhraní se používá ke komunikaci s `StockTickerHub` instance. (Další informace najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](index.md).)

Kontejner IoC jsme vám pomůže trochu untangle tyto závislosti. Nejprve umožňuje zjednodušit `StockTickerHub` a `StockTicker` třídy. V následujícím kódu I jste komentované části jsme nepotřebujete.

Odebrat konstruktor bez parametrů z `StockTicker`. Místo toho jsme vždy použije DI k vytvoření rozbočovače.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

U StockTicker odeberte instanci typu singleton. Později použijeme kontejner IoC k řízení životního cyklu StockTicker. Také zveřejněte konstruktoru.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

V dalším kroku jsme Refaktorovat kód tak, že vytvoříte rozhraní pro `StockTicker`. Toto rozhraní použijeme oddělit `StockTickerHub` z `StockTicker` třídy.

Sada Visual Studio provádí tento druh refaktoringu easy přístup. Otevřete soubor StockTicker.cs, klikněte pravým tlačítkem na `StockTicker` deklarace třídy a vyberte **Refaktorovat** ... **Extrahování rozhraní**.

![](dependency-injection/_static/image1.png)

V **extrahování rozhraní** dialogové okno, klikněte na tlačítko **Vybrat vše**. Nechte ostatní výchozí hodnoty. Click **OK**.

![](dependency-injection/_static/image2.png)

Visual Studio vytvoří nové rozhraní s názvem `IStockTicker`a také změní `StockTicker` k odvozování z `IStockTicker`.

Otevřete soubor IStockTicker.cs a změňte rozhraní **veřejné**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

V `StockTickerHub` třídy, změňte dvě instance `StockTicker` k `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Vytvoření `IStockTicker` rozhraní není nezbytně nutné, ale vy jste chtěli zobrazit, jak DI může přispět ke snížení párování mezi komponentami ve vaší aplikaci.

## <a name="add-the-ninject-library"></a>Přidání knihovny Ninject

Existuje mnoho kontejnerů IoC open source pro .NET. V tomto kurzu budete použít [Ninject](http://www.ninject.org/). (Zahrnout další oblíbené knihovny [rošády Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), a [StructureMap ](http://docs.structuremap.net).)

Použití Správce balíčků NuGet k instalaci [Ninject knihovny](https://nuget.org/packages/Ninject/3.0.1.10). V sadě Visual Studio z **nástroje** nabídky vyberte možnost **Správce balíčků knihoven** | **Konzola správce balíčků**. V okně konzoly Správce balíčků zadejte následující příkaz:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>Nahraďte překladač závislostí pro SignalR

Pokud chcete použít Ninject v rámci SignalR, vytvořte třídu, která je odvozena z **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Tato třída přepsání **metody GetService** a **metodu GetServices** metody **DefaultDependencyResolver**. SignalR volá tyto metody vytvořit různé objekty za běhu, včetně instance hub, jakož i různé služby, které se používá interně SignalR.

- **Metody GetService** metoda vytvoří jednu instanci typu. Potlačí tuto metodu volat jádra Ninject **TryGet** metoda. Pokud tato metoda vrátí hodnotu null, přejít k výchozí překladač.
- **Metodu GetServices** metoda vytvoří kolekci objektů zadaného typu. Potlačí tuto metodu ke zřetězení výsledků Ninject s výsledky z výchozí překladač.

## <a name="configure-ninject-bindings"></a>Konfigurovat Ninject vazby

Nyní použijeme Ninject deklarovat typ vazby.

Otevřete soubor RegisterHubs.cs. V `RegisterHubs.Start` metoda, vytvoření kontejneru Ninject, který volá Ninject *jádra*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Vytvoření instance překladače naše vlastní závislosti:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

Vytvoření vazby pro `IStockTicker` následujícím způsobem:

[!code-html[Main](dependency-injection/samples/sample17.html)]

Tento kód vlastně říká dvě věci. První, vždy, když aplikace potřebuje `IStockTicker`, jádra by měl vytvořit instanci `StockTicker`. Druhý, `StockTicker` třída by měla být vytvořená jako objekt typu singleton. Ninject vytvoří jednu instanci objektu a vrátit stejnou instanci pro každý požadavek.

Vytvoření vazby pro **IHubConnectionContext** následujícím způsobem:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Tento kód creatres anonymní funkce, která vrací **IHubConnection**. **WhenInjectedInto** metoda informuje Ninject tuto funkci použít, pouze při vytváření `IStockTicker` instance. Důvodem je skutečnost, že se vytvoří SignalR **IHubConnectionContext** instance interně, a nechceme přepsat, jak je vytvořila SignalR. Tato funkce se vztahuje pouze na našem `StockTicker` třídy.

Předat do překladače závislostí **MapHubs** metoda:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Teď SignalR použije překladač zadaný v **MapHubs**, namísto výchozí překladač.

Zde je kód úplný výpis pro `RegisterHubs.Start`.

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

Ke spuštění aplikace StockTicker v sadě Visual Studio, stisknutím klávesy F5. V okně prohlížeče, přejděte na `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Aplikace má přesně stejné funkce jako před. (Popis najdete v tématu [Server všesměrového vysílání pomocí funkce SignalR technologie ASP.NET](index.md).) Jsme nedošlo ke změně chování; právě snazší kód pro testování, údržbu a momentální.
