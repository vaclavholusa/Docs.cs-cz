---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: "Rozhraní ASP.NET Web API 2 testování částí | Microsoft Docs"
author: tfitzmac
description: "Tento pokyny a aplikace ukazují, jak vytvářet testy částí jednoduché pro vaši aplikaci webovém rozhraní API 2. Tento kurz ukazuje, jak zahrnout proj test jednotky..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/05/2014
ms.topic: article
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 4d6102dd81589e41894d8ecd95bf9ddd761a65bd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-aspnet-web-api-2"></a>Rozhraní ASP.NET Web API 2 testování částí
====================
podle [tní FitzMacken](https://github.com/tfitzmac)

[Stáhněte si dokončený projekt](http://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-e2867d4d)

> Tento pokyny a aplikace ukazují, jak vytvářet testy částí jednoduché pro vaši aplikaci webovém rozhraní API 2. Tento kurz ukazuje, jak chcete zahrnout projektu testů jednotek ve vašem řešení a zapisovat zkušební metody, které zkontrolujte vrácené hodnoty z metody kontroleru.
> 
> Tento kurz předpokládá, že se seznámíte se základními koncepcemi rozhraní ASP.NET Web API. Úvodní kurz, najdete v části [Začínáme s ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).
> 
> Testování částí v tomto tématu jsou záměrně omezeny na jednoduché datové scénáře. Testování pokročilejší scénáře datových částí, najdete v části [Mocking Entity Framework při jednotky testování ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
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

    - [Přidání projektu testů jednotek při vytváření aplikace](#whencreate)
    - [Přidání projektu testů jednotek do existující aplikace](#addtoexisting)
- [Nastavení aplikace webové rozhraní API 2](#setupproject)
- [Instalace balíčků NuGet v testovacího projektu](#testpackages)
- [Vytváření testů](#tests)
- [Spouštění testů](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a>Požadavky

Visual Studio 2017 Community, Professional nebo Enterprise edition

<a id="download"></a>
## <a name="download-code"></a>Stáhněte si kód

Stažení [dokončený projekt](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11). Ke stažení projekt zahrnuje jednotka testovacího kódu pro toto téma a [Mocking Entity Framework při jednotky testování rozhraní ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) tématu.

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a>Vytvoření aplikace pomocí projektu testování částí

Můžete buď vytvoření projektu testů jednotek při vytváření aplikace nebo přidání projektu testů jednotek do existující aplikace. Tento kurz ukazuje obě metody pro vytvoření projektu testování částí. Chcete-li v tomto kurzu, můžete použít buď způsob.

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a>Přidání projektu testů jednotek při vytváření aplikace

Vytvoření nové webové aplikace ASP.NET s názvem **StoreApp**.

![Vytvoření projektu](unit-testing-with-aspnet-web-api/_static/image1.png)

V systému windows nový projekt ASP.NET, vyberte **prázdný** šablony a přidat složky a základní odkazy pro webového rozhraní API. Vyberte **přidat testování částí** možnost. Automaticky s názvem projektu testování částí **StoreApp.Tests**. Můžete ponechat tento název.

![Vytvoření projektu testování částí](unit-testing-with-aspnet-web-api/_static/image2.png)

Po vytvoření aplikace, zobrazí se, že obsahuje dva projekty.

![dva projekty](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a>Přidání projektu testů jednotek do existující aplikace

Pokud jste nevytvořili projektu testů jednotek při vytváření aplikace, můžete přidat jeden kdykoli. Předpokládejme například, že již máte aplikaci s názvem StoreApp a chcete přidat testování částí. Chcete-li přidat projektu testů jednotek, klikněte pravým tlačítkem na řešení a vyberte **přidat** a **nový projekt**.

![Přidat nový projekt do řešení](unit-testing-with-aspnet-web-api/_static/image4.png)

Vyberte **Test** v levém podokně a vyberte **projektu testování částí** pro typ projektu. Název projektu **StoreApp.Tests**.

![Přidání projektu testování částí](unit-testing-with-aspnet-web-api/_static/image5.png)

Zobrazí se projektu testů jednotek ve vašem řešení.

V projektu testování částí přidáte odkaz na projekt na původní projekt.

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a>Nastavení aplikace webové rozhraní API 2

V projektu StoreApp přidat třídu soubor, který chcete **modely** složku s názvem **Product.cs**. Obsah souboru nahraďte následujícím kódem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

Sestavte řešení.

Klikněte pravým tlačítkem na složku řadiče a vyberte **přidat** a **novou vygenerovanou položku**. Vyberte **prázdný kontroler – rozhraní Web API 2**.

![Přidat nový řadič](unit-testing-with-aspnet-web-api/_static/image6.png)

Nastavte název řadiče na **SimpleProductController**a klikněte na tlačítko **přidat**.

![Zadejte řadič](unit-testing-with-aspnet-web-api/_static/image7.png)

Existujícího kódu nahraďte následujícím kódem. Pro zjednodušení tento příklad, jsou data uložena v seznamu místo databáze. V seznamu definované v této třídě představuje provozními daty. Všimněte si, že je řadič obsahuje konstruktor, který přebírá jako parametr seznam objektů produktu. Tento konstruktor umožňuje předat testovací data při testování částí. Řadičem také zahrnuje dvě **asynchronní** metody pro ilustraci asynchronních metod testování částí. Tyto asynchronní metody, které byly implementovány voláním **Task.FromResult** minimalizovat nadbytečné kód, ale obvykle metody by mělo zahrnovat náročná operace.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

Metoda GetProduct vrací instanci třídy **IHttpActionResult** rozhraní. IHttpActionResult je jednou z nových funkcí ve webovém rozhraní API 2 a usnadňují vývoj testů jednotek. Třídy, které implementují rozhraní IHttpActionResult se nacházejí v [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) oboru názvů. Tyto třídy představují možné odpovědí z akcí žádosti a odpovídají stavové kódy HTTP.

Sestavte řešení.

Teď jste připravení nastavit k testovacímu projektu.

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a>Instalace balíčků NuGet v testovacího projektu

Použijete-li vytvořit aplikaci prázdnou šablonou, projektu testování částí (StoreApp.Tests) neobsahuje žádné nainstalované balíčky NuGet. Další šablony, jako je například šabloně webového rozhraní API zahrnout některé balíčky NuGet do projektu testování částí. V tomto kurzu musí zahrnovat balíček Microsoft ASP.NET Web API 2 jádra pro projekt test.

Klikněte pravým tlačítkem na projekt StoreApp.Tests a vyberte **spravovat balíčky NuGet**. Je třeba vybrat StoreApp.Tests projektu přidat balíčky do daného projektu.

![Správa balíčků](unit-testing-with-aspnet-web-api/_static/image8.png)

Najít a nainstalovat balíček Microsoft ASP.NET Web API 2 jádra.

![instalovat balíček základní webové rozhraní api](unit-testing-with-aspnet-web-api/_static/image9.png)

Zavřete okno Spravovat balíčky NuGet.

<a id="tests"></a>
## <a name="create-tests"></a>Vytváření testů

Ve výchozím nastavení zahrnuje soubor prázdný test s názvem UnitTest1.cs testovacího projektu. Tento soubor obsahuje atributy že použijete k vytvoření testovací metody. Testy jednotek můžete použít tento soubor nebo je možné vytvořit svůj vlastní soubor.

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

V tomto kurzu vytvoříte vlastní třídu testu. Soubor UnitTest1.cs můžete odstranit. Přidejte třídu s názvem **TestSimpleProductController.cs**a kódu nahraďte následujícím kódem.

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a>Spouštění testů

Nyní jste připraveni ke spuštění testů. Všechny metody, které jsou označené **TestMethod** atribut bude být testována. Z **Test** položky nabídky, spusťte testy.

![Provádění testů](unit-testing-with-aspnet-web-api/_static/image11.png)

Otevřete **Průzkumníka testů** okně a Všimněte si výsledky testů.

![výsledky testu](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a>Souhrn

Dokončení tohoto kurzu. Data v tomto kurzu se záměrně zjednodušené zaměřit se na jednotce testování podmínky. Testování pokročilejší scénáře datových částí, najdete v části [Mocking Entity Framework při jednotky testování ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).
