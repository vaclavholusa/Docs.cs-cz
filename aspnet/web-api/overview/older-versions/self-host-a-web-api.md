---
uid: web-api/overview/older-versions/self-host-a-web-api
title: Hostování na vlastním rozhraní ASP.NET Web API 1 (C#) | Dokumentace Microsoftu
author: MikeWasson
description: Rozhraní ASP.NET Web API nevyžaduje, aby služba IIS. Webové rozhraní API můžete samoobslužné hostování ve vlastním procesu hostitele. Tento kurz ukazuje postupy při hostování webového rozhraní API uvnitř applic konzoly...
ms.author: riande
ms.date: 01/26/2012
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: 63d192a6fa2aafef3770d5b0b97ec32e001b69db
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912695"
---
<a name="self-host-aspnet-web-api-1-c"></a>Hostování na vlastním rozhraní ASP.NET Web API 1 (C#)
====================
podle [Mike Wasson](https://github.com/MikeWasson)

> Rozhraní ASP.NET Web API nevyžaduje, aby služba IIS. Webové rozhraní API můžete samoobslužné hostování ve vlastním procesu hostitele. Tento kurz ukazuje postupy při hostování webového rozhraní API v konzolové aplikaci.
> 
> **Nová aplikace by měly používat OWIN k samoobslužnému hostování webového rozhraní API.** Zobrazit [použití rozhraní OWIN k samoobslužnému hostování webového rozhraní API 2 ASP.NET](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Vytvořte projekt konzolové aplikace

Spusťte sadu Visual Studio a vyberte **nový projekt** z **Start** stránky. Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **Windows**. V seznamu šablon projektu vyberte **konzolovou aplikaci**. Pojmenujte projekt &quot;SelfHost&quot; a klikněte na tlačítko **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Nastavit cílové rozhraní (Visual Studio 2010)

Pokud používáte Visual Studio 2010, změňte cílovou architekturu na .NET Framework 4.0. (Ve výchozím nastavení šablona cíle projektu [rozhraní .net Framework Client Profile](https://msdn.microsoft.com/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

V Průzkumníku řešení klikněte pravým tlačítkem myši na projekt a vyberte **vlastnosti**. V **Cílová architektura** rozevírací seznam, změnit cílovou architekturu na .NET Framework 4.0. Po zobrazení výzvy na použití změny, klikněte na tlačítko **Ano**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalace Správce balíčků NuGet

Správce balíčků NuGet je nejjednodušší způsob, jak přidat sestavení webového rozhraní API do projektu – technologie ASP.NET.

Pokud chcete zkontrolovat, jestli je nainstalovaný Správce balíčků NuGet, klikněte na tlačítko **nástroje** nabídky v sadě Visual Studio. Pokud se zobrazí nabídka položek volá **Správce balíčků NuGet**, pak máte Správce balíčků NuGet.

Instalace Správce balíčků NuGet:

1. Spusťte sadu Visual Studio.
2. Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
3. V **rozšíření a aktualizace** dialogového okna, vyberte **Online**.
4. Pokud nevidíte "Správce balíčků NuGet", zadejte do vyhledávacího pole "Správce balíčků nuget".
5. Vyberte Správce balíčků NuGet a klikněte na tlačítko **Stáhnout**.
6. Až se stahování dokončí, zobrazí se výzva k instalaci.
7. Po dokončení instalace, vám může zobrazit výzva k restartování sady Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Přidat webový balíček NuGet rozhraní API

Po dokončení instalace Správce balíčků NuGet do projektu přidejte balíček Self-Host webové rozhraní API.

1. Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet**. *Poznámka:*: Pokud se vám nezobrazí tato nabídka položek, ujistěte se, že tento správce balíčků NuGet správně nainstalován.
2. Vyberte **spravovat balíčky NuGet pro řešení**
3. V **Správa balíčků Nuget** dialogového okna, vyberte **Online**.
4. Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Vyberte balíček ASP.NET Web API Self hostitele a klikněte na tlačítko **nainstalovat**.
6. Po instalaci balíčku, klikněte na tlačítko **zavřete** zavřete dialogové okno.

> [!NOTE]
> Ujistěte se, že k instalaci balíčku s názvem Microsoft.AspNet.WebApi.SelfHost, ne AspNetWebApi.SelfHost.

![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Vytvoření modelu a kontroler

Tento kurz používá stejné třídy modelu a kontroler jako [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.

Přidejte veřejnou třídu s názvem `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Přidejte veřejnou třídu s názvem `ProductsController`. Odvození z této třídy **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu. Tento kontroler definuje tři akce GET:

| Identifikátor URI | Popis |
| --- | --- |
| / api/produkty | Získání seznamu všech produktů. |
| / webové rozhraníAPI/produkty/*id* | Získání produktu podle ID. |
| /api/products/?category=*category* | Získáte seznam produktů podle kategorie. |

## <a name="host-the-web-api"></a>Hostování webového rozhraní API

Otevřete soubor Program.cs a přidejte následující příkazy using:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Přidejte následující kód, který **Program** třídy.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Volitelné) Přidat rezervaci Namespace adresy URL protokolu HTTP

Tato aplikace naslouchá na `http://localhost:8080/`. Ve výchozím nastavení naslouchání na konkrétní adrese HTTP vyžaduje oprávnění správce. Při spuštění tohoto kurzu, proto se může zobrazit tato chyba: "protokol HTTP nemohl zaregistrovat adresu URL http://+:8080/" existují dva způsoby, jak se vyhnout se této chybě:

- Spuštění sady Visual Studio s oprávněními zvýšenými na úroveň správce, nebo
- Pomocí Netsh.exe udělte vašeho účtu oprávnění k rezervaci adresy URL.

Pokud chcete použít Netsh.exe, otevřete příkazový řádek s oprávněními správce a zadejte následující příkaz: následující příkaz:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

kde *počítač\uživatelské_jméno* je váš uživatelský účet.

Až budete hotovi s vlastním hostováním, je potřeba odstranit rezervaci:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Volání webového rozhraní API z klientské aplikace (C#)

Napíšeme jednoduchou konzolovou aplikaci, která volá webové rozhraní API.

Přidáte do řešení nový projekt konzolové aplikace:

- V Průzkumníku řešení klikněte pravým tlačítkem myši na řešení a vyberte **přidat nový projekt**.
- Vytvořte novou konzolovou aplikaci s názvem &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Použití Správce balíčků NuGet pro přidání balíčku ASP.NET Web API základní knihovny:

- V nabídce Nástroje vyberte **Správce balíčků NuGet**.
- Vyberte **spravovat balíčky NuGet pro řešení**
- V **spravovat balíčky NuGet** dialogového okna, vyberte **Online**.
- Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Vyberte balíček Microsoft ASP.NET Web API klientské knihovny a klikněte na tlačítko **nainstalovat**.

Přidáte odkaz v ClientApp do projektu hostitel ve vlastním procesu:

- V Průzkumníku řešení klikněte pravým tlačítkem na projekt ClientApp.
- Vyberte **přidat odkaz na**.
- V **správce odkazů** dialogového okna, v části **řešení**vyberte **projekty**.
- Vyberte projekt, hostitel ve vlastním procesu.
- Klikněte na tlačítko **OK**.

![](self-host-a-web-api/_static/image6.png)

Otevřete soubor Client/Program.cs. Přidejte následující **pomocí** – příkaz:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Přidání statického **HttpClient** instance:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Přidejte následující metody, které jsou uvedeny všechny produkty, seznam produktů podle ID a seznam produktů podle kategorie.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Každá z těchto metod používá stejný vzor:

1. Volání **HttpClient.GetAsync** odešlete požadavek GET na odpovídající identifikátor URI.
2. Volání **HttpResponseMessage.EnsureSuccessStatusCode**. Tato metoda vyvolá výjimku, pokud je stav odpovědi HTTP chybový kód.
3. Volání **ReadAsAsync&lt;T&gt;**  deserializovat typ CLR z odpovědi HTTP. Tato metoda je metoda rozšiřující, definované v **System.Net.Http.HttpContentExtensions**.

**GetAsync** a **ReadAsAsync** metody jsou asynchronní. Vrátí **úloh** objekty, které představují asynchronní operace. Začínáme **výsledek** vlastnost blokuje vlákno, dokud se operace dokončí.

Další informace o používání HttpClient, včetně postupu provádění neblokující volání, naleznete v tématu [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Před voláním těchto metod, nastavte vlastnost BaseAddress nastavte na instanci HttpClient "`http://localhost:8080`". Příklad:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

To by měl výstupu následující. (Nezapomeňte nejprve spusťte aplikaci SelfHost).

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
