---
uid: web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
title: Použití rozhraní Web API s webovými formuláři ASP.NET | Dokumentace Microsoftu
author: MikeWasson
description: ''
ms.author: riande
ms.date: 04/03/2012
ms.assetid: 25da8c3f-4e90-4946-9765-4f160985e1e4
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/using-web-api-with-aspnet-web-forms
msc.type: authoredcontent
ms.openlocfilehash: a14bf0abd8c5d603cf3859891f855415cf3df9f3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752649"
---
<a name="using-web-api-with-aspnet-web-forms"></a>Použití rozhraní Web API s webovými formuláři ASP.NET
====================
podle [Mike Wasson](https://github.com/MikeWasson)

Přestože je součástí balíčku rozhraní ASP.NET Web API ASP.NET MVC, je jednoduše přidávat webového rozhraní API pro tradiční aplikace webových formulářů ASP.NET. Tento kurz vás provede kroky.

## <a name="overview"></a>Přehled

Použití webového rozhraní API v aplikaci webových formulářů, existují dva hlavní kroky:

- Přidání kontroleru webového rozhraní API, která je odvozena z **objektu ApiController** třídy.
- Přidat do směrovací tabulky **aplikace\_Start** metoda.

## <a name="create-a-web-forms-project"></a>Vytvořte projekt webových formulářů

Spusťte sadu Visual Studio a vyberte **nový projekt** z **Start** stránky. Nebo z **souboru** nabídce vyberte možnost **nový** a potom **projektu**.

V **šablony** vyberte **nainstalované šablony** a rozbalte **Visual C#** uzlu. V části **Visual C#** vyberte **webové**. V seznamu šablon projektu vyberte **aplikace webových formulářů ASP.NET**. Zadejte název projektu a klikněte na tlačítko **OK**.

![](using-web-api-with-aspnet-web-forms/_static/image1.png)

## <a name="create-the-model-and-controller"></a>Vytvoření modelu a kontroler

Tento kurz používá stejné třídy modelu a kontroler jako [Začínáme](tutorial-your-first-web-api.md) kurzu.

Nejprve přidejte třídu modelu. V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt a vyberte **přidat třídu**. Název třídy produktu a přidejte následující implementaci:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample1.cs)]

Kontroler Web API v dalším kroku přidejte do projektu., A *řadič* je objekt, který zpracovává požadavky HTTP pro webové rozhraní API.

V **Průzkumníka řešení**, klikněte pravým tlačítkem na projekt. Vyberte **přidat novou položku**.

![](using-web-api-with-aspnet-web-forms/_static/image2.png)

V části **nainstalované šablony**, rozbalte **Visual C#** a vyberte **webové**. Vyberte ze seznamu šablon **třída Kontroleru rozhraní Web API**. Název kontroleru "ProductsController" a klikněte na **přidat**.

![](using-web-api-with-aspnet-web-forms/_static/image3.png)

**Přidat novou položku** průvodce vytvořit soubor s názvem ProductsController.cs. Odstranit metody, které jsou zahrnuty v průvodci a přidejte následující metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample2.cs)]

Další informace o kódu v tomto kontroleru, najdete v článku [Začínáme](tutorial-your-first-web-api.md) kurzu.

## <a name="add-routing-information"></a>Přidat informace o směrování

V dalším kroku přidáme trasu URI tak tento identifikátory URI formuláře &quot;/webové rozhraníAPI/produkty/&quot; se směrují do kontroleru.

V **Průzkumníka řešení**, dvakrát klikněte pro otevření souboru modelu code-behind Global.asax.cs Global.asax. Přidejte následující **pomocí** příkazu.

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample3.cs)]

Pak přidejte následující kód, který **aplikace\_Start** metody:

[!code-csharp[Main](using-web-api-with-aspnet-web-forms/samples/sample4.cs)]

Další informace o směrovacích tabulek naleznete v tématu [směrování v rozhraní ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md).

## <a name="add-client-side-ajax"></a>Přidat AJAX na straně klienta

To je všechno, co potřebujete k vytvoření webového rozhraní API, které mohou klienti získat přístup. Nyní Pojďme přidat stránku HTML, který používá jQuery pro volání rozhraní API.

Ujistěte se, že stránky předlohy (například *Site.Master*) zahrnuje `ContentPlaceHolder` s `ID="HeadContent"`:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample8.html)]

Otevřete soubor Default.aspx. Jak je znázorněno, nahraďte často používaný text, který je v obsahu hlavní části:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample5.aspx)]

V dalším kroku přidáte odkaz na zdrojový soubor jQuery v `HeaderContent` části:

[!code-aspx[Main](using-web-api-with-aspnet-web-forms/samples/sample6.aspx?highlight=2)]

Poznámka: Můžete snadno přidat odkaz na skript přetažením souboru z **Průzkumníka řešení** do okna editoru kódu.

![](using-web-api-with-aspnet-web-forms/_static/image4.png)

Pod značku skriptu jQuery přidejte následující blok skriptu:

[!code-html[Main](using-web-api-with-aspnet-web-forms/samples/sample7.html)]

Po načtení dokumentu tento skript vytvoří požadavek AJAX &quot;produktů s rozhranímapi/&quot;. Požadavek vrátí seznam produktů ve formátu JSON. Skript přidá informace o produktu do tabulky HTML.

Při spuštění aplikace by měl vypadat takto:

![](using-web-api-with-aspnet-web-forms/_static/image5.png)
