---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Rozhraní Entity Framework mocking při testování rozhraní ASP.NET Web API 2 částí | Microsoft Docs
author: tfitzmac
description: Tento pokyny a aplikace ukazují, jak vytvářet testy částí pro aplikace webových rozhraní API 2, který používá rozhraní Entity Framework. Ukazuje, jak upravit...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: abfde7edec85812de3560f4edefb110c3e374580
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152862"
---
<a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a>Rozhraní Entity Framework mocking při rozhraní ASP.NET Web API 2 testování částí
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Tento pokyny a aplikace ukazují, jak vytvářet testy částí pro aplikace webových rozhraní API 2, který používá rozhraní Entity Framework. Zobrazuje postup úpravy vygenerované kontroleru k povolení, předá objekt kontextu pro testování a jak vytvořit testovací objekty, které pracují s platformou Entity Framework.
> 
> Úvod do testování částí pomocí rozhraní ASP.NET Web API, najdete v části [jednotkové testování v ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).
> 
> Tento kurz předpokládá, že se seznámíte se základními koncepcemi rozhraní ASP.NET Web API. Úvodní kurz, najdete v části [Začínáme s ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/vs/)
> - Webové rozhraní API 2


## <a name="in-this-topic"></a>V tomto tématu

Toto téma obsahuje následující oddíly:

- [Požadavky](#prereqs)
- [Stáhněte si kód](#download)
- [Vytvoření aplikace pomocí projektu testování částí](#appwithunittest)
- [Vytvořte třídu modelu](#modelclass)
- [Přidání kontroleru](#controller)
- [Přidat vkládání závislostí](#dependency)
- [Instalace balíčků NuGet v testovacího projektu](#testpackages)
- [Vytvoření kontextu testu](#testcontext)
- [Vytváření testů](#tests)
- [Spouštění testů](#runtests)

Pokud jste už dokončili kroky v [jednotkové testování v ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), můžete přeskočit k části [přidat řadič](#controller).

<a id="prereqs"></a>
## <a name="prerequisites"></a>Požadavky

Visual Studio 2017 Community, Professional nebo Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Stáhněte si kód

Stažení [dokončený projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Ke stažení projekt zahrnuje jednotka testovacího kódu pro toto téma a [jednotky testování webové rozhraní API 2 ASP.NET](unit-testing-with-aspnet-web-api.md) tématu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Vytvoření aplikace pomocí projektu testování částí

Můžete buď vytvoření projektu testů jednotek při vytváření aplikace nebo přidání projektu testů jednotek do existující aplikace. Tento kurz ukazuje, vytvoření projektu testů jednotek při vytváření aplikace.

Vytvoření nové webové aplikace ASP.NET s názvem **StoreApp**.

V systému windows nový projekt ASP.NET, vyberte **prázdný** šablony a přidat složky a základní odkazy pro webového rozhraní API. Vyberte **přidat testování částí** možnost. Automaticky s názvem projektu testování částí **StoreApp.Tests**. Můžete ponechat tento název.

![Vytvoření projektu testování částí](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

Po vytvoření aplikace, zobrazí se obsahuje dva projekty – **StoreApp** a **StoreApp.Tests**.

<a id="modelclass"></a>
## <a name="create-the-model-class"></a>Vytvořte třídu modelu

V projektu StoreApp přidat třídu soubor, který chcete **modely** složku s názvem **Product.cs**. Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

Sestavte řešení.

<a id="controller"></a>
## <a name="add-the-controller"></a>Přidání kontroleru

Klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** a **novou vygenerovanou položku**. Vyberte kontroler Web API 2 s akcemi používající rozhraní Entity Framework.

![Přidat nový řadič](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

Nastavte následující hodnoty:

- Název řadiče: **ProductController**
- Třída modelu: **produktu**
- Třída kontextu dat: [vyberte **nový kontext dat** tlačítko, které vyplní hodnoty vidíte níže]

![Zadejte řadič](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

Klikněte na tlačítko **přidat** vytvoření kontroleru pomocí automaticky generovaný kód. Tento kód obsahuje metody pro vytváření, načítání, aktualizace a odstranění instancí třídy produktu. Následující kód ukazuje metodu pro přidání produktu. Všimněte si, že metoda vrátí instanci **IHttpActionResult**.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

IHttpActionResult je jednou z nových funkcí ve webovém rozhraní API 2 a usnadňují vývoj testů jednotek.

V další části, bude přizpůsobit generovaný kód pro usnadnění předávání objektů testovací řadiče.

<a id="dependency"></a>
## <a name="add-dependency-injection"></a>Přidat vkládání závislostí

Třída ProductController v současné době je pevně zakódovaná použití instance třídy StoreAppContext. K úpravě vaší aplikace a odebrání závislostí této pevně použijete vzor názvem vkládání závislostí. Porušením této závislosti lze předat v mock objektu při testování.

Klikněte pravým tlačítkem myši **modely** složku a přidejte nové rozhraní s názvem **IStoreAppContext**.

Nahraďte kód následujícím kódem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

Otevřete soubor StoreAppContext.cs a proveďte následující zvýrazněný změny. Změny důležité si uvědomit, jsou tyto:

- Třída StoreAppContext implementuje rozhraní IStoreAppContext
- Metoda MarkAsModified je implementována.


[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

Otevřete soubor ProductController.cs. Změňte existující kód upravíte tak, aby odpovídaly zvýrazněný kód. Tyto změny přerušení závislost na StoreAppContext a povolit další třídy pro třídy kontextu předávat jiný objekt. Tato změna vám umožní předat během testování částí v kontextu testu.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

Neexistuje jeden další změny, které je třeba provést v ProductController. V **PutProduct** metoda, nahraďte řádek, který nastaví stav entity změnil pomocí volání do metody MarkAsModified.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

Sestavte řešení.

Teď jste připravení nastavit k testovacímu projektu.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalace balíčků NuGet v testovacího projektu

Použijete-li vytvořit aplikaci prázdnou šablonou, projektu testování částí (StoreApp.Tests) neobsahuje žádné nainstalované balíčky NuGet. Další šablony, jako je například šabloně webového rozhraní API zahrnout některé balíčky NuGet do projektu testování částí. V tomto kurzu musí obsahovat packge Entity Framework a balíček Microsoft ASP.NET Web API 2 jádra pro projekt test.

Klikněte pravým tlačítkem na projekt StoreApp.Tests a vyberte **spravovat balíčky NuGet**. Je třeba vybrat StoreApp.Tests projektu přidat balíčky do daného projektu.

![Správa balíčků](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

Z Online balíčků najít a nainstalovat balíček EntityFramework (verze 6.0 nebo novější). Pokud se zdá, že EntityFramework balíček je už nainstalovaný, možná jste vybrali projektu StoreApp místo StoreApp.Tests projektu.

![Přidání Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

Najít a nainstalovat balíček Microsoft ASP.NET Web API 2 jádra.

![instalovat balíček základní webové rozhraní api](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

Zavřete okno Spravovat balíčky NuGet.

<a id="testcontext"></a>
## <a name="create-test-context"></a>Vytvoření kontextu testu

Přidejte třídu s názvem **TestDbSet** pro projekt test. Tato třída slouží jako základní třída pro vaše testovací datové sady. Nahraďte kód následujícím kódem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

Přidejte třídu s názvem **TestProductDbSet** pro projekt test, který obsahuje následující kód.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

Přidejte třídu s názvem **TestStoreAppContext** a existujícího kódu nahraďte následujícím kódem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a>Vytváření testů

Ve výchozím nastavení, testovacího projektu obsahuje prázdný testovací soubor s názvem **UnitTest1.cs**. Tento soubor obsahuje atributy že použijete k vytvoření testovací metody. V tomto kurzu můžete odstranit tento soubor, protože budete přidávat nové třídy testu.

Přidejte třídu s názvem **TestProductController** pro projekt test. Nahraďte kód následujícím kódem.

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Spouštění testů

Nyní jste připraveni ke spuštění testů. Všechny metody, které jsou označené **TestMethod** atribut bude být testována. Z **Test** položky nabídky, spusťte testy.

![Provádění testů](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

Otevřete **Průzkumníka testů** okně a Všimněte si výsledky testů.

![výsledky testu](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
