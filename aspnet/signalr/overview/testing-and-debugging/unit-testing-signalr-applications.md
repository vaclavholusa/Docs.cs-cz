---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "Testování částí aplikací SignalR | Microsoft Docs"
author: pfletcher
description: "Tento článek popisuje, jak používat funkce testování částí 2.0 SignalR."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a>Aplikací SignalR testování částí
====================
podle [Patrik Fletcher](https://github.com/pfletcher)

> Tento článek popisuje použití funkce SignalR 2 testování částí. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>Verze softwaru použitým v tomto tématu
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR verze 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Dotazy a připomínky
> 
> Prosím sdělit svůj názor na tom, jak líbilo tohoto kurzu a co jsme může zlepšit v komentářích v dolní části stránky. Pokud máte otázky, které přímo nesouvisejí s kurz, můžete je do příspěvku [fórum pro ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) nebo [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Testování částí aplikací SignalR

Funkce testů jednotek v SignalR 2 slouží k vytvoření testů jednotek pro aplikace SignalR. SignalR 2 zahrnuje [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) rozhraní, které slouží k vytvoření mock objektu k simulaci vaší metod rozbočovače pro testování.

V této části přidáte testy částí pro aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí [XUnit.net](https://github.com/xunit/xunit) a [Moq](https://github.com/Moq/moq4).

XUnit.net se použije k řízení test; Moq se použije k vytvoření [model](http://en.wikipedia.org/wiki/Mock_object) objekt pro testování. Další mocking architektury lze v případě potřeby; [NSubstitute](http://nsubstitute.github.io/) je také vhodné použít. Tento kurz ukazuje, jak nastavit mock objektu dvěma způsoby: nejdřív pomocí `dynamic` objektu (dostupné v rozhraní .NET Framework 4) a druhý, přes rozhraní.

### <a name="contents"></a>Obsah

Tento kurz obsahuje následující části.

- [Testování částí pomocí dynamické](#dynamic)
- [Podle typu testování částí](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Testování částí pomocí dynamické

V této části přidáte testů jednotek pro aplikace vytvořené v [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí dynamických objektů.

1. Nainstalujte [XUnit Runner rozšíření](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) pro Visual Studio 2013.
2. Buď dokončení [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md), nebo stáhnout hotová aplikace z [galerie kódů MSDN](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Pokud používáte verzi stažení aplikace Začínáme, otevřete **Konzola správce balíčků** a klikněte na tlačítko **obnovení** přidejte balíček SignalR do projektu.

    ![Obnovení balíčků](unit-testing-signalr-applications/_static/image1.png)
4. Přidáte projekt do řešení pro testování částí. Klikněte pravým tlačítkem na řešení v **Průzkumníku řešení** a vyberte **přidat**, **nový projekt...** . V části **C#** uzlu, vyberte **Windows** uzlu. Vyberte **třídy knihovny**. Název nového projektu **TestLibrary** a klikněte na tlačítko **OK**.

    ![Vytvořit testovací knihovny](unit-testing-signalr-applications/_static/image2.png)
5. Přidáte odkaz v projektu knihovny testu do projektu SignalRChat. Klikněte pravým tlačítkem myši **TestLibrary** projektu a vyberte **přidat**, **odkaz...** . Vyberte **projekty** pod uzlem **řešení** uzlu a zkontrolujte **SignalRChat**. Click **OK**.

    ![Přidat odkaz na projekt](unit-testing-signalr-applications/_static/image3.png)
6. Přidat SignalR, Moq a XUnit balíčky, do kterých **TestLibrary** projektu. V **Konzola správce balíčků**, nastavte **výchozí projekt** rozevírací k **TestLibrary**. V okně konzoly spusťte následující příkazy:

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Instalovat balíčky](unit-testing-signalr-applications/_static/image4.png)
7. Vytvoření testovacího souboru. Klikněte pravým tlačítkem myši **TestLibrary** projektu a klikněte na tlačítko **přidat...** , **Třída**. Pojmenujte novou třídu **Tests.cs**.
8. Obsah Tests.cs nahraďte následujícím kódem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Ve výše uvedeném kódu, je vytvořený pomocí testovacího klienta `Mock` objektu z [Moq](https://github.com/Moq/moq4) knihovny typu [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (v SignalR 2.1 přiřadit `dynamic` pro typ parametr.) `IHubCallerConnectionContext` Rozhraní je objekt proxy, pomocí kterého můžete volat metody na straně klienta. `broadcastMessage` Funkce je pak definované pro imitované klienta tak, aby ji můžete volat `ChatHub` třídy. Modul testu pak zavolá `Send` metodu `ChatHub` třída, která volá mocked `broadcastMessage` funkce.
9. Sestavte řešení stisknutím **F6**.
10. Spuštění testování částí. V sadě Visual Studio, vyberte **Test**, **Windows**, **Průzkumníka testů**. V okně Průzkumníka testů, klikněte pravým tlačítkem na **HubsAreMockableViaDynamic** a vyberte **spuštění testů vybrané**.

    ![Průzkumník testů](unit-testing-signalr-applications/_static/image5.png)
11. Ověřte, že test proběhl úspěšně kontrolou dolním podokně, v okně Průzkumníka testů. Okno se zobrazí, že test proběhl úspěšně.

    ![Test proběhl úspěšně.](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Podle typu testování částí

V této části přidáte testu pro vytvořené v aplikaci [kurzu Začínáme](../getting-started/tutorial-getting-started-with-signalr.md) pomocí rozhraní, které obsahuje metodu má být testována.

1. Dokončete kroky 1-7 v [testování částí pomocí dynamické](#dynamic) kurzu výše.
2. Obsah Tests.cs nahraďte následujícím kódem.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Ve výše uvedeném kódu, se vytvoří rozhraní definování podpis `broadcastMessage` metoda, pro který modul testu vytvoří imitované klienta. Imitované klienta je poté jste vytvořili pomocí `Mock` objektu typu [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (v SignalR 2.1 přiřadit `dynamic` pro parametr typu.) `IHubCallerConnectionContext` Rozhraní je objekt proxy, pomocí kterého můžete volat metody na straně klienta.

    Test potom vytvoří instanci `ChatHub`a potom vytvoří na cvičnou verzi `broadcastMessage` metoda, která zase se vyvolá při volání `Send` metoda rozbočovače.
3. Sestavte řešení stisknutím **F6**.
4. Spuštění testování částí. V sadě Visual Studio, vyberte **Test**, **Windows**, **Průzkumníka testů**. V okně Průzkumníka testů, klikněte pravým tlačítkem na **HubsAreMockableViaDynamic** a vyberte **spuštění testů vybrané**.

    ![Průzkumník testů](unit-testing-signalr-applications/_static/image7.png)
5. Ověřte, že test proběhl úspěšně kontrolou dolním podokně, v okně Průzkumníka testů. Okno se zobrazí, že test proběhl úspěšně.

    ![Test proběhl úspěšně.](unit-testing-signalr-applications/_static/image8.png)
