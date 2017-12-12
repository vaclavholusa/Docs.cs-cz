---
uid: web-api/overview/older-versions/self-host-a-web-api
title: "Hostování na vlastním rozhraní ASP.NET Web API 1 (C#) | Microsoft Docs"
author: MikeWasson
description: "Rozhraní ASP.NET Web API nevyžaduje službu IIS. V procesu hostitele, může hostovat samoobslužné webové rozhraní API. Tento kurz ukazuje, jak k hostování webové rozhraní API uvnitř konzoly applic..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2012
ms.topic: article
ms.assetid: be5ab1e2-4140-4275-ac59-ca82a1bac0c1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/self-host-a-web-api
msc.type: authoredcontent
ms.openlocfilehash: b308ee9ec209ba8bbb021827655c83443dd149e6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="self-host-aspnet-web-api-1-c"></a>Hostování na vlastním rozhraní ASP.NET Web API 1 (C#)
====================
podle [Wasson Jan](https://github.com/MikeWasson)

> Rozhraní ASP.NET Web API nevyžaduje službu IIS. V procesu hostitele, může hostovat samoobslužné webové rozhraní API. Tento kurz ukazuje, jak k hostování webové rozhraní API v konzolové aplikaci.
> 
> **Nové aplikace by měly používat OWIN pro hostování na vlastním serveru webového rozhraní API.** V tématu [použít OWIN k hostování na vlastním rozhraní ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md).
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>V tomto kurzu použili verze softwaru
> 
> 
> - Webové rozhraní API 1
> - Visual Studio 2012


## <a name="create-the-console-application-project"></a>Vytvořte projekt konzolové aplikace

Spuštění sady Visual Studio a vyberte **nový projekt** z **spustit** stránky. Nebo z **soubor** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** podokně, vyberte **nainstalovaných šablonách** a rozbalte **Visual C#** uzlu. V části **Visual C#**, vyberte **Windows**. V seznamu šablon projektu, vyberte **konzolové aplikace**. Název projektu &quot;SelfHost&quot; a klikněte na tlačítko **OK**.

![](self-host-a-web-api/_static/image1.png)

## <a name="set-the-target-framework-visual-studio-2010"></a>Nastavení rozhraní Target Framework (Visual Studio 2010)

Pokud používáte Visual Studio 2010, změňte cílový framework rozhraní .NET Framework 4.0. (Ve výchozím nastavení šablona cíle projektu [profilu rozhraní .net Framework klienta](https://msdn.microsoft.com/en-us/library/cc656912.aspx#features_not_included_in_the_net_framework_client_profile).)

V Průzkumníku řešení klikněte pravým tlačítkem na projekt a vyberte **vlastnosti**. V **cílové rozhraní** rozevíracího seznamu, změňte cílový framework na .NET Framework 4.0. Po zobrazení výzvy na použití změny, klikněte na tlačítko **Ano**.

![](self-host-a-web-api/_static/image2.png)

## <a name="install-nuget-package-manager"></a>Instalace Správce balíčků NuGet

Správce balíčků NuGet je nejjednodušší způsob, jak přidat sestavení webového rozhraní API do projektu mimo technologii ASP.NET.

Zkontrolujte, zda je nainstalován Správce balíčků NuGet, klikněte na tlačítko **nástroje** nabídky v sadě Visual Studio. Pokud se zobrazí nabídky položky názvem **Správce balíčků knihoven**, pak máte Správce balíčků NuGet.

Instalace Správce balíčků NuGet:

1. Spuštění sady Visual Studio.
2. Z **nástroje** nabídce vyberte možnost **rozšíření a aktualizace**.
3. V **rozšíření a aktualizace** dialogovém okně, vyberte **Online**.
4. Pokud nevidíte "Správce balíčků NuGet", zadejte do vyhledávacího pole "Správce balíčků nuget".
5. Vyberte Správce balíčků NuGet a klikněte na **Stáhnout**.
6. Po dokončení stahování, zobrazí se výzva k instalaci.
7. Po dokončení instalace může být vyzvání k restartování sady Visual Studio.

![](self-host-a-web-api/_static/image3.png)

## <a name="add-the-web-api-nuget-package"></a>Přidejte balíček NuGet rozhraní API webové

Po instalaci Správce balíčků NuGet do projektu přidejte balíček Self-Host webové rozhraní API.

1. Z **nástroje** nabídce vyberte možnost **Správce balíčků knihoven**. *Poznámka:*: Pokud není se tato nabídka položky, ujistěte se, že Správce balíčků NuGet správně nainstalován.
2. Vyberte **spravovat balíčky NuGet pro řešení...**
3. V **Správa balíčků Nuget** dialogovém okně, vyberte **Online**.
4. Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.SelfHost&quot;.
5. Vyberte balíček, ASP.NET Web API Self Host a klikněte na tlačítko **nainstalovat**.
6. Po instalaci balíčku, klikněte na tlačítko **zavřete** zavřete dialogové okno.

> [!NOTE]
> Ujistěte se, že jste nainstalovali balíček s názvem Microsoft.AspNet.WebApi.SelfHost, není AspNetWebApi.SelfHost.


![](self-host-a-web-api/_static/image4.png)

## <a name="create-the-model-and-controller"></a>Vytvoření modelu a řadiče

Tento kurz používá stejný model a řadič třídy, jako [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu.

Přidejte veřejnou třídu s názvem `Product`.

[!code-csharp[Main](self-host-a-web-api/samples/sample1.cs)]

Přidejte veřejnou třídu s názvem `ProductsController`. Odvození z této třídy **System.Web.Http.ApiController**.

[!code-csharp[Main](self-host-a-web-api/samples/sample2.cs)]

Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md) kurzu. Tento řadič definuje tři GET akce:

| Identifikátor URI | Popis |
| --- | --- |
| / api/produkty | Získání seznamu všech produktů. |
| /API/produkty/*id* | Získání produktu podle ID. |
| /API/produkty /? kategorie =*kategorie* | Získáte seznam produktů podle kategorie. |

## <a name="host-the-web-api"></a>Hostitel webové rozhraní API

Otevřete soubor Program.cs a přidejte následující příkazy:

[!code-csharp[Main](self-host-a-web-api/samples/sample3.cs)]

Přidejte následující kód, který **Program** třídy.

[!code-csharp[Main](self-host-a-web-api/samples/sample4.cs)]

## <a name="optional-add-an-http-url-namespace-reservation"></a>(Volitelné) Přidat rezervaci Namespace adresy URL protokolu HTTP

Tato aplikace naslouchá `http://localhost:8080/`. Ve výchozím nastavení naslouchání na konkrétní adrese HTTP vyžaduje oprávnění správce. Když spustíte tohoto kurzu, proto může se tato chyba: "Protokolu HTTP nebylo možné zaregistrovat URL http://+:8080/" existují dva způsoby, jak se vyhnout této chybě:

- Visual Studio spustit s oprávněními zvýšenými na úroveň správce, nebo
- Pomocí Netsh.exe udělit oprávnění pro uživatelský účet tak, aby vyhradil adresu URL.

Pokud chcete používat Netsh.exe, otevřete příkazový řádek s oprávněními správce a zadejte následující příkaz: následující příkaz:

[!code-console[Main](self-host-a-web-api/samples/sample5.cmd)]

kde *machine\username* je váš uživatelský účet.

Po dokončení vlastní hostování, ujistěte se, jestli chcete odstranit rezervaci:

[!code-console[Main](self-host-a-web-api/samples/sample6.cmd)]

## <a name="call-the-web-api-from-a-client-application-c"></a>Volání webového rozhraní API z klientské aplikace (C#)

Můžete napsat jednoduchý konzolovou aplikaci, která volá webové rozhraní API.

Do řešení přidáte nový projekt konzolové aplikace:

- V Průzkumníku řešení klikněte pravým tlačítkem na řešení a vyberte **přidat nový projekt**.
- Vytvořte novou konzolovou aplikaci s názvem &quot;ClientApp&quot;.

![](self-host-a-web-api/_static/image5.png)

Správce balíčků NuGet použijte k přidání balíčku ASP.NET Web API Core Libraries:

- Z nabídky Nástroje, vyberte **Správce balíčků knihoven**.
- Vyberte **spravovat balíčky NuGet pro řešení...**
- V **spravovat balíčky NuGet** dialogovém okně, vyberte **Online**.
- Do vyhledávacího pole zadejte &quot;Microsoft.AspNet.WebApi.Client&quot;.
- Vyberte balíček Microsoft ASP.NET Web API Client Libraries a klikněte na tlačítko **nainstalovat**.

Přidejte odkaz na projekt SelfHost ClientApp:

- V Průzkumníku řešení klikněte pravým tlačítkem na projekt ClientApp.
- Vyberte **přidat odkaz**.
- V **správce odkazů** dialogové okno, v části **řešení**, vyberte **projekty**.
- Vyberte projekt SelfHost.
- Click **OK**.

![](self-host-a-web-api/_static/image6.png)

Otevřete soubor Client/Program.cs. Přidejte následující **pomocí** příkaz:

[!code-csharp[Main](self-host-a-web-api/samples/sample7.cs)]

Přidání statického **HttpClient** instance:

[!code-csharp[Main](self-host-a-web-api/samples/sample8.cs)]

Přidejte následující metody k zobrazení seznamu všech produktů, seznam produktů podle ID a seznam produktů podle kategorie.

[!code-csharp[Main](self-host-a-web-api/samples/sample9.cs)]

Každá z těchto metod dodržuje stejného vzoru:

1. Volání **HttpClient.GetAsync** odeslat požadavek GET na odpovídající identifikátor URI.
2. Volání **HttpResponseMessage.EnsureSuccessStatusCode**. Tato metoda vyvolá výjimku, pokud je stav odpovědi HTTP chybový kód.
3. Volání **ReadAsAsync&lt;T&gt;**  k deserializaci typ CLR z odpovědi HTTP. Tato metoda je metody rozšíření, definované v **System.Net.Http.HttpContentExtensions**.

**GetAsync** a **ReadAsAsync** jsou oba asynchronní metody. Vracejí **úloh** objekty představující asynchronní operaci. Získávání **výsledek** vlastnost blokuje vlákno, dokud se operace nedokončí.

Další informace o používání HttpClient, včetně toho, jak provádět volání neblokující najdete v části [volání webového rozhraní API z klienta .NET](../advanced/calling-a-web-api-from-a-net-client.md).

Před voláním těchto metod, nastavte vlastnost BaseAddress na instanci systému na HttpClient "`http://localhost:8080`". Příklad:

[!code-csharp[Main](self-host-a-web-api/samples/sample10.cs)]

To by výstup následující. (Nezapomeňte nejprve spustit aplikaci SelfHost.)

[!code-console[Main](self-host-a-web-api/samples/sample11.cmd)]

![](self-host-a-web-api/_static/image7.png)
